---
title: ProcessingMode-Element (ASSL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
- docset-sql-devref
ms.topic: reference
api_name:
- ProcessingMode Element
api_location:
- http://schemas.microsoft.com/analysisservices/2003/engine
topic_type:
- apiref
f1_keywords:
- ProcessingMode
helpviewer_keywords:
- ProcessingMode element
ms.assetid: dff6eeba-f09c-4d8c-ad81-caef76254af0
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: 550b19dc291818052b954b476d3673fd717d38ac
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48148600"
---
# <a name="processingmode-element-assl"></a>ProcessingMode-Element (ASSL)
  Gibt an, ob die Instanz während oder nach der Verarbeitung indizieren und aggregieren soll.  
  
## <a name="syntax"></a>Syntax  
  
```xml  
  
<Cube> <!-- or Dimension, MeasureGroup, Partition -->  
   ...  
   <ProcessingMode>...</ProcessingMode>  
   ...  
</Cube>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|Description|  
|--------------------|-----------------|  
|Datentyp und -länge|Zeichenfolge (Enumeration)|  
|Standardwert|*Reguläre*|  
|Cardinality|0-1: Optionales Element, das nur einmal auftreten kann.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Element|  
|------------------|-------------|  
|Übergeordnete Elemente|[Cube](../objects/cube-element-assl.md), [Dimension](../objects/dimension-element-assl.md), [MeasureGroup](../objects/group-element-assl.md), [Partition](../objects/partition-element-assl.md)|  
|Untergeordnete Elemente|None|  
  
## <a name="remarks"></a>Hinweise  
 Der Wert des `ProcessingMode` auf die `Cube` ist die Standardeinstellung für den Cube und kann überschrieben werden, indem `ProcessingMode` für jede Partition.  
  
 Der Wert dieses Elements ist auf eine der in der folgenden Tabelle aufgelisteten Zeichenfolgen beschränkt.  
  
|value|Description|  
|-----------|-----------------|  
|*Reguläre*|Die Instanz indiziert Aggregationen und führt diese während der Verarbeitung aus.|  
|*LazyOptimizations*|Die Instanz indiziert Aggregationen und führt diese nach der Verarbeitung aus.|  
  
 Die Enumeration, die den zulässigen Werten für entspricht `ProcessingMode` im Objekt Analysis Management Objects (AMO) Modell ist <xref:Microsoft.AnalysisServices.ProcessingMode>.  
  
## <a name="see-also"></a>Siehe auch  
 [Eigenschaften &#40;ASSL&#41;](properties-assl.md)  
  
  
