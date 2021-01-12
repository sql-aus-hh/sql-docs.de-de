---
description: sys.partition_functions (Transact-SQL)
title: sys.partition_functions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.partition_functions_TSQL
- partition_functions
- sys.partition_functions
- partition_functions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_functions catalog view
ms.assetid: 96515727-728b-4bea-804a-36ce915b8b75
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e5049d582e43e39c18cb7554a566a1212deb80f8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100638"
---
# <a name="syspartition_functions-transact-sql"></a>sys.partition_functions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jede Partitionsfunktion in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der Name der Partitionsfunktion. Ist in der Datenbank eindeutig.|  
|**function_id**|**int**|Die Partitionsfunktions.ID. Ist in der Datenbank eindeutig.|  
|**type**|**char(2)**|Funktionstyp<br /><br /> R = Bereich|  
|**type_desc**|**nvarchar(60)**|Funktionstyp<br /><br /> RANGE|  
|**fanout**|**int**|Anzahl der von der Funktion erstellten Partitionen|  
|**boundary_value_on_right**|**bit**|Für die Bereichspartitionierung.<br /><br /> 1 = Der Begrenzungswert ist im RIGHT-Bereich der Begrenzung enthalten.<br /><br /> 0 = LEFT.|  
|**is_system**||**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> 1 = Das Objekt wird für Volltextindexfragmente verwendet.<br /><br /> 0 = Das Objekt wird nicht für Volltextindexfragmente verwendet.|  
|**create_date**|**datetime**|Datum, an dem die Funktion erstellt wurde|  
|**modify_date**|**datetime**|Datum, an dem die Funktion zuletzt mithilfe einer ALTER-Anweisung geändert wurde|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalog Sichten für Partitions Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/partition-function-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.partition_range_values &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partition_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)  
  
  
