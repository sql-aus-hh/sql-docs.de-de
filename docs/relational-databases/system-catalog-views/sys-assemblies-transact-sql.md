---
description: sys.assemblies (Transact-SQL)
title: sys. Assemblys (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.assemblies
- assemblies_TSQL
- sys.assemblies_TSQL
- assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assemblies catalog view
ms.assetid: e321753f-293f-42ab-b225-d118713df40b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6860b76d61730a91140ddf502f1bedf57f607b4a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102889"
---
# <a name="sysassemblies-transact-sql"></a>sys.assemblies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jede Assembly zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der Name der Assembly. Ist in der Datenbank eindeutig.|  
|**principal_id**|**int**|ID des Prinzipals, der der Besitzer dieser Assembly ist.|  
|**assembly_id**|**int**|Assembly-ID. Ist innerhalb einer Datenbank eindeutig.|  
|**clr_name**|**nvarchar(4000)**|Kanonische Zeichenfolge, die den einfachen Namen, die Versionsnummer, die Kultur, den öffentlichen Schlüssel und die Architektur der Assembly codiert. Durch diesen Wert wird die CLR-seitige Assembly (Common Language Runtime) eindeutig identifiziert.|  
|**permission_set**|**tinyint**|Berechtigungssatz/Sicherheitsebene für die Assembly.<br /><br /> 1 = Sicherer Zugriff<br /><br /> 2 = Externer Zugriff<br /><br /> 3 = Unsicherer Zugriff|  
|**permission_set_desc**|**nvarchar(60)**|Beschreibung von Berechtigungssatz/Sicherheitsebene für die Assembly.<br /><br /> SAFE_ACCESS<br /><br /> EXTERNAL_ACCESS<br /><br /> UNSAFE_ACCESS|  
|**is_visible**|**bit**|1 = Assembly ist zum Registrieren des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Einstiegpunktes sichtbar.<br /><br /> 0 = Assembly ist nur für verwaltete Aufrufer gedacht. Die Assembly stellt also die interne Implementierung für andere Assemblys in der Datenbank bereit.|  
|**create_date**|**datetime**|Datum, an dem die Assembly erstellt oder registriert wurde.|  
|**modify_date**|**datetime**|Datum, an dem die Assembly geändert wurde.|  
|**is_user_defined**|**bit**|Gibt die Quelle der Assembly an.<br /><br /> 0 = Systemdefinierte Assemblys (beispielsweise Microsoft.SqlServer.Types für den Datentyp **hierarchyid** )<br /><br /> 1 = Benutzerdefinierte Assemblys|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [CLR-assemblykatalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/clr-assembly-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Assemblyproperty &#40;Transact-SQL-&#41;](../../t-sql/functions/assemblyproperty-transact-sql.md)  
  
  
