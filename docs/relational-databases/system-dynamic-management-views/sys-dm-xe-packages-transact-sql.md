---
description: sys.dm_xe_packages (Transact-SQL)
title: sys.dm_xe_packages (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xe_packages_TSQL
- sys.dm_xe_packages_TSQL
- dm_xe_packages
- sys.dm_xe_packages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_packages dynamic management view
- extended events [SQL Server], views
ms.assetid: 2e5ecbe9-3ea8-45e6-a161-e31671a03e1d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f4d23f493b669744f0c3bb53b6148056131ebe17
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093927"
---
# <a name="sysdm_xe_packages-transact-sql"></a>sys.dm_xe_packages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Listet alle Pakete auf, die bei der Engine für erweiterte Ereignisse registriert sind.  
  
 
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Der Name des Pakets. Die Beschreibung wird vom Paket selbst verfügbar gemacht. Lässt keine NULL-Werte zu.|  
|guid|**uniqueidentifier**|Die GUID, die das Paket identifiziert. Lässt keine NULL-Werte zu.|  
|description|**nvarchar (3072)**|Die Paketbeschreibung. descriptionis wird vom Paket Ersteller festgelegt und lässt keine NULL-Werte zu.|  
|capabilities|**int**|Bitmap, die die Funktionen dieses Pakets beschreibt. Lässt NULL-Werte zu.|  
|capabilities_desc|**nvarchar(256)**|Eine Liste aller möglichen Funktionen für dieses Paket. Lässt NULL-Werte zu.|  
|module_guid|**nvarchar(60)**|Die GUID des Moduls, das dieses Paket verfügbar macht. Lässt keine NULL-Werte zu.|  
|module_address|**varbinary(8)**|Die Basisadresse, an der das Modul geladen wird, das das Paket enthält. Ein einzelnes Modul macht möglicherweise mehrere Pakete verfügbar. Lässt keine NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="remarks"></a>Bemerkungen  
 Die für die Engine für erweiterte Ereignisse registrierten Pakete machen Folgendes verfügbar: Ereignisse, die Aktionen, die beim Auslösen der Ereignisse ausgeführt werden können, sowie Ziele für die synchrone und asynchrone Verarbeitung von Ereignisdaten.  
  
 Diese Pakete können dynamisch in einen Prozessadressbereich geladen werden. Sobald das Paket geladen wird, registriert es alle Objekte, die es verfügbar macht, für die Engine für erweiterte Ereignisse.  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
| Von | Beschreibung | Relationship |
| ---- | -- | ------------ |  
|sys.dm_xe_packages.module_address|sys.dm_os_loaded_modules.base_address|n:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

