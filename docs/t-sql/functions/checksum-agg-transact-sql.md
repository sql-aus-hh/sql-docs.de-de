---
description: CHECKSUM_AGG (Transact-SQL)
title: CHECKSUM_AGG (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKSUM_AGG
- CHECKSUM_AGG_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- checksum values
- CHECKSUM_AGG function
- groups [SQL Server], checksum values
ms.assetid: cdede70c-4eb5-4c92-98ab-b07787ab7222
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2bea0aad7839bc0b487b77b6ac1434396b84b765
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102633"
---
# <a name="checksum_agg-transact-sql"></a>CHECKSUM_AGG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt die Prüfsumme der Werte in einer Gruppe zurück. `CHECKSUM_AGG` ignoriert NULL-Werte. Die [OVER-Klausel](../../t-sql/queries/select-over-clause-transact-sql.md) kann `CHECKSUM_AGG` folgen.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CHECKSUM_AGG ( [ ALL | DISTINCT ] expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
**ALL**  
Wendet die Aggregatfunktion auf alle Werte an. ALL ist das Standardargument.
  
DISTINCT  
Gibt an, dass `CHECKSUM_AGG` die Prüfsumme von eindeutigen Werten zurückgeben soll.
  
*expression*  
Ein [Integerausdruck](../../t-sql/language-elements/expressions-transact-sql.md). `CHECKSUM_AGG` lässt das Verwenden von Aggregatfunktionen und Unterabfragen nicht zu.
  
## <a name="return-types"></a>Rückgabetypen
Gibt die Prüfsumme aller *Ausdruckswerte* als **int** zurück.
  
## <a name="remarks"></a>Hinweise  
`CHECKSUM_AGG` kann Änderungen in einer Tabelle erkennen.
  
Das `CHECKSUM_AGG`-Ergebnis hängt nicht von der Reihenfolge der Zeilen in der Tabelle ab. Zudem lassen `CHECKSUM_AGG`-Funktionen das Verwenden des Schlüsselworts `DISTINCT` und der Klausel `GROUP BY` zu.
  
Wenn der Listenwert eines Ausdrucks geändert wird, ist es wahrscheinlich, dass sich auch die Werteliste der Listenprüfsumme verändert. Es besteht jedoch auch die Möglichkeit, dass sich die berechnete Prüfsumme nicht ändert.
  
`CHECKSUM_AGG` verfügt über Funktionen, die denen von Aggregatfunktionen ähneln. Weitere Informationen finden Sie unter [Aggregatfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md).
  
## <a name="examples"></a>Beispiele  
In den folgenden Beispielen wird `CHECKSUM_AGG` verwendet, um Änderungen in der `Quantity`-Spalte der `ProductInventory`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zu erkennen.
  
```sql
--Get the checksum value before the column value is changed.  

SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
262  
```  
  
```sql
UPDATE Production.ProductInventory   
SET Quantity=125  
WHERE Quantity=100;  
GO  

--Get the checksum of the modified column.  
SELECT CHECKSUM_AGG(CAST(Quantity AS INT))  
FROM Production.ProductInventory;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
------------------------  
287  
```  
  
## <a name="see-also"></a>Siehe auch
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
[OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
