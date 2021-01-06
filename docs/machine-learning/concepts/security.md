---
title: Sicherheitsarchitektur für die Erweiterbarkeit
description: In diesem Artikel wird die Sicherheitsarchitektur für das Erweiterbarkeitsframework in SQL Server Machine Learning Services beschrieben. Dies umfasst Sicherheit für Anmelde- und Benutzerkonten, den SQL Server-Launchpad-Dienst, Workerkonten, das Ausführen mehrerer Skripts und Dateiberechtigungen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.custom: contperf-fy21q1, seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 63138dbd9ef2acd45dfc508bac20bd6694d49d90
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878886"
---
# <a name="security-architecture-for-the-extensibility-framework-in-sql-server-machine-learning-services"></a>Sicherheitsarchitektur für das Erweiterbarkeitsframework in SQL Server Machine Learning Services

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Artikel wird die Sicherheitsarchitektur beschrieben, die zur Integration der SQL Server-Datenbank-Engine sowie der zugehörigen Komponenten mit dem Erweiterbarkeitsframework in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) verwendet wird. Es werden die sicherungsfähigen Elemente, Dienste, Prozessidentität und Berechtigungen untersucht. Zu den Hauptthemen dieses Artikels gehören der Zweck von Launchpad, SQLRUserGroup und Workerkonten, die Prozessisolation von externen Skripts und die Zuordnung von Benutzeridentitäten zu Workerkonten.

Weitere Informationen zu den wichtigsten Konzepten und Komponenten der Erweiterbarkeit in SQL Server finden Sie unter [Erweiterbarkeitsarchitektur in SQL Server Machine Learning Services](extensibility-framework.md).

## <a name="securables-for-external-script"></a>Sicherungsfähige Elemente für externes Skript

Ein externes Skript wird als Eingabeparameter für eine [im gespeicherte Systemprozedur](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) übermittelt, die zu diesem Zweck erstellt oder in eine von Ihnen definierte gespeicherte Prozedur eingeschlossen wird. Dieses Skript kann in R, Python oder externen Sprachen wie Java oder .NET geschrieben werden. Alternativ können Sie auch über Modelle verfügen, die vorab trainiert und in einem Binärformat in einer Datenbanktabelle gespeichert sind und somit in einer [PREDICT](../../t-sql/queries/predict-transact-sql.md)-T-SQL-Funktion aufgerufen werden können.

Da das Skript über vorhandene Datenbankschemaobjekte, gespeicherte Prozeduren und Tabellen bereitgestellt wird, gibt es keine neuen [sicherungsfähige Elemente](../../relational-databases/security/securables.md) für SQL Server Machine Learning Services.

Unabhängig davon, wie Sie Skripts verwenden oder woraus sie bestehen, werden Datenbankobjekte erstellt und wahrscheinlich gespeichert, aber es wird kein neuer Objekttyp für die Speicherung des Skripts eingeführt. Folglich hängt die Fähigkeit, Datenbankobjekte zu nutzen, zu erstellen und zu speichern, weitgehend von den bereits für Ihre Benutzer definierten Datenbankberechtigungen ab.

<a name="permissions"></a>

## <a name="permissions"></a>Berechtigungen

Das Datensicherheitsmodell mit Datenbankanmeldungen und Rollen von SQL Server umfasst auch externe Skripts. Zum Ausführen externer Skripts, die SQL Server-Daten verwenden oder mit SQL Server als Computekontext ausgeführt werden, ist eine SQL Server-Anmeldung oder ein Windows-Benutzerkonto erforderlich. Datenbankbenutzer, die über Berechtigungen zum Ausführen einer Abfrage verfügen, können auf dieselben Daten aus externen Skripts zugreifen.

Das Anmelde- oder Benutzerkonto identifiziert den *Sicherheitsprinzipal*, der je nach den Anforderungen des externen Skripts mehrere Zugriffsebenen benötigen kann:

+ Die Berechtigung für den Zugriff auf die Datenbank, in der externe Skripts aktiviert sind.
+ Berechtigungen zum Lesen von Daten aus gesicherten Objekten wie Tabellen.
+ Die Möglichkeit, neue Daten in eine Tabelle zu schreiben, z. B. ein Modell, oder Ergebnisse zu bewerten.
+ Die Möglichkeit, neue Objekte zu erstellen, wie z. B. Tabellen, gespeicherte Prozeduren, die das externe Skript verwenden, oder benutzerdefinierte Funktionen, die externe Skriptaufträge verwenden.
+ Die Berechtigung, neue Pakete auf dem SQL Server-Computer zu installieren oder Pakete zu verwenden, die einer Gruppe von Benutzern zur Verfügung gestellt werden.

