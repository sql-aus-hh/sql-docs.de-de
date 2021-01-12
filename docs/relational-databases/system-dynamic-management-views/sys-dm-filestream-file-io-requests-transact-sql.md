---
description: sys.dm_filestream_file_io_requests (Transact-SQL)
title: sys.dm_filestream_file_io_requests (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_filestream_file_io_requests
- dm_filestream_file_io_requests
- sys.dm_filestream_file_io_requests_TSQL
- dm_filestream_file_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_requests catalog view
ms.assetid: d41e39a5-14d5-4f3d-a2e3-a822b454c1ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 69b34fd2a8ec5adf393994399011cf1b2df6ea03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097659"
---
# <a name="sysdm_filestream_file_io_requests-transact-sql"></a>sys.dm_filestream_file_io_requests (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt eine Liste mit E/A-Anforderungen an, die im betreffenden Moment vom Namespace-Besitzer (NSO) verarbeitet werden.  
  
|Spalte|Typ|Beschreibung|  
|------------|----------|-----------------|  
|**request_context_address**|**varbinary(8)**|Zeigt die interne Adresse des NSO-Speicherblocks an, der die E/A-Anforderung des Treibers enthält. Lässt keine NULL-Werte zu.|  
|**current_spid**|**smallint**|Zeigt die System Prozess-ID (SPID) für die Verbindung des aktuellen SQL Server an. Lässt keine NULL-Werte zu.|  
|**request_type**|**nvarchar(60)**|Zeigt den Typ des E/A-Anforderungspakets (IRP) an. Die möglichen Anforderungstypen sind REQ_PRE_CREATE, REQ_POST_CREATE, REQ_RESOLVE_VOLUME, REQ_GET_VOLUME_INFO, REQ_GET_LOGICAL_NAME, REQ_GET_PHYSICAL_NAME, REQ_PRE_CLEANUP, REQ_POST_CLEANUP, REQ_CLOSE, REQ_FSCTL, REQ_QUERY_INFO, REQ_SET_INFO, REQ_ENUM_DIRECTORY, REQ_QUERY_SECURITY und REQ_SET_SECURITY. Lässt keine NULL-Werte zu.|  
|**request_state**|**nvarchar(60)**|Zeigt den Status der E/A-Anforderung in NSO an. Mögliche Werte sind REQ_STATE_RECEIVED, REQ_STATE_INITIALIZED, REQ_STATE_ENQUEUED, REQ_STATE_PROCESSING, REQ_STATE_FORMATTING_RESPONSE, REQ_STATE_SENDING_RESPONSE, REQ_STATE_COMPLETING und REQ_STATE_COMPLETED. Lässt keine NULL-Werte zu.|  
|**request_id**|**int**|Zeigt die eindeutige Anforderungs-ID an, die der Anforderung vom Treiber zugewiesen ist. Lässt keine NULL-Werte zu.|  
|**irp_id**|**int**|Zeigt die eindeutige IRP-ID an. Dies ist zur Ermittlung aller E/A-Anforderungen hilfreich, die mit der vorliegenden IRP in Verbindung stehen. Lässt keine NULL-Werte zu.|  
|**handle_id**|**int**|Gibt die Handle-ID für den Namespace an. Dies ist der NSO-spezifische Bezeichner und in einer Instanz eindeutig. Lässt keine NULL-Werte zu.|  
|**client_thread_id**|**varbinary(8)**|Zeigt die Thread-ID der Client Anwendung an, von der die Anforderung stammt.<br /><br /> Warnung Dies ist nur sinnvoll, wenn die Client Anwendung auf dem gleichen Computer wie SQL Server ausgeführt wird. **\* \* \* \*** Wird die Clientanwendung remote ausgeführt, zeigt die **client_thread_id** die Thread-ID eines Systemprozesses an, der für den Remoteclient ausgeführt wird.<br /><br /> Lässt NULL-Werte zu.|  
|**client_process_id**|**varbinary(8)**|Zeigt die Prozess-ID der Clientanwendung an, wenn die Clientanwendung auf dem gleichen Computer wie SQL Server ausgeführt wird. Im Falle eines Remoteclients wird die Systemprozess-ID angezeigt, die für die Clientanwendung angezeigt wird. Lässt NULL-Werte zu.|  
|**handle_context_address**|**varbinary(8)**|Zeigt die Adresse der internen nso-Struktur an, die dem Handle des Clients zugeordnet ist. Lässt NULL-Werte zu.|  
|**filestream_transaction_id**|**varbinary(128)**|Zeigt die ID der Transaktion an, die der vorliegenden Handle und allen dieser Handle zugeordneten Anforderungen zugeordnet ist. Hierbei handelt es sich um den Wert, der von der **get_filestream_transaction_context** -Funktion zurückgegeben wird. Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten für FILESTREAM und FILETABLE &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
