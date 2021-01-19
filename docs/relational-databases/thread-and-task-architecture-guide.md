---
title: Handbuch zur Thread- und Taskarchitektur | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Thread- und Taskarchitektur in SQL Server, einschließlich der Aufgabenplanung, des Hinzufügens von CPUs bei laufendem Systembetrieb und der bewährten Methoden für die Verwendung von Computern mit mehr als 64 CPUs.
ms.custom: ''
ms.date: 09/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
- task scheduling
- working threads
- Large Deficit First scheduling
- LDF scheduling
- scheduling, SQL Server
- tasks, SQL Server
- threads, SQL Server
- quantum, SQL Server
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
author: pmasl
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 02ddc1ad96f45ba67ed613ee7446d8a1c12e1e5b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171402"
---
# <a name="thread-and-task-architecture-guide"></a>Handbuch zur Thread- und Taskarchitektur
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

## <a name="operating-system-task-scheduling"></a>Planung von Betriebssystemtasks
Threads sind die kleinsten Verarbeitungseinheiten, die von einem Betriebssystem ausgeführt werden können, und ermöglichen die Trennung der Anwendungslogik in mehrere gleichzeitige Ausführungspfade. Threads sind insbesondere dann hilfreich, wenn komplexe Anwendungen viele Tasks aufweisen, die gleichzeitig ausgeführt werden können. 

Wenn ein Betriebssystem eine Instanz einer Anwendung ausführt, wird eine als Prozess bezeichnete Einheit erstellt, um die Instanz zu verwalten. Der Prozess besitzt einen so genannten Ausführungsthread. Dabei handelt es sich um eine Folge von Programmieranweisungen, die vom Anwendungscode ausgeführt werden. Wenn beispielsweise eine einfache Anwendung eine einzige Gruppe von Anweisungen umfasst, die seriell ausgeführt werden können, wird diese Gruppe von Anweisungen als ein einzelner **Task** behandelt, und es gibt lediglich einen Ausführungspfad (bzw. **Thread**) durch die Anwendung. Komplexere Anwendungen können mehrere **Tasks** aufweisen, die gleichzeitig statt seriell ausgeführt werden können. Eine Anwendung kann dies tun, indem sie für jeden Task gesonderte Prozesse startet, was ein ressourcenintensiver Vorgang ist, oder einzelne Threads startet, die vergleichsweise weniger ressourcenintensiv sind. Außerdem kann das Ausführen der einzelnen Threads unabhängig von anderen Threads geplant werden, die einem Prozess zugeordnet sind.

Threads ermöglichen, dass komplexe Anwendungen Prozessoren (CPUs) effizienter nutzen, selbst auf Computern mit nur einer CPU. Bei einer einzigen CPU kann zu einem bestimmten Zeitpunkt immer nur ein Thread ausgeführt werden. Wenn ein Thread einen umfangreiche Vorgang ausführt, für die die CPU nicht verwendet wird, z. B. beim Lesen oder Schreiben vom bzw. auf den Datenträger, kann so lange ein anderer Thread ausgeführt werden, bis der erste Vorgang beendet ist. Da es möglich ist, Threads auszuführen, während andere Threads auf das Abschließen eines Vorgangs warten, kann eine Anwendung die CPU optimal nutzen. Dies gilt insbesondere für Multibenutzeranwendungen mit umfangreichen E/A-Vorgängen, wie es bei einem Datenbankserver der Fall ist. Computer mit mehreren CPUs können pro CPU nur einen Thread gleichzeitig ausführen. Wenn ein Computer beispielsweise über acht CPUs verfügt, können gleichzeitig acht Threads ausgeführt werden.

