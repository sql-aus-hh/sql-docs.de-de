---
description: sys.dm_exec_external_operations (Transact-SQL)
title: sys.dm_exec_external_operations (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- DM_EXEC_EXTERNAL_OPERATIONS_TSQL
- DM_EXEC_EXTERNAL_OPERATIONS
- SYS.DM_EXEC_EXTERNAL_OPERATIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- sys.dm_exec_external_operations management view
- dm_exec_external_operations management view
ms.assetid: d268217a-85b8-4b7f-9cd1-87865eba2be1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 013a6c0ff6110bc8dcf051195e3f7c1ab5fe4798
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098973"
---
# <a name="sysdm_exec_external_operations-transact-sql"></a>sys.dm_exec_external_operations (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  Erfasst Informationen über externe polybase-Vorgänge.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|**nvarchar(32)**|Eindeutige Abfrage-ID, die polybase-Abfrage zugeordnet ist|Siehe ID in [sys.dm_exec_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|  
|step_index|**int**|Index des Abfrage Schritts|Weitere Informationen finden Sie unter step_index in [sys.dm_exec_distributed_request_steps &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md)|  
|operation_-Typ|**nvarchar(128)**|Beschreibt einen Hadoop-Vorgang oder einen anderen externen Vorgang.|"Externer Hadoop-Vorgang"|  
|operation_ Name|**nvarchar(4000)**|Gibt an, wie der Status des Auftrags in Prozent (wie viel die Eingabe verbraucht) ist.|0-1-multipliziert mit Faktor 100 (abgeschlossen)|  
|map_ Status|**float**|Gibt an, wie der Status eines Reduzierungs Auftrags in Prozent (falls vorhanden) reduziert wird.|0-1-multipliziert mit Faktor 100 (abgeschlossen)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Problembehandlung bei polybase mit dynamischen Verwaltungs Sichten](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
