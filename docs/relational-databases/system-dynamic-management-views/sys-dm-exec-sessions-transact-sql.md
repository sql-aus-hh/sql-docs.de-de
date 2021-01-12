---
description: sys.dm_exec_sessions (Transact-SQL)
title: sys.dm_exec_sessions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_sessions_TSQL
- sys.dm_exec_sessions
- dm_exec_sessions
- sys.dm_exec_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sessions dynamic management view
ms.assetid: 2b7e8e0c-eea0-431e-819f-8ccd12ec8cfa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 49f04a21e95c0cc7a63b97fd9fd4add1729b25a7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098974"
---
# <a name="sysdm_exec_sessions-transact-sql"></a>sys.dm_exec_sessions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine Zeile pro authentifizierter Sitzung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück. sys.dm_exec_sessions ist eine Sicht des Serverbereichs mit Informationen zu allen aktiven Benutzerverbindungen und internen Tasks. Zu diesen Informationen zählen u. a. die Clientversion, der Name des Clientprogramms, die Clientanmeldezeit, der angemeldete Benutzer und die aktuelle Sitzungseinstellung. Mit sys.dm_exec_sessions zeigen Sie zuerst die aktuelle Systemauslastung an und identifizieren eine interessante Sitzung, und informieren Sie sich dann in dynamischen Verwaltungssichten oder dynamischen Verwaltungsfunktionen weiter über diese Sitzung.  
  
 Die sys.dm_exec_connections, sys.dm_exec_sessions und sys.dm_exec_requests dynamischen Verwaltungs Sichten werden der [sys.sysProzesse](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) -Systemtabelle zugeordnet.  
  
> **Hinweis:** Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_exec_sessions**.  
  
