---
description: sys.dm_repl_tranhash (Transact-SQL)
title: sys.dm_repl_tranhash (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_repl_tranhash
- sys.dm_repl_tranhash_TSQL
- dm_repl_tranhash_TSQL
- dm_repl_tranhash
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_tranhash dynamic management view
ms.assetid: 0cc52338-e805-4ed4-9835-b19bbf72448e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d099f878fd2507d0f491723ffc494ce335246843
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098845"
---
# <a name="sysdm_repl_tranhash-transact-sql"></a>sys.dm_repl_tranhash (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu Transaktionen zurück, die in einer Transaktionsveröffentlichung repliziert werden.  
  
|column_name|data_type|description|  
|------------------|----------------|-----------------|  
|**buckets**|**bigint**|Die Anzahl der Buckets in der Hashtabelle.|  
|**hashed_trans**|**bigint**|Anzahl der im aktuellen Batch replizierten Transaktionen, für die ein Commit durchgeführt wurde.|  
|**completed_trans**|**bigint**|Anzahl der bisher abgeschlossenen Transaktionen.|  
|**compensated_trans**|**bigint**|Anzahl der Transaktionen, die teilweise Rollbacks enthalten.|  
|**first_begin_lsn**|**nvarchar (64)**|Erste Protokollfolgenummer (LSN, Log Sequence Number) im aktuellen Batch.|  
|**last_commit_lsn**|**nvarchar (64)**|LSN des letzten Commits im aktuellen Batch.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung für die Veröffentlichungsdatenbank, die **dm_repl_tranhash** aufruft.  
  
## <a name="remarks"></a>Bemerkungen  
 Informationen werden nur für replizierte Datenbankobjekte zurückgegeben, die zurzeit in den Replikationsartikelcache geladen sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Replikation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  
