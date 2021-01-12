---
description: Vergleichsoperatoren (Transact-SQL)
title: Vergleichsoperatoren (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], testing
- operators [Transact-SQL], comparison
- testing expressions
- Boolean data type
- Boolean expressions
- comparing expressions
- comparison operators [SQL Server]
ms.assetid: b0cc68ef-3029-484c-a917-0c15dcbc230d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51d1afb08dc3ea9bd40a968aa76c2b64e06989f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092176"
---
# <a name="comparison-operators-transact-sql"></a>Vergleichsoperatoren (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vergleichsoperatoren testen, ob zwei Ausdrücke gleichwertig sind. Vergleichsoperatoren können für alle Ausdrücke angewendet werden, außer für Ausdrücke der Datentypen **text**, **ntext** oder **image**. In der folgenden Tabelle werden die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vergleichsoperatoren aufgelistet.  
  
|Operator|Bedeutung|  
|--------------|-------------|  
|[= (Ist gleich)](../../t-sql/language-elements/equals-transact-sql.md)|Gleich|  
|[> (Greater Than) (> (Größer als))](../../t-sql/language-elements/greater-than-transact-sql.md)|Größer als|  
|[< (Less Than) (< (Kleiner als))](../../t-sql/language-elements/less-than-transact-sql.md)|Kleiner als|  
|[>= (Greater Than or Equal To) (>= (Größer als oder gleich)](../../t-sql/language-elements/greater-than-or-equal-to-transact-sql.md)|Größer als oder gleich|  
|[<= (Less Than or Equal To) (<= (Kleiner als oder gleich))](../../t-sql/language-elements/less-than-or-equal-to-transact-sql.md)|Kleiner als oder gleich|  
|[<> (Not Equal To) (<> (Ungleich))](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)|Ungleich|  
|[\!= (Ungleich)](../../t-sql/language-elements/not-equal-to-transact-sql-exclamation.md)|Nicht gleich (kein ISO-Standard)|  
|[\!< (Nicht kleiner als)](../../t-sql/language-elements/not-less-than-transact-sql.md)|Nicht kleiner als (kein ISO-Standard)|  
|[\!> (Nicht größer als)](../../t-sql/language-elements/not-greater-than-transact-sql.md)|Nicht größer als (kein ISO-Standard)|  
  
## <a name="boolean-data-type"></a>Boolean-Datentyp  
 Das Ergebnis eines Vergleichsoperators weist den Datentyp **Boolean** auf. Es kann drei Werte annehmen: TRUE, FALSE und UNKNOWN. Ausdrücke, die einen Wert vom Datentyp **Boolean** zurückgeben, werden auch als boolesche Ausdrücke bezeichnet.  
  
 Ein **Boolean**-Datentyp kann nicht wie die anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentypen als Datentyp für eine Tabellenspalte oder eine Variable angegeben werden. Boolesche Werte können auch nicht in einem Resultset zurückgegeben werden.  
  
 Wenn SET ANSI_NULLS auf ON festgelegt ist, gibt ein Operator mit einem oder zwei NULL-Ausdrücken UNKNOWN zurück. Wenn SET ANSI_NULLS auf OFF festgelegt ist, gelten die gleichen Regeln mit Ausnahme der Operatoren gleich (=) und ungleich (<>). Wenn SET ANSI_NULLS auf OFF festgelegt ist, behandeln diese Operatoren NULL als bekannten Wert, der jedem anderen NULL entspricht, und geben entweder TRUE oder FALSE zurück (aber nie UNKNOWN).  
  
 Ausdrücke mit **Boolean**-Datentypen werden in drt WHERE-Klausel zum Filtern von Zeilen verwendet, die bestimmten Suchbedingungen entsprechen. Außerdem werden sie in den Sprachkonstrukten zur Ablaufsteuerung, wie IF und WHILE, verwendet. Beispiel:  
  
```syntaxsql  
-- Uses AdventureWorks  
  
DECLARE @MyProduct INT;  
SET @MyProduct = 750;  
IF (@MyProduct <> 0)  
   SELECT ProductID, Name, ProductNumber  
   FROM Production.Product  
   WHERE ProductID = @MyProduct;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
