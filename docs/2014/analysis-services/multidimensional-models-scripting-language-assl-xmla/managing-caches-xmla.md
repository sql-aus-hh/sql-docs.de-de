---
title: Verwalten von Caches (XMLA) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
- docset-sql-devref
ms.topic: reference
helpviewer_keywords:
- XMLA, cache
- XML for Analysis, cache
- clearing cache
- cache [Analysis Services]
ms.assetid: afad5c39-d4c3-4307-b3b9-a06617da0028
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: 7e99be9f6a2af7dbbaab624ba592b2486cf807f2
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48048160"
---
# <a name="managing-caches-xmla"></a>Verwalten von Caches (XMLA)
  Sie können die [ClearCache](../xmla/xml-elements-commands/clearcache-element-xmla.md) -Befehl in XML for Analysis (XMLA) zum Löschen des Caches von einer angegebenen Dimension oder Partition. Das Löschen des Cache zwingt [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] den Cache für dieses Objekt neu erstellen.  
  
## <a name="specifying-objects"></a>Angeben von Objekten  
 Die [Objekt](../xmla/xml-elements-properties/object-element-xmla.md) Eigenschaft der `ClearCache` Befehl kann einen Objektverweis nur für eine der folgenden Objekte enthalten. Bei einem Objektverweis, der sich nicht auf eines der folgenden Objekte bezieht, tritt ein Fehler auf:  
  
 Datenbank  
 Löscht den Cache für alle Dimensionen und Partitionen, die in der Datenbank enthalten sind.  
  
 Dimension  
 Löscht den Cache für die angegebene Dimension.  
  
 Cube  
 Löscht den Cache für alle Partitionen, die in den Measuregruppen für den Cube enthalten sind.  
  
 Measuregruppe  
 Löscht den Cache für alle Partitionen, die in der Measuregruppe enthalten sind.  
  
 Partition  
 Löscht den Cache für die angegebene Partition.  
  
## <a name="see-also"></a>Siehe auch  
 [Entwickeln mit XMLA in Analysis Services](developing-with-xmla-in-analysis-services.md)  
  
  
