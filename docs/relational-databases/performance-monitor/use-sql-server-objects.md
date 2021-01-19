---
title: Verwenden von SQL Server-Objekten | Microsoft-Dokumentation
description: Erfahren Sie mehr über SQL Server-Objekte und -Leistungsindikatoren, die der Systemmonitor zur Überwachung der Aktivität von Computern nutzt, die eine Instanz von SQL Server ausführen.
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- server performance [SQL Server], objects for monitoring
- database monitoring [SQL Server], objects for monitoring
- charts [SQL Server]
- System Monitor [SQL Server], counters
- counters [SQL Server], listed
- objects [SQL Server], performance monitoring
- objects [SQL Server], Windows System Monitor
- performance counters [SQL Server], about performance counters
- System Monitor [SQL Server], objects
- performance counters [SQL Server]
- counters [SQL Server], about performance counters
- tuning databases [SQL Server], objects for monitoring
- database performance [SQL Server], objects for monitoring
- SQL Server, objects
- monitoring performance [SQL Server], objects for monitoring
- Windows System Monitor [SQL Server], objects
- Windows System Monitor [SQL Server], counters
- counters [SQL Server]
- performance counters [SQL Server], listed
ms.assetid: bcd731b1-3c4e-4086-b58a-af7a3af904ad
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e293b33b0b20ab7545744c7d4e3f47351f8e0c29
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172192"
---
# <a name="use-sql-server-objects"></a>Verwenden von SQL Server-Objekten
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Objekte und Leistungsindikatoren bereitgestellt, die vom Systemmonitor zum Überwachen der Aktivität von Computern, die eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausführen, verwendet werden können. Ein Objekt ist eine beliebige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ressource, z.B. eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sperre oder ein Windows-Prozess. Jedes Objekt enthält einen oder mehrere Leistungsindikatoren, die verschiedene Aspekte der zu überwachenden Objekte ermitteln. So enthält z.B. das Objekt **SQL Server-Sperren** Leistungsindikatoren für die **Anzahl der Deadlocks/Sekunde** und die **Sperrtimeouts/Sekunde**.  
  
 Einige Objekte verfügen über mehrere Instanzen, wenn mehrere Ressourcen eines bestimmten Typs auf dem Computer vorhanden sind. So weist z.B. der Objekttyp **Prozessor** mehrere Instanzen auf, wenn ein System über mehrere Prozessoren verfügt. Der Objekttyp **Datenbanken** verfügt über eine Instanz für jede Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Einige Objekttypen (z.B. für den **Speicher-Manager** ) verfügen nur über eine Instanz. Wenn ein Objekttyp über mehrere Instanzen verfügt, können Sie Leistungsindikatoren hinzufügen, um die Statistiken für jede Instanz (oder in vielen Fällen für alle Instanzen gleichzeitig) nachzuverfolgen. Leistungsindikatoren für die Standardinstanz werden im Format **SQLServer:** _\<object name>_ angezeigt. Leistungsindikatoren für benannte Instanzen werden im Format **MSSQL$** _\<instance name>_ **:** _\<counter name>_ oder **SQLAgent$** _\<instance name>_ **:** _\<counter name>_ angezeigt.  
  
Werte von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Leistungsindikatoren werden mithilfe der Windows-Leistungsindikator-Engine (Windows Performance Counter, WPC) generiert. Einige Indikatorwerte werden nicht direkt von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] berechnet. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt Basiswerte für die WPC-Engine bereit, die die erforderlichen Berechnungen durchführt (z. B. die der Prozentsätze). Die dynamische Verwaltungssicht [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) stellt alle Indikatoren mit dem ursprünglichen, von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generierten Wert bereit. Die Spalte `cntr_type` gibt den Typ des Indikators an. Wie die WPC-Engine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Indikatorwerte verarbeitet, hängt von diesem Typ ab. Weitere Informationen zu Typen von Leistungsindikatoren finden Sie in der [WMI-Dokumentation](/windows/win32/wmisdk/wmi-performance-counter-types).
  
 Durch Hinzufügen oder Entfernen von Leistungsindikatoren zum bzw. aus dem Diagramm und Speichern der Diagrammeinstellungen können Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Objekte und -Leistungsindikatoren angeben, die beim Starten des Systemmonitors überwacht werden.  
  
 Sie können den Systemmonitor so konfigurieren, dass Statistiken von jedem beliebigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Leistungsindikator angezeigt werden. Darüber hinaus können Sie einen Schwellenwert für jeden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Leistungsindikator festlegen und anschließend eine Warnung generieren, wenn ein Leistungsindikator einen Schwellenwert überschreitet. Weitere Informationen zum Einrichten von Warnungen finden Sie unter [Erstellen einer SQL Server-Datenbankwarnung](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md).  
    
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Statistiken werden nur angezeigt, wenn eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist. Wenn Sie eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]anhalten und neu starten, wird die Anzeige der Statistiken unterbrochen und anschließend automatisch fortgesetzt. Beachten Sie außerdem, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Leistungsindikatoren im Systemmonitor-Snap-In angezeigt werden, selbst wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht ausgeführt wird. Bei einer gruppierten Instanz sind Leistungsindikatoren nur auf dem Knoten funktionsfähig, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.  
  
 Dieses Thema enthält folgende Abschnitte:  
  
