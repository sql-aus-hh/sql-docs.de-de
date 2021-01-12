---
description: sys.trace_columns (Transact-SQL)
title: sys.trace_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.trace_columns
- trace_columns
- trace_columns_TSQL
- sys.trace_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_columns catalog view
ms.assetid: 5c48eb09-9e9b-45dd-b151-ca39b026ece5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 740f553e4809a5eb971bb7bfe7e97b5b628dfebd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094387"
---
# <a name="systrace_columns-transact-sql"></a>sys.trace_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **sys.trace_columns** -Katalog Sicht enthält eine Liste aller Spalten der Ablauf Verfolgungs Ereignisse. Diese Spalten ändern sich für eine bestimmte Version von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nicht.  
  
 Eine vollständige Liste der unterstützten Ablaufverfolgungsereignisse finden Sie unter [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen die Katalogsichten für erweiterte Ereignisse.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**trace_event_id**|**smallint**|Eindeutige ID dieser Spalte.|  
|**name**|**nvarchar(128)**|Eindeutiger Name dieser Spalte. Dieser Parameter ist nicht lokalisiert.|  
|**type_name**|**nvarchar(128)**|Datentypname dieser Spalte.|  
|**max_size**|**int**|Maximale Datengröße dieser Spalte in Bytes.|  
|**is_filterable**|**bit**|Gibt an, ob die Spalte in einer Filterdefinition verwendet werden kann.<br /><br /> 0 = false<br /><br /> 1 = TRUE|  
|**is_repeatable**|**bit**|Gibt an, ob in den Daten für wiederholte Spalten auf diese Spalte verwiesen werden kann.<br /><br /> 0 = false<br /><br /> 1 = TRUE|  
|**is_repeated_base**|**bit**|Gibt an, ob diese Spalte als eindeutiger Schlüssel für den Verweis auf wiederholte Daten verwendet werden kann.<br /><br /> 0 = false<br /><br /> 1 = TRUE|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys. Traces &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys.trace_categories &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_events &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys.trace_event_bindings &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-trace-event-bindings-transact-sql.md)   
 [sys.trace_subclass_values &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  
