---
title: Überwachen der Leistung von Verfügbarkeitsgruppen
description: In diesem Artikel werden der Synchronisierungsprozess und die Berechnung einiger der wichtigsten Metriken beschrieben. Zudem enthält der Artikel Links zu einigen allgemeinen leistungsbezogenen Problembehandlungsszenarien.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: dfd2b639-8fd4-4cb9-b134-768a3898f9e6
author: rothja
ms.author: jroth
ms.openlocfilehash: fa6d63d665a2a81d0a112a1685b3ead1969479d1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643736"
---
# <a name="monitor-performance-for-always-on-availability-groups"></a>Überwachen der Leistung von Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Der Leistungsaspekt der Always On-Verfügbarkeitsgruppen ist entscheidend, um die Vereinbarung zum Servicelevel (SLA) für Ihre unternehmenskritischen Datenbanken zu erfüllen. Wenn Sie den Vorgang bei Verfügbarkeitsgruppen zum Senden von Protokollen an sekundäre Replikate verstehen, können Sie die Recovery Time Objective (RTO) und Recovery Point Objective (RPO) Ihrer Verfügbarkeitsimplementierung besser einschätzen und Engpässe bei leistungsschwachen Verfügbarkeitsgruppen oder Replikaten ausfindig machen. In diesem Artikel werden der Synchronisierungsprozess und die Berechnung einiger der wichtigsten Metriken beschrieben. Zudem enthält der Artikel Links zu einigen allgemeinen leistungsbezogenen Problembehandlungsszenarien.  
   
##  <a name="data-synchronization-process"></a>Datensynchronisierungsprozess  
 Um die Zeit bis zur vollständigen Synchronisierung einzuschätzen und den Engpass zu identifizieren, müssen Sie den Synchronisierungsprozess verstehen. Leistungsengpässe können an einer beliebigen Stelle im Prozess auftreten. Durch die Ermittlung des Engpasses können Sie den zugrunde liegenden Problemen besser auf den Grund gehen. Der folgende Abbildung und Tabelle veranschaulichen den Datensynchronisierungsprozess:  
  
 ![Datensynchronisierung für Verfügbarkeitsgruppen](media/always-onag-datasynchronization.gif "Datensynchronisierung für Verfügbarkeitsgruppen")  
  