|Spaltenname|Datentyp|Beschreibung und versionsspezifische Informationen|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifiziert die einer aktiven primären Verbindung zugeordnete Sitzung. Lässt keine NULL-Werte zu.|  
|login_time|**datetime**|Uhrzeit, zu der die Sitzung eingerichtet wurde. Lässt keine NULL-Werte zu. Sitzungen, bei denen die Anmeldung nicht abgeschlossen ist, wenn diese DMV abgefragt wird, werden mit einer Anmeldezeit von angezeigt `1900-01-01` .|  
|host_name|**nvarchar(128)**|Name der für eine Sitzung spezifischen Clientarbeitsstation. Der Wert ist für interne Sitzungen NULL. Lässt NULL-Werte zu.<br /><br /> **Sicherheitshinweis:** Die Client Anwendung stellt den Namen der Arbeitsstation bereit und kann ungenaue Daten bereitstellen. Verwenden Sie HOST_NAME nicht als Sicherheitsfunktion.|  
|program_name|**nvarchar(128)**|Name des Clientprogramms, mit dem die Sitzung initiiert wurde. Der Wert ist für interne Sitzungen NULL. Lässt NULL-Werte zu.|  
|host_process_id|**int**|Prozess-ID des Clientprogramms, mit dem die Sitzung initiiert wurde. Der Wert ist für interne Sitzungen NULL. Lässt NULL-Werte zu.|  
|client_version|**int**|Die vom Client für die Verbindung mit dem Server verwendete TDS-Protokollversion der Schnittstelle. Der Wert ist für interne Sitzungen NULL. Lässt NULL-Werte zu.|  
|client_interface_name|**nvarchar(32)**|Der Name der Bibliothek/des Treibers, der vom Client für die Kommunikation mit dem Server verwendet wird. Der Wert ist für interne Sitzungen NULL. Lässt NULL-Werte zu.|  
|security_id|**varbinary(85)**|Microsoft Windows-Sicherheits-ID, die der Anmeldung zugeordnet ist. Lässt keine NULL-Werte zu.|  
|login_name|**nvarchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldename, unter dem die Sitzung gegenwärtig ausgeführt wird. Den ursprünglichen Anmeldenamen, mit dem die Sitzung erstellt wurde, finden Sie unter original_login_name. Kann ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentifizierter Anmelde Name oder ein authentifizierter Windows-Domänen Benutzername sein. Lässt keine NULL-Werte zu.|  
|nt_domain|**nvarchar(128)**|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> Die Windows-Domäne für den Client, wenn für die Sitzung die Windows-Authentifizierung oder eine vertrauenswürdige Verbindung verwendet wird. Dieser Wert ist für interne Sitzungen und andere Benutzer als Domänenbenutzer NULL. Lässt NULL-Werte zu.|  
|nt_user_name|**nvarchar(128)**|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> Der Windows-Benutzername für den Client, wenn für die Sitzung die Windows-Authentifizierung oder eine vertrauenswürdige Verbindung verwendet wird. Dieser Wert ist für interne Sitzungen und andere Benutzer als Domänenbenutzer NULL. Lässt NULL-Werte zu.|  
|status|**nvarchar(30)**|Status der Sitzung. Mögliche Werte:<br /><br /> **Running** – Aktuell wird mindestens eine Anforderung ausgeführt.<br /><br /> **Sleeping** – Aktuell werden keine Anforderungen ausgeführt.<br /><br /> **Ruhende** Sitzung wurde aufgrund von Verbindungspooling zurückgesetzt und befindet sich jetzt im Zustand vor der Anmeldung.<br /><br /> **Preconnect**– Die Sitzung ist in der Klassifizierungsfunktion der Ressourcenkontrolle.<br /><br /> Lässt keine NULL-Werte zu.|  
|context_info|**varbinary(128)**|CONTEXT_INFO-Wert für die Sitzung. Die Kontextinformationen werden vom Benutzer mithilfe der [SET CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) -Anweisung festgelegt. Lässt NULL-Werte zu.|  
|cpu_time|**int**|Die von der Sitzung verwendete CPU-Zeit in Millisekunden. Lässt keine NULL-Werte zu.|  
|memory_usage|**int**|Anzahl der von der Sitzung verwendeten 8 KB-Speicherseiten. Lässt keine NULL-Werte zu.|  
|total_scheduled_time|**int**|Gesamtzeit in Millisekunden, die für die Ausführung der Sitzung (sowie der darin enthaltenen Anforderungen) eingeplant wurde. Lässt keine NULL-Werte zu.|  
|total_elapsed_time|**int**|Zeit in Millisekunden seit dem Einrichten der Sitzung. Lässt keine NULL-Werte zu.|  
|endpoint_id|**int**|ID des Endpunktes, der der Sitzung zugeordnet ist. Lässt keine NULL-Werte zu.|  
|last_request_start_time|**datetime**|Uhrzeit, zu der die letzte Anforderung in der Sitzung gestartet wurde. Dies schließt die derzeit ausgeführte Anforderung ein. Lässt keine NULL-Werte zu.|  
|last_request_end_time|**datetime**|Uhrzeit, zu der eine Anforderung in der Sitzung zuletzt abgeschlossen wurde. Lässt NULL-Werte zu.|  
|Lesevorgänge|**bigint**|Anzahl von Lesevorgängen von Anforderungen in dieser Sitzung oder während dieser Sitzung. Lässt keine NULL-Werte zu.|  
|Schreibvorgänge|**bigint**|Anzahl der Schreibvorgänge von Anforderungen in dieser Sitzung oder während dieser Sitzung. Lässt keine NULL-Werte zu.|  
|logical_reads|**bigint**|Anzahl von logischen Lesevorgängen in der Sitzung. Lässt keine NULL-Werte zu.|  
|is_user_process|**bit**|0, wenn es sich um eine Systemsitzung handelt. Andernfalls ist der Wert 1. Lässt keine NULL-Werte zu.|  
|text_size|**int**|TEXTSIZE-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|language|**nvarchar(128)**|LANGUAGE-Einstellung für die Sitzung. Lässt NULL-Werte zu.|  
|date_format|**nvarchar (3)**|DATEFORMAT-Einstellung für die Sitzung. Lässt NULL-Werte zu.|  
|date_first|**smallint**|DATEFIRST-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|quoted_identifier|**bit**|QUOTED_IDENTIFIER-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|arithabort|**bit**|ARITHABORT-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|ansi_null_dflt_on|**bit**|ANSI_NULL_DFLT_ON-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|ansi_defaults|**bit**|ANSI_DEFAULTS-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|ansi_warnings|**bit**|ANSI_WARNINGS-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|ansi_padding|**bit**|ANSI_PADDING-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|ansi_nulls|**bit**|ANSI_NULLS-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|concat_null_yields_null|**bit**|CONCAT_NULL_YIELDS_NULL-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|transaction_isolation_level|**smallint**|Isolationsstufe für Transaktionen der Sitzung.<br /><br /> 0 = Unspecified<br /><br /> 1 = nicht committet<br /><br /> 2 = ReadCommitted<br /><br /> 3 = RepeatableRead<br /><br /> 4 = Serializable<br /><br /> 5 = Momentaufnahme<br /><br /> Lässt keine NULL-Werte zu.|  
|lock_timeout|**int**|LOCK_TIMEOUT-Einstellung für die Sitzung. Der Wert ist in Millisekunden angegeben. Lässt keine NULL-Werte zu.|  
|deadlock_priority|**int**|DEADLOCK_PRIORITY-Einstellung für die Sitzung. Lässt keine NULL-Werte zu.|  
|row_count|**bigint**|Anzahl der bisher in der Sitzung zurückgegebenen Zeilen. Lässt keine NULL-Werte zu.|  
|prev_error|**int**|ID des letzten in der Sitzung zurückgegebenen Fehlers. Lässt keine NULL-Werte zu.|  
|original_security_id|**varbinary(85)**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Sicherheits-ID, die original_login_name zugeordnet ist. Lässt keine NULL-Werte zu.|  
|original_login_name|**nvarchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldename, der vom Client zum Erstellen dieser Sitzung verwendet wurde. Kann ein authentifizierter Anmeldename von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ein authentifizierter Benutzername einer Windows-Domäne oder ein Benutzer einer eigenständigen Datenbank sein. Beachten Sie, dass für die Sitzung nach der ersten Verbindung möglicherweise viele implizite oder explizite Kontextwechsel erfolgt sind. Wenn z. b. [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) verwendet wird. Lässt keine NULL-Werte zu.|  
|last_successful_logon|**datetime**|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> Zeitpunkt der letzten erfolgreichen Anmeldung für original_login_name, bevor die aktuelle Sitzung gestartet wurde.|  
|last_unsuccessful_logon|**datetime**|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> Zeitpunkt des letzten nicht erfolgreichen Anmeldeversuchs für original_login_name, bevor die aktuelle Sitzung gestartet wurde.|  
|unsuccessful_logons|**bigint**|**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.<br /><br /> Anzahl der nicht erfolgreichen Anmeldeversuche für original_login_name zwischen last_successful_logon und login_time.|  
|group_id|**int**|ID der Arbeitsauslastungsgruppe, zu der diese Sitzung gehört. Lässt keine NULL-Werte zu.|  
|database_id|**smallint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Die ID der aktuellen Datenbank für jede Sitzung.|  
|authenticating_database_id|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> ID der Datenbank, die den Prinzipal authentifiziert. Bei Anmeldungen beträgt der Wert 0. Für Benutzer von eigenständigen Datenbanken ist der Wert die Datenbank-ID der eigenständigen Datenbank.|  
|open_transaction_count|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Die Anzahl der offenen Transaktionen pro Sitzung.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
|page_server_reads|**bigint**|**Gilt für**: hyperskalierung von Azure SQL-Datenbank<br /><br /> Anzahl von Seiten Server Lesevorgängen, die während dieser Sitzung von Anforderungen in dieser Sitzung ausgeführt werden. Lässt keine NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
Jeder Benutzer kann seine eigenen Sitzungsinformationen sehen.  
**[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] :** Erfordert `VIEW SERVER STATE` die-Berechtigung für [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , um alle Sitzungen auf dem Server anzuzeigen.  
**[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] :** Erfordert `VIEW DATABASE STATE` , dass alle Verbindungen mit der aktuellen Datenbank angezeigt werden. `VIEW DATABASE STATE` kann nicht in der Datenbank erteilt werden `master` . 
  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn die Server Konfigurationsoption **Common Criteria-Konformität aktiviert** aktiviert ist, werden die Anmelde Statistiken in den folgenden Spalten angezeigt.  
  
-   last_successful_logon  
  
-   last_unsuccessful_logon  
  
-   unsuccessful_logons  
  
 Ist diese Option nicht aktiviert, geben diese Spalten NULL-Werte zurück. Weitere Informationen zum Festlegen dieser Server Konfigurationsoption finden Sie unter [Common Criteria-Kompatibilität aktiviert (Server Konfigurationsoption](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)).  
 
 
 Für die Administrator Verbindungen in Azure SQL-Datenbank wird eine Zeile pro authentifizierter Sitzung angezeigt. Die Sitzungen "SA", die im Resultset angezeigt werden, haben keine Auswirkung auf das Benutzer Kontingent für Sitzungen. Bei nicht-admin-Verbindungen werden nur Informationen zu ihren Datenbankbenutzer Sitzungen angezeigt.
 
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Für/Anwendung|Relationship|  
|----------|--------|---------------|------------------|  
|sys.dm_exec_sessions|[sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|session_id|1:0 oder 1:viele|  
|sys.dm_exec_sessions|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)|session_id|1:0 oder 1:viele|  
|sys.dm_exec_sessions|[sys.dm_tran_session_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)|session_id|1:0 oder 1:viele|  
|sys.dm_exec_sessions|[sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)(session_id &#124; 0)|session_id CROSS APPLY<br /><br /> OUTER APPLY|1:0 oder 1:viele|  
|sys.dm_exec_sessions|[sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)|session_id|1:1|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-finding-users-that-are-connected-to-the-server"></a>A. Ermitteln der Benutzer, die mit dem Server verbunden sind  
 Im folgenden Beispiel werden die Benutzer ermittelt, die mit dem Server verbunden sind, und die Anzahl der Sitzungen für die einzelnen Benutzer zurückgegeben.  
  
```sql  
SELECT login_name ,COUNT(session_id) AS session_count   
FROM sys.dm_exec_sessions   
GROUP BY login_name;  
```  
  
### <a name="b-finding-long-running-cursors"></a>B. Suchen von Cursorn mit langer Ausführungszeit  
 Im folgenden Beispiel werden die Cursor, die seit einer längeren Zeit als der angegebenen geöffnet sind, die Benutzer, die die Cursor erstellt haben, und die Sitzungen, in denen die Cursor verwendet werden, gesucht.  
  
```sql  
USE master;  
GO  
SELECT creation_time ,cursor_id   
    ,name ,c.session_id ,login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s   
   ON c.session_id = s.session_id   
WHERE DATEDIFF(mi, c.creation_time, GETDATE()) > 5;  
```  
  
### <a name="c-finding-idle-sessions-that-have-open-transactions"></a>C. Suchen von Sitzungen im Leerlauf, für die Transaktionen geöffnet sind  
 Im folgenden Beispiel werden Sitzungen gesucht, für die Transaktionen geöffnet sind und die sich im Leerlauf befinden. Sitzungen, für die derzeit keine Anforderung ausgeführt wird, befinden sich im Leerlauf.  
  
```sql  
SELECT s.*   
FROM sys.dm_exec_sessions AS s  
WHERE EXISTS   
    (  
    SELECT *   
    FROM sys.dm_tran_session_transactions AS t  
    WHERE t.session_id = s.session_id  
    )  
    AND NOT EXISTS   
    (  
    SELECT *   
    FROM sys.dm_exec_requests AS r  
    WHERE r.session_id = s.session_id  
    );  
```  
  
### <a name="d-finding-information-about-a-queries-own-connection"></a>D: Suchen nach Informationen über die eigene Verbindung einer Abfrage  
 Typische Abfrage zum Sammeln von Informationen über die eigene Verbindung einer Abfrage.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



