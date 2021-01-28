---
title: Überwachen der Speicherauslastung
description: 'Überwachen einer SQL Server-Instanz, um sicherzustellen, dass sich die Speicherauslastung im normalen Bereich bewegt. Arbeitsspeicher verwenden: Verfügbare Bytes und Arbeitsspeicher: „Seiten/s“-Zähler.'
ms.custom: ''
ms.date: 01/20/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tuning databases [SQL Server], memory
- monitoring server performance [SQL Server], memory usage
- isolating memory [SQL Server]
- paging rate [SQL Server]
- memory [SQL Server], monitoring usage
- monitoring [SQL Server], memory usage
- low-memory conditions
- database monitoring [SQL Server], memory usage
- available memory [SQL Server]
- page faults [SQL Server]
- monitoring performance [SQL Server], memory usage
- server performance [SQL Server], memory
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8954573b0c32eef45ec655b27d26e03e72be96ed
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688760"
---
# <a name="monitor-memory-usage"></a>Überwachen der Arbeitsspeicherauslastung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollten regelmäßig überwacht werden, um sicherzustellen, dass sich die Speicherauslastung im normalen Bereich bewegt. 

## <a name="configuring-ssnoversion-max-memory"></a>Konfigurieren des maximalen Arbeitsspeichers von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Standardmäßig kann eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz im Laufe der Zeit einen Großteil des verfügbaren Arbeitsspeichers des Windows-Betriebssystems auf dem Server verbrauchen. Sobald der Arbeitsspeicher abgerufen wurde, wird er nicht freigegeben, es sei denn, es wird eine hohe Arbeitsspeicherauslastung erkannt. Dies ist entwurfsbedingt und kein Anzeichen für einen Arbeitsspeicherverlust im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess. Verwenden Sie [die Option **Max. Serverarbeitsspeicher**](../../database-engine/configure-windows/server-memory-server-configuration-options.md), um die Menge des Arbeitsspeichers einzuschränken, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die meisten Anwendungsfälle belegen darf. Weitere Informationen finden Sie im [Handbuch zur Arbeitsspeicherverwaltungsarchitektur](../../relational-databases/memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).

