---
title: Beheben von Verbindungsfehlern mit der SQL Server-Datenbank-Engine
description: Informationen zum Beheben von Verbindungsproblemen Hier erfahren Sie mehr über die auszuführenden Schritte, wenn Sie über TCP/IP keine Verbindung mit einer SQL Server-Datenbank-Engine auf einem Einzelserver herstellen können.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: troubleshooting
helpviewer_keywords:
- troubleshooting, connecting to Database Engine
- connecting to Database Engine, troubleshooting
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: sqlfreshmay19
ms.date: 11/25/2019
ms.openlocfilehash: 2ef49ed5d3d46e4f5c5602972ea87347b686d9bb
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617569"
---
# <a name="troubleshoot-connecting-to-the-sql-server-database-engine"></a>Beheben von Verbindungsfehlern mit der SQL Server-Datenbank-Engine

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Artikel werden Vorgehensweisen zur Problembehandlung aufgeführt, wenn Sie keine Verbindung mit einer Instanz der SQL Server-Datenbank-Engine auf einem Einzelserver herstellen können.

>[!NOTE]
>Für anderen Szenarien siehe:
>- [Verfügbarkeitsgruppenlistener](../availability-groups/windows/listeners-client-connectivity-application-failover.md)
>- [Failoverclusterinstanzen](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

Dabei wird nicht mit den häufigsten Problemen begonnen, auf die Sie sicher schon geprüft haben. Diese Schritte sind in der Reihenfolge von den grundlegendsten Problemen bis hin zu den komplexeren. Dabei wird vorausgesetzt, dass Sie von einem anderen Computer aus über das TCP/IP-Protokoll eine Verbindung mit der SQL Server-Instanz herstellen (die gängigste Situation).

Diese Anweisungen sind für die Problembehandlung des Fehlers "**Verbinden mit dem Server**" nützlich, der `Error Number: 11001 (or 53), Severity: 20, State: 0` aufweist. Das folgende Beispiel veranschaulicht eine Fehlermeldung:

> `A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.`
>
> `(provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server) (Microsoft SQL Server, Error: 53)`
>
> `(provider: TCP Provider, error: 0 - No such host is known.) (Microsoft SQL Server, Error: 11001)`

Dieser Fehler bedeutet in der Regel, dass der Client die SQL Server-Instanz nicht finden kann. Dies geschieht normalerweise, wenn mindestens eines der folgenden Probleme aufgetreten ist:

- Name des Computers, der den SQL Server hostet
- Die Instanz löst die korrekte IP-Adresse nicht auf
- Die TCP-Portnummer ist nicht ordnungsgemäß angegeben

> [!TIP]
> Eine interaktive Seite zur Problembehebung im [!INCLUDE[msCoName_md](../../includes/msconame-md.md)]-Kundendienst unter [Solving Connectivity errors to SQL Server (Lösen von Verbindungsproblemen zu SQL Server)](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) verfügbar.

## <a name="not-included"></a>Nicht enthalten

- Dieses Thema enthält keine Informationen zu SSPI-Fehlern. Informationen zu SSPI-Fehlern finden Sie unter [Problembehandlung bei der Fehlermeldung „SSPI-Kontext kann nicht generiert werden“](https://support.microsoft.com/kb/811889).
- Dieses Thema enthält keine Informationen zu Kerberos-Fehlern. Hilfe finden Sie im [Microsoft Kerberos-Konfigurations-Manager für SQL Server](https://www.microsoft.com/download/details.aspx?id=39046).
- Dieses Thema enthält keine Informationen zur Konnektivität von Azure SQL-Datenbank. Hilfe finden Sie unter [Troubleshooting connectivity issues with Microsoft Azure SQL Database (Problembehandlung bei Verbindungsproblemen mit Microsoft Azure SQL-Datenbank)](/azure/sql-database/troubleshoot-connectivity-issues-microsoft-azure-sql-database).

## <a name="get-instance-name-from-configuration-manger"></a>Abrufen des Instanznamens vom Konfigurations-Manager

Überprüfen Sie auf dem Server, der die SQL Server-Instanz hostet, den Instanznamen. Verwenden Sie den [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md).

Der Konfigurations-Manager wird automatisch auf dem Computer installiert, wenn SQL Server installiert ist. Anweisungen zum Starten des Konfigurations-Managers variieren geringfügig, je nach SQL Server- und Windows-Version.) Versionsspezifische Details finden Sie unter [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md).)

1. Melden Sie sich auf dem Hostcomputer der SQL Server-Instanz an.
2. Starten Sie den SQL Server-Konfigurations-Manager.
3. Wählen Sie im linken Bereich **SQL Server-Dienste** aus.
4. Überprüfen Sie im rechten Bereich den Namen der Instanz der Datenbank-Engine.

   - `SQL SERVER (MSSQLSERVER)` bezeichnet eine Standardinstanz von SQL Server. Der Name der Standardinstanz ist `<computer name>`.
   - `SQL SERVER (<instance name>)` bezeichnet eine benannte Instanz von SQL Server. Der Name der benannten Instanz ist `<computer name>\<instance name>`.

## <a name="verify---the-instance-is-running"></a>Überprüfen, ob die Instanz ausgeführt wird

Um zu überprüfen, ob die Instanz ausgeführt wird, sehen Sie sich im SQL Server-Konfigurations-Manager das Symbol neben der SQL Server-Instanz an.

- Ein grüner Pfeil weist darauf hin, dass eine Instanz ausgeführt wird.
- Ein rotes Quadrat weist darauf hin, dass eine Instanz angehalten ist.

Wenn die Instanz angehalten ist, klicken Sie mit der rechten Maustaste auf die Instanz, und klicken Sie anschließend auf **Starten**. Die Serverinstanz wird gestartet, und der Indikator wird ein grüner Pfeil.

## <a name="verify---sql-server-browser-service-is-running"></a><a name = "startbrowser"></a> Überprüfen – Ausführung von SQL Server-Browser

Der SQL Server-Browserdienst muss ausgeführt werden, um eine Verbindung mit einer benannten Instanz herzustellen. Überprüfen Sie im Konfigurations-Manager, ob der Dienst **SQL Server-Browser** ausgeführt wird. Wenn er nicht ausgeführt wird, starten Sie ihn. Der SQL Server-Browserdienst ist für Standardinstanzen nicht erforderlich.

Für eine Standardinstanz von SQL Server ist kein SQL Server-Browserdienst erforderlich.

## <a name="testing-a-local-connection"></a>Testen einer lokalen Verbindung

Überprüfen Sie vor der Problembehandlung eines Verbindungsproblems von einem anderen Computer zunächst, ob Sie eine Verbindung von einer Clientanwendung aus herstellen können, die lokal auf dem Computer installiert ist, der SQL Server ausführt. Das herstellen einer lokalen Verbindung vermeidet Probleme mit Netzwerken und Firewalls. 

Dieses Verfahren verwendet SQL Server Management Studio. Wenn Sie Management Studio nicht installiert haben, besuchen Sie das [Download Center von SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md). Wenn Sie Management Studio nicht installieren können, können Sie die Verbindung mithilfe des Hilfsprogramms `sqlcmd.exe` überprüfen. `sqlcmd.exe` wird mithilfe der Datenbank-Engine installiert. Weitere Informationen zu `sqlcmd.exe`finden Sie unter [sqlcmd (Hilfsprogramm)](../../tools/sqlcmd-utility.md).)

