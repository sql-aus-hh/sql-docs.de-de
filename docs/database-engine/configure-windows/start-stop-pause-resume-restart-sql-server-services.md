---
title: Starten, Beenden, Anhalten, Fortsetzen und Neustarten von SQL Server-Diensten
description: Hier erfahren Sie, wie Sie verschiedene SQL Server-Dienste starten, beenden, anhalten, fortsetzen oder neu starten. Zudem erfahren Sie, wie Sie Transact-SQL, PowerShell und andere Tools für diese Aktionen verwenden können.
ms.custom: ''
ms.date: 03/05/2020
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 697cf043c4582796015b061c857145c214d105fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091765"
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>Starten, Beenden, Anhalten, Fortsetzen und Neustarten von SQL Server-Diensten

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Artikel wird erläutert, wie die SQL Server-Datenbank-Engine, der SQL Server-Agent oder der SQL Server-Browser-Dienst mithilfe des SQL Server-Konfigurations-Managers, mit SQL Server Management Studio (SSMS), mithilfe von Net-Befehlen über eine Eingabeaufforderung oder mithilfe von Transact-SQL oder PowerShell gestartet, beendet, angehalten, fortgesetzt und neu gestartet werden.

## <a name="identify-the-service"></a>Informationen zum Dienst

SQL Server-Komponenten sind ausführbare Programme, die als Windows-Dienst ausgeführt werden. Programme, die als Windows-Dienst ausgeführt werden, lassen sich ohne Anzeige von Aktivitäten auf dem Computerbildschirm weiterhin ausführen.

### <a name="database-engine-service"></a>Datenbank-Engine-Dienst

Hierbei handelt es sich um den ausführbaren Prozess, der die SQL Server-Datenbank-Engine ist. Die Datenbank-Engine kann der Standardinstanz (eine pro Computer) oder einer von vielen benannten Instanzen der Datenbank-Engine entsprechen. Verwenden Sie den SQL Server-Konfigurations-Manager, um zu bestimmen, welche Instanzen der Datenbank-Engine auf dem Computer installiert sind. Die Standardinstanz wird im Fall der Installation als **SQL Server (MSSQLSERVER)** aufgeführt. Benannte Instanzen werden im Fall der Installation als **SQL Server (<instance_name>)** aufgelistet. Standardmäßig wird SQL Server Express als **SQL Server (SQLEXPRESS)** installiert.

### <a name="sql-server-agent-service"></a>SQL Server-Agent-Dienst

Entspricht einem Windows-Dienst, der geplante administrative Tasks ausführt, die als Aufträge und Warnungen bezeichnet werden. Weitere Informationen finden Sie unter [SQL Server Agent](../../ssms/agent/sql-server-agent.md). Der SQL Server-Agent ist nicht in jeder Version von SQL Server verfügbar. Eine Liste der Features, die von den SQL Server-Versionen unterstützt werden, finden Sie unter [Versionen und unterstützte Features von SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).

### <a name="sql-server-browser-service"></a>SQL Server-Browserdienst

Hierbei geht es um einen Windows-Dienst, der auf eingehende Anforderungen für Microsoft SQL Server-Ressourcen lauscht und Clientinformationen zu den auf dem Computer installierten SQL Server-Instanzen bereitstellt. Eine einzelne Instanz des SQL Server-Browserdiensts wird für alle auf dem Computer installierten SQL Server-Instanzen verwendet.

### <a name="additional-information"></a>Zusätzliche Informationen

- Durch das Anhalten des Datenbank-Engine-Diensts wird verhindert, dass neue Benutzer eine Verbindung mit der Datenbank-Engine herstellen. Benutzer, die bereits verbunden sind, können jedoch ihre Arbeit fortsetzen, bis die jeweilige Verbindung unterbrochen wird. Halten Sie den Dienst an, wenn Benutzer zuerst ihre Arbeit abschließen sollen, bevor Sie den Dienst beenden. Dadurch können sie Transaktionen abschließen, die gerade verarbeitet werden. Mit der Funktion zum *Fortsetzen* kann die Datenbank-Engine neue Verbindungen wieder zulassen. Der SQL Server-Agent-Dienst kann nicht angehalten oder fortgesetzt werden.  

