---
description: BETWEEN (Transact-SQL)
title: BETWEEN (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BETWEEN
- BETWEEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- inclusive ranges
- testing range
- exclusive ranges
- NOT BETWEEN operator
- BETWEEN operator
- range to test [SQL Server]
ms.assetid: a5d5b050-203e-4355-ac85-e08ef5ca7823
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d41c356b20dd0bb0577f10ee0abc47681ed38e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096010"
---
# <a name="between-transact-sql"></a>BETWEEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt einen zu testenden Bereich an.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
test_expression [ NOT ] BETWEEN begin_expression AND end_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *test_expression*  
 Ist der [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md), auf den hin in dem Bereich getestet werden soll, von *begin_expression* und *end_expression* definiert, muss *test_expression* denselben Datentyp wie *begin_expression* und *end_expression* aufweisen.  
  
 NICHT  
 Gibt an, dass das Ergebnis des Prädikats negiert wird.  
  
 *begin_expression*  
 Ist ein beliebiger gültiger Ausdruck. *begin_expression* muss denselben Datentyp wie *test_expression* und *end_expression* aufweisen.  
  
 *end_expression*  
 Ist ein beliebiger gültiger Ausdruck. *end_expression* muss denselben Datentyp wie *test_expression* und *begin_expression* aufweisen.  
  
 UND  
 Fungiert als Platzhalter, der angibt, dass sich *test_expression* innerhalb des von *begin_expression* und *end_expression* angegebenen Bereichs befinden sollte.  
  
## <a name="result-types"></a>Ergebnistypen  
 **Boolescher Wert**  
  
## <a name="result-value"></a>Ergebniswert  
 BETWEEN gibt **TRUE** zurück, wenn der Wert von *test_expression* größer oder gleich dem Wert von *begin_expression* und kleiner oder gleich dem Wert von *end_expression* ist.  
  
 NOT BETWEEN gibt **TRUE** zurück, wenn der Wert von *test_expression* kleiner als der Wert von *begin_expression* und größer als der Wert von *end_expression* ist.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die Operatoren Größer-als (>) und Kleiner-als (<), um einen Exklusivbereich anzugeben. Ist einer der Eingabewerte für das BETWEEN- oder NOT BETWEEN-Prädikat NULL, lautet das Ergebnis UNKNOWN.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-between"></a>A. Verwenden von BETWEEN  
 Im folgenden Beispiel werden Informationen zu den Datenbankgruppen in einer Datenbank zurückgegeben. Die erste Abfrage gibt alle Rollen zurück. Im zweiten Beispiel werden die Rollen mithilfe der `BETWEEN`-Klausel auf die angegebenen `database_id` Werte beschränkt.  
  
```sql  
SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R';

SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R'
AND principal_id BETWEEN 16385 AND 16390;
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
principal_id    name
------------  ---- 
0               public
16384           db_owner
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
16391           db_datawriter
16392           db_denydatareader
16393           db_denydatawriter
```  
```  
principal_id    name
------------  ---- 
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
```  
  
### <a name="b-using--and--instead-of-between"></a>B. Verwenden von > und < anstelle von BETWEEN  
 Im folgenden Beispiel werden die Operatoren Größer-als (`>`) und Kleiner-als (`<`) verwendet. Da diese Operatoren die Bereichsgrenzen nicht einschließen, werden im Unterschied zu den zehn Zeilen des vorherigen Beispiels nur neun Zeilen zurückgegeben.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate > 27 AND ep.Rate < 30  
ORDER BY ep.Rate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 FirstName   LastName             Rate  
 ---------   -------------------  ---------  
 Paula       Barreto de Mattos    27.1394  
 Janaina     Bueno                27.4038  
 Dan         Bacon                27.4038  
 Ramesh      Meyyappan            27.4038  
 Karen       Berg                 27.4038  
 David       Bradley              28.7500  
 Hazem       Abolrous             28.8462  
 Ovidiu      Cracium              28.8462  
 Rob         Walters              29.8462  
 ```    
  
### <a name="c-using-not-between"></a>C. Verwenden von NOT BETWEEN  
 Im folgenden Beispiel werden alle Zeilen gesucht, die außerhalb des angegebenen Bereiches von `27` bis `30` liegen.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate NOT BETWEEN 27 AND 30  
ORDER BY ep.Rate;  
GO  
```  
  
### <a name="d-using-between-with-datetime-values"></a>D: Verwenden von BETWEEN mit datetime-Werten  
 Im folgenden Beispiel werden Zeilen abgerufen, die **datetime**-Werte zwischen `'20011212'` und `'20020105'` (inklusiv) enthalten.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, RateChangeDate  
FROM HumanResources.EmployeePayHistory  
WHERE RateChangeDate BETWEEN '20011212' AND '20020105';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 BusinessEntityID RateChangeDate  
 ----------- -----------------------  
 3           2001-12-12 00:00:00.000  
 4           2002-01-05 00:00:00.000  
 ```  
 
 Durch die Abfrage werden die erwarteten Zeilen abgerufen, da die Datumswerte in der Abfrage und die **datetime**-Werte in der `RateChangeDate`-Spalte ohne den Zeitteil des Datums angegeben wurden. Wenn der Zeitteil nicht angegeben wird, wird standardmäßig 0:00 Uhr verwendet. Beachten Sie, dass eine Zeile mit einen Zeitteil, der nach 0:00 Uhr am Datum 2002-01-05 liegt, von dieser Abfrage nicht zurückgegeben wird, da das Datum außerhalb des Bereichs liegt.  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [&#62; &#40;Greater Than&#41; &#40;Transact-SQL&#41; (&#62; &#40;Größer als&#41; &#40;Transact-SQL&#41;)](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [&#60; &#40;Less Than&#41; &#40;Transact-SQL&#41; (&#60; &#40;Kleiner als&#41; &#40;Transact-SQL&#41;)](../../t-sql/language-elements/less-than-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  


