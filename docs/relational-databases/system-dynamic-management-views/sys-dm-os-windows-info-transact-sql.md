---
description: sys.dm_os_windows_info (Transact-SQL)
title: sys.dm_os_windows_info (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_windows_info
- dm_os_windows_info_TSQL
- sys.dm_os_windows_info
- sys.dm_os_windows_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_windows_info dynamic management view
ms.assetid: adc81283-fdc2-46c0-bb48-abe82bbf2459
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 111e5b732dae02a5bc2bc8dcffebb3b563206f9c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094028"
---
# <a name="sysdm_os_windows_info-transact-sql"></a>sys.dm_os_windows_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt in einer Zeile Informationen zur Windows-Betriebssystemversion zurück.  
  
  Gilt nur für SQL Server, die unter Windows ausgeführt wird. Verwenden Sie [sys.dm_os_host_info &#40;Transact-SQL-&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md), um eine ähnliche Information für SQL Server, die auf einem nicht-Windows-Host ausgeführt wird, wie z. b. Linux, anzuzeigen. 
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**windows_release**|**nvarchar(256)**|Für Windows wird die Releasenummer zurückgegeben. Eine Liste der Werte mit Beschreibungen finden Sie unter [Betriebssystemversion (Windows)](/windows/desktop/SysInfo/operating-system-version). Lässt keine NULL-Werte zu.|  
|**windows_service_pack_level**|**nvarchar(256)**| Für Windows wird die Service Pack Zahl zurückgegeben. Lässt keine NULL-Werte zu. |  
|**windows_sku**|**int**|Für Windows wird die Windows-SKU-ID (Stock Keeping Unit) zurückgegeben. Eine Liste mit SKU-IDs und Beschreibungen finden Sie unter [Funktion "GetProductInfo"](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo). Lässt NULL-Werte zu. |  
|**os_language_version**|**int**| Für Windows gibt den Windows-Gebiets Schema Bezeichner (LCID) des Betriebssystems zurück. Eine Liste mit LCID-Werten und Beschreibungen finden Sie unter [Von Microsoft zugewiesene Gebietsschemabezeichner (LCIDs)](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c). Lässt keine NULL-Werte zu.|  
  
  
## <a name="permissions"></a>Berechtigungen  
Die SELECT-Berechtigung für sys.dm_os_windows_info wird standardmäßig der public-Rolle erteilt. Wenn Sie widerrufen wird, ist die View Server State-Berechtigung auf dem Server erforderlich.  

## <a name="limitations-and-restrictions"></a>Einschränkungen
Verwenden Sie [sys.dm_os_host_info &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md), um die Informationen zu SQL anzuzeigen, die auf einem nicht-Windows-Host ausgeführt werden, z. b. Linux. 
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle Spalten aus der Sicht **sys.dm_os_windows_info** zurückgegeben.  
  
```  
SELECT windows_release, windows_service_pack_level, windows_sku, os_language_version  
FROM sys.dm_os_windows_info;  
```  
  
 Hier ist ein Beispielresultset.  
  
 `windows_release  windows_service_pack_level   windows_sku   os_language_version`  
  
 `---------------  ---------------------------  ------------  -------------------`  
  
 `6.0              Service Pack 2                4            1033`  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_os_host_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md)  
  