1. Melden Sie sich mithilfe eines Anmeldenamens, der über Zugriffsberechtigung für SQL Server verfügt, auf dem Computer an, auf dem SQL Server installiert ist. (Während der Installation benötigt SQL Server mindestens einen Anmeldenamen, der als SQL Server-Administrator angegeben ist. Wenn Ihnen kein Administrator bekannt ist, finden Sie unter [Herstellen einer Verbindung mit SQL Server, wenn Systemadministratoren gesperrt sind](connect-to-sql-server-when-system-administrators-are-locked-out.md) weitere Informationen.)
2. Tippen Sie auf der Startseite oder auf älteren Versionen von Windows im Startmenü **SQL Server Management Studio** ein, zeigen Sie auf **Alle Programme** und anschließend auf **Microsoft SQL Server**, und klicken Sie anschließend auf **SQL Server Management Studio**.
3. Wählen Sie im Dialogfeld **Verbindung mit Server herstellen** , im Feld **Server** die Option **Datenbank-Engine** aus. Wählen Sie im Feld **Authentifizierung** die Option **Windows-Authentifizierung**. Geben Sie in das Feld **Servername** einen der folgenden Verbindungstypen ein:

   |Verbinden mit|type|Beispiel|
   |:-----------------|:---------------|:-----------------|
   |Standardinstanz|`<computer name>`|`ACCNT27`|
   |Benannte Instanz|`<computer name\instance name>`|`ACCNT27\PAYROLL`|

   > [!NOTE]
   > Bei der Verbindung mit einem SQL Server von einer Clientanwendung auf dem gleichen Computer wird das Shared Memory-Protokoll verwendet. Shared Memory ist ein Typ einer lokalen Named Pipe. Manchmal treten jedoch Fehler bezüglich der Pipes auf.

   Wenn Sie zu diesem Zeitpunkt eine Fehlermeldung erhalten, müssen Sie den Fehler beheben, bevor Sie fortfahren. Viele Dinge könnten ein Problem darstellen. Ihr Anmeldename ist möglicherweise nicht autorisiert, eine Verbindung herzustellen. Möglicherweise ist Ihre Standarddatenbank nicht vorhanden.

   > [!NOTE]
   > Einige Fehlermeldungen, die absichtlich an den Client übergeben wurden, stellen nicht genügend Informationen dazu bereit, wie das Problem behoben werden kann. Dies ist eine Sicherheitsfunktion, die verhindern soll, dass Informationen über SQL Server an einen Angreifer übermittelt werden. Sehen Sie sich das SQL Server-Fehlerprotokoll an, um alle Informationen über den Fehler anzuzeigen. Dort finden Sie die Einzelheiten. 

