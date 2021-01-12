---
description: GET CONVERSATION GROUP (Transact-SQL)
title: GET CONVERSATION GROUP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GET
- CONVERSATION_GROUP_TSQL
- GET_TSQL
- GET_CONVERSATION_GROUP_TSQL
- GET CONVERSATION GROUP
- CONVERSATION GROUP
- GET CONVERSATION
- GET_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GET CONVERSATION GROUP statement
- conversations [Service Broker], groups
ms.assetid: 4da8a855-33c0-43b2-a49d-527487cb3b5c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e54839bd7690b0417142fb4ac77994470f3a076e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091913"
---
# <a name="get-conversation-group-transact-sql"></a>GET CONVERSATION GROUP (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Gibt den Bezeichner der Konversationsgruppe für die nächste zu empfangende Nachricht zurück und sperrt die Konversationsgruppe für die Konversation, die die Nachricht enthält. Der Konversationsgruppenbezeichner kann zum Abrufen der Informationen zum Konversationsstatus vor dem Abrufen der eigentlichen Nachricht verwendet werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql 
[ WAITFOR ( ]  
   GET CONVERSATION GROUP @conversation_group_id  
      FROM <queue>  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 WAITFOR  
 Gibt an, dass die GET CONVERSATION GROUP-Anweisung darauf wartet, dass eine Nachricht in der Warteschlange eintrifft, wenn zurzeit keine Nachrichten vorhanden sind.  
  
 *\@conversation_group_id*  
 Eine Variable zum Speichern der Konversationsgruppen-ID, die von der GET CONVERSATION GROUP-Anweisung zurückgegeben wird. Die Variable muss vom Typ **uniqueidentifier** sein. Sind keine Konversationsgruppen verfügbar, wird die Variable auf NULL festgelegt.  
  
 FROM  
 Gibt die Warteschlange an, aus der die Konversationsgruppe abgerufen werden soll.  
  
 *database_name*  
 Der Name der Datenbank, die die Warteschlange enthält, aus der die Konversationsgruppe abgerufen werden soll. Wenn *database_name* nicht bereitgestellt wird, wird standardmäßig die aktuelle Datenbank verwendet.  
  
 *schema_name*  
 Der Name des Schemas, das die Warteschlange besitzt, aus der die Konversationsgruppe abgerufen werden soll. Wenn *schema_name* nicht angegeben wird, wird standardmäßig das Standardschema für den aktuellen Benutzer verwendet.  
  
 *queue_name*  
 Der Name der Warteschlange, aus der die Konversationsgruppe abgerufen werden soll.  
  
 TIMEOUT *timeout*  
 Gibt die Zeitdauer (in Millisekunden) an, die Service Broker auf das Eintreffen einer Nachricht in der Warteschlange wartet. Diese Klausel darf nur zusammen mit der WAITFOR-Klausel verwendet werden. Wenn eine Anweisung, die WAITFOR verwendet, diese Klausel nicht einschließt, oder wenn *timeout* den Wert -1 aufweist, unterliegt die Wartezeit keiner Begrenzung. Wenn das Timeout abläuft, legt GET CONVERSATION GROUP die Variable *\@conversation_group_id* auf NULL fest.  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!IMPORTANT]  
>  Falls die GET CONVERSATION GROUP-Anweisung nicht die erste Anweisung in einem Batch oder einer gespeicherten Prozedur ist, muss die vorherige Anweisung mit einem Semikolon (**;**) enden, dem Abschlusszeichen für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen.  
  
 Steht die in der Warteschlange angegebene GET CONVERSATION GROUP-Anweisung nicht zur Verfügung, wird für die Anweisung ein [!INCLUDE[tsql](../../includes/tsql-md.md)]-Fehler erzeugt.  
  
 Diese Anweisung gibt die nächste Konversationsgruppe zurück, wobei Folgendes gilt:  
  
-   Die Konversationsgruppe kann erfolgreich gesperrt werden.  
  
-   Die Konversationsgruppe verfügt in der Warteschlange über Nachrichten.  
  
-   Die Konversationsgruppe weist von allen Konversationsgruppen, die die zuvor genannten Kriterien erfüllen, die höchste Prioritätsebene auf. Die Prioritätsebene einer Konversationsgruppe ist die höchste Prioritätsebene, die einer Konversation zugeordnet ist, die der Gruppe angehört und in der Warteschlange Nachrichten aufweist.  
  
 Aufeinander folgende Aufrufe von GET CONVERSATION GROUP innerhalb derselben Transaktion können mehr als eine Konversationsgruppe sperren. Steht keine Konversationsgruppe zur Verfügung, gibt die Anweisung NULL als Bezeichner der Konversationsgruppe zurück.  
  
 Wenn die WAITFOR-Klausel angegeben wird, wartet die Anweisung bis zum Ablauf des angegebenen Timeouts oder so lange, bis eine Konversationsgruppe verfügbar ist. Wird die Warteschlange gelöscht, während die Anweisung wartet, gibt die Anweisung sofort einen Fehler zurück.  
  
 GET CONVERSATION GROUP ist in einer benutzerdefinierten Funktion ungültig.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Abrufen des Bezeichners einer Konversationsgruppe aus einer Warteschlange benötigt der aktuelle Benutzer die RECEIVE-Berechtigung für die Warteschlange.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-getting-a-conversation-group-waiting-indefinitely"></a>A. Abrufen einer Konversationsgruppe ohne Begrenzung der Wartezeit  
 Im folgenden Beispiel wird `@conversation_group_id` auf den Konversationsgruppenbezeichner für die nächste verfügbare Nachricht in `ExpenseQueue` festgelegt. Der Befehl wartet so lange, bis eine Nachricht verfügbar ist.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
WAITFOR (  
 GET CONVERSATION GROUP @conversation_group_id  
     FROM ExpenseQueue  
) ;  
```  
  
### <a name="b-getting-a-conversation-group-waiting-one-minute"></a>B. Abrufen einer Konversationsgruppe mit einer Wartezeit von einer Minute  
 Im folgenden Beispiel wird `@conversation_group_id` auf den Konversationsgruppenbezeichner für die nächste verfügbare Nachricht in `ExpenseQueue` festgelegt. Ist keine Nachricht innerhalb einer Minute verfügbar, wird GET CONVERSATION GROUP ohne Änderung des Werts von `@conversation_group_id` zurückgegeben.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER  
  
WAITFOR (  
    GET CONVERSATION GROUP @conversation_group_id   
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="c-getting-a-conversation-group-returning-immediately"></a>C. Abrufen einer Konversationsgruppe mit unmittelbarer Rückgabe  
 Im folgenden Beispiel wird `@conversation_group_id` auf den Konversationsgruppenbezeichner für die nächste verfügbare Nachricht in `ExpenseQueue` festgelegt. Wenn keine Nachricht verfügbar ist, wird `GET CONVERSATION GROUP` sofort ohne Änderung des Werts von `@conversation_group_id` zurückgegeben.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
GET CONVERSATION GROUP @conversation_group_id  
FROM AdventureWorks.dbo.ExpenseQueue ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)  
  
  
