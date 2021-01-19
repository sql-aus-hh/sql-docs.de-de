---
title: Sichern und Wiederherstellen von SQL Server-Datenbanken | Microsoft-Dokumentation
description: In diesem Artikel werden die Vorteile der Sicherung von SQL Server-Datenbanken erläutert. Darüber hinaus bietet er eine Einführung in Sicherungs- und Wiederherstellungsstrategien sowie Sicherheitsüberlegungen.
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], see restoring [SQL Server]
- backups [SQL Server]
- restoring databases [SQL Server]
- backup [SQL Server], see backing up [SQL Server]
- databases [SQL Server], restoring
- backing up databases [SQL Server]
- restore [SQL Server], see restoring [SQL Server]
- disaster recovery [SQL Server], see backing up [SQL Server]
- backing up [SQL Server]
- Database Engine [SQL Server], backups
- databases [SQL Server], backups
ms.assetid: 570a21b3-ad29-44a9-aa70-deb2fbd34f27
author: cawrites
ms.author: chadam
ms.openlocfilehash: 76aa3e890169e73d30d9a15aa57aa4fd832a7ef9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171312"
---
# <a name="back-up-and-restore-of-sql-server-databases"></a>Sichern und Wiederherstellen von SQL Server-Datenbanken
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Dieser Artikel erläutert die Vorteile der Sicherung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken und grundlegende Begriffe zu Sicherung und Wiederherstellung. Darüber hinaus bietet der Artikel eine Einführung in Sicherungs- und Wiederherstellungsstrategien für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sowie Sicherheitsüberlegungen zu Sicherungen und Wiederherstellungen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

> In diesem Artikel werden SQL Server-Sicherungen vorgestellt. Die spezifischen Schritte zum Sichern von SQL Server-Datenbanken finden Sie unter [Erstellen von Sicherungen](#creating-backups).
  
 Durch die Sicherungs- und Wiederherstellungskomponente von SQL Server wird eine wichtige Vorrichtung zum Schutz wichtiger Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken bereitgestellt. Um die Gefahr eines schwerwiegenden Datenverlusts zu minimieren, müssen Sie Ihre Datenbanken regelmäßig sichern, um die Änderungen an Ihren Daten aufzubewahren. Eine gut geplante Sicherungs- und Wiederherstellungsstrategie hilft, Datenbanken vor Datenverlusten zu schützen, die durch eine Vielzahl von Fehlern verursacht werden können. Testen Sie Ihre Strategie, indem Sie einen Sicherungssatz und anschließend Ihre Datenbank wiederherstellen, um sich auf die effektive Reaktion auf einen Notfall vorzubereiten.
  
 Neben dem lokalen Speicher für das Speichern der Sicherung unterstützt SQL Server auch das Sichern und Wiederherstellen mit dem Azure Blob Storage-Dienst. Weitere Informationen finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). Für Datenbankdateien, die mit dem Microsoft Azure Blob-Speicherdienst gespeichert wurden, bietet [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] die Option, Azure-Momentaufnahmen für nahezu sofortige Sicherungen und schnellere Wiederherstellungen zu nutzen. Weitere Informationen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md). Azure bietet auch eine Sicherungslösung der Unternehmensklasse für SQL Server-Instanzen auf Azure-VMs. Es handelt sich um eine vollständig verwaltete Sicherungslösung, die Always On-Verfügbarkeitsgruppen, Langzeitaufbewahrung Zeitpunktwiederherstellung sowie zentrale Verwaltung und Überwachung unterstützt. Weitere Informationen finden Sie unter [Informationen zur SQL Server-Sicherung auf virtuellen Azure-Computern](/azure/backup/backup-azure-sql-database).
  
