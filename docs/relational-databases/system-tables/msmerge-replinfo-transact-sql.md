---
description: MSmerge_replinfo (Transact-SQL)
title: MSmerge_replinfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_replinfo_TSQL
- MSmerge_replinfo
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_replinfo system table
ms.assetid: b0924094-c0cc-49c1-869a-65be0d0465a0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6d7ba27eb3b01fcc34e5f53d954b12d120542116
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098619"
---
# <a name="msmerge_replinfo-transact-sql"></a>MSmerge_replinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_replinfo** Tabelle enthält eine Zeile für jedes Abonnement. In dieser Tabelle werden Informationen zu Abonnements nachverfolgt. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**repid**|**uniqueidentifier**|Die eindeutige ID für das Replikat.|  
|**use_interactive_resolver**|**bit**|Gibt an, ob der interaktive Konfliktlöser während der Abgleichung verwendet wird.<br /><br /> **0** = der interaktive Konflikt Löser wird nicht verwendet.<br /><br /> **1** = verwenden Sie den interaktiven Konflikt Löser.|  
|**validation_level**|**int**|Überprüfungstyp, der für das Abonnement durchgeführt wird. Die angegebene Überprüfungsebene kann einen der folgenden Werte haben:<br /><br /> **0** = keine Validierung.<br /><br /> **1** = nur Überprüfung der Zeilen Anzahl.<br /><br /> **2** = Überprüfung der Zeilen Anzahl und der Prüfsumme.<br /><br /> **3** = Überprüfung der Zeilen Anzahl und der binären Prüfsumme.|  
|**resync_gen**|**bigint**|Die Generierungsnummer, die für die erneute Synchronisierung des Abonnements verwendet wird. Der Wert **-1** gibt an, dass das Abonnement nicht für die erneute Synchronisierung markiert ist.|  
|**login_name**|**sysname**|Der Name des Benutzers, der das Abonnement erstellt hat.|  
|**hostname**|**sysname**|Der Wert, der beim Generieren der Partition für das Abonnement von den parametrisierten Zeilenfiltern verwendet wird.|  
|**merge_jobid**|**Binary (16)**|Die ID des Mergeauftrags für dieses Abonnement.|  
|**sync_info**|**int**|Nur intern verwendet.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
