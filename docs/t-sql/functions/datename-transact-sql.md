---
description: DATENAME (Transact-SQL)
title: DATENAME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9279883c9f032574e32056b2360a5947ffba022
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766207"
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt eine Zeichenfolge zurück, die den angegebenen *datepart*-Wert des angegebenen *date*-Werts darstellt.

Unter [Datums- und Uhrzeitdatentypen und zugehörige Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) finden Sie eine Übersicht über alle [!INCLUDE[tsql](../../includes/tsql-md.md)] Datums- und Uhrzeitdatentypen und zugehörige Funktionen.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DATENAME ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*datepart*  
Derjenige Teil des *date*-Arguments, der von `DATENAME` zurückgegeben wird. In der folgenden Tabelle werden alle gültigen *datepart*-Argumente aufgeführt.

> [!NOTE]
> `DATENAME` akzeptiert keine benutzerdefinierten Variablenentsprechungen für die *datepart*-Argumente.
  
|*datepart*|Abkürzungen|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**weekday**|**dw, w**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  

Ein Ausdruck, der in einen der folgenden Datentypen aufgelöst werden kann: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Bei *date* akzeptiert `DATENAME` einen Spaltenausdruck, einen Ausdruck, ein Zeichenfolgenliteral oder eine benutzerdefinierte Variable. Um Mehrdeutigkeiten zu vermeiden, sollten Sie vierstellige Jahreszahlen verwenden. Unter [Konfigurieren der Serverkonfigurationsoption „Umstellungsjahr für Angaben mit zwei Ziffern“](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) finden Sie weitere Informationen zu zweistelligen Jahreszahlen.
  
## <a name="return-type"></a>Rückgabetyp  
**nvarchar**
  
## <a name="return-value"></a>Rückgabewert  
  
-   Jedes *datepart*-Argument und die jeweils zugehörigen Abkürzungen geben den gleichen Wert zurück.  
  
Der Rückgabewert hängt von der Sprachumgebung ab, die durch [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) und durch die [Konfiguration der Serverkonfigurationsoption „Standardsprache“](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) für die Anmeldung festgelegt wurde. Der Rückgabewert hängt von [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) ab, wenn *date* ein Zeichenfolgenliteral einiger Formate darstellt. SET DATEFORMAT ändert den Rückgabewert nicht, wenn das Datum einen Spaltenausdruck eines Datums- oder Uhrzeittyps darstellt.
  
Wenn der *date*-Parameter ein **date**-Datentypargument aufweist, hängt der Rückgabewert von der Einstellung ab, die durch [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md) angegeben wurde.
  
## <a name="tzoffset-datepart-argument"></a>datepart-Argumentdes Typs TZoffset  
Wenn das *datepart*-Argument **TZoffset** (**tz**) lautet und das *date*-Argument über keinen Zeitzonenoffset verfügt, gibt `DATEADD` 0 (null) zurück.
  
## <a name="smalldatetime-date-argument"></a>date-Argument des Typs smalldatetime  
Wenn *date* vom Typ [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) ist, gibt `DATENAME` Sekunden als „00“ zurück.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>Zurückgeben des Standardwerts für ein datepart-Argument, das nicht im date-Argument enthalten ist  
Wenn der Datentyp des *date*-Arguments nicht über den angegebenen *datepart* verfügt, gibt `DATENAME` den Standardwert für *datepart* nur zurück, wenn das *date*-Argument ein Literal enthält.
  
Beispielsweise wird bei Jahr-Monat-Tag für jeden **date**-Datentyp standardmäßig der Wert 1900-01-01 angegeben. Die folgende Anweisung verfügt über datepart-Argumente für *datepart*, ein time-Argument für *date*. Deswegen gibt `DATENAME` den Wert `1900, January, 1, 1, Monday` zurück.
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
Wenn *date* als Variable oder Tabellenspalte angegeben ist und der Datentyp für diese Variable oder Spalte nicht über das angegebene *datepart*-Argument verfügt, gibt `DATENAME` den Fehler 9810 zurück. Im folgenden Beispiel hat die Variable *\@t* den Datentyp **time**. Bei der Ausführung würde ein Fehler auftreten, weil der Datumsteil „year“ für den **time**-Datentyp ungültig ist:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>Bemerkungen  

Verwenden Sie `DATENAME` in den folgenden Klauseln:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] wandelt DATENAME Zeichenfolgenliterale implizit in den **datetime2**-Typ um. `DATENAME` unterstützt das Format YDM also nicht, wenn das Datum als Zeichenfolge übergeben wird. Sie müssen die Zeichenfolge explizit in den Typ **datetime** oder **smalldatetime** umwandeln, um das YDM-Format zu verwenden.
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel werden die Datumsteile für das angegebene Datum zurückgegeben. Ersetzen Sie einen *datepart*-Wert in der Tabelle durch das `datepart`-Argument in der SELECT-Anweisung:
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Rückgabewert|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Oktober|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Tuesday|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Im folgenden Beispiel werden die Datumsteile für das angegebene Datum zurückgegeben. Ersetzen Sie einen *datepart*-Wert in der Tabelle durch das `datepart`-Argument in der SELECT-Anweisung:
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Rückgabewert|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Oktober|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Tuesday|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

