---
description: + (Unäres Plus) (Transact-SQL)
title: + (Unäres Plus) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- + (positive)
- +
- positive
dev_langs:
- TSQL
helpviewer_keywords:
- + (positive operator)
- positive operator (+)
- positive values [SQL Server]
ms.assetid: 0f31c5cc-3078-4f6a-9870-7eb1a98053fb
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 70dae64868be97cf243a788075a1b84c37872b31
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095902"
---
# <a name="unary-operators---positive"></a>Unäre Operatoren: Positiv
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Gibt den Wert eines numerischen Ausdrucks zurück (ein unärer Operator). Unäre Operatoren führen eine Operation mit nur einem Ausdruck eines beliebigen Datentyps der numerischen Datentypkategorie aus.   
  
|Operator|Bedeutung|  
|--------------|-------------|  
|[+ (Positive) (+ (Positiv))](../../t-sql/language-elements/unary-operators-positive.md)|Numerischer Wert ist positiv.|  
|[- (Negativ)](../../t-sql/language-elements/unary-operators-negative.md)|Numerischer Wert ist negativ.|  
|[~ (Bitweises NOT)](../../t-sql/language-elements/bitwise-not-transact-sql.md)|Gibt das Einerkomplement der Zahl zurück.|  
  
 Die Operatoren + (Positiv) und - (Negativ) können für einen beliebigen Ausdruck eines jeden Datentyps der numerischen Datentypkategorie verwendet werden. Der Operator ~ (Bitweises NOT) kann nur für Ausdrücke eines Datentyps der ganzzahligen Datentypkategorie verwendet werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
+ numeric_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *numeric_expression*  
 Jeder gültige [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) eines Datentyps der numerischen Datentypkategorie. Gilt allerdings nicht für die Datentypen **datetime** und **smalldatetime**.  
  
## <a name="result-types"></a>Ergebnistypen  
 Gibt den Datentyp von *numeric_expression* zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Obwohl ein unäres Plus vor jedem numerischen Ausdruck angezeigt werden kann, führt es keinen Vorgang mit dem Wert aus, der von dem Ausdruck zurückgegeben wird. Insbesondere gibt es nicht den positiven Wert eines negativen Ausdrucks zurück. Verwenden Sie zum Zurückgeben des positiven Werts eines negativen Ausdrucks die [ABS](../../t-sql/functions/abs-transact-sql.md)-Funktion.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-setting-a-variable-to-a-positive-value"></a>A. Festlegen einer Variablen auf einen positiven Wert  
 Im folgenden Beispiel wird eine Variable auf einen positiven Wert festgelegt.  
  
```sql  
DECLARE @MyNumber DECIMAL(10,2);  
SET @MyNumber = +123.45;  
SELECT @MyNumber;  
GO  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
-----------   
123.45            
  
(1 row(s) affected)  
```  
  
### <a name="b-using-the-unary-plus-operator-with-a-negative-value"></a>B. Verwenden des unären Plus-Operators mit einem negativen Wert  
 Im folgenden Beispiel wird die Verwendung des unären Plus-Operators mit einem negativen Ausdruck und der ABS()-Funktion mit dem gleichen negativen Ausdruck gezeigt. Das unäre Plus hat keine Auswirkungen auf den Ausdruck, die ABS()-Funktion gibt jedoch den positiven Wert des Ausdrucks zurück.  
  
```sql  
USE tempdb;  
GO  
DECLARE @Num1 INT;  
SET @Num1 = -5;  
SELECT +@Num1, ABS(@Num1);  
GO  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
----------- -----------  
-5          5  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [ABS &#40;Transact-SQL&#41;](../../t-sql/functions/abs-transact-sql.md)  
  
  
