---
description: CURRENT_TIMESTAMP (Transact-SQL)
title: CURRENT_TIMESTAMP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURRENT_TIMESTAMP
- CURRENT_TIMESTAMP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- niladic functions
- current date and time [SQL Server]
- time [SQL Server], current
- date and time [SQL Server], CURRENT_TIMESTAMP
- functions [SQL Server], time
- system date and time [SQL Server]
- system time [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], current date and time
- dates [SQL Server], system date and time
- CURRENT_TIMESTAMP function [SQL Server]
- time [SQL Server], system
ms.assetid: c724d9cc-7b1f-4c71-bdf5-08bc52b33afc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8725fa782ef5918ac952e927097721d2cbf14afa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093724"
---
# <a name="current_timestamp-transact-sql"></a>CURRENT_TIMESTAMP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt den aktuellen Zeitstempel des Datenbanksystems ohne den Zeitzonenoffset der Datenbank als **datetime**-Wert zurück. `CURRENT_TIMESTAMP` leitet diesen Wert aus dem Betriebssystem des Computers ab, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.
  
> [!NOTE]  
>  `SYSDATETIME` und `SYSUTCDATE` haben, weil sie in Sekundenbruchteilen gemessen werden, eine höhere Genauigkeit als `GETDATE` und `GETUTCDATE`. Die `SYSDATETIMEOFFSET`-Funktion berücksichtigt den Zeitzonenoffset des Systems. Sie können `SYSDATETIME`, `SYSUTCDATE` und `SYSDATETIMEOFFSET` einer Variablen zuweisen, die einen der Datums- und Uhrzeittypen hat.  
  
Diese Funktion ist die ANSI SQL-Entsprechung zu [GETDATE](../../t-sql/functions/getdate-transact-sql.md).
  
Unter [Datums- und Uhrzeitdatentypen und Funktionen ](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) finden Sie eine Übersicht über alle Datums- und Uhrzeitdatentypen und -funktionen für [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CURRENT_TIMESTAMP  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
Diese Funktion akzeptiert keine Argumente.
  
## <a name="return-type"></a>Rückgabetyp  
**datetime**
  
## <a name="remarks"></a>Bemerkungen  
[!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen können an jeder Stelle auf `CURRENT_TIMESTAMP` verweisen, an der sie auf einen **datetime**-Ausdruck verweisen können.
  
`CURRENT_TIMESTAMP` ist eine nichtdeterministische Funktion. Sichten und Ausdrücke, die auf diese Spalte verweisen, können nicht indiziert werden.
  
## <a name="examples"></a>Beispiele  
In diesen Beispielen werden die sechs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemfunktionen, die aktuelle Datums- und Uhrzeitwerte zurückgeben, dazu verwendet, das Datum, die Uhrzeit oder beides zurückzugeben. In den Beispielen werden die Werte der Reihe nach zurückgegeben, sodass sich deren Sekundenbruchteile unterscheiden können. Beachten Sie, dass die tatsächlich zurückgegebenen Werte dem tatsächlichen Zeitpunkt (Tag und Uhrzeit) der Ausführung entsprechen.
  
### <a name="a-get-the-current-system-date-and-time"></a>A. Abrufen des aktuellen Systemdatums und der aktuellen Systemzeit  
  
```sql
SELECT SYSDATETIME()  
    ,SYSDATETIMEOFFSET()  
    ,SYSUTCDATETIME()  
    ,CURRENT_TIMESTAMP  
    ,GETDATE()  
    ,GETUTCDATE();  
/* Returned:  
SYSDATETIME()      2007-04-30 13:10:02.0474381  
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00  
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381  
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047  
GETDATE()          2007-04-30 13:10:02.047  
GETUTCDATE()       2007-04-30 20:10:02.047  
*/
```  
  
### <a name="b-get-the-current-system-date"></a>B. Abrufen des aktuellen Systemdatums  
  
```sql
SELECT CONVERT (DATE, SYSDATETIME())  
    ,CONVERT (DATE, SYSDATETIMEOFFSET())  
    ,CONVERT (DATE, SYSUTCDATETIME())  
    ,CONVERT (DATE, CURRENT_TIMESTAMP)  
    ,CONVERT (DATE, GETDATE())  
    ,CONVERT (DATE, GETUTCDATE());  
  
/* Returned   
SYSDATETIME()      2007-05-03  
SYSDATETIMEOFFSET()2007-05-03  
SYSUTCDATETIME()   2007-05-04  
CURRENT_TIMESTAMP  2007-05-03  
GETDATE()          2007-05-03  
GETUTCDATE()       2007-05-04  
*/  
```  
  
### <a name="c-get-the-current-system-time"></a>C. Abrufen der aktuellen Systemzeit  
  
```sql
SELECT CONVERT (TIME, SYSDATETIME())  
    ,CONVERT (TIME, SYSDATETIMEOFFSET())  
    ,CONVERT (TIME, SYSUTCDATETIME())  
    ,CONVERT (TIME, CURRENT_TIMESTAMP)  
    ,CONVERT (TIME, GETDATE())  
    ,CONVERT (TIME, GETUTCDATE());  
  
/* Returned  
SYSDATETIME()      13:18:45.3490361  
SYSDATETIMEOFFSET()13:18:45.3490361  
SYSUTCDATETIME()   20:18:45.3490361  
CURRENT_TIMESTAMP  13:18:45.3470000  
GETDATE()          13:18:45.3470000  
GETUTCDATE()       20:18:45.3470000  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
```sql
SELECT CURRENT_TIMESTAMP;  
```  
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

