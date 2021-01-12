---
description: MSmerge_metadataaction_request (Transact-SQL)
title: MSmerge_metadataaction_request (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_metadataaction_request
- MSmerge_metadataaction_request_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_metadataaction_request system table
ms.assetid: cd31a114-900a-4218-ab58-d959e547c647
author: cawrites
ms.author: chadam
ms.openlocfilehash: ceef4e122ced8e8f14bdc52cae86ef358f9c3a6e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102034"
---
# <a name="msmerge_metadataaction_request-transact-sql"></a>MSmerge_metadataaction_request (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_metadataaction_request** Tabelle speichert eine Zeile für jede erforderliche kompensierende Aktion. Wenn bei der Websynchronisierung ein Fehler auftritt und die Synchronisierung erneut versucht werden muss, wird ein Eintrag in **MSmerge_metadataaction_request** erstellt. Während der Uploadphase der nachfolgenden Zusammenführung werden Anforderungen zum Synchronisieren aller zur Veröffentlichung gehörenden Artikel von dieser Tabelle abgerufen und hochgeladen. Wenn die Synchronisierung erfolgreich abgeschlossen wurde, wird die entsprechende Zeile in der **MSmerge_metadataaction_request** Tabelle gelöscht. Diese Tabelle wird auf dem Verleger in der Veröffentlichungsdatenbank und auf dem Abonnenten in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Der Spitzname der veröffentlichten Tabelle.|  
|**ROWGUID**|**uniqueidentifier**|Der Zeilenbezeichner für die angegebene Zeile.|  
|**action**|**tinyint**|Bezeichnet die erforderliche kompensierende Aktion.|  
|**Stro**|**bigint**|Der Wert der Generierung, für die die kompensierende Aktion benötigt wird.|  
|**tes**|**int**|Nur intern verwendet.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [Websynchronisierung für die Mergereplikation](../../relational-databases/replication/web-synchronization-for-merge-replication.md)  
  
  