|Sequenz|Beschreibung des Schritts|Kommentare|Nützliche Metriken|  
|-|-|-|-|  
|1|Protokollgenerierung|Protokolldaten werden auf den Datenträger geleert. Dieses Protokoll muss in die sekundären Replikate repliziert werden. Die Protokolldatensätze werden in die Sendewarteschlange eingereiht.|[SQL Server: Datenbank > Geleerte Protokollbytes/Sekunde](~/relational-databases/performance-monitor/sql-server-databases-object.md)|  
|2|Erfassung|Für jede Datenbank werden Protokolle erfasst und an die entsprechende Partnerwarteschlange gesendet (eine pro Datenbankreplikatspaar). Dieser Erfassungsprozess wird fortlaufend ausgeführt, solange das Verfügbarkeitsreplikat verbunden ist und die Datenverschiebung nicht aus irgendeinem Grund angehalten wird. Der Status des Datenbankreplikatspaar lautet entweder „Wird synchronisiert“ oder „Synchronisiert“. Wenn die Nachrichten beim Erfassungsprozess nicht schnell genug überprüft und in die Warteschlange eingereiht werden können, führt dies zum Anwachsen der Protokollsendewarteschlange.|[SQL Server: Verfügbarkeitsreplikat > An Replikat gesendete Bytes/Sekunde](~/relational-databases/performance-monitor/sql-server-availability-replica.md): Eine Aggregation der Summe aller Datenbanknachrichten in der Warteschlange für dieses Verfügbarkeitsreplikat.<br /><br /> [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) (KB) und [log_bytes_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) (KB/s) für das primäre Replikat|  
|3|Send|Die Nachrichten in jeder Datenbankreplikatswarteschlange werden aus der Warteschlange entfernt und über das Netzwerk an das entsprechende sekundäre Replikat gesendet.|[SQL Server: Verfügbarkeitsreplikat > An den Transport gesendete Bytes/Sekunde](~/relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|4|Empfang und Zwischenspeicherung|Jedes sekundäre Replikat empfängt und speichert die Nachricht zwischen.|Leistungsindikator [SQL Server: Verfügbarkeitsreplikat > Empfangene Protokollbytes/Sekunde](~/relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|5|Festschreibung|Ein Protokoll wird zur Festschreibung für das sekundäre Replikat geleert. Nachdem das Protokoll geleert wurde, wird eine Bestätigung an das primäre Replikat zurückgesendet.<br /><br /> Ist das Protokoll festgeschrieben, wurde ein Datenverlust abgewendet.|Leistungsindikator [SQL Server: Datenbank > Geleerte Protokollbytes/Sekunde](~/relational-databases/performance-monitor/sql-server-databases-object.md)<br /><br /> Wartetyp [HADR_LOGCAPTURE_SYNC](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)|  
|6|Wiederholen|Wiederholen Sie die geleerten Seiten auf dem sekundären Replikat. Seiten werden in der Wiederholungswarteschlange beibehalten, während sie darauf warten, wiederholt zu werden.|[SQL Server: Datenbankreplikat > Wiederholte Bytes/Sekunde](~/relational-databases/performance-monitor/sql-server-database-replica.md)<br /><br /> [redo_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) (KB) und [redo_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md).<br /><br /> Wartetyp [REDO_SYNC](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)|  
  
##  <a name="flow-control-gates"></a>Flusssteuerungsgates  
 Verfügbarkeitsgruppen werden mit Flusssteuerungsdaten für das primäre Replikat entworfen, um eine übermäßige Ressourcenauslastung wie etwa bei Netzwerk- und Speicherressourcen für alle Verfügbarkeitsreplikate zu vermeiden. Diese Flusssteuerungsgates wirken sich nicht auf den Synchronisierungsintegritätszustand der Verfügbarkeitsreplikate aus, können jedoch die allgemeine Leistung der Verfügbarkeitsdatenbanken beeinflussen, einschließlich der RPO.  
  
 Nachdem die Protokolle für das primäre Replikat erfasst wurden, unterliegen sie zwei Ebenen der Flusssteuerungen, wie in der folgenden Tabelle gezeigt.  
  
|Ebene|Anzahl der Gates|Anzahl der Nachrichten|Nützliche Metriken|  
|-|-|-|-|  
|Transport|1 pro Verfügbarkeitsreplikat|8192|Erweiterte Ereignisse **database_transport_flow_control_action**|  
|Datenbank|1 pro Verfügbarkeitsdatenbank|11200 (x64)<br /><br /> 1600 (x86)|[DBMIRROR_SEND](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)<br /><br /> Erweitertes Ereignis **hadron_database_flow_control_action**|  
  
 Sobald der Nachrichtenschwellenwert von einem der beiden Gates erreicht wird, werden keine Protokollnachrichten mehr an ein bestimmtes Replikat oder für eine bestimmte Datenbank gesendet. Nachrichten können gesendet werden, sobald Bestätigungsnachrichten für die gesendeten Nachrichten empfangen wurden, um die Anzahl der gesendeten Nachrichten auf einen Wert unterhalb des Schwellenwerts zu senken.  
  
 Neben den Flusssteuerungsgates gibt es einen weiteren Faktor, der das Senden der Protokollnachrichten verhindern kann. Durch die Synchronisierung von Replikaten wird sichergestellt, dass die Nachrichten gesendet und in der Reihenfolge der Protokollfolgenummern (Log Sequence Numbers, LSN) angewendet werden. Bevor eine Protokollnachricht gesendet wird, wird auch dessen LSN mit der niedrigsten bestätigten LSN-Zahl abgeglichen, um sicherzustellen, dass diese unter einem der Schwellenwerte (abhängig vom Nachrichtentyp) liegt. Wenn die Diskrepanz zwischen den zwei LSN-Zahlen größer als der Schwellenwert ist, werden die Nachrichten nicht gesendet. Wenn die Diskrepanz wieder unter dem Schwellenwert liegt, werden die Nachrichten gesendet.  
  
 Die zwei nützlichen Leistungsindikatoren [SQL Server: Verfügbarkeitsreplikat > Flusssteuerung/Sekunde](~/relational-databases/performance-monitor/sql-server-availability-replica.md) und [SQL Server: Verfügbarkeitsreplikat > Flusssteuerungszeit (ms/Sekunde)](~/relational-databases/performance-monitor/sql-server-availability-replica.md) zeigen, wie oft die Flusssteuerung innerhalb der letzten Sekunde aktiviert und wie viel Zeit für das Warten auf die Flusssteuerung benötigt wurde. Längere Wartezeiten bei der Flusssteuerung bedeuten eine höhere RPO. Weitere Informationen zu den Arten von Problemen, die eine lange Wartezeit für die Flusssteuerung verursachen können, finden Sie unter [Problembehandlung: Verfügbarkeitsgruppe hat RPO überschritten](troubleshoot-availability-group-exceeded-rpo.md).  
  
##  <a name="estimating-failover-time-rto"></a>Einschätzen der Failoverzeit (RTO)  
 Die RTO in Ihrer SLA hängt von der Failoverzeit Ihrer Always On-Implementierung an einem bestimmten Zeitpunkt ab, die mit der folgenden Formel ausgedrückt werden kann:  
  
 ![Verfügbarkeitsgruppen: RTO-Berechnung](media/always-on-rto.gif "Verfügbarkeitsgruppen: RTO-Berechnung")  
  
> [!IMPORTANT]  
>  Wenn eine Verfügbarkeitsgruppe mehr als eine Verfügbarkeitsdatenbank enthält, wird die Verfügbarkeitsdatenbank mit dem höchsten Tfailover-Wert zum Grenzwert für die RTO-Konformität.  
  
 Die Ausfallerkennungszeit, Tdetection, ist der Zeitaufwand, den das System zur Erkennung des Fehlers benötigt. Diese Zeit hängt von den Einstellungen auf Clusterebene ab, nicht von den einzelnen Verfügbarkeitsreplikaten. Abhängig von der konfigurierten Bedingung für ein automatisches Failover kann ein Failover als sofortige Antwort auf einen kritischen internen SQL Server-Fehler wie verwaiste Spinlocks ausgelöst werden. In diesem Fall kann die Erkennung genauso schnell sein wie das Senden des Fehlerberichts [sp_server_diagnostics &#40;Transact-SQL&#41; ](~/relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) an den WSFC-Cluster (das Standardintervall beträgt 1/3 des Timeout für die Integritätsprüfung). Ein Failover kann auch aufgrund eines Timeouts ausgelöst werden, z.B., wenn das Timeout für die Clusterintegritätsprüfung (standardmäßig 30 Sekunden) oder die Lease zwischen Ressourcen-DLLs und der SQL Server-Instanz (standardmäßig 20 Sekunden) abgelaufen ist. In diesem Fall ist die Erkennungszeit genauso lang wie das Timeoutintervall. Weitere Informationen finden Sie unter [Flexible Failoverrichtlinie für automatisches Failover einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](./configure-flexible-automatic-failover-policy.md?viewFallbackFrom=sql-server-2014).  
  
 Um für ein Failover bereit zu sein, muss das sekundäre Replikat lediglich eine Wiederholung ausführen, um das Ende des Protokolls zu erreichen. Die Wiederholungszeit, Tredo, wird mit der folgenden Formel berechnet:  
  
 ![Verfügbarkeitsgruppen: Berechnung der Wiederholungszeit](media/always-on-redo.gif "Verfügbarkeitsgruppen: Berechnung der Wiederholungszeit")  
  
 Hierbei steht *redo_queue* für den Wert in [redo_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) und *redo_rate* für den Wert in [redo_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md).  
  
 Der zeitliche Mehraufwand für das Failover, Toverhead, schließt den Zeitaufwand ein, der für ein Failover des WSFC-Clusters und die Aktivierung der Datenbanken erforderlich ist. Diese Zeit ist normalerweise kurz und konstant.  
  
## <a name="estimating-potential-data-loss-rpo"></a>Einschätzen des möglichen Datenverlusts (RPO)  
 Die RPO in Ihrer SLA hängt von dem möglichen Datenverlust Ihrer Always On-Implementierung zu einem beliebigen Zeitpunkt ab. Dieser mögliche Datenverlust kann mit der folgenden Formel ausgedrückt werden:  
  
 ![Verfügbarkeitsgruppen: RPO-Berechnung](media/always-on-rpo.gif "Verfügbarkeitsgruppen: RPO-Berechnung")  
  
 Hierbei steht *log_send_queue* für den Wert von [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) und *log generation rate* für den Wert von [SQL Server: Datenbank > Geleerte Protokollbytes/Sekunde](~/relational-databases/performance-monitor/sql-server-databases-object.md).  
  
> [!WARNING]  
>  Wenn eine Verfügbarkeitsgruppe mehr als eine Verfügbarkeitsdatenbank enthält, wird die Verfügbarkeitsdatenbank mit der höchsten Tdata_loss-Wert zum Grenzwert für die RPO-Konformität.  
  
 Die Protokollsendewarteschlange stellt alle Daten dar, die aufgrund eines schwerwiegenden Fehlers verloren gehen können. Auf den ersten Blick fällt auf, dass anstelle der Protokollsenderate die Protokollgenerierungsrate verwendet wird (siehe [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)). Denken Sie jedoch daran, dass Sie durch Verwendung der Protokollsenderate lediglich die Zeit zur Synchronisierung erhalten, während die RPO den Datenverlust abhängig von der Dauer des Datenverlusts misst, nicht abhängig von der Synchronisierungsdauer.  
  
 Eine einfachere Möglichkeit zur Einschätzung von Tdata_loss bietet die Verwendung von [last_commit_time](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md). Die DMV für das primäre Replikat meldet diesen Wert für alle Replikate. Sie können die Diskrepanz zwischen dem Wert für das primäre Replikat und dem für das sekundäre Replikat berechnen, um einzuschätzen, wie schnell das Protokoll für das sekundäre Replikat im Vergleich zum primären Replikat verarbeitet wird. Wie bereits erwähnt, sagt diese Berechnung nichts über den möglichen Datenverlust basierend auf der Dauer der Protokollgenerierung aus, sollte jedoch eine weitgehende Annäherung darstellen.  

## <a name="estimate-rto--rpo-with-the-ssms-dashboard"></a>Einschätzen von RTO und RPO mit dem SSMS-Dashboard
In AlwaysOn-Verfügbarkeitsgruppen werden RTO und RPO für die in sekundären Replikaten gehosteten Datenbanken berechnet und angezeigt. Auf dem Dashboard des primären Replikats werden RTO und RPO nach dem sekundären Replikat gruppiert. 

Um RTO und RPO im Dashboard anzuzeigen, führen Sie folgende Schritte aus:
1. Erweitern Sie in SQL Server Management Studio den Knoten **Hochverfügbarkeit mit Always On**. Klicken Sie mit der rechten Maustaste auf den Namen Ihrer Verfügbarkeitsgruppen, und klicken Sie dann auf **Dashboard anzeigen**. 
1. Wählen Sie **Spalten hinzufügen/entfernen** unter der Registerkarte **Gruppieren nach** aus. Aktivieren Sie **Geschätzte Wiederherstellungszeit (Sekunden)** [RTO] und **Geschätzter Datenverlust (Zeit)** [RPO]. 

   ![Screenshot: RTO-RPO-Dashboard](media/rto-rpo-dashboard.png)

### <a name="calculation-of-secondary-database-rto"></a>Berechnung des RTO der sekundären Datenbank 
Durch die Berechnung der Wiederherstellungszeit wird ermittelt, wie viel Zeit benötigt wird, um die *sekundäre Datenbank* nach einem Failover wiederherzustellen.  Die Failoverzeit ist normalerweise kurz und konstant. Die Erkennungszeit hängt von den Einstellungen auf Clusterebene und nicht von den einzelnen Verfügbarkeitsreplikaten ab. 


Bei einer sekundären Datenbank (DB_sec) basieren Berechnung und Darstellung des RTO auf ihren Werte für **redo_queue_size** und **redo_rate**:

![Berechnung des RTO](media/calculate-rto.png)

Mit Ausnahme von Sonderfällen ist die Formel zur Berechnung des RTO einer Sekundärdatenbank wie folgt:

![Formel zum Berechnen des RTO](media/formula-calc-second-dba-rto.png)



### <a name="calculation-of-secondary-database-rpo"></a>Berechnung des RPO der sekundären Datenbank

Für eine sekundäre Datenbank (DB_sec) basieren Berechnung und Anzeige des RPO auf den Werten für „is_failover_ready“, „last_commit_time“ und dem Wert für „last_commit_time“ ihrer primären korrelierten Datenbank (DB_pri). Wenn „secondary database.is_failover_ready = 1“, werden Daten synchronisiert, und beim Failover erfolgt kein Datenverlust. Wenn dieser Wert jedoch 0 ist, besteht eine Lücke zwischen **last_commit_time** in der primären Datenbank und **last_commit_time** in der sekundären Datenbank. 

Für die primäre Datenbank ist **last_commit_time** die Zeit, zu der für die letzte Transaktion ein Commit erfolgt ist. Für die sekundäre Datenbank ist **last_commit_time** die letzte Commitzeit für die Transaktion in der primären Datenbank, die auch in der sekundären Datenbank erfolgreich festgeschrieben wurde. Dieser Wert sollte für die primäre und die sekundäre Datenbank identisch sein. Eine Lücke zwischen diesen beiden Werten ist die Dauer, in der ausstehende Transaktionen nicht in der Sekundärdatenbank festgeschrieben wurden und bei einem Failover verloren gehen. 

![Berechnung des RPO](media/calculate-rpo.png)

### <a name="performance-counters-used-in-rtorpo-formulas"></a>In RTO-/RPO-Formeln verwendete Leistungsindikatoren

- **redo_queue_size** (KB) [*für RTO verwendet*]: Die Größe der Wiederholungswarteschlange ist die Größe der Transaktionsprotokolle zwischen **last_received_lsn** und **last_redone_lsn**. **last_received_lsn** ist die Protokollblock-ID, die den Punkt angibt, bis zu dem alle Protokollblöcke vom sekundären Replikat empfangen wurden, das diese sekundäre Datenbank hostet. **last_redone_lsn** ist die tatsächliche Protokollfolgenummer des letzten Protokolldatensatzes, der zuletzt für die sekundäre Datenbank wiederholt wurde. Basierend auf diesen beiden Werten können wir die IDs des Anfangsprotokollblocks (**last_received_lsn**) und des Endprotokollblocks (**last_redone_lsn**) bestimmen. Der Abstand zwischen diesen beiden Protokollblöcken kann dann abbilden, wie viele Transaktionsprotokollblöcke noch nicht wiederholt wurden. Die Messung erfolgt in Kilobyte (KB).
-  **redo_rate** (KB/Sek.) [*für RTO verwendet*]: Ein kumulierter Wert, der für einen abgelaufenen Zeitraum angibt, wie viel des Transaktionsprotokolls (KB) in der sekundären Datenbank in Kilobytes(KB)/Sekunde wiederholt wurde. 
- **last_commit_time** (Datetime) [*für RPO verwendet*]: Für die primäre Datenbank ist **last_commit_time** der Zeitpunkt, zu der für die letzte Transaktion ein Commit erfolgt ist. Für die sekundäre Datenbank ist **last_commit_time** die letzte Commitzeit für die Transaktion in der primären Datenbank, die auch in der sekundären Datenbank erfolgreich festgeschrieben wurde. Da dieser Wert in der sekundären Datenbank mit dem gleichen Wert für die primäre synchronisiert werden sollte, ist jede Lücke zwischen diesen beiden Werten die Schätzung des Datenverlusts (RPO).  
 
## <a name="estimate-rto-and-rpo-using-dmvs"></a>Schätzen von RTO und RPO mithilfe dynamischer Verwaltungssichten (DMVs)

Es ist möglich, die DMVs [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) und [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) abzufragen, um das RPO und RTO einer Datenbank zu schätzen. Die folgenden Abfragen erstellen gespeicherte Prozeduren, die beide Aufgaben bewältigen. 

  >[!NOTE]
  > Stellen Sie sicher, dass Sie die gespeicherte Prozedur erstellen und ausführen, um das RTO zuerst zu schätzen, da die von ihr erzeugten Werte notwendig sind, um die gespeicherte Prozedur zur Schätzung des RPO auszuführen. 

### <a name="create-a-stored-procedure-to-estimate-rto"></a>Erstellen einer gespeicherten Prozedur zum Schätzen des RTO 

1. Erstellen Sie für das sekundäre Zielreplikat die gespeicherte Prozedur **proc_calculate_RTO**. Wenn diese gespeicherte Prozedur bereits vorhanden ist, löschen Sie sie zuerst, und erstellen Sie sie dann neu. 

 ```sql
    if object_id(N'proc_calculate_RTO', 'p') is not null
        drop procedure proc_calculate_RTO
    go
    
    raiserror('creating procedure proc_calculate_RTO', 0,1) with nowait
    go
    --
    -- name: proc_calculate_RTO
    --
    -- description: Calculate RTO of a secondary database.
    -- 
    -- parameters:  @secondary_database_name nvarchar(max): name of the secondary database.
    --
    -- security: this is a public interface object.
    --
    create procedure proc_calculate_RTO
    (
    @secondary_database_name nvarchar(max)
    )
    as
    begin
      declare @db sysname
      declare @is_primary_replica bit 
      declare @is_failover_ready bit 
      declare @redo_queue_size bigint 
      declare @redo_rate bigint
      declare @replica_id uniqueidentifier
      declare @group_database_id uniqueidentifier
      declare @group_id uniqueidentifier
      declare @RTO float 

      select 
      @is_primary_replica = dbr.is_primary_replica, 
      @is_failover_ready = dbcs.is_failover_ready, 
      @redo_queue_size = dbr.redo_queue_size, 
      @redo_rate = dbr.redo_rate, 
      @replica_id = dbr.replica_id,
      @group_database_id = dbr.group_database_id,
      @group_id = dbr.group_id 
      from sys.dm_hadr_database_replica_states dbr join sys.dm_hadr_database_replica_cluster_states dbcs    on dbr.replica_id = dbcs.replica_id and 
      dbr.group_database_id = dbcs.group_database_id  where dbcs.database_name = @secondary_database_name

      if  @is_primary_replica is null or @is_failover_ready is null or @redo_queue_size is null or @replica_id is null or @group_database_id is null or @group_id is null
      begin
        print 'RTO of Database '+ @secondary_database_name +' is not available'
        return
      end
      else if @is_primary_replica = 1
      begin
        print 'You are visiting wrong replica';
        return
      end

      if @redo_queue_size = 0 
        set @RTO = 0 
      else if @redo_rate is null or @redo_rate = 0 
      begin
        print 'RTO of Database '+ @secondary_database_name +' is not available'
        return
      end
      else 
        set @RTO = CAST(@redo_queue_size AS float) / @redo_rate
    
      print 'RTO of Database '+ @secondary_database_name +' is ' + convert(varchar, ceiling(@RTO))
      print 'group_id of Database '+ @secondary_database_name +' is ' + convert(nvarchar(50), @group_id)
      print 'replica_id of Database '+ @secondary_database_name +' is ' + convert(nvarchar(50), @replica_id)
      print 'group_database_id of Database '+ @secondary_database_name +' is ' + convert(nvarchar(50), @group_database_id)
    end
 ```

2. Führen Sie **proc_calculate_RTO** mit dem Namen der sekundären Zieldatenbank aus:
  ```sql
   exec proc_calculate_RTO @secondary_database_name = N'DB_sec'
  ```
3. Die Ausgabe zeigt den RTO-Wert der Zieldatenbank des sekundären Replikats. Speichern Sie *group_id*, *replica_id* und *group_database_id*, um diese Angaben mit der gespeicherten Prozedur zur Schätzung des RPO zu verwenden. 
   
   Beispielausgabe:
<br>RTO von Database DB_sec ist 0
<br>group_id von Datenbank DB4 ist F176DD65-C3EE-4240-BA23-EA615F965C9B
<br>replica_id von Datenbank DB4 ist 405554F6-3FDC-4593-A650-2067F5FABFFD
<br>group_database_id von Datenbank DB4 ist 39F7942F-7B5E-42C5-977D-02E7FFA6C392

### <a name="create-a-stored-procedure-to-estimate-rpo"></a>Erstellen einer gespeicherten Prozedur zum Schätzen des RPO 
1. Erstellen Sie für das primäre Replikat die gespeicherte Prozedur **proc_calculate_RPO**. Wenn sie bereits vorhanden ist, löschen Sie sie zuerst, und erstellen Sie sie dann neu. 

 ```sql
    if object_id(N'proc_calculate_RPO', 'p') is not null
                    drop procedure proc_calculate_RPO
    go
    
    raiserror('creating procedure proc_calculate_RPO', 0,1) with nowait
    go
    --
    -- name: proc_calculate_RPO
    --
    -- description: Calculate RPO of a secondary database.
    -- 
    -- parameters:  @group_id uniqueidentifier: group_id of the secondary database.
    --              @replica_id uniqueidentifier: replica_id of the secondary database.
    --              @group_database_id uniqueidentifier: group_database_id of the secondary database.
    --
    -- security: this is a public interface object.
    --
    create procedure proc_calculate_RPO
    (
     @group_id uniqueidentifier,
     @replica_id uniqueidentifier,
     @group_database_id uniqueidentifier
    )
    as
    begin
          declare @db_name sysname
          declare @is_primary_replica bit
          declare @is_failover_ready bit
          declare @is_local bit
          declare @last_commit_time_sec datetime 
          declare @last_commit_time_pri datetime      
          declare @RPO nvarchar(max) 

          -- secondary database's last_commit_time 
          select 
          @db_name = dbcs.database_name,
          @is_failover_ready = dbcs.is_failover_ready, 
          @last_commit_time_sec = dbr.last_commit_time 
          from sys.dm_hadr_database_replica_states dbr join sys.dm_hadr_database_replica_cluster_states dbcs on dbr.replica_id = dbcs.replica_id and 
          dbr.group_database_id = dbcs.group_database_id  where dbr.group_id = @group_id and dbr.replica_id = @replica_id and dbr.group_database_id = @group_database_id

          -- correlated primary database's last_commit_time 
          select
          @last_commit_time_pri = dbr.last_commit_time,
          @is_local = dbr.is_local
          from sys.dm_hadr_database_replica_states dbr join sys.dm_hadr_database_replica_cluster_states dbcs on dbr.replica_id = dbcs.replica_id and 
          dbr.group_database_id = dbcs.group_database_id  where dbr.group_id = @group_id and dbr.is_primary_replica = 1 and dbr.group_database_id = @group_database_id

          if @is_local is null or @is_failover_ready is null
          begin
            print 'RPO of database '+ @db_name +' is not available'
            return
          end

          if @is_local = 0
          begin
            print 'You are visiting wrong replica'
            return
          end  

          if @is_failover_ready = 1
            set @RPO = '00:00:00'
          else if @last_commit_time_sec is null or  @last_commit_time_pri is null 
          begin
            print 'RPO of database '+ @db_name +' is not available'
            return
          end
          else
          begin
            if DATEDIFF(ss, @last_commit_time_sec, @last_commit_time_pri) < 0
            begin
                print 'RPO of database '+ @db_name +' is not available'
                return
            end
            else
                set @RPO =  CONVERT(varchar, DATEADD(ms, datediff(ss ,@last_commit_time_sec, @last_commit_time_pri) * 1000, 0), 114)
          end
          print 'RPO of database '+ @db_name +' is ' + @RPO
      end
 ```

2. Führen Sie **proc_calculate_RPO** mit *group_id*, *replica_id* und *group_database_id* der sekundären Zieldatenbank aus. 

 ```sql
   exec proc_calculate_RPO @group_id= 'F176DD65-C3EE-4240-BA23-EA615F965C9B',
        @replica_id =  '405554F6-3FDC-4593-A650-2067F5FABFFD',
        @group_database_id  = '39F7942F-7B5E-42C5-977D-02E7FFA6C392'
 ```
3. Die Ausgabe zeigt den RPO-Wert der Zieldatenbank des sekundären Replikats. 

  
##  <a name="monitoring-for-rto-and-rpo"></a>Überwachen von RTO und RPO  
 In diesem Abschnitt wird das Überwachen von Verfügbarkeitsgruppen für die Metriken RTO und RPO veranschaulicht. Diese Demonstration ähnelt dem GUI-Tutorial unter [The Always On health model, part 2: Extending the health model (Always On-Integritätsmodell, Teil 2: Erweitern des Integritätsmodells)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model).  
  
 Elemente der Failoverzeit und Berechnungen des möglichen Datenverlusts unter [Einschätzen der Failoverzeit (RTO)](#estimating-failover-time-rto) und [Einschätzen des möglichen Datenverlusts (RPO)](#estimating-potential-data-loss-rpo) werden praktischerweise als Leistungsmetriken in der Richtlinienverwaltungsfacets **Datenbankreplikatszustand** bereitgestellt (siehe [Anzeigen der Facets der richtlinienbasierten Verwaltung für ein SQL Server-Objekt](~/relational-databases/policy-based-management/view-the-policy-based-management-facets-on-a-sql-server-object.md)). Sie können diese beiden Metriken nach einem Zeitplan überwachen und werden benachrichtigt, wenn die Metriken Ihre RTO bzw. RPO überschreiten.  
  
 Die angezeigten Skripts erstellen zwei Systemrichtlinien mit den folgenden Merkmalen, die basierend auf ihren jeweiligen Zeitplänen ausgeführt werden:  
  
-   Eine RTO-Richtlinie mit einer Auswertung im 5-Minuten-Takt, bei der ein Fehler auftritt, wenn die geschätzte Failoverzeit 10 Minuten überschreitet  
  
-   Eine RPO-Richtlinie mit einer Auswertung im 30-Minuten-Takt, bei der ein Fehler auftritt, wenn die geschätzte Failoverzeit 1 Stunde überschreitet  
  
-   Beide Richtlinien weisen die gleich Konfiguration für alle Verfügbarkeitsreplikate auf.  
  
-   Auf allen Servern werden Richtlinien ausgewertet, jedoch nur für die Verfügbarkeitsgruppen, bei denen das lokale Verfügbarkeitsreplikat das primäre Replikat darstellt. Wenn das lokale Verfügbarkeitsreplikat nicht das primäre Replikat darstellt, werden die Richtlinien nicht ausgewertet.  
  
-   Richtlinienfehler werden praktischerweise auf dem Always On-Dashboard angezeigt, wenn Sie diese für das primäre Replikat anzeigen.  

Um die Richtlinien zu erstellen, befolgen Sie die nachfolgenden Anweisungen für alle Serverinstanzen, die in der Verfügbarkeitsgruppe enthalten sind:  

1.  [Starten Sie den SQL Server-Agent-Dienst](~/ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md), sofern dieser noch nicht gestartet wurde.  
  
2.  Klicken Sie in SQL Server Management Studio im Menü **Tools** auf **Optionen**.  
  
3.  Wählen Sie auf der Registerkarte **SQL Server Always On** die Option **Benutzerdefinierte Always On-Richtlinie aktivieren** aus, und klicken Sie auf **OK**.  
  
     Durch diese Einstellung können Sie ordnungsgemäß konfigurierte benutzerdefinierte Richtlinien auf dem Always On-Dashboard anzeigen.  
  
4.  Erstellen Sie eine [richtlinienbasierte Verwaltungsbedingung](~/relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) mit den folgenden Spezifikationen:  
  
    -   **Name**: `RTO`  
  
    -   **Facet:** **Database Replica State** (Zustand des Datenbankreplikats)  
  
    -   **Feld**: `Add(@EstimatedRecoveryTime, 60)`  
  
    -   **Operator**: **<=**  
  
    -   **Wert**: `600`  
  
     Bei dieser Bedingung tritt ein Fehler auf, wenn die mögliche Failoverzeit 10 Minuten überschreitet, einschließlich eines Mehraufwands von 60 Sekunden für die Fehlererkennung und das Failover.  
  
5.  Erstellen Sie eine zweite [richtlinienbasierte Verwaltungsbedingung](~/relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) mit den folgenden Spezifikationen:  
  
    -   **Name**: `RPO`  
  
    -   **Facet:** **Database Replica State** (Zustand des Datenbankreplikats)  
  
    -   **Feld**: `@EstimatedDataLoss`  
  
    -   **Operator**: **<=**  
  
    -   **Wert**: `3600`  
  
     Bei dieser Bedingung tritt ein Fehler auf, wenn der Datenverlust 1 Stunde überschreitet.  
  
6.  Erstellen Sie eine dritte [richtlinienbasierte Verwaltungsbedingung](~/relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) mit den folgenden Spezifikationen:  
  
    -   **Name**: `IsPrimaryReplica`  
  
    -   **Facet:** **Verfügbarkeitsgruppe**  
  
    -   **Feld**: `@LocalReplicaRole`  
  
    -   **Operator**: **=**  
  
    -   **Wert**: `Primary`  
  
     Diese Bedingung überprüft, ob das lokale Verfügbarkeitsreplikat für eine bestimmte Verfügbarkeitsgruppe das primäre Replikat ist.  
  
7.  Erstellen Sie eine [richtlinienbasierte Verwaltungsrichtlinie](~/relational-databases/policy-based-management/create-a-policy-based-management-policy.md) mit den folgenden Spezifikationen:  
  
    -   Seite **Allgemein**:  
  
        -   **Name**: `CustomSecondaryDatabaseRTO`  
  
        -   **Bedingung überprüfen**: `RTO`  
  
        -   **Für Ziele:** **Alle DatabaseReplicaState** in **IsPrimaryReplica AvailabilityGroup**  
  
             Durch diese Einstellung wird sichergestellt, dass die Richtlinie nur für Verfügbarkeitsgruppen ausgewertet wird, bei denen das lokale Verfügbarkeitsreplikat das primäre Replikat darstellt.  
  
        -   **Auswertungsmodus:** **Nach Zeitplan**  
  
        -   **Zeitplan:** **CollectorSchedule_Every_5min**  
  
        -   **Aktiviert**: **Ausgewählt**  
  
    -   Seite **Beschreibung**:  
  
        -   **Kategorie:** **Availability database warnings** (Warnungen zu Verfügbarkeitsdatenbanken)  
  
             Mit dieser Einstellung können die Ergebnisse der Richtlinienauswertung auf dem Always On-Dashboard angezeigt werden.  
  
        -   **Beschreibung**: **Das aktuelle Replikat ist eine RTO, die 10 Minuten überschreitet. Hierbei wird von einem Mehraufwand von 1 Minute für die Erkennung und das Failover ausgegangen. Sie sollten Leistungsprobleme in der jeweiligen Serverinstanz sofort untersuchen.**  
  
        -   **Anzuzeigender Text:** **RTO wurde überschritten.**  
  
8.  Erstellen Sie eine zweite [richtlinienbasierte Verwaltungsrichtlinie](~/relational-databases/policy-based-management/create-a-policy-based-management-policy.md) mit den folgenden Spezifikationen:  
  
    -   Seite **Allgemein**:  
  
        -   **Name**: `CustomAvailabilityDatabaseRPO`  
  
        -   **Bedingung überprüfen**: `RPO`  
  
        -   **Für Ziele:** **Alle DatabaseReplicaState** in **IsPrimaryReplica AvailabilityGroup**  
  
        -   **Auswertungsmodus:** **Nach Zeitplan**  
  
        -   **Zeitplan:** **CollectorSchedule_Every_30min**  
  
        -   **Aktiviert**: **Ausgewählt**  
  
    -   Seite **Beschreibung**:  
  
        -   **Kategorie:** **Availability database warnings** (Warnungen zu Verfügbarkeitsdatenbanken)  
  
        -   **Beschreibung**: **Die Verfügbarkeitsdatenbank hat Ihre RPO von einer Stunde überschritten. Sie sollten Leistungsprobleme in den Verfügbarkeitsreplikaten sofort untersuchen.**  
  
        -   **Anzuzeigender Text:** **RPO wurde überschritten.**  
  
 Wenn Sie fertig sind, werden zwei neue SQL Server-Agent-Aufträge erstellt, jeweils einer für den Richtlinienauswertungszeitplan. Diese Aufträge sollten mit Namen versehen werden, die mit **syspolicy_check_schedule** beginnen.  
  
 Sie können zur Überprüfen der Auswertungsergebnisse den Auftragsverlauf einsehen. Fehler bei der Auswertung werden auch im Windows-Anwendungsprotokoll (in der Ereignisanzeige) mit der Ereignis-ID 34052 erfasst. Sie können auch den SQL Server-Agent für das Senden von Warnungen für Richtlinienfehler konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von Warnungen zur Benachrichtigung von Richtlinienadministratoren bei Richtlinienfehlern](~/relational-databases/policy-based-management/configure-alerts-to-notify-policy-administrators-of-policy-failures.md).  
  
##  <a name="performance-troubleshooting-scenarios"></a><a name="BKMK_SCENARIOS"></a> Leistungsbezogene Problembehandlungsszenarien  
 Die folgende Tabelle enthält die allgemeinen leistungsbezogenen Problembehandlungsszenarien.  
  
|Szenario|BESCHREIBUNG|  
|--------------|-----------------|  
|[Problembehandlung: Verfügbarkeitsgruppe hat RTO überschritten](troubleshoot-availability-group-exceeded-rto.md)|Nach einem automatischen Failover oder einem geplanten manuellen Failover ohne Datenverlust überschreitet die Failoverzeit die RTO. Ein anderer Fall: Wenn Sie die Failoverzeit eines sekundären Replikats im synchronen Commitmodus (z.B. eines Partners für das automatische Failover) einschätzen, stellen Sie fest, dass diese Ihre RTO überschreitet.|  
|[Problembehandlung: Verfügbarkeitsgruppe hat RPO überschritten](troubleshoot-availability-group-exceeded-rpo.md)|Nachdem Sie ein erzwungenes manuelles Failover ausgeführt haben, ist der Datenverlust größer als Ihre RPO. Ein anderer Fall: Wenn Sie den möglichen Datenverlust eines sekundäres Replikats im asynchronen Commitmodus berechnen, stellen Sie fest, dass dieser Ihre RPO überschreitet.|  
|[Problembehandlung: Änderungen am primären Replikat spiegeln sich nicht im sekundären Replikat wider](troubleshoot-primary-changes-not-reflected-on-secondary.md)|Die Clientanwendung führt erfolgreich ein Update für das primäre Replikat durch, wobei jedoch die Abfrage des sekundären Replikats ergibt, dass die Änderung nicht widergespiegelt wird.|  
  
##  <a name="useful-extended-events"></a><a name="BKMK_XEVENTS"></a> Nützliche erweiterte Ereignisse  
 Für die Behandlung von Problemen mit Replikaten im Zustand **Wird synchronisiert** sind folgende erweiterte Ereignisse nützlich.  
  
|Veranstaltungsname|Category|Channel|Verfügbarkeitsreplikat|  
|----------------|--------------|-------------|--------------------------|  
|redo_caught_up|Transaktionen|Debuggen|Secondary|  
|redo_worker_entry|Transaktionen|Debuggen|Secondary|  
|hadr_transport_dump_message|`alwayson`|Debuggen|Primär|  
|hadr_worker_pool_task|`alwayson`|Debuggen|Primär|  
|hadr_dump_primary_progress|`alwayson`|Debuggen|Primär|  
|hadr_dump_log_progress|`alwayson`|Debuggen|Primär|  
|hadr_undo_of_redo_log_scan|`alwayson`|Analytic|Secondary|