## <a name="sql-server-task-scheduling"></a>Planen von SQL Server-Tasks
In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ist eine **Anforderung** die logische Darstellung einer Abfrage oder eines Batches. Eine Anforderung stellt auch Vorgänge dar, die für Systemthreads erforderlich sind, wie z. B. Prüfpunkt- oder Protokollschreibvorgänge. Anforderungen existieren während ihrer gesamten Lebensdauer in verschiedenen Zuständen und können Wartezeiten anhäufen, wenn Ressourcen, die zur Ausführung der Anforderung erforderlich sind, nicht verfügbar sind, z. B. aufgrund von [Sperren](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md#locks) oder [Latches](../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md#latches). Weitere Informationen zu Anforderungszuständen finden Sie unter [sys.dm_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).

Ein **Task** stellt die Arbeitseinheit dar, die abgeschlossen werden muss, um die Anforderung zu erfüllen. Ein oder mehrere Tasks können einer einzelnen Anforderung zugewiesen werden. 
-  Parallele Anforderungen verfügen über mehrere aktive Tasks, die gleichzeitig anstatt seriell ausgeführt werden, einschließlich eines **übergeordneten Tasks** (bzw. koordinierenden Tasks) und mehrerer **untergeordneter Tasks**. Ein Ausführungsplan für eine parallele Anforderung kann serielle Branches umfassen, d. h. Bereiche des Plans mit Operatoren, die nicht parallel ausgeführt werden. Der übergeordnete Task ist auch für die Ausführung dieser seriellen Operatoren verantwortlich.
-  Serielle Anforderungen verfügen während der Ausführung immer nur über einen einzigen aktiven Task.     
Tasks existieren während ihrer gesamten Lebensdauer in verschiedenen Zuständen. Weitere Informationen zu Zuständen von Tasks finden Sie unter [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Tasks im Zustand SUSPENDED warten darauf, dass Ressourcen, die für ihre Ausführung benötigt werden, verfügbar werden. Weitere Informationen zu wartenden Tasks finden Sie unter [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md).

Ein **Arbeitsthread** in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], auch bekannt als Worker oder Thread, ist eine logische Darstellung eines Betriebssystemthreads. Bei der Ausführung **serieller Anforderungen** erzeugt die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] einen Worker, um den aktiven Task (1:1) auszuführen. Bei der Ausführung **paralleler Anforderungen** im [Zeilenmodus](../relational-databases/query-processing-architecture-guide.md#execution-modes) weist die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] einen Worker zu, der die untergeordneten Worker koordiniert, die für die Ausführung (ebenfalls 1:1) der ihnen zugewiesenen Tasks zuständig sind. Dies wird als **übergeordneter Thread** (oder koordinierender Thread) bezeichnet. Dem übergeordneten Thread ist ein übergeordneter Task zugeordnet. Der übergeordnete Thread ist der Einstiegspunkt der Anforderung und ist auch vorhanden, bevor die Engine eine Abfrage analysiert. Die Hauptaufgaben des übergeordneten Threads lauten: 
-  Koordinieren eines parallelen Scans
-  Starten untergeordneter paralleler Worker
-  Erfassen von Zeilen von parallelen Threads und Senden dieser an den Client
-  Durchführen lokaler und globaler Aggregationen    

> [!NOTE]
> Wenn ein Abfrageplan serielle und parallele Branches aufweist, ist einer der parallelen Tasks für die Ausführung des seriellen Branches verantwortlich. 

Die Anzahl der Arbeitsthreads, die für jeden Task erzeugt werden, hängt von folgenden Faktoren ab:
-   Ob die Anforderung entsprechend der Bestimmung durch den Abfrageoptimierer für Parallelität geeignet war.
-   Wie hoch der tatsächlich verfügbare [Grad an Parallelität (Degree Of Parallelism, DOP)](../relational-databases/query-processing-architecture-guide.md#DOP) im System basierend auf der aktuellen Last ist. Dies kann vom geschätzten DOP abweichen, der auf der Serverkonfiguration des maximalen Grads an Parallelität (MAXDOP) basiert. So kann beispielsweise die Serverkonfiguration für MAXDOP 8 sein, aber der zur Laufzeit verfügbare DOP kann nur 2 sein, was die Abfrageleistung beeinträchtigt. 

> [!NOTE]
> Der Grenzwert des **maximalen Grads an Parallelität (MAXDOP)** wird task- und nicht anforderungsbezogen festgelegt. Dies bedeutet, dass eine einzelne Anforderung während einer parallelen Abfrageausführung mehrere Tasks bis zum Grenzwert MAXDOP erzeugen kann, wobei jeder Task einen Worker nutzt. Weitere Informationen zu MAXDOP finden Sie unter [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Ein **Planer**, auch bekannt als SOS-Planer, verwaltet Arbeitsthreads, die Verarbeitungszeit benötigen, um Aufgaben im Rahmen von Tasks auszuführen. Jeder Planer ist einem einzelnen Prozessor (CPU) zugeordnet. Die Zeit, die ein Worker in einem Planer aktiv bleiben kann, wird als Betriebssystemquantum bezeichnet, wobei das Maximum bei 4 ms liegt. Nach Ablauf seiner Quantumzeit gibt ein Worker seine Zeit an andere Worker weiter, die auf CPU-Ressourcen zugreifen müssen, und ändert seinen Zustand. Diese Zusammenarbeit zwischen Workern zur Maximierung des Zugriffs auf CPU-Ressourcen wird als **kooperative Planung** oder auch nicht präemptive Planung bezeichnet. Die Änderung des Workerzustands wiederum wird an den Task, der diesem Worker zugeordnet ist, und an die mit dem Task verbundene Anforderung weitergegeben. Weitere Informationen zu Zuständen von Workern finden Sie unter [sys.dm_os_workers](../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md). Weitere Informationen zu Planern finden Sie unter [sys.dm_os_schedulers](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 

Zusammenfassend kann eine **Anforderung** eine oder mehrere **Tasks** erzeugen, um Arbeitseinheiten auszuführen. Jeder Task wird einem **Arbeitsthread** zugewiesen, der für das Abschließen des Tasks zuständig ist. Jeder Arbeitsthread muss für die aktive Ausführung des Tasks geplant (d. h. auf einem **Scheduler** platziert) werden. 

> [!NOTE]
> Nehmen Sie das folgende Szenario als Beispiel:   
> -  Worker 1 ist ein Task mit langer Ausführungszeit, z. B. eine Leseabfrage, die Read-Ahead- für In-Memory-Tabellen verwendet. Worker 1 stellt fest, dass die erforderlichen Datenseiten bereits im Pufferpool vorhanden sind, sodass er nicht auf E/A-Vorgänge warten muss und sein vollständiges Quantum nutzen kann, bevor Ergebnisse geliefert werden.   
> -  Worker 2 führt kürzere Tasks mit einer Dauer unter einer Millisekunde aus und muss daher vor dem Erreichen des vollständigen Quantums Ergebnisse zurückgeben.     
>
> In diesem Szenario und bis zu [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]kann Worker 1 den Scheduler im Grunde durch eine höhere gesamte Quantumzeit monopolisieren.   
> Ab [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] umfasst die kooperative Planung LDF-Planung (Large Deficit First). Bei der LDF-Planung werden die Quantumverwendungsmuster überwacht, und ein Workerthread monopolisiert keinen Scheduler. In demselben Szenario ist es für Worker 2 zulässig, wiederholte Quantums zu nutzen, bevor Worker 1 mehr Quantum nutzen darf, sodass Worker 1 daran gehindert wird, den Scheduler mit einem unfreundlichen Muster zu monopolisieren.

### <a name="scheduling-parallel-tasks"></a>Planen von parallelen Tasks
Angenommen, ein [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ist mit MAXDOP 8 konfiguriert, und die CPU-Affinität ist für 24 CPUs (Scheduler) zwischen den NUMA-Knoten 0 und 1 konfiguriert. Die Scheduler 0 bis 11 gehören zum NUMA-Knoten 0, die Scheduler 12 bis 23 zum NUMA-Knoten 1. Eine Anwendung sendet die folgende Abfrage (Anforderung) an die [!INCLUDE[ssde_md](../includes/ssde_md.md)]:

```sql
SELECT h.SalesOrderID, h.OrderDate, h.DueDate, h.ShipDate
FROM Sales.SalesOrderHeaderBulk AS h 
INNER JOIN Sales.SalesOrderDetailBulk AS d ON h.SalesOrderID = d.SalesOrderID 
WHERE (h.OrderDate >= '2014-3-28 00:00:00');
```

> [!TIP]
> Die Beispielabfrage kann mithilfe der [AdventureWorks2016_EXT-Beispieldatenbank](../samples/adventureworks-install-configure.md) ausgeführt werden. Die Tabellen `Sales.SalesOrderHeader` und `Sales.SalesOrderDetail` wurden 50-mal vergrößert und in `Sales.SalesOrderHeaderBulk` und `Sales.SalesOrderDetailBulk` umbenannt.

Der Ausführungsplan zeigt einen [Hashjoin](../relational-databases/performance/joins.md#hash) zwischen zwei Tabellen sowie alle parallel ausgeführten Operatoren, wie dies durch den gelben Kreis mit den beiden Pfeilen angegeben wird. Jeder Parallelitätsoperator ist ein anderer Branch im Plan. Daher weist der nachfolgende Ausführungsplan drei Branches auf. 

![Paralleler Abfrageplan](../relational-databases/media/schedule-parallel-query-plan.png)

> [!NOTE]
> Wenn Sie sich einen Ausführungsplan als Struktur vorstellen, ist ein **Branch** ein Bereich des Plans, der einen oder mehrere Operatoren zwischen Parallelitätsoperatoren gruppiert. Hierfür wird auch die Bezeichnung Exchange-Iteratoren verwendet. Weitere Informationen zu Planoperatoren finden Sie unter [Referenz zu logischen und physischen Showplanoperatoren](../relational-databases/showplan-logical-and-physical-operators-reference.md). 

Zwar enthält der Ausführungsplan drei Branches, während der Ausführung können jedoch immer nur zwei Branches gleichzeitig in diesem Ausführungsplan ausgeführt werden:
1.  Der Branch, in dem ein *Clustered Index Scan* für `Sales.SalesOrderHeaderBulk` (Buildeingabe des Joins) verwendet wird, wird allein ausgeführt.
2.  Der Branch, in dem ein *gruppierter Indexscan* im `Sales.SalesOrderDetailBulk` (Testeingabe des Joins) verwendet wird, wird dann gleichzeitig mit dem Branch ausgeführt, in dem die *Bitmap* erstellt wurde und in dem zur selben Zeit der *Hashvergleich* ausgeführt wird.

Die Showplan XML-Ereignisklasse zeigt an, dass 16 Workerthreads reserviert und auf den NUMA-Knoten 0 verwendet wurden:

```xml
<ThreadStat Branches="2" UsedThreads="16">
  <ThreadReservation NodeId="0" ReservedThreads="16" />
</ThreadStat>
```

Die Threadreservierung stellt sicher, dass die [!INCLUDE[ssde_md](../includes/ssde_md.md)] über genügend Arbeitsthreads verfügt, um alle Tasks auszuführen, die für die Anforderung benötigt werden. Threads können für mehrere NUMA-Knoten oder nur in einem NUMA-Knoten reserviert werden. Die Threadreservierung erfolgt zur Laufzeit vor Beginn der Ausführung und hängt von der Auslastung des Schedulers ab. Die Anzahl der reservierten Arbeitsthreads wird generisch mithilfe der Formel ***gleichzeitige Branches* * *Laufzeit-DOP*** ermittelt und schließt den übergeordneten Arbeitsthread aus. Jeder Branch ist auf eine Anzahl von Arbeitsthreads beschränkt, die MAXDOP entspricht. In diesem Beispiel gibt es zwei gleichzeitige Branches, und MAXDOP ist auf 8 festgelegt, daher gilt **2 * 8 = 16**.

Beachten Sie als Referenz den Live-Ausführungsplan im Artikel [Live-Abfragestatistik](../relational-databases/performance/live-query-statistics.md), bei dem ein Branch abgeschlossen ist und zwei Branches gleichzeitig ausgeführt werden.

![Paralleler Live-Abfrageplan](../relational-databases/media/schedule-parallel-query-live-plan.png)

Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] weist einen Arbeitsthread zum Ausführen eines aktiven Tasks (1:1) zu. Dies kann während der Abfrageausführung beobachtet werden, indem die DMV [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) wie im folgenden Beispiel gezeigt abgefragt wird:

```sql
SELECT parent_task_address, task_address, 
       task_state, scheduler_id, worker_address
FROM sys.dm_os_tasks
WHERE session_id = <insert_session_id>
ORDER BY parent_task_address, scheduler_id;
```

> [!TIP]
> Die Spalte `parent_task_address` ist für den übergeordneten Task stets NULL. 

> [!TIP]
> Bei einer stark ausgelasteten [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] überschreitet die Anzahl der aktiven Tasks möglicherweise den von reservierten Threads festgelegten Grenzwert. Diese Tasks können zu einem Branch gehören, der nicht mehr verwendet wird und sich in einem Übergangszustand befindet, während er auf seine Bereinigung wartet. 

[!INCLUDE[ssResult](../includes/ssresult-md.md)] Beachten Sie, dass es 17 aktive Tasks für die gleichzeitig ausgeführten Branches gibt: 16 untergeordnete Tasks, die den reservierten Threads entsprechen, sowie der übergeordnete bzw. koordinierende Task.

|parent_task_address|task_address|task_state|scheduler_id|worker_address|
|--------|--------|--------|--------|--------|
|NULL|**0x000001EF4758ACA8**|SUSPENDED|3|0x000001EFE6CB6160|
|0x000001EF4758ACA8|0x000001EFE43F3468|SUSPENDED|0|0x000001EF6DB70160|
|0x000001EF4758ACA8|0x000001EEB243A4E8|SUSPENDED|0|0x000001EF6DB7A160|
|0x000001EF4758ACA8|0x000001EC86251468|SUSPENDED|5|0x000001EEC05E8160|
|0x000001EF4758ACA8|0x000001EFE3023468|SUSPENDED|5|0x000001EF6B46A160|
|0x000001EF4758ACA8|0x000001EFE3AF1468|SUSPENDED|6|0x000001EF6BD38160|
|0x000001EF4758ACA8|0x000001EFE4AFCCA8|SUSPENDED|6|0x000001EF6ACB4160|
|0x000001EF4758ACA8|0x000001EFDE043848|SUSPENDED|7|0x000001EEA18C2160|
|0x000001EF4758ACA8|0x000001EF69038108|SUSPENDED|7|0x000001EF6AEBA160|
|0x000001EF4758ACA8|0x000001EFCFDD8CA8|SUSPENDED|8|0x000001EFCB6F0160|
|0x000001EF4758ACA8|0x000001EFCFDD88C8|SUSPENDED|8|0x000001EF6DC46160|
|0x000001EF4758ACA8|0x000001EFBCC54108|SUSPENDED|9|0x000001EFCB886160|
|0x000001EF4758ACA8|0x000001EC86279468|SUSPENDED|9|0x000001EF6DE08160|
|0x000001EF4758ACA8|0x000001EFDE901848|SUSPENDED|10|0x000001EFF56E0160|
|0x000001EF4758ACA8|0x000001EF6DB32108|SUSPENDED|10|0x000001EFCC3D0160|
|0x000001EF4758ACA8|0x000001EC8628D468|SUSPENDED|11|0x000001EFBFA4A160|
|0x000001EF4758ACA8|0x000001EFBD3A1C28|SUSPENDED|11|0x000001EF6BD72160|

Beachten Sie, dass jedem der 16 untergeordneten Tasks ein anderer Arbeitsthread zugewiesen ist (siehe Spalte `worker_address`), aber alle Worker demselben Pool von acht Schedulern (0, 5, 6, 7, 8, 9, 10, 11) zugewiesen sind, und dass der übergeordnete Tasks einem Scheduler außerhalb dieses Pools (3) zugewiesen ist.

> [!IMPORTANT]
> Sobald die ersten parallelen Tasks für einen bestimmten Branch eingeplant wurden, verwendet die [!INCLUDE[ssde_md](../includes/ssde_md.md)] denselben Pool von Schedulern für alle zusätzlichen Tasks in anderen Branches. Dies bedeutet, dass dieselben Scheduler für alle parallelen Tasks im gesamten Ausführungsplan verwendet werden, nur begrenzt durch MAXDOP.  
> Die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] versucht immer, Planer aus demselben NUMA-Knoten für die Taskausführung zuzuweisen. Diese werden sequenziell (im Roundrobin-Verfahren) zugewiesen, wenn Planer verfügbar sind. Der dem übergeordneten Task zugewiesene Arbeitsthread kann jedoch von anderen Tasks in einem anderen NUMA-Knoten platziert werden.

Ein Arbeitsthread kann im Scheduler nur für die Dauer seines Quantums (4 ms) aktiv bleiben und muss seinen Scheduler nach Ablauf dieses Quantums anhalten, sodass möglicherweise ein Arbeitsthread aktiv wird, der einem anderen Task zugewiesen ist. Wenn das Quantum eines Workers abläuft und nicht mehr aktiv ist, wird der betreffende Task im Status RUNNABLE in einer FIFO-Warteschlange abgelegt, bis er nochmals in den Status RUNNING wechselt. Dabei wird vorausgesetzt, dass der Task keinen Zugriff auf Ressourcen benötigt, die momentan nicht verfügbar sind, z. B. ein Latch oder eine Sperre. Andernfalls würde der Task nicht in den Status RUNNABLE, sondern in den Status SUSPENDED versetzt, bis die Ressourcen verfügbar sind.  

> [!TIP] 
> Für die Ausgabe der oben gezeigten DMV befinden sich alle aktiven Tasks im Status SUSPENDED. Ausführlichere Informationen zu wartenden Tasks erhalten Sie durch Abfragen der DMV [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md). 

Zusammenfassend erzeugt eine parallele Anforderung mehrere Aufgaben. Jeder Task muss einem einzelnen Arbeitsthread zugewiesen werden. Jeder Arbeitsthread muss einem einzelnen Scheduler zugewiesen werden. Daher kann die Anzahl der verwendeten Scheduler nicht die Anzahl der parallelen Tasks pro Branch überschreiten, die durch die MaxDOP-Konfiguration oder einen Abfragehinweis festgelegt ist. Der koordinierende Thread trägt nicht zum MaxDOP-Limit bei. 

### <a name="allocating-threads-to-a-cpu"></a>Zuteilen von Threads zu einer CPU
Standardmäßig startet jede Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] jeden Thread, und das Betriebssystem verteilt Threads von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanzen je nach Last auf die Prozessoren (CPUs) eines Computers. Wenn Prozessaffinität auf Betriebssystemebene aktiviert wurde, weist das Betriebssystem jeden Thread einer bestimmten CPU zu. Im Gegensatz dazu weist [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-**Arbeitsthreads** zu **Planern** zu, die die Threads gleichmäßig auf die CPUs verteilen (im Roundrobin-Verfahren).
    
Um Multitasking zu ermöglichen, z. B. wenn mehrere Anwendungen auf dieselbe CPU-Gruppe zugreifen, verschiebt das Betriebssystem mitunter Arbeitsthreads zwischen verschiedenen CPUs. Obwohl dies aus der Sicht des Betriebssystems effizient ist, kann diese Aktivität die Leistung von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bei starker Systemauslastung reduzieren, da jeder Prozessorcache wiederholt mit Daten geladen wird. Durch das Zuweisen von CPUs für bestimmte Threads kann unter diesen Bedingungen die Leistung verbessert werden, weil das erneute Laden von Daten in den Prozessor entfällt und die Threadmigration zwischen CPUs reduziert wird (wodurch der Kontextwechsel reduziert wird). Diese Zuordnung zwischen einem Thread und einem Prozessor wird als Prozessoraffinität bezeichnet. Wenn Affinität aktiviert wurde, weist das Betriebssystem jeden Thread einer bestimmten CPU zu. 

Die [Affinitätsmaskenoption](../database-engine/configure-windows/affinity-mask-server-configuration-option.md) wird mit [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) festgelegt. Wenn die Affinitätsmaske nicht festgelegt wird, ordnet die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz den Zeitplanungsmodulen, die nicht durch die Bitmaske ausgeschlossen werden, gleichmäßig eine bestimmte Anzahl von Arbeitsthreads zu.

> [!CAUTION]
> Konfigurieren Sie nicht die CPU-Affinität im Betriebssystem und gleichzeitig die Affinitätsmaske in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Diese Einstellungen zielen auf dasselbe Ergebnis. Wenn die Konfigurationen inkonsistent sind, kann dies zu unvorhersehbaren Ergebnissen führen. Weitere Informationen hierzu finden Sie unter [Affinitätsmaskenoption](../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

Threadpools erleichtern das Optimieren der Leistung, wenn sehr viele Clients mit dem Server verbunden sind. Üblicherweise wird ein separater Betriebssystemthread für jede Abfrageanforderung erstellt. Wenn jedoch bei Hunderten von Verbindungen mit dem Server weiterhin ein Thread pro Abfrageanforderung verwendet wird, kann dabei eine große Menge an Systemressourcen verbraucht werden. Die Option [Max. Anzahl von Arbeitsthreads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) ermöglicht [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] das Erstellen eines Pools mit Arbeitsthreads, der eine große Anzahl von Abfrageanforderungen versorgen kann und so zur Verbesserung der Leistung beiträgt. 

### <a name="using-the-lightweight-pooling-option"></a>Verwenden der Lightweightpooling-Option
Das Wechseln zwischen Threadkontexten stellt möglicherweise keinen sehr großen Aufwand dar. Für die meisten Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] stellt es keinen Leistungsunterschied dar, ob die Lightweightpooling-Option auf 0 oder 1 festgelegt ist. Die einzigen Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], die möglicherweise von [Lightweightpooling](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) profitieren, sind diejenigen, die auf einem Computer mit folgenden Eigenschaften ausgeführt werden:    
* Ein großer Server mit mehreren CPUs.
* Alle CPUs werden mit nahezu maximaler Kapazität ausgeführt.
* Eine große Anzahl an Kontextwechseln findet statt.

Für diese Systeme kann eventuell eine geringe Leistungssteigerung erzielt werden, indem der Wert für Lightweightpooling auf 1 festgelegt wird.

> [!IMPORTANT]
> Verwenden Sie die Fibermodusplanung nicht für Routinevorgänge. Die Verwendung könnte zu Leistungseinbußen führen, wenn die gängigen Vorteile des Kontextwechsels nicht genutzt werden können, und dass einige Komponenten von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] im Fibermodus nicht ordnungsgemäß arbeiten können. Weitere Informationen finden Sie unter [Lightweightpooling](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

## <a name="thread-and-fiber-execution"></a>Thread- und Fiberausführung
Von Microsoft Windows wird ein numerisches Prioritätssystem verwendet, das von 1 bis 31 reicht, um die Ausführung von Threads zu planen. Null ist für die Verwendung durch das Betriebssystem reserviert. Wenn mehrere Threads auf die Ausführung warten, sendet Windows den Thread mit der höchsten Priorität.

Standardmäßig hat jede Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die Priorität 7, was als normale Priorität bezeichnet wird. Die Priorität von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Threads ist somit hoch genug, um ausreichend CPU-Ressourcen zu erhalten, ohne jedoch im Gegenzug andere Anwendungen zu beeinträchtigen. 

> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../includes/ssnotedepfuturedontuse-md.md)]  

Mithilfe der Konfigurationsoption [Prioritätserhöhung](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) kann die Priorität der Threads von einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf 13 heraufgesetzt werden. Diese Einstellung wird als "hohe Priorität" bezeichnet. Durch diese Einstellung erhalten [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Threads eine höhere Priorität als die meisten anderen Anwendungen. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Threads werden somit aller Wahrscheinlichkeit nach gesendet, sobald sie zur Ausführung bereit sind, und müssen nicht für Threads anderer Anwendungen zurücktreten. Auf diese Weise kann die Leistung gesteigert werden, wenn ein Server nur Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und keine anderen Anwendungen ausführt. Wenn jedoch eine arbeitsspeicherintensive Operation in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] durchgeführt wird, verfügen andere Anwendungen in der Regel nicht über eine ausreichend hohe Priorität, um Vorrang vor dem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Thread zu besitzen. 

Falls Sie mehrere Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf einem Computer ausführen und Prioritätserhöhung nur für einige Instanzen aktiviert ist, kann die Leistung von Instanzen, die mit normaler Priorität ausgeführt werden, beeinträchtigt werden. Auch kann die Leistung anderer Anwendungen und Komponenten auf dem Server beeinträchtigt werden, wenn die Prioritätserhöhung aktiviert ist. Diese Option sollte somit nur in genau kontrollierten Situationen verwendet werden.


## <a name="hot-add-cpu"></a>Hinzufügen von CPUs bei laufendem Systembetrieb
Hinzufügen von CPUs im laufenden Systembetrieb bedeutet, dass Sie CPUs dynamisch hinzufügen können, während das System ausgeführt wird. CPUs können auf verschiedene Weise hinzugefügt werden: physisch durch neue Hardware, logisch durch eine Onlinepartitionierung der Hardware oder virtuell über eine Virtualisierungsschicht. Das Hinzufügen von CPUs im laufenden Systembetrieb wird in [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] ab [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt.

Voraussetzungen für das Hinzufügen von CPUs im laufenden Systembetrieb:  
* Die Hardware muss das Hinzufügen von CPUs zu einem laufenden System unterstützen.
* Sie müssen eines der folgenden Betriebssysteme verwenden: Windows Server 2008 Datacenter 64-Bit Edition oder Windows Server 2008 Enterprise Edition für Itanium-basierte Systeme
* Erfordert [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise.
* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] kann nicht für die Verwendung von Soft-NUMA konfiguriert werden. Weitere Informationen zu Soft-NUMA finden Sie unter [Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md).

Neu hinzugefügte CPUs werden nicht automatisch von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verwendet. So wird verhindert, dass [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] CPUs verwendet, die aus einem anderen Grund hinzugefügt wurden. Führen Sie die [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md)-Anweisung aus, nachdem Sie CPUs hinzugefügt haben, damit die neuen CPUs von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] als verfügbare Ressourcen erkannt werden.

> [!NOTE]
> Falls Sie die Option [Affinity64 Mask](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) konfiguriert haben, muss diese Option so angepasst werden, dass die neuen CPUs verwendet werden.
 
## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>Bewährte Methoden zum Ausführen von SQL Server auf Computern mit mehr als 64 CPUs

### <a name="assigning-hardware-threads-with-cpus"></a>Zuweisen von Hardwarethreads zu CPUs
Verwenden Sie die Serverkonfigurationsoptionen „Affinity Mask“ und „Affinity64 Mask“ nicht, um Prozessoren an bestimmte Threads zu binden. Diese Optionen sind auf 64 CPUs beschränkt. Verwenden Sie stattdessen die Option `SET PROCESS AFFINITY` von [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md).

### <a name="managing-the-transaction-log-file-size"></a>Verwalten der Größe der Transaktionsprotokolldatei
Verlassen Sie sich nicht auf automatische Vergrößerung, um die Transaktionsprotokolldatei zu vergrößern. Das Vergrößern des Transaktionsprotokolls muss ein serieller Prozess sein. Das Erweitern des Protokolls kann bis zum Abschließen der Protokollerweiterung verhindern, dass Transaktionsschreibvorgänge fortgeführt werden. Weisen Sie stattdessen allen Protokolldateien im Voraus Speicherplatz zu, indem Sie die Dateigröße auf einen Wert festlegen, der hoch genug ist, um der typischen Arbeitsauslastung in der Umgebung gerecht zu werden.

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>Festlegen des maximalen Grads an Parallelität für Indexvorgänge
Die Leistung von Indexvorgängen, z. B. das Erstellen bzw. das erneute Erstellen von Indizes, kann auf Computern mit vielen CPUs verbessert werden, indem das Wiederherstellungsmodell der Datenbank vorübergehend entweder auf das massenprotokollierte oder auf das einfache Wiederherstellungsmodell festgelegt wird. Diese Indexvorgänge können eine bedeutende Protokollaktivität generieren, und Protokollkonflikte können sich auf den besten Grad an Parallelität (Degree of Parallelism, DOP) von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auswirken.

Zusätzlich zum Anpassen der Serverkonfigurationsoption **Max. Grad an Parallelität (MAXDOP)** sollten Sie die Parallelität für Indexvorgänge mit der Option [MAXDOP](../t-sql/statements/alter-index-transact-sql.md) anpassen. Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../relational-databases/indexes/configure-parallel-index-operations.md). Weitere Informationen und Leitlinien zum Anpassen des maximalen Grads an Parallelität finden Sie unter [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

### <a name="setting-the-maximum-number-of-worker-threads"></a>Festlegen der maximalen Anzahl von Arbeitsthreads
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] konfiguriert beim Start dynamisch die Serverkonfigurationsoption **Max. Anzahl von Arbeitsthreads**. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verwendet die Anzahl verfügbarer CPUs und die Systemarchitektur, um diese Serverkonfiguration während des Startvorgangs unter Verwendung einer dokumentierten [Formel](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md#Recommendations) zu bestimmen.

Diese Option ist eine erweiterte Option und sollte ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten SQL Server-Experten geändert werden. Wenn Sie ein Leistungsproblem vermuten, ist es wahrscheinlich nicht die Verfügbarkeit von Arbeitsthreads. Eine wahrscheinlichere Ursache ist z.B. E/A, die ein Warten der Arbeitsthreads bewirkt. Sinnvollerweise sollten Sie die Grundursache eines Leistungsproblems ermitteln, bevor Sie die Einstellung für die maximale Anzahl der Arbeitsthreads ändern. Wenn Sie jedoch die maximale Anzahl der Arbeitsthreads manuell festlegen müssen, muss dieser Konfigurationswert stets auf einen Wert festgelegt werden, der mindestens das Siebenfache der Anzahl der CPUs des Systems beträgt. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption „Maximale Anzahl von Arbeitsthreads“](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

### <a name="using-sql-trace-and-sql-server-profiler"></a>Verwenden der SQL-Ablaufverfolgung und von SQL Server Profiler
Es wird davon abgeraten, die SQL-Ablaufverfolgung und SQL Profiler in einer Produktionsumgebung zu verwenden. Der Aufwand zum Ausführen dieser Tools nimmt mit zunehmender Anzahl von CPUs zu. Wenn Sie die SQL-Ablaufverfolgung in einer Produktionsumgebung verwenden müssen, sollten Sie die Anzahl der Ablaufverfolgungsereignisse auf ein Minimum beschränken. Erstellen Sie für jedes Ablaufverfolgungsereignis ein Profil, und führen Sie einen Auslastungstest durch. Vermeiden Sie es, Ereignisse zu kombinieren, die die Leistung stark beeinträchtigen.

> [!IMPORTANT]
> Die SQL-Ablaufverfolgung und [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] sind veraltet. Der *Microsoft.SqlServer.Management.Trace*-Namespace, der die Objekte für die Microsoft SQL Server-Ablaufverfolgung und -Wiedergabe enthält, ist ebenfalls veraltet. 
> [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] 
> Verwenden Sie stattdessen erweiterte Ereignisse. Weitere Informationen zu [erweiterten Ereignissen](../relational-databases/extended-events/extended-events.md) finden Sie unter [Schnellstart: Erweiterte Ereignisse in SQL Server](../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) und [Verwenden des SSMS XEvent Profilers](../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] für die Analysis Services-Workloads ist NICHT veraltet und wird weiterhin unterstützt.

### <a name="setting-the-number-of-tempdb-data-files"></a>Festlegen der Anzahl der tempdb-Datendateien
Die Anzahl der Dateien hängt von der Anzahl der (logischen) Prozessoren auf dem Computer ab. Als allgemeine Regel gilt: Verwenden Sie die Anzahl von Datendateien, die der Anzahl von logischen Prozessoren entspricht, falls die Anzahl von logischen Prozessoren acht oder weniger beträgt. Verwenden Sie acht Datendateien, wenn die Anzahl von logischen Prozessoren größer als acht ist. Falls weiterhin ein Konflikt besteht, erhöhen Sie die Anzahl von Datendateien um ein Vielfaches von vier, bis der Konflikt auf ein akzeptables Ausmaß reduziert ist, oder ändern Sie die Workload bzw. den Code. Beachten Sie auch andere Empfehlungen für tempdb, die unter [Optimieren der tempdb-Leistung in SQL Server](../relational-databases/databases/tempdb-database.md#optimizing-tempdb-performance-in-sql-server) verfügbar sind. 

Sie können den Aufwand für die Datenbankverwaltung jedoch reduzieren, indem Sie die Parallelitätsanforderungen von tempdb sorgfältig überdenken. Wenn ein System beispielsweise über 64 CPUs verfügt und tempdb in der Regel nur von 32 Abfragen verwendet wird, lässt sich keine Leistungsverbesserung erzielen, indem die Anzahl der tempdb-Dateien auf 64 erhöht wird.

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>SQL Server-Komponenten, die mehr als 64 CPUs verwenden können
In der folgenden Tabelle sind [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Komponenten aufgeführt, und es wird angegeben, ob sie mehr als 64 CPUs verwenden können.

|Prozessname   |Ausführbares Programm |Verwenden von mehr als 64 CPUs |  
|----------|----------|----------|  
|SQL Server-Datenbank-Engine |Sqlserver.exe  |Ja |  
|Reporting Services |Rs.exe |Nein |  
|Analysis Services  |As.exe |Nein |  
|Integration Services   |Is.exe |Nein |  
|Service Broker |Sb.exe |Nein |  
|Volltextsuche   |Fts.exe    |Nein |  
|SQL Server-Agent   |Sqlagent.exe   |Nein |  
|SQL Server Management Studio   |Ssms.exe   |Nein |  
|SQL Server-Setup   |Setup.exe  |Nein |  
