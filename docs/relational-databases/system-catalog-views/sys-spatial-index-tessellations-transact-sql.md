---
description: sys.spatial_index_tessellations (Transact-SQL)
title: sys.spatial_index_tessellations (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ddf7b9cd75999da6cc48f6e7a83bab7a733f822b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095417"
---
# <a name="sysspatial_index_tessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 Stellt die Informationen zum Mosaikschema und zu den Parametern jedes räumlichen Indexes dar.  
  
> [!NOTE]  
>  Informationen zu Mosaiken finden Sie unter [Übersicht über räumliche Indizes](../../relational-databases/spatial/spatial-indexes-overview.md).  
  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Die ID des Objekts, für das der Index definiert wird. Jedes Paar (object_id index_id) verfügt über einen entsprechenden Eintrag in [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md).|  
|index_id|**int**|Die ID des räumlichen Indexes, in dem die indizierte Spalte definiert wird.|  
|tessellation_scheme|**sysname**|Der Name des Mosaik Schemas, z. a.: GEOMETRY_GRID GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|X-Koordinate der unteren linken Ecke des umgebenden Felds, eines von: NULL = nicht zutreffend für ein bestimmtes Mosaik Schema (z. b. GEOGRAPHY_GRID) *n* =, Wenn tessellation_scheme GEOMETRY_GRID ist, der minimale x-Koordinaten Wert.                     **Hinweis:** Die durch die Parameter des Begrenzungs Rahmens definierten Koordinaten werden für jedes Objekt entsprechend seiner [SRID (Spatial Reference Identifier)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)interpretiert.|  
|bounding_box_ymin|**float(53)**|Die y-Koordinate der unteren linken Ecke des umgebenden Felds, eine von: NULL = ist für ein bestimmtes Mosaik Schema (z. b. GEOGRAPHY_GRID) *n* = nicht zutreffend, Wenn tessellation_scheme GEOMETRY_GRID ist, der y-minimale Koordinaten Wert.|  
|bounding_box_xmax|**float(53)**|X-Koordinate der oberen rechten Ecke des umgebenden Felds, eine von: NULL = ist für ein bestimmtes Mosaik Schema nicht zutreffend (z. b. GEOGRAPHY_GRID) *n* = Wenn tessellation_scheme GEOMETRY_GRID ist, der maximale x-Koordinaten Wert|  
|bounding_box_ymax|**float(53)**|Die y-Koordinate der oberen rechten Ecke des umgebenden Felds, eine von: NULL = ist für ein bestimmtes Mosaik Schema (z. b. GEOGRAPHY_GRID) *n* = nicht zutreffend, Wenn tessellation_scheme GEOMETRY_GRID ist, der maximale y-Koordinaten Wert|  
|level_1_grid|**smallint**|Die Dichte des Rasters der obersten Ebene. Wenn tessellation_scheme GEOMETRY_GRID oder GEOGRAPHY_GRID ist, gilt Folgendes: 16 = 4 x 4 Grid (Low) 64 = 8 x 8 Grid (Medium) 256 = 16 by 16 Grid (High) NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_1_grid_desc|**nvarchar(60)**|Die Raster Dichte für das Raster der obersten Ebene, eines der folgenden Werte: niedrig Mittelhohe NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_2_grid|**smallint**|Die Dichte des Rasters der zweiten Ebene. Wenn tessellation_scheme GEOMETRY_GRID oder GEOGRAPHY_GRID ist, gilt Folgendes: 16 = 4 x 4 Grid (Low) 64 = 8 x 8 Grid (Medium) 256 = 16 by 16 Grid (High) NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_2_grid_desc|**nvarchar(60)**|Die Raster Dichte für das Raster der zweiten Ebene, eines der folgenden Werte: niedrig Mittelhohe NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_3_grid|**smallint**|Die Dichte des Rasters der dritten Ebene.   Wenn tessellation_scheme GEOMETRY_GRID oder GEOGRAPHY_GRID ist, gilt Folgendes: 16 = 4 x 4 Grid (Low) 64 = 8 x 8 Grid (Medium) 256 = 16 by 16 Grid (High) NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_3_grid_desc|**nvarchar(60)**|Die Raster Dichte für das Raster der dritten Ebene, eines der folgenden Werte: niedrig Mittelhohe NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_4_grid|**smallint**|Die Dichte des Rasters der vierten Ebene. Wenn tessellation_scheme GEOMETRY_GRID oder GEOGRAPHY_GRID ist, gilt Folgendes: 16 = 4 x 4 Grid (Low) 64 = 8 x 8 Grid (Medium) 256 = 16 by 16 Grid (High) NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|level_4_grid_desc|**nvarchar(60)**|Die Raster Dichte für das Raster der vierten Ebene, eines der folgenden Werte: < niedrige mittlere hohe NULL = ist für den angegebenen räumlichen Indextyp oder das Mosaik Schema nicht zutreffend.  NULL wird zurückgegeben, wenn das neue Mosaikschema von SQL Server 11 verwendet wird.|  
|cells_per_object|**int**|Anzahl von Zellen pro räumlichem Objekt. folgende Werte sind möglich: Wenn tessellation_scheme GEOMETRY_GRID oder GEOGRAPHY_GRID ist, *n* = Anzahl der Zellen pro Objekt NULL = nicht zutreffend für den angegebenen räumlichen Indextyp oder das Mosaik Schema.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Übersicht über räumliche Indizes](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
