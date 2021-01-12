---
description: sys.dm_io_cluster_valid_path_names (Transact-SQL)
title: sys.dm_io_cluster_valid_path_names (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_io_cluster_valid_path_names
- dm_io_cluster_valid_path_names_TSQL
- sys.dm_io_cluster_valid_path_names_TSQL
- dm_io_cluster_valid_path_names
dev_langs:
- TSQL
helpviewer_keywords:
- dm_io_cluster_valid_path_names
- sys.dm_io_cluster_valid_path_names
- cluster valid path names
- csv name
- cluster shared volume names
ms.assetid: 5bc8a0e5-6c72-425b-8c58-f276eb9add2c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 24bddc071b9ad5b64ef796f16718d1465dec7eaa
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097580"
---
# <a name="sysdm_io_cluster_valid_path_names-transact-sql"></a>sys.dm_io_cluster_valid_path_names (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu allen gültigen freigegebenen Datenträgern, einschließlich gruppierten freigegebenen Volumes, für eine SQL Server-Failoverclusterinstanz zurück. Wenn die Instanz nicht gruppiert ist, wird ein leeres Rowset zurückgegeben.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**path_name**|**Nvarchar (512)**|Der Einbindungspunkt oder Laufwerkpfad des Volumes, der als Stammverzeichnis für Datenbank- und Protokolldateien verwendet werden kann. Lässt keine NULL-Werte zu.|  
|**cluster_owner_node**|**Nvarchar (64)**|Der aktuelle Besitzer des Laufwerks. Bei freigegebenen Clustervolumes (CSV) entspricht der Besitzer dem Knoten, der den MetaData-Server hostet. Lässt keine NULL-Werte zu.|  
|**is_cluster_shared_volume**|**Trate**|Gibt 1 zurück, wenn das Laufwerk, auf dem sich der Pfad befindet, ein freigegebenes Clustervolume ist; andernfalls wird 0 zurückgegeben.|  
  
## <a name="remarks"></a>Bemerkungen  
 Eine SQL Server-Failoverclusterinstanz (FCI) muss auf allen FCI-Knoten freigegebenen Speicher zur Speicherung von Daten- und Protokolldateien verwenden. Die in dieser Sicht aufgeführten Datenträger sind die Datenträger der Clusterressourcengruppe, die der Instanz zugeordnet ist. Es sind die einzigen Datenträger, die zur Speicherung von Daten- oder Protokolldateien verwendet werden können.  
  
> [!NOTE]  
>  In dieser Ansicht wird [sys.dm_io_cluster_shared_drives &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md) in einer zukünftigen Version ersetzt.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer muss über die VIEW SERVER STATE-Berechtigung für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verfügen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird sys.dm_io_cluster_valid_path_names verwendet, um die freigegebenen Laufwerke in einer Instanz eines gruppierten Servers zu ermitteln:  
  
```  
SELECT * FROM sys.dm_io_cluster_valid_path_names;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_os_cluster_nodes &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

