---
title: Abonnements initialisieren | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- replication
ms.topic: conceptual
f1_keywords:
- sql12.rep.newsubwizard.initializesubscriptions.f1
ms.assetid: 7b170e4e-470d-4828-a9ed-7435b0b03514
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 19b520bc5f977e3d6d1c56919f75f4cfa5195ce2
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48156310"
---
# <a name="initialize-subscriptions"></a>Abonnements initialisieren
  Bevor Abonnenten replizierte Daten empfangen können, müssen sie initialisiert werden. Zwar ist kein Anfangsdataset erforderlich, jedoch müssen für den Abonnenten mindestens das Schema für jedes replizierte Objekt sowie alle für die Replikation benötigten Metadatentabellen und -prozeduren vorhanden sein.  
  
## <a name="options"></a>Tastatur  
 **Abonnementeigenschaften**  
 Aktivieren Sie dieses Kontrollkästchen in der **Initialisieren** -Spalte für jeden Abonnenten, für den ein Anfangsdataset erforderlich ist. Wenn das Kontrollkästchen deaktiviert ist, werden nur die Metadaten und Prozeduren für die Replikation initialisiert. Weitere Informationen über das Initialisieren eines Abonnements ohne Momentaufnahmen finden Sie unter [Initialisieren eines Transaktionsabonnements ohne Momentaufnahme](initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 Wählen Sie aus der Dropdownliste in der **Initialisierungszeitpunkt** -Spalte die Option **Sofort** aus, wenn die Momentaufnahmedateien vom Merge- oder Verteilungs-Agent unmittelbar nach dem Abschließen des Assistenten an den Abonnenten übertragen werden sollen. Wählen Sie **Bei der ersten Synchronisierung** aus, wenn die Dateien bei der nächsten laut Zeitplan festgelegten Ausführung der Momentaufnahme übertragen werden sollen. Die Option **Sofort** steht für Pullabonnements in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]nicht zur Verfügung. Da der Merge-Agent und der Verteilungs-Agent für Instanzen von [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]nicht ausgeführt werden können, muss das Abonnement durch eine andere Methode initialisiert werden.  
  
> [!NOTE]  
>  Der Assistent fordert möglicherweise zur Eingabe einer Verbindung mit dem Verteiler auf, um den entsprechenden Auftrag für den Verteilungs- oder Merge-Agent zu starten.  
  
## <a name="see-also"></a>Siehe auch  
 [Create a Pull Subscription](create-a-pull-subscription.md)   
 [Create a Push Subscription](create-a-push-subscription.md)   
 [Initialisieren eines Abonnements](initialize-a-subscription.md)   
 [Subscribe to Publications](subscribe-to-publications.md)  
  
  