Jede Person, die ein externes Skript mit SQL Server als Ausführungskontext ausführt, muss einem Benutzer in der Datenbank zugeordnet werden. Anstatt individuelle Berechtigungen für Datenbankbenutzer festzulegen, können Sie Rollen erstellen, um Berechtigungen zu verwalten und Benutzer diesen Rollen zuzuordnen.

Weitere Informationen finden Sie unter [Give users permission to SQL Server Machine Learning Services (Erteilen von Benutzerberechtigungen für SQL Server Machine Learning Services)](../../machine-learning/security/user-permission.md).

## <a name="permissions-when-using-an-external-client-tool"></a>Berechtigungen bei Verwendung eines externen Clienttools

Für Benutzer, die Skripts in einem externen Clienttool verwenden, müssen die Anmeldeinformationen bzw. das Konto einem Benutzer in der Datenbank zugeordnet sein, wenn sie ein externes Skript in der Datenbank ausführen oder auf Datenbankobjekte und -daten zugreifen müssen. Die gleichen Berechtigungen sind erforderlich, unabhängig davon, ob das externe Skript von einem entfernten Data Science-Client gesendet oder mit einer gespeicherten T-SQL-Prozedur ausgeführt wird.

Angenommen, Sie haben ein externes Skript erstellt, das auf Ihrem lokalen Computer ausgeführt wird, und Sie möchten dieses Skript in SQL Server ausführen. Sie müssen sicherstellen, dass die folgenden Bedingungen erfüllt sind:

+ Die Datenbank lässt Remoteverbindungen zu.
+ Die SQL-Anmeldeinformationen bzw. die Windows-Kontodaten, die Sie für den Zugriff auf die Datenbank verwendet haben, wurden zur SQL Server-Instanz hinzugefügt.
+ Für die SQL-Anmeldung bzw. den Windows-Benutzer muss die Berechtigung zum Ausführen externer Skripts erteilt werden. Diese Berechtigung kann in der Regel nur von einem Datenbankadministrator hinzugefügt werden.
+ Für die SQL-Anmeldung bzw. als Windows-Benutzer muss in jeder Datenbank, in der das externe Skript eine dieser Vorgänge ausführt, ein Benutzer mit entsprechenden Berechtigungen hinzugefügt werden:
  + Abrufen von Daten.
  + Schreiben oder Aktualisieren von Daten.
  + Erstellen neuer Objekte, wie z. B. Tabellen oder gespeicherte Prozeduren.

Nachdem das Anmelde- oder Windows-Benutzerkonto bereitgestellt und mit den erforderlichen Berechtigungen versehen wurde, können Sie ein externes Skript in SQL Server ausführen, indem Sie ein Datenquellenobjekt in R oder die Bibliothek **revoscalepy** in Python verwenden oder eine gespeicherte Prozedur aufrufen, die das externe Skript enthält.

Wann immer ein externes Skript auf der SQL Server-Instanz gestartet wird, ermittelt die Sicherheitsfunktion der Datenbank-Engine den Sicherheitskontext des Benutzers, der den Auftrag gestartet hat, und verwaltet die Zuordnungen des Benutzers bzw. des Anmeldekontos zu sicherungsfähigen Objekten.

Aus diesem Grund müssen alle externen Skripts, die von einem Remoteclient initiiert werden, die Anmelde- oder Benutzerinformationen als Teil der Verbindungszeichenfolge angeben.

<a name="launchpad"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="services-used-in-external-processing-launchpad"></a>In der externen Verarbeitung verwendete Dienste (Launchpad)

