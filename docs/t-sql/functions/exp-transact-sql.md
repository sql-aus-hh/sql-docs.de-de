---
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
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7be8b0b19fea7373a9df50f4f7f05d6dc5ee6989
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47678148"
---
# <a name="exp-transact-sql"></a>EXP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Gibt den exponentiellen Wert des angegebenen **float**-Ausdrucks zurück.  
  
 ![Themenlinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions (Transact-SQL-Syntaxkonventionen)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
EXP ( float_expression )  
```  
  
## <a name="arguments"></a>Argumente  
 *float_expression*  
 Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) vom Typ **float** oder von einem Typ, der implizit in **float** konvertiert werden kann.  
  
## <a name="return-types"></a>Rückgabetypen  
 **float**  
  
## <a name="remarks"></a>Remarks  
 Die Konstante **e** (2.718281…) ist die Basis von natürlichen Logarithmen.  
  
 Der Exponent einer Zahl ist die Konstante **e** potenziert mit der Zahl. Beispielsweise EXP(1,0) = e^1,0 = 2,71828182845905 und EXP(10) = e^10 = 22026,4657948067.  
  
 Der natürliche Logarithmus des exponentiellen Werts einer Zahl ist die Zahl selbst: EXP (LOG (*n*)) = *n*. Und der natürliche Logarithmus des exponentiellen Werts einer Zahl ist die Zahl selbst: LOG (EXP (*n*)) = *n*.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-finding-the-exponent-of-a-number"></a>A. Suchen des Exponenten einer Zahl  
 Im folgenden Beispiel wird eine Variable deklariert und der exponentielle Wert der angegebenen Variablen (`10`) mit einer Textbeschreibung zurückgegeben.  
  
```  
DECLARE @var float  
SET @var = 10  
SELECT 'The EXP of the variable is: ' + CONVERT(varchar,EXP(@var))  
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
  
```  
SELECT EXP( LOG(20)), LOG( EXP(20))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------- ----------------------  
20                     20  
  
(1 row(s) affected)  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
### <a name="c-finding-the-exponent-of-a-number"></a>C. Suchen des Exponenten einer Zahl  
 Im folgenden Beispiel wird der exponentielle Wert des angegebenen Werts (`10`) zurückgegeben.  
  
```  
SELECT EXP(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------  
22026.4657948067  
```  
  
### <a name="d-finding-exponential-values-and-natural-logarithms"></a>D. Suchen von exponentiellen Werten und natürlichen Logarithmen  
 Im folgenden Beispiel werden der exponentielle Wert des natürlichen Logarithmus von `20` sowie der natürliche Logarithmus des exponentiellen Werts von `20` zurückgegeben. Bei diesen Funktionen handelt es sich um Umkehrfunktionen anderer Funktionen. Deshalb ist der Rückgabewert in beiden Fällen `20`.  
  
```  
SELECT EXP( LOG(20)), LOG( EXP(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------- -----------------  
20                  20  
```  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
 [Mathematische Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [LOG &#40;Transact-SQL&#41;](../../t-sql/functions/log-transact-sql.md)   
 [LOG10 &#40;Transact-SQL&#41;](../../t-sql/functions/log10-transact-sql.md)  
  
  

