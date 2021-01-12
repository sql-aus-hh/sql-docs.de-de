---
description: ERROR_LINE (Transact-SQL)
title: ERROR_LINE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ERROR_LINE
- ERROR_LINE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], line number
- TRY...CATCH [SQL Server]
- line number of error [SQL Server]
- ERROR_LINE function
- CATCH block
ms.assetid: 47335734-0baf-45a6-8b3b-6c4fd80d2cb8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 44402d9f0af858381041e9f5bb5ff989971f44f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097183"
---
# <a name="error_line-transact-sql"></a>ERROR_LINE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt die Zeilennummer des Fehlers zurück, der die Ausführung des CATCH-Blocks eines TRY...CATCH-Konstrukts ausgelöst hat.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax

```syntaxsql
ERROR_LINE ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Rückgabetyp
**int**

## <a name="return-value"></a>Rückgabewert  
Wenn die Funktion in einem CATCH-Block aufgerufen wird, gibt `ERROR_LINE`  
  
-   die Nummer der Zeile zurück, in der der Fehler aufgetreten ist    
-   die Zeilennummer in einer Routine zurück, wenn der Fehler in einer gespeicherten Prozedur oder einem Trigger aufgetreten ist  
-   NULL zurück, wenn sie außerhalb des Bereichs eines CATCH-Blocks aufgerufen wurde.  
  
## <a name="remarks"></a>Bemerkungen  
Ein Aufruf von `ERROR_LINE` kann überall im Bereich eines CATCH-Blocks auftreten.  
  
`ERROR_LINE` gibt die Nummer der Zeile zurück, in der der Fehler aufgetreten ist. Dies geschieht unabhängig davon, wo `ERROR_LINE` innerhalb des Bereichs vom CATCH-Block aufgerufen wurde, und unabhängig davon, wie oft `ERROR_LINE` aufgerufen wurde. Dies steht im Gegensatz zu Funktionen wie @@ERROR. @@ERROR gibt eine Fehlernummer in der Anweisung zurück, die unmittelbar auf die folgt, die einen Fehler verursacht hat sowie in der ersten Anweisung eines CATCH-Blocks.  
  
In geschachtelten CATCH-Blöcken gibt `ERROR_LINE` die für den Bereich des CATCH-Blockes spezifische Fehlerzeilennummer zurück, auf die im Block verwiesen wird. So könnte beispielsweise der CATCH-Block eines TRY...CATCH-Konstrukts ein geschachteltes TRY...CATCH-Konstrukt enthalten. Innerhalb des geschachtelten CATCH-Blocks gibt `ERROR_LINE` die Zeilennummer des Fehlers zurück, der den geschachtelten CATCH-Block aufgerufen hat. Wenn `ERROR_LINE` im äußeren CATCH-Block ausgeführt wird, gibt es die Zeilennummer des Fehlers zurück, der den spezifischen CATCH-Block aufgerufen hat.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-error_line-in-a-catch-block"></a>A. Verwenden von ERROR_LINE in einem CATCH-Block  
In diesem Codebeispiel wird eine `SELECT`-Anweisung dargestellt, die einen Fehler aufgrund einer Division durch 0 (null) generiert. `ERROR_LINE` gibt die Nummer der Zeile zurück, in der der Fehler aufgetreten ist.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
```  
  
### <a name="b-using-error_line-in-a-catch-block-with-a-stored-procedure"></a>B. Verwenden von ERROR_LINE in einem CATCH-Block mit einer gespeicherten Prozedur  
Dieses Beispiel zeigt eine gespeicherte Prozedur, in der ein Fehler aufgrund einer Division durch 0 (null) generiert wird. `ERROR_LINE` gibt die Nummer der Zeile zurück, in der der Fehler aufgetreten ist.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that  
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
``` 


### <a name="c-using-error_line-in-a-catch-block-with-other-error-handling-tools"></a>C. Verwenden von ERROR_LINE in einem CATCH-Block mit anderen Tools zur Fehlerbehandlung  
In diesem Codebeispiel wird eine `SELECT`-Anweisung dargestellt, die einen Fehler aufgrund einer Division durch 0 (null) generiert. `ERROR_LINE` gibt die Nummer der Zeile, in der der Fehler aufgetreten ist, sowie Informationen zum Fehler selbst zurück.  
  
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
  
## <a name="see-also"></a>Weitere Informationen  
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  
