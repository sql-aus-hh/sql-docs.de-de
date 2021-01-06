---
title: 'Problembehandlung: Verfügbarkeitsgruppe hat RTO überschritten (SQL Server) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die Problembehandlung bei einem Failover einer AlwaysOn-Verfügbarkeitsgruppe durchführen, wenn das Failover mehr Zeit als Ihr RTO (Recovery Time Objective, Wiederherstellungszeitziel) in SQL Server benötigt.
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: e83e4ef8-92f0-406f-bd0b-dc48dc210517
author: rothja
ms.author: jroth
ms.openlocfilehash: 5847162edb0d962826931624f4d14a3d78468066
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641876"
---
# <a name="troubleshoot-availability-group-exceeded-rto"></a>Problembehandlung: Verfügbarkeitsgruppe hat RTO überschritten
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Nach einem automatischen Failover oder einem geplanten manuellen Failover ohne Datenverlust für eine Verfügbarkeitsgruppe stellen Sie möglicherweise fest, dass die Failoverzeit Ihre Recovery Time Objective (RTO) überschritten hat. Ein anderer Fall: Wenn Sie die Failoverzeit eines sekundären Replikats im synchronen Commitmodus (z.B. eines Partners für das automatische Failover) mithilfe der Methode (siehe [Überwachen der Leistung von Always On-Verfügbarkeitsgruppen](monitor-performance-for-always-on-availability-groups.md)) einschätzen, stellen Sie fest, dass diese Ihre RTO überschreitet.  
  
 Wenn Ihr automatische Failover trotzdem nicht abgeschlossen wurde, lesen Sie [Problembehandlung bei automatischen Failover in SQL Server 2012 Always On-Umgebungen](https://support.microsoft.com/kb/2833707).  
  
 In den folgenden Abschnitten werden häufige Ursachen für die Überschreitung der RTO im Rahmen der Failoverzeit beschrieben.  
  
1.  [Eine meldende Workload verhindert die Ausführung des Wiederholungsthreads.](#BKMK_REDOBLOCK)  
  
2.  [Ein Wiederholungsthread gerät aufgrund von Ressourcenkonflikten in den Rückstand.](#BKMK_CONTENTION)  
  
##  <a name="reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> Eine meldende Workload verhindert die Ausführung des Wiederholungsthreads.  
 Der Wiederholungsthread auf dem sekundären Replikat wird durch eine lang andauernde schreibgeschützte Abfrage an der Durchführung von Änderungen an der Datendefinitionssprache (DDL) gehindert.  
  
### <a name="explanation"></a>Erklärung  
 Auf dem sekundären Replikat rufen die schreibgeschützten Abfragen Schemastabilitätssperren (`Sch-S`) ab. Diese `Sch-S`-Sperren können den Wiederholungsthread am Abruf von Schemaänderungssperren (`Sch-M`) zur Durchführung von DDL-Änderungen hindern. Ein blockierter Wiederholungsthread kann erst Protokolldatensätze anwenden, wenn die Blockierung aufgehoben wurde. Sobald die Blockierung aufgehoben wurde, kann dieser bis zum Ende des Protokolls aufholen und die Fortsetzung des nachfolgenden Prozesses zur Rückgängigmachung und den Failoverprozess zulassen.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Wenn der Wiederholungsthread blockiert wurde, wird ein erweitertes Ereignis namens `sqlserver.lock_redo_blocked` generiert. Darüber hinaus können Sie die DMV „sys.dm_exec_request“ für das sekundäre Replikat abfragen, um herauszufinden, welche Sitzung den REDO-Thread blockiert, und dann entsprechende Korrekturmaßnahmen treffen. Die folgende Abfrage gibt die Sitzungs-ID der schreibgeschützten Abfrage zurück, die den Wiederholungsthread blockiert.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 Sie können die Ausführung der meldenden Workload abschließen. An diesem Punkt ist die Blockierung des Wiederholungsthreads aufgehoben. Alternativ können Sie die Blockierung des Wiederholungsthreads sofort aufheben, indem Sie den Befehl [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) für die blockierende Sitzungs-ID ausführen.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_CONTENTION"></a> Ein Wiederholungsthread gerät aufgrund von Ressourcenkonflikten in den Rückstand.  
 Eine umfangreiche meldende Workload auf dem sekundären Replikat hat das sekundäre Replikat verlangsamt, weshalb der Wiederholungsthread in den Rückstand geraten ist.  
  
### <a name="explanation"></a>Erklärung  
 Bei der Anwendung von Protokolldatensätzen auf das sekundäre Replikat liest der Wiederholungsthread die Protokolldatensätze vom Protokolldatenträger und greift dann für jeden Protokolldatensatz auf die Datenseiten zu, um den Protokolldatensatz anzuwenden. Der Seitenzugriff kann E/A-gebunden sein (beim Zugriff auf den physischen Datenträger), wenn sich die Seite noch nicht im Pufferpool befindet. Wenn eine E/A-gebundene meldende Workload vorhanden ist, konkurriert die meldende Workload mit dem Wiederholungsthread um E/A-Ressourcen und kann den Wiederholungsthread verlangsamen.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Anhand der folgenden DMV-Abfrage können Sie erkennen, wie weit der Wiederholungsthread im Rückstand liegt, indem Sie die Diskrepanz zwischen `last_redone_lsn` und `last_received_lsn` messen.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Wenn der Wiederholungsthread tatsächlich im Rückstand liegt, müssen Sie der Ursache für die Leistungsbeeinträchtigung beim sekundären Replikat auf den Grund gehen. Falls ein E/A-Konflikt bei der meldenden Workload besteht, können Sie mithilfe des [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) bis zu einem gewissen Grad die von der meldenden Workload verwendeten CPU-Zyklen und so indirekt die durchgeführten E/A-Zyklen steuern. Wenn die meldende Workload 10 Prozent der CPU verbraucht, die Workload jedoch E/A-gebunden ist, können Sie zur Drosselung von Leseworkloads den CPU-Ressourceneinsatz mithilfe des Resource Governor auf 5 % beschränken, wodurch die Auswirkungen auf die E/A minimiert werden.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Behandlung von Leistungsproblemen in SQL Server (gilt für SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  
