---
description: log_shipping_monitor_error_detail (Transact-SQL)
title: log_shipping_monitor_error_detail (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_monitor_error_detail_TSQL
- log_shipping_monitor_error_detail
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_error_detail system table
ms.assetid: 0c38a625-60d2-4ee2-bcf3-2ba367914220
author: cawrites
ms.author: chadam
ms.openlocfilehash: 131ee9bfe37dce356c6e1e4e604e8e8f0c3df2c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097459"
---
# <a name="log_shipping_monitor_error_detail-transact-sql"></a>log_shipping_monitor_error_detail (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Speichert Fehlerdetails für Protokollversandaufträge. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
 Die verlaufs- und überwachungsverbundenen Tabellen werden auch auf dem primären und dem sekundären Server verwendet.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**agent_id**|**uniqueidentifier**|Die primäre ID für Sicherungsvorgänge oder die sekundäre ID für Kopier- oder Wiederherstellungsvorgänge.|  
|**agent_type**|**tinyint**|Der Typ des Protokollversandauftrags.<br /><br /> 0 = Sicherungsauftrag<br /><br /> 1 = Kopierauftrag<br /><br /> 2 = Wiederherstellungsauftrag|  
|**session_id**|**int**|Die Sitzungs-ID für den Sicherungs-/Kopier-/Wiederherstellungsauftrag|  
|**database_name**|**sysname**|Der Name der diesem Fehlerdatensatz zugeordneten Datenbank. Die primäre Datenbank für Sicherungsaufträge, die sekundäre Datenbank für Wiederherstellungsaufträge oder eine leere Datenbank für Kopieraufträge.|  
|**sequence_number**|**int**|Eine inkrementelle Zahl, die auf die richtige Reihenfolge von Fehlerinformationen hinweist, die mehrere Datensätze umfassen.|  
|**log_time**|**datetime**|Datum und Uhrzeit der Datensatzerstellung|  
|**log_time_utc**|**datetime**|Datum und Uhrzeit der Datensatzerstellung in UTC (Coordinated Universal Time, Koordinierte Weltzeit)|  
|**Nachricht**|**nvarchar**|Meldungstext.|  
|**Quelle**|**nvarchar**|Die Quelle der Fehlermeldung oder des Ereignisses.|  
|**help_url**|**nvarchar**|Die URL (sofern vorhanden), unter der weitere Informationen zum Fehler zur Verfügung stehen.|  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Tabelle enthält Fehlerdetails für die Protokollversand-Agents. Jeder Fehler wird als eine Sequenz von Ausnahmen protokolliert. Für jede Sitzung eines Agents sind mehrere Fehler (Sequenzen) möglich.  
  
 Die mit dem primären verbundenen Informationen Server werden sowohl auf dem Remoteüberwachungsserver als auch auf dem primären Server in der **log_shipping_monitor_error_detail** -Tabelle gespeichert. Die mit dem sekundären Server verbundenen Informationen werden ebenfalls auf dem sekundären Server in der **log_shipping_monitor_error_detail** -Tabelle gespeichert.  
  
 Zur Identifizierung einer Agentsitzung können Sie die Spalten **agent_id**, **agent_type** und **session_id** verwenden. Sortieren Sie die Spalten nach **log_time** , um die Fehler in der Reihenfolge ihrer Protokollierung anzuzeigen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [log_shipping_monitor_history_detail &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [Systemtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
