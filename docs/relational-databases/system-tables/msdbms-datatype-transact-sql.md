---
description: MSdbms_datatype (Transact-SQL)
title: MSdbms_datatype (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype
- MSdbms_datatype_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype system table
ms.assetid: 606168cc-79a8-442f-ab43-936f8f884d72
author: cawrites
ms.author: chadam
ms.openlocfilehash: daf45f65ddc0e7ea496a60a3da8b9fc9d270fc73
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098214"
---
# <a name="msdbms_datatype-transact-sql"></a>MSdbms_datatype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **MSdbms_datatype** Tabelle enthält die komplette Liste der systemeigenen Datentypen an jedem unterstützten Datenbank-Management System (DBMS), das bei der heterogenen Daten Bank Replikation entweder als Verleger oder Abonnent verwendet wird. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**datatype_id**|**int**|Identifiziert jeden eindeutigen Datentyp.|  
|**dbms_id**|**int**|Identifiziert das DBMS, zu dem der Typ gehört.|  
|**type**|**sysname**|Der Name des Datentyps (systemeigen).|  
|**"kreateparameams"**|**int**|Das Bitmap, das beschreibt, welche Kombination aus Länge, Genauigkeit und Dezimalstellen für jeden Datentyp gilt:<br /><br /> **0x1** = Genauigkeit.<br /><br /> **0x2** = skalieren.<br /><br /> **0x4** = Länge.|  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Tabelle enthält Einträge für SQL Server-Datentypen, weil eine Instanz von SQL Server eine Nicht-SQL Server-Datenbank abonnieren und in einem Nicht-SQL Server-Abonnenten veröffentlichen kann.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Heterogene Datenbankreplikation](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Angeben von Datentyp Zuordnungen für einen Oracle-Verleger](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikationssichten &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
