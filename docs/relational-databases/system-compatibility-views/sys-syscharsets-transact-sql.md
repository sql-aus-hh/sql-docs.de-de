---
description: sys.syscharsets (Transact-SQL)
title: sys.sysZeichensätze (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscharsets
- syscharsets
- sys.syscharsets_TSQL
- syscharsets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscharsets system table
- sys.syscharsets compatibility view
ms.assetid: f16d987c-bd19-4668-9ef7-785b8fb9ff5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 081ab46f57f78ed4e41f3a25050e9ba428c37507
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097791"
---
# <a name="syssyscharsets-transact-sql"></a>sys.syscharsets (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jeden Zeichensatz und jede Sortierreihenfolge, die für die Verwendung in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]definiert sind. Eine der Sortierreihenfolgen ist in **sysconfigures** als Standard-Sortierreihenfolge markiert. Dies ist die einzige Sortierreihenfolge, die tatsächlich verwendet wird.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**type**|**smallint**|Typ der Entität, die diese Zeile darstellt:<br /><br /> 1001 = Zeichensatz<br /><br /> 2001 = Sortierreihenfolge|  
|**id**|**tinyint**|Eindeutige ID für den Zeichensatz oder die Sortierreihenfolge. Beachten Sie, dass Sortierreihenfolgen und Zeichensätze nicht dieselbe ID haben können. Der ID-Bereich von 1 bis 240 ist für die Verwendung in [!INCLUDE[ssDE](../../includes/ssde-md.md)]reserviert.|  
|**CSID**|**tinyint**|Wenn die Zeile einen Zeichensatz darstellt, ist dieses Feld ungenutzt. Wenn die Zeile eine Sortierreihenfolge darstellt, ist dieses Feld die ID des Zeichensatzes, auf dem die Sortierreihenfolge aufgebaut ist. Es wird davon ausgegangen, dass eine Zeichensatzzeile mit dieser ID in dieser Tabelle vorhanden ist.|  
|**status**|**smallint**|Bits für Informationen über den internen Systemstatus.|  
|**name**|**sysname**|Eindeutiger Name für den Zeichensatz oder die Sortierreihenfolge. Dieses Feld darf nur die Buchstaben A-Z oder a-z, die Zahlen 0-9 und Unterstriche (_) enthalten; es muss außerdem mit einem Buchstaben beginnen.|  
|**description**|**nvarchar(255)**|Optionale Beschreibung der Funktionen des Zeichensatzes oder der Sortierreihenfolge.|  
|**binarydefinition**|**varbinary (6000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**Definition**|**image**|Interne Definition des Zeichensatzes oder der Sortierreihenfolge. Die Struktur der Daten in diesem Feld hängt vom Typ ab.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
