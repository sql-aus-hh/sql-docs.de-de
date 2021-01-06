---
title: PowerShell-Cmdlet für die Migrationsauswertung | Microsoft-Dokumentation
description: Lernen Sie das Cmdlet „Save-SqlMigrationReport“ kennen, das die Eignung von Objekten einer SQL Server-Datenbank für die Migration zu In-Memory OLTP bewertet.
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 117250d3-9982-47fe-94fd-6f29f6159940
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78b4d84a98d7ea2c753a673a222a1a19e0b7e5e2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641276"
---
# <a name="powershell-cmdlet-for-migration-evaluation"></a>PowerShell-Cmdlet für die Migrationsauswertung

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Das `Save-SqlMigrationReport` ist ein Tool zur Auswertung der Eignung mehrerer Objekte einer SQL Server-Datenbank für die Migration.

Derzeit beschränkt sich dieses Cmdlet noch auf die Auswertung der Migrationseignung von In-Memory-OLTP. Das Cmdlet kann in einer erweiterten Windows PowerShell-Umgebung und in Sqlps ausgeführt werden.

Als Alternative zum direkten Ausführen dieses PowerShell-Cmdlets können Sie das Cmdlet implizit mithilfe von SQL Server Management Studio (SSMS) ausführen. Klicken Sie im **Objekt-Explorer** von SSMS mit der rechten Maustaste auf eine Tabelle, und klicken Sie dann auf **Ratgeber für die Speicheroptimierung**.

## <a name="syntax"></a>Syntax

```
Save-SqlMigrationReport
    -FolderPath <output_path>
    [ -MigrationType <migration_scenario_type> ]
    [
        [ -Server <server_name> -Database <database_name>
            [ -Schema <schema_name> ] [ -Object <object_name> ]
        ]
       |
        [ -InputObject <smo_object> ]
    ]
;
```

## <a name="parameters"></a>Parameter

Die Parameter werden in der folgenden Tabelle erläutert.

Es gibt Syntaxaspekte, auf die hingewiesen werden sollte. Wenn Sie den Parameter `-InputObject` angeben, können Sie keinen der folgenden Parameter angeben:

- `-Server`
- `-Database`
- `-Schema`
- `-Object`

Wenn Sie `-InputObject`_nicht_ angeben, dann müssen Sie umgekehrt `-Server` und `-Database` angeben. Wenn Sie `-Server` angeben, haben Sie die Möglichkeit, den Bereich einzuschränken, indem Sie entweder `-Schema` oder `-Object` oder beides angeben.

| Parametername | BESCHREIBUNG |
| :------------- | :---------- |
| Datenbank | Der Name der SQL Server-Zieldatenbank. Obligatorisch, wenn `-Server` obligatorisch ist.<br/><br/> Optional in SQLPS. |
| FolderPath | Der Ordner, in dem das Cmdlet die generierten Berichte speichern soll.<br/><br/> Erforderlich. |
| InputObject | Das SMO-Zielobjekt des Cmdlets.<br/><br/> In der Windows PowerShell-Umgebung obligatorisch, wenn `-Server` nicht angegeben wird.<br/><br/> Optional in SQLPS. |
| MigrationType | Der Typ des Migrationsszenarios, den das Cmdlet überprüft. Derzeit ist der einzige Wert der Standardwert **„OLTP“** .<br/><br/> Optional. |
| Object | Der Name des Objekts, zu dem ein Bericht erstellt werden soll. Kann eine Tabelle oder eine gespeicherte Prozedur sein. |
| Kennwort | Ist erforderlich, wenn `-Username` erforderlich ist. |
| Schema | Der Name des Schemas, das das Objekt besitzt, zu dem ein Bericht erstellt werden soll.<br/><br/> Optional. |
| Server | Der Name der SQL Server-Zielinstanz. In der Windows PowerShell-Umgebung obligatorisch, wenn der Parameter `-InputObject` nicht angegeben wird.<br/><br/> Optional in SQLPS. |
| Username | Erforderlich, wenn eine Verbindung über SQL Server-Authentifizierung hergestellt wird (im Gegensatz zu Windows-Authentifizierung). Kann andernfalls ausgelassen werden. |
| &nbsp; | &nbsp; |

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie dieses Cmdlet ausführen können, müssen Sie zunächst das Modul mit dem Namen **SqlServer** installieren:

- `Install-Module -Name SqlServer`

