---
description: MSrepl_queuedtraninfo (Transact-SQL)
title: MSrepl_queuedtraninfo (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSrepl_queuedtraninfo_TSQL
- MSrepl_queuedtraninfo
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_queuedtraninfo system table
ms.assetid: af7a5baf-32ea-475f-b6b9-68c557b4980c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 957314a333b26cec5e73758a4d4c2826770420a9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102723"
---
# <a name="msrepl_queuedtraninfo-transact-sql"></a>MSrepl_queuedtraninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSreplication_queuedtraninfo** Tabelle wird vom Replikationsprozess zum Speichern von Informationen zu den in der Warteschlange stehenden Befehlen verwendet, die von allen Abonnements mit verzögertem Update über eine Warteschlange ausgegeben werden Diese Tabelle wird in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Der Name des Verlegers.|  
|**publisher_db**|**sysname**|Der Name der Veröffentlichungs Datenbank.|  
|**ung**|**sysname**|Der Name der Veröffentlichung.|  
|**tranid**|**sysname**|Die Transaktions-ID, unter der der Befehl in der Warteschlange ausgeführt wurde|  
|**maxorderkey**|**bigint**|Nur intern verwendet.|  
|**commandcount**|**bigint**|Nur intern verwendet.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
