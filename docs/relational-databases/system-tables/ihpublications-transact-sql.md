---
description: IHpublications (Transact-SQL)
title: IHpublications (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHpublications_TSQL
- IHpublications
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublications system table
ms.assetid: b519a101-fa53-44be-bd55-6ea79245b5d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: a97462b4224244633a945f2108370b8c4f549f63
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098243"
---
# <a name="ihpublications-transact-sql"></a>IHpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **IHpublications** -Systemtabelle enthält eine Zeile für jede nicht SQL Server Veröffentlichung mithilfe des aktuellen Verteilers. Diese Tabelle wird in der Verteilungsdatenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**pubid**|**int**|Die Identitätsspalte mit einer eindeutigen ID für die Veröffentlichung.|  
|**name**|**sysname**|Der eindeutige der Veröffentlichung zugeordnete Name.|  
|**repl_freq**|**tinyint**|Replikationshäufigkeit:<br /><br /> **0** = Transaktions basiert.<br /><br /> **1** = geplante Tabellen Aktualisierung|  
|**status**|**tinyint**|Der Status der Veröffentlichung; dieser kann einen der folgenden Werte annehmen:<br /><br /> **0** = inaktiv.<br /><br /> **1** = aktiv.|  
|**sync_method**|**tinyint**|Die Synchronisierungsmethode:<br /><br /> **1** = Massen Kopiervorgang für Zeichen.<br /><br /> **4** = concurrent_c. Dies bedeutet, dass das Massen Kopieren von Zeichen verwendet wird, aber Tabellen während der Momentaufnahme nicht gesperrt sind.|  
|**snapshot_jobid**|**binary**|Die ID des geplanten Tasks.|  
|**enabled_for_internet**|**bit**|Gibt an, ob die Synchronisierungs Dateien für die Veröffentlichung über FTP und andere Dienste im Internet verfügbar gemacht werden, wobei **1** bedeutet, dass über das Internet auf Sie zugegriffen werden kann.|  
|**immediate_sync_ready**|**bit**|Gibt an, ob die Synchronisierungs Dateien verfügbar sind, wobei **1** bedeutet, dass Sie verfügbar sind. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**allow_queued_tran**|**bit**|Gibt an, ob das Einreihen von Änderungen auf dem Abonnenten in Warteschlangen, bis diese Änderungen auf dem Verleger angewendet werden können, aktiviert wurde. Bei **1** werden Änderungen auf dem Abonnenten in die Warteschlange eingereiht. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**allow_sync_tran**|**bit**|Gibt an, ob Abonnements mit sofortiger Aktualisierung für die Veröffentlichung zulässig sind. **1** bedeutet, dass Abonnements mit sofortigem Update zulässig sind. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**autogen_sync_procs**|**bit**|Gibt an, ob die synchronisierende gespeicherte Prozedur für Abonnements mit sofortiger Aktualisierung beim Verleger generiert wird. der Wert **1** bedeutet, dass er auf dem Verleger generiert wird. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**snapshot_in_defaultfolder**|**bit**|Gibt an, ob Momentaufnahme Dateien im Standardordner gespeichert werden. Wenn der Wert **0** ist, wurden die Momentaufnahme Dateien am alternativen Speicherort gespeichert, der durch *alternate_snapshot_folder* angegeben wurde. Wenn der Wert **1** ist, befinden sich die Momentaufnahme Dateien im Standardordner.|  
|**alt_snapshot_folder**|**nvarchar (510)**|Gibt den Speicherort des anderen Ordners für die Momentaufnahme an.|  
|**pre_snapshot_script**|**nvarchar (510)**|Gibt einen Zeiger auf einen **SQL** -Datei Speicherort an. Der Verteilungs-Agent führt das vor der Momentaufnahme ausgeführte Skript vor allen Skripts für replizierte Objekte aus, wenn die Momentaufnahme auf einem Abonnenten angewendet wird.|  
|**post_snapshot_script**|**nvarchar (510)**|Gibt einen Zeiger auf einen **SQL** -Datei Speicherort an. Der Verteilungs-Agent führt das Skript nach der Momentaufnahme aus, nachdem alle anderen Skripts für replizierte Objekte und Daten während der erst Synchronisierung angewendet wurden.|  
|**compress_snapshot**|**bit**|Gibt an, dass die Momentaufnahme, die an den *alt_snapshot_folder* Speicherort geschrieben wird, in das CAB-Format komprimiert werden soll [!INCLUDE[msCoName](../../includes/msconame-md.md)] . der Wert **0** gibt an, dass die Momentaufnahme nicht komprimiert wird.|  
|**ftp_address**|**sysname**|Die Netzwerkadresse des FTP-Dienstanbieter für den Verteiler. Gibt an, wo die Veröffentlichungsmomentaufnahmedateien zum Aufnehmen durch den Verteilungs-Agent gespeichert sind.|  
|**ftp_port**|**int**|Die Portnummer des FTP-Dienstanbieter für den Verteiler. Gibt an, wo die Veröffentlichungsmomentaufnahmedateien zur Aufnahme durch den Verteilungs-Agent gespeichert sind.|  
|**ftp_subdirectory**|**nvarchar (510)**|Gibt an, wo die Momentaufnahmedateien für den Verteilungs-Agent zum Aufnehmen verfügbar sind, wenn die Veröffentlichung das Weitergeben von Momentaufnahmen mithilfe von FTP unterstützt.|  
|**ftp_login**|**nvarchar(256)**|Der zum Herstellen einer Verbindung mit dem FTP-Dienst verwendete Benutzername.|  
|**ftp_password**|**nvarchar (1048)**|Das Benutzer Kennwort, das für die Verbindung mit dem FTP-Dienst verwendet wird|  
|**allow_dts**|**bit**|Gibt an, dass die Veröffentlichung Datentransformationen zulässt. **1** gibt an, dass DTS-Transformationen zulässig sind. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**allow_anonymous**|**bit**|Gibt an, ob anonyme Abonnements für die Veröffentlichung zulässig sind. der Wert **1** bedeutet, dass Sie zulässig sind.|  
|**centralized_conflicts**|**bit**|Gibt an, ob Konfliktdatensätze auf dem Verleger gespeichert werden:<br /><br /> **0** = Konflikt Datensätze werden sowohl auf dem Verleger als auch auf dem Abonnenten gespeichert, der den Konflikt verursacht hat.<br /><br /> **1** = Konflikt Datensätze werden auf dem Verleger gespeichert.<br /><br /> *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**conflict_retention**|**int**|Gibt die Konfliktaufbewahrungsdauer in Tagen an. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**conflict_policy**|**int**|Gibt die Richtlinie zur Konfliktlösung an, die für die Option zur verzögerten Aktualisierung über eine Warteschlange verwendet wird. Einer der folgenden Werte ist möglich:<br /><br /> **1** = der Verleger gewinnt den Konflikt.<br /><br /> **2** = Abonnent gewinnt den Konflikt.<br /><br /> **3** = das Abonnement wird erneut initialisiert.<br /><br /> *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**queue_type**|**int**|Gibt an, welcher Wartenschlangentyp verwendet wird. Einer der folgenden Werte ist möglich:<br /><br /> **1** = MSMQ, das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing zum Speichern von Transaktionen verwendet.<br /><br /> **2** = SQL, das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Speichern von Transaktionen verwendet.<br /><br /> Diese Spalte wird von nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verlegern nicht verwendet.<br /><br /> Hinweis: die Verwendung von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing wurde als veraltet markiert und wird nicht mehr unterstützt.<br /><br /> *Diese Spalte wird für nicht-SQL-Verleger nicht unterstützt.*|  
|**ad_guidname**|**sysname**|Gibt an, ob die Veröffentlichung in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory veröffentlicht wird. Ein gültiger Globally Unique Identifier (GUID) gibt an, dass die Veröffentlichung im Active Directory veröffentlicht wird [!INCLUDE[msCoName](../../includes/msconame-md.md)] , und der GUID ist das entsprechende Active Directory Veröffentlichungs Objekt **objectGUID**. Wenn dieser Wert NULL ist, wird die Veröffentlichung nicht in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory veröffentlicht. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**backward_comp_level**|**int**|Datenbankkompatibilitätsgrad, der einen der folgenden Werte annehmen kann:<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .<br /><br /> *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**description**|**nvarchar(255)**|Beschreibender Eintrag für die Veröffentlichung.|  
|**independent_agent**|**bit**|Gibt an, ob eine eigenständige Verteilungs-Agent für diese Veröffentlichung vorhanden ist.<br /><br /> **0** = die Veröffentlichung verwendet einen freigegebenen Verteilungs-Agent, und jedes Paar aus Verleger Datenbank und Abonnenten Datenbank verfügt über einen einzelnen freigegebenen Agent.<br /><br /> **1** = für diese Veröffentlichung ist ein eigenständiger Verteilungs-Agent vorhanden.|  
|**immediate_sync**|**bit**|Gibt an, ob die Synchronisierungs Dateien bei jeder Ausführung des Momentaufnahmen-Agent erstellt oder neu erstellt werden, wobei **1** bedeutet, dass Sie bei jeder Ausführung des Agents erstellt werden.|  
|**allow_push**|**bit**|Gibt an, ob Pushabonnements für die Veröffentlichung zulässig sind. der Wert **1** bedeutet, dass Sie zulässig sind.|  
|**allow_pull**|**bit**|Gibt an, ob Pullabonnements für die Veröffentlichung zulässig sind, wobei **1** bedeutet, dass Sie zulässig sind.|  
|**zurück**|**int**|Der Änderungsumfang in Stunden, der für die angegebene Veröffentlichung eingespart werden soll.|  
|**allow_subscription_copy**|**bit**|Gibt an, ob die Möglichkeit zum Kopieren der Abonnementdatenbanken aktiviert wurde, die diese Veröffentlichung abonniert haben. **1** bedeutet, dass das Kopieren zulässig ist.|  
|**allow_initialize_from_backup**|**bit**|Gibt an, ob Abonnenten ein Abonnement für diese Veröffentlichung über eine Sicherung anstelle einer Anfangsmomentaufnahme initialisieren können. **1** bedeutet, dass Abonnements aus einer Sicherung initialisiert werden können, und **0** bedeutet, dass Sie nicht möglich sind. Weitere Informationen finden Sie unter [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)initialisiert wird. *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**min_autonosync_lsn**|**Binary (1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Gibt an, ob die Schema Replikation für die Veröffentlichung unterstützt wird. **1** gibt an, dass auf dem Verleger ausgeführte DDL-Anweisungen repliziert werden, und **0** gibt an, dass DDL-Anweisungen nicht repliziert werden. Weitere Informationen finden Sie unter [Vornehmen von Schemaänderungen in Veröffentlichungsdatenbanken](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md). *Dies wird für Nicht-SQL-Verleger nicht unterstützt.*|  
|**options**|**int**|Bitmuster, mit dem zusätzliche Veröffentlichungsoptionen angegeben werden, mit den folgenden bitweisen Optionswerten:<br /><br /> **0x1** : für die Peer-zu-Peer-Replikation aktiviert.<br /><br /> **0x2** -nur lokale Änderungen veröffentlichen.<br /><br /> **0x4** -aktiviert für nicht-SQL Server-Abonnenten.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [syspublications &#40;System Ansicht&#41; &#40;Transact-SQL-&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)   
 [syspublications &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/syspublications-transact-sql.md)  
  
  
