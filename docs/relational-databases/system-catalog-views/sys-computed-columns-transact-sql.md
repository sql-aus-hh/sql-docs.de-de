---
description: sys.computed_columns (Transact-SQL)
title: sys.computed_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.computed_columns_TSQL
- sys.computed_columns
- computed_columns_TSQL
- computed_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.computed_columns catalog view
ms.assetid: c962c619-e18f-4315-9251-8d9862462299
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7dbe7e9176605dea875e1846b72e1f00301be55
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100701"
---
# <a name="syscomputed_columns-transact-sql"></a>sys.computed_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jede Spalte, die in **sys. Columns** gefunden wird, die eine berechnete Spalte ist.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**\<Inherited columns>**||In der **sys.computed_columns** Sicht werden alle Spalten in der **sys. Columns** -Sicht zurückgegeben. Darüber hinaus werden die unten beschriebenen, zusätzlichen Spalten zurückgegeben. Eine Beschreibung der Spalten, die die **sys.computed_columns** Sicht von **sys. Columns** erbt, finden Sie unter [sys. Columns &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md). Der Wert der Spalte **IS_COMPUTED** wird in der **sys.computed_columns** Sicht immer auf 1 festgelegt.|  
|**Definition**|**nvarchar(max)**|SQL-Text, der diese berechnete Spalte definiert.|  
|**uses_database_collation**|**bit**|1 = Die Spaltendefinition hängt von der richtigen Auswertung der Standardsortierung der Datenbank ab, andernfalls ist der Wert 0. Durch diese Abhängigkeit wird verhindert, dass die Standardsortierung der Datenbank geändert wird.|  
|**is_persisted**|**bit**|Die berechnete Spalte ist persistent.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
