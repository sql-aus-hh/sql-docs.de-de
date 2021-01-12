---
description: sys.sysaltfiles (Transact-SQL)
title: sys.sysaltfiles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysaltfiles_TSQL
- sys.sysaltfiles
- sysaltfiles_TSQL
- sysaltfiles
dev_langs:
- TSQL
helpviewer_keywords:
- sysaltfiles system table
- sys.sysaltfiles compatibility view
ms.assetid: 698dec23-5336-4108-87a5-f8e407f8da09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f6139fb221ab1d960d8cfb455e592ac16fbcee09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097799"
---
# <a name="syssysaltfiles-transact-sql"></a>sys.sysaltfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält unter bestimmten Umständen Zeilen, die den Dateien in einer Datenbank entsprechen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**FileID**|**smallint**|Datei-ID. Diese ist für jede Datenbank eindeutig.|  
|**groupID**|**smallint**|Dateigruppen-ID.|  
|**size**|**int**|Dateigröße in Seiten mit einer Größe von 8 KB.|  
|**MaxSize**|**int**|Maximale Dateigröße in Seiten mit einer Größe von 8 KB.<br /><br /> 0 = Keine Vergrößerung.<br /><br /> -1 = Datei wird vergrößert, bis der Datenträger voll ist.<br /><br /> 268435456 = Protokolldatei wird bis zu einer maximalen Größe von 2 TB vergrößert.<br /><br /> Hinweis: Datenbanken, die mit einer unbegrenzten Protokolldatei Größe aktualisiert werden, melden-1 für die maximale Größe der Protokolldatei.|  
|**wachsen**|**int**|Zuwachsgröße für die Datenbank.<br /><br /> 0 = Keine Vergrößerung. Kann je nach dem Wert von status entweder die Seitenanzahl oder der Prozentsatz der Dateigröße sein. Falls **status** 0x100000 ist, entspricht **growth** dem Prozentsatz der Dateigröße. Andernfalls handelt es sich um die Anzahl von Seiten.|  
|**status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**Leistungs**|**int**|Reserviert.|  
|**dbid**|**smallint**|Datenbank-ID der Datenbank, zu der diese Datei gehört.|  
|**name**|**sysname**|Logischer Name der Datei.|  
|**filename**|**nvarchar(260)**|Name des physischen Geräts. Der Name schließt den vollständigen Pfad der Datei ein.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
