---
title: Veraltete Master Data Services-Funktionen in SQLServer 2014 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- master-data-services
ms.topic: conceptual
ms.assetid: d8506bda-66dd-45a4-bfc9-3a10fa665acc
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 89d2d5d8cee989d6541cdb256b0a7aaf48d00162
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48142211"
---
# <a name="deprecated-master-data-services-features-in-sql-server-2014"></a>Veraltete Master Data Services-Funktionen in SQL Server 2014
  In diesem Thema werden die als veraltet markierten Funktionen von [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] beschrieben, die in [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]noch verfügbar sind. Diese Funktionen werden voraussichtlich in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]entfernt. Als veraltet markierte Funktionen sollten in neuen Anwendungen nicht verwendet werden.  
  
## <a name="staging-process"></a>Stagingprozess  
 Der Stagingprozess, der in [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] verwendet wurde, ist nicht mehr in der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Webanwendung verfügbar; er ist jedoch immer noch in [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]verfügbar.  
  
 Stagingfehler aus dem [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] -Stagingprozess werden nicht mehr in der Benutzeroberfläche angezeigt. Fehlercodes, die während des Stagingprozesses aufgefüllt werden weiterhin in den Stagingtabellen verfügbar sind, und finden Sie hier: [ http://msdn.microsoft.com/library/ff487022.aspx ](http://msdn.microsoft.com/library/ff487022.aspx).  
  
 Die Stagingtabellen (tblStgMember, tblStgMemberAttribute und tblStgRelationship) sind nach wie vor in der Datenbank verfügbar. Die gespeicherte Prozedur, die verwendet wurde, um den Stagingprozess (mdm.udpStagingSweep) zu initiieren, ist nach wie vor in der Datenbank verfügbar.  
  
 Die Webdienstmethoden, die den Stagingprozess aufrufen, sind immer noch verfügbar.  
  
 Das in [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] festgelegte Stagingintervall gilt für den Stagingprozess sowohl in [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] als auch in [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  
  
 Ein neuer, leistungsstärkerer Stagingprozess wurde in [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]implementiert. Weitere Informationen finden Sie unter [Datenimport &#40;Master Data Services&#41;](overview-importing-data-from-tables-master-data-services.md).  
  
## <a name="metadata"></a>Metadaten  
 Obwohl das Metadatenmodell immer noch in der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Webanwendung angezeigt wird, sollte es nicht verwendet werden. Dieses Verhalten wird in einer zukünftigen Version entfernt. Benutzer können zudem keine Metadaten mehr im Funktionsbereich **Explorer** anzeigen, und Sie können keine Versionen des Metadatenmodells mehr erstellen.  
  
## <a name="see-also"></a>Siehe auch  
 [Eingestellte Master Data Services-Features in SQL Server 2014](discontinued-master-data-services-features.md)  
  
  