- Der SQL Server-Konfigurations-Manager und SSMS zeigen den aktuellen Dienststatus an. Dazu werden folgende Symbole verwendet.  

    **SQL Server-Konfigurations-Manager**

  - Ein grüner Pfeil im Symbol neben dem Dienstnamen gibt an, dass der Dienst gestartet wurde.

  - Ein rotes Quadrat im Symbol neben dem Dienstnamen gibt an, dass der Dienst beendet wurde.

  - Zwei vertikale blaue Linien im Symbol neben dem Dienstnamen geben an, dass der Dienst angehalten wurde.

  - Beim Neustart der Datenbank-Engine weist ein rotes Quadrat darauf hin, dass der Dienst beendet wurde. Ein grüner Pfeil gibt daraufhin an, dass der Dienst erfolgreich gestartet wurde.

    **SQL Server Management Studio (SSMS)**

  - Ein weißer Pfeil in einem grünen Kreis neben dem Dienstnamen gibt an, dass der Dienst gestartet wurde.  

  - Ein weißes Quadrat in einem roten Kreis neben dem Dienstnamen gibt an, dass der Dienst beendet wurde.  

  - Zwei vertikale weiße Linien in einem blauen Kreis neben dem Dienstnamen geben an, dass der Dienst angehalten wurde.  

- Bei der Verwendung des SQL Server-Konfigurations-Manager und von SSMS sind beide möglichen Funktionen verfügbar. Wurde der Dienst beispielsweise bereits gestartet, ist die Option **Start** nicht verfügbar.

- Im Fall der Ausführung auf einem Cluster lässt sich der SQL Server-Datenbank-Engine-Dienst am besten mittels Clusterverwaltung verwalten.  

### <a name="security"></a>Sicherheit

#### <a name="permissions"></a>Berechtigungen

