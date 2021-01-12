---
title: DATEADD (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Funktion DATEADD Diese Funktion gibt ein Datum zurück, das durch das angegebene datepart-Argument geändert wurde.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4edc9ff57e9f8d1bd875df6d62814b3317c7ac45
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093680"
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion fügt einen angegebenen *number*-Wert (als ganze Zahl mit Vorzeichen) zu einem angegebenen *datepart*-Wert eines eingegebenen *date*-Werts hinzu und gibt diesen geänderten Wert anschließend zurück.
  
Unter [Datums- und Uhrzeitdatentypen und zugehörige Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) finden Sie eine Übersicht über alle [!INCLUDE[tsql](../../includes/tsql-md.md)] Datums- und Uhrzeitdatentypen und zugehörige Funktionen.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```sql
DATEADD (datepart , number , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*datepart*  
Der Teil des *date*-Werts, zu dem `DATEADD` einen **ganzzahligen** *number*-Wert hinzufügt. In der folgenden Tabelle werden alle gültigen *datepart*-Argumente aufgeführt. 

> [!NOTE]
> `DATEADD` akzeptiert keine benutzerdefinierten Variablenentsprechungen für die *datepart*-Argumente. 
  
|*datepart*|Abkürzungen|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**, **w**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*Zahl*  
Ein Ausdruck, der in einen [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)-Wert aufgelöst werden kann, den `DATEADD` zu einem *datepart*-Argument von *date* hinzufügt. `DATEADD` akzeptiert für *number* benutzerdefinierte Variablenwerte. `DATEADD` schneidet einen angegebenen *number*-Wert ab, wenn dieser einen Dezimalbruch aufweist. In diesem Fall wird der *number*-Wert nicht gerundet.
  
*date*  
Ein Ausdruck, der in einen der folgenden Werte aufgelöst werden kann: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Bei *date* akzeptiert `DATEADD` einen Spaltenausdruck, einen Ausdruck, ein Zeichenfolgenliteral oder eine benutzerdefinierte Variable. Ein Zeichenfolgenliteralwert muss in ein **datetime**-Argument aufgelöst werden. Um Mehrdeutigkeiten zu vermeiden, sollten Sie vierstellige Jahreszahlen verwenden. Unter [Konfigurieren der Serverkonfigurationsoption „Umstellungsjahr für Angaben mit zwei Ziffern“](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) finden Sie weitere Informationen zu zweistelligen Jahreszahlen.
  
## <a name="return-types"></a>Rückgabetypen

Der Datentyp des Rückgabewerts für diese Methode ist dynamisch. Der Rückgabetyp hängt von dem Argument ab, das für `date` angegeben wird. Wenn der Wert für `date` ein Zeichenfolgenliterale für ein Datum ist, gibt `DATEADD` einen **datetime** Wert zurück. Wenn für `date` ein anderer gültiger Eingabedatentyp angegeben wird, gibt `DATEADD` denselben Datentyp zurück. `DATEADD` löst einen Fehler aus, wenn die Staffelung des Zeichenfolgenliterals in Sekunden mehr als drei Dezimalstellen (,nnn) umfasst oder wenn das Zeichenfolgenliteral den Teil des Zeitzonenoffsets enthält.
  
## <a name="return-value"></a>Rückgabewert  
  
## <a name="datepart-argument"></a>datepart-Argument  
**dayofyear**, **day** und **weekday** geben den gleichen Wert zurück.
  
Jedes *datepart*-Argument und die jeweils zugehörigen Abkürzungen geben den gleichen Wert zurück.
  
Wenn Folgendes zutrifft:

+ Für das *datepart*-Argument ist **month** festgelegt
+ Der Monat mit dem Wert *date* weist mehr Tage auf als der Rückgabemonat
+ Der Tag *date* ist im Rückgabemonat nicht vorhanden

Anschließend gibt `DATEADD` den letzten Tag des Rückgabemonats zurück. Beispiel: Der September hat 30 (dreißig) Tage. Daher geben die beiden Anweisungen 2006-09-30 00:00:00.000 zurück:
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>number-Argument  
Das *number*-Argument kann den Bereich von **int** nicht überschreiten. In den folgenden Anweisungen überschreitet das Argument für *number* den Bereich von **int** um 1. Diese Anweisungen geben folgende Fehlermeldung zurück: „`Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`“
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>date-Argument  
`DATEADD` akzeptiert kein *date*-Argument, das in einen Wert außerhalb des Bereichs der zugehörigen Daten inkrementiert wird. In den folgenden Anweisungen überschreitet der *number*-Wert, der zum *date*-Wert hinzugefügt wird, den Bereich des Datentyps *date*. `DATEADD` gibt die folgende Fehlermeldung zurück: „`Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`.“
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>Rückgabewerte für ein Datum vom Typ smalldatetime und einen datepart-Wert in Sekunden oder Sekundenbruchteilen  
Die Sekundenangabe eines [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)-Werts ist immer 00. Für einen **smalldatetime**-*date*-Wert gilt Folgendes: 

-   Bei dem *datepart*-Wert **second** und einem *number*-Wert zwischen -30 und +29 nimmt `DATEADD` keine Änderungen vor.  
-   Bei dem *datepart*-Wert **second** und einem *number*-Wert, der niedriger als -30 oder höher als +29 ist, beginnt `DATEADD` bei einer Minute mit der Hinzufügung.  
-   Bei dem *datepart*-Wert **millisecond** und einem *number*-Wert zwischen -30001 und +29998 nimmt `DATEADD` keine Änderungen vor.  
-   Bei dem *datepart*-Wert **millisecond** und einem *number*-Wert, der niedriger als -30001 oder höher als +29998 ist, beginnt `DATEADD` bei einer Minute mit der Hinzufügung.  
  
## <a name="remarks"></a>Bemerkungen  
Verwenden Sie `DATEADD` in den folgenden Klauseln:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
## <a name="fractional-seconds-precision"></a>Genauigkeit in Millisekunden
`DATEADD` lässt die Addition eines *datepart*-Arguments vom Typ **microsecond** oder **nanosecond** mit den Datentypen **smalldatetime**, **date** und **datetime** bei *date* nicht zu.
  
Millisekunden besitzen drei Dezimalstellen (,123). Mikrosekunden besitzen sechs Dezimalstellen (,123456) und Nanosekunden besitzen neun Dezimalstellen (,123456789). Die Datentypen **time**, **datetime2** und **datetimeoffset** weisen maximal 7 Dezimalstellen (,1234567) auf. Bei dem **nanosecond**-Wert *datepart* muss *number* vor 100 liegen, bevor die Sekundenbruchteile von *date* erhöht werden. Ein *number*-Wert zwischen 1 und 49 wird auf 0 abgerundet, und ein „number“-Wert zwischen 50 und 99 wird auf bis zu 100 aufgerundet.
  
Folgende Anweisungen fügen *datepart* mit einem Wert von **millisecond**, **microsecond** oder **nanosecond** hinzu.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>Zeitzonenoffset
`DATEADD` lässt das Hinzufügen für einen Zeitzonenoffset nicht zu.
  
## <a name="examples"></a>Beispiele  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>A. Inkrementieren von datepart mit einem Intervall von 1  
Jede dieser Anweisungen inkrementiert *datepart* mit einem Intervall von 1:
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>B. Inkrementieren mehrerer Ebenen von datepart in einer Anweisung  
Jede dieser Anweisungen inkrementiert *datepart* um einen *number*-Wert, der hoch genug ist, um auch den nächsthöheren *datepart*-Wert von *date* zu inkrementieren:
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.1111111  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.1111111  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.1111111  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.1111111  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.1111111  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.1111111  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.1111111  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.1111111  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.1111111  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.1121111  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>C. Verwenden von Ausdrücken als Argumente für den number-Parameter und den date-Parameter  
In diesen Beispielen werden verschiedene Typen von Ausdrücken als Argumente für die Parameter *number* und *date* verwendet. In den Beispielen wird die Datenbank „AdventureWorks“ verwendet.
  
#### <a name="specifying-a-column-as-date"></a>Angeben einer Spalte als date-Parameter  
Im folgenden Beispiel wird zu jedem Wert in der Spalte `OrderDate` der Wert `2` (zwei) hinzugefügt, um eine neue Spalte mit dem Namen `PromisedShipDate` abzuleiten:
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
Im Folgenden finden Sie einen Auszug aus dem Resultset:
  
```
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Angeben von benutzerdefinierten Variablen als Argumente für number und date  
In diesem Beispiel werden benutzerdefinierte Variablen als Argumente für *number* und *date* angegeben:
  
```sql
DECLARE @days INT = 365,   
        @datetime DATETIME = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>Angeben einer skalaren Systemfunktion als Argument für date  
In diesem Beispiel wird `SYSDATETIME` für *date* angegeben. Welcher Wert genau zurückgegeben wird, hängt davon ab, an welchem Tag und zu welcher Uhrzeit die Anweisung ausgeführt wird:
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Angeben von skalaren Unterabfragen und skalaren Funktionen als Argumente für number und date  
In diesem Beispiel werden skalare Unterabfragen (`MAX(ModifiedDate)`) als Argumente für *number* und *date* verwendet. `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` dient als Beispielargument für den Parameter „number“, das veranschaulicht, wie ein *number*-Argument aus einer Werteliste ausgewählt wird.
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Angeben von numerischen Ausdrücken und skalaren Systemfunktionen als Argumente für number und date  
In diesem Beispiel werden numerische Ausdrücke (–`(10/2))`, [unäre Operatoren](../../mdx/unary-operators.md) (`-`), ein [arithmetischer Operator](../../mdx/arithmetic-operators.md) (`/`) und skalare Systemfunktionen (`SYSDATETIME`) als Argumente für *number* und *date* verwendet.
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>Angeben von Rangfolgefunktionen als Argumente für number  
In diesem Beispiel wird eine Rangfolgefunktion als Argument für *number* verwendet.
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>Angeben einer Aggregatfensterfunktion als Argument für number  
In diesem Beispiel wird eine Aggregatfensterfunktion als Argument für *number* verwendet.
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

