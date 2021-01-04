---
title: SqlPackage.exe
description: In diesem Artikel erfahren Sie, wie Sie Datenbankentwicklungsaufgaben mithilfe von SqlPackage.exe automatisieren können. Außerdem sehen Sie Beispiele und verfügbare Parameter, Eigenschaften und SQLCMD-Variablen.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577835"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** ist ein Befehlszeilen-Hilfsprogramm, das die folgenden Aufgaben bei der Datenbankentwicklung automatisiert:  
  
- [Version](#version): Gibt die Buildnummer der SqlPackage-Anwendung zurück.  In Version 18.6 hinzugefügt.

- [Extract:](sqlpackage-extract.md) Erstellt eine Datenbankmomentaufnahme (DACPAC-Datei) von einer SQL Server- oder Azure SQL-Livedatenbank.  
  
- [Veröffentlichen](sqlpackage-publish.md): Aktualisiert ein Datenbankschema inkrementell, sodass dieses dem Schema einer DACPAC-Quelldatei entspricht Wenn die Datenbank auf dem Server nicht vorhanden ist, wird sie durch den Veröffentlichungsvorgang erstellt. Andernfalls wird eine vorhandene Datenbank aktualisiert.  
  
- [Export](sqlpackage-export.md): Exportiert eine Livedatenbank – einschließlich des Datenbankschemas und der Benutzerdaten – aus SQL Server oder Azure SQL-Datenbank in ein BACPAC-Paket (BACPAC-Datei).  
  
- [Import](sqlpackage-import.md): Importiert das Schema und die Tabellendaten aus einem BACPAC-Paket in eine neue Benutzerdatenbank einer Instanz von SQL Server oder Azure SQL-Datenbank.  
  
- [DeployReport:](sqlpackage-deploy-drift-report.md) Erstellt einen XML-Bericht der Änderungen, die durch eine Veröffentlichungsaktion vorgenommen würden  
  
- [DriftReport](sqlpackage-deploy-drift-report.md): Erstellt einen XML-Bericht der Änderungen, die seit der letzten Registrierung an einer registrierten Datenbank vorgenommen wurden.  
  
- [Script:](sqlpackage-script.md) Erstellt ein inkrementelles Transact-SQL-Updateskript, durch das das Schema eines Ziels aktualisiert wird, sodass es dem Schema einer Quelle entspricht.  
  
In der Befehlszeile von **SqlPackage.exe** können diese Aktionen zusammen mit aktionsspezifischen Parametern und Eigenschaften angegeben werden.  

**[Aktuelle Version herunterladen](sqlpackage-download.md)** Weitere Informationen über die neueste Version finden Sie in den [Versionshinweisen](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Befehlszeilensyntax

**SqlPackage.exe** initiiert die Aktionen, die mithilfe der Parameter, Eigenschaften und SQLCMD-Variablen angegeben wurden, die in der Befehlszeile angegeben sind.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Anwendungsbeispiele

**Generieren eines Vergleichs zwischen Datenbanken mithilfe von DACPAC-Dateien mit SQL-Skriptpausgabe**

Erstellen Sie zunächst eine DACPAC-Datei aus Ihren letzten Datenbankänderungen:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Erstellen Sie eine DACPAC-Datei Ihres Datenbankziels (das keine Änderungen aufweist):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Erstellen Sie ein SQL-Skript, das die Unterschiede der zwei DACPAC-Dateien generiert:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Version

Zeigt die Version von SqlPackage als Buildnummer.  Kann in interaktiven Eingabeaufforderungen und [automatisierten Pipelines](sqlpackage-pipelines.md)verwendet werden.

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Exitcodes

Befehle, die die folgenden Exitcodes zurückgeben:

- 0 = Erfolg
- Ungleich = Fehler


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Extract-Aktion von SqlPackage](sqlpackage-extract.md).
- Erfahren Sie mehr über die [Publish-Aktion von SqlPackage](sqlpackage-publish.md).
- Erfahren Sie mehr über die [Export-Aktion von SqlPackage](sqlpackage-export.md).
- Erfahren Sie mehr über die [Import-Aktion von SqlPackage](sqlpackage-import.md).