##  <a name="why-back-up"></a>Warum Sichern (back up)?  
-   Sie können sich vor schwerwiegendem Datenverlust schützen, indem Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken sichern, Testwiederherstellungsprozeduren für die Sicherungen ausführen und Kopien der Sicherungen an einem sicheren Ort außerhalb Ihrer Geschäftsräume aufbewahren. **Das Sichern ist die einzige Möglichkeit, Ihre Daten zu schützen.**

     Mithilfe gültiger Datenbanksicherungen können Sie die Daten nach vielen Fehlern wiederherstellen, z. B.:  
  
    -   Medienfehler    
    -   Benutzerfehler (z. B. versehentliches Löschen einer Tabelle)    
    -   Hardwarefehler (z. B. ein beschädigter Datenträger oder der endgültige Verlust eines Servers)    
    -   Naturkatastrophen. Mit der SQL Server-Sicherung im Azure Blob Storage-Dienst können Sie eine externe Sicherung in einer anderen Region als an Ihrem lokalen Standort erstellen. Diese wird dann verwendet, wenn der lokale Standort durch eine Naturkatastrophe in Mitleidenschaft gezogen wird.  
  
-   Darüber hinaus sind Sicherungen einer Datenbank hilfreich für Routineverwaltungsaufgaben, wie z. B. Kopieren einer Datenbank zwischen Servern, Einrichten von [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] oder Datenbankspiegelung und Archivierung.  
  
##  <a name="glossary-of-backup-terms"></a>Glossarbegriffe für die Sicherung
 **Sichern** [Verb]  
 Der Prozess zum Erstellen einer **Sicherung [Substantiv]** durch Kopieren von Datensätzen aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank oder Protokolldatensätzen aus dem jeweiligen Transaktionsprotokoll.  
  
 **Sicherung** [Substantiv]  
 Eine Datenkopie, die zum Wiederherstellen der Daten nach einem Fehler verwendet werden kann. Sicherungen einer Datenbank können auch verwendet werden, um eine Kopie der Datenbank an einem neuen Speicherort wiederherzustellen.  
  
**Sicherungsgerät**  
 Ein Datenträger oder Bandgerät, auf den bzw. das SQL Server-Sicherungen geschrieben werden und von dem sie wiederhergestellt werden können. SQL Server-Sicherungen können auch in einen Azure Blob Storage-Dienst geschrieben werden. Das **URL**-Format wird verwendet, um das Ziel und den Namen der Sicherungsdatei anzugeben. Weitere Informationen finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
**Sicherungsmedien**  
 Bänder oder Datenträgerdateien, auf die Sicherungen geschrieben wurden  
  
**Datensicherung**  
 Eine Sicherung von Daten einer vollständigen Datenbank (Datenbanksicherung), einer partiellen Datenbank (partielle Sicherung) oder einem Satz von Datendateien oder Dateigruppen (Dateisicherung).  
  
**Datenbanksicherung**  
 Eine Sicherung einer Datenbank. Vollständige Datenbanksicherungen stellen die gesamte Datenbank zum Zeitpunkt dar, an dem die Sicherung abgeschlossen wurde. Differenzielle Datenbanksicherungen enthalten nur Änderungen, die seit der letzten vollständigen Datenbanksicherung an der Datenbank vorgenommen wurden.  
  
**Differenzielle Sicherung**  
 Eine Datensicherung, die auf der letzten vollständigen Sicherung einer vollständigen oder partiellen Datenbank oder einem Satz von Datendateien oder Dateigruppen (differenzielle Basis) basiert und nur die Daten enthält, die sich gegenüber der Basis geändert haben.  
  
**Vollständige Sicherung**  
 Eine Datensicherung, die alle Daten in einer bestimmten Datenbank oder einem Satz von Dateigruppen oder Dateien enthält. Außerdem muss die Sicherung genügend Protokolle enthalten, um die Wiederherstellung dieser Daten zu ermöglichen.  
  
**Protokollsicherung**  
 Eine Sicherung von Transaktionsprotokollen, die alle Protokolldatensätze enthält, die nicht in einer vorherigen Protokollsicherung gesichert wurden. (Vollständiges Wiederherstellungsmodell)  
  
**recover**  
 Wiederherstellen eines stabilen und konsistenten Datenbankzustands.  
  
**recovery**  
 Eine Phase beim Starten der Datenbank oder einer Wiederherstellung, in der ein transaktionskonsistenter Zustand der Datenbank hergestellt wird.  
  
