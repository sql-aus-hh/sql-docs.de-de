---
title: MSmerge_generation_partition_mappings (T-SQL)
description: Beschreibt die MSmerge_generation_partition_mappings gespeicherte Prozedur, die zum Nachverfolgen von Änderungen an Partitionen in einer Mergeveröffentlichung verwendet wird.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_generation_partition_mappings_TSQL
- MSmerge_generation_partition_mappings
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_generation_partition_mappings system table
ms.assetid: 443a4024-ce48-4772-9ee5-95bd6fb6476b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e4bc8d5e833f5ca4c9c19c81985f744c497dfcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102054"
---
# <a name="msmerge_generation_partition_mappings-transact-sql"></a>MSmerge_generation_partition_mappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSmerge_generation_partition_mappings** Tabelle dient zum Nachverfolgen von Änderungen an Partitionen in einer Mergeveröffentlichung. Diese Tabelle wird in der Veröffentlichungs- und der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publication_number**|**smallint**|Identifiziert die Mergeveröffentlichung|  
|**Stro**|**bigint**|Der Generierungswert.|  
|**partition_id**|**int**|Identifiziert die Partition.|  
|**changecount**|**int**|Die Anzahl der Änderungen der Partition.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
