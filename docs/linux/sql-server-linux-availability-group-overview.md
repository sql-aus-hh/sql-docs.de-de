---
title: Verfügbarkeitsgruppen für SQL Server unter Linux
description: Hier erfahren Sie mehr über die Merkmale von Always On-Verfügbarkeitsgruppen für SQL Server unter Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e37742d4-541c-4d43-9ec7-a5f9b2c0e5d1
ms.openlocfilehash: 7573e1faa9d6cb4bbf75dc4e516da22344523bff
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172022"
---
# <a name="always-on-availability-groups-on-linux"></a>Always On-Verfügbarkeitsgruppen unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden die Merkmale von Always on-Verfügbarkeitsgruppen unter Linux-basierten [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Installationen beschrieben. Außerdem werden die Unterschiede zwischen auf Linux-und Windows Server-Failoverclustern (WSFC) basierenden Tags behandelt. Grundlegende Informationen zu Verfügbarkeitsgruppen finden Sie in der [Windows-Dokumentation](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Die Funktionsweise unter Windows und Linux ist mit Ausnahme von WSFCs identisch.

Im Allgemeinen entsprechen die Verfügbarkeitsgruppen unter [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] für Linux den WSFC-basierten Implementierungen. Das bedeutet, dass alle Einschränkungen und Features (mit einigen Ausnahmen) identisch sind. Die Hauptunterschiede bestehen in Folgendem:

-   Microsoft Distributed Transaction Coordinator (MS DTC) wird unter Linux ab SQL Server 2017 CU16 unterstützt. In Verfügbarkeitsgruppen unter Linux wird DTC jedoch noch nicht unterstützt. Wenn Ihre Anwendungen verteilte Transaktionen verwenden und eine Verfügbarkeitsgruppe benötigen, stellen Sie [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] unter Windows bereit.
-   Für Linux-basierte Bereitstellungen, die Hochverfügbarkeit erfordern, verwenden Sie für das Clustering Pacemaker anstelle eines WSFC.
-   Im Gegensatz zu den meisten Konfiguration für Verfügbarkeitsgruppen unter Windows (außer im Workgroupclusterszenario) ist Active Directory Domain Services (AD DS) für Pacemaker nicht erforderlich.
-   Das Failover einer Verfügbarkeitsgruppe von einem Knoten auf einen anderen unterscheidet sich zwischen Linux und Windows.
-   Bestimmte Einstellungen wie `required_synchronized_secondaries_to_commit` können unter Linux nur über Pacemaker geändert werden, während bei einer WSFC-basierten Installation Transact-SQL verwendet wird.

## <a name="number-of-replicas-and-cluster-nodes"></a>Anzahl der Replikate und Clusterknoten

Eine Verfügbarkeitsgruppe in [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] kann insgesamt über zwei Replikate verfügen: ein primäres Replikat und ein sekundäres Replikat, das nur zu Verfügbarkeitszwecken verwendet werden kann. Es kann nicht für andere Zwecke wie lesbare Abfragen verwendet werden. Eine Verfügbarkeitsgruppe in [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] kann insgesamt über bis zu neun Replikate verfügen: ein primäres Replikat und bis zu acht sekundäre Replikate, von denen bis zu drei (einschließlich des primären Replikats) synchron sein können. Wenn ein zugrunde liegender Cluster verwendet wird, können maximal 16 Knoten vorhanden sein, wenn Corosync beteiligt ist. Eine Verfügbarkeitsgruppe kann mit [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] neun von 16 Knoten umfassen und mit [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] zwei Knoten.

