---
description: sys.spatial_reference_systems (Transact-SQL)
title: sys.spatial_reference_systems (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_reference_systems_TSQL
- sys.spatial_reference_systems_TSQL
- sys.spatial_reference_systems
- spatial_reference_systems
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_reference_systems catalog view
- spatial_reference_systems
ms.assetid: 3c9bc120-67c3-463f-9e24-29fd623f25a0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d968fc9ccb4d54e5345250477d72281152b29f68
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100182"
---
# <a name="sysspatial_reference_systems-transact-sql"></a>sys.spatial_reference_systems (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Listet die räumlichen Referenzsysteme (SRIDs) auf, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt werden.  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|spatial_reference_id|**int**|Das von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützte SRID.|  
|authority_name|**nvarchar(128)**|Die Autorität des SRID.|  
|authorized_spatial_reference_id|**int**|Der SRID, der von der in **authority_name** benannten Autorität angegeben wird.|  
|well_known_text|**nvarchar(4000)**|Die WKT-Darstellung des SRID.|  
|unit_of_measure|**nvarchar(128)**|Der Name der Maßeinheit.|  
|unit_conversion_factor|**float**|Die Länge der Maßeinheit in Metern.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
  
