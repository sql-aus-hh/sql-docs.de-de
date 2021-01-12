---
description: SET STATISTICS TIME (Transact-SQL)
title: SET STATISTICS TIME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_TIME_TSQL
- SET STATISTICS TIME
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- time [SQL Server], statement processing statistics
- SET STATISTICS TIME statement
- STATISTICS TIME option
- statements [SQL Server], statistical information
- parsing [SQL Server], SET STATISTICS TIME statement
- compile times [SQL Server]
- execution processing time [SQL Server]
ms.assetid: eec2e1cd-a29d-4cf3-a271-be9d61506f15
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 36db16f45f3f73742c89b6269fedc5c4603e648f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096903"
---
# <a name="set-statistics-time-transact-sql"></a>SET STATISTICS TIME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Zeigt an, wie viele Millisekunden zum Analysieren, Kompilieren und Ausführen jeder Anweisung benötigt wurden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET STATISTICS TIME { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Hinweise
 Wenn SET STATISTICS TIME auf ON festgelegt ist, wird die Zeitstatistik für eine Anweisung angezeigt. Bei OFF wird die Zeitstatistik nicht angezeigt.  
  
 Die Einstellung von SET STATISTICS TIME wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.  
  
 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist nicht in der Lage, im Fibermodus exakte Statistiken zu erzeugen. Dieser Modus wird aktiviert, wenn Sie die Konfigurationsoption **Lightweightpooling** aktivieren.  
  
 Die **cpu**-Spalte in der **sysprocesses**-Tabelle wird nur dann aktualisiert, wenn eine Abfrage mit SET STATISTICS TIME ON ausgeführt wird. Wenn SET STATISTICS TIME den Wert OFF hat, wird der Wert **0** zurückgegeben.  
  
 Die Einstellungen ON und OFF wirken sich auch auf die CPU-Spalte in der Prozessinfo-Sicht für den Verwaltungsordner Aktuelle Aktivität in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aus.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Verwenden von SET STATISTICS TIME müssen Benutzer über entsprechende Berechtigungen zum Ausführen der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung verfügen. Die SHOWPLAN-Berechtigung ist nicht erforderlich.  
  
## <a name="examples"></a>Beispiele  
 In diesem Beispiel werden die Serverausführungs-, Analyse- und Kompilierzeit gezeigt.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS TIME ON;  
GO  
SELECT ProductID, StartDate, EndDate, StandardCost   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS TIME OFF;  
GO  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
```  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
  
(269 row(s) affected)  
  
SQL Server Execution Times:  
   CPU time = 0 ms,  elapsed time = 2 ms.  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET STATISTICS IO &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-io-transact-sql.md)  
  
  
