---
description: MSreplmonthresholdmetrics (Transact-SQL)
title: Msreplmon-oldmetrics (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- msreplmonthresholdmetrics_TSQL
- msreplmonthresholdmetrics
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplmonthresholdmetrics system table
ms.assetid: 0cc9b40a-36ce-485b-9bc2-d4abd5aa6727
author: cawrites
ms.author: chadam
ms.openlocfilehash: e8d7d5c1ea9bbf421632e1d742cb316fd2c94cd2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094771"
---
# <a name="msreplmonthresholdmetrics-transact-sql"></a>MSreplmonthresholdmetrics (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Mithilfe der **MSreplmonthresholdmetrics** -Tabelle werden die für die Überwachung der Replikation bereitgestellten metrischen Daten definiert. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**metric_id**|**int**|Identifiziert eine Replikationsleistungsmetrik. Einer der folgenden Werte ist möglich.<br /><br /> **1** = Ablauf<br /><br /> **2** = Latenz<br /><br /> **4** = mergeablauf<br /><br /> **5** = mergeslowrunduration<br /><br /> **6** = mergefastrunduration<br /><br /> **7** = mergefastrunspeed<br /><br /> **8** = mergeslowrunspeed|  
|**title**|**sysname**|Der Name der Replikationsleistungsmetrik|  
|**warningbitstatus**|**int**|Der bitweise Bezeichner, der zum Bereitstellen einer Warnung bei einer Schwellenwertverletzung für die folgenden metrischen Daten verwendet wird:<br /><br /> **1** = Ablauf: ein Abonnement für eine Transaktions Veröffentlichung hat die Beibehaltungs Dauer um mehr als den zulässigen Schwellenwert überschritten (als Prozentsatz der Beibehaltungs Dauer).<br /><br /> **2** = Latenz: die Zeit, die zum Replizieren von Daten von einem transaktionalen Verleger auf den Abonnenten benötigt wird, überschreitet den Schwellenwert (in Sekunden).<br /><br /> **4** = mergeablauf: ein Abonnement für eine Mergeveröffentlichung hat die Beibehaltungs Dauer um mehr als den zulässigen Schwellenwert überschritten, als Prozentsatz der Beibehaltungs Dauer.<br /><br /> **8** = mergefastrauunduration: die Zeit, die zum Abschließen der Synchronisierung eines Mergeabonnements benötigt wird, überschreitet den Schwellenwert über eine schnelle Netzwerkverbindung (in Sekunden).<br /><br /> **16** = mergeslowrunduration: die Zeit für die Synchronisierung eines Mergeabonnements überschreitet den Schwellenwert über eine langsame oder DFÜ-Netzwerkverbindung (in Sekunden).<br /><br /> **32** = mergefastrauunspeed-die Übermittlungs Rate für Zeilen während der Synchronisierung eines Mergeabonnements konnte den Schwellenwert (in Zeilen pro Sekunde) über eine schnelle Netzwerkverbindung nicht aufrechterhalten.<br /><br /> **64** = mergeslowrunspeed-die Übermittlungs Rate für Zeilen während der Synchronisierung eines Mergeabonnements konnte den Schwellenwert (in Zeilen pro Sekunde) über eine langsame oder DFÜ-Netzwerkverbindung nicht aufrechterhalten.|  
|**alertmessageid**|**int**|Die ID der Fehlermeldung, die angezeigt wird, wenn die Schwellenwert-Warnungsbedingung auftritt|  
|**description**|**nvarchar (3000)**|Die Beschreibung der Replikationsleistungsmetrik|  
|**default_value**|**sql_variant**|Ein Standardwert für die Replikationsleistungsmetrik|  
|**min_value**|**sql_variant**|Der Mindestwert für eine gebundene Replikationsleistungsmetrik|  
|**max_value**|**sql_variant**|Der Höchstwert für eine gebundene Replikationsleistungsmetrik|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
