---
title: AggregationAttribute-Datentyp (ASSL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
- docset-sql-devref
ms.topic: reference
api_name:
- AggregationAttribute Data Type
api_location:
- http://schemas.microsoft.com/analysisservices/2003/engine
topic_type:
- apiref
f1_keywords:
- AggregationAttribute
helpviewer_keywords:
- AggregationAttribute data type
ms.assetid: 636827c7-938d-4b7d-9827-46da3bc60d9a
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: 05b0f2f1bc4ed517c289cb4e68810265118d9ea7
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48078670"
---
# <a name="aggregationattribute-data-type-assl"></a>AggregationAttribute-Datentyp (ASSL)
  Definiert einen Grunddatentyp, der die Zuordnung zwischen einem [Aggregation](../objects/aggregation-element-assl.md) Element und ein Attribut.  
  
## <a name="syntax"></a>Syntax  
  
```xml  
  
<AggregationAttribute>  
   <AttributeID>...</AttributeID>  
      <Annotations>...</Annotations>  
</AggregationAttribute>  
```  
  
## <a name="data-type-characteristics"></a>Datentypmerkmale  
  
|Merkmal|Description|  
|--------------------|-----------------|  
|Basisdatentypen|None|  
|Abgeleitete Datentypen|None|  
  
## <a name="data-type-relationships"></a>Datentypbeziehungen  
  
|Beziehung|Element|  
|------------------|-------------|  
|Übergeordnete Elemente|None|  
|Untergeordnete Elemente|[AttributeID](../properties/id-element-assl.md), [Anmerkungen](../collections/annotations-element-assl.md)|  
|Abgeleitete Elemente|[Attribut](../objects/attribute-element-assl.md) ([Attribute](../collections/attributes-element-assl.md) Auflistung von [AggregationDimension](dimension-data-type-assl.md))|  
  
## <a name="remarks"></a>Hinweise  
 Die entsprechende Klasse im Analysis Management Objects (AMO)-Objektmodell ist <xref:Microsoft.AnalysisServices.AggregationAttribute>.  
  
## <a name="see-also"></a>Siehe auch  
 [Aggregation-Element &#40;ASSL&#41;](../objects/aggregation-element-assl.md)   
 [Analysis Services Scripting Language-XML-Datentypen &#40;ASSL&#41;](analysis-services-scripting-language-xml-data-types-assl.md)  
  
  
