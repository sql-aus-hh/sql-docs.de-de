---
description: sys.function_order_columns (Transact-SQL)
title: sys.function_order_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- function_order_columns
- sys.function_order_columns_TSQL
- function_order_columns_TSQL
- sys.function_order_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.function_order_columns catalog view
ms.assetid: 29287973-3125-4d35-8ca9-92cb45828854
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8a3408d1f4e7a32aafed3318d32f0a9ed7a97d3f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102755"
---
# <a name="sysfunction_order_columns-transact-sql"></a>sys.function_order_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro Spalte zurück, die Teil eines **Order** -Ausdrucks einer Common Language Runtime-Tabellenwert Funktion (CLR) ist.  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID des Objekts (CLR-Tabellenwertfunktion), das die Reihenfolge definiert.|  
|**order_column_id**|**int**|Die ID der Sortierspalte. **order_column_id** ist nur innerhalb von **object_id** eindeutig.<br /><br /> **order_column_id** stellt die Position dieser Spalte in der Sortierung dar.|  
|**column_id**|**int**|ID der Spalte in **object_id**.<br /><br /> **column_id** ist nur innerhalb von **object_id** eindeutig.|  
|**is_descending**|**bit**|1 = Die Sortierspalte hat eine absteigende Sortierreihenfolge.<br /><br /> 0 = Die Sortierspalte hat eine aufsteigende Sortierreihenfolge.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
