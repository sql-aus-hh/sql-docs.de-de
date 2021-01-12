---
description: SET QUOTED_IDENTIFIER (Transact-SQL)
title: SET QUOTED_IDENTIFIER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- QUOTED_IDENTIFIER_TSQL
- SET_QUOTED_IDENTIFIER_TSQL
- SET QUOTED_IDENTIFIER
- QUOTED_IDENTIFIER
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- identifiers [SQL Server], delimited
- QUOTED_IDENTIFIER option
- quoted identifiers
- ISO delimited identifiers rules
- SET QUOTED_IDENTIFIER statement
ms.assetid: 10f66b71-9241-4a3a-9292-455ae7252565
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 232c287564593eee0deab4ae37dc13c36d2d689b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100752"
---
# <a name="set-quoted_identifier-transact-sql"></a>SET QUOTED_IDENTIFIER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Bewirkt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die ISO-Regeln für Anführungszeichen bei Bezeichnern und Literalzeichenfolgen befolgt. Bei Bezeichnern, die mit Anführungszeichen begrenzt werden, kann es sich um [!INCLUDE[tsql](../../includes/tsql-md.md)]-Schlüsselwörter oder um Bezeichner mit Zeichen handeln, die nach den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntaxregeln für Bezeichner sonst nicht zulässig sind.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and serverless SQL pool in Azure Synapse Analytics

SET QUOTED_IDENTIFIER { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET QUOTED_IDENTIFIER ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
Wenn `SET QUOTED_IDENTIFIER` auf ON festgelegt ist, können Bezeichner durch doppelte Anführungszeichen (" ") begrenzt werden. Literale müssen in diesem Fall durch einfache Anführungszeichen (' ') begrenzt werden. Alle Zeichenfolgen, die durch doppelte Anführungszeichen begrenzt werden, werden als Objektbezeichner interpretiert. Daher müssen Bezeichner in Anführungszeichen nicht den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Sie können reservierte Schlüsselwörter darstellen und Zeichen einschließen, die in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Bezeichnern sonst nicht zulässig sind. Doppelte Anführungszeichen dürfen nicht zum Begrenzen von Ausdrücken mit Literalzeichenfolgen verwendet werden, da Literalzeichenfolgen in einfache Anführungszeichen eingeschlossen werden müssen. Ist ein einfaches Anführungszeichen (') Teil des Zeichenfolgenliterals, kann es durch zwei einfache Anführungszeichen (") dargestellt werden. `SET QUOTED_IDENTIFIER` muss auf ON festgelegt sein, wenn in der Datenbank reservierte Schlüsselwörter für Objektnamen verwendet werden.

Wenn `SET QUOTED_IDENTIFIER` auf OFF festgelegt ist, können Bezeichner nicht in Anführungszeichen eingeschlossen werden und müssen allen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md). Literale können in einfache oder doppelte Anführungszeichen eingeschlossen werden. Eine in doppelte Anführungszeichen eingeschlossene Literalzeichenfolge kann eingebundene einfache Anführungszeichen, wie z. B. Apostrophe, enthalten.

> [!NOTE]
> QUOTED_IDENTIFIER wirkt sich nicht auf Begrenzungsbezeichner aus, die in eckige Klammern ([ ]) eingeschlossen sind.

`SET QUOTED_IDENTIFIER` muss auf ON festgelegt sein, wenn Indizes für berechnete Spalten oder indizierte Sichten erstellt oder geändert werden. Ist `SET QUOTED_IDENTIFIER` auf OFF festgelegt, schlagen CREATE-, UPDATE-, INSERT- und DELETE-Anweisungen in Tabellen mit Indizes für berechneten Spalte oder in Tabellen mit indizierten Sichten fehl. Weitere Informationen zu den erforderlichen Einstellungen der SET-Option mit indizierten Sichten und Indizes für berechnete Spalten finden Sie unter [Überlegungen beim Verwenden von SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).

`SET QUOTED_IDENTIFIER` muss auf ON festgelegt sein, wenn Sie einen gefilterten Index erstellen.

`SET QUOTED_IDENTIFIER` muss auf ON festgelegt sein, wenn Sie XML-Datentypmethoden aufrufen.

Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-ODBC-Treiber und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-OLE DB-Anbieter für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] legen QUOTED_IDENTIFIER beim Herstellen einer Verbindung automatisch auf ON fest. Diese Einstellung kann in ODBC-Datenquellen, in ODBC-Verbindungsattributen oder in OLE DB-Verbindungseigenschaften konfiguriert werden. SET QUOTED_IDENTIFIER ist für Verbindungen von DB-Library-Anwendungen standardmäßig auf OFF festgelegt.

Beim Erstellen einer Tabelle wird die Option QUOTED IDENTIFIER immer als ON in den Metadaten der Tabelle gespeichert, selbst wenn die Option beim Erstellen der Tabelle auf OFF festgelegt war.

Wenn eine gespeicherte Prozedur erstellt wird, werden die SET QUOTED_IDENTIFIER- und SET ANSI_NULLS-Einstellungen aufgezeichnet und in späteren Aufrufen der gespeicherten Prozedur wieder verwendet.

Wird SET QUOTED_IDENTIFIER innerhalb einer gespeicherten Prozedur ausgeführt, wird die Einstellung nicht geändert.

Wenn `SET ANSI_DEFAULTS` auf ON festgelegt ist, wird auch QUOTED_IDENTIFIER auf ON festgelegt.

