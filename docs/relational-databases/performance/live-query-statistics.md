---
title: Live-Abfragestatistik | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Liveausführungsplan einer aktiven Abfrage in SQL Server Management Studio anzeigen. Verwenden Sie die Ausführungsstatistiken zum Debuggen von Abfrageleistungsproblemen.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query statistics [SQL Server] live query stats
- live query statistics
- debugging [SQL Server], live query stats
- statistics [SQL Server], live query statistics
- query profiling
- lightweight query profiling
- lightweight profiling
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee989011b8e5cf6092fdabd9ee0afcd1a06e0e58
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171372"
---
# <a name="live-query-statistics"></a>Live-Abfragestatistik
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] bietet die Möglichkeit, den Live-Ausführungsplan einer aktiven Abfrage anzuzeigen. Dieser Live-Abfrageplan bietet Einblicke in Echtzeit in den Ausführungsprozess der Abfrage, während die Steuerelemente von einem [Abfrageplanoperator](../../relational-databases/showplan-logical-and-physical-operators-reference.md) zu einem anderen übertragen werden. Der Live-Abfrageplan zeigt den gesamten Abfragestatus und die Laufzeit-Ausführungsstatistik auf Operatorebene an, wie z.B. die Anzahl der erzeugten Zeilen, die verstrichene Zeit, den Operatorstatus usw. Da diese Daten in Echtzeit verfügbar sind und es nicht nötig ist, auf den Abschluss der Abfrage zu warten, sind diese Ausführungsstatistiken äußerst nützlich für das Debuggen von Leistungsproblemen in Zusammenhang mit Abfragen. Diese Funktion ist ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] verfügbar, funktioniert unter Umständen jedoch auch mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  

> [!NOTE]
> Intern nutzen Live-Abfragestatistiken die [dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)-DMV.
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
> [!WARNING]  
> Diese Funktion wird hauptsächlich für Problembehandlungszwecke vorgesehen. Mit dieser Funktion kann die gesamte Abfrageleistung leicht verlangsamt werden, insbesondere in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Weitere Informationen finden Sie unter [Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md).  
> Diese Funktion kann mit dem [Transact-SQL-Debugger](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md)verwendet werden.  
  
## <a name="to-view-live-query-statistics-for-one-query"></a>So zeigen Sie Live-Abfragestatistiken für eine Abfrage an 
  
1.  Klicken Sie zum Anzeigen des Live-Abfrageausführungsplans im Menü „Extras“ auf das Symbol **Live-Abfragestatistik einschließen**.  
  
     ![Schaltfläche „Live-Abfragestatistik“ auf der Symbolleiste](../../relational-databases/performance/media/livequerystatstoolbar.png "Schaltfläche „Live-Abfragestatistik“ auf der Symbolleiste")  
  
     Zugriff auf den Ausführungsplan einer aktiven Abfrage erhalten Sie außerdem, indem Sie mit der rechten Maustaste auf eine ausgewählte Abfrage in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] klicken und anschließend auf **Live-Abfragestatistiken einschließen** klicken.  
  
     ![Schaltfläche „Live-Abfragestatistik“ im Popupmenü](../../relational-databases/performance/media/livequerystatsmenu.png "Schaltfläche „Live-Abfragestatistik“ im Popupmenü")  
  
2.  Führen Sie nun die Abfrage aus. Der Live-Abfrageplan zeigt den Abfragestatus sowie Statistiken zur Laufzeitausführung (beispielsweise verstrichene Zeit, Status usw.) für den Abfrageplanoperator an. Die Informationen zum Abfragestatus und die Ausführungsstatistik werden während der Ausführung der Abfrage regelmäßig aktualisiert. Diese Informationen geben Auskunft über den Status der Abfrageausführung und sind für das Debuggen von Abfragen mit langer Ausführungszeit, von Abfragen mit unbegrenzter Ausführungszeit, von Abfragen, die einen tempdb-Überlauf verursachen können, sowie von Timeoutproblemen nützlich.  
  
     ![Schaltfläche „Live-Abfragestatistik“ im Showplan](../../relational-databases/performance/media/livequerystatsplan.png "Schaltfläche „Live-Abfragestatistik“ im Showplan")  
  
## <a name="to-view-live-query-statistics-for-any-query"></a>So zeigen Sie Live-Abfragestatistiken für eine beliebige Abfrage an 

Sie können auf den Plan für aktive Abfragen auch über den **[Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md)** zugreifen, indem Sie mit der rechten Maustaste auf eine beliebige Abfrage in der Tabelle **Prozesse** oder **Aktuelle ressourcenintensive Abfragen** klicken.  
  
 ![Schaltfläche „Live-Abfragestatistik“ im Aktivitätsmonitor](../../relational-databases/performance/media/livequerystatsactmon.png "Schaltfläche „Live-Abfragestatistik“ im Aktivitätsmonitor")  
  
## <a name="remarks"></a>Bemerkungen  
 Die Infrastruktur des Statistikprofils muss aktiviert sein, bevor die Live-Abfragestatistik Informationen zum Status von Abfragen erfassen kann. Abhängig von der Version kann der Mehraufwand erheblich sein. Weitere Informationen zu diesem Mehraufwand finden Sie unter [Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md).
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die `SHOWPLAN`-Berechtigung auf Datenbankebene, um die **Live-Abfragestatistik**-Ergebnisseite mit Daten aufzufüllen und benötigt alle erforderlichen Berechtigungen zum Ausführen der Abfrage.
Unter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird die Berechtigung `VIEW SERVER STATE` auf Serverebene benötigt, um die Live-Statistik anzuzeigen.  
In [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Premium-Tarifen ist die `VIEW DATABASE STATE`-Berechtigung für die Datenbank zur Anzeige der Live-Statistiken erforderlich. In den Tarifen [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Standard und Basic ist das Konto **Serveradministrator** oder **Azure Active Directory-Administrator** erforderlich, um Live-Statistiken anzuzeigen.
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausführungspläne](../../relational-databases/performance/execution-plans.md)    
 [Leitfaden zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md)    
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Tools für die Leistungsüberwachung und -optimierung](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Öffnen des Aktivitätsmonitors &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
 [Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md)