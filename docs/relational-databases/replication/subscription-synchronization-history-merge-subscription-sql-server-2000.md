---
title: Abonnement, Synchronisierungsverlauf (Mergeabonnement, SQL Server 2000) | Microsoft Dokumentation
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.subscription.downlevelsynchhistory.f1
ms.assetid: 0a0deab2-1c08-4371-9681-d9403e0236cc
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: acf0a3810fd53b695a9ea7f44c6766601d32733a
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47603508"
---
# <a name="subscription-synchronization-history-merge-subscription-sql-server-2000"></a>Abonnement, Synchronisierungsverlauf (Mergeabonnement, SQL Server 2000)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Die Registerkarte **Synchronisierungsverlauf** zeigt detaillierte Informationen zum Merge-Agent an, u. a. Status, Verlauf, Informationsmeldungen und alle Fehlermeldungen.  
  
## <a name="options"></a>Tastatur  
 Wählen Sie im Menü **Sicht** aus, welche Sitzungen des Merge-Agents angezeigt werden, und wählen Sie dann eine bestimmte Sitzung aus dem Raster mit der Bezeichnung **Sitzungen des Merge-Agents**aus. Detaillierte Informationen zu dieser Sitzung werden im Raster mit der Bezeichnung **Aktionen in der ausgewählten Sitzung**angezeigt. Wenn die ausgewählte Sitzung mit einem Fehler beendet wurde, wird auch der Textbereich mit der Bezeichnung **Fehlerdetails oder Meldung der ausgewählten Sitzung** angezeigt.  
  
 **Ansicht**  
 Wählen Sie aus, welche Sitzungen des Merge-Agents angezeigt werden. Da der Merge-Agent normalerweise kontinuierlich ausgeführt wird, wird gegebenenfalls nur eine Sitzung angezeigt.  
  
 **Status**  
 Der Status des Merge-Agents. In der folgenden Liste sind die möglichen Statuswerte aufgeführt:  
  
-   Fehler  
  
-   Abgeschlossen  
  
-   Wird wiederholt  
  
-   Wird ausgeführt  
  
 **Startzeit**  
 Startzeit der Sitzung.  
  
 **Beendigungszeit**  
 Beendigungszeit der Sitzung. Wenn der Agent noch nicht beendet wurde, ist dieses Feld leer.  
  
 **Dauer**  
 Zeitspanne, für die der Merge-Agent in dieser Sitzung ausgeführt wurde. Die Zeit stellt die bisher abgelaufene Zeit dar, wenn der Agent immer noch ausgeführt wird, und die Gesamtzeit, wenn die Agentsitzung beendet ist.  
  
 **Fehlermeldung**  
 Wenn eine Sitzung mit einem Fehler beendet wurde, wird in diesem Feld die Fehlermeldung angezeigt, die vom Merge-Agent zuletzt protokolliert wurde. Wurde die Sitzung nicht mit einem Fehler beendet, ist dieses Feld leer.  
  
 **Aktionsmeldung**  
 Alle Informationsmeldungen und Fehlermeldungen, die der Merge-Agent während der ausgewählten Sitzung protokolliert hat.  
  
 **Aktionszeit**  
 Zeit, zu der die unter **Aktionsmeldung** beschriebene Aktion ausgeführt wurde.  
  
 **Fehlerdetails oder Meldung der ausgewählten Sitzung**  
 Wird nur angezeigt, wenn in der ausgewählten Sitzung in der **Status** -Spalte der Wert **Fehler** angezeigt wird. Dieser Textbereich zeigt detaillierte Fehlerinformationen sowie den Befehl an, der zum Zeitpunkt des Fehlers auszuführen versucht wurde. Er enthält außerdem Links zu weiteren Informationen, die sich auf den Fehler beziehen.  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
 [Starten des Replikationsmonitors](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Anzeigen von Informationen und Ausführen von Aufgaben für die einem Abonnement zugeordneten Agents &#40;Replikationsmonitor&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md)   
 [Überwachen der Replikation](../../relational-databases/replication/monitor/monitoring-replication-overview.md)   
 [Übersicht über Replikations-Agents](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