Standardmäßig können nur Mitglieder der lokalen Administratorgruppe einen Dienst starten, beenden, anhalten, fortsetzen oder neu starten. Informationen dazu, wie Sie es Nichtadministratoren ermöglichen, Dienste zu verwalten, finden Sie unter [How to grant users rights to manage services in Windows Server 2003](https://support.microsoft.com/kb/325349)(So erteilen Sie Benutzern die Berechtigung zum Verwalten von Diensten in Windows Server 2003). (Dieser Vorgang ist bei anderen Versionen von Windows Server ähnlich.)

Das Beenden der Datenbank-Engine unter Verwendung des Transact-SQL-**SHUTDOWN**-Befehls erfordert die Mitgliedschaft in den festen Serverrollen **sysadmin** oder **serveradmin**. Diese Mitgliedschaft ist nicht übertragbar.

## <a name="sql-server-configuration-manager"></a>SQL Server-Konfigurations-Manager

### <a name="starting-sql-server-configuration-manager"></a>Starten des SQL Server-Konfigurations-Managers

Zeigen Sie im Menü **Start** auf **Alle Programme**, **Microsoft SQL Server**, **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.

Da der SQL Server-Konfigurations-Manager ein Snap-In für das Microsoft Management Console-Programm und kein eigenständiges Programm ist, wird der SQL Server-Konfigurations-Manager in neueren Versionen von Windows nicht als Anwendung angezeigt. Nachfolgend sind die Pfade der letzten vier Versionen aufgeführt (unter der Annahme, dass Windows auf Laufwerk C installiert ist).  

|Version|`Path`|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|SQL Server 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|SQL Server 2014|C:\Windows\SysWOW64\SQLServerManager12.msc|
|SQL Server 2012|C:\Windows\SysWOW64\SQLServerManager11.msc|

#### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="configmande"></a> Starten, Beenden, Anhalten, Fortsetzen und Neustarten einer Instanz der SQL Server-Datenbank-Engine

1. Starten Sie den SQL Server-Konfigurations-Manager mithilfe der oben genannten Anweisungen.

2. Wenn das Dialogfeld **Benutzerkontensteuerung** angezeigt wird, klicken Sie auf **Ja**.

3. Klicken Sie im linken Bereich des SQL Server-Konfigurations-Managers auf **SQL Server-Dienste**.

4. Klicken Sie im Ergebnisbereich mit der rechten Maustaste auf **SQL Server (MSSQLServer)** oder auf eine benannte Instanz, und klicken Sie anschließend auf **Starten**, **Beenden**, **Anhalten**, **Fortsetzen** oder **Neu starten**.

5. Klicken Sie auf **OK**, um den SQL Server-Konfigurations-Manager zu schließen.

> [!NOTE]
> Weitere Informationen zum Starten einer SQL Server-Datenbank-Engine-Instanz mit Startoptionen finden Sie unter [SCM-Dienste: Konfigurieren der Serverstartoptionen](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  

#### <a name="to-start-stop-pause-resume-or-restart-the-sql-server-browser-or-an-instance-of-the-sql-server-agent"></a>Starten, Beenden, Anhalten, Fortsetzen oder Neustarten des SQL Server-Browsers oder einer Instanz des SQL Server-Agents

1. Starten Sie den SQL Server-Konfigurations-Manager mithilfe der oben genannten Anweisungen.

2. Wenn das Dialogfeld **Benutzerkontensteuerung** angezeigt wird, klicken Sie auf **Ja**.

3. Klicken Sie im linken Bereich des SQL Server-Konfigurations-Managers auf **SQL Server-Dienste**.

4. Klicken Sie im Ergebnisbereich mit der rechten Maustaste auf **SQL Server-Browser**, oder auf **SQL Server-Agent** (MSSQLServer) oder für eine benannte Instanz auf **SQL Server-Agent (<Instanz_Name>)** , und klicken Sie dann auf **Starten**, **Beenden**, **Anhalten**, **Fortsetzen** oder **Neu starten**.  

5. Klicken Sie auf **OK**, um den SQL Server-Konfigurations-Manager zu schließen.  

> [!NOTE]
> Der SQL Server-Agent kann nicht angehalten werden.

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="ssmsde"></a> Starten, Beenden, Anhalten, Fortsetzen und Neustarten einer Instanz der SQL Server-Datenbank-Engine

1. Stellen Sie im Objekt-Explorer eine Verbindung mit der Datenbank-Engine-Instanz her, klicken Sie mit der rechten Maustaste auf die zu startende Datenbank-Engine-Instanz und anschließend auf **Starten**, **Beenden**, **Anhalten**, **Fortsetzen** oder **Neu starten**.

    Klicken Sie alternativ im Bereich „Registrierte Server“ mit der rechten Maustaste auf die zu startende Datenbank-Engine-Instanz, zeigen Sie auf die Option **Dienstkontrolle**, und klicken Sie anschließend auf **Starten**, **Beenden**, **Anhalten**, **Fortsetzen** oder **Neu starten**.

2. Wenn das Dialogfeld **Benutzerkontensteuerung** angezeigt wird, klicken Sie auf **Ja**.

3. Wenn Sie gefragt werden, ob Sie eine Aktion ausführen möchten, klicken Sie auf **Ja**.  

#### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-agent"></a>Starten, Beenden oder Neustarten einer SQL Server-Agent-Instanz

1. Stellen Sie im Objekt-Explorer eine Verbindung mit der Datenbank-Engine-Instanz her, klicken Sie mit der rechten Maustaste auf **SQL Server-Agent**, und klicken Sie anschließend auf **Starten**, **Beenden** oder **Neu starten**.

2. Wenn das Dialogfeld **Benutzerkontensteuerung** angezeigt wird, klicken Sie auf **Ja**.

3. Wenn Sie gefragt werden, ob Sie eine Aktion ausführen möchten, klicken Sie auf **Ja**.

## <a name="command-prompt-window-using-net-commands"></a><a name="CommandPrompt"></a> Eingabeaufforderungsfenster und Verwenden von Net-Befehlen

Die Microsoft SQL Server-Dienste können mithilfe der **Net**-Befehle von Microsoft Windows gestartet, beendet oder angehalten werden.

### <a name="to-start-the-default-instance-of-the-database-engine"></a><a name="dbDefault"></a> Starten der Standardinstanz der Datenbank-Engine

- Geben Sie an einer Eingabeaufforderung einen der folgenden Befehle ein:  
  
    **net start "SQL Server (MSSQLSERVER)"**

   Oder  

    **net start MSSQLSERVER**

### <a name="to-start-a-named-instance-of-the-database-engine"></a><a name="dbNamed"></a> Starten einer benannten Instanz der Datenbank-Engine

- Geben Sie an einer Eingabeaufforderung einen der folgenden Befehle ein. Ersetzen Sie *\<instancename>* durch den Namen der Instanz, die Sie verwalten möchten.  
  
    **net start "SQL Server (** *Instanzname* **)"**
  
   Oder  
  
    **net start MSSQL$** *Instanzname*  
  
### <a name="to-start-the-database-engine-with-startup-options"></a><a name="dbStartup"></a> Starten der Datenbank-Engine mit Startoptionen  

- Fügen Sie Startoptionen am Ende der Anweisung **net start "SQL Server (MSSQLSERVER)"** hinzu (durch ein Leerzeichen getrennt). Beim Starten mithilfe von **net start** wird ein Schrägstrich (/) anstelle eines Bindestriches (-) für die Startoptionen verwendet.  
  
    **net start "SQL Server (MSSQLSERVER)" /f /m**
  
   Oder  
  
    **net start MSSQLSERVER /f /m**
  
  > [!NOTE]
  >  Weitere Informationen finden Sie unter [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
###  <a name="to-start-the-sql-server-agent-on-the-default-instance-of-sql-server"></a><a name="agDefault"></a> Starten des SQL Server-Agents auf der Standardinstanz von SQL Server
  
- Geben Sie an einer Eingabeaufforderung einen der folgenden Befehle ein:  
  
    **net start "SQL Server-Agent (MSSQLSERVER)"**
  
   Oder  
  
    **net start SQLSERVERAGENT**
  
###  <a name="to-start-the-sql-server-agent-on-a-named-instance-of-sql-server"></a><a name="agNamed"></a> Starten des SQL Server-Agents auf einer benannten Instanz von SQL Server  
  
- Geben Sie an einer Eingabeaufforderung einen der folgenden Befehle ein. Ersetzen Sie *Instanzname* durch den Namen der Instanz, die Sie verwalten möchten.  
  
    **net start "SQL Server Agent(** *Instanzname* **)"**
  
   Oder  
  
    **net start SQLAgent$** *Instanzname*  
  
 Informationen zum Ausführen des SQL Server-Agents im ausführlichen Modus zur Problembehandlung finden Sie unter [sqlagent90 (Anwendung)](../../tools/sqlagent90-application.md).  

### <a name="to-start-the-sql-server-browser"></a><a name="Browser"></a> Starten des SQL Server-Browsers  

- Geben Sie an einer Eingabeaufforderung einen der folgenden Befehle ein:  
  
    **net start "SQL Server Browser"**
  
   Oder  
  
    **net start SQLBrowser**
  
### <a name="to-pause-or-stop-services-from-the-command-prompt-window"></a><a name="pauseStop"></a> So werden Dienste über das Eingabeaufforderungsfenster angehalten oder beendet  

- Ändern Sie zum Anhalten oder Beenden von Diensten die Befehle wie folgt.  

- Um einen Dienst anzuhalten, ersetzen Sie **net start** durch **net pause**.  

- Um einen Dienst zu beenden, ersetzen Sie **net start** durch **net stop**.  

## <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL

Die Datenbank-Engine lässt sich mit der **SHUTDOWN**-Anweisung beenden.  

### <a name="to-stop-the-database-engine-using-transact-sql"></a>Beenden der Datenbank-Engine mithilfe von Transact-SQL

- Führen Sie die folgende Anweisung aus, um die Datenbank-Engine nach der vollständigen Ausführung der Transact-SQL-Anweisungen und gespeicherten Prozeduren zu beenden.  
  
    ```sql
    SHUTDOWN;
    ```
  
- Führen Sie die folgende Anweisung aus, um die Datenbank-Engine sofort zu beenden.  
  
    ```sql
    SHUTDOWN WITH NOWAIT;
    ```

Weitere Informationen zur **SHUTDOWN**-Anweisung finden Sie unter [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md).  

## <a name="powershell"></a><a name="PowerShellProcedure"></a> PowerShell

### <a name="to-start-and-stop-database-engine-services"></a>Starten und Beenden von Datenbank-Engine-Diensten

1. Starten Sie in einem Eingabeaufforderungsfenster SQL Server PowerShell durch das Ausführen des folgenden Befehls.  

    ```cmd
    sqlps
    ```

2. Führen Sie an einer SQL Server PowerShell-Eingabeaufforderung den folgenden Befehl aus. Ersetzen Sie `computername` durch den Namen des Computers.  

    ```powershell
    # Get a reference to the ManagedComputer class.
    CD SQLSERVER:\SQL\computername
    $Wmi = (get-item .).ManagedComputer
    ```

3. Identifizieren Sie den Dienst, den Sie beenden oder starten möchten. Wählen Sie eine der folgenden Zeilen aus. Ersetzen Sie `instancename` durch den Namen der benannten Instanz.

    - Abrufen eines Verweises auf die Standardinstanz der Datenbank-Engine

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQLSERVER']
        ```

    - Abrufen eines Verweises auf die benannte Instanz der Datenbank-Engine

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQL$instancename']
        ```

    - Abrufen eines Verweises auf den SQL Server-Agent-Dienst auf der Standardinstanz der Datenbank-Engine  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']
        ```

    - Abrufen eines Verweises auf den SQL Server-Agent-Dienst auf einer benannten Instanz der Datenbank-Engine  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']
        ```

    - Abrufen eines Verweises auf den SQL Server-Browserdienst

        ```powershell
        $DfltInstance = $Wmi.Services['SQLBROWSER']
        ```

4. Starten Sie anhand des Beispiels den ausgewählten Dienst, und beenden Sie ihn anschließend.
  
    ```powershell
    # Display the state of the service.
    $DfltInstance
    # Start the service.
    $DfltInstance.Start();
    # Wait until the service has time to start.
    # Refresh the cache.  
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    # Stop the service.
    $DfltInstance.Stop();
    # Wait until the service has time to stop.
    # Refresh the cache.
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    ```  
  
##  <a name="using-service-controller-class"></a><a name="ServiceController"></a> Verwenden der ServiceController-Klasse

Sie können die ServiceController-Klasse verwenden, um den SQL Server-Dienst oder einen anderen Windows-Dienst zu steuern. Ein Verwendungsbeispiel finden Sie unter [ServiceController-Klasse](/dotnet/api/system.serviceprocess.servicecontroller).

## <a name="manage-the-sql-server-service-on-linux"></a>Verwalten des SQL Server-Diensts unter Linux

### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-database-engine"></a>Starten, Beenden oder Neustarten einer Instanz der SQL Server-Datenbank-Engine

In den folgenden Abschnitten wird beschrieben, wie der [SQL Server-Dienst unter Linux](../../linux/sql-server-linux-troubleshooting-guide.md#manage-the-sql-server-service) gestartet, beendet, neu gestartet und dessen Status überprüft wird.

Überprüfen Sie den Status des SQL Server-Diensts mit dem folgenden Befehl:

   ```bash
   sudo systemctl status mssql-server
   ```

Sie können den SQL Server-Dienst nach Bedarf mit den folgenden Befehlen beenden, starten oder neu starten:

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die SQL Server-Setupdokumentation](../install-windows/install-sql-server.md)
- [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [SQL Server-Konfigurations-Manager](../../relational-databases/sql-server-configuration-manager.md)
- [Starten Sie von SQL Server mit Minimalkonfiguration](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)
- [Von den Editionen von SQL Server 2016 unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)