---
description: SET ANSI_DEFAULTS (Transact-SQL)
title: SET ANSI_DEFAULTS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET ANSI_DEFAULTS
- ANSI_DEFAULTS
- SET_ANSI_DEFAULTS_TSQL
- ANSI_DEFAULTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ANSI_DEFAULTS option
- SET ANSI_DEFAULTS statement
ms.assetid: bd721d97-6e23-488b-8c8c-c0453d5b3b86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0e9bbbc3a8f5ae996eb55e80896c532c2507dab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095670"
---
# <a name="set-ansi_defaults-transact-sql"></a>SET ANSI_DEFAULTS (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Steuert eine Gruppe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Einstellungen, die zusammen einen bestimmten Teil des Standardverhaltens von ISO angeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntax

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Syntax für [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] und [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]
```syntaxsql
SET ANSI_DEFAULTS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Syntax für [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] und [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_DEFAULTS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
ANSI_DEFAULTS ist eine serverseitige Einstellung, mit der sich das Verhalten für alle Clientverbindungen aktivieren lässt. Der Client fordert die Einstellung in der Regel bei der Initialisierung der Verbindung oder Sitzung an. Benutzer sollten die Servereinstellung nicht ändern.   
Zum Ändern des Clientverhaltens sollten Benutzer die clientspezifischen Methoden wie `SQL_COPT_SS_PRESERVE_CURSORS` verwenden. Weitere Informationen finden Sie unter [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).
  
Wird die Option aktiviert (ON), werden die folgenden ISO-Einstellungen aktiviert:  
  
:::row:::
    :::column:::
        SET ANSI_NULLS
    :::column-end:::
    :::column:::
        SET CURSOR_CLOSE_ON_COMMIT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_NULL_DFLT_ON
    :::column-end:::
    :::column:::
        SET IMPLICIT_TRANSACTIONS
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_PADDING
    :::column-end:::
    :::column:::
        SET QUOTED_IDENTIFIER
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_WARNINGS
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

Diese SET-Optionen gemäß ISO-Standard definieren zusammen die Abfrageverarbeitungsumgebung für die Dauer der Arbeitssitzung des Benutzers oder für den Zeitraum, in dem ein Trigger oder eine gespeicherte Prozedur ausgeführt wird. Die aufgeführten SET-Optionen schließen jedoch nicht alle Optionen ein, die erforderlich wären, um dem ISO-Standard zu entsprechen.  
  
Beim Verwenden von Indizes für berechnete Spalten und indizierte Sichten müssen vier dieser Standardwerte (`ANSI_NULLS`, `ANSI_PADDING`, `ANSI_WARNINGS` und `QUOTED_IDENTIFIER`) auf ON (Ein) festgelegt sein. Diese Standardwerte gehören zu den insgesamt sieben SET-Optionen, für die bestimmte Werte beim Erstellen und Ändern von Indizes für berechnete Spalten und indizierte Sichten zugewiesen werden müssen. Die anderen SET-Optionen sind `ARITHABORT` (ON), `CONCAT_NULL_YIELDS_NULL` (ON) und `NUMERIC_ROUNDABORT` (OFF) (Aus). Weitere Informationen zu den erforderlichen Einstellungen der SET-Option mit indizierten Sichten und Indizes für berechnete Spalten finden Sie im Abschnitt [Überlegungen beim Verwenden von SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).  
  
Der ODBC-Treiber von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client und der OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] legen für ANSI_DEFAULTS beim Herstellen einer Verbindung automatisch ON fest. Treiber und Anbieter legen anschließend CURSOR_CLOSE_ON_COMMIT und IMPLICIT_TRANSACTIONS auf OFF fest. Diese OFF-Einstellungen für `CURSOR_CLOSE_ON_COMMIT` und `IMPLICIT_TRANSACTIONS` können in ODBC-Datenquellen, in ODBC-Verbindungsattributen oder in OLE DB-Verbindungseigenschaften konfiguriert werden, die in der Anwendung festgelegt werden, bevor die Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt wird. Der Standardwert von `ANSI_DEFAULTS` lautet OFF für Verbindungen von DB-Library-Anwendungen.  
  
Wenn SET ANSI_DEFAULTS ausgeführt wird, wird QUOTED_IDENTIFIER zur Analysezeit festgelegt, und die folgenden Optionen werden zur Ausführungszeit festgelegt:  
  
:::row:::
    :::column:::
        SET ANSI_NULLS
    :::column-end:::
    :::column:::
        SET ANSI_WARNINGS
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_NULL_DFLT_ON
    :::column-end:::
    :::column:::
        SET CURSOR_CLOSE_ON_COMMIT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        SET ANSI_PADDING
    :::column-end:::
    :::column:::
        SET IMPLICIT_TRANSACTIONS
    :::column-end:::
:::row-end:::

## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel wird ANSI_DEFAULTS auf ON festgelegt, und die `DBCC USEROPTIONS`-Anweisung wird zum Anzeigen der betroffenen Einstellungen verwendet.  
  
```sql  
-- SET ANSI_DEFAULTS ON.  
SET ANSI_DEFAULTS ON;  
GO  

-- Display the current settings.  
DBCC USEROPTIONS;  
GO 

-- SET ANSI_DEFAULTS OFF.  
SET ANSI_DEFAULTS OFF;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DBCC USEROPTIONS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)   
 [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CURSOR_CLOSE_ON_COMMIT &#40;Transact-SQL&#41;](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)   
 [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
  
  
