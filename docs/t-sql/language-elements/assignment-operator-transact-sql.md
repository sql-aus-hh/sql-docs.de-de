---
description: = (Zuweisungsoperator) (Transact-SQL)
title: = (Zuweisungsoperator) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], assignment
- assignment operators [Transact-SQL]
- headings [SQL Server columns]
- relationships [SQL Server], assignment operators
- column headings [SQL Server]
ms.assetid: c3040db6-21d6-40ac-a783-82c98ec006cc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9b68e1d58926751166820dfe1d2c697efa6d6f7e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100928"
---
# <a name="-assignment-operator-transact-sql"></a>= (Zuweisungsoperator) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all_md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  Das Gleichheitszeichen (=) ist der einzige Zuweisungsoperator von [!INCLUDE[tsql](../../includes/tsql-md.md)]. Im folgenden Beispiel wird die `@MyCounter`-Variable erstellt. Anschließend legt der Zuweisungsoperator `@MyCounter` auf einen Wert fest, der von einem Ausdruck zurückgegeben wird.  
  
```sql  
DECLARE @MyCounter INT;  
SET @MyCounter = 1;  
```  
  
 Mit dem Zuweisungsoperator kann auch eine Beziehung zwischen einer Spaltenüberschrift und dem Ausdruck hergestellt werden, der die Werte für die Spalte definiert. Im folgenden Beispiel werden die Spaltenüberschriften `FirstColumnHeading` und `SecondColumnHeading` angezeigt. Die Zeichenfolge `xyz` wird unter der Spaltenüberschrift `FirstColumnHeading` für alle Zeilen angezeigt. Jede Produkt-ID aus der `Product`-Tabelle wird unter der Spaltenüberschrift `SecondColumnHeading` aufgelistet.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstColumnHeading = 'xyz',  
       SecondColumnHeading = ProductID  
FROM Production.Product;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
