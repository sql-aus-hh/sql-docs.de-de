---
description: sys.filetable_system_defined_objects (Transact-SQL)
title: sys.filetable_system_defined_objects (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.filetable_system_defined_objects_TSQL
- filetable_system_defined_objects
- filetable_system_defined_objects_TSQL
- sys.filetable_system_defined_objects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filetable_system_defined_objects catalog view
ms.assetid: 62022e6b-46f6-495f-b14b-53f41e040361
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f18fdc79d85239422d80687a8ba54b5027caae85
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098006"
---
# <a name="sysfiletable_system_defined_objects-transact-sql"></a>sys.filetable_system_defined_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt eine Liste mit systemdefinierten Objekten an, die sich auf FileTables beziehen. Enthält eine Zeile für jedes systemdefinierte Objekt.  
  
 Wenn Sie eine FileTable erstellen, werden verknüpfte Objekte, z. B. Einschränkungen und Indizes, zur gleichen Zeit erstellt. Sie können diese Objekte nicht ändern oder löschen; sie verschwinden nur, wenn die FileTable selbst gelöscht wird.  
  
 Weitere Informationen zu FileTables finden Sie unter [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
|Column|Datentyp|BESCHREIBUNG|  
|------------|---------------|-----------------|  
|**object_id**|**int**|Objekt-ID des systemdefinierten Objekts für eine FileTable.<br /><br /> Verweist auf das Objekt in **sys.objects**.|  
|**parent_object_id**|**int**|Objekt-ID der übergeordneten FileTable.<br /><br /> Verweist auf das Objekt in **sys.objects**.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen, Ändern und Löschen von FileTables](../../relational-databases/blob/create-alter-and-drop-filetables.md)   
 [Verwalten von FileTables](../../relational-databases/blob/manage-filetables.md)  
  
  
