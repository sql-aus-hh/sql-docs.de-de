---
description: Datenebenenanwendungssichten-dbo.sysdac_instances
title: dbo.sysdac_instances (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysdac_instances_TSQL
- sysdac_instances
- sysdac_instances_TSQL
- dbo.sysdac_instances
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.sysdac_instances
- sysdac_instances
ms.assetid: 28285f3d-3889-439f-8b24-3bdef08e46b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5178fdb50b8a959471c8be973080d1416ba10f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092663"
---
# <a name="data-tier-application-views---dbosysdac_instances"></a>Datenebenenanwendungssichten-dbo.sysdac_instances
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt eine Zeile für jede Instanz der Datenebenenanwendung (DAC) an, die für eine [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz bereitgestellt wird. sysdac_instances gehört zum dbo-Schema in der msdb-Datenbank. In der folgenden Tabelle werden die Spalten in der sysdac_instances Sicht beschrieben.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|instance_id|**uniqueidentifier**|Der Bezeichner der DAC-Instanz.|  
|instance_name|**sysname**|Der Name der DAC-Instanz, die bei der Bereitstellung der DAC angegeben wurde.|  
|type_name|**sysname**|Der Name der DAC, die bei der Erstellung des DAC-Pakets angegeben wurde.|  
|type_version|**nvarchar (64)**|Die Version der DAC, die bei der Erstellung des DAC-Pakets angegeben wurde.|  
|description|**nvarchar(4000)**|Eine Beschreibung der DAC, die bei der Erstellung des DAC-Pakets geschrieben wurde.|  
|type_stream|**varbinary(max)**|Ein Bitdatenstrom, der eine codierte Darstellung der in der DAC enthaltenen logischen Objekte, z. B. Tabellen und Sichten, enthält.|  
|date_created|**datetime**|Datum und Uhrzeit der Erstellung der DAC-Instanz.|  
|created_by|**sysname**|Anmelde Name, der die DAC-Instanz erstellt hat.|  
|database_name|**sysname**|Der Name der Datenbank, die für die DAC-Instanz erstellt wurde.|  
  
## <a name="remarks"></a>Bemerkungen  
 Eine DAC umfasst einen DAC-Typ, der eine Definition der von einer Anwendung verwendeten logischen Datenebenenobjekte, z. B. Tabellen und Sichten, ist. Ein DAC-Paket ist eine Datei, die zur Bereitstellung einer DAC verwendet wird. Das DAC-Paket enthält eine Darstellung aller logischen Objekte, die im DAC-Typ enthalten sind. Das DAC-Paket kann dazu verwendet werden, eine oder mehrere Kopien bzw. Instanzen der DAC auf einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz bereitzustellen. Jede DAC-Instanz, die vom selben DAC-Paket bereitgestellt wird, verwendet den gleichen Typ; ihr wird jedoch ein eindeutiger Instanzname und Bezeichner zugewiesen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle sysadmin, um alle Spalten anzuzeigen. Mitglieder der Rolle public können die Spalten instance_name, description und type_version anzeigen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenebenenanwendungen](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Datenebenenanwendungssichten &#40;Transact-SQL-&#41;]()  
  
