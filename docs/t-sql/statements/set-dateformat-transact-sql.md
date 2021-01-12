---
description: SET DATEFORMAT (Transact-SQL)
title: SET DATEFORMAT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEFORMAT
- SET DATEFORMAT
- SET_DATEFORMAT_TSQL
- DATEFORMAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], formats
- dates [SQL Server], ordering date parts
- SET DATEFORMAT option [SQL Server]
- DATEFORMAT option [SQL Server]
- date and time [SQL Server], SET DATEFORMAT
- options [SQL Server], date
- date and time [SQL Server], DATEFORMAT
- dateparts [SQL Server], dateformat
ms.assetid: da217878-7ec4-477e-aa13-604073c948f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 51300c851c474b2326de5fe219fe476b5793c2c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091886"
---
# <a name="set-dateformat-transact-sql"></a>SET DATEFORMAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Legt die Reihenfolge für die Datumsteile Monat, Tag und Jahr für die Interpretation von Zeichenfolgen für das Datum fest. Diese Zeichenfolgen können folgende Typen aufweisen: **date**, **smalldatetime**, **datetime**, **datetime2** oder **datetimeoffset**.  
  
 Eine Übersicht über alle Datums- und Uhrzeitdatentypen und zugehörige Funktionen für [!INCLUDE[tsql](../../includes/tsql-md.md)] finden Sie unter [Date and Time Data Types and Functions &#40;Transact-SQL&#41; (Datums- und Uhrzeitdatentypen und zugehörige Funktionen)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SET DATEFORMAT { format | @format_var }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *format* |  **@** _format_var_  
 Reihenfolge der Datumsteile. Gültige Parameter sind **mdy**, **dmy**, **ymd**, **ydm**, **myd**, and **dym**. Kann entweder in Unicode oder in Doppelbyte-Zeichensätzen (Double-Byte Character Set, DBCS), die in Unicode konvertiert wurden, dargestellt werden. Der Standard für Der Standard für Englisch ist **mdy**. Die standardmäßige DATEFORMAT-Einstellung aller Unterstützungssprachen finden Sie unter [sp_helplanguage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md).  
  
## <a name="remarks"></a>Hinweise  
 Die DATEFORMAT-Option **ydm** wird für die Datentypen **date**, **datetime2** und **datetimeoffset** nicht unterstützt.  
  
 Die Einstellung DATEFORMAT kann Zeichenfolgen für Datumsdatentypen je nach Zeichenfolgenformat anders interpretieren. Beispielsweise entsprechen die Interpretationen von **datetime** und **smalldatetime** möglicherweise nicht **date**, **datetime2** oder **datetimeoffset**. DATEFORMAT wirkt sich bei der Konvertierung von Datumswerten für die Datenbank auf die Interpretation von Zeichenfolgen aus. Es wirkt sich nicht auf die Anzeige der Werte von Datumsdatentypen und deren Speicherformat in der Datenbank aus.  
  
 Einige Zeichenfolgenformate, z. B. ISO 8601, werden unabhängig von der DATEFORMAT-Einstellung interpretiert.  
  
 Die Einstellung von SET DATEFORMAT wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.  
  
 SET DATEFORMAT überschreibt die implizite Einstellung für das Datumsformat von [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden andere Datumszeichenfolgen als Eingaben in Sitzungen mit derselben `DATEFORMAT`-Einstellung verwendet.  
  
```sql
-- Set date format to day/month/year.  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar DATETIME2 = '31/12/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: 2008-12-31 09:01:01.123  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar DATETIME2 = '12/31/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: Msg 241: Conversion failed when converting date and/or time -- from character string.  
  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  