-   [Leistungsobjekte für den SQL Server-Agent](#SQLServerAgentPOs)  
  
-   [Service Broker-Leistungsobjekte](#ServiceBrokerPOs)  
  
-   [SQL Server-Leistungsobjekte](#SQLServerPOs)  
  
-   [Leistungsobjekte für die SQL Server-Replikation](#SQLServerReplicationPOs)  
  
-   [SSIS-Pipelineleistungsindikatoren](#SsisPipelineCounters)  
  
-   [Erforderliche Berechtigungen](#RequiredPermissions)  
  
##  <a name="sql-server-agent-performance-objects"></a><a name="SQLServerAgentPOs"></a> Leistungsobjekte für den SQL Server-Agent  
 In der folgenden Tabelle sind die Leistungsobjekte für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent aufgeführt:  
  
|Leistungsobjekt|BESCHREIBUNG|  
|------------------------|-----------------|  
|[SQLAgent:Warnungen](../../relational-databases/performance-monitor/sql-server-agent-alerts-object.md)|Stellt Informationen zu Warnungen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bereit.|  
|[SQLAgent:Aufträge](../../relational-databases/performance-monitor/sql-server-agent-jobs-object.md)|Stellt Informationen zu Aufträgen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bereit.|  
|[SQLAgent:Auftragsschritte](../../relational-databases/performance-monitor/sql-server-agent-jobsteps-object.md)|Stellt Informationen zu Auftragsschritten des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bereit.|  
|[SQLAgent:Statistik](../../relational-databases/performance-monitor/sql-server-agent-statistics-object.md)|Stellt allgemeine Informationen zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent bereit.|  
  
##  <a name="service-broker-performance-objects"></a><a name="ServiceBrokerPOs"></a> Service Broker-Leistungsobjekte  
 In der folgenden Tabelle sind die Leistungsobjekte für [!INCLUDE[ssSB](../../includes/sssb-md.md)]aufgeführt.  
  
|Leistungsobjekt|BESCHREIBUNG|  
|------------------------|-----------------|  
|[SQLServer:Broker-Aktivierung](../../relational-databases/performance-monitor/sql-server-broker-activation-object.md)|Stellt Informationen zu aktivierten [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Tasks bereit.|  
|[SQLServer:Broker-Statistik](../../relational-databases/performance-monitor/sql-server-broker-statistics-object.md)|Stellt allgemeine Informationen zu [!INCLUDE[ssSB](../../includes/sssb-md.md)] bereit.|  
|[SQLServer:Broker-Transport](../../relational-databases/performance-monitor/sql-server-broker-dbm-transport-object.md)|Stellt Informationen zum [!INCLUDE[ssSB](../../includes/sssb-md.md)] -Netzwerk bereit.|  
  
##  <a name="sql-server-performance-objects"></a><a name="SQLServerPOs"></a> SQL Server-Leistungsobjekte  
 In der folgenden Tabelle werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Objekte beschrieben.  
  
|Leistungsobjekt|BESCHREIBUNG|  
|------------------------|-----------------|  
|[SQLServer:Zugriffsmethoden](../../relational-databases/performance-monitor/sql-server-access-methods-object.md)|Durchsucht und misst die Anzahl der Zuordnungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankobjekten (z.B. die Anzahl von Indexsuchläufen oder die Anzahl von Seiten, die Indizes und Daten zugeordnet sind).|  
|[SQLServer:Sicherungsmedium](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)|Stellt Informationen über Sicherungsmedien bereit, die von Sicherungs- und Wiederherstellungsvorgängen verwendet werden, z. B. über den Durchsatz des Sicherungsmediums.|  
|[SQL Server: Statistiken zu Batchantworten](../../relational-databases/performance-monitor/sql-server-batch-resp-statistics-object.md)|Leistungsindikatoren zum Nachverfolgen der SQL-Batchantwortzeiten.| 
|[SQLServer:Puffer-Manager](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)|Stellt Informationen über die Speicherpuffer bereit, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet werden, z.B. **freier Arbeitsspeicher** und **Puffercache-Trefferquote**.|  
|[SQLServer: Buffer Node](../../relational-databases/performance-monitor/sql-server-buffer-node.md)|Stellt Informationen dazu bereit, wie oft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] freie Seiten anfordert und auf diese zugreift.|  
|[SQLServer: Katalogmetadaten](../../relational-databases/performance-monitor/sql-server-catalog-metadata-object.md)|Definiert einen Objektmanager für Katalogmetadaten für SQL Server.| 
|[SQLServer:CLR](../../relational-databases/performance-monitor/sql-server-clr-object.md)|Stellt Informationen über die Common Language Runtime (CLR) bereit.|  
|[SQLServer:Columnstore](../../relational-databases/performance-monitor/sql-server-columnstore-object.md)|**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher).<br /><br /> Stellt Informationen zu Zeilengruppen und Segmenten für Columnstore-Indizes bereit.|  
|[SQLServer:Cursor-Manager nach Typ](../../relational-databases/performance-monitor/sql-server-cursor-manager-by-type-object.md)|Stellt Informationen zu Cursorn bereit.|  
|[SQLServer:Cursor-Manager gesamt](../../relational-databases/performance-monitor/sql-server-cursor-manager-total-object.md)|Stellt Informationen zu Cursorn bereit.|  
|[SQLServer:Datenbankspiegelung](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md)|Stellt Informationen zur Datenbankspiegelung bereit.|  
|[SQLServer:Datenbanken](../../relational-databases/performance-monitor/sql-server-databases-object.md)|Stellt Informationen zu einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank bereit, z.B. zum Umfang des freien Protokollspeichers oder zur Anzahl aktiver Transaktionen in der Datenbank. Es kann mehrere Instanzen dieses Objekts geben.|  
|[SQL Server:Als veraltet markierte Funktionen](../../relational-databases/performance-monitor/sql-server-deprecated-features-object.md)|Zählt, wie oft veraltete Funktionen verwendet werden.|  
|[SQLServer:Ausführungsstatistik](../../relational-databases/performance-monitor/sql-server-execstatistics-object.md)|Stellt Informationen zur Ausführungsstatistik bereit.|  
|[SQL Server:Externe Skripts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)|**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher).<br /><br /> Stellt Informationen zur externen Skriptausführung bereit.|  
|[SQLServer: FileTable](../../relational-databases/performance-monitor/sql-server-filetable-object.md)|Mit „FileTable“ verknüpfte Statistiken und nicht transaktionsgebundener Zugriff.|  
|[SQLServer, Allgemeine Statistik](../../relational-databases/performance-monitor/sql-server-general-statistics-object.md)|Stellt Informationen zur allgemeinen serverweiten Aktivität bereit, z. B. die Anzahl von Benutzern, die mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verbunden sind.|  
|[SQL Server:HADR-Verfügbarkeitsreplikat](../../relational-databases/performance-monitor/sql-server-availability-replica.md)|Stellt Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../includes/sshadr-md.md)] -Verfügbarkeitsreplikaten bereit.|  
|[SQL Server:HADR-Datenbankreplikat](../../relational-databases/performance-monitor/sql-server-database-replica.md)|Stellt Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../includes/sshadr-md.md)] -Datenbankreplikaten bereit.|  
|[SQL Server:HTTP-Speicher](../../relational-databases/performance-monitor/sql-server-http-storage-object.md)|Bietet Informationen zur Überwachung eines Microsoft Azure Storage-Kontos, wenn [SQL Server-Datendateien in Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md) verwendet werden.|  
|[SQLServer:Latches](../../relational-databases/performance-monitor/sql-server-latches-object.md)|Stellt Informationen zu Latches auf internen Ressourcen (z. B. Datenbankseiten) bereit, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet werden.|  
|[SQLServer:Sperren](../../relational-databases/performance-monitor/sql-server-locks-object.md)|Stellt Informationen zu einzelnen Sperranforderungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]bereit, z.B. Timeouts für Sperren und Deadlocks. Es kann mehrere Instanzen dieses Objekts geben.|  
|[SQLServer: LogPool FreePool](../../relational-databases/performance-monitor/sql-server-logpool-freepool-object.md)|Beschreibt Statistiken für den freien Pool innerhalb des Protokollpools.|
|[SQLServer: Speicherbrokerclerks](../../relational-databases/performance-monitor/sql-server-memory-broker-clerks-object.md)|Statistiken zu Speicherbrokerclerks.|
|[SQLServer:Speicher-Manager](../../relational-databases/performance-monitor/sql-server-memory-manager-object.md)|Stellt Informationen zur Speicherauslastung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bereit, z.B. die Gesamtanzahl der aktuell zugewiesenen Sperrstrukturen.|  
|[SQLServer:Plancache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)|Stellt Informationen zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Cache bereit, der zum Speichern von Objekten wie gespeicherten Prozeduren, Triggern und Abfrageplänen verwendet wird.|  
|[SQLServer: Abfragespeicher](../../relational-databases/performance-monitor/sql-server-query-store-object.md)|Stellt Informationen zum Abfragespeicher bereit.|  
|[SQLServer: Ressourcenpool-ID](../../relational-databases/performance-monitor/sql-server-resource-pool-stats-object.md)|Stellt Informationen über Statistiken für Ressourcenpools in der Ressourcenkontrolle bereit.|  
|[SQLServer:SQL-Fehler](../../relational-databases/performance-monitor/sql-server-sql-errors-object.md)|Stellt Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlern bereit.|  
|[SQLServer:SQL-Statistik](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)|Stellt Informationen zu Aspekten von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfragen bereit, z.B. die Anzahl von Batches von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]empfangen hat.|  
|[SQLServer:Transaktionen](../../relational-databases/performance-monitor/sql-server-transactions-object.md)|Stellt Informationen zu den aktiven Transaktionen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]bereit, z.B. die Gesamtanzahl von Transaktionen und die Anzahl von Momentaufnahmetransaktionen.|  
|[SQLServer:Benutzerdefinierbar](../../relational-databases/performance-monitor/sql-server-user-settable-object.md)|Führt eine benutzerdefinierte Überwachung aus. Jeder Leistungsindikator kann eine benutzerdefinierte gespeicherte Prozedur oder eine beliebige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung sein, die einen Wert zurückgibt, der überwacht werden soll.|  
|[SQLServer: Wartestatistik](../../relational-databases/performance-monitor/sql-server-wait-statistics-object.md)|Stellt Informationen zu Wartezeiten bereit.|  
|[SQLServer: Statistiken für Arbeitsauslastungsgruppen](../../relational-databases/performance-monitor/sql-server-workload-group-stats-object.md)|Stellt Informationen zur Ressourcenkontrollen-Arbeitsauslastungsgruppenstatistik bereit.|  
  
