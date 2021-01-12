---
description: log_shipping_secondary_databases (Transact-SQL)
title: log_shipping_secondary_databases (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_secondary_databases_TSQL
- log_shipping_secondary_databases
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_secondary_databases system table
ms.assetid: ba2374af-86b8-480c-a10c-51e7c4e3ae23
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9a7fb2574f436c5abe40b98723860f03f73a27c4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102103"
---
# <a name="log_shipping_secondary_databases-transact-sql"></a>log_shipping_secondary_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Speichert einen Datensatz pro sekundärer Datenbank in einer Protokollversandkonfiguration. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**secondary_database**|**sysname**|Der Name der sekundären Datenbank in der Protokollversandkonfiguration.|  
|**secondary_id**|**uniqueidentifier**|Die ID für den sekundären Server in der Protokollversandkonfiguration.|  
|**restore_delay**|**int**|Die Zeitspanne in Minuten, die der sekundäre Server wartet, bevor er eine angegebene Sicherungsdatei wiederherstellt. Die Standardeinstellung beträgt 0 Minuten.|  
|**restore_all**|**bit**|Ist der Wert gleich 1, stellt der sekundäre Server bei Ausführung des Wiederherstellungsauftrags alle verfügbaren Sicherungen des Transaktionsprotokolls wieder her. Andernfalls wird Sie beendet, nachdem eine Datei wieder hergestellt wurde.|  
|**restore_mode**|**bit**|Der Wiederherstellungsmodus für die sekundäre Datenbank.<br /><br /> 0 = Das Protokoll wird mit NORECOVERY wiederhergestellt.<br /><br /> 1 = Das Protokoll wird mit STANDBY wiederhergestellt.|  
|**disconnect_users**|**bit**|Ist der Wert gleich 1, werden Benutzer beim Ausführen eines Wiederherstellungsvorgangs von der sekundären Datenbank getrennt. Der Standardwert ist 0.|  
|**block_size**|**int**|Die Größe in Bytes, die als Blockgröße für das Sicherungsmedium verwendet wird.|  
|**buffer_count**|**int**|Die Gesamtanzahl der beim Sicherungs- oder Wiederherstellungsvorgang verwendeten Puffer.|  
|**max_transfer_size**|**int**|Die Größe der maximalen Eingabe- oder Ausgabeanforderung in Bytes, die von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an das Sicherungsmedium ausgegeben wird.|  
|**last_restored_file**|**nvarchar (500)**|Der Dateiname der letzten Sicherungsdatei, die in der sekundären Datenbank wiederhergestellt wurde.|  
|**last_restored_date**|**datetime**|Datum und Uhrzeit des letzten Wiederherstellungsvorgangs für die sekundäre Datenbank.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_secondary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)   
 [sp_delete_log_shipping_secondary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [Systemtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
