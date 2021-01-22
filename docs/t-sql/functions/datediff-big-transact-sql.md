---
title: DATEDIFF_BIG (Transact-SQL)
description: DATEDIFF_BIG (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: 8f6078aebab5456b8867aca1b7f6987d8a1eb6fd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168176"
---
# <a name="datediff_big-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Diese Funktion gibt die Anzahl (große ganze Zahl mit Vorzeichen) der angegebenen *datepart*-Begrenzungen zurück, die zwischen den angegebenen Werten für *startdate* und *enddate* überschritten wurden.
  
Unter [Datums- und Uhrzeitdatentypen und zugehörige Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) finden Sie eine Übersicht über alle [!INCLUDE[tsql](../../includes/tsql-md.md)] Datums- und Uhrzeitdatentypen und zugehörige Funktionen.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  

## <a name="arguments"></a>Argumente
*datepart*  
Der Teil von *startdate* und *enddate*, der den Typ der überschrittenen Begrenzung angibt.

> [!NOTE]
> `DATEDIFF_BIG` akzeptiert keine *datepart*-Werte von benutzerdefinierten Variablen oder als Zeichenfolgen in Anführungszeichen.

In der folgenden Tabelle sind alle gültigen Namen und Abkürzungen für *datepart*-Argumente aufgeführt.
  
|*datepart*-Name| *datepart*-Abkürzung|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  

> [!NOTE]
> Alle spezifischen *datepart*-Namen und Abkürzungen für diesen *datepart*-Namen geben den gleichen Wert zurück.

*startdate*  
Ein Ausdruck, der in einen der folgenden Werte aufgelöst werden kann:

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Bei *date* akzeptiert `DATEDIFF_BIG` einen Spaltenausdruck, einen Ausdruck, ein Zeichenfolgenliteral oder eine benutzerdefinierte Variable. Ein Zeichenfolgenliteralwert muss in ein **datetime**-Argument aufgelöst werden. Um Mehrdeutigkeiten zu vermeiden, sollten Sie vierstellige Jahreszahlen verwenden. `DATEDIFF_BIG` subtrahiert *startdate* von *enddate*. Um Mehrdeutigkeiten zu vermeiden, sollten Sie vierstellige Jahreszahlen verwenden. Unter [Konfigurieren der Serverkonfigurationsoption „Umstellungsjahr für Angaben mit zwei Ziffern“](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) finden Sie weitere Informationen zu zweistelligen Jahreszahlen.
  
*enddate*  
Weitere Informationen finden Sie unter *startdate*.
  
## <a name="return-type"></a>Rückgabetyp  
Signierter **bigint**-Wert  
  
## <a name="return-value"></a>Rückgabewert  
Hierbei wird der **bigint**-Unterschied zwischen den Werten für *startdate* und *enddate*, ausgedrückt in dem durch *datepart* festgelegten Grenzwert, zurückgegeben.
  
Bei einem Rückgabewert, der sich außerhalb des gültigen Bereichs für **bigint** (-9,223,372,036,854,775,808 bis 9,223,372,036,854,775,807) befindet, gibt `DATEDIFF_BIG` einen Fehler zurück. In einem anderen Fall hingegen wird ein **int**-Wert zurückgegeben. Aus diesem Grund kann ein **minute**- oder ein höherer Wert zu einem Überlauf führen. In `DATEDIFF_BIG` kann ein Überlauf auftreten, wenn die Genauigkeit **nanosecond** verwendet wird und der Unterschied zwischen *enddate* und *startdate* 292 Jahre, 3 Monate, 10 Tage, 23 Stunden 47 Minuten und 16,8547758 Sekunden überschreitet.
  
Wenn *startdate* und *enddate* jeweils nur ein Uhrzeitwert zugewiesen ist und *datepart* keine Zeit-*datepart*-Funktion ist, gibt `DATEDIFF_BIG` 0 zurück.
  
Beim Berechnen des Rückgabewerts verwendet `DATEDIFF_BIG` eine Komponente von *startdate* oder *enddate* für den Zeitzonenoffset.
  
Bei einem **smalldatetime**-Wert, der für *startdate* oder *enddate* verwendet wird, legt `DATEDIFF_BIG` die Sekunden und Millisekunden im Rückgabewert immer auf 0 (null) fest, da [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) nur auf die Minute genaue Werte enthält.
  
Wenn der Variablen eines Datumsdatentyps nur ein Uhrzeitwert zugewiesen ist, legt `DATEDIFF_BIG` den Wert des fehlenden Datumsteils auf den Standardwert `1900-01-01` fest. Wenn der Variablen eines Uhrzeit- oder Datumsdatentyps nur ein Datumswert zugewiesen ist, legt `DATEDIFF_BIG` den Wert des fehlenden Uhrzeitteils auf den Standardwert `00:00:00` fest. Wenn entweder *startdate* oder *enddate* nur über einen Uhrzeitteil und der andere nur über einen Datumsteil verfügt, legt `DATEDIFF_BIG` für die fehlenden Uhrzeit- und Datumstypen die Standardwerte fest.
  
Wenn *startdate* und *enddate* unterschiedliche Datumsdatentypen aufweisen und ein Datentyp mehr Uhrzeitteile oder eine höhere Genauigkeit bezüglich der Bruchteile von Sekunden aufweist als der andere Teil, legt `DATEDIFF_BIG` für die fehlenden Teile des anderen Datentyps 0 (null) fest.
  
## <a name="datepart-boundaries"></a>datepart-Begrenzungen
Die folgenden Anweisungen weisen bei *startdate* und *enddate* den gleichen Wert auf. Die Datumsangaben folgen aufeinander und unterscheiden sich in der Uhrzeit um eine Mikrosekunde (0,0000001 Sekunden). Der Unterschied zwischen *startdate* und *enddate* in jeder Anweisung überschreitet eine Kalender- oder Uhrzeitbegrenzung des zugehörigen *datepart*. Jede Anweisung gibt 1 zurück. Wenn *startdate* und *enddate* unterschiedliche Jahreswerte aufweisen, die Kalenderwochenwerte jedoch identisch sind, gibt `DATEDIFF_BIG` für den *datepart*-Wert **week** 0 (null) zurück.

```sql
SELECT DATEDIFF_BIG(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>Bemerkungen  
Verwenden Sie `DATEDIFF_BIG` in den Klauseln `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` und `ORDER BY`.
  
`DATEDIFF_BIG` wandelt Zeichenfolgenliterale implizit in den **datetime2**-Typ um. Daher unterstützt `DATEDIFF_BIG` das Format YDM nicht, wenn das Datum als Zeichenfolge übergeben wird. Sie müssen die Zeichenfolge explizit in den Typ **datetime** oder **smalldatetime** umwandeln, um das YDM-Format zu verwenden.
  
Das Festlegen von `SET DATEFIRST` wirkt sich nicht auf `DATEDIFF_BIG` aus. `DATEDIFF_BIG` verwendet immer Sonntag als ersten Wochentag, um sicherzustellen, dass die Funktion deterministisch ist.

In `DATEDIFF_BIG` kann bei einem **nanosecond**-Wert ein Überlauf auftreten, wenn der Unterschied zwischen *enddate* und *startdate* einen Wert zurückgibt, der sich außerhalb des Bereichs von **bigint** befindet.
  
## <a name="examples"></a>Beispiele 
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>Angeben von Spalten für startdate und enddate  
In diesem Beispiel werden verschiedene Typen von Ausdrücken als Argumente für die Parameter *startdate* und *enddate* verwendet. Die Anzahl der Tagesbegrenzungen wird berechnet, die von den Datumsangaben in zwei Spalten in einer Tabelle überschritten wurden.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF_BIG(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  

### <a name="finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>Ermitteln des Unterschieds zwischen „startdate“ und „enddate“ als Zeichenfolgen für Datumsteile

```sql
DECLARE @date1 DATETIME2, @date2 DATETIME2, @result VARCHAR(100)
DECLARE @years BIGINT, @months BIGINT, @days BIGINT, @hours BIGINT, @minutes BIGINT, @seconds BIGINT, @milliseconds BIGINT

SET @date1 = '0001-01-01 00:00:00.00000000'
SET @date2 = '2018-12-12 07:08:01.12345678'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
          + CASE WHEN @milliseconds > 0 THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
               ELSE '' END 
          + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
2017 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```   

Weitere vergleichbare Beispiele finden Sie unter [DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md).
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md)
  
  
