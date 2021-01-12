---
description: sys.dm_exec_distributed_sql_requests (Transact-SQL)
title: sys.dm_exec_distributed_sql_requests (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_DISTRIBUTED_SQL_REQUESTS_TSQL
- SYS.DM_EXEC_DISTRIBUTED_SQL_REQUESTS_TSQL
- DM_EXEC_DISTRIBUTED_SQL_REQUESTS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- sys.dm_exec_distributed_requests management view
- dm_exec_distributed_requests management view
ms.assetid: d065dc01-35d4-472f-9554-53ac41e7d104
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b03ee4a4c974be2b035074f03b8397df580f75d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099003"
---
# <a name="sysdm_exec_distributed_sql_requests-transact-sql"></a>sys.dm_exec_distributed_sql_requests (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Enthält Informationen zu allen SQL-Abfrage Verteilungen als Teil eines SQL-Schritts in der Abfrage.  In dieser Ansicht werden die Daten für die letzten 1000-Anforderungen angezeigt. für aktive Anforderungen sind die Daten in dieser Sicht immer vorhanden.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|execution_id und step_index bilden den Schlüssel für diese Ansicht. Eindeutige numerische ID, die der Anforderung zugeordnet ist.|Siehe ID in [sys.dm_exec_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|  
|step_index|**int**|Index des Abfrage Schritts, zu dem diese Distribution gehört.|Weitere Informationen finden Sie unter step_index in [sys.dm_exec_distributed_request_steps &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md).|  
|compute_node_id|**int**|Der Typ des Vorgangs, der durch diesen Schritt dargestellt wird.|Weitere Informationen finden Sie unter compute_node_id in [sys.dm_exec_compute_nodes &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|distribution_id|**int**|Gibt an, wo der Schritt ausgeführt wird.|Legen Sie für Anforderungen, die im Knotenbereich ausgeführt werden, nicht den Verteilungsbereich auf-1 fest.|  
|status|**nvarchar(32)**|Status dieses Schritts|Aktiv, abgebrochen, abgeschlossen, fehlgeschlagen, in Warteschlange eingereiht|  
|error_id|**nvarchar (36)**|Eindeutige ID des Fehlers, der diesem Schritt zugeordnet ist, sofern vorhanden.|Siehe ID des [sys.dm_exec_compute_node_errors &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md); NULL, wenn kein Fehler aufgetreten ist.|  
|start_time|**datetime**|Zeitpunkt, zu dem die Ausführung des Schritts gestartet wurde|Kleiner oder gleich der aktuellen Zeit und größer oder gleich end_compile_time der Abfrage, zu der dieser Schritt gehört.|  
|end_time|**datetime**|Der Zeitpunkt, zu dem dieser Schritt die Ausführung abgeschlossen hat, abgebrochen wurde oder fehlgeschlagen ist.|Kleiner oder gleich der aktuellen Zeit und größer oder gleich start_time, auf NULL für Schritte festgelegt, die gerade ausgeführt werden oder in die Warteschlange eingereiht werden.|  
|total_elapsed_time|**int**|Gesamtzeit Spanne, für die der Abfrage Schritt ausgeführt wurde (in Millisekunden)|Zwischen 0 und dem Unterschied zwischen end_time und start_time. 0 für Schritte in der Warteschlange.|  
|row_count|**bigint**|Gesamtanzahl der von dieser Anforderung geänderten oder zurückgegebenen Zeilen|0 für Schritte, bei denen keine Daten geändert oder zurückgegeben wurden, die Anzahl der betroffenen Zeilen. Legen Sie für DMS-Schritte den Wert-1 fest.|  
|spid|**int**|Sitzungs-ID für die SQL Server Instanz, die die Abfrage Verteilung ausführt||  
|-Befehl.|nvarchar(4000)|Enthält den vollständigen Text des Befehls dieses Schritts.|Eine beliebige gültige Anforderungs Zeichenfolge für einen Schritt. Wird abgeschnitten, wenn mehr als 4000 Zeichen enthalten sind.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Problembehandlung bei polybase mit dynamischen Verwaltungs Sichten](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