**Wiederherstellungsmodell**  
 Eine Datenbankeigenschaft, die die Pflege der Transaktionsprotokolle auf einer Datenbank steuert. Es stehen drei Wiederherstellungsmodelle zur Verfügung: einfach, vollständig und massenprotokolliert. Das Wiederherstellungsmodell der Datenbank bestimmt die Sicherungs- und Wiederherstellungsanforderungen.  
  
**restore**  
 Ein aus mehreren Phasen bestehender Prozess, in dem alle Daten und Protokollseiten aus einer angegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung in eine angegebene Datenbank kopiert werden und ein Rollforward für alle Transaktionen ausgeführt wird, die in der Sicherung protokolliert sind. Dies wird erreicht, indem die Daten durch die Übernahme protokollierter Änderungen aktualisiert werden.  
  
 ##  <a name="backup-and-restore-strategies"></a>Sicherungs- und Wiederherstellungsstrategien  
 Das Sichern und Wiederherstellen von Daten muss jedoch auf die entsprechende Umgebung und auf die verfügbaren Ressourcen abgestimmt werden. Die zuverlässige Verwendung von Sicherungen und Wiederherstellungen erfordert daher eine Sicherungs- und Wiederherstellungsstrategie. Eine gut entworfene Sicherungs- und Wiederherstellungsstrategie findet einen Mittelweg zwischen maximaler Datenverfügbarkeit und minimalem Datenverlust und berücksichtigt gleichzeitig die Kosten für das Verwalten und Speichern von Sicherungen.  

 Eine Sicherungs- und Wiederherstellungsstrategie enthält einen Sicherungsteil und einen Wiederherstellungsteil. Der Sicherungsteil der Strategie definiert den Typ und die Häufigkeit der Sicherungen, die Art und die Geschwindigkeit der dafür benötigten Hardware, die vorgesehene Testmethode für die Sicherungen sowie Speicherort und -methode von Sicherungsmedien (einschließlich Sicherheitsüberlegungen). Im Wiederherstellungsteil der Strategie wird definiert, wer für die Ausführung von Wiederherstellungen verantwortlich ist, wie Wiederherstellungen ausgeführt werden sollen, um die jeweiligen Ziele hinsichtlich der Verfügbarkeit der Datenbank und der Minimierung von Datenverlusten zu erreichen, sowie die Art und Weise, wie Wiederherstellungen getestet werden. 
  
 Das Entwerfen einer effektiven Sicherungs- und Wiederherstellungsstrategie erfordert sorgfältiges Planen, Implementieren und Testen. Tests sind absolut notwendig: Sie verfügen erst dann über eine Sicherungsstrategie, wenn Sie die Sicherungen Ihrer Wiederherstellungsstrategie in allen Kombinationen erfolgreich wiederhergestellt und die physische Konsistenz der wiederhergestellten Datenbank getestet haben. Sie müssen eine Vielzahl von Faktoren berücksichtigen: Dazu gehören:  
  
- Die Ziele des Unternehmens hinsichtlich der Produktionsdatenbanken, besonders die Anforderungen an Verfügbarkeit und Schutz vor Datenverlust oder -beschädigung.  
  
-  Die Merkmale der einzelnen Datenbanken, wie etwa Größe, Verwendungsmuster, Art des Inhalts und Anforderungen hinsichtlich der Daten.  
  
-   Einschränkungen hinsichtlich der Ressourcen, wie z. B. Hardware, Mitarbeiter, Platz zum Aufbewahren von Sicherungsmedien und physische Sicherheit der aufbewahrten Medien.  

## <a name="best-practice-recommendations"></a>Empfehlungen zu bewährten Methoden

