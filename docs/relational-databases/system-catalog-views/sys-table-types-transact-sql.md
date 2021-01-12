---
description: sys.table_types (Transact-SQL)
title: sys.table_types (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- table_types_TSQL
- sys.table_types
- sys.table_types_TSQL
- table_types
dev_langs:
- TSQL
helpviewer_keywords:
- table types [SQL Server]
- table-valued parameters, sys.table_types
- sys.table_types
- UDTT
ms.assetid: c05fd873-aff2-4a89-9936-a54c2ea09996
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c920317b40f0fbcd922c1fe0c440c1a210e0a984
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094443"
---
# <a name="systable_types-transact-sql"></a>sys.table_types (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Zeigt Eigenschaften von benutzerdefinierten Tabellentypen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an. Ein Tabellentyp ist ein Typ, von dem Tabellenvariablen oder Tabellenwertparameter deklariert werden können. Jeder Tabellentyp verfügt über eine **type_table_object_id** , bei der es sich um einen Fremdschlüssel in die [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) -Katalog Sicht handelt. Sie können diese ID-Spalte verwenden, um verschiedene Katalog Sichten auf eine Weise abzufragen, die mit einer **object_id** -Spalte einer regulären Tabelle vergleichbar ist, um die Struktur des Tabellentyps, wie z. b. die Spalten und Einschränkungen, zu ermitteln.    
 
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|*\<inherited columns>*||Eine Liste der Spalten, die diese Sicht erbt, finden Sie unter [sys. types &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md).|  
|**type_table_object_id**|**int**|Objekt-ID. Diese Nummer ist innerhalb einer Datenbank eindeutig.|  
|**is_memory_optimized**|**bit**|**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.<br /><br /> Folgende Werte sind möglich:<br /><br /> 0 = ist nicht speicheroptimiert<br /><br /> 1 = ist speicheroptimiert<br /><br /> Der Standardwert ist 0 (null).<br /><br /> Tabellentypen werden immer mit DURABILITY = SCHEMA_ONLY erstellt. Nur das Schema wird auf dem Datenträger beibehalten.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Verwenden Sie Table-Valued Parameter &#40;Datenbank-Engine&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)   
 [In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
