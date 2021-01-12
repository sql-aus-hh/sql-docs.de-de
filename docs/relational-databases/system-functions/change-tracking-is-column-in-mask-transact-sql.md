---
description: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
title: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_IS_COLUMN_IN_MASK_TSQL
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], CHANGE_TRACKING_IS_COLUMN_IN_MASK
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
ms.assetid: 649b370b-da54-4915-919d-1b597a39d505
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a9d5ee6c44e0b0ca9544ee4a32b7cbd455497fe6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095010"
---
# <a name="change_tracking_is_column_in_mask-transact-sql"></a>CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Interpretiert den SYS_CHANGE_COLUMNS Wert, der von der CHANGETABLE (changes...)-Funktion zurückgegeben wird. Dies ermöglicht es einer Anwendung zu ermitteln, ob die angegebene Spalte in den Werten enthalten ist, die für SYS_CHANGE_COLUMNS zurückgegeben werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
CHANGE_TRACKING_IS_COLUMN_IN_MASK ( column_id , change_columns )  
```  
  
## <a name="arguments"></a>Argumente  
 *column_id*  
 Die ID der zu überprüfenden Spalte. Die Spalten-ID kann mithilfe der [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) -Funktion abgerufen werden.  
  
 *change_columns*  
 Die Binärdaten aus der SYS_CHANGE_COLUMNS-Spalte der [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) -Daten.  
  
## <a name="return-type"></a>Rückgabetyp  
 **bit**  
  
## <a name="return-values"></a>Rückgabewerte  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK gibt die folgenden Werte zurück.  
  
|Rückgabewert|BESCHREIBUNG|  
|------------------|-----------------|  
|0|Die angegebene Spalte befindet sich nicht in der *change_columns* Liste.|  
|1|Die angegebene Spalte ist in der *change_columns* Liste enthalten.|  
  
## <a name="remarks"></a>Bemerkungen  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK führt keine Überprüfungen aus, um den *column_id* Wert zu überprüfen, oder der *change_columns* Parameter wurde aus der Tabelle abgerufen, aus der der *column_id* abgerufen wurde.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird bestimmt, ob die `Salary`-Spalte in der `Employees`-Tabelle aktualisiert wurde. Die- `COLUMNPROPERTY` Funktion gibt die Spalten-ID der `Salary` Spalte zurück. Für die lokale Variable `@change_columns` müssen die Ergebnisse einer Abfrage unter Verwendung von CHANGETABLE als Datenquelle festgelegt werden.  
  
```sql  
SET @SalaryChanged = CHANGE_TRACKING_IS_COLUMN_IN_MASK  
    (COLUMNPROPERTY(OBJECT_ID('Employees'), 'Salary', 'ColumnId')  
    ,@change_columns);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Änderungsnachverfolgungsfunktionen &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [Nachverfolgen von Datenänderungen &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
