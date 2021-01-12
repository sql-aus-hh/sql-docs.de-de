---
description: sys.xml_schema_types (Transact-SQL)
title: sys.xml_schema_types (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.xml_schema_types_TSQL
- xml_schema_types_TSQL
- sys.xml_schema_types
- xml_schema_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_types catalog view
ms.assetid: 441ba49d-f778-4fa1-98c4-ced375a01a34
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2bd282bdecec59df5c1edf611cf2e2a8d6cfa08c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097874"
---
# <a name="sysxml_schema_types-transact-sql"></a>sys.xml_schema_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro XML-Schemakomponente zurück, die einem Typ entspricht. Dabei ist **symbol_space** gleich **T**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Erbt Spalten von [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_abstract**|**bit**|1 = Typ ist ein abstrakter Typ. Alle Instanzen eines Elements dieses Typs müssen **xsi:type** verwenden, um auf einen abgeleiteten Typ hinzuweisen, der nicht abstrakt ist.<br /><br /> 0 = Typ ist nicht abstrakt. (Standard)|  
|**allows_mixed_content**|**bit**|1 = Gemischter Inhalt ist zulässig<br /><br /> 0 = Gemischter Inhalt ist nicht zulässig (Standard)|  
|**is_extension_blocked**|**bit**|1 = Ersetzung durch eine Erweiterung des Typs wird in-Instanzen blockiert, wenn das Block-Attribut in der **complexType** -Definition oder das **Block default** -Attribut des Elements des Vorgänger \<schema> Elements auf "Extension" oder "#All" festgelegt ist.<br /><br /> 0 = Ersetzen durch Erweiterung ist nicht blockiert.|  
|**is_restriction_blocked**|**bit**|1 = Ersetzung durch eine Einschränkung des Typs wird in-Instanzen blockiert, wenn das Block-Attribut in der **complexType** -Definition oder das **Block default** -Attribut des Elements des Vorgänger \<schema> Elements auf "Einschränkung" oder "#All" festgelegt ist.<br /><br /> 0 = Ersetzen durch Einschränkung ist nicht blockiert. (Standard)|  
|**is_final_extension**|**bit**|1 = Ableitung durch Erweiterung des Typs wird blockiert, wenn das abschließende Attribut in der **complexType** -Definition oder das **Final default** -Attribut des Elements des Vorgänger \<schema> Elements auf "Extension" oder "#All" festgelegt ist.<br /><br /> 0 = Erweiterung ist zulässig. (Standard)|  
|**is_final_restriction**|**bit**|1 = Ableitung durch Einschränkung des Typs wird blockiert, wenn das abschließende Attribut in der Simple-oder **complexType** -Definition oder das **Final default** -Attribut des \<schema> Elements des Vorgänger Elements auf "Einschränkung" oder "#All" festgelegt ist.<br /><br /> 0 = Einschränkung ist zulässig. (Standard)|  
|**is_final_list_member**|**bit**|1 = Dieser einfache Typ kann nicht als Elementtyp in einer Liste verwendet werden.<br /><br /> 0 = Dieser Typ ist ein komplexer Typ oder kann als Listenelementtyp verwendet werden. (Standard)|  
|**is_final_union_member**|**bit**|1 = Dieser einfache Typ kann nicht als Elementtyp eines Vereinigungstyps verwendet werden.<br /><br /> 0 = Dieser Typ ist ein komplexer Typ oder kann als Vereinigungselementtyp verwendet werden. (Standard)|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [XML-Schemas &#40;XML-Typsystem&#41; Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
