---
description: sys.syscolumns (Transact-SQL)
title: sys.sysSpalten (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-enginel, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.syscolumns
- sys.syscolumns_TSQL
- syscolumns_TSQL
- syscolumns
dev_langs:
- TSQL
helpviewer_keywords:
- syscolumns system table
- sys.syscolumns compatibility view
ms.assetid: 863fd87b-ff33-4ac5-9aa9-df21140681da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9ecd4982135135fbf23c700a30318d86026d266
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099200"
---
# <a name="syssyscolumns-transact-sql"></a>sys.syscolumns (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Gibt jeweils eine Zeile für die einzelnen Spalten aller Tabellen und Sichten sowie eine Zeile für jeden Parameter einer gespeicherten Prozedur in der Datenbank zurück.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Name der Spalte oder des Prozedurparameters|  
|**id**|**int**|Objekt-ID der Tabelle, zu der diese Spalte gehört, oder ID der gespeicherten Prozedur, der dieser Parameter zugeordnet ist|  
|**xType**|**tinyint**|Physischer Speichertyp aus **sys. types**.|  
|**typestat**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|ID des erweiterten benutzerdefinierten Datentyps. Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl der Datentypen 32.767 übersteigt.|  
|**length**|**smallint**|Maximale physische Speicherlänge aus **sys**.**types**.|  
|**xprec**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**ColId**|**smallint**|Spalten- oder Parameter-ID|  
|**xoffset**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bleiben**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|ID des Standards für diese Spalte|  
|**-**|**int**|ID der Regel oder CHECK-Einschränkung für diese Spalte|  
|**Zahl**|**smallint**|Nummer der Unterprozedur, wenn die Prozedur gruppiert ist.<br /><br /> 0 = Einträge, die sich nicht auf eine Prozedur beziehen.|  
|**colorder**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**varbinary(8000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**smallint**|Offset in die Zeile, in der diese Spalte enthalten ist|  
|**collationid**|**int**|ID der Sortierung der Spalte. NULL für nicht zeichenbasierte Spalten.|  
|**status**|**tinyint**|Bitmuster, das zum Beschreiben einer Eigenschaft der Spalte oder des Parameters verwendet wird:<br /><br /> 0x08 = In der Spalte sind NULL-Werte zulässig.<br /><br /> 0x10 = ANSI-Auffüllung war aktiviert, als **varchar** oder **varbinary** -Spalten hinzugefügt wurden. Nachfolgende Leerzeichen werden bei **varchar** -Spalten beibehalten, nachfolgende Nullen werden bei **varbinary** -Spalten beibehalten.<br /><br /> 0x40 = Der Parameter ist ein OUTPUT-Parameter.<br /><br /> 0x80 = Die Spalte ist eine Identitätsspalte.|  
|**type**|**tinyint**|Physischer Speichertyp aus **sys**.**types**.|  
|**usertype**|**smallint**|ID des benutzerdefinierten Datentyps aus **sys.types**. Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl der Datentypen 32.767 übersteigt.|  
|**printfmt**|**varchar (255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Genauigkeitsgrad für diese Spalte.<br /><br /> -1 = **xml** oder ein Typ für hohe Werte.|  
|**scale**|**int**|Dezimalstellen in dieser Spalte.<br /><br /> NULL = Datentyp nicht numerisch.|  
|**IsComputed**|**int**|Flag, das anzeigt, ob die Spalte berechnet ist:<br /><br /> 0 = Nicht berechnet<br /><br /> 1 = Berechnet|  
|**isoutparam**|**int**|Gibt an, ob der Prozedurparameter ein Ausgabeparameter ist.<br /><br /> 1 = True<br /><br /> 0 = False|  
|**IsNullable**|**int**|Gibt an, ob die Spalte NULL-Werte zulässt.<br /><br /> 1 = True<br /><br /> 0 = False|  
|**Sortierung**|**sysname**|Name der Sortierung der Spalte. NULL, wenn es keine zeichenbasierte Spalte ist.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Kompatibilitätssichten &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
