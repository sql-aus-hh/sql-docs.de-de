---
description: sysmergesubscriptions (Transact-SQL)
title: sysmergeabonnements (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergesubscriptions_TSQL
- sysmergesubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubscriptions system table
ms.assetid: 6adc78da-991d-4c08-98c3-ecb4762e0e99
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3caa66b5e8e938a0e54f7212b37e0442e29ebfe
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090767"
---
# <a name="sysmergesubscriptions-transact-sql"></a>sysmergesubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jeden bekannten Abonnenten und stellt eine lokale Tabelle auf dem Verleger dar. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|subscriber_server|**sysname**|Die ID des Servers. Diese wird verwendet, um das Feld srvid dem serverspezifischen Wert zuzuordnen, wenn eine Kopie der Abonnementdatenbank auf einen anderen Server migriert wird.|  
|db_name|**sysname**|Der Name der abonnierenden Datenbank.|  
|pubid|**uniqueidentifier**|Die ID der Veröffentlichung, aus der das aktuelle Abonnement erstellt wurde.|  
|datasource_type|**int**|Der Typ der Datenquelle:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **2** = Jet-OLE DB.|  
|subid|**uniqueidentifier**|Die eindeutige ID für das Abonnement.|  
|replnickname|**binary**|Der komprimierte Spitzname für das Replikat.|  
|replicastate|**uniqueidentifier**|Ein eindeutiger Bezeichner, anhand dessen bestimmt wird, ob die vorhergehende Synchronisierung erfolgreich war. Dafür wird der Wert auf dem Verleger mit dem Wert auf dem Abonnenten verglichen.|  
|status|**tinyint**|Status des Abonnements:<br /><br /> **0** = inaktiv.<br /><br /> **1** = aktiv.<br /><br /> **2** = gelöscht.|  
|subscriber_type|**int**|Der Typ des Abonnenten:<br /><br /> **1** = Global.<br /><br /> **2** = lokal.<br /><br /> **3** = anonym.|  
|subscription_type|**int**|Der Typ des Abonnements:<br /><br /> **0** = Push.<br /><br /> **1** = Pull.<br /><br /> **2** = anonym.|  
|sync_type|**tinyint**|Typ der Synchronisierung:<br /><br /> **1** = automatisch.<br /><br /> **2** = keine Synchronisierung.|  
|description|**nvarchar(255)**|Kurze Beschreibung des Abonnements.|  
|priority|**real**|Gibt die Priorität des Abonnements an und lässt die Implementierung von prioritätsbasierten Routinen zur Konfliktlösung zu. Ist für alle lokalen oder anonymen Abonnements **0,00** .|  
|recgen|**bigint**|Die Nummer der zuletzt empfangenen Generierung.|  
|recguid|**uniqueidentifier**|Die eindeutige ID der zuletzt empfangenen Generierung.|  
|sentgen|**bigint**|Nummer der zuletzt gesendeten Generierung.|  
|sentguid|**uniqueidentifier**|Die eindeutige ID der zuletzt gesendeten Generierung.|  
|schemaversion|**int**|Die Nummer des zuletzt empfangenen Schemas.|  
|schemaguid|**uniqueidentifier**|Die eindeutige ID des zuletzt empfangenen Schemas.|  
|last_validated|**datetime**|Der **DateTime** -Wert der letzten erfolgreichen Überprüfung der Abonnentendaten.|  
|attempted_validate|**datetime**|Der letzte **DateTime** -Wert für die Überprüfung des Abonnements.|  
|last_sync_date|**datetime**|Der **DateTime** -Wert für die Synchronisierung.|  
|last_sync_status|**int**|Der Abonnementstatus:<br /><br /> **0** = alle Aufträge warten auf den Start.<br /><br /> **1** = mindestens ein Auftrag wird gestartet.<br /><br /> **2** = alle Aufträge wurden erfolgreich ausgeführt.<br /><br /> **3** = mindestens ein Auftrag wird ausgeführt.<br /><br /> **4** = alle Aufträge werden geplant und befinden sich im Leerlauf.<br /><br /> **5** = mindestens ein Auftrag versucht, nach einem vorherigen Fehler auszuführen.<br /><br /> **6** = mindestens ein Auftrag konnte nicht erfolgreich ausgeführt werden.|  
|last_sync_summary|**sysname**|Die Beschreibung der letzten Synchronisierungsergebnisse.|  
|metadatacleanuptime|**datetime**|Der letzte **DateTime** -Wert, zu dem abgelaufene Metadaten aus mergereplikationssystemtabellen entfernt wurden.|  
|partition_id|**int**|Identifiziert die vorausberechnete Partition, zu der das Abonnement gehört.|  
|cleanedup_unsent_changes|**bit**|Gibt an, dass Metadaten für nicht gesendete Änderungen auf dem Abonnenten bereinigt wurden.|  
|replica_version|**int**|Identifiziert die Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für den Abonnenten, zu dem das Abonnement gehört. Die folgenden Werte sind möglich:<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|supportability_mode|**int**|Nur zur internen Verwendung.|  
|application_name|**nvarchar(128)**|Nur zur internen Verwendung.|  
|subscriber_number|**int**|Nur zur internen Verwendung.|  
|last_makegeneration_datetime|**datetime**|Der letzte **DateTime** -Wert, den der makegeneration-Prozess für den Verleger ausgeführt hat. Weitere Informationen finden Sie unter dem-MakeGenerationInterval-Parameter in der [Replikations Merge-Agent](../../relational-databases/replication/agents/replication-merge-agent.md).|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikationstabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
