---
title: Erstellen und Konfigurieren einer Verfügbarkeitsgruppe für SQL Server für Linux
description: Dieses Tutorial zeigt, wie Sie Verfügbarkeitsgruppen für SQL Server für Linux erstellen und konfigurieren sowie Endpunkte und Zertifikate für Verfügbarkeitsgruppen erstellen.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/28/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ecb634a770b4990bfbbc094b15498b03bdea4a57
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172002"
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>Erstellen und Konfigurieren einer Verfügbarkeitsgruppe für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Tutorial wird gezeigt, wie eine Verfügbarkeitsgruppe (AG) für [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] für Linux erstellt und konfiguriert wird. Im Gegensatz zu [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] und früheren Versionen unter Windows ist es zum Aktivieren von Verfügbarkeitsgruppen nun egal, ob Sie zuerst den zugrunde liegenden Cluster erstellen oder nicht. Die Integration in den Cluster erfolgt bei Bedarf erst später.

Im Tutorial werden die folgenden Aufgaben behandelt:
 
> [!div class="checklist"]
> * Aktivieren von Verfügbarkeitsgruppen
> * Erstellen von Verfügbarkeitsgruppenendpunkten und -zertifikaten
> * Verwenden von [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) oder Transact-SQL zum Erstellen einer Verfügbarkeitsgruppe
> * Erstellen des [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Anmeldenamens und der Berechtigungen für Pacemaker
> * Erstellen von Verfügbarkeitsgruppenressourcen in einem Pacemaker-Cluster (nur externer Typ)

## <a name="prerequisite"></a>Voraussetzung
- Stellen Sie den Pacemaker-Cluster mit Hochverfügbarkeit wie in [Deploy a Pacemaker cluster for SQL Server on Linux (Bereitstellen eines Pacemaker-Clusters für SQL Server für Linux)](sql-server-linux-deploy-pacemaker-cluster.md) beschrieben bereit.


## <a name="enable-the-availability-groups-feature"></a>Aktivieren der Verfügbarkeitsgruppenfunktion

Anders als unter Windows können Sie PowerShell oder [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Configuration Manager nicht verwenden, um die Verfügbarkeitsgruppenfunktion (AG) zu aktivieren. Unter Linux müssen Sie `mssql-conf` zum Aktivieren der Funktion verwenden. Es gibt zwei Möglichkeiten, die Verfügbarkeitsgruppenfunktion zu aktivieren: Entweder Sie verwenden das `mssql-conf`-Hilfsprogramm, oder Sie Bearbeiten die `mssql.conf`-Datei manuell.

> [!IMPORTANT]
> Die Verfügbarkeitsgruppenfunktion muss nur für Replikate im Modus „Nur Konfiguration“ aktiviert werden (auch für [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)]).

### <a name="use-the-mssql-conf-utility"></a>Verwenden des mssql-conf-Hilfsprogramms

Geben Sie bei einer Eingabeaufforderung Folgendes ein:

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>Bearbeiten der mssql.conf-Datei

Sie können zum Hinzufügen der folgenden Zeilen auch die `mssql.conf`-Datei ändern, die sich unter dem Ordner `/var/opt/mssql` befindet:

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-ssnoversion-md"></a>[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] neu starten
Nachdem Sie Verfügbarkeitsgruppen wie unter Windows aktiviert haben, müssen Sie [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] neu starten. Dies erreichen Sie auf folgende Weise:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>Erstellen der Verfügbarkeitsgruppenendpunkte und -zertifikate

Eine Verfügbarkeitsgruppe verwendet TCP-Endpunkte für die Kommunikation. Unter Linux werden Endpunkte für eine Verfügbarkeitsgruppe nur dann unterstützt, wenn Zertifikate für die Authentifizierung verwendet werden. Dies bedeutet, dass das Zertifikat von einer Instanz auf allen anderen Instanzen wiederhergestellt werden muss, die Replikate der gleichen Verfügbarkeitsgruppe sind. Der Zertifikatprozess ist auch für ein Replikat im Modus „Nur Zertifikat“ erforderlich. 

Das Erstellen von Endpunkten und Speichern von Zertifikaten ist nur über Transact-SQL möglich. Sie können auch nicht von [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] generierte Zertifikate verwenden. Außerdem benötigen Sie einen Prozess zum Verwalten und Ersetzen von Zertifikaten, die ablaufen.

> [!IMPORTANT]
> Wenn Sie den [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]-Assistenten zum Erstellen der Verfügbarkeitsgruppe verwenden möchten, müssen Sie die Zertifikate mithilfe von Transact-SQL unter Linux erstellen und wiederherstellen.

Eine vollständige Syntax der Optionen, die für die verschiedenen Befehle verfügbar sind (wie zusätzliche Sicherheit), finden Sie unter:

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> Obwohl Sie eine Verfügbarkeitsgruppe erstellen, verwendet der Typ des Endpunkts *FOR DATABASE_MIRRORING*, da einige zugrunde liegende Aspekte einmal mit dem mittlerweile veralteten Feature freigegeben wurden.

In diesem Beispiel werden Zertifikate für eine Konfiguration mit drei Knoten erstellt. Die Instanznamen lauten LinAGN1, LinAGN2 und LinAGN3.

1.  Führen Sie auf LinAGN1 den folgenden Befehl aus, um den Hauptschlüssel, das Zertifikat und den Endpunkt zu erstellen, und sichern Sie das Zertifikat. In diesem Beispiel wird der typische TCP-Port 5022 für den Endpunkt verwendet.
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  Führen Sie auf LinAGN2 ebenfalls die zuvor beschriebenen Schritte aus:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  Führen Sie schließlich die gleiche Sequenz auf LinAGN3 aus:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  Kopieren Sie mithilfe von `scp` oder einem anderen Hilfsprogramm die Sicherungsdateien des Zertifikats auf jedem Knoten, der Teil der Verfügbarkeitsgruppe wird.
    
    In diesem Beispiel:
    
    - Kopieren Sie LinAGN1_Cert.cer in LinAGN2 und LinAGN3.
    - Kopieren Sie LinAGN2_Cert.cer in LinAGN1 und LinAGN3.
    - Kopieren Sie LinAGN3_Cert.cer in LinAGN1 und LinAGN2.
    
5.  Ändern Sie den Besitzer und die Gruppe, die `mssql` zusammen mit den kopierten Zertifikatsdateien zugeordnet sind.
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  Erstellen Sie die Anmeldungen auf Instanzebene und die Benutzer, die auf LinAGN1 LinAGN2 und LinAGN3 zugeordnet sind.
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  Stellen Sie LinAGN2_Cert und LinAGN3_Cert auf LinAGN1 wieder her. Die Zertifikate der anderen Replikate sind ein wichtiger Aspekt der Kommunikation und Sicherheit der Verfügbarkeitsgruppe.
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  Erstellen Sie die Anmeldenamen auf Instanzebene und die Benutzer, die auf LinAGN2 LinAGN1 und LinAGN3 zugeordnet sind.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10. Stellen Sie LinAGN1_Cert und LinAGN3_Cert auf LinAGN2 wieder her.
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    ```
    
11. Erteilen Sie den Anmeldenamen, die LinAGN1 und LinAGN3 zugeordnet sind, die Berechtigungen, eine Verbindung mit dem Endpunkt auf LinAGN2 herzustellen.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12. Erstellen Sie die Anmeldenamen auf Instanzebene und die Benutzer, die auf LinAGN3 LinAGN1 und LinAGN2 zugeordnet sind.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13. Stellen Sie LinAGN1_Cert und LinAGN2_Cert auf LinAGN3 wieder her. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    ```
    
14. Erteilen Sie den Anmeldenamen, die LinAGN1 und LinAGN2 zugeordnet sind, die Berechtigungen, eine Verbindung mit dem Endpunkt auf LinAGN3 herzustellen.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>Erstellen der Verfügbarkeitsgruppe

In diesem Abschnitt wird beschrieben, wie [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) oder Transact-SQL zum Erstellen der Verfügbarkeitsgruppe für [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] verwendet wird.

### <a name="use-ssmanstudiofull-md"></a>Verwenden Sie [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]

In diesem Abschnitt wird gezeigt, wie mithilfe von SSMS mit dem neuen Verfügbarkeitsgruppenassistenten eine Verfügbarkeitsgruppe mit dem Clustertyp „Extern“ erstellt wird.

1.  Erweitern Sie in SSMS **Hochverfügbarkeit mit Always On**, und klicken Sie zuerst mit der rechten Maustaste auf **Verfügbarkeitsgruppen** und dann auf **Assistent für neue Verfügbarkeitsgruppen**.

2.  Klicken Sie im Dialogfeld „Einführung“ auf **Weiter**.

3.  Geben Sie im Dialogfeld „Optionen der Verfügbarkeitsgruppe angeben“ einen Namen für die Verfügbarkeitsgruppe ein, und wählen Sie in der Dropdownliste den Clustertyp „EXTERN“ oder „KEINE“ aus. „Extern“ sollte verwendet werden, wenn Pacemaker bereitgestellt wird. „Keine“ ist für spezialisierte Szenarios wie Leseaufskalierung. Die Auswahl der Option für die Integritätserkennung auf Datenbankebene ist optional. Weitere Informationen zu dieser Option finden Sie unter [Availability group database level health detection failover option (Failoveroption für die Integritätserkennung auf Datenbankebene in einer Verfügbarkeitsgruppe)](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). Klicken Sie auf **Weiter**.

    ![Erstellen der Verfügbarkeitsgruppe 03](./media/sql-server-linux-create-availability-group/image3.png)

4.  Klicken Sie im Dialogfeld „Datenbanken auswählen“ auf die Datenbank(en), die in die Verfügbarkeitsgruppe vorhanden sein sollen. Jede Datenbank muss über eine vollständige Sicherung verfügen, bevor sie zur Verfügbarkeitsgruppe hinzugefügt werden kann. Klicken Sie auf **Weiter**.

5.  Klicken Sie im Dialogfeld „Replikate angeben“ auf **Replikat hinzufügen**.

6.  Geben Sie im Dialogfeld „Verbindung mit Server herstellen“ den Namen der Linux-Instanz von [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], die das sekundäre Replikat sein wird, und die Anmeldeinformationen zum Herstellen einer Verbindung ein. Klicken Sie auf **Verbinden**.

7.  Wiederholen Sie die vorherigen beiden Schritte für die Instanz, die ein Replikat im Modus „Nur Konfiguration“ oder ein anderes sekundäres Replikat enthalten soll.

8.  Alle drei Instanzen sollten jetzt im Dialogfeld „Replikate angeben“ aufgeführt werden. Wenn Sie den Clustertyp „Extern“ verwenden, sollten Sie sicherstellen, dass der „Verfügbarkeitsmodus“ für das (tatsächlich) sekundäre Replikat mit dem des primären Replikats übereinstimmt, und dass der Failovermodus auf „Extern“ festgelegt ist. Wählen Sie für das Replikat im Modus „Nur Konfiguration“ den Verfügbarkeitsmodus „Nur Konfiguration“ aus.

    Das folgende Beispiel zeigt eine Verfügbarkeitsgruppe mit zwei Replikaten, den Clustertyp „Extern“ und ein Replikat im Modus „Nur Konfiguration“.

    ![Erstellen der Verfügbarkeitsgruppe 04](./media/sql-server-linux-create-availability-group/image4.png)

    Das folgende Beispiel zeigt eine Verfügbarkeitsgruppe mit zwei Replikaten, den Clustertyp „Keine“ und ein Replikat im Modus „Nur Konfiguration“.

    ![Erstellen der Verfügbarkeitsgruppe 05](./media/sql-server-linux-create-availability-group/image5.png)

9.  Klicken Sie auf die Registerkarte „Sicherungseinstellungen“, wenn Sie die Sicherungseinstellungen ändern möchten. Weitere Informationen zu Sicherungseinstellungen für Verfügbarkeitsgruppen finden Sie unter [Konfigurieren der Sicherung von Verfügbarkeitsreplikaten](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).

10. Wenn Sie lesbare sekundäre Replikate verwenden oder eine Verfügbarkeitsgruppe mit dem Clustertyp „Keine“ für die Leseskalierung erstellen, können Sie durch Auswählen der Registerkarte „Listener“ einen Listener erstellen. Ein Listener kann auch zu einem späteren Zeitpunkt hinzugefügt werden. Klicken Sie zum Erstellen eines Listeners auf die Option **Verfügbarkeitsgruppenlistener erstellen**. Geben Sie dann einen Namen und einen TCP/IP-Port ein, und legen Sie fest, ob eine statisch oder automatisch zugewiesene DHCP-IP-Adresse verwendet werden soll. Beachten Sie, dass die IP-Adresse für eine Verfügbarkeitsgruppe mit dem Clustertyp „Keine“ statisch sein sollte und auf die IP-Adresse des primären Replikats festgelegt ist.

    ![Erstellen der Verfügbarkeitsgruppe 06](./media/sql-server-linux-create-availability-group/image6.png)

11. Wenn für lesbare Szenarios ein Listener erstellt wird, ermöglicht SSMS 17.3 oder höher die Erstellung des schreibgeschützten Routings im Assistenten. Er kann auch später über SSMS oder Transact-SQL hinzugefügt werden. So fügen Sie das schreibgeschützte Routing jetzt hinzu:

    a.  Klicken Sie auf die Registerkarte „Read-Only Routing“ (Schreibgeschütztes Routing).

    b.  Geben Sie die URLs für die schreibgeschützten Replikate ein. Diese URLs ähneln den Endpunkten, mit dem Unterschied, dass sie anstelle des Endpunkts den Port der Instanz verwenden.

    c.  Wählen Sie die einzelnen URLs und weiter unten die lesbaren Replikate aus. Sie können die UMSCHALTTASTE gedrückt halten oder klicken und ziehen, um alle auszuwählen.

12. Klicken Sie auf **Weiter**.

13. Wählen Sie aus, wie das/die sekundäre(n) Replikat(e) initialisiert werden soll(en). Standardmäßig wird das [automatische Seeding](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md) verwendet, das denselben Pfad auf allen an der Verfügbarkeitsgruppe beteiligten Server benötigt. Sie können die Datenbank auch mithilfe des Assistenten sichern, kopieren und wiederherstellen (zweite Option). Wenn Sie die Datenbank manuell gesichert, kopiert und wiederhergestellt haben, wird diese auf dem/den Replikat(en) verknüpft (dritte Option). Eine weitere Möglichkeit besteht darin, die Datenbank zu einem späteren Zeitpunkt hinzuzufügen (letzte Option). Wie bei Zertifikaten müssen beim manuellen Sichern und Kopieren Berechtigungen für die Sicherungsdateien für die anderen Replikate festgelegt werden. Klicken Sie auf **Weiter**.

14. Wenn nicht alles als „Erfolgreich“ zurückgegeben wird, können Sie die Ergebnisse im Dialogfeld „Validierung“ überprüfen. Einige Warnungen, die beispielsweise ausgegeben werden, wenn Sie keinen Listener erstellen, sind nicht so schlimm und haben keine großen Auswirkungen. Klicken Sie auf **Weiter**.

15. Klicken Sie im Dialogfeld „Zusammenfassung“ auf **Fertig stellen**. Der Prozess zum Erstellen der Verfügbarkeitsgruppe beginnt jetzt.

16. Klicken Sie nach der Erstellung der Verfügbarkeitsgruppe in der Registerkarte „Ergebnisse“ auf **Schließen**. Jetzt können Sie in der dynamischen Verwaltungssicht und unter dem Ordner „Hochverfügbarkeit mit Always On“ in SSMS die Verfügbarkeitsgruppe auf den Replikaten sehen.

### <a name="use-transact-sql"></a>Verwenden von Transact-SQL

Dieser Abschnitt enthält Beispiele für das Erstellen einer Verfügbarkeitsgruppe mithilfe von Transact-SQL. Der Listener und das schreibgeschützte Routing können nach dem Erstellen der Verfügbarkeitsgruppe konfiguriert werden. Die Verfügbarkeitsgruppe kann zwar mit `ALTER AVAILABILITY GROUP` geändert werden, doch der Clustertyp kann in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] nicht geändert werden. Wenn Sie keine Verfügbarkeitsgruppe mit dem Clustertyp „Extern“ erstellen wollten, müssen Sie sie löschen, und neu mit dem Clustertyp „Keine“ erstellen. Weitere Informationen und andere Optionen finden Sie unter den folgenden Links:

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)
-   [ALTER AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [Konfigurieren des schreibgeschützten Routings für eine Verfügbarkeitsgruppe (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one---two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>Beispiel 1: Zwei Replikate mit einem Replikat im Modus „Nur Konfiguration“ (externer Clustertyp)

Dieses Beispiel zeigt, wie Sie eine Verfügbarkeitsgruppe mit zwei Replikaten erstellen, die ein Replikat im Modus „Nur Konfiguration“ verwendet.

1.  Die Ausführung erfolgt auf dem Knoten, der das primäre Replikat mit der vollständigen Lese-/Schreibkopie der Datenbank(en) sein soll. In diesem Beispiel wird das automatische Seeding verwendet.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  Führen Sie in einem Abfragefenster, das mit dem anderen Replikat verbunden ist, Folgendes aus, um das Replikat mit der Verfügbarkeitsgruppe zu verknüpfen und den Seedingprozess vom primären zum sekundären Replikat zu initiieren.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3. Verknüpfen Sie es in einem Abfragefenster, das mit dem Replikat im Modus „Nur Konfiguration“ verbunden ist, mit der Verfügbarkeitsgruppe.
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two---three-replicas-with-read-only-routing-external-cluster-type"></a>Beispiel 2: Drei Replikate mit schreibgeschütztem Routing (externer Clustertyp)

In diesem Beispiel werden drei vollständige Replikate und die Konfiguration des schreibgeschützten Routings als Teil der Erstellung der anfänglichen Verfügbarkeitsgruppe veranschaulicht.

1.  Die Ausführung erfolgt auf dem Knoten, der das primäre Replikat mit der vollständigen Lese-/Schreibkopie der Datenbank(en) sein soll. In diesem Beispiel wird das automatische Seeding verwendet.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    Bei dieser Konfiguration sind einige Punkte zu beachten:
    
    - *AGName* ist der Name der Verfügbarkeitsgruppe.
    - *DBName* ist der Name der Datenbank, die mit der Verfügbarkeitsgruppe verwendet wird. Dies kann auch eine Liste mit durch Kommas getrennte Namen sein.
    - *ListenerName* ist ein Name, der sich von den zugrunde liegenden Servern/Knoten unterscheidet. Sie wird in DNS zusammen mit *IPAddress* registriert.
    - *IPAddress* ist eine IP-Adresse, die *ListenerName* zugeordnet ist. Sie ist eindeutig und mit keinem Server/Knoten identisch. Anwendungen und Endbenutzer verwenden entweder *ListenerName* oder *IPAddress*, um eine Verbindung mit der Verfügbarkeitsgruppe herzustellen.
    - *SubnetMask* ist die Subnetzmaske von *IPAddress* (beispielsweise 255.255.255.0).

2.  Führen Sie in einem Abfragefenster, das mit dem anderen Replikat verbunden ist, Folgendes aus, um das Replikat mit der Verfügbarkeitsgruppe zu verknüpfen und den Seedingprozess vom primären zum sekundären Replikat zu initiieren.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Wiederholen Sie Schritt 2 für das dritte Replikat.

#### <a name="example-three---two-replicas-with-read-only-routing-none-cluster-type"></a>Beispiel 3: Zwei Replikate mit schreibgeschütztem Routing (Clustertyp „Keine“)

In diesem Beispiel wird gezeigt, wie eine Konfiguration mit zwei Replikaten mit dem Clustertyp „Keine“ erstellt wird. Sie wird für das Leseskalierungsszenario verwendet, bei dem kein Failover erwartet wird. Dadurch wird der Listener, der tatsächlich das primäre Replikat ist, sowie das schreibgeschützte Routing mit der Roundrobin-Funktionalität erstellt.

1.  Die Ausführung erfolgt auf dem Knoten, der das primäre Replikat mit der vollständigen Lese-/Schreibkopie der Datenbank(en) sein soll. In diesem Beispiel wird das automatische Seeding verwendet.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    Hierbei gilt:
    - *AGName* ist der Name der Verfügbarkeitsgruppe.
    - *DBName* ist der Name der Datenbank, die mit der Verfügbarkeitsgruppe verwendet wird. Dies kann auch eine Liste mit durch Kommas getrennte Namen sein.
    - *PortOfEndpoint* ist die Portnummer, die vom erstellten Endpunkt verwendet wird.
    - *PortOfInstance* ist die Portnummer, die von der Instanz von [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] verwendet wird.
    - *ListenerName* ist ein Name, der sich von den zugrunde liegenden Replikaten unterscheidet, aber nicht verwendet wird.
    - *PrimaryReplicaIPAddress* ist die IP-Adresse des primären Replikats.
    - *SubnetMask* ist die Subnetzmaske von *IPAddress*. Beispiel: 255.255.255.0.
    
2.  Verknüpfen Sie das sekundäre Replikat mit der Verfügbarkeitsgruppe, und initiieren Sie das automatische Seeding.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-ssnoversion-md-login-and-permissions-for-pacemaker"></a>Erstellen des [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Anmeldenamens und der Berechtigungen für Pacemaker

Ein unter Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] zugrunde liegender Pacemaker-Cluster mit Hochverfügbarkeit benötigt Zugriff auf die [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Instanz und Berechtigungen für die Verfügbarkeitsgruppe selbst. Mit diesen Schritten werden der Anmeldename und die zugehörigen Berechtigungen zusammen mit einer Datei erstellt, die Pacemaker mitteilt, wie die Anmeldung bei [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] erfolgen soll.

1.  Führen Sie in einem Abfragefenster, das mit dem ersten Replikat verbunden ist, Folgendes aus:

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD ='<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  Geben Sie auf Knoten 1 den Befehl ein. 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    Dadurch wird der Emacs-Editor geöffnet.
    
3.  Geben Sie die beiden folgenden Zeilen in den Editor ein:

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  Halten Sie die STRG-Taste gedrückt, drücken Sie X und anschließend C, um die Datei zu beenden und zu speichern.

5.  Execute 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    um die Datei zu sperren.

6.  Wiederholen Sie die Schritte 1 bis 5 auf den anderen Servern, die als Replikate fungieren.

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>Erstellen der Verfügbarkeitsgruppenressourcen im Pacemaker-Cluster (nur „Extern“)

Nachdem eine Verfügbarkeitsgruppe in [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] erstellt wurde, müssen die entsprechenden Ressourcen in Pacemaker erstellt werden, wenn der Clustertyp „Extern“ angegeben wird. Einer Verfügbarkeitsgruppe sind zwei Ressourcen zugeordnet: die Verfügbarkeitsgruppe selbst und eine IP-Adresse. Das Konfigurieren der IP-Adressressource ist zwar optional, wird allerdings empfohlen, wenn Sie die Listenerfunktion nicht verwenden.

Die erstellte Verfügbarkeitsgruppenressource ist eine spezielle Art von Ressource, die als Klon bezeichnet wird. Die Verfügbarkeitsgruppe verfügt im Wesentlichen über Kopien der einzelnen Knoten, und es gibt eine Steuerungsressource, die als „Master“ bezeichnet wird. Diese Ressource wird dem Server zugeordnet, der das primäre Replikat hostet. Die anderen Ressourcen hosten sekundäre Replikate („Regulär“ oder „Nur Konfiguration“) und können in einem Failover zum Master heraufgestuft werden.

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

1.  Erstellen Sie die Verfügbarkeitsgruppenressource mit der folgenden Syntax:

    **Red Hat Enterprise Linux (RHEL) und Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failure-timeout=30s --master meta notify=true
    ```

    >[!NOTE]
    >Unter RHEL 7.4 kann bei der Verwendung von „--master“ eine Warnung auftreten. Verwenden Sie `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failover-timeout=30s master notify=true`, um dies zu vermeiden.
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    meta failure-timeout=60s \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    wird verwendet, wenn *NameForAGResource* der eindeutige Name dieser Clusterressource für die Verfügbarkeitsgruppe und *AGName* der Name der erstellten Verfügbarkeitsgruppe ist.
 
2.  Erstellen Sie die IP-Adressressource für die Verfügbarkeitsgruppe, die der Listenerfunktion zugeordnet wird.

    **RHEL und Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    wird verwendet, wenn *NameForIPResource* der eindeutige Name der IP-Adressressource und *IPAddress* die der Ressource zugewiesene statische IP-Adresse ist. In SLES müssen Sie auch die Netzmaske angeben. So hat 255.255.255.0 für *Netmask* beispielsweise den Wert „24“.
    
3.  Es muss eine Kollokationseinschränkung konfiguriert werden, um sicherzustellen, dass die IP-Adresse und die Verfügbarkeitsgruppenressource auf demselben Knoten ausgeführt werden.

    **RHEL und Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    wird verwendet, wenn *NameForIPResource* der Name der IP-Adressressource, *NameForAGResource* der Name der Verfügbarkeitsgruppenressource und *NameForConstraint* in SLES der Name der Einschränkung ist.

4.  Erstellen Sie eine Sortierungseinschränkung, um sicherzustellen, dass die Verfügbarkeitsgruppenressource vor der IP-Adresse ausgeführt wird. Obwohl die Kollokationseinschränkung eine Sortierungseinschränkung impliziert, erzwingt sie diese.

    **RHEL und Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    wird verwendet, wenn *NameForIPResource* der Name der IP-Adressressource, *NameForAGResource* der Name der Verfügbarkeitsgruppenressource und *NameForConstraint* in SLES der Name der Einschränkung ist.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde gezeigt, wie eine Verfügbarkeitsgruppe für [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] unter Linux erstellt und konfiguriert wird. Sie haben Folgendes gelernt:
> [!div class="checklist"]
> * Aktivieren von Verfügbarkeitsgruppen
> * Erstellen von Verfügbarkeitsgruppenendpunkten und -zertifikaten
> * Verwenden von [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) oder Transact-SQL zum Erstellen einer Verfügbarkeitsgruppe
> * Erstellen des [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Anmeldenamens und der Berechtigungen für Pacemaker
> * Erstellen von Verfügbarkeitsgruppenressourcen in einem Pacemaker-Cluster

Informationen zu den meisten Verwaltungsaufgaben in Bezug auf Verfügbarkeitsgruppen einschließlich Upgrades und Failovers finden Sie unter:

> [!div class="nextstepaction"]
> [Arbeiten mit einer Verfügbarkeitsgruppe mit Hochverfügbarkeit für SQL Server für Linux](sql-server-linux-availability-group-failover-ha.md)

