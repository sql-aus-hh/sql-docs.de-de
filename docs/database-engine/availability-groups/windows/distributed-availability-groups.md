---
title: Verteilte Verfügbarkeitsgruppen
description: Eine verteilte Verfügbarkeitsgruppe ist ein spezieller Typ einer Verfügbarkeitsgruppe, der zwei separate Verfügbarkeitsgruppen umfasst. Die Verfügbarkeitsgruppen, die Teil einer verteilten Verfügbarkeitsgruppe sind, müssen sich nicht am selben Ort befinden.
ms.custom: seodec18
ms.date: 10/15/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], distributed
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0b6a1c97a1a5958811111915117793da8c416a0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643627"
---
# <a name="distributed-availability-groups"></a>Verteilte Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Verteilte Verfügbarkeitsgruppen sind eine neue Funktion, die in SQL Server 2016 als Variation der bestehenden Funktion für Always On-Verfügbarkeitsgruppen eingeführt. Dieser Artikel erläutert einige Aspekte von verteilten Verfügbarkeitsgruppen und ergänzt die bestehende [SQL Server documentation (SQL Server-Dokumentation)](../../../sql-server/index.yml).

> [!NOTE]
> „DAG“ ist nicht die offizielle Abkürzung für *verteilte Verfügbarkeitsgruppe*, da die Abkürzung bereits für die Database Availability Group-Exchange-Funktion verwendet wird. Diese Exchange-Funktion steht nicht in Zusammenhang mit den Verfügbarkeitsgruppen oder verteilten Verfügbarkeitsgruppen in SQL Server.

Informationen zum Konfigurieren einer verteilten Verfügbarkeitsgruppe finden Sie unter [Configure distributed availability groups (Konfigurieren von verteilten Verfügbarkeitsgruppen)](configure-distributed-availability-groups.md).

## <a name="understand-distributed-availability-groups"></a>Grundlegendes zu verteilten Verfügbarkeitsgruppen

Eine verteilte Verfügbarkeitsgruppe ist ein spezieller Typ einer Verfügbarkeitsgruppe, der zwei separate Verfügbarkeitsgruppen umfasst. Die Verfügbarkeitsgruppen, die Teil einer verteilten Verfügbarkeitsgruppe sind, müssen sich nicht am selben Ort befinden. Sie können physisch, virtuell, lokal, in der öffentlichen Cloud oder an einem sonstigen Ort sein, der die Bereitstellung von Verfügbarkeitsgruppen unterstützt. Dazu zählen domänen- und sogar plattformübergreifende Orte, z.B. zwischen einer Verfügbarkeitsgruppe, die unter Linux gehostet wird, und einer anderen, die unter Windows gehostet wird. Solange zwei Verfügbarkeitsgruppen kommunizieren können, können Sie mit diesen eine verteilte Verfügbarkeitsgruppe konfigurieren.