4. Wenn Sie den Fehler `18456 Login failed for user` erhalten, sehen Sie sich das Thema [MSSQLSERVER_18456](../../relational-databases/errors-events/mssqlserver-18456-database-engine-error.md) in der Onlinedokumentation an, das weitere Informationen zu Fehlercodes enthält. Auf Aaron Bertrands Blog unter [Troubleshooting Error 18456](https://sqlblog.org/2011/01/14/troubleshooting-error-18456) (Problembehandlung von Fehler 18456) finden Sie ebenfalls eine umfangreiche Liste mit Fehlercodes. Sie können das Fehlerprotokoll mit SSMS (falls Sie eine Verbindung herstellen können) im Abschnitt „Verwaltung“ des Objekt-Explorers anzeigen. Andernfalls können Sie das Fehlerprotokoll mit dem Windows-Editor anzeigen. Der standardmäßige Speicherort variiert je nach Version und kann während des Setups geändert werden. Der Standardspeicherort für [!INCLUDE[ssSQL15_md](../../includes/sssqlv15-md.md)] ist `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Log\ERRORLOG`. 

5. Wenn Sie keine Verbindung mithilfe von Shared Memory herstellen können, versuchen Sie eine Verbindung mithilfe von TCP herzustellen. Sie können eine TCP-Verbindung erzwingen, indem Sie `tcp:` vor dem Namen angeben. Beispiel:

   |Verbinden mit:|Typ:|Beispiel:|
   |-----------------|---------------|-----------------|
   |Standardinstanz|`tcp:<computer name>`|`tcp:ACCNT27`|
   |Benannte Instanz|`tcp:<computer name/instance name>`|`tcp:ACCNT27\PAYROLL`|

6. Wenn Sie zwar mit Shared Memory eine Verbindung herstellen können, jedoch nicht mit TCP, müssen Sie das TCP-Problem beheben. Am wahrscheinlichsten ist es, dass TCP nicht aktiviert ist. Führen Sie die obigen Schritte zum [Aktivieren von Protokollen](#enableprotocols) aus, um TCP zu aktivieren.

7. Wenn Sie sich mit einem anderen Konto als mit dem Administratorkonto verbinden möchten, versuchen Sie, erneut eine Verbindung herzustellen. Dazu verwenden Sie die Windows-Authentifizierungsanmeldung oder die SQL Server-Authentifizierungsanmeldung, die von der Clientanwendung verwendet wird.

## <a name="get-the-ip-address-of-the-server"></a>Abrufen der IP-Adresse des Servers

Rufen Sie die IP-Adresse des Hostcomputers der Instanz von SQL Server ab.

1. Klicken Sie im Startmenü auf **Ausführen**. Geben Sie im Fenster **Ausführen** **cmd** ein, und klicken Sie anschließend auf **OK**.
2. Geben Sie im Eingabeaufforderungsfenster `ipconfig` ein, und drücken Sie die EINGABETASTE. Notieren Sie sich die **IPv4** Adresse und die **IPv6** Adresse.

  >SQL Server kann sowohl mit dem IP-Protokoll der Version 4 als auch dem der Version 6 eine Verbindung herstellen. Ihr Netzwerk kann eine oder beide Möglichkeiten zulassen. Die meisten Benutzer fangen an, indem sie die Probleme der **IPv4** -Adresse behandeln. Dies ist kürzer und leichter einzugeben

## <a name="get-the-sql-server-instance-tcp-port"></a><a name = "getTCP"></a>Abrufen des TCP-Ports der SQL Server-Instanz

In den meisten Fällen stellen Sie eine Verbindung mit der Datenbank-Engine von einem anderen Computer aus her, der das TCP-Protokoll verwendet.

1. Verwenden Sie SQL Server Management Studio auf dem Computer, auf dem SQL Server ausgeführt wird, und stellen Sie eine Verbindung mit SQL Server her. Erweitern Sie im Objekt-Explorer **Management** und **SQL Server-Protokolle**, und doppelklicken Sie auf das aktuelle Protokoll.
2. Klicken Sie im Protokoll-Viewer auf der Symbolleiste auf die Schaltfläche **Filter** . Geben Sie in das Feld **Meldung enthält Text**`server is listening on` ein, klicken Sie auf **Filter anwenden** und anschließend auf **OK**.
3. Eine Meldung wie `Server is listening on [ 'any' <ipv4> 1433]` sollte aufgelistet werden. 

Diese Meldung gibt an, dass diese Instanz von SQL Server an alle IP-Adressen auf diesen Computer (für IP-Version 4) lauscht sowie an den TCP-Port 1433. (TCP-Port 1433 ist in der Regel der Port, der von der Datenbank-Engine oder einer Standardinstanz von SQL Server verwendet wird. Nur eine Instanz von SQL Server kann einen Port verwenden. Falls also mehrere Instanzen von SQL Server installiert sind, müssen einige Instanzen andere Portnummern verwenden.) Notieren Sie sich die von der [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Instanz verwendete Portnummer, mit der Sie eine Verbindung herstellen möchten.

  > [!NOTE]
  > Wahrscheinlich wird `IP address 127.0.0.1` aufgelistet. Diese Adresse wird als „Loopbackadapter-Adresse“ bezeichnet. Nur Prozesse, die sich auf demselben Computer befinden, können damit eine Verbindung herstellen. Sie kann für die Problembehandlung nützlich sein, aber Sie können diese Adresse nicht für Verbindungen von einem anderen Computer aus verwenden.

## <a name="enable-protocols"></a><a name = "enableprotocols"></a>Aktivieren von Protokollen

Bei einigen Installationen von SQL Server ist die Verbindung mit der Datenbank-Engine von einem anderen Computer aus nicht aktiviert, es sei denn, ein Administrator verwendet den Konfigurations-Manager, um die Verbindung zu aktivieren. So aktivieren Sie Verbindungen von einem anderen Computer:

1. Öffnen Sie den SQL Server-Konfigurations-Manager, wie zuvor beschrieben.
2. Erweitern Sie im linken Bereich des Konfigurations-Managers **SQL Server-Netzwerkkonfiguration**, und wählen Sie anschließend die SQL Server-Instanz, mit der Sie eine Verbindung herstellen möchten. Der rechte Bereich listet die verfügbaren Verbindungsprotokolle auf. Shared Memory ist in der Regel aktiviert. Es kann nur auf demselben Computer verwendet werden, deshalb haben die meisten Installationen Shared Memory aktiviert. Sie verwenden in der Regel TCP/IP, um eine Verbindung mit SQL Server von einem anderen Computer aus herzustellen. Falls TCP/IP nicht aktiviert ist, klicken Sie mit der rechten Maustaste auf **TCP/IP** und anschließend auf **Aktivieren**.
3. Falls Sie die Aktivierungseinstellung für ein Protokoll geändert haben, starten Sie die Datenbank-Engine erneut. Wählen Sie im linken Bereich **SQL Server-Dienste** aus. Klicken Sie im rechten Bereich mit der rechten Maustaste auf die Instanz der Datenbank-Engine, und klicken Sie anschließend auf **Neu starten**.

## <a name="testing-tcpip-connectivity"></a><a name="testTCPIP"></a>Testen der TCP-/IP-Konnektivität

Das Herstellen einer Verbindung mit SQL Server über TCP/IP erfordert, dass Windows die Verbindung herstellen kann. Verwenden Sie das `ping` -Tool zum Testen von TCP.

1. Klicken Sie im Startmenü auf **Ausführen**. Geben Sie im Fenster **Ausführen** **cmd** ein, und klicken Sie anschließend auf **OK**. 
2. Geben Sie im Eingabeaufforderungsfenster `ping <ip address>` ein, und anschließend die IP-Adresse des Computers, auf dem SQL Server ausgeführt wird. Beispiel:

    - IPv4: `ping 192.168.1.101`
    - IPv6: `ping fe80::d51d:5ab5:6f09:8f48%11`

3. Wenn Ihr Netzwerk richtig konfiguriert ist, gibt B. `ping``Reply from <IP address>` zurück, gefolgt von einigen zusätzlichen Informationen. Wenn `ping``Destination host unreachable` oder `Request timed out` zurückgibt, ist TCP/IP nicht ordnungsgemäß konfiguriert. An diesem Punkt können Fehler auf ein Problem mit dem Clientcomputer oder dem Servercomputer hindeuten, oder dass etwas mit dem Netzwerk, z.B. dem Router, nicht in Ordnung ist. Informationen zur Behandlung von Netzwerkproblemen finden Sie unter [Erweiterte Behandlung von TCP/IP-Problemen](/windows/client-management/troubleshoot-tcpip).
4. Wenn der Ping-Test mithilfe der IP-Adresse erfolgreich war, überprüfen Sie, ob der Computername in einer TCP/IP-Adresse aufgelöst wird. Geben Sie im Eingabeaufforderungsfenster auf dem Clientcomputer `ping` ein, und anschließend den Computernamen, auf dem SQL Server ausgeführt wird. Beispiel: `ping newofficepc`.
5. Wenn das `ping` an die IP-Adresse erfolgreich ist, `ping` aber `Destination host unreachable` oder `Request timed out` an den Computer zurückgibt, besitzen Sie möglicherweise alte (veraltete) Namensauflösungsinformationen, die auf dem Clientcomputer zwischengespeichert sind. Tippen Sie `ipconfig /flushdns` ein, um den DNS-Cache (Dynamic Name Resolution) zu leeren. Pingen Sie den Computer anschließend mithilfe des Namens erneut an. Der Clientcomputer sucht mit dem geleerten DNS-Cache nach den neuesten Informationen zur IP-Adresse für den Servercomputer.
6. Wenn Ihr Netzwerk richtig konfiguriert ist, gibt B. `ping``Reply from <IP address>` zurück, gefolgt von einigen zusätzlichen Informationen. Wenn Sie den Servercomputer erfolgreich unter der IP-Adresse pingen können, jedoch beim Pingen mit dem Computernamen einen Fehler wie `Destination host unreachable.` oder `Request timed out.` erhalten, ist die Namensauflösung nicht richtig konfiguriert. (Weitere Informationen finden Sie im Artikel aus dem Jahr 2006, der zuvor erwähnt wurde, [Chapter 16 – Troubleshooting TCP/IP](https://support.microsoft.com/kb/169790) (Kapitel 16 – Problembehebung von TCP/IP).) Die erfolgreiche Namensauflösung ist für die Verbindung mit SQL Server nicht erforderlich. Wenn der Computername jedoch nicht in eine IP-Adresse aufgelöst werden kann, müssen die Verbindungen unter Angabe der IP-Adresse hergestellt werden. Die Namensauflösung kann später noch korrigiert werden.

## <a name="open-a-port-in-the-firewall"></a>Öffnen eines Ports in der Firewall

Die Windows-Firewall ist standardmäßig aktiviert und blockiert Verbindungen von einem anderen Computer. Sie müssen auf dem SQL Server-Computer die Firewall so konfigurieren, dass sie Verbindungen zum TCP-Port zulässt, der von der Datenbank-Engine verwendet wird, um eine Verbindung mithilfe vom TCP/IP von einem anderen Computer herzustellen. Die Standardinstanz lauscht standardmäßig an TCP-Port 1433. Wenn Sie über benannte Instanzen verfügen oder den Port der Standardinstanz geändert haben, lauscht der [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] TCP-Port möglicherweise an einem anderen Port. Siehe [Abrufen des TCP-Ports der SQL Server-Instanz](#getTCP).

Wenn Sie eine Verbindung mit einer benannten Instanz oder einem anderen Port als TCP-Port 1433 herstellen, müssen Sie auch den UDP-Port 1434 für den SQL Server-Browserdienst öffnen. Eine Schritt-für-Schritt-Anleitung zum Öffnen eines Ports in der Windows-Firewall finden Sie unter [Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](configure-a-windows-firewall-for-database-engine-access.md).

## <a name="test-the-connection"></a>Testen der Verbindung

Wenn Sie eine Verbindung mithilfe von TCP auf dem gleichen Computer herstellen können, dann ist es an der Zeit, eine Verbindung vom Clientcomputer herzustellen. Theoretisch können Sie jede beliebige Clientanwendung verwenden. Um jedoch zusätzliche Komplexität zu vermeiden, installieren Sie die SQL Server Management-Tools auf dem Client, und führen Sie einen Versuch mit SQL Server Management Studio durch.

1. Versuchen Sie auf dem Client-Computer mit SQL Server Management Studio, eine Verbindung über die IP-Adresse und die TCP-Portnummer im Format „IP-Adresse, Komma (,), Portnummer“ herzustellen. Beispiel: `192.168.1.101,1433`. Wenn diese Verbindung fehlschlägt, haben Sie wahrscheinlich eines der folgenden Probleme:

   - Ein `ping` der IP-Adresse funktioniert nicht, was auf ein allgemeines TCP-Konfigurationsproblem hinweist. Navigieren Sie zurück zum Abschnitt [Testen der TCP-/IP-Konnektivität](#testTCPIP).
   - Das TCP-Protokoll ist für SQL Server nicht aktiviert. Navigieren Sie zurück zum Abschnitt [Aktivieren von Protokollen](#enableprotocols).
   - SQL Server lauscht an einen anderen Port als den Port, die Sie angegeben haben. Wechseln Sie zurück zum Abschnitt [Abrufen der TCP-Portnummer](#getTCP).
   - Der SQL Server TCP-Port wird durch die Firewall blockiert. Navigieren Sie zurück zum Abschnitt [Öffnen eines Ports in der Firewall](#open-a-port-in-the-firewall).

2. Sobald Sie eine Verbindung mithilfe der IP-Adresse und der Portnummer herstellen können, versuchen Sie eine Verbindung mit der IP-Adresse ohne eine Portnummer herzustellen. Für eine Standardinstanz müssen Sie nur die IP-Adresse verwenden. Für eine benannte Instanz verwenden Sie die IP-Adresse und den Instanznamen im Format „IP-Adresse umgekehrter Schrägstrich (\) Instanzname“, also z.B. `192.168.1.101\<instance name>`. Wenn dies nicht funktioniert, haben Sie wahrscheinlich eines der folgenden Probleme:

   - Wenn Sie eine Verbindung mit der Standardinstanz herstellen, lauscht diese möglicherweise an einem anderen Port als TCP-Port 1433, und der Client versucht nicht, eine Verbindung mit der richtigen Portnummer herzustellen.
   - Wenn Sie eine Verbindung mit einer benannten Instanz herstellen, wird die Portnummer nicht an den Client zurückgegeben.

   Beide Probleme beziehen sich auf den SQL Server-Browserdienst, der die Portnummer an dem Client bereitstellt. Die Lösungen sind:

   - Starten des SQL-Browserdiensts. Sehen Sie in den Anleitungen zum [Starten des Browsers im SQL Server-Konfigurations-Manager](#startbrowser) nach.
   - Der SQL Server-Browserdienst wird durch die Firewall blockiert. Öffnen von UDP-Port 1434 in der Firewall. Navigieren Sie zurück zum Abschnitt [Öffnen eines Ports in der Firewall](#open-a-port-in-the-firewall). Stellen Sie sicher, dass Sie keinen TCP-Port, sondern einen UDP-Port öffnen.
   - Die Informationen über den UDP-Port 1434 werden durch einen Router gesperrt. Die UDP-Kommunikation (User Datagram-Protokoll) ist nicht dafür ausgelegt, Router zu durchlaufen. Dadurch wird verhindert, dass das Netzwerk mit Datenverkehr mit niedriger Priorität gefüllt wird. Sie können Ihren Router so konfigurieren, dass er UDP-Datenverkehr weiterleitet, oder Sie geben immer die Portnummer an, wenn Sie eine Verbindung herstellen.
   - Wenn der Clientcomputer Windows 7 oder Windows Server 2008 (oder ein neueres Betriebssystem) verwendet, wird der UDP-Datenverkehr möglicherweise vom Clientbetriebssystem gelöscht, da die Antwort vom Server von einer anderen IP-Adresse zurückgegeben wird, als von der, von der abgefragt wurde. Hierbei handelt es sich um eine Sicherheitsfunktion, die „Loose Source Mapping“ (Lockere Quellzuordnung) blockiert. Weitere Informationen finden Sie im Abschnitt **Mehrere Server-IP-Adressen** der Onlinedokumentation [Problembehandlung: Das Timeout ist abgelaufen](/previous-versions/sql/sql-server-2008-r2/ms190181(v=sql.105)). Dies ist ein Artikel aus SQL Server 2008 R2, aber die Prinzipale sind weiterhin gültig. Sie können den Client so konfigurieren, dass er die korrekte IP-Adresse verwendet, oder Sie geben immer die Portnummer an, wenn Sie eine Verbindung herstellen.

3. Nachdem Sie eine Verbindung mithilfe der IP-Adresse (oder mithilfe der IP-Adresse und des Instanznamens für eine benannte Instanz) hergestellt haben, versuchen Sie, eine Verbindung über den Computernamen (oder über einen Computernamen und einen Instanznamen für eine benannte Instanz) herzustellen. Fügen Sie `tcp:` vor dem Computernamen ein, um eine TCP/IP-Verbindung zu erzwingen. Verwenden Sie `ACCNT27`für die Standardinstanz auf einem Computer mit dem Namen `tcp:ACCNT27` . Verwenden Sie `PAYROLL`für eine benannte Instanz mit dem Namen `tcp:ACCNT27\PAYROLL` auf diesem Computer. Wenn Sie eine Verbindung über die IP-Adresse, jedoch nicht über den Computernamen herstellen können, haben Sie ein Problem mit der Auflösung des Computernamens. Navigieren Sie zurück zu Abschnitt 4 mit dem Titel **Testen der TCP-/IP-Konnektivität**.

4. Nachdem Sie eine Verbindung über den Computernamen hergestellt haben und TCP erzwungen haben, versuchen Sie, eine Verbindung über den Computernamen herzustellen und das TCP nicht zu erzwingen. Verwenden Sie für eine Standardinstanz z.B. nur den Computernamen, wie `CCNT27` . Verwenden Sie für eine benannte Instanz den Computernamen und den Instanznamen, z.B. `ACCNT27\PAYROLL` . Wenn Sie eine Verbindung herstellen können, während Sie das TCP erzwingen, aber nicht ohne das TCP zu erzwingen, verwendet der Client möglicherweise ein anderes Protokoll (so wie Named Pipes).

    1. Erweitern Sie mithilfe des SQL Server-Konfigurations-Managers auf dem Clientcomputer im linken Bereich **SQL Native Client** *Version* **Konfiguration**, und klicken Sie dann auf **Client Protocols** (Clientprotokolle).
    2. Überprüfen Sie, ob TCP/IP im rechten Bereich aktiviert ist. Falls TCP/IP nicht aktiviert ist, klicken Sie mit der rechten Maustaste auf **TCP/IP** und anschließend auf **Aktivieren**.
    3. TC/IP muss in der Reihenfolge der Protokolle vor „Named Pipes“ (oder „VIA“ für ältere Versionen) stehen. Im Allgemeinen sollten Sie Shared Memory als Reihenfolge 1 und TCP/IP als Reihenfolge 2 lassen. Shared Memory wird nur verwendet, wenn der Client und SQL Server auf dem gleichen Computer ausgeführt werden. Alle aktivierten Protokolle werden in dieser Reihenfolge getestet, bis eine Verbindung hergestellt werden kann. Wenn die Verbindung nicht mit demselben Computer hergestellt wird, wird gemeinsam genutzter Speicher jedoch übersprungen.