---
description: sp_changepublication (Transact-SQL)
title: sp_changepublication (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_changepublication
- sp_changepublication_TSQL
helpviewer_keywords:
- sp_changepublication
ms.assetid: c36e5865-25d5-42b7-b045-dc5036225081
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6e29fade73adba6cb82b6d4ac22cae122d4c60cd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169443"
---
# <a name="sp_changepublication-transact-sql"></a>sp_changepublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ändert die Eigenschaften einer Veröffentlichung. Diese gespeicherte Prozedur wird auf dem Verleger für die Veröffentlichungs Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_changepublication [ [ @publication = ] 'publication' ]  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @publication = ] 'publication'` Der Name der Veröffentlichung. *Publication* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
`[ @property = ] 'property'` Die Veröffentlichungs Eigenschaft, die geändert werden soll. die *Eigenschaft* ist vom Datentyp **nvarchar (255)**.  
  
`[ @value = ] 'value'` Der neue Eigenschafts Wert. der Wert ist vom Datentyp **nvarchar (255)** und hat den Standard *Wert* NULL.  
  
 Diese Tabelle beschreibt die änderbaren Eigenschaften der Veröffentlichung sowie die Einschränkungen für die Werte dieser Eigenschaften.  
  
|Eigenschaft|Wert|Beschreibung|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|**true**|Anonyme Abonnements können für die angegebene Veröffentlichung erstellt werden, und *immediate_sync* muss ebenfalls den Wert " **true**" aufweisen. Kann für Peer-zu-Peer-Veröffentlichungen nicht geändert werden.|  
||**false**|Anonyme Abonnements können für die Veröffentlichung nicht erstellt werden. Kann für Peer-zu-Peer-Veröffentlichungen nicht geändert werden.|  
|**allow_initialize_from_backup**|**true**|Abonnenten können ein Abonnement für diese Veröffentlichung aus einer Sicherung statt aus einer Anfangsmomentaufnahme initialisieren. Diese Eigenschaft kann für Nicht-[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**false**|Abonnenten müssen die Anfangsmomentaufnahme verwenden. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**allow_partition_switch**|**true**|ALTER TABLE... Switch-Anweisungen können für die veröffentlichte Datenbank ausgeführt werden. Weitere Informationen finden Sie unter [Replicate Partitioned Tables and Indexes](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).|  
||**false**|ALTER TABLE... Switch-Anweisungen können nicht für die veröffentlichte Datenbank ausgeführt werden.|  
|**allow_pull**|**true**|Pullabonnements sind für die angegebene Veröffentlichung zulässig. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**false**|Pullabonnements sind für die angegebene Veröffentlichung nicht zulässig. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**allow_push**|**true**|Pushabonnements sind für die angegebene Veröffentlichung zulässig.|  
||**false**|Pushabonnements sind für die angegebene Veröffentlichung nicht zulässig.|  
|**allow_subscription_copy**|**true**|Aktiviert die Möglichkeit zum Kopieren von Datenbanken, die diese Veröffentlichung abonnieren. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**false**|Deaktiviert die Möglichkeit zum Kopieren von Datenbanken, die diese Veröffentlichung abonnieren. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**alt_snapshot_folder**||Der Speicherort des alternativen Ordners für die Momentaufnahme.|  
|**centralized_conflicts**|**true**|Die Konfliktdatensätze werden auf dem Verleger gespeichert. Kann nur geändert werden, wenn keine aktiven Abonnements vorhanden sind. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**false**|Die Konfliktdatensätze werden auf dem Verleger und auf dem Abonnenten gespeichert, der den Konflikt verursacht hat. Kann nur geändert werden, wenn keine aktiven Abonnements vorhanden sind. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**compress_snapshot**|**true**|Die Momentaufnahme in einem alternativen Momentaufnahmeordner wird in das CAB-Dateiformat komprimiert. Die Momentaufnahme im Standard-Momentaufnahmeordner kann nicht komprimiert werden.|  
||**false**|Die Momentaufnahme wird nicht komprimiert. Dies ist das Standardverhalten für die Replikation.|  
|**conflict_policy**|**pub wins**|Richtlinie zur Konfliktlösung für Updateabonnenten, bei der der Verleger den Konflikt gewinnt. Diese Eigenschaft kann nur geändert werden, wenn keine aktiven Abonnements vorhanden sind. Diese Option wird für Oracle-Verleger nicht unterstützt.|  
||**sub reinit**|Wenn für Updateabonnenten ein Konflikt auftritt, muss das Abonnement erneut initialisiert werden. Diese Eigenschaft kann nur geändert werden, wenn keine aktiven Abonnements vorhanden sind. Diese Option wird für Oracle-Verleger nicht unterstützt.|  
||**sub wins**|Richtlinie zur Konfliktlösung für Updateabonnenten, bei der der Abonnent den Konflikt gewinnt. Diese Eigenschaft kann nur geändert werden, wenn keine aktiven Abonnements vorhanden sind. Diese Option wird für Oracle-Verleger nicht unterstützt.|  
|**conflict_retention**||**int** , der die Beibehaltungs Dauer für den Konflikt in Tagen angibt. Die Standardaufbewahrungsdauer beträgt 14 Tage. der Wert **0** bedeutet, dass kein Konflikt Bereinigungs Vorgang erforderlich ist. Diese Option wird für Oracle-Verleger nicht unterstützt.|  
|**description**||Eine optionale Beschreibung der Veröffentlichung.|  
|**enabled_for_het_sub**|**true**|Aktiviert die Unterstützung von Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abonnenten durch die Veröffentlichung. **enabled_for_het_sub** kann nicht geändert werden, wenn Abonnements für die Veröffentlichung vorhanden sind. Sie müssen möglicherweise [gespeicherte Replikations Prozeduren (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) ausführen, um die folgenden Anforderungen zu erfüllen, bevor Sie **enabled_for_het_sub** auf "true" festlegen:<br /> - **allow_queued_tran** muss den Wert **false** aufweisen.<br /> - **allow_sync_tran** muss den Wert **false** aufweisen.<br /> Wenn **enabled_for_het_sub** in **true** geändert wird, können vorhandene Veröffentlichungs Einstellungen geändert werden. Weitere Informationen finden Sie unter [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md). Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**false**|Die Veröffentlichung unterstützt Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abonnenten nicht. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**enabled_for_internet**|**true**|Die Veröffentlichung ist für das Internet aktiviert. FTP (File Transfer Protocol) kann dazu verwendet werden, die Momentaufnahmedateien an einen Abonnenten zu übermitteln. Die Synchronisierungsdateien für die Veröffentlichung werden im folgenden Verzeichnis abgelegt: C:\Programme\Microsoft SQL Server\MSSQL\Repldata\ftp. *ftp_address* darf nicht NULL sein. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**false**|Die Veröffentlichung ist nicht für das Internet aktiviert. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**enabled_for_p2p**|**true**|Die Veröffentlichung unterstützt die Peer-zu-Peer-Replikation. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.<br /> Um **enabled_for_p2p** auf " **true**" festzulegen, gelten die folgenden Einschränkungen:<br /> - **allow_anonymous** muss " **false** " sein.<br /> - **allow_dts** muss den Wert **false** aufweisen.<br /> - **allow_initialize_from_backup** muss " **true** " sein.<br /> - **allow_queued_tran** muss den Wert **false** aufweisen.<br /> - **allow_sync_tran** muss den Wert **false** aufweisen.<br /> - **enabled_for_het_sub** muss den Wert **false** aufweisen.<br /> - **independent_agent** muss " **true**" sein.<br /> - **repl_freq** muss **fortlaufend** sein.<br /> - **replicate_ddl** muss **1** sein.|  
||**false**|Die Veröffentlichung unterstützt die Peer-zu-Peer-Replikation nicht. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**ftp_address**||Der Speicherort der Veröffentlichungsmomentaufnahmedateien, auf den über FTP zugegriffen werden kann. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**ftp_login**||Der Benutzername, mit dem eine Verbindung mit dem FTP-Dienst hergestellt wird. Der Wert ANONYMOUS ist zulässig. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**ftp_password**||Das Kennwort für den Benutzernamen, mit dem eine Verbindung mit dem FTP-Dienst hergestellt wird. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**ftp_port**||Die Nummer des Anschlusses für den FTP-Dienst des Verteilers. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**ftp_subdirectory**||Gibt an, wo die Momentaufnahme Dateien erstellt werden, wenn die Veröffentlichung das Weitergeben von Momentaufnahmen über FTP unterstützt Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
|**immediate_sync**|**true**|Die Synchronisierungsdateien für die Veröffentlichung werden bei jeder Ausführung des Momentaufnahme-Agents erstellt oder neu erstellt. Abonnenten können die Synchronisierungsdateien unmittelbar nach der Abonnierung erhalten, wenn der Momentaufnahme-Agent vor der Abonnierung abgeschlossen wurde. Neue Abonnements rufen die neuesten Synchronisierungsdateien ab, die von der letzten Ausführung des Momentaufnahmeagents generiert wurden. *independent_agent* muss ebenfalls " **true**" sein. Weitere Informationen zu **immediate_sync** finden Sie unten in den hinweisen.|  
||**false**|Synchronisierungsdateien werden nur erstellt, wenn neue Abonnements vorhanden sind. Abonnenten können die Synchronisierungs Dateien nach dem Abonnement erst empfangen, wenn die Momentaufnahmen-Agent gestartet und abgeschlossen wurde.|  
|**independent_agent**|**true**|Die Veröffentlichung verfügt über ihren eigenen dedizierten Verteilungs-Agent.|  
||**false**|Die Veröffentlichung verwendet einen freigegebenen Verteilungs-Agent, und jedes Paar aus Veröffentlichungsdatenbank und Abonnementdatenbank verfügt über einen freigegebenen Agent.|  
|**p2p_continue_onconflict**|**true**|Der Verteilungs-Agent setzt bei Erkennung eines Konflikts die Verarbeitung von Änderungen fort.<br /> **Vorsicht:** Es wird empfohlen, den Standardwert von zu verwenden `FALSE` . Wenn diese Option auf festgelegt ist `TRUE` , versucht der Verteilungs-Agent, Daten in der Topologie zusammenzuführen, indem die Konflikt verursachende Zeile von dem Knoten mit der höchsten Absender-ID angewendet wird. Bei dieser Methode ist keine Konvergenz garantiert. Sie sollten sicherstellen, dass die Topologie nach der Erkennung eines Konflikts konsistent ist. Weitere Informationen finden Sie im Abschnitt "Konfliktbehandlung" unter [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
||**false**|Der Verteilungs-Agent beendet bei Erkennung eines Konflikts die Verarbeitung von Änderungen.|  
|**post_snapshot_script**||Gibt den Speicherort einer Skriptdatei von [!INCLUDE[tsql](../../includes/tsql-md.md)] an, die der Verteilungs-Agent ausführt, nachdem alle anderen Skripts für replizierte Objekte und Daten während der Anfangssynchronisierung angewendet wurden.|  
|**pre_snapshot_script**||Gibt den Speicherort einer Skriptdatei von [!INCLUDE[tsql](../../includes/tsql-md.md)] an, die der Verteilungs-Agent ausführt, bevor alle anderen Skripts für replizierte Objekte und Daten während der Anfangssynchronisierung angewendet wurden.|  
|**publish_to_ActiveDirectory**|**true**|Dieser Parameter wurde als veraltet markiert und wird nur zum Sicherstellen der Abwärtskompatibilität von Skripts unterstützt. Für [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory ist das Hinzufügen von Veröffentlichungsinformationen nicht länger möglich.|  
||**false**|Entfernt die Veröffentlichungsinformationen aus Active Directory.|  
|**queue_type**|**sql**|Verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Speichern von Transaktionen. Diese Eigenschaft kann nur geändert werden, wenn keine aktiven Abonnements vorhanden sind.<br /><br /> Hinweis: die Unterstützung für die Verwendung von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing wurde eingestellt. Die Angabe eines Werts von **MSMQ** für *value* führt zu einem Fehler.|  
|**repl_freq**|**Continuous**|Veröffentlicht die Ausgabe aller protokollbasierten Transaktionen.|  
||**Überblick**|Veröffentlicht nur geplante Synchronisierungsereignisse.|  
|**replicate_ddl**|**1**|Auf dem Verleger ausgeführte Anweisungen der Datendefinitionssprache (DDL, Data Definition Language) werden repliziert. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden.|  
||**0**|DDL-Anweisungen werden nicht repliziert. Diese Eigenschaft kann für Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Veröffentlichungen nicht geändert werden. Die Replikation von Schemaänderungen kann nicht deaktiviert werden, wenn Peer-zu-Peer-Replikation verwendet wird.|  
|**replicate_partition_switch**|**true**|ALTER TABLE... Switch-Anweisungen, die für die veröffentlichte Datenbank ausgeführt werden, sollten auf Abonnenten repliziert werden. Diese Option ist nur gültig, wenn *allow_partition_switch* auf true festgelegt ist. Weitere Informationen finden Sie unter [Replicate Partitioned Tables and Indexes](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).|  
||**false**|ALTER TABLE... Switch-Anweisungen sollten nicht auf Abonnenten repliziert werden.|  
|**zurück**||**int** , der die Beibehaltungs Dauer für Abonnement Aktivitäten in Stunden darstellt. Wenn ein Abonnement innerhalb der Beibehaltungsdauer nicht aktiv ist, wird es entfernt.|  
|**snapshot_in_defaultfolder**|**true**|Momentaufnahmedateien werden im Standardmomentaufnahmeordner gespeichert. Wenn *alt_snapshot_folder* ebenfalls angegeben ist, werden Momentaufnahme Dateien sowohl am Standard Speicherort als auch an anderen Speicherorten gespeichert.|  
||**false**|Momentaufnahme Dateien werden an dem alternativen Speicherort gespeichert, der durch *alt_snapshot_folder* angegeben wird.|  
|**status**|**active**|Veröffentlichungsdaten sind für Abonnenten sofort beim Erstellen der Veröffentlichung verfügbar. Diese Option wird für Oracle-Verleger nicht unterstützt.|  
||**inactive**|Veröffentlichungsdaten sind für Abonnenten nicht beim Erstellen der Veröffentlichung verfügbar. Diese Option wird für Oracle-Verleger nicht unterstützt.|  
|**sync_method**|**native**|Verwendet beim Synchronisieren von Abonnements eine Massenkopierausgabe aller Tabellen im einheitlichen Modus.|  
||**character**|Verwendet beim Synchronisieren von Abonnements eine Massenkopierausgabe aller Tabellen im Zeichenmodus.|  
||**findende**|Verwendet eine Massenkopierprogramm-Ausgabe aller Tabellen im einheitlichen Modus, sperrt jedoch die Tabellen beim Generieren der Momentaufnahme nicht. Nicht für die Momentaufnahmereplikation gültig.|  
||**concurrent_c**|Verwendet eine Massenkopierprogramm-Ausgabe aller Tabellen im Zeichenmodus, sperrt jedoch die Tabellen beim Generieren der Momentaufnahme nicht. Nicht für die Momentaufnahmereplikation gültig.|  
|**TaskID**||Diese Eigenschaft wurde als veraltet markiert und wird nicht mehr unterstützt.|  
|**allow_drop**|**true**|Aktiviert `DROP TABLE` die DLL-Unterstützung für Artikel, die Teil der Transaktions Replikation sind. Unterstützte Mindestversion: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Service Pack 2 oder höher und [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Service Pack 1 oder höher. Zusätzlicher Verweis: [KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1)|
||**false**|Deaktiviert `DROP TABLE` die DLL-Unterstützung für Artikel, die Teil der Transaktions Replikation sind. Dies ist der **Standard** Wert für diese Eigenschaft.|
|**Null** (Standard)||Gibt die Liste der unterstützten Werte für die- *Eigenschaft* zurück.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Bestätigt, dass die von dieser gespeicherten Prozedur ausgeführte Aktion eine vorhandene Momentaufnahme für ungültig erklären kann. *force_invalidate_snapshot* ist ein **Bit**, der Standardwert ist **0**.  
  - der Wert **0** gibt an, dass Änderungen am Artikel nicht bewirken, dass die Momentaufnahme ungültig wird. Wenn die gespeicherte Prozedur erkennt, dass die Änderungen eine neue Momentaufnahme erfordern, tritt ein Fehler auf und es werden keine Änderungen vorgenommen.  
  - der Wert **1** gibt an, dass Änderungen am Artikel bewirken können, dass die Momentaufnahme ungültig wird. Wenn Abonnements vorhanden sind, die eine neue Momentaufnahme erfordern würden, wird mit diesem Wert die Berechtigung erteilt, die vorhandene Momentaufnahme als veraltet zu markieren und eine neue Momentaufnahme zu generieren.   
Weitere Informationen zu den Eigenschaften, bei deren Änderung die Generierung einer neuen Momentaufnahme erforderlich ist, finden Sie im Abschnitt "Hinweise".  
  
[**@force_reinit_subscription =** ] *force_reinit_subscription*  
 Bestätigt, dass die von dieser gespeicherten Prozedur ausgeführte Aktion möglicherweise erfordert, dass vorhandene Abonnements erneut initialisiert werden. *force_reinit_subscription* ist ein **Bit** mit einem Standardwert von **0**.  
  - der Wert **0** gibt an, dass Änderungen am Artikel nicht bewirken, dass das Abonnement erneut initialisiert wird. Wenn die gespeicherte Prozedur erkennt, dass die Änderung die Neuinitialisierung vorhandener Abonnements erfordert, tritt ein Fehler auf, und es werden keine Änderungen durchgeführt.  
  - der Wert **1** gibt an, dass Änderungen am Artikel bewirken, dass das vorhandene Abonnement erneut initialisiert wird, und erteilt die Berechtigung für die erneute Initialisierung des Abonnements.  
  
`[ @publisher = ] 'publisher'` Gibt einen nicht-- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verleger an. *Publisher* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
  > [!NOTE]  
  >  der *Verleger* sollte nicht verwendet werden, wenn Artikeleigenschaften auf einem Verleger geändert werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_changepublication** wird bei der Momentaufnahme-und Transaktions Replikation verwendet.  
  
 Nachdem Sie eine der folgenden Eigenschaften geändert haben, müssen Sie eine neue Momentaufnahme generieren, und Sie müssen den Wert **1** für den *force_invalidate_snapshot* -Parameter angeben.  
-   **alt_snapshot_folder**  
-   **compress_snapshot**  
-   **enabled_for_het_sub**  
-   **ftp_address**  
-   **ftp_login**  
-   **ftp_password**  
-   **ftp_port**  
-   **ftp_subdirectory**  
-   **post_snapshot_script**  
-   **pre_snapshot_script**  
-   **snapshot_in_defaultfolder**  
-   **sync_mode**  
  
Um Veröffentlichungs Objekte im Active Directory mithilfe des **publish_to_active_directory** -Parameters aufzulisten, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss das Objekt bereits im Active Directory erstellt werden.  
  
## <a name="impact-of-immediate-sync"></a>Auswirkungen von "immediate_sync"  
 Wenn immediate_sync aktiviert ist, werden alle Änderungen im Protokoll unmittelbar im Anschluss an die erste Momentaufnahme nachverfolgt, auch wenn keine Abonnements vorhanden sind. Protokollierte Änderungen werden verwendet, wenn ein Kunde eine Sicherung verwendet, um einen neuen Peer Knoten hinzuzufügen. Nachdem die Sicherung wieder hergestellt wurde, wird der Peer mit allen anderen Änderungen synchronisiert, die nach dem Erstellen der Sicherung durchgeführt wurden. Da die Befehle in der Verteilungs Datenbank nachverfolgt werden, kann die Synchronisierungs Logik die letzte gesicherte LSN sehen und diese als Ausgangspunkt verwenden. dabei ist zu erkennen, dass der Befehl verfügbar ist, wenn die Sicherung innerhalb der maximalen Beibehaltungs Dauer erstellt wurde. (Der Standardwert für die minimale Beibehaltungs Dauer beträgt 0 Std. die maximale Beibehaltungs Dauer beträgt 24 Stunden.)  
  
 Wenn immediate_sync deaktiviert ist, werden die Änderungen mindestens über die minimale Beibehaltungsdauer vorgehalten und sofort für alle bereits replizierten Transaktionen bereinigt. Wenn immediate_sync deaktiviert und mit der Standardbeibehaltungsdauer konfiguriert ist, ist es wahrscheinlich, dass die erforderlichen Änderungen nach der Sicherung bereinigt wurden und dass der neue Peerknoten nicht ordnungsgemäß initialisiert wird. Die einzige Option besteht darin, die Topologie in einen inaktiven Zustand zu versetzen. Wenn Sie immediate_synch aktivieren, erhöht sich die Flexibilität. Darüber hinaus ist dies die empfohlene Einstellung für P2P-Replikationen.  
  
## <a name="example"></a>Beispiel  
 [!code-sql[HowTo#sp_changepublication](../../relational-databases/replication/codesnippet/tsql/sp-changepublication-tra_1.sql)]  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** oder der festen Daten Bank Rolle **db_owner** können **sp_changepublication** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen und Ändern von Veröffentlichungseigenschaften](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Ändern von Veröffentlichungs- und Artikeleigenschaften](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addpublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [Gespeicherte Automatisierungsprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
