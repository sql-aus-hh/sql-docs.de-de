---
title: Sys. xml_schema_elements (Transact-SQL) | Microsoft-Dokumentation
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
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ed5c7efe1c5adacba99b74ab40e2978a51ba7517
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47716688"
---
# <a name="sysxmlschemaelements-transact-sql"></a>sys.xml_schema_elements (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Gibt eine Zeile pro XML-Schemakomponente, die ein Typ ist **Symbol_space** von **E**.  
  
|Spaltenname|Datentyp|Description|  
|-----------------|---------------|-----------------|  
|**\<geerbte Spalten >**|**--**|Erbt Spalten von [xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_default_fixed**|**bit**|1 = Standardwert ist ein fester Wert. Dieser Wert kann nicht in XML-Instanz nicht überschrieben werden.<br /><br /> 0 = Standardwert ist kein fester Wert für das Element. (Standard).|  
|**is_abstract**|**bit**|1 = Element ist abstrakt und kann in einem Instanzdokument nicht verwendet werden. Ein Mitglied der Ersetzungsgruppe des Elements muss im Instanzdokument vorkommen.<br /><br /> 0 = Element ist nicht abstrakt. (Standard).|  
|**is_nillable**|**bit**|1 = Element kann ein NULL-Wert sein.<br /><br /> 0 = Element kann kein NULL-Wert sein. (Standard)|  
|**must_be_qualified**|**bit**|1 = Element muss explizit als Namespace qualifiziert werden.<br /><br /> 0 = Element kann implizit als Namespace qualifiziert werden. (Standard)|  
|**is_extension_blocked**|**bit**|1 = Ersetzen durch eine Instanz eines Erweiterungstyps ist blockiert.<br /><br /> 0 = Ersetzen durch Erweiterungstyp ist zulässig. (Standard)|  
|**is_restriction_blocked**|**bit**|1 = Ersetzen durch eine Instanz eines Einschränkungstyps ist blockiert.<br /><br /> 0 = Ersetzen durch Einschränkungstyp ist zulässig. (Standard)|  
|**is_substitution_blocked**|**bit**|1 = Instanz einer Ersetzungsgruppe kann nicht verwendet werden.<br /><br /> 0 = Ersetzen durch Ersetzungsgruppe ist zulässig. (Standard)|  
|**is_final_extension**|**bit**|1 = Ersetzen durch eine Instanz eines Erweiterungstyps ist nicht zulässig.<br /><br /> 0 = Ersetzen in einer Instanz eines Erweiterungstyps ist zulässig. (Standard)|  
|**is_final_restriction**|**bit**|1 = Ersetzen durch eine Instanz eines Einschränkungstyps ist nicht zulässig.<br /><br /> 0 = Ersetzen in einer Instanz eines Einschränkungstyps ist zulässig. (Standard)|  
|**default_value**|**Nvarchar (4000)**|Standardwert des Elements. NULL, wenn kein Standardwert bereitgestellt wird.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Siehe auch  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML-Schemas &#40;XML-Typsystem&#41; Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
