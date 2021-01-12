---
description: syssubscriptions (Transact-SQL)
title: sysabonnements (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- syssubscriptions_TSQL
- syssubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- syssubscriptions system table
ms.assetid: 106c1707-e0e0-49b4-ba50-25380c40fab2
author: cawrites
ms.author: chadam
ms.openlocfilehash: ff1d234e66077e5658099848ca9371905bb8ae22
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102665"
---
# <a name="syssubscriptions-transact-sql"></a>syssubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jedes in der Datenbank definierte Abonnement. Diese Tabelle wird in der Veröffentlichungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Die eindeutige ID eines Artikels.|  
|**srvid**|**smallint**|Die Server-ID des Abonnenten.|  
|**dest_db**|**sysname**|Der Name der Zieldatenbank.|  
|**status**|**tinyint**|Status des Abonnements:<br /><br /> **0** = inaktiv.<br /><br /> **1** = abonniert.<br /><br /> **2** = aktiv.|  
|**sync_type**|**tinyint**|Der Typ der Erstsynchronisierung:<br /><br /> **1** = automatisch.<br /><br /> **2** = keine|  
|**login_name**|**sysname**|Der Anmeldename, der beim Hinzufügen des Abonnements verwendet wird.|  
|**subscription_type**|**int**|Der Typ des Abonnements:<br /><br /> 0 = Push - der Verteilungs-Agent wird auf dem Verteiler ausgeführt.<br /><br /> 1 = Pull - der Verteilungs-Agent wird auf dem Abonnenten ausgeführt.|  
|**distribution_jobid**|**Binary (16)**|Die Auftrags-ID der Verteilungs-Agent.|  
|**timestamp**|**timestamp**|Der Zeitstempel.|  
|**update_mode**|**tinyint**|Updatemodus:<br /><br /> **0** = schreibgeschützt.<br /><br /> **1** = sofortige Aktualisierung.|  
|**loopback_detection**|**bit**|Gilt für Abonnements, die Teil einer bidirektionalen Transaktionsreplikationstopologie sind. Bestimmt, ob der Verteilungs-Agent Transaktionen des Abonnenten zurück an den Abonnenten sendet:<br /><br /> **0** = sendet zurück.<br /><br /> **1** = sendet nicht zurück.|  
|**queued_reinit**|**bit**|Gibt an, ob der Artikel für die Initialisierung oder erneute Initialisierung markiert ist. Der Wert **1** gibt an, dass der abonnierte Artikel für die Initialisierung oder erneute Initialisierung markiert ist.|  
|**nosync_type**|**tinyint**|Der Typ der Abonnementinitialisierung:<br /><br /> **0** = automatisch (Momentaufnahme)<br /><br /> **1** = nur Replikations Unterstützung<br /><br /> **2** = mit Sicherung initialisieren<br /><br /> **3** = Initialisieren von der Protokoll Folge Nummer (Log Sequence Number, LSN)<br /><br /> Weitere Informationen finden Sie unter dem **\@ sync_type** -Parameter von [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).|  
|**srvname**|**sysname**|Den Namen des Abonnenten.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
