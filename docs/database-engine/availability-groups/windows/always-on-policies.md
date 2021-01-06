---
title: Verwenden von Gruppenrichtlinien für die Integrität von Verfügbarkeitsgruppen
description: Hier erfahren Sie, wie Sie die Systemrichtlinien für Gruppen anzeigen, die das Always On-Dashboard verwendet, um Informationen zur Integrität der Verfügbarkeitsgruppe bereitzustellen.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 26bf8f71-c2b8-45ef-b3a3-372b96c9e6e3
author: rothja
ms.author: jroth
ms.openlocfilehash: f2f6357e434e4a8d4d6385de10cac58c7bd6f2d0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641432"
---
# <a name="evaluate-health-of-the-always-on-availability-group-using-group-policies"></a>Evaluieren der Integrität der Always On-Verfügbarkeitsgruppe mit Gruppenrichtlinien
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Die Systemrichtlinien für Always On-Verfügbarkeitsgruppen werden vom Always On-Dashboard verwendet, um dem Benutzer Informationen zur Integrität der Verfügbarkeitsgruppe bereitzustellen. Diese sind bei der ersten Problembehandlung von Betriebsproblemen von Verfügbarkeitsgruppen nützlich. Diese Richtlinien können zur Anpassung des Always On-Dashboards erweitert werden oder sofort ausgeführt werden, um die gewünschten Integritätsinformationen bereitzustellen.  
  
 Es gibt 14 Systemrichtlinien für Verfügbarkeitsgruppen. Ausführliche Informationen zu jeder Richtlinie finden Sie unter [Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen (SQL Server)](always-on-policies-for-operational-issues-always-on-availability.md).  
  
## <a name="view-or-evaluate-availability-groups-system-policies"></a>Anzeigen und Auswerten von Systemrichtlinien von Verfügbarkeitsgruppen  
 Führen Sie Folgendes durch, um Systemrichtlinien von Verfügbarkeitsgruppen in SQL Server Management Studio (SSMS) anzuzeigen oder auszuführen:  
  
1.  Erweitern Sie in **Objekt-Explorer** **Verwaltung**, **Richtlinienverwaltung**, **Richtlinien**, und wählen Sie dann **Systemrichtlinien** aus.  
  
2.  Klicken Sie mit der rechten Maustaste auf eine der Richtlinien, und klicken Sie dann auf **Auswerten**. Wenn Sie die ausgewählte Richtlinie auswerten möchten, müssen Sie keine weitere Aktion durchführen. Sie können im Dialogfeld **Zieldetails** auf **Anzeigen** klicken, um Details zu den Ergebnissen der Auswertung anzuzeigen.  
  
3.  Klicken Sie im Bereich **Seite auswählen** auf **Richtlinienauswahl**, um alle Systemrichtlinien der Verfügbarkeitsgruppen anzuzeigen.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [The Always On Health Model Part 2 -- Extending the Health Model (Das Always On-Zustandsmodell, Teil 2: Erweitern des Zustandsmodells)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)   
  