Die Ressourcen einer herkömmlichen Verfügbarkeitsgruppe werden in einem WSFC-Cluster konfiguriert. Eine verteilte Verfügbarkeitsgruppe kann keine Konfigurationen in einem WSFC-Cluster vornehmen. Sie wird vollständig in SQL Server verwaltet. Informationen zum Anzeigen der Informationen für eine verteilte Verfügungsgruppe finden Sie unter [Viewing distributed availability group information (Anzeigen der Informationen für eine verteilte Verfügbarkeitsgruppe)](#monitor-distributed-availability-group-health). 

Bei einer verteilten Verfügbarkeitsgruppe ist erforderlich, dass die zugrunde liegenden Verfügbarkeitsgruppen über einen Listener verfügen. Anstatt den zugrundeliegenden Servernamen für eine eigenständige Instanz (oder bei einer SQL Server-Failoverclusterinstanz (FCI) den Wert, der der Netzwerknamenressource zugeordnet ist) bereitzustellen, wie es bei einer herkömmlichen Verfügbarkeitsgruppe üblich ist, geben Sie beim Erstellen den für die verteilte Verfügbarkeitsgruppe konfigurierten Listener mithilfe des Parameters „ENDPOINT_URL“ an. Obwohl jede zugrunde liegende Verfügbarkeitsgruppe der verteilten Verfügbarkeitsgruppe über einen Listener verfügt, besitzt eine verteilte Verfügbarkeitsgruppe nicht über einen Listener.

In der folgenden Abbildung finden Sie eine Ansicht auf höchster Ebene für eine verteilte Verfügbarkeitsgruppe, die zwei Verfügbarkeitsgruppen (AG 1 und AG 2) umfasst, von der jede in ihrem eigenen WSFC-Cluster konfiguriert wurde. Die verteilte Verfügbarkeitsgruppe besitzt zwei Replikate in jeder Verfügbarkeitsgruppe, also insgesamt vier Replikate. Jede Verfügbarkeitsgruppe kann die maximale Anzahl von Replikaten unterstützen. Eine verteilte Verfügbarkeitsgruppe kann also insgesamt bis zu 18 Replikate einschließen.


![Allgemeine Ansicht einer verteilten Verfügbarkeitsgruppe](./media/distributed-availability-group/dag-01-high-level-view-distributed-ag.png)

Sie können die Datenverschiebung in verteilten Verfügbarkeitsgruppen als „synchron“ oder „asynchron“ konfigurieren. Die Datenverschiebung innerhalb von verteilten Verfügbarkeitsgruppen weicht jedoch geringfügig von der in einer herkömmlichen Verfügbarkeitsgruppe ab. Obwohl jede Verfügbarkeitsgruppe über ein primäres Replikat verfügt, kann nur eine Kopie der Datenbanken, die Teil einer verteilten Verfügbarkeitsgruppe sind, Einfügungen, Updates und Löschungen akzeptieren. AG 1 ist die primäre Verfügbarkeitsgruppe, wie in der folgenden Abbildung dargestellt. Das primäre Replikat sendet Transaktionen an das sekundäre Replikat von AG 1 und an das primäre Replikat von AG 2. Das primäre Replikat von AG 2 ist auch als *Weiterleitung* bekannt. Eine Weiterleitung ist ein primäres Replikat in einer sekundären Verfügbarkeitsgruppe in einer verteilten Verfügbarkeitsgruppe. Die Weiterleitung empfängt Transaktionen vom primären Replikat in der primären Verfügbarkeitsgruppe und leitet diese an die sekundären Replikaten in der eigenen Verfügbarkeitsgruppe weiter.  Die Weiterleitung hält dann die sekundären Replikate von AG 2 auf dem neuesten Stand. 

![Datenverschiebung in verteilten Verfügbarkeitsgruppen](./media/distributed-availability-group/dag-02-distributed-ag-data-movement.png)

Die einzige Möglichkeit, um dem primären Replikat von AG 2 das Akzeptieren von Einfügungen, Updates und Löschungen zu ermöglichen, ist das Auslösen eines manuellen Failovers der verteilten Verfügbarkeitsgruppe von AG 1. In der obigen Abbildung enthält AG 1 die beschreibbare Kopie der Datenbank, deshalb macht das Auslösen eines Failovers AG 2 zu der Verfügbarkeitsgruppe, die Einfügungen, Updates und Löschungen verarbeiten kann. Weitere Informationen dazu, wie eine verteilte Verfügbarkeitsgruppe ein Failover auf eine andere auslösen kann, finden Sie unter [Failover to a secondary availability group (Failover auf eine sekundäre Verfügbarkeitsgruppe)](configure-distributed-availability-groups.md#failover).

> [!NOTE]
> Verteilte Verfügbarkeitsgruppen in SQL Server 2016 unterstützen die Failover-Funktion nur von einer Verfügbarkeitsgruppe auf eine andere mithilfe der Option „FORCE_FAILOVER_ALLOW_DATA_LOSS“.

> [!NOTE]
> Wenn Sie die Transaktionsreplikation mit verteilten Verfügbarkeitsgruppen verwenden, kann das Weiterleitungsreplikat nicht als Verleger konfiguriert werden.

## <a name="sql-server-version-and-edition-requirements-for-distributed-availability-groups"></a>Anforderungen für die SQL Server-Version und -Edition für verteilte Verfügbarkeitsgruppen

Verteilte Verfügbarkeitsgruppen in SQL Server 2017 oder höher können verschiedene Hauptversionen von SQL Server in der gleichen verteilten Verfügbarkeitsgruppe kombinieren. Die Verfügbarkeitsgruppe mit dem primärem Replikat für Lese-/Schreibzugriff kann dieselbe oder eine niedrigere Version als die anderen Verfügbarkeitsgruppen aufweisen, die Teil der verteilten Verfügbarkeitsgruppe sind. Die anderen Verfügbarkeitsgruppen können dieselbe oder eine höhere Version aufweisen. Dieses Szenario gilt für Upgrade- und Migrationsszenarios. Wenn die Verfügbarkeitsgruppe, die das primäre Replikat für Lese-/Schreibzugriff enthält, beispielsweise SQL Server 2016 aufweist, Sie jedoch per Migration oder Upgrade zu SQL Server 2017 oder höher wechseln möchten, können die anderen Verfügbarkeitsgruppen in der verteilten Verfügbarkeitsgruppe mit SQL Server 2017 konfiguriert werden.

Da die Funktion für verteilte Verfügbarkeitsgruppen in SQL Server 2012 oder 2014 noch nicht existiert hat, können Verfügbarkeitsgruppen, die mit diesen Versionen erstellt wurden, nicht Teil von verteilten Verfügbarkeitsgruppen werden. 

> [!NOTE]
> Verteilte Verfügbarkeitsgruppen können nicht mit der Standard Edition oder mit einer Kombination aus Standard Edition und Enterprise Edition konfiguriert werden.

Da zwei separate Verfügbarkeitsgruppen vorliegen, weicht der Installationsprozess für ein Service Pack oder ein kumulatives Update auf ein Replikat, das Teil einer verteilten Verfügbarkeitsgruppe ist, geringfügig von dem für eine herkömmliche Verfügbarkeitsgruppe ab:

1. Beginnen Sie mit dem Aktualisieren der Replikate der sekundären Verfügbarkeitsgruppe in der verteilten Verfügbarkeitsgruppe.

2. Patchen Sie die Replikate der primären Verfügbarkeitsgruppe in der verteilten Verfügbarkeitsgruppe.

3. Lösen Sie, genau wie bei einer herkömmlichen Verfügbarkeitsgruppe, ein Failover der primären Verfügbarkeitsgruppe auf eins ihrer eigenen Replikate (nicht auf das primäre Replikat der sekundären Verfügbarkeitsgruppe) aus und patchen Sie dieses. Wenn kein anderes als das primäre Replikat vorhanden ist, ist ein manuelles Failover auf die sekundäre Verfügbarkeitsgruppe erforderlich.

> [!NOTE]
> Es gibt bisher keine Ankündigungen dazu, ob zukünftige Versionen von SQL Server es ermöglichen werden, dass frühere Versionen Teil derselben verteilten Verfügbarkeitsgruppe sein können. Wenn dieses Szenario möglich wäre, wären verteilte Verfügbarkeitsgruppen als Teil des Upgradeplans einer SQL Server-Version denkbar.

### <a name="windows-server-versions-and-distributed-availability-groups"></a>Windows Server-Versionen und verteilte Verfügbarkeitsgruppen

Eine verteilte Verfügbarkeitsgruppe umfasst mehrere Verfügbarkeitsgruppen, von denen jede ihren eigenen zugrunde liegenden WSFC-Cluster besitzt. Eine verteilte Verfügbarkeitsgruppe ist ein nur in SQL Server verfügbares Konstrukt.  Das bedeutet, dass die WSFC-Cluster, auf denen sich die einzelnen Verfügbarkeitsgruppen befinden, verschiedene Hauptversionen von Windows Server verwenden können. Die Hauptversionen von SQL müssen, wie im vorherigen Abschnitt erläutert wurde, identisch sein. Ähnlich wie bei der ersten Abbildung zeigt die folgende Abbildung AG 1 und AG 2 als Teil einer verteilten Verfügbarkeitsgruppe, aber jeder der WSFC-Cluster verwendet eine unterschiedliche Version von Windows Server.


![Verteilte Verfügbarkeitsgruppen mit WSFC-Clustern, die unterschiedliche Versionen von Windows Server verwenden](./media/distributed-availability-group/dag-03-distributed-ags-wsfcs-different-versions-windows-server.png)

Für die einzelnen WSFC-Cluster und ihre entsprechenden Verfügbarkeitsgruppen gelten die herkömmlichen Regeln. Das bedeutet, dass sie mit einer Domäne verknüpft werden können oder nicht (Windows Server 2016 und höher). Wenn zwei verschiedene Verfügbarkeitsgruppen in einer einzigen verteilten Verfügbarkeitsgruppe vereint werden, gibt es vier Szenarios:

* Beide WSFC-Cluster werden mit derselben Domäne verknüpft.
* Jeder WSFC-Cluster wird mit einer unterschiedlichen Domäne verknüpft.
* Ein WSFC-Cluster ist mit einer Domäne verknüpft und ein WSFC-Cluster ist nicht mit einer Domäne verknüpft.
* Keiner der WSFC-Cluster ist mit einer Domäne verknüpft.

Wenn beide WSFC-Cluster mit derselben Domäne (gilt nicht für vertrauenswürdige Domänen), müssen Sie beim Erstellen der verteilten Verfügbarkeitsgruppe keine weiteren Schritte durchführen. Bei Verfügbarkeitsgruppen und WSFC-Clustern, die nicht mit derselben Domäne verknüpft sind, müssen Sie Zertifikate verwenden, damit die verteilte Verfügbarkeitsgruppe funktioniert. Dabei gehen Sie so vor wie beim Erstellen einer Verfügbarkeitsgruppe für eine domänenunabhängige Verfügbarkeitsgruppe. Weitere Informationen zum Konfigurieren von Zertifikaten für eine verteilte Verfügbarkeitsgruppe finden Sie in den Schritten 3 bis 13 unter [Create a domain-independent availability group (Erstellen einer domänenunabhängigen Verfügbarkeitsgruppe)](domain-independent-availability-groups.md).

Bei einer verteilten Verfügbarkeitsgruppe müssen die primären Replikate in jeder zugrunde liegenden Verfügbarkeitsgruppe jeweils die Zertifikate der anderen besitzen. Wenn Sie bereits Endpunkte haben, die keine Zertifikate verwenden, konfigurieren Sie diese Endpunkte neu, indem Sie die Anweisung [ALTER ENDPOINT](../../../t-sql/statements/alter-endpoint-transact-sql.md) verwenden, um das Verwenden von Zertifikaten zu berücksichtigen.

## <a name="distributed-availability-group-usage-scenarios"></a>Verwendungsszenarios für verteilte Verfügbarkeitsgruppen

Im Folgenden finden Sie die drei Hauptverwendungsszenarios für eine verteilte Verfügbarkeitsgruppe: 

* [Disaster recovery and easier multi-site configurations (Notfallwiederherstellung und vereinfachte Konfigurationen für mehrere Standorte)](#disaster-recovery-and-multi-site-scenarios)
* [Migration to new hardware or configurations, which might include using new hardware or changing the underlying operating systems (Migration zu neuer Hardware oder neuen Konfigurationen, die möglicherweise das Verwenden neuer Hardware oder das Ändern des zugrunde liegenden Betriebssystems erfordert)](#migrate-by-using-a-distributed-availability-group)
* [Increasing the number of readable replicas beyond eight in a single availability group by spanning multiple availability groups (Erhöhen der Anzahl der lesbaren Replikate auf mehr als acht in einer einzigen Verfügbarkeitsgruppe, indem mehrere Verfügbarkeitsgruppen umfasst werden)](#scale-out-readable-replicas-with-distributed-availability-groups)

### <a name="disaster-recovery-and-multi-site-scenarios"></a>Szenarios für Notfallwiederherstellung und mehrere Standorte

Bei einer herkömmlichen Verfügbarkeitsgruppe ist erforderlich, dass alle Server Teil desselben WSFC-Clusters sind, wodurch das Umfassen mehrerer Rechenzentren eine Herausforderung darstellen kann. Die folgende Abbildung zeigt eine herkömmliche Architektur für eine Verfügbarkeitsgruppe mit mehreren Standorten, einschließlich des Datenflusses. Es gibt ein primäres Replikat, das Transaktionen an alle sekundären Replikate sendet. Diese Konfiguration umfasst weniger Aspekte als eine verteilte Verfügbarkeitsgruppe. Sie müssen beispielsweise Elemente wie Active Directory (sofern vorhanden) und den Zeugen für ein Quorum in den WSFC-Cluster implementieren. Sie müssen gegebenenfalls auch andere Aspekte eines WSFC-Clusters berücksichtigen, zum Beispiel das Ändern von Knotenvoten.

![Herkömmliche Verfügbarkeitsgruppe mit mehreren Standorten](./media/distributed-availability-group/dag-04-traditional-multi-site-ag.png)

Verteilte Verfügbarkeitsgruppen bieten ein flexibleres Bereitstellungsszenario für Verfügbarkeitsgruppen, die mehrere Rechenzentren umfassen. Sie können verteilte Verfügbarkeitsgruppen sogar statt früheren Funktionen, z. B. [Protokollversand]( https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server), für Szenarien wie die Notfallwiederherstellung verwenden. Im Gegensatz zum Protokollversand ist bei verteilten Verfügbarkeitsgruppen die verzögerte Anwendung von Transaktionen jedoch nicht verzögern. Das bedeutet, dass weder Verfügbarkeitsgruppen noch verteilte Verfügbarkeitsgruppen Sie im Fall eines menschlichen Fehlers unterstützen können, wenn beispielsweise Daten falsch aktualisiert oder gelöscht werden.

Verteilte Verfügbarkeitsgruppen sind lose gekoppelt, was in diesem Fall bedeutet, dass sie keinen einzelnen WSFC-Cluster erfordern und von SQL Server verwaltet werden. Da die WSFC-Cluster einzeln verwaltet werden und die Synchronisation zwischen den beiden Verfügbarkeitsgruppen in erster Linie asynchron abläuft, ist die Konfiguration der Notfallwiederherstellung auf einer anderen Website einfacher. Die primären Replikate in jeder Verfügbarkeitsgruppe synchronisieren ihre eigenen sekundären Replikate.

* Für eine verteilte Verfügbarkeitsgruppe wird nur manuelles Failover unterstützt. Im Fall einer Notfallwiederherstellung, bei der Sie die Rechenzentren wechseln müssen, sollten Sie kein automatisches Failover konfigurieren (mit wenigen Ausnahmen). 
* Sie müssen einige der herkömmlichen Elemente oder Parameter für WSFC-Cluster für Subnetze oder mehrere Standorte (z.B: CrossSubnetTreshold) wahrscheinlich nicht festlegen, aber Sie müssen dennoch dafür sorgen, dass die Netzwerklatenz sich für den Datentransport auf einer anderen Ebene befindet. Der Unterschied besteht darin, dass jeder WSFC-Cluster seine eigene Verfügbarkeit verwaltet, der Cluster ist also nicht eine große Entität mit vier Knoten. Sie haben zwei separate WSFC-Cluster mit je zwei Knoten, wie in der vorherigen Abbildung dargestellt.  
* Wir empfehlen eine asynchrone Datenverschiebung, da diese Vorgehensweise für die Notfallwiederherstellung verwendet wird.
* Wenn Sie eine synchrone Datenverschiebung zwischen dem primären Replikat und mindestens einem sekundären Replikat der sekundären Verfügbarkeitsgruppe konfigurieren, und Sie die synchrone Verschiebung auf der verteilten Verfügbarkeitsgruppe konfigurieren, wird die verteilte Verfügbarkeitsgruppe warten, bis alle synchronen Kopien bestätigen, dass sie die Daten empfangen haben.

### <a name="migrate-by-using-a-distributed-availability-group"></a>Migrieren mithilfe einer verteilten Verfügbarkeitsgruppe

Da verteilte Verfügbarkeitsgruppen auch zwei vollkommen unterschiedliche Konfigurationen von Verfügbarkeitsgruppen unterstützen, ermöglichen diese nicht nur eine einfachere Notfallwiederherstellung und Szenarios für mehrere Standorte, sondern auch Migrationsszenarios. Unabhängig davon, ob Sie zu neuer Hardware oder virtuellen Computern (lokal oder durch IaaS in der öffentlichen Cloud) migrieren, ermöglicht eine verteilte Verfügbarkeitsgruppe das Ausführen einer Migration in Fällen, in denen Sie in der Vergangenheit Sicherungen, Kopien und Wiederherstellungen oder Protokollversand verwendet haben. 

Die Migration ist besonders nützlich in Szenarios, in denen Sie das zugrunde liegende Betriebssystem ändern oder aktualisieren, während Sie dieselbe Version von SQL Server behalten. Obwohl Windows Server 2016 ein paralleles Upgrade von Windows Server 2012 R2 auf derselben Hardware zulässt, entscheiden sich die meisten Benutzer für das Bereitstellen von neuer Hardware oder virtuellen Computern. 

Beenden Sie am Ende des Migrationsprozesses den gesamten Datenverkehr zur ursprünglichen Verfügbarkeitsgruppe, und ändern Sie die verteilte Verfügbarkeitsgruppe zur synchronen Datenverschiebung, um die Migration zur neuen Konfiguration abzuschließen. Diese Aktion gewährleistet, dass das primäre Replikat der sekundären Verfügbarkeitsgruppe vollständig synchronisiert wird, sodass keine Daten verloren gehen. Führen Sie ein Failover der verteilten Verfügbarkeitsgruppe auf die sekundäre Verfügbarkeitsgruppe aus, nachdem Sie die Synchronisierung überprüft haben. Weitere Informationen finden Sie unter [Fail over to a secondary availability group (Failover auf eine sekundäre Verfügbarkeitsgruppe)](configure-distributed-availability-groups.md#failover).

Nach der Migration ist die sekundäre Verfügbarkeitsgruppe nun die neue primäre Verfügbarkeitsgruppe, und Sie müssen eventuell eine der folgenden Aktionen durchführen:

* Benennen Sie den Listener auf der sekundären Verfügbarkeitsgruppe um (und löschen oder benennen Sie den alten Listener auf der ursprünglichen primären Verfügbarkeitsgruppe nach Möglichkeit um), oder erstellen Sie ihn mithilfe des Listeners auf der ursprünglichen primären Verfügbarkeitsgruppe neu, sodass Anwendungen und Benutzer auf die neue Konfiguration zugreifen können.
* Wenn eine Umbenennung oder Neuerstellung nicht möglich ist, verweisen Sie Anwendungen und Benutzer auf den Listener auf der sekundären Verfügbarkeitsgruppe.

### <a name="scale-out-readable-replicas-with-distributed-availability-groups"></a>Aufskalieren von lesbaren Replikaten mit verteilten Verfügbarkeitsgruppen

Eine einzelne verteilte Verfügbarkeitsgruppe kann je nach Bedarf bis zu 16 sekundäre Replikate ausweisen. Sie kann also bis zu 18 Kopien zum Lesen besitzen, einschließlich der zwei primären Replikat der unterschiedlichen Verfügbarkeitsgruppen. Dieser Ansatz bedeutet, dass mehr als eine Website beinahe Echtzeitzugriff haben kann, um Berichte an zahlreiche Anwendungen zu senden.

Verteilte Verfügbarkeitsgruppen bieten Ihnen ein besseres Aufskalieren einer schreibgeschützten Farm als eine einzige Verfügbarkeitsgruppe. Für das Aufskalieren von lesbaren Replikaten mit einer verteilten Verfügbarkeitsgruppe gibt es zwei Möglichkeiten:

* Sie können das primäre Replikat der sekundären Verfügbarkeitsgruppe in einer verteilten Verfügbarkeitsgruppe verwenden, um eine weitere verteilte Verfügbarkeitsgruppe zu erstellen, obwohl sich die Datenbank nicht um Status „RECOVERY“ befindet.
* Sie können auch das primäre Replikat der primären Verfügbarkeitsgruppe verwenden, um eine weitere verteilte Verfügbarkeitsgruppe zu erstellen.

Das heißt, ein primäres Replikat kann Teil von zwei unterschiedlichen verteilten Verfügbarkeitsgruppen sein. Die folgende Abbildung zeigt AG 1 und AG 2, die beide Teil der verteilten AG 1 sind, während AG 2 und AG 3 Teil der verteilten AG 2 sind. Das primäre Replikat (oder die Weiterleitung) von AG 2 ist sowohl ein sekundäres Replikat für die verteilte AG 1 als auch ein primäres Replikat der verteilten AG 2.

![Horizontales Hochskalieren von Lesevorgängen mithilfe von verteilten Verfügbarkeitsgruppen](./media/distributed-availability-group/dag-05-scaling-out-reads-with-distributed-ags.png)

Die folgende Abbildung zeigt AG 1 als das primäre Replikat von zwei unterschiedliche verteilte Verfügbarkeitsgruppen: von der verteilten AG 1 (bestehend aus AG 1 und AG 2) und der verteilten AG 2 (bestehend aus AG 1 und AG 3).


![Beispiel: eine weitere Möglichkeit zum horizontalen Hochskalieren von Lesevorgängen mithilfe von verteilten Verfügbarkeitsgruppen]( ./media/distributed-availability-group/dag-06-another-scaling-out-reads-using-distributed-ags-example.png)


In den beiden obigen Beispielen können insgesamt bis zu 27 Replikate in den drei Verfügbarkeitsgruppen vorhanden sein, von denen jedes für schreibgeschützte Abfragen verwendet werden kann. 

Das [schreibgeschützte Routing]( https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server) funktioniert nicht vollständig mit verteilten Verfügbarkeitsgruppen. Dies umfasst insbesondere Folgendes:

1. kann das schreibgeschützte Routing konfiguriert werden, um für die primäre Verfügbarkeitsgruppe der verteilten Verfügbarkeitsgruppe zu funktionieren. 
2. Das schreibgeschützte Routing kann konfiguriert werden, funktioniert jedoch nicht für die sekundäre Verfügbarkeitsgruppe der verteilten Verfügbarkeitsgruppe. Alle Abfragen, die den Listener verwenden, um eine Verbindung mit der sekundären Verfügbarkeitsgruppe herzustellen, werden an das primäre Replikat der sekundären Verfügbarkeitsgruppe weitergeleitet. Andernfalls müssen Sie jedes Replikat so konfigurieren, dass alle Verbindungen als sekundäres Replikat zugelassen werden und direkt auf diese zugegriffen wird. Das schreibgeschützte Routing funktioniert jedoch, wenn die sekundäre Verfügbarkeitsgruppe nach einem Failover zur primären Verfügbarkeitsgruppe wird. Dieses Verhalten wird mit einem Update zu SQL Server 2016 oder in einer zukünftigen Version von SQL Server möglicherweise geändert.


## <a name="initialize-secondary-availability-groups-in-a-distributed-availability-group"></a>Initialisieren von sekundären Verfügbarkeitsgruppen in eine verteilte Verfügbarkeitsgruppe

Verteilte Verfügbarkeitsgruppen wurden mit [automatischem Seeding](./automatically-initialize-always-on-availability-group.md) als Hauptmethode zum Initialisieren des primären Replikats auf die sekundäre Verfügbarkeitsgruppe entwickelt. Eine vollständige Wiederherstellung einer Datenbank auf dem primären Replikat der sekundären Verfügbarkeitsgruppe ist möglich, wenn Sie folgende Aktionen durchführen:

1. Stellen Sie die Datenbanksicherung mit „WITH NORECOVERY“ wieder her.
2. Falls nötig, stellen Sie die richtigen Sicherungen der Transaktionsprotokolle mit „WITH NORECOVERY“ wieder her.
3. Erstellen Sie die sekundäre Verfügbarkeitsgruppe ohne einen Datenbanknamen anzugeben, und legen Sie SEEDING_MODE auf AUTOMATIC fest.
4. Erstellen Sie die verteilte Verfügbarkeitsgruppe mithilfe von automatischem Seeding.

Wenn Sie das primäre Replikat der sekundären Verfügbarkeitsgruppe zu der verteilten Verfügbarkeitsgruppe hinzufügen, wird das Replikat mit der primären Datenbank der primären Verfügbarkeitsgruppe verglichen und die Datenbank wird durch Seeding auf den gleichen Stand wie die Quelle gebracht. Es gibt einige Vorbehalte:

* Die Ausgabe in `sys.dm_hadr_automatic_seeding` auf dem primären Replikat der sekundären Verfügbarkeitsgruppe wird als `current_state` „FAILED“ mit dem Hinweis „Nachrichtentimeout bei der Seedingüberprüfung“ angezeigt.

* Das aktuelle SQL Server-Protokoll auf dem primären Replikat der sekundären Verfügbarkeitsgruppe wird anzeigen, dass das Seeding erfolgreich war und dass die [LSNs](../../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md) synchronisiert wurden.

* Die Ausgabe, die in `sys.dm_hadr_automatic_seeding` auf dem primären Replikat der primären Verfügbarkeitsgruppe angezeigt wird, zeigt „COMPLETED“ als „current_state“ an. 

* Das Seeding weist bei verteilten Verfügbarkeitsgruppen ebenfalls ein unterschiedliches Verhalten auf. Damit das Seeding auf dem sekundären Replikat beginnt, müssen Sie den Befehl `ALTER AVAILABILITY GROUP [AGName] GRANT CREATE ANY DATABASE` auf dem Replikat ausführen. Obwohl diese Bedingung immer noch auf jedes sekundäre Replikat zutrifft, das Teil der zugrunde liegenden Verfügbarkeitsgruppe ist, verfügt das primäre Replikat der sekundären Verfügbarkeitsgruppe schon über die erforderlichen Berechtigungen, um mit dem Seeding zu beginnen, nachdem es zu der verteilten Verfügbarkeitsgruppe hinzugefügt wurde. 

## <a name="monitor-distributed-availability-group-health"></a>Überwachen der Integrität einer verteilten Verfügbarkeitsgruppe

Eine verteilte Verfügbarkeitsgruppe ist ein nur in SQL Server verfügbares Konstrukt und wird nicht im zugrunde liegenden WSFC-Cluster angezeigt. Die folgende Abbildung zeigt zwei unterschiedliche WSFC-Cluster („CLUSTER_A“ und „CLUSTER_B“), von denen jedes seine eigenen Verfügbarkeitsgruppen besitzt. Im Folgenden werden nur AG 1 in „CLUSTER_A“ und AG 2 in „CLUSTER_B“ erläutert. 

[Zwei WSFC-Cluster mit mehreren Verfügbarkeitsgruppen über den PowerShell-Befehl „Get-ClusterGroup“](./media/distributed-availability-group/dag-07-two-wsfcs-multiple-ags-through-get-clustergroup-command.png)


```
PS C:\> Get-ClusterGroup -Cluster CLUSTER_A

Name                            OwnerNode             State
----                            ---------             -----
AG1                             DENNIS                Online
Available Storage               GLEN                  Offline
Cluster Group                   JY                    Online
New_RoR                         DENNIS                Online
Old_RoR                         DENNIS                Online
SeedingAG                       DENNIS                Online


PS C:\> Get-ClusterGroup -Cluster CLUSTER_B

Name                            OwnerNode             State
----                            ---------             -----
AG2                             TOMMY                 Online
Available Storage               JC                    Offline
Cluster Group                   JC                    Online
```

Die detaillierten Informationen über eine verteilte Verfügbarkeitsgruppe befindet sich in SQL Server, insbesondere in den dynamischen Verwaltungsansichten für Verfügbarkeitsgruppen. Derzeit werden Informationen über verteilte Verfügbarkeitsgruppen in SQL Server Management Studio nur auf dem primären Replikat der Verfügbarkeitsgruppe angezeigt. Wie in der folgenden Abbildung dargestellt zeigt SQL Server Management Studio unter dem Ordner „Verfügbarkeitsgruppen“ an, dass es eine verteilte Verfügbarkeitsgruppe gibt. Die Abbildung zeigt AG1 als das primäre Replikat einer einzelnen lokalen Verfügbarkeitsgruppe dieser Instanz, nicht aber als das einer verteilten Verfügbarkeitsgruppe.

![Ansicht des primären Replikats des ersten WSFC-Clusters einer verteilten Verfügbarkeitsgruppe in SQL Server Management Studio](./media/distributed-availability-group/dag-08-view-smss-primary-replica-first-wsfc-distributed-ag.png)


Wenn Sie jedoch mit der rechten Maustaste auf die verteilte Verfügbarkeitsgruppe klicken, sind keine Optionen verfügbar (siehe folgende Abbildung) und die erweiterten Verfügbarkeitsdatenbanken, Verfügbarkeitsgruppenlistener und Verfügbarkeitsreplikatordner sind alle leer. SQL Server Management Studio 16 zeigt dieses Ergebnis an, das sich jedoch in einer zukünftigen Version von SQL Server Management Studio ändern könnte.

![Keine Optionen für eine Aktion verfügbar](./media/distributed-availability-group/dag-09-no-options-available-action.png)

Wie in der folgenden Abbildung dargestellt zeigen sekundäre Replikate in SQL Server Management Studio nichts an, das sich auf die verteilte Verfügbarkeitsgruppe bezieht. Die Namen dieser Verfügbarkeitsgruppen können den Rollen zugeordnet werden, die im vorherigen Bild (CLUSTER_A – WSFC-Cluster) angezeigt wurden.

![Ansicht eines sekundären Replikats in SQL Server Management Studio](./media/distributed-availability-group/dag-10-view-ssms-secondary-replica.png)

### <a name="dmv-to-list-all-availability-replica-names"></a>DMV für die Auflistung aller Verfügbarkeitsreplikatnamen

Die gleichen Konzepte gelten, wenn Sie die dynamischen Verwaltungsansichten verwenden. Indem Sie die folgende Abfrage verwenden, können Sie alle Verfügbarkeitsgruppen (reguläre und verteilte) und die enthaltenen Knoten anzeigen lassen. Dieses Ergebnis wird nur angezeigt, wenn Sie das primäre Replikat in einem der WSFC-Cluster abfragen, die in der verteilten Verfügbarkeitsgruppe enthalten sind. Es gibt eine neue Spalte namens `is_distributed` in der dynamischen Verwaltungsansicht `sys.availability_groups`, die den Wert „1“ enthält, wenn es sich bei der Verfügbarkeitsgruppe um eine verteilte Verfügbarkeitsgruppe handelt. Zum Anzeigen dieser Spalte ist Folgendes nötig:

```sql
-- shows replicas associated with availability groups
SELECT 
   ag.[name] AS [AG Name], 
   ag.Is_Distributed, 
   ar.replica_server_name AS [Replica Name]
FROM sys.availability_groups AS ag 
INNER JOIN sys.availability_replicas AS ar       
   ON ag.group_id = ar.group_id
GO
```

In der folgenden Abbildung wird ein Beispiel für eine Ausgabe des zweiten WSFC-Clusters gezeigt, das in der verteilten Verfügbarkeitsgruppe enthalten ist. SPAG1 besteht aus zwei Replikaten: DENNIS und JY. Die verteilte Verfügbarkeitsgruppe namens „SPDistAG“ verfügt jedoch über die Namen der zwei enthaltenen Verfügbarkeitsgruppen (SPAG1 und SPAG2) statt den Namen der Instanzen, wie es bei herkömmlichen Verfügbarkeitsgruppen der Fall ist. 

![Beispielausgabe der vorangegangenen Abfrage](./media/distributed-availability-group/dag-11-example-output-of-query-above.png)

### <a name="dmv-to-list-distributed-ag-health"></a>DMV für die Auflistung der Integrität der verteilten Verfügbarkeitsgruppe

Jeder Status, der in SQL Server Management Studio auf dem Dashboard und in anderen Bereichen angezeigt wird, ist nur für die lokale Synchronisierung innerhalb dieser Verfügbarkeitsgruppe bestimmt. Führen Sie eine Abfrage der dynamischen Verwaltungsansicht durch, um die Integrität einer verteilten Verfügbarkeitsgruppe anzuzeigen. Die folgende Beispielabfrage erweitert und präzisiert die vorherige Abfrage:

```sql
-- shows sync status of distributed AG
SELECT 
   ag.[name] AS [AG Name], 
   ag.is_distributed, 
   ar.replica_server_name AS [Underlying AG], 
   ars.role_desc AS [Role], 
   ars.synchronization_health_desc AS [Sync Status]
FROM  sys.availability_groups AS ag
INNER JOIN sys.availability_replicas AS ar 
   ON  ag.group_id = ar.group_id        
INNER JOIN sys.dm_hadr_availability_replica_states AS ars       
   ON  ar.replica_id = ars.replica_id
WHERE ag.is_distributed = 1
GO
```
       
       
![Status der verteilten Verfügbarkeitsgruppe](./media/distributed-availability-group/dag-12-distributed-ag-status.png)

### <a name="dmv-to-view-underlying-performance"></a>DMV zum Anzeigen der zugrunde liegenden Leistung

Sie können die vorherige Abfrage weiter erweitern, indem Sie `sys.dm_hadr_database_replicas_states` hinzufügen, um die zugrunde liegende Leistung in der dynamischen Verwaltungsansicht anzuzeigen. Die dynamische Verwaltungsansicht speichert derzeit nur Informationen über die zweite Verfügbarkeitsgruppe. Die folgende Beispielabfrage, die auf der primären Verfügbarkeitsgruppe ausgeführt wird, erzeugt die unten dargestellte Beispielausgabe:

```sql
-- shows underlying performance of distributed AG
SELECT 
   ag.[name] AS [Distributed AG Name], 
   ar.replica_server_name AS [Underlying AG], 
   dbs.[name] AS [Database],
   ars.role_desc AS [Role],
   drs.synchronization_health_desc AS [Sync Status],
   drs.log_send_queue_size,
   drs.log_send_rate, 
   drs.redo_queue_size, 
   drs.redo_rate
FROM sys.databases AS dbs
INNER JOIN sys.dm_hadr_database_replica_states AS drs
   ON dbs.database_id = drs.database_id
INNER JOIN sys.availability_groups AS ag
   ON drs.group_id = ag.group_id
INNER JOIN sys.dm_hadr_availability_replica_states AS ars
   ON ars.replica_id = drs.replica_id
INNER JOIN sys.availability_replicas AS ar
   ON ar.replica_id = ars.replica_id
WHERE ag.is_distributed = 1
GO
```


![Leistungsinformationen einer verteilten Verfügbarkeitsgruppe](./media/distributed-availability-group/dag-13-performance-information-distributed-ag.png)

### <a name="dmv-to-view-performance-counters-for-distributed-ag"></a>DMV zum Anzeigen der Leistungsindikatoren für die verteilte Verfügbarkeitsgruppe
Die unten dargestellte Abfrage zeigt die Leistungsindikatoren an, die explizit mit der verteilten Verfügbarkeitsgruppe verknüpft sind. 


 ```sql
 -- displays OS performance counters related to the distributed ag named 'distributedag'
 SELECT * FROM sys.dm_os_performance_counters WHERE instance_name LIKE '%distributed%'
 ```

 ![DMV, die die Leistungsindikatoren des Betriebssystems für den gerichteten azyklischen Graph anzeigt](./media/distributed-availability-group/dmv-os-performance-counters.png)


 >[!NOTE]
 >Der Filter `LIKE` muss den Namen der verteilten Verfügbarkeitsgruppe besitzen. In diesem Beispiel lautet der Name der verteilten Verfügbarkeitsgruppe „distributedag“. Ändern Sie den `LIKE`-Modifizierer, um den Namen Ihrer verteilten Verfügbarkeitsgruppe widerzuspiegeln.  

### <a name="dmv-to-display-health-of-both-ag-and-distributed-ag"></a>DMV zum Anzeigen der Integrität der Verfügbarkeitsgruppe und der verteilten Verfügbarkeitsgruppe
Die folgende Abfrage zeigt umfangreiche Informationen über die Integrität der Verfügbarkeitsgruppe und der verteilten Verfügbarkeitsgruppe an. [Vielen Dank an Tracy Boggiano!](https://tracyboggiano.com/archive/2017/11/distributed-availability-groups-setup-and-monitoring/)

 ```sql
 -- displays sync status, send rate, and redo rate of availability groups, including distributed AG
 SELECT 
        ag.name AS 'AG Name', 
        ag.is_distributed, 
        ar.replica_server_name AS 'AG', 
        dbs.name AS 'Database',
    ars.role_desc, 
    drs.synchronization_health_desc, 
    drs.log_send_queue_size, 
    drs.log_send_rate, 
    drs.redo_queue_size, 
    drs.redo_rate,
    drs.suspend_reason_desc,
    drs.last_sent_time,
    drs.last_received_time,
    drs.last_hardened_time,
    drs.last_redone_time,
    drs.last_commit_time,
    drs.secondary_lag_seconds
 FROM sys.databases dbs 
 INNER JOIN sys.dm_hadr_database_replica_states drs 
    ON dbs.database_id = drs.database_id
 INNER JOIN sys.availability_groups ag 
    ON drs.group_id = ag.group_id
 INNER JOIN sys.dm_hadr_availability_replica_states ars 
    ON ars.replica_id = drs.replica_id
 INNER JOIN sys.availability_replicas ar 
    ON ar.replica_id =  ars.replica_id
 --WHERE ag.is_distributed = 1
 GO
 ```

![Integrität der Verfügbarkeitsgruppe und der verteilen Verfügbarkeitsgruppe](./media/distributed-availability-group/dmv-sync-status-send-rate.png)

### <a name="dmvs-to-view-metadata-of-distributed-ag"></a>DMVs zum Anzeigen von Metadaten der verteilten Verfügbarkeitsgruppe
Die folgende Abfragen zeigt Informationen über Endpunkt-URLs an, die von den Verfügbarkeitsgruppen verwendet werden, einschließlich der verteilten Verfügbarkeitsgruppe.  [Vielen Dank an David Barbarin!](https://blog.dbi-services.com/sql-server-2016-alwayson-distributed-availability-groups/)



 ```sql
 -- shows endpoint url and sync state for ag, and dag
 SELECT
    ag.name AS group_name,
    ag.is_distributed,
    ar.replica_server_name AS replica_name,
    ar.endpoint_url,
    ar.availability_mode_desc,
    ar.failover_mode_desc,
    ar.primary_role_allow_connections_desc AS allow_connections_primary,
    ar.secondary_role_allow_connections_desc AS allow_connections_secondary,
    ar.seeding_mode_desc AS seeding_mode
 FROM sys.availability_replicas AS ar
 JOIN sys.availability_groups AS ag
    ON ar.group_id = ag.group_id
 GO
 ```


![Metadaten der DMV für die verteilte Verfügbarkeitsgruppe](./media/distributed-availability-group/dmv-metadata-dag2.png)



### <a name="dmv-to-show-current-state-of-seeding"></a>DMV zum Anzeigen des aktuellen Seedingstatus
Die folgende Abfrage zeigt Informationen zum aktuellen Seedingstatus an. Dies ist nützlich für die Problembehandlung von Synchronisierungsfehlern zwischen Replikaten. [Nochmals vielen Dank an David Barbarin!](https://blog.dbi-services.com/sql-server-2016-alwayson-distributed-availability-groups/)

 ```sql
 -- shows current_state of seeding 
 SELECT
    ag.name AS aag_name,
    ar.replica_server_name,
    d.name AS database_name,
    has.current_state,
    has.failure_state_desc AS failure_state,
    has.error_code,
    has.performed_seeding,
    has.start_time,
    has.completion_time,
    has.number_of_attempts
 FROM sys.dm_hadr_automatic_seeding AS has
 JOIN sys.availability_groups AS ag
    ON ag.group_id = has.ag_id
 JOIN sys.availability_replicas AS ar
    ON ar.replica_id = has.ag_remote_replica_id
 JOIN sys.databases AS d
    ON d.group_database_id = has.ag_db_id
 GO
 ```


![Aktueller Seedingstatus](./media/distributed-availability-group/dmv-seeding.png)




### <a name="next-steps"></a>Nächste Schritte 

* [Verwenden des Assistenten für Verfügbarkeitsgruppen (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

* [Verwenden des Dialogfelds „Neue Verfügbarkeitsgruppe“ (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
* [Erstellen einer Verfügbarkeitsgruppe mit Transact-SQL](create-an-availability-group-transact-sql.md)