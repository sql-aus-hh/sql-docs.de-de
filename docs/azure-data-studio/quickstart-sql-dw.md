---
title: Verbinden und Abfragen mit Azure Synapse Analytics
description: Dieser Schnellstart veranschaulicht das Herstellen einer Verbindung mit einem dedizierten SQL-Pool in Azure Synapse Analytics mithilfe von Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: maghan, jrasnick
ms.custom: seodec18; seo-lt-2019
ms.date: 10/15/2020
ms.openlocfilehash: 8215e84c4c61935b3f5831ca771e2868e27be428
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091723"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Schnellstart: Verwenden von Azure Data Studio zum Verbinden mit und Abfragen von Daten mithilfe eines dedizierten SQL-Pools in Azure Synapse Analytics

Dieser Schnellstart veranschaulicht das Herstellen einer Verbindung mit einem dedizierten SQL-Pool in Azure Synapse Analytics mithilfe von Azure Data Studio.

## <a name="prerequisites"></a>Voraussetzungen
Um diesen Schnellstart ausführen zu können, benötigen Sie Azure Data Studio und einen dedizierten SQL-Pool in Azure Synapse Analytics.

- [Installieren Sie Azure Data Studio](./download-azure-data-studio.md).

Wenn Sie noch nicht über einen dedizierten SQL-Pool verfügen, lesen Sie [Erstellen eines dedizierten SQL-Pools](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

Merken Sie sich den Servernamen und die Anmeldeinformationen!


## <a name="connect-to-your-dedicated-sql-pool"></a>Herstellen einer Verbindung mit Ihrem dedizierten SQL-Pool

Verwenden Sie Azure Data Studio, um eine Verbindung mit Ihrem Azure Synapse Analytics-Server herzustellen.

1. Wenn Sie Azure Data Studio zum ersten Mal ausführen, sollte die **Verbindungsseite** geöffnet werden. Wird die Seite **Verbindung** nicht angezeigt, wählen Sie **Verbindung hinzufügen** oder das Symbol **Neue Verbindung** in der Randleiste **SERVER** aus:
   
   ![Screenshot der Seite „Verbindungen“, auf dem das Symbol „Neue Verbindung“ hervorgehoben wird](media/quickstart-sql-dw/new-connection-icon.png)

2. In diesem Artikel wird die *SQL-Anmeldung* verwendet, aber auch die *Windows-Authentifizierung* wird unterstützt. Füllen Sie die Felder wie folgt aus, wozu Sie den Servernamen, den Benutzernamen und das Kennwort für *Ihre* Azure SQL Server-Instanz verwenden:

   |   Einstellung    | Vorgeschlagener Wert | BESCHREIBUNG |
   |--------------|-----------------|-------------| 
   | **Servername** | Der vollqualifizierte Servername | Beispielsweise sollte der Name ähnlich wie hier aussehen: **sqlpoolservername.database.windows.net**. |
   | **Authentifizierung** | SQL-Anmeldung| In diesem Tutorial wird SQL-Authentifizierung verwendet. |
   | **Benutzername** | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | **Kennwort (SQL-Anmeldung)** | Das Kennwort für das Serveradministratorkonto | Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |
   | **Kennwort speichern** | Ja oder Nein | Wählen Sie Ja aus, wenn Sie nicht bei jedem Neustart Ihr Kennwort eingeben möchten. |
   | **Datenbankname** | *Leer lassen* | Dies ist der Name der Datenbank, mit der eine Verbindung hergestellt werden soll. |
   | **Servergruppe** | <Default> auswählen | Wenn Sie eine Servergruppe erstellt haben, können Sie dieses Feld auf eine bestimmte Servergruppe festlegen. | 

3. Wenn der Server nicht über eine Firewallregel verfügt, die Azure Data Studio ermöglicht, eine Verbindung herzustellen, wird das Formular **Neue Firewallregel erstellen** geöffnet. Füllen Sie das Formular aus, um eine neue Firewallregel zu erstellen. Weitere Informationen finden Sie unter [Firewallregeln](/azure/sql-database/sql-database-firewall-configure).

4. Nachdem die Verbindung erfolgreich hergestellt wurde, wird Ihr Server in der Randleiste *SERVER* geöffnet.

## <a name="create-a-database-in-your-dedicated-sql-pool"></a>Erstellen einer Datenbank in Ihrem dedizierten SQL-Pool

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf Ihren Server, und wählen Sie **Neue Abfrage** aus.

2. Fügen Sie den folgenden Codeausschnitt in den Abfrage-Editor ein, und wählen Sie **Ausführen** aus:

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```

## <a name="create-a-table"></a>Erstellen einer Tabelle

Der Abfrage-Editor ist immer noch mit der Datenbank *master* verbunden, aber wir möchten eine Tabelle in der Datenbank *TutorialDB* erstellen. 

1. Ändern Sie den Verbindungskontext in **TutorialDB**:

2. Fügen Sie den folgenden Codeausschnitt in den Abfrage-Editor ein, und wählen Sie **Ausführen** aus:

   > [!NOTE]
   > Sie können diesen Codeausschnitt im Editor an die vorherige Abfrage anfügen oder überschreiben. Beachten Sie, dass beim Auswählen von **Ausführen** nur die ausgewählte Abfrage ausgeführt wird. Wenn nichts ausgewählt ist, werden durch Auswählen von **Ausführen** alle Abfragen im Editor ausgeführt.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-table.png" alt-text="Erstellen einer Tabelle in der TutorialDB-Datenbank":::


## <a name="insert-rows"></a>Einfügen von Zeilen

1. Fügen Sie den folgenden Codeausschnitt in den Abfrage-Editor ein, und wählen Sie **Ausführen** aus:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-rows.png" alt-text="Erstellen von Zeilen in der Tabelle":::

## <a name="view-the-result"></a>Anzeigen des Ergebnisses

1. Fügen Sie den folgenden Codeausschnitt in den Abfrage-Editor ein, und wählen Sie **Ausführen** aus:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

2. Die Abfrageergebnisse werden angezeigt:

    :::image type="content" source="media/quickstart-sql-dw/view-results.png" alt-text="Ansicht der Ergebnisse":::


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mehr mit den in diesem Artikel erstellen Beispieldatenbanken arbeiten möchten, [löschen Sie die Ressourcengruppe](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Synapse SQL über Azure Data Studio (Vorschau)](/azure/synapse-analytics/sql/get-started-azure-data-studio).

Nachdem Sie erfolgreich eine Verbindung mit Azure Synapse Analytics hergestellt und eine Abfrage ausgeführt haben, wechseln Sie zum [Tutorial zum Code-Editor](tutorial-sql-editor.md).