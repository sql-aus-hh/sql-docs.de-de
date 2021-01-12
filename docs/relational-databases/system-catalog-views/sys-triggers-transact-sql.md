---
description: sys.triggers (Transact-SQL)
title: sys. Triggers (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- triggers
- triggers_TSQL
- sys.triggers
- sys.triggers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.triggers catalog view
ms.assetid: cefa4fc4-b8b9-4cd7-b124-eed5283acbfc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9fbf3f59de6e7508fd8ee1fac13a98bb36170b1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094316"
---
# <a name="systriggers-transact-sql"></a>sys.triggers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Enthält eine Zeile für jedes Objekt, bei dem es sich um einen Trigger mit dem Typ TR (SQL-Trigger) oder TA (Assemblytrigger (CLR)) handelt. DML-Triggernamen besitzen Schemas als Bereiche und werden daher in **sys.objects** angezeigt. Der Bereich von DDL-Triggernamen wird durch die übergeordnete Entität bestimmt, und DDL-Triggernamen werden nur in dieser Sicht angezeigt.  
  
 Durch die Spalten **parent_class** und **name** wird der Trigger in der Datenbank eindeutig identifiziert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Triggername. Die Namen von DML-Triggern stammen aus dem Bereich des Schemas. Der Bereich von DDL-Triggernamen richtet sich nach der übergeordneten Entität.|  
|**object_id**|**int**|Objekt-ID. Ist innerhalb einer Datenbank eindeutig.|  
|**parent_class**|**tinyint**|Klasse des übergeordneten Objekts des Triggers.<br /><br /> 0 = Datenbank, für die DDL-Trigger.<br /><br /> 1 = Objekt oder Spalte für die DML-Trigger.|  
|**parent_class_desc**|**nvarchar(60)**|Beschreibung der übergeordneten Klasse des Triggers.<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN|  
|**parent_id**|**int**|ID des übergeordneten Objekts des Triggers:<br /><br /> 0 = Trigger, deren übergeordnetes Objekt eine Datenbank ist.<br /><br /> Bei DML-Triggern ist dies die **object_id** der Tabelle oder Sicht, für die der DML-Trigger definiert ist.|  
|**type**|**char(2)**|Objekttyp:<br /><br /> TA = Assembly (CLR) Trigger<br /><br /> TR = SQL-Trigger|  
|**type_desc**|**nvarchar(60)**|Beschreibung des Objekttyps.<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**create_date**|**datetime**|Das Datum, an dem der Trigger erstellt wurde.|  
|**modify_date**|**datetime**|Das Datum, an dem das Objekt zuletzt mithilfe einer ALTER-Anweisung geändert wurde.|  
|**is_ms_shipped**|**bit**|Der Trigger, der für den Benutzer durch eine interne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Komponente erstellt wurde.|  
|**is_disabled**|**bit**|Trigger ist deaktiviert.|  
|**is_not_for_replication**|**bit**|Der Trigger wurde mit der NOT FOR REPLICATION-Option erstellt.|  
|**is_instead_of_trigger**|**bit**|1 = INSTEAD OF-Trigger<br /><br /> 0 = AFTER-Trigger|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
