---
description: sys.event_log (Azure SQL-Datenbank)
title: sys.event_log (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- event_log
- sys.event_log_TSQL
- event_log_TSQL
- sys.event_log
dev_langs:
- TSQL
helpviewer_keywords:
- event_log
- sys.event_log
ms.assetid: ad5496b5-e5c7-4a18-b5a0-3f985d7c4758
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: c3a358e23d9428440166dd98a0f29ca8e4ea11c5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093098"
---
# <a name="sysevent_log-azure-sql-database"></a>sys.event_log (Azure SQL-Datenbank)

[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Gibt erfolgreiche [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Datenbankverbindungen, Verbindungsfehler und Deadlocks zurück. Sie können diese Informationen verwenden, um die Datenbankaktivität mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] nachzuverfolgen oder um Fehler zu beheben.  
  
> [!CAUTION]  
> Bei Installationen, die eine große Anzahl von Datenbanken oder eine große Anzahl von Anmeldungen aufweisen, kann die Aktivität in sys.event_log zu Leistungseinschränkungen führen, eine hohe CPU-Auslastung und möglicherweise zu Anmelde Fehlern führen. Abfragen von sys.event_log können zum Problem beitragen. Microsoft arbeitet daran, dieses Problem zu beheben. Beschränken Sie in der Zwischenzeit Abfragen von sys.event_log, um die Auswirkungen dieses Problems zu verringern. Benutzer des newrelic-SQL Server-Plug-ins sollten Microsoft Azure SQL-Datenbank Plug-in- [Optimierungs & Leistungs Anpassungen](https://discuss.newrelic.com/t/microsoft-azure-sql-database-plugin-tuning-performance-tweaks/30729) für zusätzliche Konfigurationsinformationen besuchen.  
  
 Die `sys.event_log`-Sicht enthält die folgenden Spalten.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Der Name der Datenbank. Wenn die Verbindung nicht hergestellt werden kann und der Benutzer keinen Datenbanknamen angegeben hat, ist diese Spalte leer.|  
|**start_time**|**datetime2**|UTC-Datum und -Zeit des Beginns des Aggregationsintervalls. Für aggregierte Ereignisse ist die Zeit immer ein Vielfaches von 5 Minuten. Beispiel:<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|UTC-Datum und -Zeit des Endes des Aggregationsintervalls. Bei aggregierten Ereignissen ist **End_time** immer genau 5 Minuten später als die entsprechende **start_time** in derselben Zeile. Bei Ereignissen, die nicht aggregiert werden, **start_time** und **end_time** dem tatsächlichen UTC-Datum und der tatsächlichen UTC-Uhrzeit des Ereignisses entsprechen.|  
|**event_category**|**nvarchar (64)**|Die Komponente auf hoher Ebene, die dieses Ereignis generiert hat.<br /><br /> Eine Liste möglicher Werte finden Sie unter [Ereignis Typen](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_type**|**nvarchar (64)**|Art des Ereignisses.<br /><br /> Eine Liste möglicher Werte finden Sie unter [Ereignis Typen](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_subtype**|**int**|Der Untertyp des eintretenden Ereignisses.<br /><br /> Eine Liste möglicher Werte finden Sie unter [Ereignis Typen](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**event_subtype_desc**|**nvarchar (64)**|Die Beschreibung des Ereignisuntertyps.<br /><br /> Eine Liste möglicher Werte finden Sie unter [Ereignis Typen](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**severity**|**int**|Der Schweregrad des Fehlers. Mögliche Werte:<br /><br /> 0 = Information<br />1 = Warning<br />2 = Fehler|  
|**event_count**|**int**|Gibt an, wie oft dieses Ereignis für die angegebene Datenbank innerhalb des angegebenen Zeitintervalls eingetreten ist (**start_time** und **end_time**).|  
|**description**|**nvarchar(max)**|Detaillierte Beschreibung des Ereignisses.<br /><br /> Eine Liste möglicher Werte finden Sie unter [Ereignis Typen](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) .|  
|**additional_data**|**XML**|*Hinweis: dieser Wert ist für Azure SQL-Datenbank V12 immer NULL. Weitere Informationen zum Abrufen von Deadlockereignissen für V12 finden Sie im Abschnitt " [Beispiele](#Deadlock) ".*<br /><br /> Bei **Deadlock** -Ereignissen enthält diese Spalte das Deadlockdiagramm. Bei anderen Ereignistypen enthält diese Spalte NULL. |  
  
##  <a name="event-types"></a><a name="EventTypes"></a> Ereignis Typen

 Die Ereignisse, die von jeder Zeile in dieser Ansicht aufgezeichnet werden, werden durch eine Kategorie (**event_category**), einen Ereignistyp (**event_type**) und einen Untertyp (**event_subtype**) identifiziert. In der folgenden Tabelle werden die Ereignistypen aufgeführt, die in dieser Sicht gesammelt werden.  
  
 Für Ereignisse in der **konnektivitätskategorie** sind zusammenfassende Informationen in der sys.database_connection_stats Ansicht verfügbar.  
  
> [!NOTE]  
> Diese Sicht enthält nicht alle [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Datenbankereignisse, die eintreten können, sondern nur die hier aufgeführten. Zusätzliche Kategorien, Ereignistypen und Untertypen werden in zukünftigen Versionen von [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ggf. hinzugefügt.  
  
|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**description**|  
|-------------------------|---------------------|------------------------|------------------------------|------------------|---------------------|  
|**Stech**|**connection_successful**|0|**connection_successful**|0|Die Verbindung mit der Datenbank war erfolgreich.|  
|**Stech**|**connection_failed**|0|**invalid_login_name**|2|Der Anmeldename ist in dieser SQL Server-Version nicht gültig.|  
|**Stech**|**connection_failed**|1|**windows_auth_not_supported**|2|Windows-Anmeldungen werden in dieser SQL Server-Version nicht unterstützt.|  
|**Stech**|**connection_failed**|2|**attach_db_not_supported**|2|Der Benutzer hat das Anfügen einer Datenbankdatei angefordert, die nicht unterstützt wird.|  
|**Stech**|**connection_failed**|3|**change_password_not_supported**|2|Der Benutzer hat angefordert, das Kennwort des angemeldeten Benutzers zu ändern. Dies wird nicht unterstützt.|  
|**Stech**|**connection_failed**|4|**login_failed_for_user**|2|Fehler bei der Anmeldung für den Benutzer.|  
|**Stech**|**connection_failed**|5|**login_disabled**|2|Die Anmeldung wurde deaktiviert.|  
|**Stech**|**connection_failed**|6|**failed_to_open_db**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Datenbank konnte nicht geöffnet werden. Die Ursache hierfür kann darin liegen, dass die Datenbank nicht vorhanden ist oder keine Authentifizierung zum Öffnen der Datenbank vorhanden ist.|  
|**Stech**|**connection_failed**|7|**blocked_by_firewall**|2|Client-IP-Adresse ist nicht berechtigt, auf den Server zuzugreifen.|  
|**Stech**|**connection_failed**|8|**client_close**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Möglicher Timeout beim Client, als die Verbindung hergestellt wurde. Versuchen Sie, das Verbindungstimeout zu erhöhen.|  
|**Stech**|**connection_failed**|9|**Neukonfiguration**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Verbindungsfehler, da die Datenbank zu diesem Zeitpunkt eine Neukonfiguration durchlaufen hat.|  
|**Stech**|**connection_terminated**|0|**idle_connection_timeout**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Verbindung ist länger im Leerlauf, als der vom System definierte Schwellenwert angibt.|  
|**Stech**|**connection_terminated**|1|**Neukonfiguration**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Sitzung wurde aufgrund einer Neukonfiguration der Datenbank beendet.|  
|**Stech**|**Drosselung**|*\<reason code>*|**reason_code**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Anforderung wird gedrosselt.  Ursachen Code für Drosselung: *\<reason code>* . Weitere Informationen finden Sie unter [Engine-Drosselung](/previous-versions/azure/dn338079(v=azure.100)).|  
|**Stech**|**throttling_long_transaction**|40549|**long_transaction**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Sitzung wird aufgrund einer Transaktion mit langer Laufzeit beendet. Verkürzen Sie die Transaktion. Weitere Informationen finden Sie unter [Ressourcen Limits](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Stech**|**throttling_long_transaction**|40550|**excessive_lock_usage**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Sitzung wurde beendet, da zu viele Sperren abgerufen wurden. Reduzieren Sie die Anzahl der in einer einzelnen Transaktion gelesenen oder geänderten Zeilen. Weitere Informationen finden Sie unter [Ressourcen Limits](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Stech**|**throttling_long_transaction**|40551|**excessive_tempdb_usage**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Sitzung wurde aufgrund übermäßiger TEMPDB-Auslastung beendet. Ändern Sie die Abfrage, um die Verwendung des temporären Tabellenbereichs zu verringern. Weitere Informationen finden Sie unter [Ressourcen Limits](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Stech**|**throttling_long_transaction**|40552|**excessive_log_space_usage**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Sitzung wurde aufgrund übermäßiger Verwendung des Speicherplatzes für das Transaktionsprotokoll beendet. Reduzieren Sie die Anzahl der in einer einzelnen Transaktion geänderten Zeilen. Weitere Informationen finden Sie unter [Ressourcen Limits](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Stech**|**throttling_long_transaction**|40553|**excessive_memory_usage**|2|*Hinweis: gilt nur für Azure SQL-Datenbank v11.*<br /><br /> Die Sitzung wurde aufgrund übermäßiger Speicherauslastung beendet. Ändern Sie die Abfrage, damit weniger Zeilen verarbeitet werden. Weitere Informationen finden Sie unter [Ressourcen Limits](/previous-versions/azure/dn338081(v=azure.100)).|  
|**engine**|**deadlock**|0|**deadlock**|2|Deadlock ist aufgetreten.|  
  
## <a name="permissions"></a>Berechtigungen

 Benutzer, die über die Berechtigung zum Zugriff auf die **Master** -Datenbank verfügen, haben schreibgeschützten Zugriff auf diese Sicht.  
  
## <a name="remarks"></a>Bemerkungen  
  
### <a name="event-aggregation"></a>Ereignisaggregation

 Die Ereignisinformationen für diese Sicht werden gesammelt und innerhalb von 5-minütigen Intervallen aggregiert. Die Spalte **event_count** gibt an, wie oft ein bestimmter **event_type** und **event_subtype** für eine bestimmte Datenbank innerhalb eines bestimmten Zeitintervalls aufgetreten sind.  
  
> [!NOTE]  
> Einige Ereignisse wie Deadlocks werden nicht aggregiert. Für diese Ereignisse werden **event_count** 1 und **start_time** und **end_time** gleich dem tatsächlichen UTC-Datum und der tatsächlichen UTC-Uhrzeit des Ereignisses sein.  
  
 Wenn ein Benutzer zum Beispiel aufgrund eines ungültigen Anmeldenamens sieben Mal zwischen 11:00 und 11:05 Uhr am 05.02.2012 (UTC) keine Verbindung mit der Datenbank Database1 herstellen kann, sind diese Informationen in dieser Sicht in einer einzelnen Zeile verfügbar:  
  
|**database_name**|**start_time**|**end_time**|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**event_count**|**description**|**additional_data**|  
|------------------------|---------------------|-------------------|-------------------------|---------------------|------------------------|------------------------------|------------------|----------------------|---------------------|--------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`connectivity`|`connection_failed`|`4`|`login_failed_for_user`|`2`|`7`|`Login failed for user.`|`NULL`|  
  
### <a name="interval-start_time-and-end_time"></a>start_time und end_time des Intervalls  
 Ein Ereignis ist in einem Aggregations Intervall enthalten, wenn das Ereignis *am* oder _nach_**start_time** und _vor_**end_time** für dieses Intervall auftritt. Beispielsweise würde ein Ereignis, das genau zum Zeitpunkt `2012-10-30 19:25:00.0000000` eintritt, nur im zweiten unten gezeigten Intervall aufgenommen werden:  
  
```
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```

### <a name="data-updates"></a>Datenupdates

 Die Daten in dieser Sicht werden im Zeitverlauf gesammelt. Normalerweise werden die Daten innerhalb einer Stunde nach Beginn des Aggregationsintervalls gesammelt, es kann aber maximal 24 Stunden dauern, bis alle Daten in der Sicht angezeigt werden. Während dieser Zeit werden die Informationen in einer einzelnen Zeile möglicherweise gelegentlich aktualisiert.  
  
### <a name="data-retention"></a>Datenaufbewahrung

 Die Daten in dieser Sicht werden maximal 30 Tage lang aufbewahrt, abhängig von der Anzahl der Datenbanken und der Anzahl der von jeder Datenbank generierten eindeutigen Ereignisse. Um diese Informationen für einen längeren Zeitraum beizubehalten, kopieren Sie die Daten in eine separate Datenbank. Nachdem Sie eine erste Kopie der Sicht erstellt haben, werden die Zeilen in der Sicht möglicherweise aktualisiert, während die Daten gesammelt werden. Damit die Kopie der Daten aktuell bleibt, führen Sie regelmäßig einen Tabellenscan der Zeilen aus, um nach einer Erhöhung der Ereignisanzahl für vorhandene Zeilen zu suchen und um neue Zeilen zu ermitteln (eindeutige Zeilen bestimmen Sie anhand der Start- und Endzeiten). Aktualisieren Sie dann die Kopie der Daten mit diesen Änderungen.  
  
### <a name="errors-not-included"></a>Fehler nicht enthalten

 Diese Sicht enthält möglicherweise nicht alle Verbindungs- und Fehlerinformationen:  
  
- Diese Sicht enthält nicht alle [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Datenbankfehler, die auftreten können, sondern nur die, die in den [Ereignis Typen](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) in diesem Thema angegeben sind.  
- Wenn ein Computerfehler innerhalb des [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Rechenzentrums auftritt, fehlt in der Ereignis Tabelle möglicherweise eine kleine Menge von Daten.  
- Wenn eine IP-Adresse von DoSGuard blockiert wurde, können Verbindungsversuchsereignisse von dieser IP-Adresse nicht gesammelt werden. Diese werden in dieser Sicht nicht angezeigt.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="simple-examples"></a>Einfache Beispiele

 Die folgende Abfrage gibt alle Ereignisse zurück, die zwischen 12 Uhr mittags am 25.09.2011 und 12 Uhr mittags am 28.09.2011 (UTC) eingetreten sind. Standardmäßig werden Abfrageergebnisse nach **start_time** (aufsteigender Reihenfolge) sortiert.  

```sql
SELECT * FROM sys.event_log
WHERE start_time >= '2011-09-25 12:00:00'
    AND end_time <= '2011-09-28 12:00:00';  
```

Die folgende Abfrage gibt alle Deadlockereignisse für die Datenbank Database1 zurück (gilt nur für Azure SQL-Datenbank v11).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'deadlock'
    AND database_name = 'Database1';  
```

<a name="Deadlock"></a> Die folgende Abfrage gibt alle Deadlockereignisse für die Datenbank Database1 zurück (gilt nur für Azure SQL-Datenbank V12).  

```sql
WITH CTE AS (  
       SELECT CAST(event_data AS XML)  AS [target_data_XML]  
   FROM sys.fn_xe_telemetry_blob_target_read_file('dl', null, null, null)  
)  
SELECT target_data_XML.value('(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
target_data_XML.query('/event/data[@name=''xml_report'']/value/deadlock') AS deadlock_xml,  
target_data_XML.query('/event/data[@name=''database_name'']/value').value('(/value)[1]', 'nvarchar(100)') AS db_name  
FROM CTE  
```

Die folgende Abfrage gibt harte Drosselungen für SQL-Arbeitsthreadereignisse zurück, die zwischen 10:00 und 11:00 Uhr am 25.09.2011 (UTC) eingetreten sind.  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'throttling'
    AND event_subtype = 4194307
    AND start_time >= '2011-09-25 10:00:00'
    AND end_time <= '2011-09-25 11:00:00';  
```

### <a name="db-scoped-extended-event"></a>Erweiterte Ereignis DB-Scoped

 Verwenden Sie den folgenden Beispielcode, um die Sitzung für erweiterte Ereignisse (XEvent) mit DB-Gültigkeitsbereich einzurichten:  

```sql
IF EXISTS  
    (SELECT * from sys.database_event_sessions  
        WHERE name = 'azure_monitor_deadlock_session')  
BEGIN  
    ALTER EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE  
        DROP TARGET package0.ring_buffer;  
  
    DROP EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE;  
END  
  
CREATE EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    ADD EVENT sqlserver.database_xml_deadlock_report  
    ADD TARGET package0.ring_buffer  
    (  
        SET max_memory = 2048, max_events_limit = 10  
    )  
    WITH (STARTUP_STATE = ON,  
          EVENT_RETENTION_MODE = ALLOW_SINGLE_EVENT_LOSS);  
  
ALTER EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    STATE = START;  
```

### <a name="check-for-deadlock"></a>Auf Deadlock überprüfen

Verwenden Sie die folgende Abfrage, um zu überprüfen, ob ein Deadlock vorliegt.  

```sql
WITH CTE AS (  
    SELECT CAST(xet.target_data AS XML)  AS [target_data_XML]  
        FROM            sys.dm_xe_database_session_targets AS xet  
             INNER JOIN sys.dm_xe_database_sessions        AS xe  
                 ON (xe.address = xet.event_session_address)  
        WHERE xe.name = 'azure_monitor_deadlock_session'  
)  
, CTE2 AS (  
    SELECT  
            T2.EventData.query('.').value(  
                '(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
            T2.EventData.query('.').query(  
                '(/event/data/value/deadlock)[1]')     AS deadlock_xml  
        FROM CTE  
            CROSS Apply [target_data_XML].nodes(  
                '/RingBufferTarget/event') AS T2(EventData)  
)  
SELECT * FROM CTE2;  
```

## <a name="see-also"></a>Weitere Informationen

 [Erweiterte Ereignisse in Azure SQL-Datenbank](/azure/azure-sql/database/xevent-db-diff-from-svr)  
