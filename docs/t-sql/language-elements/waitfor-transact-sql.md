---
description: WAITFOR (Transact-SQL)
title: WAITFOR (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WAITFOR
- WAITFOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TIME option
- delaying executions [SQL Server]
- batches [SQL Server], execution times
- stored procedures [SQL Server], executing
- DELAY
- time [SQL Server], execution delays
- blocking executions
- transactions [SQL Server], execution times
- WAITFOR statement
- timing executions
ms.assetid: 8e896e73-af27-4cae-a725-7a156733f3bd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0933c90f711f5c86425c7e2c6b4b350c47612ddc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095871"
---
# <a name="waitfor-transact-sql"></a>WAITFOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Blockiert die Ausführung eines Batches, einer gespeicherten Prozedur oder einer Transaktion, bis eine bestimmte Zeit oder ein bestimmtes Zeitintervall verstrichen ist oder bis eine angegebene Anweisung mindestens eine Zeile ändert oder zurückgibt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
WAITFOR   
{  
    DELAY 'time_to_pass'   
  | TIME 'time_to_execute'   
  | [ ( receive_statement ) | ( get_conversation_group_statement ) ]   
    [ , TIMEOUT timeout ]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 DELAY  
 Die angegebene Zeit bis maximal 24 Stunden, die verstreichen muss, bevor die Ausführung eines Batches, einer gespeicherten Prozedur oder einer Transaktion fortgesetzt wird.  
  
 '*time_to_pass*'  
 Der Zeitraum, für dessen Dauer gewartet werden soll. *time_to_pass* kann entweder im **datetime**-Format oder als lokale Variable definiert werden. Es können keine Datumsangaben gemacht werden, daher ist der Datumsteil des **datetime**-Werts nicht zulässig. *time_to_pass* wird im Format hh:mm[[:ss].mss] angezeigt.
  
 TIME  
 Die angegebene Zeit, zu der der Batch, die gespeicherte Prozedur oder die Transaktion ausgeführt wird.  
  
 '*time_to_execute*'  
 Die Zeit, zu der die WAITFOR-Anweisung beendet wird. *time_to_execute* kann im **datetime**-Format oder als lokale Variable definiert werden. Es können keine Datumsangaben gemacht werden, daher ist der Datumsteil des **datetime**-Werts nicht zulässig. *time_to_execute* wird im Format hh:mm[[:ss].mss] angezeigt und kann optional auch ein Datum im Format 1900-01-01 enthalten.
  
 *receive_statement*  
 Eine gültige RECEIVE-Anweisung.  
  
> [!IMPORTANT]  
>  Zusammen mit *receive_statement* kann WAITFOR nur für [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Meldungen verwendet werden. Weitere Informationen finden Sie unter [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md).  
  
 *get_conversation_group_statement*  
 Eine gültige GET CONVERSATION GROUP-Anweisung.  
  
> [!IMPORTANT]  
>  Zusammen mit *get_conversation_group_statement* kann WAITFOR nur für [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Meldungen verwendet werden. Weitere Informationen finden Sie unter [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
 TIMEOUT *timeout*  
 Gibt in Millisekunden den Zeitraum an, für dessen Dauer gewartet werden soll, bis eine Meldung die Warteschlange erreicht.  
  
> [!IMPORTANT]  
>  Zusammen mit TIMEOUT kann WAITFOR nur für [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Meldungen verwendet werden. Weitere Informationen finden Sie unter [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md) und [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
## <a name="remarks"></a>Bemerkungen  
 Während der Ausführung der WAITFOR-Anweisung ist die Transaktion im Gange, sodass keine weiteren Anforderungen für dieselbe Transaktion ausgeführt werden können.  
  
 Die tatsächliche Zeitverzögerung kann von der in *time_to_pass*, *time_to_execute* oder *timeout* angegebenen Zeit abweichen und hängt von der Aktivitätsstufe des Servers ab. Die Zeitzählung beginnt, wenn die WAITFOR-Anweisung geplant ist. Wenn der Server ausgelastet ist, wird der Thread möglicherweise nicht sofort geplant. Daher kann die Zeitverzögerung länger als die angegebene Zeit sein.  
  
 WAITFOR nimmt keine Änderung an der Semantik einer Abfrage vor. Wenn die Abfrage keine Zeilen zurückgeben kann, hält die Wartezeit von WAITFOR endlos oder bis zum Erreichen von TIMEOUT (falls angegeben) an.  
  
 Cursor können nicht für WAITFOR-Anweisungen geöffnet werden.  
  
 Ansichten können nicht für WAITFOR-Anweisungen definiert werden.  
  
 Falls die Abfrage die Option Abfragewartezeit überschreitet, kann das Argument der WAITFOR-Anweisung abgeschlossen werden, ohne dass es zur Ausführung kommt. Weitere Informationen zur Konfigurationsoption finden Sie unter [Konfigurieren der Serverkonfigurationsoption Abfragewartezeit](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Mit [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) zeigen Sie die aktiven und wartenden Prozesse an.  
  
 Jeder WAITFOR-Anweisung ist ein Thread zugeordnet. Wenn viele WAITFOR-Anweisungen auf demselben Server angegeben werden, können auch viele Threads durch das Warten auf die Ausführung dieser Anweisungen gebunden werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überwacht die Anzahl die Threads der WAITFOR-Anweisungen und beendet einige davon per Zufallsauswahl, sobald Threads des Servers nicht mehr auf die CPU zugreifen können.  
  
 Sie können einen Deadlock erstellen, indem Sie eine Abfrage mit WAITFOR innerhalb einer Transaktion ausführen, die auch Sperren zur Verhinderung von Änderungen an dem Rowset enthält, auf das die WAITFOR-Anweisung zugreift. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifiziert derartige Szenarien und gibt ein leeres Resultset zurück, wenn die Möglichkeit eines solchen Deadlocks vorhanden ist.  
  
> [!CAUTION]  
>  Durch das Einfügen von WAITFOR wird die Beendigung des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozesses verlangsamt, was zu einer Timeoutmeldung in der Anwendung führen kann. Passen Sie die Timeouteinstellung für die Verbindung ggf. auf Anwendungsebene an.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-waitfor-time"></a>A. Verwenden von WAITFOR TIME  
 Im folgenden Beispiel wird die gespeicherte Prozedur `sp_update_job` in der msdb-Datenbank um 22:20 Uhr ausgeführt. (`22:20`).  
  
```sql  
EXECUTE sp_add_job @job_name = 'TestJob';  
BEGIN  
    WAITFOR TIME '22:20';  
    EXECUTE sp_update_job @job_name = 'TestJob',  
        @new_name = 'UpdatedJob';  
END;  
GO  
```  
  
### <a name="b-using-waitfor-delay"></a>B. Verwenden von WAITFOR DELAY  
 Das folgende Beispiel führt die gespeicherte Prozedur nach einer Verzögerung von 2 Stunden aus.  
  
```sql  
BEGIN  
    WAITFOR DELAY '02:00';  
    EXECUTE sp_helpdb;  
END;  
GO  
```  
  
### <a name="c-using-waitfor-delay-with-a-local-variable"></a>C. Verwenden von WAITFOR DELAY mit einer lokalen Variablen  
 Das folgende Beispiel zeigt, wie Sie eine lokale Variable mit der Option `WAITFOR DELAY` verwenden. Diese gespeicherte Prozedur wartet einen variablen Zeitraum ab und gibt dann Informationen bezüglich der verstrichenen Stunden, Minuten und Sekunden an den Benutzer zurück.  
  
```sql  
IF OBJECT_ID('dbo.TimeDelay_hh_mm_ss','P') IS NOT NULL  
    DROP PROCEDURE dbo.TimeDelay_hh_mm_ss;  
GO  
CREATE PROCEDURE dbo.TimeDelay_hh_mm_ss   
    (  
    @DelayLength char(8)= '00:00:00'  
    )  
AS  
DECLARE @ReturnInfo VARCHAR(255)  
IF ISDATE('2000-01-01 ' + @DelayLength + '.000') = 0  
    BEGIN  
        SELECT @ReturnInfo = 'Invalid time ' + @DelayLength   
        + ',hh:mm:ss, submitted.';  
        -- This PRINT statement is for testing, not use in production.  
        PRINT @ReturnInfo   
        RETURN(1)  
    END  
BEGIN  
    WAITFOR DELAY @DelayLength  
    SELECT @ReturnInfo = 'A total time of ' + @DelayLength + ',   
        hh:mm:ss, has elapsed! Your time is up.'  
    -- This PRINT statement is for testing, not use in production.  
    PRINT @ReturnInfo;  
END;  
GO  
/* This statement executes the dbo.TimeDelay_hh_mm_ss procedure. */  
EXEC TimeDelay_hh_mm_ss '00:00:10';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `A total time of 00:00:10, in hh:mm:ss, has elapsed. Your time is up.`  
  
## <a name="see-also"></a>Weitere Informationen  
 [Control-of-Flow Language &#40;Transact-SQL&#41; (Sprachkonstrukte zur Ablaufsteuerung (Transact-SQL))](~/t-sql/language-elements/control-of-flow.md)   
 [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
