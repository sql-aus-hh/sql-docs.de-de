---
title: 'Schnellstart: Herstellen einer Verbindung mit und Abfragen von SQL Server'
description: In diesem Schnellstart verwenden Sie Azure Data Studio, um eine Verbindung mit SQL Server herzustellen, und anschließend verwenden Sie T-SQL-Anweisungen (Transact-SQL) zum Erstellen einer Datenbank.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 08/02/2019
ms.openlocfilehash: 6a637791eac7858e905f766126ca93d9277b65c1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637626"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-sql-server"></a>Schnellstart: Verwenden von Azure Data Studio zum Verbinden mit und Abfragen von SQL Server

In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure Data Studio zum Herstellen einer Verbindung mit SQL Server verwenden und dann mit T-SQL-Anweisungen (Transact-SQL) die in Tutorials für Azure Data Studio verwendete *TutorialDB*-Komponente erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Um diesen Schnellstart abzuschließen, benötigen Sie Azure Data Studio und Zugriff auf SQL Server.

- [Installieren Sie Azure Data Studio](./download-azure-data-studio.md).

Wenn Sie nicht auf eine SQL Server-Instanz zugreifen können, wählen Sie Ihre Plattform über einen der folgenden Links aus (hierzu benötigen Sie Ihren SQL-Anmeldenamen und das zugehörige Kennwort):

- [Windows: Herunterladen der SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)
- [macOS: Herunterladen von SQL Server 2017 für Docker](../linux/quickstart-install-connect-docker.md)
- [Linux: Herunterladen der SQL Server 2017 Developer Edition](../linux/sql-server-linux-overview.md#install): Sie müssen nur die Schritte zum *Erstellen und Abfragen von Daten* ausführen.

## <a name="connect-to-a-sql-server"></a>Herstellen einer Verbindung mit SQL Server

1. Starten Sie **Azure Data Studio**.

2. Wenn Sie Azure Data Studio zum ersten Mal ausführen, sollte die **Homepage** geöffnet werden. Wenn die **Homepage** nicht angezeigt wird, wählen Sie **Hilfe** > **Willkommen** aus. Wählen Sie **Neue Verbindung** aus, um den Bereich **Verbindung** zu öffnen:

   ![Symbol „Neue Verbindung“](media/quickstart-sql-server/new-connection-icon.png)

3. In diesem Artikel wird die *SQL-Anmeldung* verwendet, aber auch die *Windows-Authentifizierung* unterstützt. Füllen Sie die Felder wie folgt aus:

   - **Servername:** Geben Sie hier den Servernamen ein. Beispiel: localhost.
   - **Authentifizierungstyp:** SQL-Anmeldung
   - **Benutzername:** Benutzername für die SQL Server-Instanz
   - **Kennwort:** Kennwort für die SQL Server-Instanz
   - **Datenbankname:** \<Default\>
   - **Servergruppe:** \<Default\>

   ![Bildschirm „Neue Verbindung“](media/quickstart-sql-server/new-connection-screen.png)

## <a name="create-a-database"></a>Erstellen einer Datenbank

Mit den folgenden Schritten wird eine Datenbank mit dem Namen **TutorialDB** erstellt:

1. Klicken Sie mit der rechten Maustaste auf Ihren Server, **localhost**, und wählen Sie **Neue Abfrage** aus.

2. Fügen Sie den folgenden Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**.

    ```sql
    USE master
    GO
    IF NOT EXISTS (
     SELECT name
     FROM sys.databases
     WHERE name = N'TutorialDB'
    )
     CREATE DATABASE [TutorialDB];
    GO
    IF SERVERPROPERTY('ProductVersion') > '12'
     ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON;
    GO
    ```

   Nachdem die Abfrage abgeschlossen ist, wird die neue Datenbank **TutorialDB** in der Datenbankliste angezeigt. Wenn die Datenbank nicht angezeigt wird, klicken Sie zuerst mit der rechten Maustaste auf den Knoten **Datenbanken**, und wählen Sie **Aktualisieren** aus.

   ![Erstellen einer Datenbank](media/quickstart-sql-server/create-database.png)

## <a name="create-a-table"></a>Erstellen einer Tabelle

Der Abfrage-Editor ist immer noch mit der Datenbank *master* verbunden, aber wir möchten eine Tabelle in der Datenbank *TutorialDB* erstellen.

1. Ändern Sie den Verbindungskontext in **TutorialDB**:

   ![Kontext ändern](media/quickstart-sql-server/change-context.png)

2. Fügen Sie den folgenden Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**:

   > [!NOTE]
   > Sie können diesen Codeausschnitt im Editor anfügen oder die vorherige Abfrage überschreiben. Beachten Sie, dass beim Klicken auf **Ausführen** nur die ausgewählte Abfrage ausgeführt wird. Wenn nichts ausgewählt ist, werden beim Klicken auf **Ausführen** alle Abfragen im Editor ausgeführt.

    ```sql
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
     DROP TABLE dbo.Customers;
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
     CustomerId int NOT NULL PRIMARY KEY, -- primary key column
     Name nvarchar(50) NOT NULL,
     Location nvarchar(50) NOT NULL,
     Email nvarchar(50) NOT NULL
    );
    GO
    ```

Nachdem die Abfrage abgeschlossen ist, wird die neue Tabelle **Customers** in der Tabellenliste angezeigt. Möglicherweise müssen Sie mit der rechten Maustaste auf den Knoten **TutorialDB > Tables** klicken und **Aktualisieren** auswählen.

## <a name="insert-rows"></a>Einfügen von Zeilen

- Fügen Sie den folgenden Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**:

    ```sql
    -- Insert rows into table 'Customers'
    INSERT INTO dbo.Customers
     ([CustomerId], [Name], [Location], [Email])
    VALUES
     ( 1, N'Orlando', N'Australia', N''),
     ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
     ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
     ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
    GO
    ```

## <a name="view-the-data-returned-by-a-query"></a>Anzeigen der von einer Abfrage zurückgegebenen Daten

 - Fügen Sie den folgenden Codeausschnitt in das Abfragefenster ein, und klicken Sie auf **Ausführen**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

   ![Select-Ergebnis](media/quickstart-sql-server/select-results.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfolgreich eine Verbindung mit SQL Server hergestellt und eine Abfrage ausgeführt haben, probieren Sie das [Tutorial zum Code-Editor](tutorial-sql-editor.md) aus.