> [!NOTE]
> Das alte `SQLPS`-Modul wird nicht mehr unterstützt. Verwenden Sie das neuere `SqlServer`-Modul.

Weitere Informationen finden Sie unter [Installieren des SQL Server PowerShell-Moduls](../../powershell/download-sql-server-ps-module.md).

## <a name="example-cmdlet-line"></a>Cmdlet-Beispielzeile

Als nächstes folgt die eigentliche Cmdlet-Zeile, die ausgeführt wurde, um den Bericht zu generieren, der später in diesem Artikel gezeigt wird.

```powershell
Save-SqlMigrationReport `
  -FolderPath 'C:\Test\PowerShell-ps1\Save-SqlMigrationReport\' `
  -Server 'MyUserName123456.database.windows.net' `
  -Database 'MyDatabaseName_31' `
  -Schema 'dbo' `
  -Object 'Table2' `
  -Username 'MyUserName' `
  -Password 'MyPassword' `
  -MigrationType 'OLTP' `
;
```

## <a name="example-output-report"></a>Beispielausgabe des Berichts

Unter dem für den Parameter `-FolderPath` angegebenen Ordner werden die folgenden beiden Ordnerpfade durch Ausführen dieses Cmdlets erstellt. Beide Pfade beginnen mit dem Wert _server\_name_:

- MyDatabaseName_31\Tables\
- MyDatabaseName_31\Stored Procedures\

Jede Objektberichtsdatei wird im entsprechenden Ordner gespeichert.

Die Namen der Berichtsdateien tragen die Dateierweiterung **.html**. Beispielsweise ist dies ein tatsächlich generierter Dateiname: **MigrationAdvisorChecklistReport_Table2_20190728.html**.

Die HTML-Datei besteht größtenteils aus einer zweispaltige Tabelle mit den folgenden Kopfzeilen:

- BESCHREIBUNG
- Überprüfungsergebnis

Nun folgt ein tatsächliches Beispiel für den HTML-Bericht für eine Tabelle.

```html
<?xml version="1.0" encoding="utf-8"?>
<html>
  <head>
    <title>Memory optimization checklist for [MyDatabaseName_31].[Table2]</title>
  </head>
  <body>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 14pt;">
      <b>Memory optimization checklist for [MyDatabaseName_31].[Table2]</b>
    </p>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 10pt;">
      <b>Report Date/Time:</b>7/28/2019 2:25 PM<br /></p>
    <table border="1" cellpadding="5" cellspacing="0" STYLE="font-family: Verdana, Arial, sans-serif; font-size: 9pt;">
      <tr style="background-color:Silver">
        <th colspan="2" align="center">Description</th>
        <th align="center">Validation Result</th>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported data types are defined on this table. </td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No sparse columns are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No identity columns with unsupported seed and increment are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No foreign key relationships are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported constraints are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No unsupported indexes are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported triggers are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">Post migration row size does not exceed the row size limit of memory-optimized tables.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">Table is not partitioned or replicated.</td>
        <td>Succeeded</td>
      </tr>
    </table>
  </body>
</html>
```

Die Tabelle sieht ungefähr wie folgt aus.

| BESCHREIBUNG | Überprüfungsergebnis |
| :---------- | :---------------- |
| Für diese Tabelle sind keine nicht unterstützten Datentypen definiert. | Erfolgreich |
| Für diese Tabelle sind keine Sparsespalten definiert. | Erfolgreich |
| Für diese Tabelle sind keine Identitätsspalten mit nicht unterstütztem Seed und Inkrement definiert. | Erfolgreich |
| Für diese Tabelle sind keine Fremdschlüsselbeziehungen definiert. | Erfolgreich |
| Für diese Tabelle sind keine nicht unterstützten Einschränkungen definiert. | Erfolgreich |
| Für diese Tabelle sind keine nicht unterstützten Indizes definiert. | Erfolgreich |
| Für diese Tabelle sind keine nicht unterstützten Trigger definiert. | Erfolgreich |
| Die Zeilengröße nach der Migration überschreitet nicht die maximale Zeilengröße speicheroptimierter Tabellen. | Erfolgreich |
| Die Tabelle ist nicht partitioniert oder repliziert. | Erfolgreich |
| &nbsp; | &nbsp; |

## <a name="related-links"></a>Verwandte Links

- Referenzdokumentation: [Save-SqlMigrationReport](/powershell/module/sqlserver/save-sqlmigrationreport)
