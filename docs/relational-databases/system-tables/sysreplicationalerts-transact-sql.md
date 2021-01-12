---
description: sysreplicationalerts (Transact-SQL)
title: sysreplicationalerts (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysreplicationalerts_TSQL
- sysreplicationalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysreplicationalerts system table
ms.assetid: 6ed15828-8cca-4cf0-b2ff-1ecd0d8db11a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2f44fb1f1b07b53705b45825955918c3026a2485
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097320"
---
# <a name="sysreplicationalerts-transact-sql"></a>sysreplicationalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält Informationen zu den Bedingungen, die die Auslösung einer Replikationswarnung verursachen. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|Die ID der Warnung.|  
|**status**|**int**|Der benutzerdefinierte Wert:<br /><br /> **0** = nicht gewartet.<br /><br /> **1** = gewartet.|  
|**agent_type**|**int**|Typ der Momentaufnahme:<br /><br /> **1** = Momentaufnahmen-Agent.<br /><br /> **2** = Protokolllese-Agent.<br /><br /> **3** = Verteilungs-Agent.<br /><br /> **4** = Merge-Agent.|  
|**agent_id**|**int**|Die Agent-ID aus den Tabellen **MSsnapshot_agents**, **MSlogreader_agents**, **MSdistribution_agents** oder **MSmerge_agents**.|  
|**error_id**|**int**|Die ID des Fehlers, der in **MSrepl_errors** gespeichert ist.|  
|**alert_error_code**|**int**|Die Meldungs-ID der bei der Protokollierung dieses Datensatzes ausgelösten Warnung.|  
|**time**|**datetime**|Der Zeitpunkt, zu dem der Datensatz eingefügt wurde.|  
|**publisher**|**sysname**|Der Name des Verlegers, der dem Agent zugeordnet ist, der diese Warnung ausgelöst hat.|  
|**publisher_db**|**sysname**|Die Verlegerdatenbank, die dem Agent zugeordnet ist, der diese Warnung ausgelöst hat.|  
|**ung**|**sysname**|Die Veröffentlichung, die dem Agent zugeordnet ist, der diese Warnung ausgelöst hat.|  
|**publication_type**|**int**|Der Typ der Veröffentlichung:<br /><br /> **0** = Momentaufnahme.<br /><br /> **1** = transaktional.<br /><br /> **2** = Merge.|  
|**Abonnenten**|**sysname**|Der Name des Abonnenten, der dem Agent zugeordnet ist, der diese Warnung ausgelöst hat.|  
|**subscriber_db**|**sysname**|Der Name der Abonnentendatenbank, die dem Agent zugeordnet ist, der diese Warnung ausgelöst hat.|  
|**Artikel**|**sysname**|Der Name des Artikels, der dem Agent zugeordnet ist, der diese Warnung ausgelöst hat.|  
|**destination_object**|**sysname**|Der Name der der Warnung zugeordneten Abonnementtabelle.|  
|**source_object**|**sysname**|Der Name der der Warnung zugeordneten veröffentlichten Tabelle.|  
|**alert_error_text**|**ntext**|Der Text der Warnung.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
