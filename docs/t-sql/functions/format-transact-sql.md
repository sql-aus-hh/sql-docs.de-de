---
title: FORMAT (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Funktion FORMAT
ms.date: 08/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FORMAT_TSQL
- FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- FORMAT function
ms.assetid: dad6f24c-b8d9-4dbe-a561-9b167b8f20c8
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: bfd9a1079dded6f6ffa466e8161aa98eb20c0106
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093576"
---
# <a name="format-transact-sql"></a>FORMAT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Gibt einen mit dem angegebenen Format und der optionalen Kultur formatierten Wert zurück. Verwenden Sie die FORMAT-Funktion für die gebietsschemabasierte Formatierung von Datums-/Uhrzeitwerten sowie numerischen Werten als Zeichenfolgen. Für allgemeine Datentypkonvertierungen verwenden Sie CAST oder CONVERT.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
FORMAT( value, format [, culture ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

 *value*  
 Ausdruck eines unterstützten Datentyps, der formatiert werden soll. Eine Liste gültiger Typen finden Sie in der Tabelle im folgenden Abschnitt mit Hinweisen.  
  
 *format*  
 **nvarchar** -Formatmuster.  
  
 Das *format* -Argument muss eine gültige .NET Framework-Formatzeichenfolge enthalten, entweder als Standardformatzeichenfolge (z. B. "C" oder "D") oder als ein Muster aus benutzerdefinierten Zeichen für Datumsangaben und numerische Werte (z. B. "MMMM-DD, yyyy (dddd)"). Kombinierte Formatierung wird nicht unterstützt. Ausführliche Erläuterungen zu diesen Formatierungsmustern können Sie der .NET Framework-Dokumentation zur allgemeinen Zeichenfolgenformatierung sowie zu benutzerdefinierten Datums- und Uhrzeitformaten und benutzerdefinierten Zahlenformaten entnehmen. Ein guter Ausgangspunkt ist das Thema zu "[Formatierungstypen](https://go.microsoft.com/fwlink/?LinkId=211776)".  
  
 *culture*  
 Optionales **nvarchar** -Argument, das eine Kultur angibt.  
  
 Wenn das *culture* -Argument nicht angegeben wurde, wird die Sprache der aktuellen Sitzung verwendet. Diese Sprache ist entweder implizit definiert oder wird explizit mit der Anweisung SET LANGUAGE festgelegt. *culture* lässt alle von .NET Framework unterstützten Kulturen als Argument zu und ist auf die explizit durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützten Sprachen beschränkt. Wenn das *culture* -Argument nicht gültig ist, löst FORMAT einen Fehler aus.  
  
## <a name="return-types"></a>Rückgabetypen

 **nvarchar** oder NULL.  
  
 Die Länge des Rückgabewerts wird von *format* bestimmt.  
  
## <a name="remarks"></a>Bemerkungen

 FORMAT gibt bei Fehlern, bei denen es sich nicht um eine *culture* handelt, die nicht *valid* ist, NULL zurück. NULL wird z. B. zurückgegeben, wenn der in *format* angegebene Wert nicht gültig ist.  

 Die FORMAT-Funktion ist nicht deterministisch.
  
 Für FORMAT muss die .NET Framework-Common Language Runtime (CLR) vorhanden sein.  
  
 Diese Funktion kann nicht remote ausgeführt werden, da sie die Existenz der CLR voraussetzt. Die Remoteausführung einer Funktion, die die CLR erfordert, kann einen Fehler auf dem Remoteserver auslösen.  
  
 FORMAT basiert auf den CLR-Formatierungsregeln, die vorgeben, dass Doppelpunkte und Punkte mit Escapezeichen versehen werden müssen. Wenn die Formatzeichenfolge (der zweite Parameter) einen Doppelpunkt oder Punkt enthält, muss dieser daher mit einem umgekehrten Schrägstrich versehen werden, wenn ein Eingabewert (erster Parameter) den Datentyp **time** aufweist. Weitere Informationen finden Sie unter [D: FORMAT mit time-Datentypen](#ExampleD).  
  
 In der folgenden Tabelle sind zulässige Datentypen für das *value* -Argument sowie die entsprechenden .NET Framework-Zuordnungstypen aufgeführt.  
  
|Category|type|.NET-Typ|  
|--------------|----------|---------------|  
|Numeric|BIGINT|Int64|  
|Numeric|INT|Int32|  
|Numeric|SMALLINT|Int16|  
|Numeric|TINYINT|Byte|  
|Numeric|Decimal|SqlDecimal|  
|Numeric|NUMERIC|SqlDecimal|  
|Numeric|float|Double|  
|Numeric|real|Single|  
|Numeric|SMALLMONEY|Decimal|  
|Numeric|money|Decimal|  
|Datum und Uhrzeit|date|Datetime|  
|Datum und Uhrzeit|time|TimeSpan|  
|Datum und Uhrzeit|datetime|Datetime|  
|Datum und Uhrzeit|smalldatetime|Datetime|  
|Datum und Uhrzeit|datetime2|Datetime|  
|Datum und Uhrzeit|datetimeoffset|DateTimeOffset|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-simple-format-example"></a>A. Einfaches Beispiel für FORMAT

 Im folgenden Beispiel wird ein einfaches, für andere Kulturen formatiertes Datum zurückgegeben.  
  
```sql  
DECLARE @d DATE = '11/22/2020';
SELECT FORMAT( @d, 'd', 'en-US' ) 'US English'  
      ,FORMAT( @d, 'd', 'en-gb' ) 'Great Britain English'  
      ,FORMAT( @d, 'd', 'de-de' ) 'German'  
      ,FORMAT( @d, 'd', 'zh-cn' ) 'Simplified Chinese (PRC)';  
  
SELECT FORMAT( @d, 'D', 'en-US' ) 'US English'  
      ,FORMAT( @d, 'D', 'en-gb' ) 'Great Britain English'  
      ,FORMAT( @d, 'D', 'de-de' ) 'German'  
      ,FORMAT( @d, 'D', 'zh-cn' ) 'Chinese (Simplified PRC)';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
US English  Great Britain English German     Simplified Chinese (PRC)  
----------  --------------------- ---------- ------------------------  
11/22/2020  22/11/2020            22.11.2020 2020/11/22 
  
US English                  Great Britain English  German                      Chinese (Simplified PRC)  
--------------------------- ---------------------- --------------------------  ---------------------------------------  
Sunday, November 22, 2020   22 November 2020       Sonntag, 22. November 2020  2020年11月22日  
  
```  
  
### <a name="b-format-with-custom-formatting-strings"></a>B. FORMAT mit benutzerdefinierten Formatierungszeichenfolgen

 Im folgenden Beispiel ist das Formatieren von numerischen Werten durch Angeben eines benutzerdefinierten Formats dargestellt. Im Beispiel wird davon ausgegangen, dass das aktuelle Datum der 27. September 2012 ist. Weitere Informationen zu diesen und anderen benutzerdefinierten Formaten finden Sie unter [Benutzerdefinierte Zahlenformatzeichenfolgen](https://msdn.microsoft.com/library/0c899ak8.aspx).  
  
```sql  
DECLARE @d DATE = GETDATE();  
SELECT FORMAT( @d, 'dd/MM/yyyy', 'en-US' ) AS 'Date'  
       ,FORMAT(123456789,'###-##-####') AS 'Custom Number';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Date        Custom Number  
----------  -------------  
22/11/2020  123-45-6789  
  
```  
  
### <a name="c-format-with-numeric-types"></a>C. FORMAT mit numerischen Typen

 Im folgenden Beispiel werden fünf Zeilen aus der **Sales.CurrencyRate**-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben. Die Spalte **EndOfDateRate** wird als **money** -Typ in der Tabelle gespeichert. In diesem Beispiel wird die Spalte unformatiert zurückgegeben und wird dann durch Angeben der Typen für das .NET-Zahlenformat, das allgemeine Format und das Währungsformat formatiert. Weitere Informationen zu diesen und anderen Zahlenformaten finden Sie unter [Benutzerdefinierte Zahlenformatzeichenfolgen](https://msdn.microsoft.com/library/dwhawy9k.aspx).  
  
```sql  
SELECT TOP(5) CurrencyRateID, EndOfDayRate  
            ,FORMAT(EndOfDayRate, 'N', 'en-us') AS 'Number Format'  
            ,FORMAT(EndOfDayRate, 'G', 'en-us') AS 'General Format'  
            ,FORMAT(EndOfDayRate, 'C', 'en-us') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1.00            1.0002          $1.00  
2              1.55          1.55            1.5500          $1.55  
3              1.9419        1.94            1.9419          $1.94  
4              1.4683        1.47            1.4683          $1.47  
5              8.2784        8.28            8.2784          $8.28  
  
```  
  
 In diesem Beispiel wird die Kultur "Deutsch" (de-de) angegeben.  
  
```sql  
SELECT TOP(5) CurrencyRateID, EndOfDayRate  
      ,FORMAT(EndOfDayRate, 'N', 'de-de') AS 'Numeric Format'  
      ,FORMAT(EndOfDayRate, 'G', 'de-de') AS 'General Format'  
      ,FORMAT(EndOfDayRate, 'C', 'de-de') AS 'Currency Format'  
FROM Sales.CurrencyRate  
ORDER BY CurrencyRateID;  
```  
  
```
CurrencyRateID EndOfDayRate  Numeric Format  General Format  Currency Format  
-------------- ------------  --------------  --------------  ---------------  
1              1.0002        1,00            1,0002          1,00 &euro;  
2              1.55          1,55            1,5500          1,55 &euro;  
3              1.9419        1,94            1,9419          1,94 &euro;  
4              1.4683        1,47            1,4683          1,47 &euro;  
5              8.2784        8,28            8,2784          8,28 &euro;  
  
```  
  
### <a name="d-format-with-time-data-types"></a><a name="ExampleD"></a> D. FORMAT mit time-Datentypen

 FORMAT gibt in diesen Fällen NULL zurück, da `.` und `:` nicht mit Escapezeichen versehen sind.  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh.mm');   --> returns NULL  
SELECT FORMAT(cast('07:35' as time), N'hh:mm');   --> returns NULL  
```  
  
 FORMAT gibt eine formatierte Zeichenfolge zurück, da `.` und `:` mit Escapezeichen versehen sind.  
  
```sql  
SELECT FORMAT(cast('07:35' as time), N'hh\.mm');  --> returns 07.35  
SELECT FORMAT(cast('07:35' as time), N'hh\:mm');  --> returns 07:35  
```  

FORMAT gibt eine formatierte aktuelle Uhrzeit mit Angabe von AM oder PM zurück.

```sql
SELECT FORMAT(SYSDATETIME(), N'hh:mm tt'); -- returns 03:46 PM
SELECT FORMAT(SYSDATETIME(), N'hh:mm t'); -- returns 03:46 P
```

Format gibt die angegebene Uhrzeit mit der Angabe AM zurück.

```sql
select FORMAT(CAST('2018-01-01 01:00' AS datetime2), N'hh:mm tt') -- returns 01:00 AM
select FORMAT(CAST('2018-01-01 01:00' AS datetime2), N'hh:mm t')  -- returns 01:00 A
```

Format gibt die angegebene Uhrzeit mit der Angabe PM zurück.

```sql
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'hh:mm tt') -- returns 02:00 PM
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'hh:mm t') -- returns 02:00 P
```
  
Format gibt die angegebene Uhrzeit im 24-Stunden-Format zurück.

```sql
select FORMAT(CAST('2018-01-01 14:00' AS datetime2), N'HH:mm') -- returns 14:00
```
  
## <a name="see-also"></a>Weitere Informationen

- [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
- [STR &#40;Transact-SQL&#41;](../../t-sql/functions/str-transact-sql.md)  
- [String Functions &#40;Transact-SQL&#41; (Zeichenfolgenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/string-functions-transact-sql.md)
