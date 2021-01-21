---
title: Tipps und Tricks für SSMS
description: Hier erfahren Sie, wie Sie mithilfe von SQL Server Management Studio Kommentare zu Ihrem Code hinzufügen und wieder aus diesem entfernen, Text einrücken, Objekte filtern, auf Fehlerprotokolle zugreifen und Namen von SQL Server-Instanzen finden können.
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
helpviewer_keywords:
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- Find SQL Server Instance
- find instance name
- find sql server instance name
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: 9d480496cad6dafddc428c6049601521ebba88b8
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596447"
---
# <a name="tips-and-tricks-for-using-sql-server-management-studio-ssms"></a>Tipps und Tricks zur Verwendung von SQL Server Management Studio (SSMS)

Dieser Artikel enthält einige Tipps und Tricks für die Verwendung des [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS). In diesem Artikel lernen Sie Folgendes: 

> [!div class="checklist"]
> * Kommentieren bzw. Aufheben der Auskommentierung Ihres T-SQL-Texts (Transact-SQL)
> * Einziehen Ihres Texts
> * Filtern von Objekten im Objekt-Explorer
> * Zugreifen auf das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll
> * Herausfinden des Namens Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz

## <a name="prerequisites"></a>Voraussetzungen

Zum Testen der in diesem Artikel beschriebenen Schritte benötigen Sie das [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], Zugriff auf eine SQL Server-Instanz und eine AdventureWorks-Datenbank. 

