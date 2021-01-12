---
description: ASIN (Transact-SQL)
title: ASIN (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ASIN_TSQL
- ASIN
dev_langs:
- TSQL
helpviewer_keywords:
- ASIN function
- sine
- arcsine
ms.assetid: 6256dd7d-83d5-486e-a933-1d59afc7e417
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f07936f49e0142f9851569112c7eab6e2037c5b0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98089373"
---
# <a name="asin-transact-sql"></a>ASIN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Eine Funktion, die den Winkel im Bogenmaß zurück gibt, dessen Sinus dem angegebenen **float**-Ausdruck entspricht. Dies wird auch als **Arkussinus** bezeichnet.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ASIN ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*float_expression*  
Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) vom Typ **float** oder von einem Typ, der implizit in float konvertiert werden kann. Nur ein Wert von -1,00 bis 1,00 ist gültig. Bei Werten außerhalb dieses Bereichs wird NULL zurückgegeben und ASIN meldet einen Domänenfehler.
  
## <a name="return-types"></a>Rückgabetypen
**float**
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird ein **float**-Ausdruck verwendet und der ASIN-Wert des angegebenen Winkels zurückgegeben.
  
```sql
/* The first value will be -1.01. This fails because the value is   
outside the range.*/  
DECLARE @angle FLOAT  
SET @angle = -1.01  
SELECT 'The ASIN of the angle is: ' + CONVERT(VARCHAR, ASIN(@angle))  
GO  
  
-- The next value is -1.00.  
DECLARE @angle FLOAT  
SET @angle = -1.00  
SELECT 'The ASIN of the angle is: ' + CONVERT(VARCHAR, ASIN(@angle))  
GO  
  
-- The next value is 0.1472738.  
DECLARE @angle FLOAT  
SET @angle = 0.1472738  
SELECT 'The ASIN of the angle is: ' + CONVERT(VARCHAR, ASIN(@angle))  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-------------------------  
.Net SqlClient Data Provider: Msg 3622, Level 16, State 1, Line 3  
A domain error occurred.  
  
---------------------------------   
The ASIN of the angle is: -1.5708                          
  
(1 row(s) affected)  
  
----------------------------------   
The ASIN of the angle is: 0.147811                         
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
In diesem Beispiel wird ein Arkussinus von 1,00 zurückgegeben.
  
```sql
SELECT ASIN(1.00) AS asinCalc;  
```  
  
In diesem Beispiel wird ein Fehler zurückgegeben, da der Arkussinus für einen Wert außerhalb des zulässigen Bereichs erforderlich ist.
  
```sql
SELECT ASIN(1.1472738) AS asinCalc;  
```  
  
## <a name="see-also"></a>Weitere Informationen
[CEILING &#40;Transact-SQL&#41;](../../t-sql/functions/ceiling-transact-sql.md)  
[Mathematische Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  
[SET ARITHIGNORE &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithignore-transact-sql.md)  
[SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md)
  
  

