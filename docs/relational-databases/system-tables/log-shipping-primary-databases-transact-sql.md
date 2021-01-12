---
description: log_shipping_primary_databases (Transact-SQL)
title: log_shipping_primary_databases (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_primary_databases
- log_shipping_primary_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_primary_databases system table
ms.assetid: 56888756-a798-42be-9b5e-0f9aa05a2cc6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8dd97d1baa37b71fec2e1ceb113e4b0a3614a2e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102144"
---
# <a name="log_shipping_primary_databases-transact-sql"></a>log_shipping_primary_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Speichert einen Datensatz für die primäre Datenbank in einer Protokollversandkonfiguration. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|Die ID der primären Datenbank für die Protokollversandkonfiguration.|  
|**primary_database**|**sysname**|Der Name der primären Datenbank in der Protokollversandkonfiguration|  
|**backup_directory**|**nvarchar (500)**|Das Verzeichnis, in dem die Dateien der Transaktionsprotokollsicherung gespeichert werden.|  
|**backup_share**|**nvarchar (500)**|Der Netzwerk- oder UNC-Pfad zum Sicherungsverzeichnis.|  
|**backup_retention_period**|**int**|Gibt an, wie lange (in Minuten) eine Protokollsicherungsdatei im Sicherungsverzeichnis aufbewahrt wird, bevor sie gelöscht wird.|  
|**backup_job_id**|**uniqueidentifier**|Die ID des [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Auftrags, die dem Sicherungsauftrag auf dem primären Server zugeordnet ist.|  
|**monitor_server**|**sysname**|Der Name der Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , die als Überwachungsserver in der Protokollversandkonfiguration verwendet wird.|  
|**monitor_server_security_mode**|**bit**|Der Sicherheitsmodus, der zum Herstellen einer Verbindung mit dem Überwachungsserver verwendet wird.<br /><br /> 1 = Windows-Authentifizierung<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung.|  
|**last_backup_file**|**nvarchar (500)**|Der absolute Pfad der jüngsten Transaktionsprotokollsicherung.|  
|**last_backup_date**|**datetime**|Uhrzeit und Datum des letzten Protokollsicherungsvorgangs.|  
|**user_specified_monitor**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Diese Spalte wird von **sp_help_log_shipping_primary_database** und **sp_help_log_shipping_secondary_primary** verwendet, um die Anzeige der Überwachungseinstellungen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zu steuern.<br /><br /> 0 = der Benutzer hat beim Aufrufen einer dieser beiden gespeicherten Prozeduren keinen expliziten Wert für den **\@ monitor_server** -Parameter angegeben.<br /><br /> 1= Der Benutzer hat einen expliziten Wert angegeben.|  
|**backup_compression**|**tinyint**|Gibt an, ob die Protokollversandkonfiguration das Verhalten der Sicherungskomprimierung auf Serverebene überschreibt.<br /><br /> 0 = Deaktiviert. Protokollsicherungen werden niemals komprimiert, unabhängig von den für den Server konfigurierten Sicherungskomprimierungseinstellungen.<br /><br /> 1 = Aktiviert. Protokollsicherungen werden immer komprimiert, unabhängig von den für den Server konfigurierten Sicherungskomprimierungseinstellungen.<br /><br /> 2 = Verwendet die Serverkonfiguration für die Serverkonfigurationsoption [View or Configure the backup compression default Server Configuration Option](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) . Dies ist der Standardwert.<br /><br /> Die Sicherungskomprimierung wird nur in der Enterprise Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]unterstützt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [Systemtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
