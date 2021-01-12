---
description: sysmergepublications (Transact-SQL)
title: sysmergepublications (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepublications
- sysmergepublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepublications system table
ms.assetid: 7f82c6c3-22d1-47c0-a92b-4d64b98cc455
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0b2a7e7783ffb67688041a6da39ea99f945a607f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097340"
---
# <a name="sysmergepublications-transact-sql"></a>sysmergepublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jede in der Datenbank definierte Mergeveröffentlichung. Diese Tabelle wird in der Veröffentlichungs- und in der Abonnementdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Entspricht dem Namen des Standardservers.|  
|**publisher_db**|**sysname**|Der Name der Standardverlegerdatenbank.|  
|**name**|**sysname**|Der Name der Veröffentlichung.|  
|**description**|**nvarchar(255)**|Eine kurze Beschreibung der Veröffentlichung.|  
|**zurück**|**int**|Die Beibehaltungs Dauer für den gesamten Veröffentlichungs Satz, wobei die Einheit durch den Wert der **retention_period_unit** Spalte angegeben wird.|  
|**publication_type**|**tinyint**|Zeigt an, ob die Veröffentlichung gefiltert wird.<br /><br /> **0** = nicht gefiltert.<br /><br /> **1** = gefiltert.|  
|**pubid**|**uniqueidentifier**|Die eindeutige ID für diese Veröffentlichung. Dieser Wert wird beim Hinzufügen der Veröffentlichung generiert.|  
|**designmasterid**|**uniqueidentifier**|Für zukünftige Verwendung reserviert.|  
|**parentid**|**uniqueidentifier**|Zeigt die übergeordnete Veröffentlichung an, aus der die aktuelle gleichgeordnete oder als Teilmenge verwendete Veröffentlichung erstellt wurde (wird für hierarchische Veröffentlichungstopologien verwendet).|  
|**sync_mode**|**tinyint**|Der Synchronisierungsmodus dieser Veröffentlichung:<br /><br /> **0** = System eigen.<br /><br /> **1** = Zeichen.|  
|**allow_push**|**int**|Zeigt an, ob die Veröffentlichung Pushabonnements zulässt.<br /><br /> **0** = Pushabonnements sind nicht zulässig.<br /><br /> **1** = Pushabonnements sind zulässig.|  
|**allow_pull**|**int**|Zeigt an, ob die Veröffentlichung Pullabonnements zulässt.<br /><br /> **0** = Pullabonnements sind nicht zulässig.<br /><br /> **1** = Pullabonnements sind zulässig.|  
|**allow_anonymous**|**int**|Zeigt an, ob die Veröffentlichung anonyme Abonnements zulässt.<br /><br /> **0** = anonyme Abonnements sind nicht zulässig.<br /><br /> **1** = anonyme Abonnements sind zulässig.|  
|**centralized_conflicts**|**int**|Zeigt an, ob die Konfliktdatensätze auf dem Verleger gespeichert werden:<br /><br /> **0** = Konflikt Datensätze werden nicht auf dem Verleger gespeichert.<br /><br /> **1** = Konflikt Datensätze werden auf dem Verleger gespeichert.|  
|**status**|**tinyint**|Für zukünftige Verwendung reserviert.|  
|**snapshot_ready**|**tinyint**|Zeigt den Status für die Momentaufnahme der Veröffentlichung an:<br /><br /> **0** = Momentaufnahme ist nicht einsatzbereit.<br /><br /> **1** = Momentaufnahme kann verwendet werden.<br /><br /> **2** = eine neue Momentaufnahme für diese Veröffentlichung muss erstellt werden.|  
|**enabled_for_internet**|**bit**|Zeigt an, ob die Synchronisierungsdateien für die Veröffentlichung im Internet, über FTP oder andere Dienste bereitgestellt werden.<br /><br /> **0** = auf Synchronisierungs Dateien kann über das Internet zugegriffen werden.<br /><br /> **1** = auf Synchronisierungs Dateien kann nicht über das Internet zugegriffen werden.|  
|**dynamic_filters**|**bit**|Gibt an, ob die Veröffentlichung mithilfe eines parametrisierten Zeilenfilters gefiltert wird.<br /><br /> **0** = die Veröffentlichung ist nicht Zeilen gefiltert.<br /><br /> **1** = die Veröffentlichung ist Zeilen gefiltert.|  
|**snapshot_in_defaultfolder**|**bit**|Gibt an, ob Momentaufnahmedateien im Standardordner gespeichert werden:<br /><br /> **0** = die Momentaufnahme Dateien befinden sich im Standardordner.<br /><br /> **1** = die Momentaufnahme Dateien werden an dem Speicherort gespeichert, der durch **alt_snapshot_folder** angegeben wird.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Der Speicherort des alternativen Ordners für die Momentaufnahme.|  
|**pre_snapshot_script**|**nvarchar(255)**|Zeiger auf. **SQL** -Datei, die der Merge-Agent vor einem der Replikations Objekt Skripts ausführt, wenn die Momentaufnahme auf dem Abonnenten angewendet wird.|  
|**post_snapshot_script**|**nvarchar(255)**|Der Zeiger auf eine. **SQL** -Datei, die der Merge-Agent ausführt, nachdem alle anderen Replikations Objekt Skripts und-Daten während einer ersten Synchronisierung angewendet wurden.|  
|**compress_snapshot**|**bit**|Gibt an, ob die an den **alt_snapshot_folder** Speicherort geschriebene Momentaufnahme in das CAB-Format komprimiert wird [!INCLUDE[msCoName](../../includes/msconame-md.md)] . der Wert **0** gibt an, dass die Datei nicht komprimiert ist.|  
|**ftp_address**|**sysname**|Die Netzwerkadresse des Dateiübertragungsprotokoll (FTP)-Dienstanbieter für den Verteiler. Gibt an, wo die Veröffentlichungs-Momentaufnahmedateien zum Abholen durch den Merge-Agent gespeichert sind, wenn FTP aktiviert ist.|  
|**ftp_port**|**int**|Die Portnummer des FTP-Dienstanbieter für den Verteiler.|  
|**ftp_subdirectory**|**nvarchar(255)**|Das Unterverzeichnis, in dem die Momentaufnahmedateien zum Abholen durch den Merge-Agent verfügbar sind.|  
|**ftp_login**|**sysname**|Der Benutzername, mit dem eine Verbindung mit dem FTP-Dienst hergestellt wird.|  
|**ftp_password**|**nvarchar (524)**|Das Benutzer Kennwort, das für die Verbindung mit dem FTP-Dienst verwendet wird|  
|**conflict_retention**|**int**|Gibt die Aufbewahrungsdauer in Tagen an, für die Konflikte beibehalten werden. Danach wird die Konfliktzeile aus der Konflikttabelle gelöscht.|  
|**keep_before_values**|**int**|Gibt an, ob die Synchronisierungsoptimierung für diese Veröffentlichung erfolgt:<br /><br /> **0** = die Synchronisierung wird nicht optimiert, und die an alle Abonnenten gesendeten Partitionen werden überprüft, wenn sich Daten in einer Partition ändern.<br /><br /> **1** = die Synchronisierung wird optimiert, und es sind nur Abonnenten betroffen, die über Zeilen in der geänderten Partition verfügen.|  
|**allow_subscription_copy**|**bit**|Gibt an, ob die Möglichkeit zum Kopieren der Abonnementdatenbank aktiviert wurde. **0** bedeutet, dass das Kopieren nicht zulässig ist.|  
|**allow_synctoalternate**|**bit**|Gibt an, ob ein alternativer Synchronisierungspartner für die Synchronisierung mit diesem Verleger zulässig ist. der Wert **0** bedeutet, dass ein Synchronisierungs Partner nicht zulässig ist.|  
|**validate_subscriber_info**|**nvarchar (500)**|Listet die Funktionen auf, die zum Abrufen der Abonnenteninformationen sowie zum Überprüfen der parametrisierten Zeilenfilterkriterien für den Abonnenten verwendet werden.|  
|**ad_guidname**|**sysname**|Gibt an, ob die Veröffentlichung in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory veröffentlicht wird. Eine gültige GUID gibt an, dass die Veröffentlichung in der Active Directory veröffentlicht wird, und die GUID ist das entsprechende Active Directory Veröffentlichungs Objekt **objectGUID**. Wenn dieser Wert NULL ist, wird die Veröffentlichung nicht in Active Directory veröffentlicht.|  
|**backward_comp_level**|**int**|Datenbankkompatibilitätsgrad. Folgenden Werte sind möglich:<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .|  
|**max_concurrent_merge**|**int**|Die maximal zulässige Anzahl gleichzeitiger Mergeprozesse. Der Wert **0** für diese Eigenschaft bedeutet, dass es keine Beschränkung für die Anzahl gleichzeitiger Mergeprozesse gibt, die zu einem beliebigen Zeitpunkt ausgeführt werden. Diese Eigenschaft legt eine Grenze für die Anzahl gleichzeitiger Mergeprozesse fest, die zu einem Zeitpunkt für eine Mergeveröffentlichung ausgeführt werden können. Wenn zum gleichen Zeitpunkt mehr Momentaufnahmeprozesse geplant sind, als der Wert für eine Ausführung zulässt, werden die überschüssigen Aufträge in eine Warteschlange eingereiht, in der diese darauf warten, dass ein aktuell ausgeführter Momentaufnahmeprozess beendet wird.|  
|**max_concurrent_dynamic_snapshots**|**int**|Die maximal zulässige Anzahl gleichzeitiger Datenfilterungs-Momentaufnahmesitzungen, die für die Mergeveröffentlichung ausgeführt werden können. Wenn der Wert **0** ist, gibt es keine Beschränkung für die maximale Anzahl gleichzeitiger gefilterter Daten Momentaufnahme Sitzungen, die gleichzeitig für die Veröffentlichung ausgeführt werden können. Diese Eigenschaft legt eine Grenze für die Anzahl gleichzeitiger Momentaufnahmeprozesse fest, die zu einem Zeitpunkt für eine Mergeveröffentlichung ausgeführt werden können. Wenn zum gleichen Zeitpunkt mehr Momentaufnahmeprozesse geplant sind, als der Wert für eine Ausführung zulässt, werden die überschüssigen Aufträge in eine Warteschlange eingereiht, in der diese darauf warten, dass ein aktuell ausgeführter Momentaufnahmeprozess beendet wird.|  
|**use_partition_groups**|**smallint**|Gibt an, ob die Veröffentlichung vorausberechnete Partitionen verwendet.|  
|**dynamic_filters_function_list**|**nvarchar (500)**|Eine durch Semikolons getrennte Liste der Funktionen, die in den parametrisierten Zeilenfiltern der Veröffentlichung verwendet werden.|  
|**partition_id_eval_proc**|**sysname**|Gibt den Namen der Prozedur an, die vom Merge-Agent eines Abonnenten ausgeführt wird, um die zugewiesene Partitions-ID zu bestimmen.|  
|**publication_number**|**smallint**|Gibt die Identitäts Spalte an, die eine 2-Byte-Zuordnung für **pubid** bereitstellt. **pubid** ist eine Globally Unique Identifier für eine Veröffentlichung, während die Veröffentlichungsnummer nur in einer angegebenen Datenbank eindeutig ist.|  
|**replicate_ddl**|**int**|Gibt an, ob die Schemareplikation für die Veröffentlichung unterstützt wird.<br /><br /> **0** = DDL-Anweisungen werden nicht repliziert.<br /><br /> **1** = auf dem Verleger ausgeführte DDL-Anweisungen werden repliziert.<br /><br /> Weitere Informationen finden Sie unter [Vornehmen von Schemaänderungen in Veröffentlichungsdatenbanken](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**allow_subscriber_initiated_snapshot**|**bit**|Gibt an, dass Abonnenten den Prozess initiieren können, mit dem die Momentaufnahme für eine Veröffentlichung mithilfe parametrisierter Filter generiert wird. der Wert **1** gibt an, dass Abonnenten den Momentaufnahme Prozess initiieren können.|  
|**dynamic_snapshot_queue_timeout**|**int**|Gibt an, wie viele Minuten ein Abonnent in der Warteschlange warten muss, damit der Momentaufnahme-Generierungsprozess gestartet wird, wenn parametrisierte Filter verwendet werden.|  
|**dynamic_snapshot_ready_timeout**|**int**|Gibt an, wie viele Minuten ein Abonnent wartet, dass der Momentaufnahme-Generierungsprozess abgeschlossen wird, wenn parametrisierte Filter verwendet werden.|  
|**Verleih**|**sysname**|Der Name des Verteilers für die Veröffentlichung.|  
|**snapshot_jobid**|**Binary (16)**|Identifiziert den Agentauftrag, der die Momentaufnahme generiert, wenn der Abonnent den Momentaufnahmeprozess initiieren kann.|  
|**allow_web_synchronization**|**bit**|Gibt an, ob die Veröffentlichung für die Websynchronisierung aktiviert ist, wobei **1** bedeutet, dass die Websynchronisierung für die Veröffentlichung aktiviert ist.|  
|**web_synchronization_url**|**nvarchar (500)**|Gibt den Standardwert für die Internet-URL an, die für die Websynchronisierung verwendet wird.|  
|**allow_partition_realignment**|**bit**|Gibt an, ob Löschvorgänge an den Abonnenten gesendet werden, wenn durch eine Änderung der Zeile auf dem Verleger die Partition geändert wird.<br /><br /> **0** = Daten aus einer alten Partition verbleiben auf dem Abonnenten. Änderungen an diesen Daten auf dem Verleger werden nicht an diesen Abonnenten repliziert, aber auf dem Abonnenten vorgenommene Änderungen werden auf den Verleger repliziert.<br /><br /> **1** = löscht den Abonnenten, um die Ergebnisse einer Partitions Änderung widerzuspiegeln, indem Daten entfernt werden, die nicht mehr Teil der Partition des Abonnenten sind.<br /><br /> Weitere Informationen finden Sie unter [sp_addmergepublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).<br /><br /> Hinweis: Daten, die auf dem Abonnenten verbleiben, wenn dieser Wert **0** ist, sollten so behandelt werden, als wären sie schreibgeschützt. Dies wird jedoch vom Replikationssystem nicht streng erzwungen.|  
|**retention_period_unit**|**tinyint**|Definiert die Einheit, die beim Definieren der *Beibehaltung* verwendet wird. die folgenden Werte sind möglich:<br /><br /> **0** = Tag.<br /><br /> **1** = Woche.<br /><br /> **2** = Monat.<br /><br /> **3** = Jahr|  
|**decentralized_conflicts**|**int**|Gibt an, ob die Konfliktdatensätze auf dem Abonnenten gespeichert werden, der den Konflikt verursacht hat:<br /><br /> **0** = Konflikt Datensätze werden nicht auf dem Abonnenten gespeichert.<br /><br /> **1** = Konflikt Datensätze werden auf dem Abonnenten gespeichert.|  
|**generation_leveling_threshold**|**int**|Gibt die Anzahl der Änderungen an, die in einer Generierung enthalten sind. Eine Generierung ist eine Auflistung von Änderungen, die an einen Verleger oder Abonnenten übermittelt werden.|  
|**automatic_reinitialization_policy**|**bit**|Gibt an, ob Änderungen vom Abonnenten vor einer automatischen erneuten Initialisierung hochgeladen werden.<br /><br /> **1** = Änderungen werden vom Abonnenten hochgeladen, bevor eine automatische Neuinitialisierung erfolgt.<br /><br /> **0** = Änderungen werden vor einer automatischen Neuinitialisierung nicht hochgeladen.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)  
  
  