Für eine Konfiguration mit zwei Replikaten, für die ein automatisches Failover auf ein anderes Replikat möglich sein muss, muss unter [Reine Konfigurationsreplikate und Quoren](#configuration-only-replica-and-quorum) beschrieben ein Replikat verwendet werden, das ausschließlich für die Konfiguration konfiguriert ist. Reine Konfigurationsreplikate wurden in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] Cumulative Update 1 (CU1) eingeführt. Diese Version sollte für die Konfiguration also als mindestens erforderliche Version festgelegt werden.

Wenn Pacemaker verwendet wird, muss die Software ordnungsgemäß konfiguriert werden, damit sie weiterhin ausgeführt wird. Das bedeutet, dass Quorum und STONITH gemäß Pacemaker-Anforderungen zusätzlich zu anderen [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Anforderungen wie reinen Konfigurationsreplikaten ordnungsgemäß implementiert werden müssen.

Lesbare sekundäre Replikate werden nur mit [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] unterstützt.

## <a name="cluster-type-and-failover-mode"></a>Clustertyp und Failovermodus

In [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] wurden Clustertypen für Verfügbarkeitsgruppen eingeführt. Für Linux gibt es zwei gültige Werte: „EXTERNAL“ und „NONE“. Der Clustertyp „EXTERNAL“ bedeutet, dass Pacemaker unterhalb der Verfügbarkeitsgruppe verwendet wird. Wenn Sie den Clustertyp „EXTERNAL“ verwenden, muss auch der Failovermodus auf „External“ festgelegt werden (ebenfalls neu in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]). Das automatische Failover wird unterstützt, aber im Gegensatz zu einem WSFC wird der Failovermodus bei Verwendung von Pacemaker nicht automatisch auf „EXTERNAL“ festgelegt. Im Gegensatz zu einem WSFC wird der Pacemaker-Teil der Verfügbarkeitsgruppe nach der Konfiguration der Verfügbarkeitsgruppe erstellt.

Der Clustertyp „NONE“ bedeutet, dass Pacemaker nicht erforderlich ist und von der Verfügbarkeitsgruppe nicht verwendet wird. Auch wenn Pacemaker auf einem Server konfiguriert wird, werden Verfügbarkeitsgruppen nicht über Pacemaker angezeigt oder verwaltet, wenn der Clustertyp „NONE“ für eine Verfügbarkeitsgruppe festgelegt ist. Der Clustertyp „NONE“ unterstützt nur manuelles Failover von einem primären zu einem sekundären Replikat. Eine Verfügbarkeitsgruppe, die mit „NONE“ erstellt wurde, ist hauptsächlich für das schreibgeschützte Aufskalieren und für Upgrades vorgesehen. Sie könnte zwar für die Notfallwiederherstellung oder die lokale Verfügbarkeit eingesetzt werden, bei denen kein automatisches Failover nötig ist, doch diese Vorgehensweise wird nicht empfohlen. Ohne Pacemaker ist der Listenerverlauf zudem komplexer.

Der Clustertyp wird in der [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]dynamischen Verwaltungssicht (DMV)`sys.availability_groups` in den Spalten `cluster_type` und `cluster_type_desc` gespeichert.

## <a name="required_synchronized_secondaries_to_commit"></a>required\_synchronized\_secondaries\_to\_commit

In [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] ist nun die neue Einstellung `required_synchronized_secondaries_to_commit` vorhanden, die von Verfügbarkeitsgruppen verwendet wird. Über diese wird der Verfügbarkeitsgruppe die Anzahl der sekundären Replikate mitgeteilt, die auf das primäre Replikat abgestimmt sein müssen. Dadurch können beispielsweise das automatische Failover (nur bei Integration mit Pacemaker und dem Clustertyp „EXTERNAL“) ermöglicht und das Verhalten der Verfügbarkeit des Replikats gesteuert werden, wenn die richtige Anzahl sekundärer Replikate online oder offline ist. Weitere Informationen zu diesem Thema finden Sie unter [Hochverfügbarkeit und Schutz von Daten für Verfügbarkeitsgruppenkonfigurationen](sql-server-linux-availability-group-ha.md). Der Wert `required_synchronized_secondaries_to_commit` wird standardmäßig festgelegt und von Pacemaker/[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]verwaltet. Sie können diesen Wert manuell überschreiben.

Die Kombination von `required_synchronized_secondaries_to_commit` und der neuen Sequenznummer (in `sys.availability_groups` gespeichert) informiert Pacemaker und [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] darüber, dass beispielsweise ein automatisches Failover möglich ist. In diesem Fall würde ein sekundäres Replikat die gleiche Sequenznummer wie das primäre Replikat aufweisen, was bedeutet, dass alle Konfigurationsinformationen auf dem neuesten Stand sind.

Für `required_synchronized_secondaries_to_commit` können drei Werte festgelegt werden: 0, 1 oder 2. Sie steuern, was geschieht, wenn ein Replikat nicht mehr verfügbar ist. Die Zahlen entsprechen der Anzahl von sekundären Replikaten, die mit dem primären Replikat synchronisiert werden müssen. Das Verhalten ist unter Linux wie folgt:

-   0: Sekundäre Replikate müssen nicht mit dem primären Replikat synchronisiert sein. Wenn die sekundären Replikate jedoch nicht synchronisiert werden, wird kein automatisches Failover ausgeführt. 
-   1: Ein sekundäres Replikat muss mit dem primären Replikat synchronisiert sein. Ein automatisches Failover ist möglich. Die primäre Datenbank ist erst verfügbar, wenn ein sekundäres synchrones Replikat verfügbar ist.
-   2: Beide sekundären Replikate in einer Verfügbarkeitsgruppenkonfiguration mit drei oder mehr Knoten müssen mit dem primären Replikat synchronisiert werden. Ein automatisches Failover ist möglich.

`required_synchronized_secondaries_to_commit` steuert nicht nur das Verhalten von Failovern mit synchronen Replikaten, sondern auch den Datenverlust. Bei einem Wert von 1 oder 2 muss immer ein sekundäres Replikat synchronisiert werden, sodass immer Datenredundanz vorliegt. Dies bedeutet, dass kein Datenverlust auftritt.

Verwenden Sie die folgende Syntax, um den Wert von `required_synchronized_secondaries_to_commit` zu ändern:

>[!NOTE]
>Das Ändern des Werts bewirkt, dass die Ressource neu gestartet wird. Es kommt zu einem kurzen Ausfall. Sie können diesen Ausfall nur vermeiden, indem Sie die Ressource so konfigurieren, dass sie vorübergehend nicht vom Cluster verwaltet wird.

**Red Hat Enterprise Linux (RHEL) und Ubuntu**

```bash
sudo pcs resource update <AGResourceName> required_synchronized_secondaries_to_commit=<Value>
```

**SUSE Linux Enterprise Server (SLES)**

```bash
sudo crm resource param ms-<AGResourceName> set required_synchronized_secondaries_to_commit <value>
```

Hierbei entspricht *AGResourceName* dem Namen der Ressource, die für die Verfügbarkeitsgruppe konfiguriert wurde, und *value* entspricht 0, 1 oder 2. Wenn Sie den Standardzustand wiederherstellen möchten, dass Pacemaker den Parameter verwaltet, führen Sie die gleiche Anweisung ohne Wert (value) aus.

Ein automatisches Failover einer Verfügbarkeitsgruppe ist möglich, wenn die folgenden Bedingungen erfüllt sind:

-   Das primäre und das sekundäre Replikat sind auf synchrone Datenverschiebung festgelegt.
-   Das sekundäre Replikat weist den Status „Synchronisiert“ (keine Synchronisierung) auf, die beiden Replikate befinden sich also am selben Datenpunkt.
-   Der Clustertyp ist auf „EXTERNAL“ festgelegt. Ein automatisches Failover ist mit dem Clustertyp „NONE“ nicht möglich.
-   Der Wert `sequence_number` des sekundären Replikats, das zum primären Replikat werden soll, weist die höchste Sequenznummer auf. Das bedeutet, dass der `sequence_number`-Wert des sekundären Replikats dem Wert des ursprünglichen primären Replikats entspricht.

Wenn diese Bedingungen erfüllt sind und der Server ausfällt, auf dem das primäre Replikat gehostet wird, wechselt die Verfügbarkeitsgruppe den Besitz zu einem synchronen Replikat. Das Verhalten von synchronen Replikaten (maximal ein primäres und zwei sekundäre Replikate) kann weiter von `required_synchronized_secondaries_to_commit` gesteuert werden. Das gilt für Verfügbarkeitsgruppen unter Windows und Linux, die Konfiguration unterscheidet sich jedoch grundlegend. Unter Linux wird der Wert automatisch vom Cluster auf der Verfügbarkeitsgruppenressource konfiguriert.

## <a name="configuration-only-replica-and-quorum"></a>Reine Konfigurationsreplikate und Quoren

Reine Konfigurationsreplikate sind ebenfalls neu in [!INCLUDE[sssql17-md](../includes/sssql17-md.md)] CU1. Da Pacemaker sich von WSFCs unterscheidet – insbesondere in Bezug auf Quoren und die Notwendigkeit von STONITH – funktioniert die Konfiguration mit nur zwei Knoten bei Verfügbarkeitsgruppen nicht. Bei einer Failoverclusterinstanz können die von Pacemaker bereitgestellten Quorummechanismen funktionieren, da die gesamte Vermittlung bei Failoverclusterinstanzen auf Clusterebene erfolgt. Bei einer Verfügbarkeitsgruppe erfolgt die Vermittlung unter Linux in [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Dort werden alle Metadaten gespeichert. In diesem Fall können reine Konfigurationsreplikate genutzt werden.

Ohne weitere Schritte wären ein dritter Knoten und mindestens ein synchronisiertes Replikat erforderlich. Das reine Konfigurationsreplikat speichert die Verfügbarkeitsgruppenkonfiguration in der Masterdatenbank. Das gleiche gilt für die anderen Replikate in der Verfügbarkeitsgruppenkonfiguration. Das reine Konfigurationsreplikat verfügt nicht über die Benutzerdatenbanken, die an der Verfügbarkeitsgruppe teilnehmen. Die Konfigurationsdaten werden synchron vom primären Replikat gesendet. Diese Konfigurationsdaten werden dann während eines Failovers verwendet, unabhängig davon, ob es automatisch oder manuell ausgeführt wird.

Damit eine Verfügbarkeitsgruppe das Quorum beibehalten und automatische Failovers für den Clustertyp „EXTERNAL“ ermöglichen kann, muss eine der folgenden Bedingungen erfüllt sein:

-   Drei synchrone Replikate müssen vorhanden sein (nur [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)]).
-   Zwei Replikate (primär und sekundär) und ein reines Konfigurationsreplikat müssen vorhanden sein.

Ein manuelles Failover kann unabhängig davon durchgeführt werden, ob der Clustertyp „EXTERNAL“ oder „NONE“ für Verfügbarkeitsgruppenkonfigurationen verwendet wird. Ein reines Konfigurationsreplikat kann zwar für eine Verfügbarkeitsgruppe konfiguriert werden, deren Clustertyp „NONE“ ist, doch diese Vorgehensweise wird nicht empfohlen, da Sie die Bereitstellung erschwert. Ändern Sie `required_synchronized_secondaries_to_commit` für diese Konfigurationen manuell, damit der Wert mindestens auf „1“ festgelegt und mindestens ein synchronisiertes Replikat vorhanden ist.

Ein reines Konfigurationsreplikat kann in jeder [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-Edition (einschließlich [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)]) gehostet werden. Dadurch werden die Lizenzierungskosten minimiert, und es wird sichergestellt, dass es mit Verfügbarkeitsgruppen in [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] kompatibel ist. Das bedeutet, dass der dritte erforderliche Server lediglich die Mindestspezifikation für [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] erfüllen muss, da er keinen Benutzertransaktionsdatenverkehr für die Verfügbarkeitsgruppe empfängt.

Wenn ein reines Konfigurationsreplikat verwendet wird, weist es folgendes Verhalten auf:

-   `required_synchronized_secondaries_to_commit` ist standardmäßig auf „0“ festgelegt. Der Wert kann bei Bedarf manuell in „1“ geändert werden.
-   Wenn das primäre Replikat ausfällt und `required_synchronized_secondaries_to_commit` den Wert „0“ aufweist, wird das sekundäre Replikat zum neuen primären Replikat und ist für das Lesen und Schreiben verfügbar. Wenn der Wert „1“ ist, wird ein automatisches Failover ausgeführt. Es werden jedoch erst dann neue Transaktionen akzeptiert, wenn das andere Replikat online ist.
-   Wenn ein sekundäres Replikat ausfällt und `required_synchronized_secondaries_to_commit` den Wert „0“ aufweist, akzeptiert das primäre Replikat weiterhin Transaktionen, aber wenn das primäre Replikat zu diesem Zeitpunkt ausfällt, werden Daten nicht geschützt, und Failover (manuell oder automatisch) sind nicht möglich, da kein sekundäres Replikat verfügbar ist.
-   Wenn die reinen Konfigurationsreplikate ausfallen, funktioniert die Verfügbarkeitsgruppe wie gewohnt, es ist jedoch kein automatisches Failover möglich.
-   Wenn ein synchrones sekundäres Replikat und das reine Konfigurationsreplikat ausfallen, kann das primäre Replikat keine Transaktionen akzeptieren, und das primäre Replikat kann kein Failover durchführen.

In CU1 gibt es einen bekannten Fehler bei der Protokollierung in der Datei „corosync.log“, die über `mssql-server-ha` generiert wird. Wenn ein sekundäres Replikat aufgrund der Anzahl erforderlicher verfügbarer Replikate nicht zum primären Replikat werden kann, enthält die Meldung aktuell den Text „Expected to receive 1 sequence numbers but only received 2. Not enough replicas are online to safely promote the local replica.“ (Es wird erwartet, 1 Sequenznummer zu empfangen, es wurden jedoch nur 2 empfangen. Es sind nicht genügend Replikate online, um das lokale Replikat sicher höher zu stufen.) Die Zahlen sollten umgekehrt eingefügt werden. Der Text sollte also lauten: „Expected to receive 2 sequence numbers but only received 1. Not enough replicas are online to safely promote the local replica.“ (Es wird erwartet, 1 Sequenznummer zu empfangen, es wurden jedoch nur 2 empfangen. Es sind nicht genügend Replikate online, um das lokale Replikat sicher höher zu stufen.) 

## <a name="multiple-availability-groups"></a>Mehrere Verfügbarkeitsgruppen 

Pro Pacemaker-Cluster oder -Servergruppe kann mehr als eine Verfügbarkeitsgruppe erstellt werden. Die Systemressourcen stellen die einzige Einschränkung dar. Der Besitzer der Verfügbarkeitsgruppe wird vom Master angezeigt. Unterschiedliche Verfügbarkeitsgruppen können sich im Besitz verschiedener Knoten befinden. Sie müssen nicht alle auf demselben Knoten ausgeführt werden.

## <a name="drive-and-folder-location-for-databases"></a>Laufwerk und Ordnerpfad für Datenbanken

Bei Windows-basierten Verfügbarkeitsgruppen sollten das Laufwerk und die Ordnerstruktur für die Benutzerdatenbanken, die an einer Verfügbarkeitsgruppe teilnehmen, identisch sein. Wenn sich die Benutzerdatenbank auf Server A beispielsweise unter `/var/opt/mssql/userdata` befindet, sollte der gleiche Ordner auf Server B vorhanden sein. Die einzige Ausnahme wird im Abschnitt [Interoperabilität mit Windows-basierten Verfügbarkeitsgruppen und Replikaten](#interoperability-with-windows-based-availability-groups-and-replicas) behandelt.

## <a name="the-listener-under-linux"></a>Der Listener unter Linux

Der Listener ist eine optionale Funktionalität für Verfügbarkeitsgruppen. Er bietet einen einzigen Einstiegspunkt für alle Verbindungen (Lese-/Schreibzugriff auf das primäre Replikat und/oder schreibgeschützter Zugriff auf sekundäre Replikate), damit Anwendungen und Endbenutzer nicht darüber informiert sein müssen, auf welchem Server die Daten gehostet werden. In einem WSFC besteht dieser aus einer Netzwerknamenressource und einer IP-Ressource, die dann (bei Bedarf) in AD DS und als DNS registriert wird. In Kombination mit der Verfügbarkeitsgruppenressource wird diese Abstraktion bereitgestellt. Weitere Informationen zu Listenern finden Sie unter [Listener, Clientkonnektivität und Anwendungsfailover](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).

Der Listener unter Linux ist anders konfiguriert, aber seine Funktionalität ist identisch. Das Konzept der Netzwerknamenressource ist in Pacemaker nicht vorhanden, und es wird kein Objekt in AD DS erstellt. Es gibt nur eine IP-Adressressource, die in Pacemaker erstellt und auf einem beliebigen Knoten ausgeführt werden kann. Es muss ein Eintrag erstellt werden, der der IP-Ressource für den Listener im DNS mit einem Anzeigenamen zugeordnet ist. Die IP-Ressource für den Listener ist nur auf dem Server aktiv, auf dem das primäre Replikat für diese Verfügbarkeitsgruppe gehostet wird.

Wenn Pacemaker verwendet und eine IP-Adressressource erstellt wird, die dem Listener zugeordnet ist, kommt es zu einem kurzen Ausfall, weil die IP-Adresse auf einem Server angehalten und auf dem anderen gestartet wird, unabhängig davon, ob es sich um ein automatisches oder manuelles Failover handelt. Dadurch wird zwar durch die Kombination aus einem einzelnen Namen und einer IP-Adresse eine Abstraktion ermöglicht, doch der Ausfall wird nicht maskiert. Eine Anwendung muss den Verbindungsverlust mithilfe einer Funktion zum Erkennen und Wiederherstellen der Verbindung bewältigen können.

Die Kombination aus dem DNS-Namen und der IP-Adresse genügt jedoch nicht, um alle Funktionen zu bieten, die ein Listener auf einem WSFC bietet (z. B. schreibgeschütztes Routing für sekundäre Replikate). Wenn Sie eine Verfügbarkeitsgruppe konfigurieren, muss dennoch ein Listener in [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] konfiguriert werden. Das wird im Assistenten und in der Syntax von Transact-SQL deutlich. Es gibt zwei Möglichkeiten, die gleiche Funktionsweise wie unter Windows durch Konfiguration zu erreichen:

-   Bei einer Verfügbarkeitsgruppe, die den Clustertyp „EXTERNAL“ aufweist, muss die IP-Adresse, die dem in [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] erstellten Listener zugeordnet ist, der IP-Adresse der in Pacemaker erstellten Ressource entsprechen.
-   Bei einer Verfügbarkeitsgruppe, die den Clustertyp „NONE“ aufweist, sollten Sie die IP-Adresse verwenden, die dem primären Replikat zugeordnet ist.

Die Instanz, die der bereitgestellten IP-Adresse zugeordnet ist, koordiniert dann beispielsweise schreibgeschützte Routinganforderungen für Anwendungen.

## <a name="interoperability-with-windows-based-availability-groups-and-replicas"></a>Interoperabilität mit Windows-basierten Verfügbarkeitsgruppen und Replikaten 

Eine Verfügbarkeitsgruppe, die den Clustertyp „EXTERNAL“ oder einen nicht in WSFCs verfügbaren Clustertyp aufweist, kann keine plattformübergreifenden Replikate besitzen. Dies gilt unabhängig davon, ob die Verfügbarkeitsgruppe für [!INCLUDE[ssstandard-md](../includes/ssstandard-md.md)] oder [!INCLUDE[ssenterprise-md](../includes/ssenterprise-md.md)] verwendet wird. Das bedeutet, dass es in einer herkömmlichen Verfügbarkeitsgruppenkonfiguration mit einem zugrunde liegenden Cluster nicht möglich ist, dass ein Replikat sich auf einem WSFC und ein anderes unter Linux mit Pacemaker befindet.

Eine Verfügbarkeitsgruppe mit dem Clustertyp „NONE“ kann Replikate auf unterschiedlichen Betriebssystemen besitzen. Es können also Linux- und Windows-basierte Replikate in derselben Verfügbarkeitsgruppe vorhanden sein. Im Folgenden sehen Sie ein Beispiel, bei dem das primäre Replikat Windows-basiert ist, während das sekundäre sich auf einer Linux-Distribution befindet.

![Hybride Replikate unter „NONE“](./media/sql-server-linux-availability-group-overview/image1.png)

Auch eine verteilte Verfügbarkeitsgruppe kann betriebssystemübergreifende Replikate besitzen. Die zugrunde liegenden Verfügbarkeitsgruppen sind an die Regeln der entsprechenden Konfiguration gebunden. So können beispielsweise eine für Linux konfigurierte Verfügbarkeitsgruppe mit dem Clustertyp „EXTERNAL“ und eine mithilfe eines WSFC konfigurierte Verfügbarkeitsgruppe miteinander verknüpft sein. Betrachten Sie das folgenden Beispiel:

![Hybride verteilte Verfügbarkeitsgruppe](./media/sql-server-linux-availability-group-overview/image2.png)

<!-- Distributed AGs are also supported for upgrades from [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] to [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. For more information on how to achieve this, see [the article "x"].

If using automatic seeding with a distributed availability group that crosses OSes, it can handle the differences in folder structure. How this works is described in [the documentation for automatic seeding].
-->
 
## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren von Verfügbarkeitsgruppen für SQL Server für Linux](sql-server-linux-availability-group-configure-ha.md)

[Konfigurieren von Leseskalierungs-Verfügbarkeitsgruppen für SQL Server für Linux](sql-server-linux-availability-group-configure-rs.md)

[Hinzufügen einer Clusterressource für Verfügbarkeitsgruppen unter RHEL](sql-server-linux-availability-group-cluster-rhel.md)

[Hinzufügen einer Clusterressource für Verfügbarkeitsgruppen unter SLES](sql-server-linux-availability-group-cluster-sles.md)

[Hinzufügen einer Clusterressource für Verfügbarkeitsgruppen unter Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

[Konfigurieren einer plattformübergreifenden Verfügbarkeitsgruppe](sql-server-linux-availability-group-cross-platform.md)