* Installieren Sie [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
* Installieren Sie [[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* Laden Sie eine [AdventureWorks-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/releases) herunter. Weitere Informationen zum Wiederherstellen einer Datenbank in SSMS finden Sie unter [Restoring a database (Wiederherstellen einer Datenbank)](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md). 

## <a name="commentuncomment-your-t-sql-code"></a>Kommentieren und Aufheben der Auskommentierung in T-SQL-Code

Mit der Schaltfläche **Kommentar** in der Symbolleiste können Sie bei Teilen Ihres Texts Kommentare hinzufügen und Auskommentierungen aufheben. Auskommentierter Text wird nicht ausgeführt.

1. Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

2. Stellen Sie eine Verbindung mit SQL Server her.

3. Öffnen Sie das Fenster „Neue Abfrage“.

4. Fügen Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code in Ihr Textfenster ein.

    ```sql
    USE master
        GO

        -- Drop the database if it already exists
        IF  EXISTS (
            SELECT name 
                FROM sys.databases 
                WHERE name = N'TutorialDB'
                )

        DROP DATABASE TutorialDB
        GO

        CREATE DATABASE TutorialDB
        GO

        ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
        GO
   ```

5. Markieren Sie den Teil des Texts mit dem Befehl **Alter Database**, und klicken Sie dann auf der Symbolleiste auf die Schaltfläche **Kommentar**: 

    ![Die Schaltfläche „Kommentar“](media/ssms-tricks/comment.png)
6. Klicken Sie auf **Ausführen**, um den Teil des Texts auszuführen, bei dem Auskommentierungen aufgehoben wurden. 

7. Markieren Sie alles außer dem Befehl **Alter Database**, und klicken Sie auf die Schaltfläche **Auskommentieren**:

    ![Alles auskommentieren](media/ssms-tricks/commenteverything.png)

    > [!NOTE]
    > Die Tastenkombination zum Auskommentieren von Text lautet **STRG + K, STRG + C**.

8. Markieren Sie den Teil des Texts mit dem Befehl **Alter Database**, und klicken Sie dann auf der Symbolleiste auf die Schaltfläche **Auskommentierung aufheben**, um die Auskommentierung aufzuheben:

    ![Auskommentierung aufheben](media/ssms-tricks/uncomment.png)

    > [!NOTE]
    > Die Tastenkombination zum Aufheben der Auskommentierung von Text lautet **STRG + K, STRG + U**.

9. Klicken Sie auf **Ausführen**, um den Teil des Texts auszuführen, bei dem Auskommentierungen aufgehoben wurden.

## <a name="indent-your-text"></a>Einziehen Ihres Texts

Sie können die Schaltflächen für den Einzug auf der Symbolleiste verwenden, um den Einzug Ihres Texts zu vergrößern oder zu verkleinern.

1. Öffnen Sie das Fenster „Neue Abfrage“.

2. Fügen Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code in Ihr Textfenster ein:

    ```sql
    USE master
      GO

      --Drop the database if it already exists
      IF  EXISTS (
            SELECT name
                FROM sys.databases
                WHERE name = N'TutorialDB'
              )

      DROP DATABASE TutorialDB
      GO

      CREATE DATABASE TutorialDB
      GO

      ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
      GO
    ```

3. Markieren Sie den Teil des Texts mit dem Befehl **Alter Database**, und klicken Sie in der Symbolleiste auf die Schaltfläche **Einzug vergrößern**, um diesen Text nach vorne zu verschieben:

    ![Vergrößern des Einzugs](media/ssms-tricks/increaseindent.png)

4. Markieren Sie den Teil des Texts mit dem Befehl **Alter Database** erneut, und klicken Sie auf die Schaltfläche **Einzug verkleinern**, um diesen Text zurück zu verschieben:

    ![Verkleinern des Einzugs](media/ssms-tricks/decreaseindent.png)

## <a name="filter-objects-in-object-explorer"></a>Filtern von Objekten im Objekt-Explorer

In Datenbanken, die über viele Objekte verfügen, können Sie nach bestimmten Tabellen, Ansichten usw. filtern. In diesem Abschnitt wird beschrieben, wie Sie Tabellen filtern können. Sie können die folgenden Schritte aber auch für jeden anderen Knoten im Objekt-Explorer verwenden:

1. Stellen Sie eine Verbindung mit SQL Server her.

2. Erweitern Sie **Datenbanken** > **AdventureWorks** > **Tabellen**. Alle Tabellen in der Datenbank werden angezeigt.

3. Klicken Sie mit der rechten Maustaste auf **Tabellen**, und klicken Sie dann auf **Filter** > **Filtereinstellungen**:

    ![Filtereinstellungen](media/ssms-tricks/filtersettings.png)

4. Im Fenster **Filtereinstellungen** können Sie die einige der folgenden Filtereinstellungen ändern:
    * Nach Name filtern: 

      ![Nach Name filtern](media/ssms-tricks/filterbyname.png)

    * Nach Schema filtern: 

      ![Nach Schema filtern](media/ssms-tricks/filterbyschema.png)

5. Klicken Sie mit der rechten Maustaste auf **Tabellen**, und klicken Sie dann auf **Filter entfernen**, um den Filter zu löschen.

    ![Filter entfernen](media/ssms-tricks/removefilter.png)

## <a name="access-your-sql-server-error-log"></a>Zugreifen auf Ihr SQL Server-Fehlerprotokoll

Das Fehlerprotokoll ist eine Datei mit Details zu Ereignissen, die in Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz auftreten. Sie können das Fehlerprotokoll im SSMS durchsuchen und abfragen. Das Fehlerprotokoll ist eine LOG-Datei, die sich auf Ihrem Datenträger befindet.

### <a name="open-the-error-log-in-ssms"></a>Öffnen des Fehlerprotokolls in SSMS

1. Stellen Sie eine Verbindung mit Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz her.  

2. Erweitern Sie **Verwaltung** > **SQL Server-Protokolle**. 

3. Klicken Sie mit der rechten Maustaste auf das Fehlerprotokoll **Current** (Aktuell), und klicken Sie dann auf **SQL Server-Protokoll anzeigen**:

    ![Öffnen des Fehlerprotokolls in SSMS](media/ssms-tricks/viewerrorloginssms.png)

### <a name="query-the-error-log-in-ssms"></a>Abfragen des Fehlerprotokolls in SSMS

1. Stellen Sie eine Verbindung mit SQL Server her.

2. Öffnen Sie das Fenster „Neue Abfrage“.

3. Fügen Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code in Ihr Abfragefenster ein:

     ```sql
       sp_readerrorlog 0,1,'Server process ID'
      ```

4. Ändern Sie den Text in den einfachen Anführungszeichen in den gewünschten Suchtext.

5. Führen Sie die Abfrage aus, und überprüfen Sie die Ergebnisse:

    ![Fehlerprotokoll abfragen](media/ssms-tricks/queryerrorlog.png)

### <a name="find-the-error-log-location-if-youre-connected-to-sql-server"></a>Suchen des Speicherorts des Fehlerprotokolls, wenn Sie mit SQL Server verbunden sind

1. Stellen Sie eine Verbindung mit Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz her.

2. Öffnen Sie das Fenster „Neue Abfrage“.

3. Fügen Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code in Ihr Abfragefenster ein, und klicken Sie dann auf **Ausführen**:

     ```sql
        SELECT SERVERPROPERTY('ErrorLogFileName') AS 'Error log file location'  
      ```

4. Die Ergebnisse zeigen den Speicherort des Fehlerprotokolls in dem Dateisystem an: 

    ![Fehlerprotokoll durch Abfrage suchen](media/ssms-tricks/finderrorlogquery.png)

### <a name="find-the-error-log-location-if-you-cant-connect-to-sql-server"></a>Suchen des Speicherorts des Fehlerprotokolls, wenn keine Verbindung mit SQL Server möglich ist

Der Pfad für Ihr [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll kann abhängig von Ihren Konfigurationseinstellungen abweichen. Der Pfad für das Fehlerprotokoll ist in den Startparametern im SQL Server-Konfigurations-Manager enthalten. Führen Sie die folgenden Schritte aus, um den relevanten Startparameter zu finden, der den Speicherort Ihres [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokolls angibt. *Ihr Pfad weicht möglicherweise vom unten angegebenen Pfad ab.*

1. Öffnen Sie den SQL Server-Konfigurations-Manager.

2. Erweitern Sie **Dienste**.

3. Klicken Sie mit der rechten Maustaste auf Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, und klicken Sie dann auf **Eigenschaften**:

    ![Konfigurations-Manager-Servereigenschaften](media/ssms-tricks/serverproperties.PNG)

4. Wählen Sie die Registerkarte **Startparameter** aus.

5. Der Pfad nach „-e“ im Bereich **Vorhandene Parameter** ist der Speicherort des Fehlerprotokolls: 

    ![Fehlerprotokoll](media/ssms-tricks/errorlog.png)

    Dort sind mehrere Fehlerprotokolldateien gespeichert. Der Dateiname, der auf „*.log“ endet, gehört zur aktuellen Fehlerprotokolldatei. Dateien mit Namen, die auf Zahlen enden, sind vorherige Protokolldateien. Jedes Mal, wenn SQL Server neu gestartet wird, wird ein neues Protokoll erstellt.

6. Öffnen Sie die Datei „errorlog.log“ in Notepad.

## <a name="find-sql-server-instance-name"></a>Suchen des Namens der SQL Server-Instanz

Es gibt mehrere Möglichkeiten, den Namen Ihrer SQL Server-Instanz herauszufinden, bevor oder nachdem eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt wurde.  

### <a name="before-you-connect-to-sql-server"></a>Vor dem Herstellen einer Verbindung mit SQL Server

1. Führen Sie die Schritte zum Finden des [SQL Server-Fehlerprotokolls auf dem Datenträger](#find-the-error-log-location-if-you-cant-connect-to-sql-server) aus. Ihr Pfad weicht möglicherweise vom Pfad im unten dargestellten Bild ab.

2. Öffnen Sie die Datei „errorlog.log“ in Notepad.

3. Suchen Sie nach dem Text *Server name is* (Der Servername lautet).

    Das, was in einfachen Anführungszeichen steht, ist der Name der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, mit der Sie eine Verbindung herstellen:

    ![Servername im Fehlerprotokoll suchen](media/ssms-tricks/servernameinlog.png)

    Das Format des Namens ist Hostname\Instanzname. Wenn nur der Hostname angezeigt wird, haben Sie die Standardinstanz installiert, und Ihr Instanzname ist „MSSQLSERVER“. Wenn Sie eine Verbindung mit einer Standardinstanz erstellen, müssen Sie nur den Hostnamen eingeben, um eine Verbindung mit SQL Server herzustellen.  

### <a name="when-youre-connected-to-sql-server"></a>Wenn Sie eine Verbindung mit SQL Server hergestellt haben

Wenn Sie eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt haben, finden Sie den Servernamen an drei Stellen: 

1. Der Name des Servers wird im Objekt-Explorer aufgeführt:

    ![SQL Server-Instanzname im Objekt-Explorer](media/ssms-tricks/nameinobjectexplorer.png)
2. Der Name des Servers wird im Abfragefenster aufgeführt:

    ![Der SQL Server-Instanzname im Abfragefenster](media/ssms-tricks/nameinquerywindow.png)
3. Der Name des Servers wird unter **Eigenschaften** aufgeführt.
    * Klicken Sie im Menü **Ansicht** auf **Eigenschaftenfenster**:

      ![Der SQL Server-Instanzname im Eigenschaftenfenster](media/ssms-tricks/nameinproperties.png)

### <a name="if-youre-connected-to-an-alias-or-availability-group-listener"></a>Wenn Sie mit einem Alias oder einem Verfügbarkeitsgruppenlistener verbunden sind

Wenn Sie mit einem Alias oder einem Verfügbarkeitsgruppenlistener verbunden sind, werden Ihnen diese Information im Objekt-Explorer und in den Eigenschaften angezeigt. In diesem Fall ist der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Name möglicherweise nicht sofort erkennbar und muss abgefragt werden:

1. Stellen Sie eine Verbindung mit SQL Server her.

2. Öffnen Sie das Fenster „Neue Abfrage“.

3. Fügen Sie den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Code in das Fenster ein:

      ```sql
       select @@Servername
     ```

4. Zeigen Sie die Ergebnisse der Abfrage an, um den Namen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zu ermitteln, mit der eine Verbindung besteht: 

    ![Abfragen des Namens vom SQL-Server](media/ssms-tricks/queryservername.png)

## <a name="next-steps"></a>Nächste Schritte

Am besten machen Sie sich mit SSMS vertraut, indem Sie einige praktische Aufgaben durchführen. Dieses *Tutorial* und Artikel zu *Vorgehensweisen* unterstützen Sie bei der Verwendung der verschiedenen Features, die in SSMS verfügbar sind.  In diesen Artikeln erfahren Sie, wie Sie die Komponenten von SSMS verwalten und wie Sie die Funktionen finden, die Sie regelmäßig verwenden.

* [Herstellen einer Verbindung mit einer Instanz und Abfragen einer Instanz](../quickstarts/ssms-connect-query-sql-server.md)
* [Skripterstellung](scripting-ssms.md)
* [Verwenden von Vorlagen in SSMS](../template/templates-ssms.md)
* [SSMS-Konfiguration](ssms-configuration.md)