In SQL Server für Linux erfolgt [das Festlegen des Arbeitsspeicherlimits](../../linux/sql-server-linux-performance-best-practices.md#advanced-configuration) über das Tool „mssql-conf“ und die [Einstellung „memory.memorylimitmb“](../../linux/sql-server-linux-configure-mssql-conf.md#memorylimit).  

## <a name="monitor-operating-system-memory"></a>Überwachen des Arbeitsspeichers für das Betriebssystem   
 Verwenden Sie die folgenden Windows-Serverleistungsindikatoren für die Überwachung auf unzureichenden Arbeitsspeicher. Viele Leistungsindikatoren für den Arbeitsspeicher des Betriebssystems können über die dynamischen Verwaltungssichten [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) und [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md) abgefragt werden.

-   **Arbeitsspeicher: Verfügbare Byte**  
Dieser Leistungsindikator gibt an, wie viele Bytes an Arbeitsspeicher derzeit für die Verwendung durch Prozesse verfügbar sind. Niedrige Werte für den Leistungsindikator **Verfügbare Bytes** können darauf hinweisen, dass insgesamt zu wenig Arbeitsspeicher für das Betriebssystem vorhanden ist. Dieser Wert kann per T-SQL mit [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).available_physical_memory_kb abgefragt werden.

-   **Arbeitsspeicher: Seiten/s**  
Dieser Leistungsindikator gibt die Anzahl der Seiten an, die entweder aufgrund von harten Seitenfehlern vom Datenträger abgerufen oder auf den Datenträger geschrieben wurden, um Speicherplatz im Arbeitssatz aufgrund von Seitenfehlern freizugeben. Ein hoher Wert für den Indikator **Seiten/s** kann auf überhöhte Auslagerungen hindeuten.  

-   **Arbeitsspeicher: Seitenfehler/s:** Dieser Leistungsindikator gibt die Rate der Seitenfehler für alle Prozesse und Systemprozesse an. Eine geringe Rate an Auslagerung auf den Datenträger, die aber nicht null (0) entspricht, ist normal, selbst wenn der Computer über ausreichend Arbeitsspeicher verfügt. Der Microsoft-Manager für virtuellen Arbeitsspeicher (VMM, Virtual Memory Manager) entnimmt Seiten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und anderen Prozessen, um die Größen der Workingsets dieser Prozesse anzupassen. Infolge der VMM-Aktivität kommt es häufig zu Seitenfehlern.  

-   **Prozess: Seitenfehler/s:** Dieser Leistungsindikator gibt die Rate der Seitenfehler für einen Benutzerprozess an. Überwachen Sie den Leistungsindikator **Prozess: Seitenfehler/s**, um zu ermitteln, ob die Datenträgeraktivität durch die Auslagerung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verursacht wird. Um zu ermitteln, ob die überhöhten Auslagerungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder einem anderen Prozess verursacht werden, sollten Sie den **Prozess: Seitenfehler/s**-Zähler der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozessinstanz überprüfen.  
  
 Weitere Informationen zum Auflösen überhöhter Auslagerungen finden Sie in der Betriebssystemdokumentation.  
  
## <a name="isolating-memory-used-by-ssnoversion"></a>Isolieren des von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendeten Arbeitsspeichers 

 Zum Überwachen der Arbeitsspeicherauslastung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Sie die folgenden [SQL Server-Objektleistungsindikatoren](use-sql-server-objects.md) verwenden. Viele SQL Server-Objektleistungsindikatoren können über die dynamischen Verwaltungssichten [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) und [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) abgefragt werden.

 Standardmäßig verwaltet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Arbeitsspeicheranforderungen dynamisch anhand der verfügbaren Systemressourcen. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mehr Arbeitsspeicher benötigt, wird das Betriebssystem nach der Verfügbarkeit von freiem physischem Arbeitsspeicher abgefragt. Anschließend wird der verfügbare Arbeitsspeicher verwendet. Wenn für das Betriebssystem nicht genügend Arbeitsspeicher zur Verfügung steht, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Arbeitsspeicher wieder für das Betriebssystem frei, bis der Zustand des unzureichenden Arbeitsspeichers beseitigt wurde, oder bis [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Limit **Min. Serverarbeitsspeicher** erreicht hat. Sie können die Option zur dynamischen Verwendung des Arbeitsspeichers jedoch auch mithilfe der Serverkonfigurationsoptionen **Min. Serverarbeitsspeicher** und **Max. Serverarbeitsspeicher** überschreiben. Weitere Informationen finden Sie unter [Arbeitsspeicheroptionen für den Server](../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
 Um die Menge des von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendeten Arbeitsspeichers zu überwachen, sollten Sie die folgenden Leistungsindikatoren überprüfen:  
  
-   **SQL Server: Speicher-Manager: Serverspeicher gesamt (KB)**  
Dieser Leistungsindikator gibt die Menge des Arbeitsspeichers des Betriebssystems an, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsspeicher-Manager derzeit für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beansprucht. Es wird erwartet, dass diese Zahl entsprechend der tatsächlichen Aktivität und nach dem Start von SQL Server steigt. Fragen Sie diesen Leistungsindikator mithilfe der dynamischen Verwaltungssicht [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) ab, und beobachten Sie die Spalte **committed_kb**.

-   **SQL Server: Speicher-Manager: Zielserverspeicher (KB):**  
Dieser Leistungsindikator gibt den idealen Arbeitsspeicher auf Grundlage der aktuellen Arbeitsauslastung an, den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verbrauchen kann. Vergleichen Sie diesen Leistungsindikator nach einiger Zeit mit normalen Betrieb mit dem **Serverspeicher gesamt**, um zu ermitteln, ob [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über den gewünschten Arbeitsspeicher verfügt. Nach einiger Zeit des normalen Betriebs sollten die Leistungsindikatoren **Serverspeicher gesamt** und **Zielserverspeicher** ähnlich sein. Wenn der Wert von **Serversspeicher gesamt** erheblich niedriger als der Wert von **Zielserverspeicher** ist, liegt bei der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz möglicherweise eine hohe Arbeitsspeicherauslastung vor. Für eine gewisse Zeit nach Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird erwartet, dass der Wert von **Serverspeicher gesamt** niedriger als der Wert von **Zielserverspeicher** ist, da **Serverspeicher gesamt** noch steigt. Fragen Sie diesen Leistungsindikator mithilfe der dynamischen Verwaltungssicht [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) ab, und beobachten Sie die Spalte **committed_target_kb**. Weitere Informationen und bewährte Methoden zum Konfigurieren des Arbeitsspeichers finden Sie in den [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).

-   **Prozess: Arbeitssatz**  
Dieser Leistungsindikator gibt den physischen Speicher an, der derzeit laut dem Betriebssystem von einem Prozess verwendet wird. Beachten Sie die Instanz „sqlservr.exe“ dieses Leistungsindikators. Fragen Sie diesen Leistungsindikator mithilfe der dynamischen Verwaltungssicht [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) ab, und beobachten Sie die Spalte **physical_memory_in_use_kb**.

-   **Prozess: Private Bytes:**  
Dieser Leistungsindikator gibt die Menge des Arbeitsspeichers an, den ein Prozess für die eigene Verwendung vom Betriebssystem angefordert hat. Beachten Sie die Instanz „sqlservr.exe“ dieses Leistungsindikators. Da dieser Leistungsindikator alle Arbeitsspeicherzuweisungen umfasst, die von „sqlservr.exe“ angefordert wurden, einschließlich derer, die nicht von der [Option **Max. Serverarbeitsspeicher**](../../database-engine/configure-windows/server-memory-server-configuration-options.md) begrenzt werden, kann dieser Leistungsindikator Werte melden, die über dem Wert der [Option **Max. Serverarbeitsspeicher**](../../database-engine/configure-windows/server-memory-server-configuration-options.md) liegen.

-   **SQL Server: Puffer-Manager: Datenbankseiten**  
Dieser Leistungsindikator gibt die Anzahl der Seiten im Pufferpool mit Datenbankinhalten an. Dieser Leistungsindikator umfasst keinen Arbeitsspeicher im SQL Server-Prozess, der nicht im Zusammenhang mit dem Pufferpool steht. Sie können diesen Leistungsindikator mithilfe der dynamischen Verwaltungssicht [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) abfragen.

-   **SQL Server: Puffer-Manager: Puffercache-Trefferquote**  
Dieser Wert ist für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spezifisch. Eine Quote von 90 oder höher ist wünschenswert. Ein Wert von über 90 gibt an, dass mehr als 90 Prozent aller Datenanforderungen vom Datencache im Arbeitsspeicher erfüllt wurden, ohne dass vom Datenträger gelesen werden musste. Weitere Informationen zum SQL Server-Puffer-Manager finden Sie im Artikel zum [SQL Server-Puffer-Manager-Objekt](sql-server-buffer-manager-object.md). Sie können diesen Leistungsindikator mithilfe der dynamischen Verwaltungssicht [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) abfragen.  
 
-   **SQL Server: Puffer-Manager: Seitenlebenserwartung**  
Dieser Leistungsindikator misst die Zeit in Sekunden, die die älteste Seite im Pufferpool verbleibt. Bei Systemen, die eine NUMA-Architektur verwenden, handelt es sich hierbei um einen Durchschnittswert aller NUMA-Knoten. Ein höherer, steigender Wert ist am besten. Ein plötzlicher Abfall weist auf signifikante Datenänderungen innerhalb und außerhalb des Pufferpools hin. Das bedeutet, dass die Arbeitsauslastung nicht vollständig von den Daten profitieren konnte, die sich bereits im Arbeitsspeicher befanden. Jeder NUMA-Knoten verfügt über einen eigenen Knoten des Pufferpools. Auf Servern mit mehreren NUMA-Knoten können Sie die Seitenlebenserwartung der einzelnen Pufferpoolknoten mit **SQL Server: Pufferknoten: Seitenlebenserwartung** abfragen. Sie können diesen Leistungsindikator mithilfe der dynamischen Verwaltungssicht [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) abfragen.

  
## <a name="examples"></a>Beispiele 

### <a name="determining-current-memory-allocation"></a>Bestimmen der aktuellen Speicherbelegung  
 Die folgenden Abfragen geben Informationen über aktuell belegten Arbeitsspeicher zurück.  
  
```  
SELECT
(total_physical_memory_kb/1024) AS Total_OS_Memory_MB,
(available_physical_memory_kb/1024)  AS Available_OS_Memory_MB
FROM sys.dm_os_sys_memory;

SELECT  
(physical_memory_in_use_kb/1024) AS Memory_used_by_Sqlserver_MB,  
(locked_page_allocations_kb/1024) AS Locked_pages_used_by_Sqlserver_MB,  
(total_virtual_address_space_kb/1024) AS Total_VAS_in_MB,
process_physical_memory_low,  
process_virtual_memory_low  
FROM sys.dm_os_process_memory;  
```  

### <a name="determining-current-sql-server-memory-utilization"></a>Ermitteln der aktuellen SQL Server-Arbeitsspeicherauslastung   
 Die folgende Abfrage gibt Informationen über die aktuelle SQL Server-Arbeitsspeicherauslastung zurück.  
```  
SELECT
sqlserver_start_time,
(committed_kb/1024) AS Total_Server_Memory_MB,
(committed_target_kb/1024)  AS Target_Server_Memory_MB
FROM sys.dm_os_sys_info;
```   

### <a name="determining-page-life-expectancy"></a>Ermitteln der Seitenlebenserwartung
 Die folgende Abfrage verwendet die dynamische Verwaltungssicht **sys.dm_os_performance_counters** zum Beobachten des aktuellen Werts für die **Seitenlebenserwartung** der SQL Server-Instanz auf Puffer-Manager-Gesamtebene und auf jeder NUMA-Knotenebene.
```
SELECT
CASE instance_name WHEN '' THEN 'Overall' ELSE instance_name END AS NUMA_Node, cntr_value AS PLE_s
FROM sys.dm_os_performance_counters    
WHERE counter_name = 'Page life expectancy';
```

## <a name="see-also"></a>Weitere Informationen
- [sys.dm_os_sys_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md)
- [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md)
- [sys.dm_os_sys_info (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)
- [sys.dm_os_performance_counters (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)
- [SQL Server, Speicher-Manager-Objekt](sql-server-memory-manager-object.md)
- [SQL Server, Puffer-Manager-Objekt](sql-server-buffer-manager-object.md)   
- [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md)
- [Handbuch zur Architektur der Speicherverwaltung](../../relational-databases/memory-management-architecture-guide.md)
