---
description: BEGIN CONVERSATION TIMER (Transact-SQL)
title: BEGIN CONVERSATION TIMER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CONVERSATION
- BEGIN_CONVERSATION_TSQL
- TIMER_TSQL
- TIMER
- CONVERSATION TIMER
- CONVERSATION_TIMER_TSQL
- BEGIN CONVERSATION TIMER
- BEGIN_CONVERSATION_TIMER_TSQL
- CONVERSATION_TSQL
- BEGIN CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CONVERSATION TIMER statement
- DialogTimer message
- dialogs [Service Broker], beginning
- timeouts [Service Broker]
- timer messages [Service Broker]
- conversations [Service Broker], timers
- starting timers [Service Broker]
- https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer message
ms.assetid: 98e49b3f-a38f-4180-8171-fa9cb30db4cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6c30351f5fe448b967d874f3ef0a9eb5dc94faa5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098452"
---
# <a name="begin-conversation-timer-transact-sql"></a>BEGIN CONVERSATION TIMER (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Startet einen Zeitgeber. Wenn das Timeout abläuft, stellt [!INCLUDE[ssSB](../../includes/sssb-md.md)] eine Nachricht des Typs `https://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer` in die lokale Warteschlange für die Konversation.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
BEGIN CONVERSATION TIMER ( conversation_handle )  
   TIMEOUT = timeout   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 BEGIN CONVERSATION TIMER **(** _conversation\_handle_ **)**  
 Gibt die Konversation an, für die ein Zeitgeber festgelegt werden soll. *conversation_handle* muss dem Typ **uniqueidentifier** entsprechen.  
  
 TIMEOUT  
 Gibt die Zeitdauer in Sekunden an, die gewartet wird, bevor die Nachricht in der Warteschlange platziert wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Mithilfe eines Konversationszeitgebers kann eine Anwendung nach einem bestimmten Zeitraum eine Nachricht zu einer Konversation empfangen. Wird BEGIN CONVERSATION TIMER für eine Konversation vor Ablauf des Zeitgebers aufgerufen, wird das Timeout auf den neuen Wert festgelegt. Anders als bei der Lebensdauer der Konversation verfügt jede Seite der Konversation über einen unabhängigen Konversationszeitgeber. Die **DialogTimer**-Nachricht kommt in der lokalen Warteschlange an, ohne dass dies Auswirkungen auf die Remoteseite der Konversation hat. Daher kann eine Zeitgebernachricht zu jedem beliebigen Zweck von einer Anwendung verwendet werden.  
  
 Sie können z. B. mithilfe des Konversationszeitgebers verhindern, dass eine Anwendung zu lang auf eine überfällige Antwort wartet. Wenn die Anwendung einen Dialog innerhalb von 30 Sekunden beenden soll, können Sie den Konversationszeitgeber für diesen Dialog auf 60 Sekunden festlegen (30 Sekunden plus einem Zeitraum von 30 Sekunden als Puffer). Falls der Dialog nach 60 Sekunden immer noch geöffnet ist, empfängt die Anwendung eine Timeoutnachricht in der Warteschlange für diesen Dialog.  
  
 Alternativ hierzu kann eine Anwendung auch mithilfe eines Konversationszeitgebers die Aktivierung zu einer bestimmten Zeit anfordern. Sie können z. B. einen Dienst erstellen, der alle fünf Minuten die Anzahl der aktiven Verbindungen angibt, oder einen Dienst, der jeden Abend die Anzahl der offenen Bestellungen angibt. Der Dienst legt einen Konversationstimer fest, der zu der gewünschten Zeit abläuft. Bei Ablauf des Zeitgebers sendet [!INCLUDE[ssSB](../../includes/sssb-md.md)] eine **DialogTimer**-Nachricht. Mit der **DialogTimer**-Nachricht startet [!INCLUDE[ssSB](../../includes/sssb-md.md)] die gespeicherte Aktivierungsprozedur für die Warteschlange. Von der gespeicherten Prozedur wird eine Nachricht an den Remotedienst gesendet und ein Neustart des Konversationszeitgebers ausgeführt.  
  
 BEGIN CONVERSATION TIMER ist in einer benutzerdefinierten Funktion nicht gültig.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Festlegen eines Konversationstimers erhalten standardmäßig die Benutzer, die über SEND-Berechtigungen für den Dienst der Konversation verfügen, die Mitglieder der festen Serverrolle **sysadmin** und die Mitglieder der festen Datenbankrolle **db_owner**.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Timeout von zwei Minuten für den durch `@dialog_handle` identifizierten Dialog festgelegt.  
  
```sql 
-- @dialog_handle is of type uniqueidentifier and  
-- contains a valid conversation handle.  
  
BEGIN CONVERSATION TIMER (@dialog_handle)  
TIMEOUT = 120 ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)  
  
  
