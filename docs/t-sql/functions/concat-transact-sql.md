---
description: CONCAT (Transact-SQL)
title: CONCAT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONCAT
- CONCAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT function
ms.assetid: fce5a8d4-283b-4c47-95e5-4946402550d5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e48b358b33276a42d4c4f9c1cc0915238434e02d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98088452"
---
# <a name="concat-transact-sql"></a>CONCAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt eine Zeichenfolge zurück, die das Ergebnis einer End-to-End-Verkettung oder -Verknüpfung von mindestens zwei Zeichenfolgenwerten darstellt. (Informationen zum Hinzufügen eines Trennwerts beim Verkettungsvorgang finden Sie unter [CONCAT_WS](../../t-sql/functions/concat-ws-transact-sql.md).)
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CONCAT ( string_value1, string_value2 [, string_valueN ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*string_value*  
Ein Zeichenfolgenwert, der mit den anderen Werten verkettet werden soll. Die `CONCAT`-Funktion erfordert mindestens zwei und nicht mehr als 254 string_value-Argumente.
  
## <a name="return-types"></a>Rückgabetypen  
*string_value*  
Ein Zeichenfolgenwert, dessen Länge und Typ von der Eingabe abhängig sind.
  
## <a name="remarks"></a>Bemerkungen  
`CONCAT` lässt eine variable Anzahl von Zeichenfolgenargumenten zu und verkettet (oder verknüpft) sie in einer einzelnen Zeichenfolge. Es sind mindestens zwei Eingabewerte erforderlich. Andernfalls wird durch `CONCAT` ein Fehler ausgelöst. Alle Argumente werden von `CONCAT` vor der Verkettung implizit in Zeichenfolgentypen konvertiert. NULL-Werte werden von `CONCAT` implizit in leere Zeichenfolgen konvertiert. Wenn `CONCAT` Argumente nur mit **NULL**-Werten empfängt, wird eine leere Zeichenfolge vom Typ **varchar**(1) zurückgegeben. Die implizite Konvertierung in Zeichenfolgen erfolgt basierend auf den vorhandenen Regeln für Datentypkonvertierungen. Weitere Informationen zu Datentypkonvertierungen finden Sie unter [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
Der Rückgabetyp hängt vom Typ der Argumente ab. In der folgenden Tabelle wird die Zuordnung veranschaulicht:
  
|Eingabetyp|Ausgabetyp und -länge|  
|---|---|
|1. Jedes Argument vom Typ<br><br />SQL-CLR<br><br />SQL-CLR-UDT<br><br />oder<br><br />`nvarchar(max)`|**nvarchar(max)**|  
|2. Andernfalls jedes Argument vom Typ<br><br />**varbinary(max)**<br><br />oder<br><br />**varchar(max)**|**varchar(max)** , sofern keiner der Parameter vom Typ **nvarchar** mit beliebiger Länge ist. In diesem Fall gibt `CONCAT` ein Ergebnis vom Typ **nvarchar(max)** zurück.|  
|3. Andernfalls jedes Argument vom Typ **nvarchar** mit höchstens 4000 Zeichen<br><br />(**nvarchar**(<= 4000))|**nvarchar**(<= 4000)|  
|4. In allen anderen Fällen|**varchar**(<=8000, **varchar** mit höchstens 8000 Zeichen), sofern keiner der Parameter vom Typ „nvarchar“ mit beliebiger Länge ist. In diesem Fall gibt `CONCAT` ein Ergebnis vom Typ **nvarchar(max)** zurück.|  
  
Wenn `CONCAT` Eingabeargumente vom Typ **nvarchar** mit einer Länge von <= 4000 Zeichen oder Eingabeargumente vom Typ **varchar** mit einer Länge von <= 8000 Zeichen empfängt, können implizite Konvertierungen die Länge des Ergebnisses beeinflussen. Andere Datentypen besitzen andere Längen, wenn sie implizit in Zeichenfolgen konvertiert werden. **int** (14) hat z.B. eine Zeichenfolgenlänge von 12, während **float** eine Länge von 32 hat. Aus diesem Grund gibt eine Verkettung von zwei ganzen Zahlen ein Ergebnis mit einer Länge von mindestens 24 Zeichen zurück.
  
Wenn keines der Eingabeargumente einen unterstützten LOB-Typ (Large Object) aufweist, wird der Rückgabetyp unabhängig vom Rückgabetyp auf eine Länge von 8000 Zeichen gekürzt. Durch diese Kürzung wird Speicherplatz eingespart und die Effizienz der Plangenerierung unterstützt.
  
Die CONCAT-Funktion kann über eine Remoteverknüpfung auf einem Verbindungsserver der Version [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher ausgeführt werden. Bei älteren Verbindungsservern wird der CONCAT-Vorgang lokal ausgeführt, nachdem die nicht verketteten Werte vom Verbindungsserver zurückgegeben wurden.
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-concat"></a>A. Verwenden von CONCAT  
  
```sql
SELECT CONCAT ( 'Happy ', 'Birthday ', 11, '/', '25' ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
-------------------------  
Happy Birthday 11/25  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-concat-with-null-values"></a>B. Verwenden von CONCAT mit NULL-Werten  
  
```sql
CREATE TABLE #temp (  
    emp_name NVARCHAR(200) NOT NULL,  
    emp_middlename NVARCHAR(200) NULL,  
    emp_lastname NVARCHAR(200) NOT NULL  
);  
INSERT INTO #temp VALUES( 'Name', NULL, 'Lastname' );  
SELECT CONCAT( emp_name, emp_middlename, emp_lastname ) AS Result  
FROM #temp;  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
------------------  
NameLastname  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [String Functions &#40;Transact-SQL&#41; (Zeichenfolgenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/string-functions-transact-sql.md)  
  


