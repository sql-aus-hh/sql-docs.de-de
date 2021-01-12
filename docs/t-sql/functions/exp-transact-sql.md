---
description: EXP (Transact-SQL)
title: EXP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EXP_TSQL
- EXP
dev_langs:
- TSQL
helpviewer_keywords:
- exponential functions
- EXP function
ms.assetid: 5a9b8c52-6fb6-4e33-8b02-a878785b2f51
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3c9429ab47de08584fbc995fa8dfa9c8182ee40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101214"
---
# <a name="exp-transact-sql"></a>EXP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt den exponentiellen Wert des angegebenen **float**-Ausdrucks zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
EXP ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *float_expression*  
 Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) vom Typ **float** oder von einem Typ, der implizit in **float** konvertiert werden kann.  
  
## <a name="return-types"></a>Rückgabetypen  
 **float**  
  
## <a name="remarks"></a>Hinweise  
 Die Konstante **e** (2,718281…) ist die Basis natürlicher Logarithmen.  
  
 Der Exponent einer Zahl ist die Konstante **e** potenziert mit der Zahl. Beispielsweise EXP(1,0) = e^1,0 = 2,71828182845905 und EXP(10) = e^10 = 22026,4657948067.  
  
 Der natürliche Logarithmus des exponentiellen Werts einer Zahl ist die Zahl selbst: EXP (LOG (*n*)) = *n*. Und der natürliche Logarithmus des exponentiellen Werts einer Zahl ist die Zahl selbst: LOG (EXP (*n*)) = *n*.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-finding-the-exponent-of-a-number"></a>A. Suchen des Exponenten einer Zahl  
 Im folgenden Beispiel wird eine Variable deklariert und der exponentielle Wert der angegebenen Variablen (`10`) mit einer Textbeschreibung zurückgegeben.  
  
```sql  
DECLARE @var FLOAT  
SET @var = 10  
SELECT 'The EXP of the variable is: ' + CONVERT(VARCHAR, EXP(@var))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------------------------------------------------  
The EXP of the variable is: 22026.5  
(1 row(s) affected)  
```  
  
### <a name="b-finding-exponentials-and-natural-logarithms"></a>B. Suchen von Exponentialgrößen und natürlichen Logarithmen  
 Im folgenden Beispiel werden der exponentielle Wert des natürlichen Logarithmus von `20` sowie der natürliche Logarithmus des exponentiellen Werts von `20` zurückgegeben. Bei diesen Funktionen handelt es sich um Umkehrfunktionen anderer Funktionen. Deshalb ist der Rückgabewert in beiden Fällen `20`.  
  
```sql  
SELECT EXP(LOG(20)), LOG(EXP(20))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------- ----------------------  
20                     20  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-finding-the-exponent-of-a-number"></a>C. Suchen des Exponenten einer Zahl  
 Im folgenden Beispiel wird der exponentielle Wert des angegebenen Werts (`10`) zurückgegeben.  
  
```sql  
SELECT EXP(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------  
22026.4657948067  
```  
  
### <a name="d-finding-exponential-values-and-natural-logarithms"></a>D: Suchen von exponentiellen Werten und natürlichen Logarithmen  
 Im folgenden Beispiel werden der exponentielle Wert des natürlichen Logarithmus von `20` sowie der natürliche Logarithmus des exponentiellen Werts von `20` zurückgegeben. Bei diesen Funktionen handelt es sich um Umkehrfunktionen anderer Funktionen. Deshalb ist der Rückgabewert in beiden Fällen `20`.  
  
```sql  
SELECT EXP( LOG(20)), LOG( EXP(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------- -----------------  
20                  20  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Mathematische Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [LOG &#40;Transact-SQL&#41;](../../t-sql/functions/log-transact-sql.md)   
 [LOG10 &#40;Transact-SQL&#41;](../../t-sql/functions/log10-transact-sql.md)  
  
  

