---
description: MSpublications (Transact-SQL)
title: MSpublications (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpublications
- MSpublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublications system table
ms.assetid: 7a0b3457-7265-4f24-a255-7f055d908f20
author: cawrites
ms.author: chadam
ms.openlocfilehash: 089270c4c95b35e25f44339a7f001b5ac194ec01
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098174"
---
# <a name="mspublications-transact-sql"></a>MSpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSpublications** -Tabelle enthält eine Zeile für jede Veröffentlichung, die von einem Verleger repliziert wird. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Die ID des Verlegers|  
|**publisher_db**|**sysname**|Der Name der Verlegerdatenbank.|  
|**ung**|**sysname**|Der Name der Veröffentlichung.|  
|**publication_id**|**int**|Die ID der Veröffentlichung.|  
|**publication_type**|**int**|Der Typ der Veröffentlichung:<br /><br /> **0** = transaktional.<br /><br /> **1** = Momentaufnahme.<br /><br /> **2** = Merge.|  
|**thirdparty_flag**|**bit**|Gibt an, ob eine Veröffentlichung eine [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbank ist:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **1** = andere Datenquelle als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**independent_agent**|**bit**|Zeigt an, ob ein Verteilungs-Agent im Einzelplatzmodus für diese Veröffentlichung vorhanden ist.|  
|**immediate_sync**|**bit**|Zeigt an, ob bei jeder Ausführung des Momentaufnahme-Agents Synchronisierungsdateien erstellt oder neu erstellt werden.|  
|**allow_push**|**bit**|Zeigt an, ob für die angegebene Veröffentlichung Pushabonnements erstellt werden können.|  
|**allow_pull**|**bit**|Zeigt an, ob für die angegebene Veröffentlichung Pullabonnements erstellt werden können.|  
|**allow_anonymous**|**bit**|Zeigt an, ob für die angegebene Veröffentlichung anonyme Abonnements erstellt werden können.|  
|**description**|**nvarchar(255)**|Die Beschreibung der Veröffentlichung.|  
|**vendor_name**|**nvarchar (100)**|Der Name des Herstellers, wenn der Verleger keine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank ist.|  
|**zurück**|**int**|Die Beibehaltungsdauer der Veröffentlichung (in Stunden).|  
|**sync_method**|**int**|Die Synchronisierungsmethode:<br /><br /> **0** = System eigen (erzeugt eine Massen Kopier Ausgabe aller Tabellen im einheitlichen Modus).<br /><br /> **1** = Zeichen (erzeugt eine Massen Kopier Ausgabe aller Tabellen im Zeichenmodus).<br /><br /> **3** = gleichzeitig (erzeugt eine Massen Kopier Ausgabe aller Tabellen im einheitlichen Modus, sperrt jedoch die Tabelle während der Momentaufnahme nicht).<br /><br /> **4** = Concurrent_c (erzeugt eine Massen Kopier Ausgabe aller Tabellen im Zeichenmodus, sperrt jedoch die Tabelle während der Momentaufnahme nicht)<br /><br /> Die Werte **3** und **4** stehen für die Transaktions Replikation und Mergereplikation zur Verfügung, jedoch nicht für die Momentaufnahme Replikation.|  
|**allow_subscription_copy**|**bit**|Aktiviert oder deaktiviert die Option zum Kopieren der Abonnementdatenbanken, die diese Veröffentlichung abonniert haben. **0** bedeutet, dass das Kopieren deaktiviert ist, und **1** bedeutet, dass es aktiviert ist.|  
|**thirdparty_options**|**int**|Gibt an, ob die Anzeige einer Veröffentlichung im Ordner Replikation unter [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] drückt wird:<br /><br /> **0** = eine heterogene Veröffentlichung im Ordner Replikation in anzeigen [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> **1** = unterdrücken Sie die Anzeige einer heterogenen Veröffentlichung im Ordner Replikation in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**allow_queued_tran**|**bit**|Gibt an, ob die Veröffentlichung verzögerte Updates über eine Warteschlange zulässt:<br /><br /> **0 =** Die Veröffentlichung ist nicht in die Warteschlange eingereiht.<br /><br /> **1** = Veröffentlichung wird in die Warteschlange eingereiht.|  
|**options**|**int**|Für diese Version sind keine Informationen verfügbar.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
