---
description: sys.systypes (Transact-SQL)
title: sys.sysTypen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.systypes_TSQL
- systypes_TSQL
- sys.systypes
- systypes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.systypes compatibility view
- systypes system table
ms.assetid: 1b0b1d0c-5f7b-470b-bd52-8bfa922d7889
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b00fc15f26b54acf7de52684949473c0bbd6ed19
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095350"
---
# <a name="syssystypes-transact-sql"></a>sys.systypes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden vom System bereitgestellten und jeden benutzerdefinierten Datentyp zurück, der in der Datenbank definiert ist.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Name des Datentyps.|  
|**xType**|**tinyint**|Physischer Speichertyp.|  
|**status**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|Erweiterter Benutzertyp. Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl der Datentypen 32.767 übersteigt.|  
|**length**|**smallint**|Physische Länge des Datentyps.|  
|**xprec**|**tinyint**|Vom Server verwendete interne Genauigkeit. Darf in Abfragen nicht verwendet werden.|  
|**xscale**|**tinyint**|Vom Server verwendete interne Dezimalstellen. Darf in Abfragen nicht verwendet werden.|  
|**tdefault**|**int**|ID der gespeicherten Prozedur zur Integritätsprüfung für diesen Datentyp.|  
|**-**|**int**|ID der gespeicherten Prozedur zur Integritätsprüfung für diesen Datentyp.|  
|**UID**|**smallint**|Schema-ID des Typbesitzers.<br /><br /> Bei Datenbanken, die von einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]aktualisiert wurden, ist die Schema-ID gleich der Benutzer-ID des Besitzers.<br /><br /> Wichtig Wenn Sie eine der folgenden DDL-Anweisungen verwenden, müssen Sie die **\* sys. types-Katalog Sicht anstelle vonsys.sys-Typen verwenden. \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) <br /><br /> ALTER AUTHORIZATION ON TYPE<br /><br /> CREATE TYPE<br /><br /> Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl von Benutzern und Rollen 32.767 übersteigt.|  
|**bleiben**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**collationid**|**int**|Wenn Zeichen basiert, ist **collationid** die ID der Sortierung der aktuellen Datenbank. Andernfalls ist der Wert NULL.|  
|**usertype**|**smallint**|User type ID. Führt zu einem Überlauf oder gibt NULL zurück, wenn die Anzahl der Datentypen 32.767 übersteigt.|  
|**variable**|**bit**|Datentyp mit variabler Länge.<br /><br /> 1 = True<br /><br /> 0 = False|  
|**allownulls**|**bit**|Zeigt die Standard-NULL-Zulässigkeit für diesen Datentyp an. Dieser Standardwert wird von überschrieben, wenn die NULL-Zulässigkeit mithilfe von [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) oder [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)angegeben wird.|  
|**type**|**tinyint**|Physischer Speicherdatentyp.|  
|**printfmt**|**varchar (255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Genauigkeitsgrad für diesen Datentyp.<br /><br /> -1 = **XML** oder große Werttypen.|  
|**scale**|**tinyint**|Dezimalstellen für diesen Datentyp (basierend auf der Genauigkeit).<br /><br /> NULL = Datentyp nicht numerisch.|  
|**Sortierung**|**sysname**|Wenn Zeichen basiert, ist **COLLATIONS** die Sortierung der aktuellen Datenbank. Andernfalls ist der Wert NULL.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Kompatibilitäts Sichten &#40;Transact-SQL-&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Zuordnung von Systemtabellen zu System Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
