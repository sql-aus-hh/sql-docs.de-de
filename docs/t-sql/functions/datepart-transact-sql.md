---
title: DATEPART (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Funktion DATEPART Diese Funktion gibt eine ganze Zahl zurück, die dem datepart-Argument eines angegebenen Datums entspricht.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adae6be66b149f930bee51c636bf86d7614e3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093668"
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Diese Funktion gibt eine ganze Zahl zurück, die das angegebene *datepart*-Argument des angegebenen *date*-Arguments darstellt.
  
Unter [Datums- und Uhrzeitdatentypen und zugehörige Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) finden Sie eine Übersicht über alle [!INCLUDE[tsql](../../includes/tsql-md.md)] Datums- und Uhrzeitdatentypen und zugehörige Funktionen.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DATEPART ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*datepart*  
Der bestimmte Teil des *date*-Arguments, für das `DATEPART` eine **ganze Zahl** zurückgibt. In der folgenden Tabelle werden alle gültigen *datepart*-Argumente aufgeführt.

> [!NOTE]
> `DATEPART` akzeptiert keine benutzerdefinierten Variablenentsprechungen für die *datepart*-Argumente.
  
|*datepart*|Abkürzungen|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**tzoffset**|**tz**|  
|**iso_week**|**isowk**, **isoww**|  
  
*date*  
Ein Ausdruck, der in einen der folgenden Datentypen aufgelöst werden kann: 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Bei *date* akzeptiert `DATEPART` einen Spaltenausdruck, einen Ausdruck, ein Zeichenfolgenliteral oder eine benutzerdefinierte Variable. Um Mehrdeutigkeiten zu vermeiden, sollten Sie vierstellige Jahreszahlen verwenden. Unter [Konfigurieren der Serverkonfigurationsoption „Umstellungsjahr für Angaben mit zwei Ziffern“](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) finden Sie weitere Informationen zu zweistelligen Jahreszahlen.
  
## <a name="return-type"></a>Rückgabetyp  
 **int**  
  
## <a name="return-value"></a>Rückgabewert  
Jedes *datepart*-Argument und die jeweils zugehörigen Abkürzungen geben den gleichen Wert zurück.
  
Der Rückgabewert hängt von der Sprachumgebung ab, die durch [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) und durch die [Konfiguration der Serverkonfigurationsoption „Standardsprache“](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) für die Anmeldung festgelegt wurde. Der Rückgabewert hängt von [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) ab, wenn *date* ein Zeichenfolgenliteral einiger Formate darstellt. SET DATEFORMAT ändert den Rückgabewert nicht, wenn das Datum einen Spaltenausdruck eines Datums- oder Uhrzeittyps darstellt.
  
In dieser Tabelle werden alle *datepart*-Argumente mit den entsprechenden Rückgabewerten für die Anweisung `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')` aufgelistet. Das *date*-Argument weist den Datentyp **datetimeoffset(7)** auf. Die letzten beiden Stellen des Rückgabewerts des *datepart*-Arguments **nanosecond** sind immer `00`, und dieser Wert verfügt über 9 Dezimalstellen:

**,123456700**
  
|*datepart*|Rückgabewert|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|3|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**tzoffset, tz**|310|  
|**iso_week, isowk, isoww**|44|  
  
## <a name="week-and-weekday-datepart-arguments"></a>datepart-Argumente des Typs week und weekday
Für das *datepart*-Argument **week** (**wk**, **ww**) oder **weekday** (**dw**) hängt der Rückgabewert `DATEPART` von dem von [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md) festgelegten Wert ab.
  
Der 1. Januar eines Jahres definiert die Anfangszahl für **week** _datepart_. Beispiel:

DATEPART (**wk**, 'Jan 1, *xxx* x') = 1

Hierbei steht *xxxx* für ein beliebiges Jahr.
  
In dieser Tabelle wird für jedes „SET DATEFIRST“-Argument der Rückgabewert der *datepart*-Argumente **week** und **weekday** für '2007-04-21' aufgelistet. Der 1. Januar 2007 ist ein Montag. Der 21. April 2007 ist ein Sonntag. Für Englisch (USA) dient

`SET DATEFIRST 7 -- ( Sunday )`

als Standardeinstellung. Nachdem Sie DATEFIRST festgelegt haben, verwenden Sie diese empfohlene SQL-Anweisung für die datepart-Tabellenwerte:

`SELECT DATEPART(week, '2007-04-21 '), DATEPART(weekday, '2007-04-21 ')`
  
|SET DATEFIRST<br /><br /> Argument|week<br /><br /> hat zurückgegeben|weekday<br /><br /> hat zurückgegeben|  
|---|---|---|
|1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17|1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>datepart-Argumente des Typs year, month und day  
Die für DATEPART (**year**, *date*), DATEPART (**month**, *date*) und DATEPART (**day**, *date*) zurückgegebenen Werte entsprechen den jeweiligen Rückgabewerten der Funktionen [YEAR](../../t-sql/functions/year-transact-sql.md), [MONTH](../../t-sql/functions/month-transact-sql.md) und [DAY](../../t-sql/functions/day-transact-sql.md).
  
## <a name="iso_week-datepart"></a>datepart-Argument „iso_week“  
ISO 8601 schließt das ISO-Wochensystem zur Nummerierung von Wochen ein. Die einzelnen Wochen werden mit dem Jahr verknüpft, in dem Donnerstag auftritt. Beispielsweise beginnt die Woche 1 im Jahr 2004 (2004W01) am 29. Dezember 2003 (Montag) und endet am 4. Januar 2004 (Sonntag). Europäische Länder und Regionen verwenden üblicherweise diese Art der Nummerierung. Nichteuropäische Länder und Regionen verwenden diese üblicherweise nicht.

Hinweis: Die höchste Wochennummer in einem Jahr kann 52 oder 53 sein.
  
Das Nummerierungssystem anderer Länder oder Regionen entspricht möglicherweise nicht dem ISO-Standard. Diese Tabelle stellt sechs Möglichkeiten dar:
  
|Erster Tag der Woche|Erste Woche im Jahr enthält|Doppelt zugewiesene Wochen|Verwendet von/in|  
|---|---|---|---|
|Sonntag|1\. Januar<br /><br /> Erster Samstag<br /><br /> 1–7 Tage im Jahr|Ja|USA|  
|Montag|1\. Januar<br /><br /> Erster Sonntag<br /><br /> 1–7 Tage im Jahr|Ja|Die meisten Länder Europas und das Vereinigte Königreich|  
|Montag|4\. Januar<br /><br /> Erster Donnerstag<br /><br /> 4–7 Tage im Jahr|Nein|ISO 8601, Norwegen und Schweden|  
|Montag|7\. Januar,<br /><br /> Erster Montag<br /><br /> 7 Tage im Jahr|Nein||  
|Wednesday|1\. Januar<br /><br /> Erster Dienstag<br /><br /> 1–7 Tage im Jahr|Ja||  
|Samstag|1\. Januar<br /><br /> Erster Freitag<br /><br /> 1–7 Tage im Jahr|Ja||  
  
## <a name="tzoffset"></a>tzoffset  
`DATEPART` gibt den **tzoffset**-Wert (**tz**) als Anzahl von Minuten (mit Vorzeichen) zurück. Diese Anweisung gibt einen Zeitzonenoffset von 310 Minuten zurück:
  
```sql
SELECT DATEPART (tzoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
`DATEPART` rendert den „tzoffset“-Wert folgendermaßen:
- Für „datetimeoffset“ und „datetime2“ gibt „tzoffset“ den Zeitoffset in Minuten zurück, wobei der Offset für „datetime2“ immer 0 Minuten beträgt.
- Für Datentypen, die implizit in **datetimeoffset** oder **datetime2** konvertiert werden können, gibt `DATEPART` den Zeitoffset in Minuten an. Eine Ausnahme stellen andere Datums- und Uhrzeitdatentypen dar.
- Parameter aller anderen Typen führen zu einem Fehler.
  
  
## <a name="smalldatetime-date-argument"></a>date-Argument des Typs smalldatetime  
Für den *date*-Wert [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) gibt `DATEPART` die Sekunden als 00 zurück.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>Zurückgeben des Standardwerts für ein datepart-Argument, das nicht in einem date-Argument enthalten ist  
Enthält der Datentyp des *date*-Arguments keine Angabe zu *datepart*, gibt `DATEPART` den Standardwert für *datepart* nur zurück, wenn für *date* ein Literal angegeben ist.
  
Beispielsweise wird bei Jahr-Monat-Tag für jeden **date**-Datentyp standardmäßig der Wert 1900-01-01 angegeben. Diese Anweisung verfügt über datepart-Argumente für *datepart*, ein time-Argument für *date* und gibt `1900, 1, 1, 1, 2` zurück.
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
Wenn *date* als Variable oder Tabellenspalte angegeben ist und der Datentyp für diese Variable oder Spalte nicht über das angegebene *datepart*-Argument verfügt, gibt `DATEPART` den Fehler 9810 zurück. Im folgenden Beispiel hat die Variable *\@t* den Datentyp **time**. Bei der Ausführung würde ein Fehler auftreten, weil der Datumsteil „year“ für den **time**-Datentyp ungültig ist:
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>Sekundenbruchteile
Diese Anweisungen veranschaulichen, dass `DATEPART` Sekundenbruchteile zurückgibt:
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>Bemerkungen  
`DATEPART` kann in den Klauseln SELECT <Liste>, WHERE, HAVING, GROUP BY und ORDER BY verwendet werden.
  
In [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] wandelt DATEPART Zeichenfolgenliterale implizit in den **datetime2**-Typ um. Daher unterstützt DATENAME das Format YDM nicht, wenn das Datum als Zeichenfolge übergeben wird. Sie müssen die Zeichenfolge explizit in den Typ **datetime** oder **smalldatetime** umwandeln, um das YDM-Format zu verwenden.
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird das Basisjahr zurückgegeben. Das Basisjahr ist bei Datumsberechnungen nützlich. Im Beispiel gibt eine Zahl das Datum an. Beachten Sie, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Wert 0 als 1. Januar 1900 interpretiert.
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  

-- Returns: 1900    1    1 
```  
  
In diesem Beispiel wird der Tag des Datums (`12/20/1974`) zurückgegeben.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 20
```  
  
In diesem Beispiel wird das Jahr des Datums (`12/20/1974`) zurückgegeben.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 1974
```  
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
