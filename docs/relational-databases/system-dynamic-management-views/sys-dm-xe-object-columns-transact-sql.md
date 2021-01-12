---
description: sys.dm_xe_object_columns (Transact-SQL)
title: sys.dm_xe_object_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_xe_object_columns
- sys.dm_xe_object_columns_TSQL
- dm_xe_object_columns_TSQL
- dm_xe_object_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_object_columns dynamic management view
- extended events [SQL Server], views
ms.assetid: d96a14f3-4284-45ff-b1fe-4858e540a013
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5da079cfecbf1ddaa622f05ce71e5c04d63f4f0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095057"
---
# <a name="sysdm_xe_object_columns-transact-sql"></a>sys.dm_xe_object_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt die Schemainformationen für alle Objekte zurück.  
  
> [!NOTE]  
>  Ereignisobjekte machen feste Schemas sowohl für schreibgeschützte Daten als auch für Daten mit Schreib-Lese-Zugriff verfügbar.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Der Name der Spalte. der Name ist innerhalb des Objekts eindeutig. Lässt keine NULL-Werte zu.|  
|column_id|**int**|Der Bezeichner der Spalte. column_id ist innerhalb des-Objekts eindeutig, wenn es mit column_type verwendet wird. Lässt keine NULL-Werte zu.|  
|object_name|**nvarchar(256)**|Der Name des Objekts, zu dem diese Spalte gehört. Es ist eine n:1-Beziehung mit sys.dm_xe_objects. ID vorhanden. Lässt keine NULL-Werte zu.|  
|object_package_guid|**uniqueidentifier**|Die GUID des Pakets, das das Objekt enthält. Lässt keine NULL-Werte zu.|  
|type_name|**nvarchar(256)**|Der Name des Typs für diese Spalte. Lässt keine NULL-Werte zu.|  
|type_package_guid|**uniqueidentifier**|Die GUID des Pakets, das den Spaltendatentyp enthält. Lässt keine NULL-Werte zu.|  
|column_type|**nvarchar(60)**|Gibt an, wie diese Spalte verwendet wird. Lässt keine NULL-Werte zu. column_type kann eine der folgenden sein:<br /><br /> readonly. Die Spalte enthält einen statischen Wert, der nicht geändert werden kann.<br /><br /> Modells. Die Spalte enthält vom Objekt verfügbar gemachte Laufzeitdaten.<br /><br /> customizable. Die Spalte enthält einen Wert, der geändert werden kann.<br /><br /> Hinweis: durch Ändern dieses Werts kann das Verhalten des-Objekts geändert werden.|  
|column_value|**nvarchar(256)**|Zeigt statische Werte an, die der Objektspalte zugeordnet sind. Lässt NULL-Werte zu.|  
|capabilities|**int**|Eine Bitmap, die die Fähigkeiten der Spalte beschreibt. Lässt NULL-Werte zu.|  
|capabilities_desc|**nvarchar(256)**|Eine Beschreibung der Fähigkeiten dieser Objektspalte. Die folgenden Werte sind möglich:<br /><br /> Erforderlich. Der Wert muss festgelegt werden, wenn das übergeordnete Objekt an eine Ereignissitzung gebunden wird.<br /><br /> Lässt NULL-Werte zu.|  
|description|**nvarchar (3072)**|Die Beschreibung dieser Objektspalte. Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
### <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Relationship|  
|----------|--------|------------------|  
|sys.dm_xe_object_columns.object_name, sys.dm_xe_object_columns.object_package_guid|sys.dm_xe_objects.name,<br /><br /> sys.dm_xe_objects.package_guid|n:1|  
|sys.dm_xe_object_columns.type_name<br /><br /> sys.dm_xe_object_columns.type_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_objects.package_guid|n:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

