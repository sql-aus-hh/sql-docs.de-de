---
description: MSdistribution_agents (Transact-SQL)
title: MSdistribution_agents (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
author: cawrites
ms.author: chadam
ms.openlocfilehash: 20b2d5f012c33ea12d4b60ed3a8d44ee475f3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091481"
---
# <a name="msdistribution_agents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSdistribution_agents** Tabelle enthält eine Zeile für jede Verteilungs-Agent, die auf dem lokalen Verteiler ausgeführt wird. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Die ID des Verteilungs-Agents.|  
|**name**|**nvarchar (100)**|Der Name des Verteilungs-Agents.|  
|**publisher_database_id**|**int**|Die ID der Verlegerdatenbank.|  
|**publisher_id**|**smallint**|Die ID des Verlegers|  
|**publisher_db**|**sysname**|Der Name der Verlegerdatenbank.|  
|**ung**|**sysname**|Der Name der Veröffentlichung.|  
|**subscriber_id**|**smallint**|Die ID des Abonnenten; wird nur von bekannten Agents verwendet. Für anonyme Agents ist dies eine reservierte Spalte.|  
|**subscriber_db**|**sysname**|Der Name der Abonnementdatenbank|  
|**subscription_type**|**int**|Der Typ des Abonnements:<br /><br /> **0** = Push.<br /><br /> **1** = Pull.<br /><br /> **2** = anonym.|  
|**local_job**|**bit**|Zeigt an, ob sich ein Auftrag des SQL Server-Agents auf dem lokalen Verteiler befindet|  
|**job_id**|**Binary (16)**|Die Auftrags-ID|  
|**subscription_guid**|**Binary (16)**|Die ID der Abonnements dieses Agents|  
|**profile_id**|**int**|Die Konfigurations-ID aus der [MSagent_profiles &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) -Tabelle.|  
|**anonymous_subid**|**uniqueidentifier**|Die ID eines anonymen Agents.|  
|**subscriber_name**|**sysname**|Der Name des Abonnenten; wird nur von anonymen Agents verwendet|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**creation_date**|**datetime**|Datum und Uhrzeit der Erstellung des Verteilungs- oder Merge-Agents|  
|**queue_id**|**sysname**|Der Bezeichner für die Suche nach der Warteschlange für Abonnements mit verzögertem Update über eine Warteschlange. Für Abonnements ohne verzögertes Aktualisieren über eine Warteschlange ist der Wert gleich NULL. Für eine Veröffentlichung, die auf [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing basiert, ist der Wert ein GUID, der eindeutig die für das Abonnement zu verwendende Warteschlange bezeichnet. Bei SQL Server-basierten Warteschlangen Veröffentlichungen enthält die Spalte den Wert **SQL**.<br /><br /> Hinweis: die Verwendung von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing wurde als veraltet markiert und wird nicht mehr unterstützt.|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|Zeigt an, ob der Agent remote aktiviert werden kann.<br /><br /> der Wert **0** gibt an, dass der Agent nicht remote aktiviert werden kann.<br /><br /> der Wert **1** gibt an, dass der Agent Remote und auf dem Remote Computer aktiviert wird, der in der *offload_server* -Eigenschaft angegeben ist.|  
|**offload_server**|**sysname**|Der Netzwerkname des Servers, auf dem der Agent remote aktiviert werden soll|  
|**dts_package_name**|**sysname**|Der Name des DTS-Pakets. Geben Sie z. b. für ein Paket mit dem Namen **DTSPub_Package** an `@dts_package_name = N'DTSPub_Package'` .|  
|**dts_package_password**|**nvarchar (524)**|Das Kennwort für das Paket|  
|**dts_package_location**|**int**|Der Speicherort des Pakets. Der Speicherort des Pakets kann **Distributor** oder **Subscriber** sein.|  
|**sid**|**varbinary(85)**|Die Sicherheits-ID (SID) für den Verteilungs-Agent oder Merge-Agent, während er das erste Mal ausgeführt wird.|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|Der Sicherheitsmodus, der vom Agent beim Herstellen einer Verbindung mit dem Abonnenten verwendet wird, wobei die folgenden Werte möglich sind:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server Authentifizierung<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Authentifizierung.|  
|**subscriber_login**|**sysname**|Der Anmeldename, der verwendet wird, um eine Verbindung mit dem Abonnenten herzustellen.|  
|**subscriber_password**|**nvarchar (524)**|Der verschlüsselte Wert des Kennworts, das beim Herstellen einer Verbindung mit dem Abonnenten verwendet wird|  
|**reset_partial_snapshot_progress**|**bit**|Gibt an, ob eine teilweise heruntergeladene Momentaufnahme verworfen wird, damit der gesamte Momentaufnahmeprozess erneut gestartet werden kann|  
|**job_step_uid**|**uniqueidentifier**|Die eindeutige ID des Auftragsschritts des SQL Server-Agents, in dem der Agent gestartet wird|  
|**SubscriptionStreams**|**tinyint**|Legt die Anzahl der Verbindungen fest, die pro Verteilungs-Agent für die parallele Anwendung von Batches von Änderungen auf einen Abonnenten zulässig sind. Mögliche Werte zwischen 1 und 64 werden unterstützt.|  
|**memory_optimized**|**bit**|der Wert 1 gibt an, dass der Abonnent für Speicher optimierte Tabellen verwendet werden kann.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikationstabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
