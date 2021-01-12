---
title: IF...ELSE (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Sprachreferenz für IF-ELSE-Anweisungen zur Bereitstellung der Ablaufsteuerung in Transact-SQL-Anweisungen
ms.date: 07/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IF_TSQL
- IF
dev_langs:
- TSQL
helpviewer_keywords:
- IF...ELSE keyword
- ELSE (IF...ELSE) keyword
- ELSE keyword
- IF keyword
ms.assetid: 676c881f-dee1-417a-bc51-55da62398e81
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c8e21beb67e0ed283f463fff629776f33c1b07a0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98085357"
---
# <a name="ifelse-transact-sql"></a>IF...ELSE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Legt Bedingungen für die Ausführung einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung fest. Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung nach dem IF-Schlüsselwort und der Bedingung wird nur ausgeführt, wenn die Bedingung erfüllt ist. Dies ist der Fall, wenn der boolesche Ausdruck TRUE zurückgibt. Das optionale ELSE-Schlüsselwort führt eine alternative [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ein, die ausgeführt wird, wenn die IF-Bedingung nicht erfüllt ist. Dies ist der Fall., wenn der boolesche Ausdruck FALSE zurückgibt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
IF Boolean_expression   
     { sql_statement | statement_block }   
[ ELSE   
     { sql_statement | statement_block } ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *Boolean_expression*  
 Ein Ausdruck, der TRUE oder FALSE zurückgibt. Wenn der boolesche Ausdruck eine SELECT-Anweisung enthält, muss die SELECT-Anweisung in Klammern eingeschlossen werden.  
  
 { *sql_statement*| *statement_block* }  
 Eine beliebige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung oder -Anweisungsgruppe, die mithilfe eines Anweisungsblockes definiert wurde. Wenn kein Anweisungsblock angegeben wurde, steuert die IF- oder ELSE-Bedingung nur die Ausführung einer einzelnen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung.  
  
 Um einen Anweisungsblock zu definieren, verwenden Sie die Schlüsselwörter zur Ablaufsteuerung BEGIN und END.  
  
## <a name="remarks"></a>Bemerkungen  
 Ein IF...ELSE-Konstrukt kann in Batches, gespeicherten Prozeduren und Ad-hoc-Abfragen verwendet werden. Wenn dieses Konstrukt in einer gespeicherten Prozedur verwendet wird, wird damit das Vorhandensein bestimmter Parameter getestet.  
  
 IF-Tests können nach einem anderen IF oder auf ein ELSE folgend geschachtelt werden. Das Limit für die Anzahl geschachtelter Ebenen hängt vom verfügbaren Arbeitsspeicher ab.  
  
## <a name="example"></a>Beispiel  
  
```sql
IF DATENAME(weekday, GETDATE()) IN (N'Saturday', N'Sunday')
       SELECT 'Weekend';
ELSE 
       SELECT 'Weekday';
```  
  
 Weitere Beispiele finden Sie unter [ELSE &#40;IF...ELSE&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/else-if-else-transact-sql.md).  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Im folgenden Beispiel wird `IF...ELSE` verwendet, um basierend auf der Gewichtung eines Elements in der `DimProduct`-Tabelle festzulegen, welche Antwort von zwei Antworten dem Benutzer angezeigt wird.  
  
```sql
-- Uses AdventureWorksDW  

DECLARE @maxWeight FLOAT, @productKey INTEGER  
SET @maxWeight = 100.00  
SET @productKey = 424  
IF @maxWeight <= (SELECT Weight from DimProduct WHERE ProductKey = @productKey)   
    SELECT @productKey AS ProductKey, EnglishDescription, Weight, 'This product is too heavy to ship and is only available for pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey
ELSE  
    SELECT @productKey AS ProductKey, EnglishDescription, Weight, 'This product is available for shipping or pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [END &#40;BEGIN...END&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/end-begin-end-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Sprachkonstrukte zur Ablaufsteuerung &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)[ELSE &#40;IF...ELSE&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/else-if-else-transact-sql.md) 
