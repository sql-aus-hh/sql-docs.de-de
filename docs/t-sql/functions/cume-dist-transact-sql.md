---
description: CUME_DIST (Transact-SQL)
title: CUME_DIST (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CUME_DIST
- CUME_DIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CUME_DIST function
- analytic functions, CUME_DIST
ms.assetid: 491b07f3-9ffd-4cdd-93e5-5abb636fc5ef
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4252c237e234fdc380ca0f4b18cfcf55f6bf9ae4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093726"
---
# <a name="cume_dist-transact-sql"></a>CUME_DIST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berechnet diese Funktion die kumulierte Verteilung eines Werts innerhalb einer Gruppe von Werten. Das heißt, `CUME_DIST` berechnet die relative Position eines angegebenen Werts in einer Gruppe von Werten. Ausgehend von aufsteigender Sortierreihenfolge ist der `CUME_DIST`-Wert eines Werts in Zeile _r_ definiert als die Anzahl der Zeilen mit Werten kleiner oder gleich dem Wert in Zeile _r_, geteilt durch die Anzahl der ausgewerteten Zeilen in der Partition oder dem Resultset der Abfrage. `CUME_DIST` ähnelt der `PERCENT_RANK`-Funktion.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CUME_DIST( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_)  

Das Argument _partition\_by\_clause_ unterteilt das Resultset der FROM-Klausel in Partitionen, auf die die Funktion angewendet wird. Wenn das Argument _partition\_by\_clause_ nicht angegeben wird, behandelt `CUME_DIST` alle Zeilen von Resultsets von Abfragen als eine einzelne Gruppe. _order\_by\_clause_ bestimmt die logische Reihenfolge, in der der Vorgang ausgeführt wird. Für `CUME_DIST` ist _order\_by\_clause_ erforderlich. `CUME_DIST` akzeptiert nicht die \<rows or range clause> der OVER-Syntax. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).
  
## <a name="return-types"></a>Rückgabetypen
**float(53)**
  
## <a name="remarks"></a>Bemerkungen  
`CUME_DIST` gibt einen Bereich von Werten größer als 0 und kleiner als oder gleich 1 zurück. Gleichwertige Werte ergeben immer den gleichen kumulierten Verteilungswert. `CUME_DIST` schließt standardmäßig NULL-Werte ein und behandelt diese Werte als die niedrigsten möglichen Werte.
  
`CUME_DIST` ist nicht deterministisch. Weitere Informationen finden Sie unter [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="examples"></a>Beispiele  
Das folgende Beispiel verwendet die `CUME_DIST`-Funktion, um das Gehaltsquantil für jeden Mitarbeiter innerhalb einer angegebenen Abteilung zu berechnen. Die `CUME_DIST`-Funktion gibt einen Wert zurück, der den Prozentsatz der Mitarbeiter darstellt, deren Gehalt kleiner oder gleich dem des aktuellen Mitarbeiters in der gleichen Abteilung ist. Die `PERCENT_RANK`-Funktion berechnet den prozentualen Rang des Gehalts des Mitarbeiters innerhalb einer Abteilung. Um die Zeilen im Resultset nach Abteilung zu partitionieren, gibt das Beispiel den _partition\_by\_clause_-Wert an. Die ORDER BY-Klausel der OVER-Klausel sortiert die Zeilen in jeder Partition logisch. Die ORDER BY-Klausel der SELECT-Anweisung bestimmt die Anzeigereihenfolge des Resultsets.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen
[PERCENT_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/percent-rank-transact-sql.md)
  
  