Das Erweiterbarkeitsframework fügt einen neuen NT-Dienst zur [Liste der Dienste](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details) in einer SQL Server-Installation hinzu: [**SQL Server-Launchpad (MSSSQLSERVER)**](extensibility-framework.md#launchpad).

Die Datenbank-Engine verwendet den SQL Server-**Launchpad**-Dienst, um eine externe Skriptsitzung als separaten Prozess zu instanziieren. 
Der Prozess wird mit einem Konto mit geringen Rechten ausgeführt. Dieses Konto unterscheidet sich von SQL Server, Launchpad und der Benutzeridentität, unter der die gespeicherte Prozedur oder Hostabfrage ausgeführt wurde. Die Ausführung von Skripts in einem separaten Prozess unter einem Konto mit niedrigen Berechtigungen ist die Grundlage des Sicherheits- und Isolationsmodells für externe Skripts in SQL Server.

SQL Server verwaltet außerdem die Zuordnung der Identität des aufrufenden Benutzers zum Workerkonto mit geringen Berechtigungen, das zum Starten des Satellitenprozesses verwendet wird. In einigen Szenarios, in denen Skripts oder Code für Daten und Vorgänge an SQL Server zurückgerufen werden, kann SQL Server den Identitätstransfer nahtlos steuern. Skripts, die SELECT-Anweisungen oder aufrufende Funktionen und andere Programmierobjekte enthalten, werden in den meisten Fällen erfolgreich sein, wenn der aufrufende Benutzer über ausreichende Berechtigungen verfügt.

> [!NOTE]
> Standardmäßig ist [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] so konfiguriert, dass es unter **NT Service\MSSQLLaunchpad** ausgeführt wird, welches mit allen notwendigen Berechtigungen zum Ausführen externer Skripts ausgestattet ist. Weitere Informationen zu konfigurierbaren Optionen finden Sie unter [Konfiguration des SQL Server-Launchpad-Diensts](../security/sql-server-launchpad-service-account.md).

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="services-used-in-external-processing-launchpad"></a>In der externen Verarbeitung verwendete Dienste (Launchpad)

Das Erweiterbarkeitsframework fügt einen neuen NT-Dienst zur [Liste der Dienste](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details) in einer SQL Server-Installation hinzu: [**SQL Server-Launchpad (MSSSQLSERVER)**](extensibility-framework.md#launchpad).

Die Datenbank-Engine verwendet den SQL Server-**Launchpad**-Dienst, um eine externe Skriptsitzung als separaten Prozess zu instanziieren. 
Der Prozess wird mit der Launchpad-Benutzeridentität ausgeführt, weist jedoch die Einschränkung auf, dass er sich in einer AppContainer-Instanz befindet. Die Ausführung von Skripts in einem separaten Prozess unter einer AppContainer-Instanz ist die Grundlage des Sicherheits- und Isolationsmodells für externe Skripts in SQL Server.

SQL Server verwaltet außerdem die Zuordnung der Identität des aufrufenden Benutzers zum Workerkonto mit geringen Berechtigungen, das zum Starten des Satellitenprozesses verwendet wird. In einigen Szenarios, in denen Skripts oder Code für Daten und Vorgänge an SQL Server zurückgerufen werden, kann SQL Server den Identitätstransfer nahtlos steuern. Skripts, die SELECT-Anweisungen oder aufrufende Funktionen und andere Programmierobjekte enthalten, werden in den meisten Fällen erfolgreich sein, wenn der aufrufende Benutzer über ausreichende Berechtigungen verfügt.

> [!NOTE]
> Standardmäßig ist [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] so konfiguriert, dass es unter **NT Service\MSSQLLaunchpad** ausgeführt wird, welches mit allen notwendigen Berechtigungen zum Ausführen externer Skripts ausgestattet ist. Weitere Informationen zu konfigurierbaren Optionen finden Sie unter [Konfiguration des SQL Server-Launchpad-Diensts](../security/sql-server-launchpad-service-account.md).

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="services-used-in-external-processing"></a>In der externen Verarbeitung verwendete Dienste

Das Erweiterbarkeitsframework fügt einen neuen Daemon in einer SQL Server-Installation hinzu: [mssql-launchpadd](extensibility-framework.md#launchpad). mssql-launchpadd wird unter dem Konto „mssql-launchpadd“ mit geringen Berechtigungen ausgeführt, das erstellt wird, wenn das Paket „mssql-server-extensibility“ installiert wird.

Nur eine Instanz der Datenbank-Engine wird unterstützt, und es ist ein launchpadd-Dienst an die Instanz gebunden. Wenn ein Skript ausgeführt wird, startet der launchpadd-Dienst einen separaten Launchpadprozess mit dem mssql_satellite-Benutzerkonto mit geringen Berechtigungen und eigener neuer PID, IPC, Einbindung und Netzwerknamespace. Jeder Satellitenprozess erbt das mssql_satellite-Benutzerkonto des Launchpads und verwendet dieses für die Dauer der Skriptausführung.

Weitere Informationen finden Sie unter [Erweiterbarkeitsarchitektur in SQL Server Machine Learning Services](extensibility-framework.md).

::: moniker-end

<a name="sqlrusergroup"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="identities-used-in-processing-sqlrusergroup"></a>Bei der Verarbeitung verwendete Identitäten (SQLRUserGroup)

**SQLRUserGroup** (eingeschränkte SQL-Benutzergruppe) wird durch das SQL Server-Setup erstellt und enthält einen Pool von lokalen Windows-Benutzerkonten mit niedriger Berechtigung. Wenn ein externer Prozess benötigt wird, verwendet Launchpad ein verfügbares Workerkonto zum Ausführen eines Prozesses. Genauer gesagt, Launchpad aktiviert ein verfügbares Workerkonto, ordnet es der Identität des aufrufenden Benutzers zu und führt das Skript unter dem Workerkonto aus.

+ **SQLRUserGroup** ist mit einer bestimmten Instanz verknüpft. Für jede Instanz, für die maschinelles Lernen aktiviert wurde, wird ein separater Pool von Workerkonten benötigt. Konten können nicht zwischen Instanzen freigegeben werden.

+ Die Größe des Benutzerkontenpools ist statisch und der Standardwert ist 20, wodurch 20 gleichzeitige Sitzungen unterstützt werden. Die Anzahl der externen Laufzeitsitzungen, die gleichzeitig gestartet werden können, ist durch die Größe dieses Benutzerkontenpools beschränkt. 

+ Workerkontonamen im Pool weisen das Format „SQLInstanzname *nn*“ auf. Auf einer Standardinstanz beispielsweise enthält **SQLRUserGroup** Konten mit den Namen MSSQLSERVER01, MSSQLSERVER02 und so weiter bis zu MSSQLSERVER20.

Für parallel ausgeführte Aufgaben sind keine zusätzlichen Konten erforderlich. Wenn ein Benutzer beispielsweise eine Bewertungstask ausführt, die parallel verarbeitet wird, wird für alle Threads das gleiche Workerkonto wiederverwendet. Wenn Sie beabsichtigen, die Machine Learning-Funktion intensiv zu nutzen, können Sie die Anzahl der Konten erhöhen, die für die Ausführung externer Skripts verwendet werden. Weitere Informationen finden Sie unter [Skalieren der gleichzeitige Ausführung externer Skripts in SQL Server-Machine Learning Services](../../machine-learning/administration/scale-concurrent-execution-external-scripts.md).

### <a name="permissions-granted-to-sqlrusergroup"></a>Für SQLRUserGroup erteilte Berechtigungen

Mitglieder von **SQLRUserGroup** verfügen standardmäßig über Lese- und Ausführungsberechtigungen für Dateien in den SQL Server-Verzeichnissen **Binn**, **R_SERVICES** und **PYTHON_SERVICES**. Dies umfasst Zugriff auf ausführbare Dateien, Bibliotheken und integrierte Datasets in den R- und Python-Verteilungen, die mit SQL Server installiert werden. 

Um vertrauliche Ressourcen in SQL Server zu schützen, können Sie optional eine Zugriffssteuerungsliste (ACL) definieren, die den Zugriff auf **SQLRUserGroup** verweigert. Umgekehrt können Sie auch Berechtigungen für lokale Datenressourcen vergeben, die auf dem Hostcomputer vorhanden sind, mit Ausnahme von SQL Server selbst. 

**SQLRUserGroup** verfügt von Haus aus über keine Anmeldekonten für Datenbanken oder Berechtigungen für Daten. Unter bestimmten Umständen sollten Sie ein Anmeldekonto erstellen, um Loopbackverbindungen zuzulassen, insbesondere wenn es sich beim aufrufenden Benutzer um eine vertrauenswürdige Windows-Identität handelt. Diese Möglichkeit wird als [*implizite Authentifizierung*](#implied-authentication) bezeichnet. Weitere Informationen finden Sie unter [Hinzufügen von SQLRUserGroup als Datenbankbenutzer](../../machine-learning/security/create-a-login-for-sqlrusergroup.md).

## <a name="identity-mapping"></a>Identitätszuordnung

Wenn eine Sitzung gestartet wird, ordnet Launchpad die Identität des aufrufenden Benutzers einem Workerkonto zu. Diese Zuordnung eines externen Windows-Benutzers oder einer gültigen SQL-Anmeldung zu einem Workerkonto gilt nur für die Lebensdauer der gespeicherten SQL-Prozedur, die das externe Skript ausführt. Parallele Abfragen von derselben Anmeldung werden demselben Benutzerworkerkonto zugeordnet.

Während der Ausführung erstellt Launchpad temporäre Ordner zum Speichern von Sitzungsdaten und löscht diese nach Beendigung der Sitzung. Der Zugriff auf die Verzeichnisse ist eingeschränkt. Für R wird diese Task von RLauncher ausgeführt. Für Python wird diese Task von PythonLauncher ausgeführt. Jedes einzelne Workerkonto ist auf seinen eigenen Ordner beschränkt und kann nicht auf Dateien in übergeordneten Verzeichnissen zugreifen. Allerdings kann das Workerkonto untergeordnete Elemente unter dem erstellten Sitzungsarbeitsordner lesen, schreiben oder löschen. Wenn Sie Administrator auf dem Computer sind, können Sie die Verzeichnisse anzeigen, die für jeden Prozess erstellt wurden. Jedes Verzeichnis wird durch die Sitzungs-GUID identifiziert.

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="appcontainer-isolation"></a>Informationen zur AppContainer-Isolation

Die Isolation wird durch [AppContainer](/windows/desktop/secauthz/appcontainer-isolation)-Instanzen erzielt. Wenn zur Laufzeit ein externes Skript in einer gespeicherten Prozedur oder Abfrage erkannt wird, ruft SQL Server Launchpad mit der Anforderung eines erweiterungsspezifischen Startprogramms auf. Launchpad ruft die entsprechende Laufzeitumgebung in einem Prozess unter seiner Identität auf und instanziiert einen AppContainer, um ihn aufzunehmen. Diese Änderung hat den Vorteil, dass keine lokalen Konten und Kennwörter mehr verwaltet werden müssen. Zudem kann dank dem Wegfall der Abhängigkeit von lokalen Benutzerkonten dieses Feature nun in Installationen verwendet werden, bei denen lokale Benutzerkonten nicht zulässig sind.

Gemäß der Implementierung durch SQL Server handelt es sich bei AppContainern um einen internen Mechanismus. Obwohl sich AppContainer im Prozessmonitor physisch nicht nachweisen lassen, sind sie dennoch in Firewallregeln für ausgehenden Datenverkehr zu finden, die beim Setup erstellt werden, um zu verhindern, dass Prozesse Netzwerkaufrufe ausführen. Weitere Informationen finden Sie unter [Firewallkonfiguration für SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md).

## <a name="identity-mapping"></a>Identitätszuordnung

Wenn eine Sitzung gestartet wird, ordnet Launchpad die Identität des aufrufenden Benutzers einer **AppContainer**-Instanz zu.

> [!Note]
> Ab SQL Server 2019 hat **SQLRUserGroup** nur ein Mitglied, das nun das einzige SQL Server-Launchpad-Dienstkonto anstelle von mehreren Workerkonten ist.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="identity-mapping"></a>Identitätszuordnung

Der **Launchpadd**-Daemon (doppeltes D, [mssql-launchpadd](extensibility-framework.md#launchpad)) weist die Identität des aufrufenden Benutzers einem separaten **Launchpad**-Prozess (mit einem D) einem „launchpad GUID“-Ordner und einem Satellitenzertifikat zu. Diese „launchpad GUID“-Ordner werden unter `/var/opt/mssql-extensibility/data/` erstellt. Der Launchpad-Prozess verwendet dieses Zertifikat für die Authentifizierung mit SQL und erstellt dann temporäre Ordner für jede Sitzungs-GUID im Ordner „launchpad GUID“. Der Satellitenprozess (R, Python oder ExtHost) kann auf den Ordner „launchpad GUID“, das darin enthaltene Zertifikat und den Ordner „session GUID“ zugreifen.

Das folgende SQL-Skript gibt die Inhalte der Launchpad-Ordner aus.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    ,@script = N'
print("Contents of /var/opt/mssql-extensibility/data :");
print(system("ls -al /var/opt/mssql-extensibility/data"));
print("Contents of Launchpad GUID folder:");
print(system("ls -al /var/opt/mssql-extensibility/data/*"));
print(system("ls -al /var/opt/mssql-extensibility/data/*/*"))
'
    ,@input_data_1 = N'SELECT 1 AS hello'
```

::: moniker-end

<a name="implied-authentication"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="implied-authentication-loopback-requests"></a>Implizite Authentifizierung (Loopbackanforderungen)

Eine *implizierte Authentifizierung* beschreibt das Verhalten von Verbindungsanforderungen, bei dem externe Prozesse, die als Workerkonten mit niedriger Berechtigung ausgeführt werden, als vertrauenswürdige Benutzeridentität für SQL Server bei Loopbackanforderungen für Daten oder Vorgänge dargestellt werden. Als Konzept ist die implizite Authentifizierung einzigartig für die Windows-Authentifizierung, bei SQL Server-Verbindungszeichenfolgen, die eine vertrauenswürdige Verbindung angeben, und bei Anforderungen, die von externen Prozessen wie R- oder Python-Skripts stammen. Sie wird manchmal auch als *Loopback* bezeichnet.

Vertrauenswürdige Verbindungen können über externe Skripts hergestellt werden, jedoch nur, wenn eine zusätzliche Konfiguration vorgenommen wird. In der Erweiterungsarchitektur werden externe Prozesse unter Workerkonten ausgeführt und erben Berechtigungen vom übergeordneten **SQLRUserGroup**-Verzeichnis. Wenn eine Verbindungszeichenfolge `Trusted_Connection=True` angibt, wird die Identität des Workerkontos für die Verbindungsanforderung angezeigt, die SQL Server standardmäßig nicht bekannt ist.

Um vertrauenswürdige Verbindungen erfolgreich herzustellen, müssen Sie ein Datenbankanmeldekonto für die **SQLRUserGroup** erstellen. Danach verfügt jede vertrauenswürdige Verbindung jedes **SQLRUserGroup**-Mitglieds über die Anmeldeberechtigung für SQL Server. Detaillierte Anweisungen finden Sie unter [Erstellen von Anmeldeinformationen für SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md).

Vertrauenswürdige Verbindungen sind nicht die am häufigsten verwendete Variante einer Verbindungsanforderung. Wenn das externe Skript eine Verbindung angibt, ist es üblicher, ein SQL-Anmeldekonto oder einen vollständig angegebenen Benutzernamen und ein Kennwort zu verwenden, sofern die Verbindung zu einer ODBC-Datenquelle besteht.

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Funktionsweise der impliziten Authentifizierung für externe Skriptsitzungen

Das folgende Diagramm zeigt die Interaktion von SQL Server-Komponenten mit der Language Runtime und die damit verbundene Authentifizierung in Windows.

![Implizite Authentifizierung in Windows](../security/media/implied-auth-windows-2017.png)

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="implied-authentication-loopback-requests"></a>Implizite Authentifizierung (Loopbackanforderungen)

Eine *implizierte Authentifizierung* beschreibt das Verhalten von Verbindungsanforderungen, bei dem externe Prozesse in AppContainer-Instanzen, als vertrauenswürdige Benutzeridentität für SQL Server bei Loopbackanforderungen für Daten oder Vorgänge dargestellt werden. Als Konzept ist die implizite Authentifizierung nicht mehr einzigartig für die Windows-Authentifizierung, bei SQL Server-Verbindungszeichenfolgen, die eine vertrauenswürdige Verbindung angeben, und bei Anforderungen, die von externen Prozessen wie R- oder Python-Skripts stammen. Sie wird manchmal auch als *Loopback* bezeichnet.

Durch die Verwaltung von Identitäten und Anmeldeinformationen verhindert die AppContainer-Instanz die Verwendung von Benutzeranmeldeinformationen, um Zugriff auf Ressourcen zu erhalten oder eine Anmeldung bei anderen Umgebungen durchzuführen. Die AppContainer-Umgebung erstellt einen Bezeichner, der die kombinierten Identitäten des Benutzers und der Anwendung verwendet, damit die Anmeldeinformationen für jeden Benutzer und jede Anwendung eindeutig sind und die Anwendung nicht die Identität des Benutzers annehmen kann. Weitere Informationen finden Sie unter [AppContainer-Isolation](/windows/win32/secauthz/appcontainer-isolation).

Weitere Informationen zu Loopbackverbindungen finden Sie unter [Loopbackverbindung mit SQL Server mithilfe eines Python- oder R-Skripts](../connect/loopback-connection.md).

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Funktionsweise der impliziten Authentifizierung für externe Skriptsitzungen

Das folgende Diagramm zeigt die Interaktion von SQL Server-Komponenten mit der Language Runtime und die damit verbundene Authentifizierung in Windows.

![Implizite Authentifizierung in Windows](../security/media/implied-auth-windows.png)

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="implied-authentication-loopback-requests"></a>Implizite Authentifizierung (Loopbackanforderungen)

Eine *implizierte Authentifizierung* beschreibt das Verhalten von Verbindungsanforderungen, bei dem externe Prozesse, die als mssql_satellite-Benutzer in ihren eigenen Namespaces mit niedriger Berechtigung ausgeführt werden, als vertrauenswürdige Benutzeridentität für SQL Server bei Loopbackanforderungen für Daten oder Vorgänge dargestellt werden. Sie wird manchmal auch als Loopback bezeichnet.

Eine Loopbackverbindung wird mithilfe des Satellitenzertifikats aus dem Ordner „launchpad GUID“ hergestellt, um die Authentifizierung bei SQL Server mit dem Satellitenprozess durchzuführen. Die Identität des aufrufenden Benutzers wird diesen Zertifikat zugewiesen, wodurch der Satellitenprozess, der die Verbindung zu SQL Server mithilfe des Zertifikats herstellt, dem aufrufenden Benutzer zugeordnet werden kann.

Weitere Informationen finden Sie unter [Loopbackverbindung mit SQL Server über ein Python- oder R-Skript](../connect/loopback-connection.md).

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Funktionsweise der impliziten Authentifizierung für externe Skriptsitzungen

Das folgende Diagramm zeigt die Interaktion von SQL Server-Komponenten mit der Language Runtime und die damit verbundene Authentifizierung in Linux.

![Implizite Authentifizierung in Linux](../security/media/implied-auth-linux.png)

::: moniker-end

## <a name="no-support-for-transparent-data-encryption-at-rest"></a>Keine Unterstützung für Transparent Data Encryption für ruhende Daten

[Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) wird nicht für Daten unterstützt, die an die externe Skriptlaufzeit gesendet oder von ihr empfangen werden. Dies liegt daran, dass der externe Prozess außerhalb des SQL Server-Prozesses ausgeführt wird. Daher sind die von der externen Laufzeit verwendeten Daten nicht durch die Verschlüsselungsfunktionen der Datenbank-Engine geschützt. Dieses Verhalten unterscheidet sich nicht von anderen Clients auf dem SQL Server-Computer, die Daten aus der Datenbank lesen und eine Kopie erstellen.

TDE wird daher **nicht** auf in externen Skripts verwendete Daten, auf gespeicherte Daten auf Datenträgern oder auf jegliche permanente Zwischenergebnisse angewendet. Andere Verschlüsselungsarten, wie die Windows BitLocker-Verschlüsselung oder Verschlüsselungsmethoden von Drittanbietern auf Datei- oder Ordnerebene, gelten jedoch weiterhin.

Im Falle von [Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) haben externe Laufzeiten keinen Zugriff auf die Verschlüsselungsschlüssel. Daher können Daten auch nicht an die Skripts gesendet werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die Komponenten und das Interaktionsmodell der Sicherheitsarchitektur kennengelernt, die in das [Erweiterbarkeitsframework](../../machine-learning/concepts/extensibility-framework.md) integriert sind. Zu den Hauptthemen dieses Artikels gehören der Zweck von Launchpad, SQLRUserGroup und Workerkonten, die Prozessisolation von externen Skripts und die Zuordnung von Benutzeridentitäten zu Workerkonten. 

Lesen Sie im nächsten Schritt die Anweisungen zum [Erteilen von Berechtigungen](../../machine-learning/security/user-permission.md). Für Server, die eine Windows-Authentifizierung verwenden, sollten Sie auch [Erstellen von Anmeldeinformationen für SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md) lesen, um zu erfahren, wann zusätzliche Einstellungen erforderlich sind.