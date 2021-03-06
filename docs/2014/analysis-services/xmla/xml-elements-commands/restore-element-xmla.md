---
title: Restore-Element (XMLA) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
- docset-sql-devref
ms.topic: reference
api_name:
- Restore Element
api_location:
- http://schemas.microsoft.com/analysisservices/2003/engine
topic_type:
- apiref
f1_keywords:
- http://schemas.microsoft.com/analysisservices/2003/engine#Restore
- urn:schemas-microsoft-com:xml-analysis#Restore
- microsoft.xml.analysis.restore
helpviewer_keywords:
- Restore command
ms.assetid: bb5a0c92-3927-4fa4-975b-6e4d79e0a912
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: df3e4814b0dafadd8ba7a5c6f572fba7d48e6d4a
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48094660"
---
# <a name="restore-element-xmla"></a>Restore-Element (XMLA)
  Stellt eine [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] Datenbank aus einer Sicherungsdatei.  
  
## <a name="syntax"></a>Syntax  
  
```xml  
  
<Command>  
   <Restore>  
      <DatabaseName>...</DatabaseName>  
      <DatabaseID>...</DatabaseID>  
      <File>...</File>  
      <Security>...</Security>  
      <AllowOverwrite>...</AllowOverwrite>  
      <Password>...</Password>  
      <Locations>...</Locations>  
      <DbStorageLocation>...</DbStorageLocation>  
   </Restore>  
</Command>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|Description|  
|--------------------|-----------------|  
|Datentyp und -länge|None|  
|Standardwert|None|  
|Cardinality|0-n: Optionales Element, das mehr als einmal auftreten kann.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Element|  
|------------------|-------------|  
|Übergeordnete Elemente|[Befehl](../xml-elements-properties/command-element-xmla.md)|  
|Untergeordnete Elemente|[AllowOverwrite](../xml-elements-properties/allowoverwrite-element-xmla.md), [DatabaseName](../xml-elements-properties/name-element-xmla.md), [DatabaseID](../xml-elements-properties/id-element-xmla.md), [Datei](../xml-elements-properties/file-element-xmla.md), [Speicherorte](../xml-elements-properties/locations-element-xmla.md), [Kennwort](../xml-elements-properties/password-element-xmla.md), [Sicherheit](../xml-elements-properties/security-element-xmla.md), [DbStorageLocation](../xml-elements-properties/dbstoragelocation-element.md)|  
  
## <a name="remarks"></a>Hinweise  
 Die `Restore` -Befehl wird eine [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] Datenbank in der `DatabaseName` Element aus einer Sicherungsdatei und optional Remotepartitionen aus remotesicherungsdateien wiederhergestellt.  
  
 Je nach dem Speichermodus der in der Sicherungsdatei gespeicherten Objekte die `Restore` Befehl in der folgenden Tabelle aufgelisteten Informationen wiederhergestellt.  
  
|Speichermodus|Information|  
|------------------|-----------------|  
|Mehrdimensionale OLAP (MOLAP)|Quelldaten, Aggregationen und Metadaten|  
|Hybride OLAP (HOLAP)|Aggregationen und Metadaten|  
|Relationale OLAP (ROLAP)|Metadaten|  
  
 Während ein `Restore` Befehl, eine exklusive Sperre für die [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] Datenbank in der `DatabaseName` Element. Die Sperre wird aufgehoben, nachdem die `Restore` -Befehl ausgeführt wurde.  
  
 Weitere Informationen zum Sichern und Wiederherstellen von Datenbanken finden Sie unter [sichern, wiederherstellen und Synchronisieren von Datenbanken &#40;XMLA&#41;](../../multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md).  
  
> [!IMPORTANT]  
>  Für jede Sicherungsdatei muss der Benutzer, der den Wiederherstellungsbefehl ausführt, über die Berechtigung zum Lesen von dem für jede Datei angegebenen Sicherungsspeicherort verfügen. Zum Wiederherstellen einer [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] -Datenbank, die nicht auf dem Server installiert ist, muss der Benutzer zusätzlich Mitglied der Serverrolle für die betreffende [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] -Instanz sein. Zum Überschreiben einer [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] -Datenbank muss der Benutzer entweder Mitglied der Serverrolle für die [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] -Instanz oder Mitglied einer Datenbankrolle mit den Berechtigungen "Vollzugriff" (Administrator) für die wiederherzustellende Datenbank sein.  
  
> [!NOTE]  
>  Nach dem Wiederherstellen einer vorhandenen Datenbank verliert der Benutzer, der die Datenbank wiederhergestellt hat, möglicherweise den Zugriff auf diese Datenbank. Dies ist u. U. der Fall, wenn der Benutzer zum Zeitpunkt der Sicherung kein Mitglied der Serverrolle oder der Datenbankrolle mit der Berechtigung "Vollzugriff (Administrator)" war.  
  
## <a name="see-also"></a>Siehe auch  
 [Sichern des Elements &#40;XMLA&#41;](backup-element-xmla.md)   
 [Batch-Element &#40;XMLA&#41;](batch-element-xmla.md)   
 [Parallele Element &#40;XMLA&#41;](../xml-elements-properties/parallel-element-xmla.md)   
 [Synchronize-Element &#40;XMLA&#41;](synchronize-element-xmla.md)   
 [Befehle &#40;XMLA&#41;](xml-elements-commands.md)  
  
  
