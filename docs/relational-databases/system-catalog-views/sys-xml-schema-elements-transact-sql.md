---
description: sys.xml_schema_elements (Transact-SQL)
title: sys.xml_schema_elements (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.xml_schema_elements
- sys.xml_schema_elements_TSQL
- xml_schema_elements
- xml_schema_elements_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_elements catalog view
ms.assetid: 190ed0cd-0c5e-4607-9db4-9e77cacf17d7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d867720ba017f45ab3e413b452ae975e2930727c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092970"
---
# <a name="sysxml_schema_elements-transact-sql"></a>sys.xml_schema_elements (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro XML-Schema Komponente zurück, bei der es sich um einen Typ handelt, **symbol_space** von **E**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Erbt Spalten von [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_default_fixed**|**bit**|1 = Standardwert ist ein fester Wert. Dieser Wert kann in der XML-Instanz nicht überschrieben werden.<br /><br /> 0 = Standardwert ist kein fester Wert für das Element. (Standard).|  
|**is_abstract**|**bit**|1 = Element ist abstrakt und kann in einem Instanzdokument nicht verwendet werden. Ein Mitglied der Ersetzungsgruppe des Elements muss im Instanzdokument vorkommen.<br /><br /> 0 = Element ist nicht abstrakt. (Standard).|  
|**is_nillable**|**bit**|1 = Element kann ein NULL-Wert sein.<br /><br /> 0 = Element kann kein NULL-Wert sein. (Standard)|  
|**must_be_qualified**|**bit**|1 = Element muss explizit als Namespace qualifiziert werden.<br /><br /> 0 = Element kann implizit als Namespace qualifiziert werden. (Standard)|  
|**is_extension_blocked**|**bit**|1 = Ersetzen durch eine Instanz eines Erweiterungstyps ist blockiert.<br /><br /> 0 = Ersetzen durch Erweiterungstyp ist zulässig. (Standard)|  
|**is_restriction_blocked**|**bit**|1 = Ersetzen durch eine Instanz eines Einschränkungstyps ist blockiert.<br /><br /> 0 = Ersetzen durch Einschränkungstyp ist zulässig. (Standard)|  
|**is_substitution_blocked**|**bit**|1 = Instanz einer Ersetzungsgruppe kann nicht verwendet werden.<br /><br /> 0 = Ersetzen durch Ersetzungsgruppe ist zulässig. (Standard)|  
|**is_final_extension**|**bit**|1 = Ersetzen durch eine Instanz eines Erweiterungstyps ist nicht zulässig.<br /><br /> 0 = Ersetzen in einer Instanz eines Erweiterungstyps ist zulässig. (Standard)|  
|**is_final_restriction**|**bit**|1 = Ersetzen durch eine Instanz eines Einschränkungstyps ist nicht zulässig.<br /><br /> 0 = Ersetzen in einer Instanz eines Einschränkungstyps ist zulässig. (Standard)|  
|**default_value**|**nvarchar (4000)**|Standardwert des Elements. NULL, wenn kein Standardwert bereitgestellt wird.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML-Schemas &#40;XML-Typsystem&#41; Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