### <a name="use-separate-storage"></a>Verwenden von getrennten Speichern 
> [!IMPORTANT] 
> Speichern Sie Ihre Datenbanksicherungen immer an einem anderen physischen Speicherort oder auf einem anderen Gerät als die Datenbankdateien. Funktioniert das physische Laufwerk, auf dem die Datenbanken gespeichert sind, nicht richtig oder stürzt ab, kann eine Wiederherstellung nur ausgeführt werden, wenn auf das separate Laufwerk oder Remotegerät, auf dem die Sicherungen gespeichert wurden, auch zugegriffen werden kann. Denken Sie daran, dass Sie auf einem physischen Datenträger mehrere logische Volumes oder Partitionen erstellen können. Überprüfen Sie das Layout der Datenträgerpartition und der logischen Volumes sorgfältig, bevor Sie einen Speicherort für die Sicherungen auswählen.

### <a name="choose-appropriate-recovery-model"></a>Auswählen eines geeigneten Wiederherstellungsmodells
 Sicherungs- und Wiederherstellungsvorgänge werden im Kontext eines [Wiederherstellungsmodells](../backup-restore/recovery-models-sql-server.md) durchgeführt. Bei einem Wiederherstellungsmodell handelt es sich um eine Datenbankeigenschaft, mit der die Verwaltung des Transaktionsprotokolls gesteuert wird. Das Wiederherstellungsmodell für eine Datenbank bestimmt also, welche Arten von Sicherungs- und Wiederherstellungsszenarios für die Datenbank unterstützt werden und wie groß die Sicherungen der Transaktionsprotokolle sein sollen. Meistens wird für Datenbanken entweder das einfache Wiederherstellungsmodell oder das vollständige Wiederherstellungsmodell verwendet. Das vollständige Wiederherstellungsmodell kann vor dem Ausführen von Massenvorgängen durch das massenprotokollierte Wiederherstellungsmodell erweitert werden. Eine Einführung zu diesen Wiederherstellungsmodellen und deren Auswirkung auf die Verwaltung des Transaktionsprotokolls finden Sie unter [Das Transaktionsprotokoll [SQL Server]](../logs/the-transaction-log-sql-server.md).  
  
 Die Wahl des für Ihre Datenbank am besten geeigneten Wiederherstellungsmodells hängt von Ihren Geschäftsanforderungen ab. Verwenden Sie das einfache Wiederherstellungsmodell, wenn Sie die Verwaltung des Transaktionsprotokolls vermeiden und den Sicherungs- und Wiederherstellungsprozess so einfach wie möglich gestalten möchten. Wenn das Risiko eines Arbeitsdatenverlusts so klein wie möglich sein soll und Sie bereit sind, dafür einen höheren Verwaltungsaufwand in Kauf zu nehmen, verwenden Sie das vollständige Wiederherstellungsmodell. Verwenden Sie das massenprotokollierte Wiederherstellungsmodell, um die Auswirkungen auf die Protokollgröße bei massenprotokollierten Vorgängen zu minimieren und gleichzeitig die Wiederherstellbarkeit dieser Vorgänge zu ermöglichen. Informationen zur Auswirkung von Wiederherstellungsmodellen auf Sicherungs- und Wiederherstellungsvorgänge finden Sie unter [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
### <a name="design-your-backup-strategy"></a>Entwerfen Ihrer Sicherungsstrategie  
 Nachdem Sie ein Wiederherstellungsmodell ausgewählt haben, das Ihre Geschäftsanforderungen für eine bestimmte Datenbank erfüllt, müssen Sie eine entsprechende Sicherungsstrategie planen und implementieren. Die optimale Sicherungsstrategie hängt von verschiedenen Faktoren ab, wobei den folgenden eine besondere Bedeutung zukommt:  
  
-   Wie viele Stunden täglich müssen Anwendungen auf die Datenbank zugreifen können?  
  
     Wenn es vorhersagbare Zeiten mit wenig Datenverkehr gibt, sollten Sie vollständige Datenbanksicherungen für diesen Zeitraum planen.  
  
-   Wie häufig werden Änderungen und Updates im Allgemeinen vorgenommen?  
  
     Berücksichtigen Sie bei häufigen Änderungen Folgendes:  
  
    -   Bei Verwendung des einfachen Wiederherstellungsmodells sollten Sie zwischen vollständigen Datenbanksicherungen differenzielle Sicherungen planen. Bei einer differenziellen Sicherung werden nur die Änderungen seit der letzten vollständigen Datenbanksicherung erfasst.  
  
    -   Bei Verwendung des vollständigen Wiederherstellungsmodells sollten Sie häufige Protokollsicherungen planen. Wenn Sie zwischen vollständigen Sicherungen differenzielle Sicherungen planen, kann dies die Wiederherstellungszeit verkürzen, da Sie nach dem Wiederherstellen der Daten nur eine geringe Anzahl von Protokollsicherungen wiederherstellen müssen.  
  
-   Werden Änderungen eher in einem kleinen oder in einem großen Bereich der Datenbank vorgenommen?  
  
     Bei einer großen Datenbank, in der sich die Änderungen auf einen Teil der Dateien oder Dateigruppen konzentrieren, können Teilsicherungen und/oder Dateisicherungen sinnvoll sein. Weitere Informationen finden Sie unter [Teilsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/partial-backups-sql-server.md) und [Vollständige Dateisicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md).  
  
-   Wie viel Speicherplatz nimmt eine vollständige Datenbanksicherung auf dem Datenträger in Anspruch?  
-   Wie weit in die Vergangenheit sollen Sicherungen zurückreichen? 

    Vergewissern Sie sich, dass Ihr Sicherungszeitplan den Anwendungs- und Geschäftsanforderungen entspricht. Je älter die Sicherungen sind, desto größer ist das Risiko für einen Datenverlust – außer Sie besitzen die Möglichkeit, alle Daten bis zum Zeitpunkt des Fehlers neu zu generieren. Entscheiden Sie vor dem Löschen von alten Sicherungen aus Speicherkapazitätsgründen, ob eine weit in die Vergangenheit reichende Wiederherstellbarkeit erforderlich ist.

  
 ### <a name="estimate-the-size-of-a-full-database-backup"></a>Schätzen der Größe einer vollständigen Datenbanksicherung  
 Vor dem Implementieren einer Sicherungs- und Wiederherstellungsstrategie sollten Sie schätzen, wie viel Speicherplatz eine vollständige Datenbanksicherung auf dem Datenträger belegen wird. Beim Sicherungsvorgang werden die in der Datenbank enthaltenen Daten in die Sicherungsdatei kopiert. Die Sicherung enthält nur die in der Datenbank vorhandenen Daten, nicht etwa ungenutzten Speicherplatz. Daher ist die Sicherung normalerweise kleiner als die Datenbank selbst. Ein Schätzwert der Größe einer vollständigen Datenbanksicherung kann mithilfe der gespeicherten Systemprozedur **sp_spaceused** ermittelt werden. Weitere Informationen finden Sie unter [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md).  
  
### <a name="schedule-backups"></a>Planen von Sicherungen  
 Da ein Sicherungsvorgang nur minimale Auswirkungen auf ausgeführte Transaktionen hat, können Sicherungsvorgänge auch während der normalen Vorgänge ausgeführt werden. Sie können eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung mit minimalen Auswirkungen auf die produktive Arbeitslast ausführen.  
   
>  Informationen zu Parallelitätseinschränkungen während der Sicherung finden Sie unter [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
 Nachdem Sie entschieden haben, welche Arten von Sicherungen Sie benötigen und wie oft Sie diese jeweils durchführen müssen, empfiehlt es sich, im Rahmen eines Datenbankwartungsplans für die Datenbank die regelmäßige Durchführung dieser Sicherungen zu planen. Informationen zu Wartungsplänen für Datenbank- und Protokollsicherungen und zu deren Erstellung finden Sie unter [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).
  
### <a name="test-your-backups"></a>Testen von Sicherungen  
 Sie verfügen erst dann über eine Wiederherstellungsstrategie, wenn Sie die Sicherungen getestet haben. Es ist entscheidend, dass Sie Ihre Sicherungsstrategie für jede Ihrer Datenbanken gründlich testen, indem Sie eine Kopie der Datenbank auf einem Testsystem wiederherstellen. Sie müssen die Wiederherstellung jedes Sicherungstyps testen, den Sie zu verwenden beabsichtigen. Zudem wird empfohlen, dass Sie nach dem Wiederherstellen der Sicherung zur Überprüfung der Datenbankkonsistenz DBCC CHECKDB für die Datenbank ausführen, um zu überprüfen, ob die Sicherungsmedien beschädigt wurden. 

### <a name="verify-media-stability-and-consistency"></a>Überprüfen der Medienstabilität und -konsistenz
Verwenden Sie hierfür die Überprüfungsoptionen, die von den Sicherungsdienstprogrammen (dem Befehl BACKUP T-SQL, SQL Server-Wartungsplänen, Ihrer Sicherungssoftware oder -lösung usw.) bereitgestellt werden. Ein Beispiel finden Sie unter [RESTORE VERIFYONLY] (../t-sql/statements/restore-statements-verifyonly-transact-sql.md). Verwenden Sie erweiterte Features wie BACKUP CHECKSUM, um Probleme mit dem Sicherungsmedium selbst zu ermitteln. Weitere Informationen finden Sie unter [Mögliche Medienfehler während der Sicherung und Wiederherstellung (SQL Server)](../backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

### <a name="document-backuprestore-strategy"></a>Dokumentieren der Sicherungs- und Wiederherstellungsstrategie 
Es empfiehlt sich, Sicherungs- und Wiederherstellungsprozeduren zu dokumentieren und eine Kopie der Dokumentation im Ausführungsbuch aufzubewahren.
Es wird auch empfohlen, für jede Datenbank ein Betriebshandbuch zu führen. In diesem Betriebshandbuch sollten der Aufbewahrungsort der Sicherungen, ggf. die Namen der Sicherungsmedien sowie Angaben zum Zeitaufwand für die Wiederherstellung der Testsicherungen vermerkt sein.



## <a name="monitor-progress-with-xevent"></a>Überwachen des Fortschritts mit xEvent
Sicherungs- und Wiederherstellungsvorgänge können aufgrund der Größe der Datenbank und der Komplexität der notwendigen Vorgänge viel Zeit in Anspruch nehmen. Wenn bei einer der beiden Vorgänge ein Problem auftritt, können Sie das erweiterte Ereignis **backup_restore_progress_trace** verwenden, um den Fortschritt in Echtzeit zu überwachen. Weitere Informationen zu erweiterten Ereignissen finden Sie unter [Erweiterte Ereignisse](../extended-events/extended-events.md).

  >[!WARNING]
  > Die Verwendung des erweiterten Ereignisses „backup_restore_progress_trace“ kann zu Leistungsproblemen führen und einen beträchtlichen Teil des Speicherplatzes verbrauchen. Verwenden Sie dieses Ereignis mit Bedacht über kurze Zeiträume, und testen Sie es sorgfältig, bevor Sie es in einer Produktionsumgebung implementieren.


```sql
-- Create the backup_restore_progress_trace extended event esssion
CREATE EVENT SESSION [BackupRestoreTrace] ON SERVER 
ADD EVENT sqlserver.backup_restore_progress_trace
ADD TARGET package0.event_file(SET filename=N'BackupRestoreTrace')
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=5 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
GO

-- Start the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = start;  
GO  

-- Stop the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = stop;  
GO  
```

### <a name="sample-output-from-extended-event"></a>Beispielausgabe für erweiterte Ereignisse 

![Beispiel für die Sicherung der Xevent-Ausgabe](media/back-up-and-restore-of-sql-server-databases/backup-xevent-example.png)
![Example of restore xevent output](media/back-up-and-restore-of-sql-server-databases/restore-xevent-example.png)
 
  
## <a name="more-about-backup-tasks"></a>Weitere Informationen zu Sicherungstasks  
-   [Erstellen eines Wartungsplans](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
-   [Erstellen eines Auftrags](../../ssms/agent/create-a-job.md)  
  
-   [Planen eines Auftrags](../../ssms/agent/schedule-a-job.md)  
  
## <a name="working-with-backup-devices-and-backup-media"></a>Arbeiten mit Sicherungsgeräten und Sicherungsmedien  
-   [Definieren eines logischen Sicherungsmediums für eine Datenträgerdatei &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [Definieren eines logischen Sicherungsmediums für ein Bandlaufwerk &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [Angeben eines Datenträgers oder ein Bands als Sicherungsziel &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [Löschen eines Sicherungsmediums &#40;SQL Server&#41;](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [Festlegen des Ablaufdatums für eine Sicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [Anzeigen der Inhalte eines Sicherungsbands oder einer -datei &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [Anzeigen der Daten und Protokolldateien in einem Sicherungssatz &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [Anzeigen der Eigenschaften und des Inhalts eines logischen Sicherungsmediums &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [Wiederherstellung einer Sicherung von einem Medium &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
## <a name="creating-backups"></a>Erstellen von Sicherungen  
**Hinweis!** Verwenden Sie für Teilsicherungen oder Kopiesicherungen die [!INCLUDE[tsql](../../includes/tsql-md.md)][BACKUP](../../t-sql/statements/backup-transact-sql.md) -Anweisung mit der Option PARTIAL bzw. COPY_ONLY.  
  
 ### <a name="using-ssms"></a>Verwenden von SSMS   
-   [Erstellen einer vollständigen Datenbanksicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Sichern eines Transaktionsprotokolls &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Sichern von Dateien und Dateigruppen &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [Erstellen einer differenziellen Datenbanksicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 ### <a name="using-t-sql"></a>Verwenden von T-SQL  
-   [Einschränken der CPU-Nutzung durch die Sicherungskomprimierung mithilfe der Ressourcenkontrolle &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [Sichern des Transaktionsprotokolls bei beschädigter Datenbank &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
-   [Aktivieren oder deaktivieren von Sicherungsprüfsummen während der Sicherung oder Wiederherstellung &#40;SQL Server&#41;](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
-   [Angeben, ob ein Sicherungs- oder Wiederherstellungsvorgang fortgesetzt wird, nachdem ein Fehler festgestellt wurde &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
## <a name="restore-data-backups"></a>Wiederherstellen von Datensicherungen 
### <a name="using-ssms"></a>Verwenden von SSMS 
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Wiederherstellen einer Datenbank an einem neuen Speicherort &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
-   [Wiederherstellen einer differenziellen Datenbanksicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Wiederherstellen von Dateien und Dateigruppen &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
### <a name="using-t-sql"></a>Verwenden von T-SQL
-   [Wiederherstellen einer Datenbanksicherung unter dem einfachen Wiederherstellungsmodell &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)  
  
-   [Wiederherstellen einer Datenbank bis zum Fehlerzeitpunkt im vollständigen Wiederherstellungsmodell &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Wiederherstellen von Dateien und Dateigruppen über vorhandene Dateien &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Wiederherstellen von Dateien an einem neuen Speicherort &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Wiederherstellen der master-Datenbank &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md)  

## <a name="restore-transaction-logs-full-recovery-model"></a>Wiederherstellen von Transaktionsprotokollen (vollständiges Wiederherstellungsmodell)
### <a name="using-ssms"></a>Verwenden von SSMS  
-   [Wiederherstellen einer Datenbank bis zu einer markierten Transaktion &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Wiederherstellen einer Transaktionsprotokollsicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Wiederherstellen einer SQL Server-Datenbank zu einem Zeitpunkt &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
 ### <a name="using-t-sql"></a>Verwenden von T-SQL 
-   [Wiederherstellen einer SQL Server-Datenbank zu einem Zeitpunkt &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
 
-   [Neustarten eines unterbrochenen Wiederherstellungsvorgangs Operation &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
-   [Wiederherstellen einer Datenbank ohne Wiederherstellung von Daten &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)  
 
## <a name="more-information-and-resources"></a>Weitere Informationen und Ressourcen
 [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Übersicht über Wiederherstellungsvorgänge &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Sichern und Wiederherstellen von Analysis Services-Datenbanken](/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)   
 [Sichern und Wiederherstellen von Volltextkatalogen und Indizes](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [Sichern und Wiederherstellen von replizierten Datenbanken](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Wiederherstellungsmodelle &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [Mediensätze, Medienfamilien und Sicherungssätze &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
