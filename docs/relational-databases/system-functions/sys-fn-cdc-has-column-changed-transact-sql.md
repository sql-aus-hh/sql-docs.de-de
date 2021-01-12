---
description: sys.fn_cdc_has_column_changed (Transact-SQL)
title: sys.fn_cdc_has_column_changed (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_has_column_changed_TSQL
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed_TSQL
- fn_cdc_has_column_changed
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed
ms.assetid: 2b9e6278-050d-4ffc-8d1a-09606180facc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e835647f387b4980a50a0f49e5c6bd967fe553f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099672"
---
# <a name="sysfn_cdc_has_column_changed-transact-sql"></a>sys.fn_cdc_has_column_changed (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Identifiziert, ob die angegebene Updatemaske darauf hinweist, dass die angegebene Spalte in der zugeordneten Änderungszeile aktualisiert wurde.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_cdc_has_column_changed ( 'capture_instance','column_name' , update_mask )  
```  
  
## <a name="arguments"></a>Argumente  
 **"** *capture_instance* **"**  
 Der Name der Aufzeichnungsinstanz. *capture_instance* ist vom **Datentyp vom Datentyp sysname**.  
  
 **"** *column_name* **"**  
 Die aufgezeichnete Spalte der angegebenen Aufzeichnungsinstanz, für die ein Bericht erstellt werden soll. *column_name* ist vom **Datentyp vom Datentyp sysname**.  
  
 *update_mask*  
 Die Maske zur Identifizierung aktualisierter Spalten in einer zugeordneten Änderungszeile. *update_mask* ist **varbinary(128)**  
  
## <a name="return-type"></a>Rückgabetyp  
 **bit**  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können diese Funktion zum Extrahieren von Informationen aus einer Updatemaske verwenden, die in einer Abfrage nach Änderungsdaten zurückgegeben wurde. Sie empfiehlt sich besonders dann, wenn Sie eine Updatemaske nachgelagert verarbeiten und wissen müssen, ob eine bestimmte Spalte in der zugeordneten Änderungszeile geändert wurde. Weitere Informationen finden Sie unter [Informationen zu Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
 Wenn diese Informationen als Teil einer Änderungs Datenabfrage zurückgegeben werden, empfiehlt es sich, die Funktionen [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) und [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) anstelle dieser Funktion zu verwenden. Verwenden Sie die Funktion fn_cdc_get_column_ordinal vor dem Abfragen von Änderungs Daten, sodass die gewünschte Spalten Ordnungszahl nur einmal berechnet wird. Verwenden Sie fn_cdc_is_bit_set in der Abfrage, um die Informationen für jede zurückgegebene Zeile aus der Update Maske zu extrahieren.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle sysadmin oder in der festen Datenbankrolle db_owner. Für alle anderen Benutzer ist die SELECT-Berechtigung für alle aufgezeichneten Spalten in der Quelltabelle und, wenn eine Gatingrolle für die Aufzeichnungsinstanz definiert wurde, eine Mitgliedschaft in dieser Datenbankrolle erforderlich.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CDC. &#60;capture_instance&#62;_CT &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)   
 [cdc.captured_columns &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
  
  
