---
description: ERROR_NUMBER (Transact-SQL)
title: ERROR_NUMBER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ERROR_NUMBER_TSQL
- ERROR_NUMBER
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], numbers
- TRY...CATCH [SQL Server]
- ERROR_NUMBER function
- CATCH block
ms.assetid: 1de85fff-1ca2-4b31-841b-926e571cb150
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 07f9d4783cb864601d6eb53ebbe9e35094d9ac89
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093629"
---
# <a name="error_number-transact-sql"></a>ERROR_NUMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt die Fehlernummer des Fehlers zurück, der die Ausführung des CATCH-Blocks eines TRY...CATCH-Konstrukts ausgelöst hat.  

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
ERROR_NUMBER ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 **int**  
  
## <a name="return-value"></a>Rückgabewert  
Wenn `ERROR_NUMBER` in einem CATCH-Block aufgerufen wird, wird die Fehlernummer des Fehlers zurückgegeben, der die Ausführung des CATCH-Blocks ausgelöst hat.  

`ERROR_NUMBER` gibt NULL zurück, wenn die Funktion außerhalb des Bereichs eines CATCH-Blocks aufgerufen wird.  
  
## <a name="remarks"></a>Bemerkungen  
`ERROR_NUMBER` kann überall im Bereich eines CATCH-Blocks aufgerufen werden.  
  
`ERROR_NUMBER` gibt unabhängig von der Anzahl der Aufrufe und dem Bereich des `CATCH`-Blocks eine relevante Fehlernummer zurück. Dies steht im Gegensatz zu Funktionen wie @@ERROR, die nur eine Fehlernummer in der Anweisung zurückgeben, die unmittelbar auf die Anweisung folgt, die einen Fehler auslöst.  

`ERROR_NUMBER` gibt in einem geschachtelten `CATCH`-Block die Fehlernummer für den entsprechenden Bereich des `CATCH`-Blocks zurück, der auf den `CATCH`-Block verwiesen hat. Zum Beispiel könnte der `CATCH`-Block eines äußeren TRY...CATCH-Konstrukts ein inneres `TRY...CATCH`-Konstrukt aufweisen. In diesem inneren `CATCH`-Block gibt `ERROR_NUMBER` die Nummer des Fehlers zurück, der den inneren `CATCH`-Block aufgerufen hat. Wenn `ERROR_NUMBER` im äußeren `CATCH`-Block ausgeführt wird, wird die Nummer des Fehlers zurückgegeben, der den äußeren `CATCH`-Block aufgerufen hat.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-error_number-in-a-catch-block"></a>A. Verwenden von ERROR_NUMBER in einem CATCH-Block  
Das folgende Beispiel zeigt eine `SELECT`-Anweisung, die einen Fehler aufgrund einer Division durch 0 (null) generiert. Der `CATCH`-Block gibt die Fehlernummer zurück.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_NUMBER() AS ErrorNumber;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorNumber
-----------
8134

(1 row(s) affected)

```  
  
### <a name="b-using-error_number-in-a-catch-block-with-other-error-handling-tools"></a>B. Verwenden von ERROR_NUMBER in einem CATCH-Block mit anderen Fehlerbehandlungstools  
Das folgende Beispiel zeigt eine `SELECT`-Anweisung, die einen Fehler aufgrund einer Division durch 0 (null) generiert. Der `CATCH`-Block gibt zusammen mir der Fehlernummer Informationen zum Fehler zurück.  

```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure   ErrorLine  ErrorMessage
----------- ------------- ----------- ---------------  ---------- ----------------------------------
8134        16            1           NULL             4          Divide by zero error encountered.

(1 row(s) affected)

```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)     
 [Fehler- und Ereignisreferenz &#40;Datenbank-Engine&#41;](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
  

