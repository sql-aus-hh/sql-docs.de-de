---
description: sys.sysfilegroups (Transact-SQL)
title: sys.sys-Dateigruppen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysfilegroups_TSQL
- sys.sysfilegroups
- sysfilegroups
- sys.sysfilegroups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfilegroups system table
- sys.sysfilegroups compatibility view
ms.assetid: e567fa07-31cd-43cc-b8c7-ba6108baca80
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 609563f4998215367649a33cfc5136b8edd0e26b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099107"
---
# <a name="syssysfilegroups-transact-sql"></a>sys.sysfilegroups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Dateigruppe in einer Datenbank. Es gibt mindestens einen Eintrag für die primäre Dateigruppe in dieser Tabelle.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**groupID**|**smallint**|Gruppen-ID, die für jede Datenbank eindeutig ist.|  
|**allocpolicy**|**smallint**|Reserviert|  
|**status**|**int**|0x8 = Schreibgeschützt<br /><br /> 0x10 = Standard|  
|**GroupName**|**sysname**|Name der Dateigruppe.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
