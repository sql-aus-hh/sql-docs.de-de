---
description: sp_addpublication (Transact-SQL)
title: sp_addpublication (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_addpublication_TSQL
- sp_addpublication
helpviewer_keywords:
- sp_addpublication
ms.assetid: c7167ed1-2b7e-4824-b82b-65f4667c4407
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9aa65ff9b5b9c14441ac555d40788e92082f01c5
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564449"
---
# <a name="sp_addpublication-transact-sql"></a>sp_addpublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Erstellt eine Momentaufnahme- oder Transaktionsveröffentlichung. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_addpublication [ @publication = ] 'publication'  
    [ , [ @taskid = ] tasked ]  
    [ , [ @restricted = ] 'restricted' ]  
    [ , [ @sync_method = ] 'sync_method' ]  
    [ , [ @repl_freq = ] 'repl_freq' ]  
    [ , [ @description = ] 'description' ]  
    [ , [ @status = ] 'status' ]  
    [ , [ @independent_agent = ] 'independent_agent' ]  
    [ , [ @immediate_sync = ] 'immediate_sync' ]  
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]  
    [ , [ @allow_push = ] 'allow_push'  
    [ , [ @allow_pull = ] 'allow_pull' ]  
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]  
    [ , [ @allow_sync_tran = ] 'allow_sync_tran' ]  
    [ , [ @autogen_sync_procs = ] 'autogen_sync_procs' ]  
    [ , [ @retention = ] retention ]  
    [ , [ @allow_queued_tran= ] 'allow_queued_updating' ]  
    [ , [ @snapshot_in_defaultfolder= ] 'snapshot_in_default_folder' ]  
    [ , [ @alt_snapshot_folder= ] 'alternate_snapshot_folder' ]  
    [ , [ @pre_snapshot_script= ] 'pre_snapshot_script' ]  
    [ , [ @post_snapshot_script= ] 'post_snapshot_script' ]  
    [ , [ @compress_snapshot= ] 'compress_snapshot' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port= ] ftp_port ]  
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @allow_dts = ] 'allow_dts' ]  
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]  
    [ , [ @conflict_policy = ] 'conflict_policy' ]  
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @queue_type = ] 'queue_type' ]  
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]  
    [ , [ @logreader_job_name = ] 'logreader_agent_name' ]  
    [ , [ @qreader_job_name = ] 'queue_reader_agent_name' ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @allow_initialize_from_backup = ] 'allow_initialize_from_backup' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @enabled_for_p2p = ] 'enabled_for_p2p' ]  
    [ , [ @publish_local_changes_only = ] 'publish_local_changes_only' ]  
    [ , [ @enabled_for_het_sub = ] 'enabled_for_het_sub' ]  
    [ , [ @p2p_conflictdetection = ] 'p2p_conflictdetection' ]  
    [ , [ @p2p_originator_id = ] p2p_originator_id  
    [ , [ @p2p_continue_onconflict = ] 'p2p_continue_onconflict'  
    [ , [ @allow_partition_switch = ] 'allow_partition_switch'  
    [ , [ @replicate_partition_switch = ]'replicate_partition_switch'  
```  
  
## <a name="arguments"></a>Argumente  
`[ \@publication = ] 'publication'` Der Name der zu erstellenden Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. Dieser Name muss innerhalb der Datenbank eindeutig sein.  
  
`[ \@taskid = ] taskid` Wird nur aus Gründen der Abwärtskompatibilität unterstützt. Verwenden Sie [sp_addpublication_snapshot &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md).  
  
`[ \@restricted = ] 'restricted'` Wird nur aus Gründen der Abwärtskompatibilität unterstützt. Verwenden Sie *default_access*.  
  
`[ \@sync_method = ] _'sync_method'` Der Synchronisierungs Modus. *sync_method* ist vom Datentyp **nvarchar (13)**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**native**|Erstellt eine Massenkopierprogramm-Ausgabe aller Tabellen im einheitlichen Modus. *Wird für Oracle-Verleger nicht unterstützt*.|  
|**character**|Erstellt eine Massenkopierprogramm-Ausgabe aller Tabellen im Zeichenmodus. _Bei einem Oracle-Verleger_ ist das **Zeichen** _nur für die Momentaufnahme Replikation gültig_.|  
|**findende**|Erzeugt eine Massenkopierprogramm-Ausgabe aller Tabellen im einheitlichen Modus, sperrt jedoch die Tabellen während der Erstellung der Momentaufnahme nicht. Wird nur für Transaktionsveröffentlichungen unterstützt. *Wird für Oracle-Verleger nicht unterstützt*.|  
|**concurrent_c**|Erzeugt eine Massenkopierprogramm-Ausgabe aller Tabellen im Zeichenmodus, sperrt jedoch die Tabellen während der Erstellung der Momentaufnahme nicht. Wird nur für Transaktionsveröffentlichungen unterstützt.|  
|**Daten Bank Momentaufnahme**|Erstellt aus einer Datenbankmomentaufnahme eine Massenkopierprogramm-Ausgabe aller Tabellen im einheitlichen Modus. Daten Bank Momentaufnahmen sind nicht in jeder Edition von verfügbar [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|**database snapshot character**|Erstellt aus einer Datenbankmomentaufnahme eine Massenkopierprogramm-Ausgabe aller Tabellen im Zeichenmodus. Daten Bank Momentaufnahmen sind nicht in jeder Edition von verfügbar [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|NULL (Standard)|Der Standardwert ist **native** für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger. Bei nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verlegern wird standardmäßig ein **Zeichen** verwendet, wenn der Wert von *repl_freq* **Momentaufnahme** ist, und für alle anderen Fälle **concurrent_c** .|  
  
`[ \@repl_freq = ] 'repl_freq'` Der Typ der Replikations Häufigkeit, *repl_freq* ist vom Datentyp **nvarchar (10)**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Continuous** (Standard)|Der Verleger stellt die Ausgabe aller protokollbasierten Transaktionen bereit. Für nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger erfordert dies, dass *sync_method* auf **concurrent_c** festgelegt werden.|  
|**Überblick**|Der Verleger gibt nur geplante Synchronisierungsereignisse aus. Für nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger erfordert dies, dass *sync_method* auf **Zeichen** festgelegt werden.|  
  
`[ \@description = ] 'description'` Ist eine optionale Beschreibung für die Veröffentlichung. die *Beschreibung* ist vom Datentyp **nvarchar (255)** und hat den Standardwert NULL.  
  
`[ \@status = ] 'status'` Gibt an, ob Veröffentlichungsdaten verfügbar sind. der *Status* ist **nvarchar (8)**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**active**|Die Veröffentlichungsdaten sind für Abonnenten sofort verfügbar.|  
|**inaktiv** (Standard)|Die Veröffentlichungsdaten sind für Abonnenten zunächst nicht verfügbar, wenn die Veröffentlichung erstellt wird (die Abonnierung ist möglich, aber die Abonnements werden nicht verarbeitet).|  
  
 *Wird für Oracle-Verleger nicht unterstützt*.  
  
`[ \@independent_agent = ] 'independent_agent'` Gibt an, ob eine eigenständige Verteilungs-Agent für diese Veröffentlichung vorhanden ist. *independent_agent* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **True** gibt an, dass eine eigenständige Verteilungs-Agent für diese Veröffentlichung vorhanden ist. Wenn der Wert **false** ist, verwendet die Veröffentlichung einen freigegebenen Verteilungs-Agent und jedes Paar aus Verleger Datenbank und Abonnenten Datenbank verfügt über einen einzelnen freigegebenen Agent.  
  
`[ \@immediate_sync = ] 'immediate_synchronization'` Gibt an, ob die Synchronisierungs Dateien für die Veröffentlichung bei jeder Ausführung des Momentaufnahmen-Agent erstellt werden. *immediate_synchronization* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **True** gibt an, dass die Synchronisierungs Dateien bei jeder Ausführung des Momentaufnahmen-Agent erstellt oder neu erstellt werden. Abonnenten können die Synchronisierungsdateien sofort abrufen, wenn der Momentaufnahme-Agent vor dem Erstellen des Abonnements abgeschlossen wurde. Neue Abonnements rufen die neuesten Synchronisierungsdateien ab, die von der letzten Ausführung des Momentaufnahmeagents generiert wurden. *independent_agent* müssen **true** sein, damit *immediate_synchronization* den Wert **true** hat. Wenn der Wert **false** ist, werden die Synchronisierungs Dateien nur erstellt, wenn neue Abonnements vorhanden sind. Sie müssen [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) für jedes Abonnement abrufen, wenn Sie einer vorhandenen Veröffentlichung inkrementell einen neuen Artikel hinzufügen. Abonnenten können die Synchronisierungsdateien nach dem Einrichten des Abonnements erst empfangen, wenn die Momentaufnahme-Agents gestartet und abgeschlossen wurden.  
  
`[ \@enabled_for_internet = ] 'enabled_for_internet'` Gibt an, ob die Veröffentlichung für das Internet aktiviert ist, und bestimmt, ob die Momentaufnahme Dateien mithilfe von FTP (File Transfer Protocol) auf einen Abonnenten übertragen werden können. *enabled_for_internet* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **True** gibt an, dass die Synchronisierungs Dateien für die Veröffentlichung im Verzeichnis c:\Programme\Microsoft SQL Server\MSSQL\MSSQL.x\Repldata\Ftp abgelegt werden. Der Benutzer muss das FTP-Verzeichnis erstellen.  
  
`[ \@allow_push = ] 'allow_push'` Gibt an, ob für die angegebene Veröffentlichung Pushabonnements erstellt werden können. *allow_push* ist vom Datentyp **nvarchar (5)** und hat den Standardwert true, mit dem Pushabonnements für die Veröffentlichung zulässig sind.  
  
`[ \@allow_pull = ] 'allow_pull'` Gibt an, ob Pullabonnements für die angegebene Veröffentlichung erstellt werden können. *allow_pull* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. Wenn der Wert **false** ist, sind Pullabonnements für die Veröffentlichung nicht zulässig.  
  
`[ \@allow_anonymous = ] 'allow_anonymous'` Gibt an, ob für die angegebene Veröffentlichung anonyme Abonnements erstellt werden können. *allow_anonymous* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **True** gibt an, dass *immediate_synchronization* auch auf **true** festgelegt werden muss. Wenn der Wert **false** ist, sind anonyme Abonnements für die Veröffentlichung nicht zulässig.  
  
`[ \@allow_sync_tran = ] 'allow_sync_tran'` Gibt an, ob Abonnements mit sofortigem Update für die Veröffentlichung zulässig sind. *allow_sync_tran* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. " **true** " wird *für Oracle-Verleger nicht unterstützt*.  
  
`[ \@autogen_sync_procs = ] 'autogen_sync_procs'` Gibt an, ob die Synchronisierungs gespeicherte Prozedur zum Aktualisieren von Abonnements auf dem Verleger generiert wird. *autogen_sync_procs* ist vom Datentyp **nvarchar (5)**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**true**|Wird automatisch festgelegt, wenn das Aktualisieren von Abonnements aktiviert ist.|  
|**false**|Wird automatisch für Oracle-Verleger festgelegt oder wenn das Aktualisieren von Abonnements nicht aktiviert ist.|  
|NULL (Standard)|Der Standardwert ist **true** , wenn das Aktualisieren von Abonnements aktiviert ist, und **false** , wenn Abonnements nicht aktiviert sind.|  
  
> [!NOTE]  
>  Der vom Benutzer angegebene Wert für *autogen_sync_procs* wird abhängig von den für *allow_queued_tran* und *allow_sync_tran* angegebenen Werten überschrieben.  
  
`[ \@retention = ] retention` Die Beibehaltungs Dauer in Stunden für Abonnement Aktivitäten. die *Beibehaltung* ist vom Datentyp **int** und hat den Standardwert 336 Stunden. Wenn ein Abonnement im Beibehaltungszeitraum nicht aktiv ist, läuft es ab und wird entfernt. Der Wert kann größer als die maximale Beibehaltungsdauer der vom Verleger verwendeten Verteilungsdatenbank sein. Wenn der Wert **0** ist, laufen bekannte Abonnements für die Veröffentlichung nie ab und werden vom abgelaufenen abonnementcleanupagent entfernt.  
  
`[ \@allow_queued_tran = ] 'allow_queued_updating'` Aktiviert oder deaktiviert das Einreihen von Änderungen auf dem Abonnenten in die Warteschlange, bis Sie auf dem Verleger angewendet werden können. *allow_queued_updating* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **False** gibt an, dass Änderungen auf dem Abonnenten nicht in die Warteschlange eingereiht werden. " **true** " wird *für Oracle-Verleger nicht unterstützt*.  
  
`[ \@snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Gibt an, ob Momentaufnahme Dateien im Standardordner gespeichert werden. *snapshot_in_default_folder* ist vom Datentyp **nvarchar (5)** und hat den Standardwert true. **True** gibt an, dass Momentaufnahme Dateien im Standardordner gefunden werden. Wenn der Wert **false** ist, wurden Momentaufnahme Dateien an dem alternativen Speicherort gespeichert, der durch *alternate_snapshot_folder* angegeben wurde. Alternative Speicherorte können sich auf einem anderen Server, auf einem Netzlaufwerk oder auf Wechselmedien befinden (z. B. auf CD-ROM oder auf einem Wechseldatenträger). Momentaufnahmedateien können auch auf einer FTP-Site gespeichert werden, um zu einem späteren Zeitpunkt vom Abonnenten abgerufen zu werden. Beachten Sie, dass dieser Parameter true sein kann und noch über eine Position im **\@ alt_snapshot_folder** -Parameter verfügt. Diese Kombination gibt an, dass die Momentaufnahmedateien sowohl im Standardpfad als auch im alternativen Pfad gespeichert werden.  
  
`[ \@alt_snapshot_folder = ] 'alternate_snapshot_folder'` Gibt den Speicherort des alternativen Ordners für die Momentaufnahme an. *alternate_snapshot_folder* ist vom Datentyp **nvarchar (255)** und hat den Standardwert NULL.  
  
`[ \@pre_snapshot_script = ] 'pre_snapshot_script'` Gibt einen Zeiger auf einen Speicherort für **SQL** -Dateien an. *pre_snapshot_script* ist vom Datentyp **nvarchar (255)** und hat den Standardwert NULL. Der Verteilungs-Agent führt das vor der Momentaufnahme ausgeführte Skript vor allen Skripts für replizierte Objekte aus, wenn die Momentaufnahme bei einem Abonnenten angewendet wird. Das Skript wird beim Herstellen der Verbindung mit der Abonnementdatenbank in dem vom Verteilungs-Agent verwendeten Sicherheitskontext ausgeführt.  
  
`[ \@post_snapshot_script = ] 'post_snapshot_script'` Gibt einen Zeiger auf einen Speicherort für **SQL** -Dateien an. *post_snapshot_script* ist vom Datentyp **nvarchar (255)** und hat den Standardwert NULL. Der Verteilungs-Agent führt das nach der Momentaufnahme ausgeführte Skript aus, nachdem alle anderen Skripts für replizierte Objekte und Daten während der Erstsynchronisierung angewendet wurden. Das Skript wird beim Herstellen der Verbindung mit der Abonnementdatenbank in dem vom Verteilungs-Agent verwendeten Sicherheitskontext ausgeführt.  
  
`[ \@compress_snapshot = ] 'compress_snapshot'`Gibt an, dass die Momentaufnahme, die an den **\@ alt_snapshot_folder** Speicherort geschrieben wird, in das CAB-Format komprimiert werden soll [!INCLUDE[msCoName](../../includes/msconame-md.md)] . *compress_snapshot* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **false** gibt an, dass die Momentaufnahme nicht komprimiert wird. **true** gibt an, dass die Momentaufnahme komprimiert wird. Momentaufnahmedateien, die größer als 2 Gigabyte (GB) sind, können nicht komprimiert werden. Komprimierte Momentaufnahmedateien werden an dem Speicherort dekomprimiert, an dem der Verteilungs-Agent ausgeführt wird. Pullabonnements werden normalerweise mit komprimierten Momentaufnahmen verwendet, sodass die Dateien auf dem Abonnenten dekomprimiert werden. Die Momentaufnahme im Standardordner kann nicht komprimiert werden.  
  
`[ \@ftp_address = ] 'ftp_address'` Die Netzwerkadresse des FTP-Dienstanbieter für den Verteiler. *ftp_address* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Gibt an, wo die Veröffentlichungsmomentaufnahmedateien für den Verteilungs-Agent oder Merge-Agent eines Abonnenten zum Abholen gespeichert sind. Da diese Eigenschaft für jede Veröffentlichung gespeichert wird, kann jede Veröffentlichung über eine andere *ftp_address* verfügen. Die Veröffentlichung muss die Weitergabe von Momentaufnahmen über FTP unterstützen.  
  
`[ \@ftp_port = ] ftp_port` Die Portnummer des FTP-Dienstanbieter für den Verteiler. *ftp_port* ist vom Datentyp **int**. der Standardwert ist 21. Gibt an, wo sich die Veröffentlichungs Momentaufnahme-Dateien für den Verteilungs-Agent oder Merge-Agent eines Abonnenten befinden, der abgerufen werden soll. Da diese Eigenschaft für jede Veröffentlichung gespeichert wird, kann jede Veröffentlichung über eine eigene *ftp_port* verfügen.  
  
`[ \@ftp_subdirectory = ] 'ftp_subdirectory'` Gibt an, wo die Momentaufnahme Dateien für die Verteilungs-Agent oder Merge-Agent des Abonnenten verfügbar sind, wenn die Veröffentlichung das Weitergeben von Momentaufnahmen mithilfe von FTP unterstützt. *ftp_subdirectory* ist vom Datentyp **nvarchar (255)** und hat den Standardwert NULL. Da diese Eigenschaft für jede Veröffentlichung gespeichert wird, kann jede Veröffentlichung über eine eigene *ftp_subdirctory* verfügen, oder es kann kein Unterverzeichnis vorhanden sein, das mit einem NULL-Wert angegeben wird.  
  
`[ \@ftp_login = ] 'ftp_login'` Der Benutzername, mit dem eine Verbindung zum FTP-Dienst hergestellt wird. *ftp_login* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert Anonymous.  
  
`[ \@ftp_password = ] 'ftp_password'` Das Benutzer Kennwort, das zum Herstellen einer Verbindung mit dem FTP-Dienst verwendet wird. *ftp_password* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
`[ \@allow_dts = ] 'allow_dts'` Gibt an, dass die Veröffentlichung Daten Transformationen zulässt. Beim Erstellen eines Abonnements können Sie ein DTS-Paket angeben. *allow_transformable_subscriptions* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false, der keine DTS-Transformationen zulässt. Wenn *allow_dts* auf true festgelegt ist, muss *sync_method* entweder auf **Zeichen** oder **concurrent_c** festgelegt werden.  
  
 " **true** " wird *für Oracle-Verleger nicht unterstützt*.  
  
`[ \@allow_subscription_copy = ] 'allow_subscription_copy'` Aktiviert oder deaktiviert die Möglichkeit zum Kopieren der Abonnement Datenbanken, die diese Veröffentlichung abonnieren. *allow_subscription_copy* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false.  
  
`[ \@conflict_policy = ] 'conflict_policy'` Gibt die Richtlinie zur Konfliktlösung an, gefolgt von der Verwendung der Option für Abonnenten mit verzögertem Update. *conflict_policy* ist vom Datentyp **nvarchar (100)** und hat den Standardwert NULL. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**pub wins**|Der Verleger gewinnt den Konflikt.|  
|**sub reinit**|Erneutes Initialisieren des Abonnements.|  
|**sub wins**|Der Abonnent gewinnt den Konflikt.|  
|NULL (Standard)|Wenn der Wert NULL ist und es sich bei der Veröffentlichung um eine Momentaufnahme Veröffentlichung handelt, wird die Standard Richtlinie **sub reinit**. Wenn der Wert NULL ist und es sich nicht um eine Momentaufnahme Veröffentlichung handelt, wird standardmäßig **pub WINS** angezeigt.|  
  
 *Wird für Oracle-Verleger nicht unterstützt*.  
  
`[ \@centralized_conflicts = ] 'centralized_conflicts'` Gibt an, ob Konflikt Datensätze auf dem Verleger gespeichert werden. *centralized_conflicts* ist vom Datentyp **nvarchar (5)** und hat den Standardwert true. **True** gibt an, dass Konflikt Datensätze auf dem Verleger gespeichert werden. Wenn der Wert **false** ist, werden die Konflikt Datensätze sowohl auf dem Verleger als auch auf dem Abonnenten gespeichert, der den Konflikt verursacht hat. *Wird für Oracle-Verleger nicht unterstützt*.  
  
`[ \@conflict_retention = ] conflict_retention` Gibt die Beibehaltungs Dauer für den Konflikt in Tagen an. Der Zeitraum, in dem Konfliktmetadaten für Peer-zu-Peer-Transaktionsreplikation und Abonnements mit verzögertem Update gespeichert werden. *conflict_retention* ist vom Datentyp **int**. der Standardwert ist 14. *Wird für Oracle-Verleger nicht unterstützt*.  
  
`[ \@queue_type = ] 'queue_type'` Gibt an, welcher Typ von Warteschlange verwendet wird. *queue_type* ist vom Datentyp **nvarchar (10)** und hat den Standardwert NULL. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**sql**|Verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Speichern von Transaktionen.|  
|NULL (Standard)|Standardmäßig wird **SQL** verwendet, das angibt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Speichern von Transaktionen verwendet werden soll.|  
  
> [!NOTE]  
>  Unterstützung für die Verwendung von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing wurde eingestellt. Wenn Sie den Wert **MSMQ** angeben, wird eine Warnung ausgegeben, und der Wert wird von der Replikation automatisch auf **SQL** festgelegt.  
  
 *Wird für Oracle-Verleger nicht unterstützt*.  
  
`[ \@add_to_active_directory = ] 'add\to_active_directory'` Dieser Parameter wurde als veraltet markiert und wird nur für die Abwärtskompatibilität von Skripts unterstützt. Für [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory ist das Hinzufügen von Veröffentlichungsinformationen nicht länger möglich.  
  
`[ \@logreader_job_name = ] 'logreader_agent_name'` Der Name eines vorhandenen Agentauftrags. *logreader_agent_name* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Dieser Parameter wird nur angegeben, wenn der Protokolllese-Agent einen vorhandenen Auftrag verwendet, anstatt dass ein neuer Auftrag erstellt wird.  
  
`[ \@qreader_job_name = ] 'queue_reader_agent_name'` Der Name eines vorhandenen Agentauftrags. *queue_reader_agent_name* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL. Dieser Parameter wird nur angegeben, wenn der Warteschlangenlese-Agent einen vorhandenen Auftrag verwendet, anstatt dass ein neuer Auftrag erstellt wird.  
  
`[ \@publisher = ] 'publisher'` Gibt einen nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger an. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
> [!NOTE]  
>  beim Hinzufügen einer Veröffentlichung zu einem Verleger sollte der *Verleger* nicht verwendet werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ \@allow_initialize_from_backup = ] 'allow_initialize_from_backup'` Gibt an, ob Abonnenten ein Abonnement für diese Veröffentlichung aus einer Sicherung anstelle einer Anfangs Momentaufnahme initialisieren können. *allow_initialize_from_backup* ist vom Datentyp **nvarchar (5)**. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**true**|Aktiviert die Initialisierung aus einer Sicherung.|  
|**false**|Deaktiviert die Initialisierung aus einer Sicherung.|  
|NULL (Standard)|Der Standardwert ist **true** für eine Veröffentlichung in einer Peer-zu-Peer-Replikations Topologie und **false** für alle anderen Veröffentlichungen.|  
  
 Weitere Informationen finden Sie unter [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)initialisiert wird.  
  
> [!WARNING]  
>  Um fehlende Abonnentendaten zu vermeiden, wenn Sie **sp_addpublication** mit `@allow_initialize_from_backup = N'true'`verwenden, sollten Sie immer `@immediate_sync = N'true'`verwenden.  
  
`[ \@replicate_ddl = ] replicate_ddl` Gibt an, ob die Schema Replikation für die Veröffentlichung unterstützt wird. *replicate_ddl* ist vom Datentyp **int**, der Standardwert ist **1** für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger und **0** für nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger. **1** gibt an, dass DDL-Anweisungen (Data Definition Language), die auf dem Verleger ausgeführt werden, repliziert werden, und **0** bedeutet, dass DDL-Anweisungen nicht repliziert werden *Die Schemareplikation wird nicht für Oracle-Verleger unterstützt.* Weitere Informationen finden Sie unter [Vornehmen von Schemaänderungen in Veröffentlichungsdatenbanken](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Der *\@ replicate_ddl* -Parameter wird berücksichtigt, wenn eine DDL-Anweisung eine Spalte hinzufügt. Der *\@ replicate_ddl* -Parameter wird ignoriert, wenn eine DDL-Anweisung eine Spalte aus den folgenden Gründen ändert oder löscht.  
  
-   Wenn eine Spalte gelöscht wird, muss sysarticlecolumns aktualisiert werden, um zu verhindern, dass neue DML-Anweisungen die gelöschte Spalte aufnehmen, wodurch der Verteilungs-Agent fehlschlagen würde. Der *\@ replicate_ddl* -Parameter wird ignoriert, da die Replikation die Schema Änderung immer replizieren muss.  
  
-   Wenn eine Spalte geändert wird, hat sich möglicherweise der Quelldatentyp oder die NULL-Zulässigkeit geändert. Dies hat zur Folge, dass DML-Anweisungen einen Wert enthalten, der möglicherweise nicht mit der Tabelle beim Abonnenten kompatibel ist. Solche DML-Anweisungen können bewirken, dass der Verteilungs-Agent fehlschlägt. Der *\@ replicate_ddl* -Parameter wird ignoriert, da die Replikation die Schema Änderung immer replizieren muss.  
  
-   Wenn von einer DDL-Anweisung eine neue Spalte hinzugefügt wird, enthält sysarticlecolumns die neue Spalte nicht. DML-Anweisungen versuchen nicht, Daten für die neue Spalte zu replizieren. Der Parameter wird berücksichtigt, da sowohl das Replizieren als auch das Nicht-Replizieren der DDL akzeptabel ist.  
  
`[ \@enabled_for_p2p = ] 'enabled_for_p2p'` Ermöglicht die Verwendung der Veröffentlichung in einer Peer-zu-Peer-Replikations Topologie. *enabled_for_p2p* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. **true** gibt an, dass die Veröffentlichung die Peer-zu-Peer-Replikation unterstützt. Wenn Sie *enabled_for_p2p* auf " **true**" festlegen, gelten die folgenden Einschränkungen:  
  
-   *allow_anonymous* muss den Wert **false** aufweisen.  
  
-   *allow_dts* muss den Wert **false** aufweisen.  
  
-   *allow_initialize_from_backup* muss " **true**" sein.  
  
-   *allow_queued_tran* muss den Wert **false** aufweisen.  
  
-   *allow_sync_tran* muss den Wert **false** aufweisen.  
  
-   *conflict_policy* muss den Wert **false** aufweisen.  
  
-   *independent_agent* muss " **true**" sein.  
  
-   *repl_freq* muss **fortlaufend** sein.  
  
-   *replicate_ddl* muss **1** sein.  
  
 Weitere Informationen finden Sie unter [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
`[ \@publish_local_changes_only = ] 'publish_local_changes_only'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ \@enabled_for_het_sub = ] 'enabled_for_het_sub'` Ermöglicht der Veröffentlichung, nicht--Abonnenten zu unterstützen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *enabled_for_het_sub* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. Der Wert **true** bedeutet, dass die Veröffentlichung nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Abonnenten unterstützt. Wenn *enabled_for_het_sub* den Wert **true** hat, gelten die folgenden Einschränkungen:  
  
-   *allow_initialize_from_backup* muss den Wert **false** aufweisen.  
  
-   *allow_push* muss " **true**" sein.  
  
-   *allow_queued_tran* muss den Wert **false** aufweisen.  
  
-   *allow_subscription_copy* muss den Wert **false** aufweisen.  
  
-   *allow_sync_tran* muss den Wert **false** aufweisen.  
  
-   *autogen_sync_procs* muss den Wert **false** aufweisen.  
  
-   *conflict_policy* muss NULL sein.  
  
-   *enabled_for_internet* muss den Wert **false** aufweisen.  
  
-   *enabled_for_p2p* muss den Wert **false** aufweisen.  
  
-   *ftp_address* muss NULL sein.  
  
-   *ftp_subdirectory* muss NULL sein.  
  
-   *ftp_password* muss NULL sein.  
  
-   *pre_snapshot_script* muss NULL sein.  
  
-   *post_snapshot_script* muss NULL sein.  
  
-   *replicate_ddl* muss 0 sein.  
  
-   *qreader_job_name* muss NULL sein.  
  
-   *queue_type* muss NULL sein.  
  
-   *sync_method* kann nicht **nativ** oder **gleichzeitig** sein.  
  
 Weitere Informationen finden Sie unter [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
`[ \@p2p_conflictdetection = ] 'p2p_conflictdetection'` Ermöglicht dem Verteilungs-Agent, Konflikte zu erkennen, wenn die Veröffentlichung für die Peer-zu-Peer-Replikation aktiviert ist. *p2p_conflictdetection* ist vom Datentyp **nvarchar (5)** und hat den Standardwert true. Weitere Informationen finden Sie unter [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
`[ \@p2p_originator_id = ] p2p_originator_id` Gibt eine ID für einen Knoten in einer Peer-zu-Peer-Topologie an. *p2p_originator_id* ist vom Datentyp **int** und hat den Standardwert NULL. Diese ID wird für die Konflikterkennung verwendet, wenn *p2p_conflictdetection* auf true festgelegt ist. Geben Sie eine positive ID ungleich 0 (null) an, die in der Topologie noch nie verwendet wurde. Führen Sie [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md)aus, um eine Liste der bereits verwendeten IDs anzuzeigen.  
  
`[ \@p2p_continue_onconflict = ] 'p2p_continue_onconflict'` Bestimmt, ob die Verteilungs-Agent die Verarbeitung von Änderungen fortsetzt, nachdem ein Konflikt erkannt wurde. *p2p_continue_onconflict* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false.  
  
> [!CAUTION]  
>  Es wird empfohlen, den Standardwert FALSE zu verwenden. Wenn diese Option auf TRUE festgelegt wird, versucht der Verteilungs-Agent, die Datenkonvergenz in der Topologie herbeizuführen, indem die konfliktverursachende Zeile von dem Knoten mit der höchsten Absender-ID angewendet wird. Bei dieser Methode ist keine Konvergenz garantiert. Sie sollten sicherstellen, dass die Topologie nach der Erkennung eines Konflikts konsistent ist. Weitere Informationen finden Sie im Abschnitt "Konfliktbehandlung" unter [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
  
`[ \@allow_partition_switch = ] 'allow_partition_switch'` Gibt an, ob ALTER TABLE... Switch-Anweisungen können für die veröffentlichte Datenbank ausgeführt werden. *allow_partition_switch* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. Weitere Informationen finden Sie unter [Replicate Partitioned Tables and Indexes](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).  
  
`[ \@replicate_partition_switch = ] 'replicate_partition_switch'` Gibt an, ob ALTER TABLE... Switch-Anweisungen, die für die veröffentlichte Datenbank ausgeführt werden, sollten auf Abonnenten repliziert werden. *replicate_partition_switch* ist vom Datentyp **nvarchar (5)** und hat den Standardwert false. Diese Option ist nur gültig, wenn *allow_partition_switch* auf true festgelegt ist.  

## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_addpublication** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
 Wenn mehrere Veröffentlichungen vorhanden sind, die das gleiche Datenbankobjekt veröffentlichen, replizieren nur Veröffentlichungen mit dem *replicate_ddl* Wert **1** die DDL-Anweisungen ALTER TABLE, Alter View, ALTER PROCEDURE, Alter Function und Alter Triggern. Eine ALTER TABLE DROP COLUMN DDL-Anweisung wird hingegen von allen Veröffentlichungen repliziert, die die gelöschte Spalte veröffentlichen.  
  
 Wenn die DDL-Replikation (*replicate_ddl*  =  **1**) für eine Veröffentlichung aktiviert ist, um nicht replizierende DDL-Änderungen an der Veröffentlichung vorzunehmen, müssen [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) zuerst ausgeführt werden, um *replicate_ddl* auf " **0**" festzulegen. Nachdem die nicht replizierenden DDL-Anweisungen ausgegeben wurden, kann [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) erneut ausgeführt werden, um die DDL-Replikation wieder zu aktivieren.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-transa_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_addpublication** ausführen. Für Anmeldungen unter Verwendung der Windows-Authentifizierung muss in der Datenbank ein Konto vorhanden sein, das das zugehörige Windows-Benutzerkonto darstellt. Ein Benutzerkonto für eine Windows-Gruppe reicht in diesem Fall nicht aus.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_addlogreader_agent &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_addpublication_snapshot &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [sp_replicationdboption &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