`SET QUOTED_IDENTIFIER` entspricht auch der QUOTED_IDENTIFIER-Einstellung von [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

`SET QUOTED_IDENTIFIER` wird während der Analysezeit von [!INCLUDE[tsql](../../includes/tsql-md.md)] wirksam und hat nur Auswirkungen auf die Analyse, nicht auf die Optimierung oder Ausführung der Abfrage.

Für einen allgemeinen Ad-hoc-Batch beginnt die Analyse unter Verwendung der aktuellen Einstellungen der Sitzung für QUOTED_IDENTIFIER. Bei der Analyse des Batches verändert jedes Auftreten von `SET QUOTED_IDENTIFIER` ab diesem Zeitpunkt das Analyseverhalten, und diese Einstellung wird für die Sitzung gespeichert. Nachdem der Batch analysiert und ausgeführt wurde, wird die QUOTED_IDENTIFER-Einstellung der Sitzung basierend auf dem letzten Auftreten von `SET QUOTED_IDENTIFIER` im Batch festgelegt.

Statische [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen in einer gespeicherten Prozedur werden mithilfe der QUOTED_IDENTIFIER-Einstellung des Batches analysiert, der die gespeicherte Prozedur erstellt oder geändert hat. `SET QUOTED_IDENTIFIER` hat beim Auftreten im Textkörper einer gespeicherten Prozedur als statische [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung keinerlei Auswirkungen.

Für einen geschachtelten Batch, der `sp_executesql` oder `exec()` verwendet, beginnt die Analyse bei Verwendung der QUOTED_IDENTIFIER-Einstellung der Sitzung. Wenn sich der geschachtelte Batch innerhalb einer gespeicherten Prozedur befindet, startet die Analyse bei Verwendung der QUOTED_IDENTIFIER-Einstellung der gespeicherten Prozedur. Bei der Analyse des geschachtelten Batches verändert jedes Auftreten von `SET QUOTED_IDENTIFIER` ab diesem Zeitpunkt das Analyseverhalten, aber die QUOTED_IDENTIFIER-Einstellung der Sitzung wird nicht aktualisiert.

Um die aktuelle Einstellung anzuzeigen, führen Sie die folgende Abfrage aus:

```sql
DECLARE @QUOTED_IDENTIFIER VARCHAR(3) = 'OFF';
IF ( (256 & @@OPTIONS) = 256 ) 
SET @QUOTED_IDENTIFIER = 'ON';

SELECT @QUOTED_IDENTIFIER AS QUOTED_IDENTIFIER;
```

## <a name="permissions"></a>Berechtigungen
Erfordert die Mitgliedschaft in der Rolle `PUBLIC`.

## <a name="examples"></a>Beispiele

### <a name="a-using-the-quoted-identifier-setting-and-reserved-word-object-names"></a>A. Verwenden der quoted identifier-Einstellung und Verwenden von Objektnamen mit reservierten Wörtern

Im folgenden Beispiel wird dargestellt, dass die `SET QUOTED_IDENTIFIER`-Einstellung auf `ON` festgelegt sein muss und dass Schlüsselwörter in Tabellennamen in doppelte Anführungszeichen eingeschlossen werden müssen, damit Objekte mit Namen von reservierten Schlüsselwörtern erstellt und verwendet werden können.

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- Create statement fails.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Create statement succeeds.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SELECT "identity","order"
FROM "select"
ORDER BY "order";
GO

DROP TABLE "SELECT";
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

### <a name="b-using-the-quoted-identifier-setting-with-single-and-double-quotation-marks"></a>B. Verwenden der quoted identifier-Einstellung mit einfachen und doppelten Anführungszeichen

 Im folgenden Beispiel wird die Verwendung von einfachen und doppelten Anführungszeichen in Zeichenfolgenausdrücken dargestellt, für die `SET QUOTED_IDENTIFIER` einmal auf `ON` und einmal auf `OFF` festgelegt ist.

```sql
SET QUOTED_IDENTIFIER OFF;
GO

USE AdventureWorks2012;
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
    WHERE TABLE_NAME = 'Test')
    DROP TABLE dbo.Test;
GO
USE AdventureWorks2012;
CREATE TABLE dbo.Test (ID INT, String VARCHAR(30)) ;
GO

-- Literal strings can be in single or double quotation marks.
INSERT INTO dbo.Test VALUES (1, "'Text in single quotes'");
INSERT INTO dbo.Test VALUES (2, '''Text in single quotes''');
INSERT INTO dbo.Test VALUES (3, 'Text with 2 '''' single quotes');
INSERT INTO dbo.Test VALUES (4, '"Text in double quotes"');
INSERT INTO dbo.Test VALUES (5, """Text in double quotes""");
INSERT INTO dbo.Test VALUES (6, "Text with 2 """" double quotes");
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Strings inside double quotation marks are now treated
-- as object names, so they cannot be used for literals.
INSERT INTO dbo."Test" VALUES (7, 'Text with a single '' quote');
GO

-- Object identifiers do not have to be in double quotation marks
-- if they are not reserved keywords.
SELECT ID, String
FROM dbo.Test;
GO

DROP TABLE dbo.Test;
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
 ID          String
 ----------- ------------------------------
 1           'Text in single quotes'
 2           'Text in single quotes'
 3           Text with 2 '' single quotes
 4           "Text in double quotes"
 5           "Text in double quotes"
 6           Text with 2 "" double quotes
 7           Text with a single ' quote
 ```

## <a name="see-also"></a>Weitere Informationen
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)    
[CREATE DEFAULT](../../t-sql/statements/create-default-transact-sql.md)    
[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)    
[CREATE RULE](../../t-sql/statements/create-rule-transact-sql.md)    
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)    
[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)    
[Datentypen](../../t-sql/data-types/data-types-transact-sql.md)    
[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)    
[SELECT](../../t-sql/queries/select-transact-sql.md)    
[SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md)    
[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)    
[sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)    
[Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md)