##  <a name="sql-server-replication-performance-objects"></a><a name="SQLServerReplicationPOs"></a> Leistungsobjekte für die SQL Server-Replikation  
 In der folgenden Tabelle sind die Leistungsobjekte für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Replikation aufgeführt:  
  
|Leistungsobjekt|BESCHREIBUNG|  
|------------------------|-----------------|  
|**SQLServer:Replikations-Agents**<br /><br /> **SQLServer:Replikationsmomentaufnahme**<br /><br /> **SQLServer:Replikationsprotokollleser**<br /><br /> **SQLServer:Replikationsverteilung**<br /><br /> **SQLServer:Replikationsmerge**<br /><br /> Weitere Informationen finden Sie unter [Monitoring Replication with System Monitor](../../relational-databases/replication/monitor/monitoring-replication-with-system-monitor.md).|Stellt Informationen zur Aktivität des Replikations-Agents bereit.|  
  
##  <a name="ssis-pipeline-counters"></a><a name="SsisPipelineCounters"></a> SSIS-Pipelineleistungsindikatoren  
 Informationen zum **SSIS-Pipeline** -Leistungsindikator finden Sie unter [Leistungsindikatoren](../../integration-services/performance/performance-counters.md).  
  
##  <a name="required-permissions"></a><a name="RequiredPermissions"></a> Erforderliche Berechtigungen  
 Die Verwendung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Objekten hängt von Windows-Berechtigungen ab, außer für **SQLAgent:Warnungen**. Die Benutzer müssen Mitglied der festen Serverrolle **sysadmin** sein, um **SQLAgent:Warnungen** zu verwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden von Leistungsobjekten](../../ssms/agent/use-performance-objects.md)   
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
