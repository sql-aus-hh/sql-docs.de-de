---
description: sys.dm_repl_traninfo (Transact-SQL)
title: sys.dm_repl_traninfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_repl_traninfo
- dm_repl_traninfo
- sys.dm_repl_traninfo_TSQL
- dm_repl_traninfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_traninfo dynamic management view
ms.assetid: 5abe2605-0506-46ec-82b5-6ec08428ba13
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f8a0613d83cebfed56ba5202a6c3535ef31c8a84
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098844"
---
# <a name="sysdm_repl_traninfo-transact-sql"></a>sys.dm_repl_traninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen über jede replizierte Transaktion bzw. jede Change Data Capture-Transaktion zurück.  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**fp2p_pub_exists**|**tinyint**|Gibt an, ob sich die Transaktion in einer Datenbank befindet, die mithilfe der Peer-zu-Peer-Transaktionsreplikation veröffentlicht wurde. Wenn True, ist der Wert gleich 1; andernfalls ist der Wert 0.|  
|**db_ver**|**int**|Die Datenbankversion.|  
|**comp_range_address**|**varbinary(8)**|Definiert einen Bereich für teilweises Rollback, der übersprungen werden muss.|  
|**textinfo_address**|**varbinary(8)**|Speicherinterne Adresse der zwischengespeicherten Textinformationsstruktur.|  
|**fsinfo_address**|**varbinary(8)**|Speicherinterne Adresse der zwischengespeicherten FILESTREAM-Informationsstruktur.|  
|**begin_lsn**|**nvarchar (64)**|Protokollsequenznummer (Log Sequence Number, LSN) des Protokolleintrags für den Beginn der Transaktion.|  
|**commit_lsn**|**nvarchar (64)**|LSN des Protokolldatensatz für den Commit der Transaktion.|  
|**dbid**|**smallint**|Datenbank-ID|  
|**rows**|**int**|ID des replizierten Befehls in der Transaktion.|  
|**xdesid für**|**nvarchar (64)**|Transaktions-ID|  
|**artcache_table_address**|**varbinary(8)**|Speicherinterne Adresse der zwischengespeicherten Artikeltabellenstruktur, die zuletzt für diese Transaktion verwendet wurde.|  
|**server**|**nvarchar (514)**|Servername.|  
|**server_len_in_bytes**|**smallint**|Zeichenlänge des Servernamens (in Bytes).|  
|**database**|**nvarchar (514)**|Datenbankname.|  
|**db_len_in_bytes**|**smallint**|Zeichenlänge des Datenbanknamens (in Bytes).|  
|**originator**|**nvarchar (514)**|Name des Servers, von dem die Transaktion stammt.|  
|**originator_len_in_bytes**|**smallint**|Zeichenlänge des Servernamens (in Bytes), von dem die Transaktion stammt.|  
|**orig_db**|**nvarchar (514)**|Name der Datenbank, von der die Transaktion stammt.|  
|**orig_db_len_in_bytes**|**smallint**|Zeichenlänge der Datenbank (in Bytes), von der die Transaktion stammt.|  
|**cmds_in_tran**|**int**|Anzahl der replizierten Befehle in der aktuellen Transaktion, die zum Bestimmen des Zeitpunktes verwendet werden, an dem ein Commit für eine logische Transaktion ausgeführt werden sollte.|  
|**is_boundedupdate_singleton**|**tinyint**|Gibt an, ob ein eindeutiges Spaltenupdate nur eine einzelne Zeile betrifft.|  
|**begin_update_lsn**|**nvarchar (64)**|Die in einem eindeutigen Spaltenupdate verwendete LSN.|  
|**delete_lsn**|**nvarchar (64)**|Die als Teil eines Updates zu löschende LSN.|  
|**last_end_lsn**|**nvarchar (64)**|Letzte LSN in einer logischen Transaktion.|  
|**fcomplete**|**tinyint**|Gibt an, ob es sich bei dem Befehl um ein teilweises Update handelt.|  
|**fcompensated**|**tinyint**|Gibt an, ob die Transaktion in ein teilweises Rollback einbezogen ist.|  
|**fprocessingtext**|**tinyint**|Gibt an, ob die Transaktion eine Spalte vom BLOB-Datentyp enthält.|  
|**max_cmds_in_tran**|**int**|Maximale Anzahl von Befehlen in einer logischen Transaktion, wie vom Protokolllese-Agent angegeben.|  
|**begin_time**|**datetime**|Zeitpunkt, zu dem die Transaktion begonnen wurde.|  
|**commit_time**|**datetime**|Zeitpunkt, zu dem ein Commit der Transaktion ausgeführt wurde.|  
|**session_id**|**int**|ID der Protokollscansitzung für Change Data Capture. Diese Spalte wird der Spalte **session_id** in [sys.dm_cdc_logscan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md)zugeordnet.|  
|**session_phase**|**int**|Zahl, die die Phase angibt, in der sich die Sitzung beim Auftreten des Fehlers befand. Diese Spalte wird der Spalte **phase_number** in [sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)zugeordnet.|  
|**is_known_cdc_tran**|**bit**|Gibt an, dass die Transaktion von Change Data Capture verfolgt wird.<br /><br /> 0 = Transaktionsreplikationstransaktion.<br /><br /> 1 = Change Data Capture-Transaktion.|  
|**error_count**|**int**|Anzahl der aufgetretenen Fehler.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung in der Veröffentlichungsdatenbank oder in der für Change Data Capture aktivierten Datenbank.  
  
## <a name="remarks"></a>Bemerkungen  
 Informationen werden nur für replizierte Datenbankobjekte oder für für Change Data Capture aktivierte Tabellen zurückgegeben, die zurzeit in den Artikelcache geladen sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Replikation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)   
 [Dynamische Verwaltungssichten in Bezug auf Change Data Capture &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)  
  
