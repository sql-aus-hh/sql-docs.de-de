---
title: DISCOVER_SCHEMA_ROWSETS-Rowsets | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
- docset-sql-devref
ms.topic: reference
api_name:
- DISCOVER_SCHEMA_ROWSETS
topic_type:
- apiref
helpviewer_keywords:
- DISCOVER_SCHEMA_ROWSETS rowset
ms.assetid: e5012aa0-6ef8-497f-96c1-2772e2394f62
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: 951914dd810b3f9ec9fca52790510541a1cca604
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48124220"
---
# <a name="discoverschemarowsets-rowset"></a>DISCOVER_SCHEMA_ROWSETS-Rowsets
  Gibt die Namen, Einschränkungen, Beschreibungen und anderen Informationen für alle Enumerationswerte und zusätzlichen anbieterspezifischen Enumerationswerte zurück, die vom [!INCLUDE[msCoName](../../../includes/msconame-md.md)] XML for Analysis-Anbieter (XMLA) unterstützt werden.  
  
 Aufrufen der [ermitteln](../../xmla/xml-elements-methods-discover.md) -Methode mit der `DISCOVER_SCHEMA_ROWSETS` Enumerationswert in der [RequestType](../../xmla/xml-elements-properties/type-element-xmla.md) -Element, die `Discover` Methode gibt die `DISCOVER_SCHEMA_ROWSETS` Rowset.  
  
## <a name="rowset-columns"></a>Rowsetspalten  
 Das DISCOVER_SCHEMA_ROWSETS-Rowset enthält die folgenden Spalten.  
  
|Spaltenname|Typindikator|Länge|Description|  
|-----------------|--------------------|------------|-----------------|  
|`SchemaName`|`DBTYPE_WSTR`||Der Name des Schemas oder der Anforderung. Diese Anforderung gibt die Werte der *RequestTypes* -Enumeration zurück.|  
|`SchemaGuid`|`DBTYPE_GUID`||Die GUID des Schemas.|  
|`Restrictions`|`DBTYPE_HCHAPTER`||Ein Array von vom Anbieter unterstützten Einschränkungen.|  
|`Description`|`DBTYPE_WSTR`||Eine lokalisierbare Beschreibung des Schemas.|  
|`RestrictionsMask`|`DBTYPE_UI8`|||  
  
 Dieses Schemarowset ist nicht sortiert.  
  
 Für einen Anbieter, der drei Einschränkungen für das DBSCHEMA_MEMBERS-Schemarowset unterstützt die `Restrictions` Array das folgende Ergebnis zurückgeben. Die Elemente im Ergebnis verweisen auf Spaltennamen im Schema.  
  
```  
<Restrictions>  
      <CATALOG_NAME type="string" />   
      <SCHEMA_NAME type="string" />   
      <CUBE_NAME type="string" />   
</Restrictions>  
```  
  
## <a name="restriction-columns"></a>Einschränkungsspalten  
 Die `DISCOVER_SCHEMA_ROWSETS` Rowset kann auf die in der folgenden Tabelle aufgeführten Spalten eingeschränkt werden.  
  
|Spaltenname|Typindikator|Einschränkungsstatus|  
|-----------------|--------------------|-----------------------|  
|`SchemaName`|`DBTYPE_WSTR`||  
  
## <a name="see-also"></a>Siehe auch  
 [XML for Analysis – Schemarowsets](xml-for-analysis-schema-rowsets.md)  
  
  
