---
description: sys.sysfiles (Transact-SQL)
title: sys.sysDateien (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysfiles
- sys.sysfiles_TSQL
- sys.sysfiles
- sysfiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfiles system table
- sys.sysfiles compatibility view
ms.assetid: 3b47f38d-1cff-404d-89d3-9342c451c802
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 862ffbdd0967068ef1b8345176edb4551e542196
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099067"
---
# <a name="syssysfiles-transact-sql"></a>sys.sysfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede Datei in einer Datenbank.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**FileID**|**smallint**|Datei-ID, die für jede Datenbank eindeutig ist.|  
|**groupID**|**smallint**|Dateigruppen-ID.|  
|**size**|**int**|Größe der Datei in Seiten mit einer Größe von 8 KB.|  
|**MaxSize**|**int**|Maximale Dateigröße in Seiten mit einer Größe von 8 KB.<br /><br /> 0 = Keine Vergrößerung.<br /><br /> -1 = Datei wird vergrößert, bis der Datenträger voll ist.<br /><br /> 268435456 = Protokolldatei wird bis zu einer maximalen Größe von 2 TB vergrößert.<br /><br /> Hinweis: Datenbanken, die mit einer unbegrenzten Protokolldatei Größe aktualisiert werden, melden-1 für die maximale Größe der Protokolldatei.|  
|**wachsen**|**int**|Zuwachsgröße für die Datenbank. Kann je nach Wert von **status** entweder die Seitenanzahl oder der Prozentsatz der Dateigröße sein.<br /><br /> 0 = Keine Vergrößerung.|  
|**status**|**int**|Statusbits für den **growth** -Wert in Megabyte (MB) oder Kilobyte (KB).<br /><br /> 0x2 = Datenträgerdatei.<br /><br /> 0x40 = Protokolldatei.<br /><br /> 0x100000 = Vergrößerung. Dieser Wert ist ein Prozentsatz (und nicht die Anzahl von Seiten).|  
|**Leistungs**|**int**|Reserviert.|  
|**name**|**sysname**|Logischer Name der Datei.|  
|**filename**|**nvarchar(260)**|Name des physischen Geräts. Der Name schließt den vollständigen Pfad der Datei ein.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
