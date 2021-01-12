---
description: GET_TRANSMISSION_STATUS (Transact-SQL)
title: GET_TRANSMISSION_STATUS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STATUS_TSQL
- TRANSMISSION
- TRANSMISSION_TSQL
- GET_TRANSMISSION_STATUS
- STATUS
- GET_TRANSMISSION_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], transmission status
- Service Broker errors, transmission status
- transmission status information
- status information [SQL Server], conversations
- GET_TRANSMISSION_STATUS statement
ms.assetid: 621805d5-49ed-4764-b3cb-2ae4a3bf797e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5a59676494b664e11b3dbd0ad86a2c9d4d9f434
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091844"
---
# <a name="get_transmission_status-transact-sql"></a>GET_TRANSMISSION_STATUS (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Gibt den Status der letzten Übermittlung für eine Seite einer Konversation zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
GET_TRANSMISSION_STATUS ( conversation_handle )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *conversation_id*  
 Bezeichnet das Konversationshandle für die Konversation. Der Parameter weist den Typ **uniqueidentifier** auf.  
  
## <a name="return-types"></a>Rückgabetypen  
 **nchar**  
  
## <a name="remarks"></a>Hinweise  
 Gibt eine Zeichenfolge zurück, die eine Beschreibung des Status des letzten Übermittlungsversuchs für eine angegebene Konversation enthält. Gibt eine leere Zeichenfolge zurück, wenn der letzte Übermittlungsversuch erfolgreich war, falls noch kein Übermittlungsversuch ausgeführt wurde oder falls *conversation_handle* nicht vorhanden ist.  
  
 Die von dieser Funktion zurückgegebenen Informationen sind mit den in der last_transmission_error-Spalte der Verwaltungssicht sys.transmission_queue angezeigten Informationen identisch. Diese Funktion kann jedoch zum Suchen nach dem Übermittlungsstatus von Konversationen verwendet werden, für die zurzeit keine Meldungen in der Übermittlungswarteschlange vorhanden sind.  
  
> [!NOTE]  
>  GET_TRANSMISSION_STATUS stellt nur Informationen zu Meldungen bereit, für die ein Konversationsendpunkt in der aktuellen Instanz vorhanden ist. Zu Meldungen, die weitergeleitet werden müssen, sind demnach keine Informationen verfügbar.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Übermittlungsstatus für die Konversation mit dem Konversationshandle `58ef1d2d-c405-42eb-a762-23ff320bddf0` gemeldet.  
  
```sql  
SELECT Status =  
    GET_TRANSMISSION_STATUS('58ef1d2d-c405-42eb-a762-23ff320bddf0') ;  
```  
  
 Es folgt ein Beispielresultset, das auf Zeilenlänge umformatiert wurde:  
  
 ```
 Status  
 ------------------------------- 
 The Service Broker protocol transport is disabled or not configured.
 ```  
  
 In diesem Fall ist [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] so konfiguriert, dass [!INCLUDE[ssSB](../../includes/sssb-md.md)] nicht über das Netzwerk kommunizieren darf.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)  
  
  
