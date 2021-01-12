---
description: CHAR (Transact-SQL)
title: CHAR (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- char_TSQL
- char
dev_langs:
- TSQL
helpviewer_keywords:
- converting int ASCII code to character
- control characters
- tab
- ASCII conversions
- CHAR function
- carriage return
- inserting control characters
- characters [SQL Server], control
- line feed
- printing ASCII values
ms.assetid: 955afe94-539c-465d-af22-16ec45da432a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4c0281782b3a21c809ee725d4dd99d80fbad20ec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097290"
---
# <a name="char-transact-sql"></a>CHAR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion wandelt einen **int**-ASCII-Code in einen Zeichenwert um.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CHAR ( integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*integer_expression*  
Eine ganze Zahl zwischen 0 und 255. `CHAR` gibt einen `NULL`-Wert für Ganzzahlausdrücke zurück, wenn sich diese außerhalb dieses Bereichs befinden oder wenn die Ganzzahl nur das erste Byte eines Doppelbytezeichens ausdrückt.

> [!NOTE]
> Einige außereuropäische Zeichensätze, z.B. [Shift Japanese Industrial Standards](https://www.wikipedia.org/wiki/Shift_JIS), enthalten Zeichen, die in einem Einzelbyte-Codierungsschema dargestellt werden können, aber eine Mehrbytecodierung erfordern. Weitere Informationen zu Zeichensätzen finden Sie unter [Einzelbyte- und Mehrbyte-Zeichensätze](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets). 
  
## <a name="return-types"></a>Rückgabetypen
**char(1)**
  
## <a name="remarks"></a>Hinweise  
Verwenden Sie `CHAR`, um Steuerzeichen in Zeichenfolgen einzufügen. In dieser Tabelle finden Sie einige häufig verwendete Steuerzeichen.
  
|Steuerzeichen|Wert|  
|---|---|
|Registerkarte|**char(9)**|  
|Zeilenvorschub|**char(10)**|  
|Wagenrücklauf|**char(13)**|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-ascii-and-char-to-print-ascii-values-from-a-string"></a>A. Verwenden von ASCII und CHAR, um die ASCII-Werte einer Zeichenfolge auszugeben  
In diesem Beispiel werden der ASCII-Wert und das Zeichen für jedes in der Zeichenfolge `New Moon` enthaltene Zeichen ausgegeben.
  
```sql
SET TEXTSIZE 0;  
-- Create variables for the character string and for the current   
-- position in the string.  
DECLARE @position INT, @string CHAR(8);  
-- Initialize the current position and the string variables.  
SET @position = 1;  
SET @string = 'New Moon';  
WHILE @position <= DATALENGTH(@string)  
   BEGIN  
   SELECT ASCII(SUBSTRING(@string, @position, 1)),   
      CHAR(ASCII(SUBSTRING(@string, @position, 1)))  
   SET @position = @position + 1  
   END;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- -
78          N  
----------- -  
101         e  
----------- -  
119         w  
----------- -  
32  
----------- -  
77          M  
----------- -  
111         o  
----------- -  
111         o  
----------- - 
110         n  
```
  
### <a name="b-using-char-to-insert-a-control-character"></a>B. Verwenden von CHAR, um ein Steuerzeichen einzufügen  
In diesem Beispiel wird `CHAR(13)` verwendet, um den Namen und die E-Mail-Adresse eines Mitarbeiters in separaten Zeilen auszugeben, wenn die Ergebnisse der Abfrage als Text zurückgegeben werden. In diesem Beispiel wird die [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank verwendet.
  
```sql
SELECT p.FirstName + ' ' + p.LastName, + CHAR(13)  + pe.EmailAddress   
FROM Person.Person p 
INNER JOIN Person.EmailAddress pe ON p.BusinessEntityID = pe.BusinessEntityID  
  AND p.BusinessEntityID = 1;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Ken Sanchez
ken0@adventure-works.com
  
(1 row(s) affected)
```
  
### <a name="c-using-ascii-and-char-to-print-ascii-values-from-a-string"></a>C. Verwenden von ASCII und CHAR, um die ASCII-Werte einer Zeichenfolge auszugeben  
Dieses Beispiel geht von einem ASCII-Zeichensatz aus. Es gibt den Zeichenwert für sechs verschiedene Zahlenwerte von ASCII-Zeichen zurück.
  
```sql
SELECT CHAR(65) AS [65], CHAR(66) AS [66],   
CHAR(97) AS [97], CHAR(98) AS [98],   
CHAR(49) AS [49], CHAR(50) AS [50];  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
65   66   97   98   49   50  
---- ---- ---- ---- ---- ----  
A    B    a    b    1    2  
```
  
### <a name="d-using-char-to-insert-a-control-character"></a>D: Verwenden von CHAR, um ein Steuerzeichen einzufügen  
In diesem Beispiel wird `CHAR(13)` verwendet, um Informationen von „sys.databases“ in separaten Zeilen zurückzugeben, wenn die Ergebnisse der Abfrage als Text zurückgegeben werden.
  
```sql
SELECT name, 'was created on ', create_date, CHAR(13), name, 'is currently ', state_desc   
FROM sys.databases;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
name                                      create_date               name                                  state_desc  
--------------------------------------------------------------------------------------------------------------------  
master                    was created on  2003-04-08 09:13:36.390   master                  is currently  ONLINE 
tempdb                    was created on  2014-01-10 17:24:24.023   tempdb                  is currently  ONLINE   
AdventureWorksPDW2012     was created on  2014-05-07 09:05:07.083   AdventureWorksPDW2012   is currently  ONLINE 
```

### <a name="e-using-char-to-return-single-byte-characters"></a>E. Verwenden von CHAR zum Zurückgeben von Einzelbytezeichen  
Dieses Beispiel verwendet die Ganzzahl- und hexadezimal Werte im gültigen Bereich für ASCII. Die CHAR-Funktion kann japanische Einzelbytezeichen ausgeben.
  
```sql
SELECT CHAR(188) AS single_byte_representing_complete_character, 
  CHAR(0xBC) AS single_byte_representing_complete_character;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
single_byte_representing_complete_character single_byte_representing_complete_character
------------------------------------------- -------------------------------------------
ｼ                                           ｼ                                         
```

### <a name="f-using-char-to-return-multibyte-characters"></a>F. Verwenden von CHAR zum Zurückgeben von Mehrbytezeichen  
Dieses Beispiel verwendet die Ganzzahl- und hexadezimal Werte im gültigen Bereich für ASCII. Die Funktion CHAR gibt jedoch NULL zurück, da der Parameter nur das erste Byte eines Multibytezeichens darstellt.
  
```sql
SELECT CHAR(129) AS first_byte_of_double_byte_character, 
  CHAR(0x81) AS first_byte_of_double_byte_character;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
first_byte_of_double_byte_character first_byte_of_double_byte_character
----------------------------------- -----------------------------------
NULL                                NULL                                         
```
  
## <a name="see-also"></a>Siehe auch
 [ASCII &#40;Transact-SQL&#41;](../../t-sql/functions/ascii-transact-sql.md)  
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)  
 [UNICODE &#40;Transact-SQL&#41;](../../t-sql/functions/unicode-transact-sql.md)  
 [+ &#40;Verketten von Zeichenfolgen&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [String Functions &#40;Transact-SQL&#41; (Zeichenfolgenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/string-functions-transact-sql.md)
  
  

