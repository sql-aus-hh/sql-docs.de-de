---
description: ALTER DATABASE (Transact-SQL)
title: ALTER DATABASE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: references_regions
ms.date: 10/30/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_DATABASE_TSQL
- ALTER DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- databases [SQL Server], modifying
- ALTER DATABASE statement
- databases [SQL Server], renaming
- renaming databases
- database modifications [SQL Server]
- ALTER DATABASE statement, syntax described
- database names [SQL Server], ALTER DATABASE
- modifying databases
- collations [SQL Server], modifying
- database mirroring [SQL Server], Transact-SQL
ms.assetid: 15f8affd-8f39-4021-b092-0379fc6983da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: 9086c0e4dcda0a98daad3e372e719bde7fc628d7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099508"
---
# <a name="alter-database-transact-sql"></a>ALTER DATABASE (Transact-SQL)

Ändern bestimmter Konfigurationsoptionen einer Datenbank

Dieser Artikel stellt die Syntax, Argumente, Anweisungen, Berechtigungen und Beispiele für das SQL-Produkt Ihrer Wahl bereit.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Übersicht: SQL Server

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ändert diese Anweisung eine Datenbank bzw. die zu dieser Datenbank gehörenden Dateien und Dateigruppen. Fügt einer Datenbank Dateien und Dateigruppen hinzu oder entfernt diese, ändert die Attribute einer Datenbank oder ihrer Dateien und Dateigruppen, ändert die Datenbanksortierung und legt Datenbankoptionen fest. Datenbankmomentaufnahmen können nicht geändert werden. Verwenden Sie zum Ändern von Datenbankoptionen für die Replikation [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).

Aufgrund ihrer Länge wird die `ALTER DATABASE`-Syntax in mehrere Artikel aufgeteilt.

ALTER DATABASE   
Der aktuelle Artikel behandelt die Syntax und weitere Informationen zum Ändern des Namens und der Sortierung einer Datenbank.

[ALTER DATABASE-Optionen für Dateien und Dateigruppen](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
Stellt die Syntax und weitere Informationen zum Hinzufügen und Entfernen von Dateien und Dateigruppen in einer Datenbank sowie zum Ändern der Datei- und Dateigruppenattribute bereit.

[ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
Stellt die Syntax und weitere Informationen zum Ändern der Datenbankattribute mithilfe der SET-Optionen von ALTER DATABASE bereit.

[ALTER DATABASE-Datenbankspiegelung](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
Stellt die Syntax und weitere Informationen für die SET-Optionen von ALTER DATABASE bereit, die sich auf die Datenbankspiegelung beziehen.

[ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
Stellt die Syntax und weitere Informationen zu den ALTER DATABASE-Optionen für [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] zum Konfigurieren einer sekundären Datenbank auf einem sekundären Replikat einer Always On-Verfügbarkeitsgruppe bereit.

[ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
Stellt die Syntax und weitere Informationen für die SET-Optionen von ALTER DATABASE bereit, die sich auf die Datenbank-Kompatibilitätsgrade beziehen.

[ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
Stellt die Syntax für datenbankweit gültige Konfigurationen bereit, die für einzelne Einstellungen auf Datenbankebene wie Abfrageoptimierung und Abfrageausführung verwendet werden.

## <a name="syntax"></a>Syntax

```syntaxsql
-- SQL Server Syntax
ALTER DATABASE { database_name | CURRENT }
{
    MODIFY NAME = new_database_name
  | COLLATE collation_name
  | <file_and_filegroup_options>
  | SET <option_spec> [ ,...n ] [ WITH <termination> ]
}
[;]

<file_and_filegroup_options>::=
  <add_or_modify_files>::=
  <filespec>::=
  <add_or_modify_filegroups>::=
  <filegroup_updatability_option>::=

<option_spec>::=
{
  | <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option><delayed_durability_option>
  | <external_access_option>
  | <FILESTREAM_options>
  | <HADR_options>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
  | <compatibility_level>
      { 150 | 140 | 130 | 120 | 110 | 100 | 90 }
}
```

## <a name="arguments"></a>Argumente

*database_name* Der Name der Datenbank, die geändert werden soll.

> [!NOTE]
> Diese Option ist in einer eigenständigen Datenbank nicht verfügbar.

CURRENT   
**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.

Legt fest, dass die zurzeit verwendete Datenbank geändert werden soll.

MODIFY NAME **=** _new_database_name_   
Benennt die Datenbank in den angegebenen Namen *new_database_name* um.

COLLATE *collation_name*   
Gibt die Sortierung für die Datenbank an. *collation_name* kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname sein. Wenn keine Sortierung angegeben ist, wird der Datenbank die Sortierung der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugewiesen.

> [!NOTE]
> Sobald die Datenbank einmal in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] erstellt wurde, kann sie nicht mehr geändert werden.

Beim Erstellen von Datenbanken mit einer von der Standardsortierung abweichenden Sortierung folgen die Daten in der Datenbank immer der angegebenen Sortierung. Bei der Erstellung einer eigenständigen Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden die internen Kataloginformationen mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Standardsortierung **Latin1_General_100_CI_AS_WS_KS_SC** verwaltet.

Weitere Informationen zu den Namen von Windows- und SQL-Sortierungen finden Sie unter [COLLATE](~/t-sql/statements/collations.md).

**\<delayed_durability_option> ::=**    
**Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.

Weitere Informationen finden Sie unter [ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md) und [Steuern der Transaktionsdauerhaftigkeit](../../relational-databases/logs/control-transaction-durability.md).

**\<file_and_filegroup_options>::=**    
Weitere Informationen finden Sie unter [ALTER DATABASE-Optionen für Dateien und Dateigruppen](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

## <a name="remarks"></a>Bemerkungen

Verwenden Sie [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md), um eine Datenbank zu entfernen.

Verwenden Sie [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md), um die Größe einer Datenbank zu reduzieren.

Die `ALTER DATABASE`-Anweisung muss im Autocommitmodus (dem Standardmodus für die Transaktionsverwaltung) ausgeführt werden und ist in einer expliziten oder impliziten Transaktion nicht zugelassen.

Der Status einer Datenbankdatei (z. B. online oder offline) wird unabhängig vom Status der Datenbank verwaltet. Weitere Informationen finden Sie im Abschnitt [Dateistatus](../../relational-databases/databases/file-states.md). Der Status der Dateien in einer Dateigruppe bestimmt die Verfügbarkeit der gesamten Dateigruppe. Damit eine Dateigruppe verfügbar ist, müssen alle Dateien in der Dateigruppe online sein. Ist eine Dateigruppe offline, verursacht jeder Versuch, über eine SQL-Anweisung auf die Dateigruppe zuzugreifen, einen Fehler. Wenn Sie Abfragepläne für SELECT-Anweisungen erstellen, vermeidet der Abfrageoptimierer nicht gruppierte Indizes und indizierte Sichten, die sich in Offlinedateigruppen befinden. Dadurch wird ein erfolgreiches Ausführen der Anweisungen ermöglicht. Enthält die Offlinedateigruppe jedoch den Heap oder gruppierten Index der Zieltabelle, schlagen die SELECT-Anweisungen fehl. Auch alle `INSERT`-, `UPDATE`- oder `DELETE`-Anweisungen, die eine Tabelle mit einem Index in einer Offlinedateigruppe ändern, schlagen fehl.

Wenn eine Datenbank sich im Status RESTORING befindet, erzeugen die meisten `ALTER DATABASE`-Anweisungen einen Fehler. Eine Ausnahme bildet das Festlegen von Datenbank-Spiegelungsoptionen. Eine Datenbank kann den Status RESTORING aufweisen, während ein Wiederherstellungsvorgang aktiv ist oder wenn ein Wiederherstellungsvorgang einer Datenbank oder Protokolldatei aufgrund einer beschädigten Sicherungsdatei fehlschlägt.

Der Plancache für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird gelöscht, indem eine der folgenden Optionen festgelegt wird.

- COLLATE
- MODIFY FILEGROUP DEFAULT
- MODIFY FILEGROUP READ_ONLY
- MODIFY FILEGROUP READ_WRITE
- MODIFY_NAME
- OFFLINE
- ONLINE
- PAGE_VERIFY
- READ_ONLY
- READ_WRITE

Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll enthält für jeden geleerten Cachespeicher im Plancache folgende informative Meldung: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

Der Plancache wird auch in den folgenden Szenarien geleert:

- Die Datenbankoption `AUTO_CLOSE` ist für eine Datenbank auf ON festgelegt. Wenn die Datenbank von keiner Benutzerverbindung verwendet wird bzw. keine Benutzerverbindung darauf verweist, versucht der Hintergrundtask, die Datenbank automatisch zu schließen und herunterzufahren.
- Sie führen mehrere Abfragen für eine Datenbank aus, die über Standardoptionen verfügt. Anschließend wird die Datenbank gelöscht.
- Eine Datenbank-Momentaufnahme für eine Quelldatenbank wird gelöscht.
- Sie erstellen das Transaktionsprotokoll für eine Datenbank erfolgreich neu.
- Sie stellen eine Datenbanksicherung wieder her.
- Sie trennen eine Datenbank.

## <a name="changing-the-database-collation"></a>Ändern der Datenbanksortierung

Bevor Sie auf eine Datenbank eine andere Sortierung anwenden, stellen Sie sicher, dass die folgenden Bedingungen erfüllt sind:

- Die Datenbank wird derzeit nur von Ihnen verwendet.
- Von der Sortierung der Datenbank hängt kein schemagebundenes Objekt ab.

Wenn die folgenden Objekte, die von der Datenbanksortierung abhängen, in der Datenbank vorhanden sind, schlägt die ALTER DATABASE *database_name* COLLATE-Anweisung fehl. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt eine Fehlermeldung für jedes Objekt zurück, das die Aktion `ALTER` blockiert:

- Benutzerdefinierte Funktionen und Sichten, die mit SCHEMABINDING erstellt wurden
- Berechnete Spalten
- CHECK-Einschränkungen
- Tabellenwertfunktionen, die Tabellen mit Zeichenspalten zurückgeben, deren Sortierungen von der Standardsortierung der Datenbank geerbt wurden
  
Abhängigkeitsinformationen für nicht schemagebundene Entitäten werden automatisch aktualisiert, wenn die Sortierung der Datenbank geändert wird.

Durch das Ändern der Sortierung der Datenbank werden keine Duplikate von Systemnamen für die Datenbankobjekte erstellt. Wenn durch die geänderte Sortierung doppelte Namen entstehen, verursachen die folgenden Namespaces möglicherweise einen Fehler bei der Änderung der Datenbanksortierung:

- Objektnamen wie z.B. der Name einer Prozedur, Tabelle, Sicht oder eines Triggers
- Schemanamen
- Prinzipale wie z.B. eine Gruppe, Rolle oder ein Benutzer
- Namen skalarer Typen wie z.B. der Name eines vom System oder Benutzer definierten Typs
- Namen von Volltextkatalogen
- Spalten- oder Parameternamen in einem Objekt
- Indexnamen in einer Tabelle

Durch doppelte Namen, die durch die neue Sortierung entstanden sind, schlägt die ALTER-Aktion fehl. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt eine Fehlermeldung zurück, in der der Namespace angegeben wird, in dem das Duplikat gefunden wurde.

## <a name="viewing-database-information"></a>Anzeigen von Datenbankinformationen

Sie können Katalogsichten, Systemfunktionen und gespeicherte Systemprozeduren verwenden, um Informationen zu Datenbanken, Dateien und Dateigruppen zurückzugeben.

## <a name="permissions"></a>Berechtigungen

Erfordert die `ALTER`-Berechtigung für die Datenbank.

## <a name="examples"></a>Beispiele

### <a name="a-changing-the-name-of-a-database"></a>A. Ändern des Namens einer Datenbank

Im folgenden Beispiel wird der Name der Datenbank `AdventureWorks2012` in `Northwind` geändert.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
Modify Name = Northwind ;
GO
```

### <a name="b-changing-the-collation-of-a-database"></a>B. Ändern der Datenbanksortierung

Im folgenden Beispiel wird die Datenbank `testdb` mit der `SQL_Latin1_General_CP1_CI_A`S-Sortierung erstellt. Danach wird die Sortierung der Datenbank `testdb` in `COLLATE French_CI_AI` geändert.

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

```sql
USE master;
GO

CREATE DATABASE testdb
COLLATE SQL_Latin1_General_CP1_CI_AS ;
GO

ALTER DATABASE testDB
COLLATE French_CI_AI ;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Systemdatenbanken](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL-Datenbank \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-database"></a>Übersicht: SQL-Datenbank

Verwenden Sie diese Anweisung in Azure SQL-Datenbank, um eine Datenbank zu ändern. Verwenden Sie diese Anweisung, um den Namen einer Datenbank zu ändern, die Edition und das Dienstziel der Datenbank zu ändern, die Datenbank einem Pool für elastische Datenbanken hinzuzufügen oder daraus zu entfernen, Datenbankoptionen festzulegen, die Datenbank als sekundäre Datenbank in einer Georeplikationsbeziehung hinzuzufügen oder daraus zu entfernen oder den Datenbank-Kompatibilitätsgrad festzulegen.

Aufgrund ihrer Länge wird die `ALTER DATABASE`-Syntax in mehrere Artikel aufgeteilt.

ALTER DATABASE   
Der aktuelle Artikel behandelt die Syntax und weitere Informationen zum Ändern des Namens und der Sortierung einer Datenbank.

[ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)    
Stellt die Syntax und weitere Informationen zum Ändern der Datenbankattribute mithilfe der SET-Optionen von ALTER DATABASE bereit.

[ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md?view=azuresqldb-current&preserve-view=true)   
Stellt die Syntax und weitere Informationen für die SET-Optionen von ALTER DATABASE bereit, die sich auf die Datenbank-Kompatibilitätsgrade beziehen.

## <a name="syntax"></a>Syntax

```syntaxsql
-- Azure SQL Database Syntax
ALTER DATABASE { database_name | CURRENT }
{
    MODIFY NAME = new_database_name
  | MODIFY ( <edition_options> [, ... n] )
  | MODIFY BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
  | SET { <option_spec> [ ,... n ] WITH <termination>}
  | ADD SECONDARY ON SERVER <partner_server_name>
    [WITH ( <add-secondary-option>::=[, ... n] ) ]
  | REMOVE SECONDARY ON SERVER <partner_server_name>
  | FAILOVER
  | FORCE_FAILOVER_ALLOW_DATA_LOSS
}
[;]

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 250 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale'}
  | SERVICE_OBJECTIVE =
       { <service-objective>
       | { ELASTIC_POOL (name = <elastic_pool_name>) }
       }
}

<add-secondary-option> ::=
   {
      ALLOW_CONNECTIONS = { ALL | NO }
     | SERVICE_OBJECTIVE =
       { <service-objective>
       | { ELASTIC_POOL ( name = <elastic_pool_name>) }
       }
   }

<service-objective> ::={ 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
       | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) }
      }

<option_spec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <compatibility_level>
    { 150 | 140 | 130 | 120 | 110 | 100 | 90 }

}
```

## <a name="arguments"></a>Argumente

*database_name*   
Der Name der Datenbank, die geändert werden soll.

CURRENT   
Legt fest, dass die zurzeit verwendete Datenbank geändert werden soll.

MODIFY NAME **=** _new_database_name_   
Benennt die Datenbank in den angegebenen Namen *new_database_name* um. Im folgenden Beispiel wird der Name einer Datenbank von `db1` in `db2` geändert:

```sql
ALTER DATABASE db1
    MODIFY Name = db2 ;
```

MODIFY (EDITION **=** ['Basic' \| 'Standard' \| 'Premium' \|'GeneralPurpose' \| 'BusinessCritical' \| 'Hyperscale'])   
Ändert die Dienstebene der Datenbank.

Im folgenden Beispiel wird die Edition in `Premium` geändert:

```sql
ALTER DATABASE current
    MODIFY (EDITION = 'Premium');
```

> [!IMPORTANT]
> Wenn die MAXSIZE-Eigenschaft für die Datenbank auf einen Wert außerhalb des gültigen, von der jeweiligen Edition unterstützten Bereichs festgelegt wird, schlägt die Änderung der EDITION-Eigenschaft fehl.

MODIFY (BACKUP_STORAGE_REDUNDANCY **=** ['LOCAL' \| 'ZONE' \| 'GEO'])   
Hiermit wird die Speicherredundanz für Sicherungen für die Zeitpunktwiederherstellung und die Langzeitaufbewahrung (sofern konfiguriert) der Datenbank geändert. Die Änderungen werden auf alle zukünftigen Sicherungen angewendet. Vorhandene Sicherungen verwenden weiterhin die vorherige Einstellung. 

> [!IMPORTANT]
> Die Option BACKUP_STORAGE_REDUNDANCY für Azure SQL-Datenbank ist derzeit nur in der Azure-Region „Brasilien, Süden“ in der Public Preview und in „Asien, Südosten“ allgemein verfügbar.  

MODIFY (MAXSIZE **=** [100 MB \| 500 MB \| 1 \| 1024...4096] GB)   
Gibt die maximale Größe der Datenbank an. Die maximale Größe muss dem gültigen Wertsatz für die EDITION-Eigenschaft der Datenbank entsprechen. Eine Änderung der maximalen Datenbankgröße kann auch dazu führen, dass die EDITION-Eigenschaft der Datenbank geändert wird.

> [!NOTE]
> Das Argument **MAXSIZE** gilt nicht für Einzeldatenbanken im Diensttarif „Hyperscale“. Datenbanken im Diensttarif „Hyperscale“ können bei Bedarf auf bis zu 100 TB skaliert werden. Der SQL-Datenbank-Dienst fügt automatisch Speicher hinzu. Sie müssen keine maximale Größe festlegen.

**DTU-Modell**

|**MAXSIZE**|**Grundlegend**|**S0-S2**|**S3-S12**|**P1-P6**|**P11-P15**|
|-----------------|---------------|------------------|-----------------|-----------------|-----------------|-----------------|
|100 MB|√|√|√|√|√|
|250 MB|√|√|√|√|√|
|500 MB|√|√|√|√|√|
|1 GB|√|√|√|√|√|
|2 GB|√ (S)|√|√|√|√|
|5 GB|–|√|√|√|√|
|10 GB|–|√|√|√|√|
|20 GB|–|√|√|√|√|
|30 GB|–|√|√|√|√|
|40 GB|–|√|√|√|√|
|50 GB|–|√|√|√|√|
|100 GB|–|√|√|√|√|
|150 GB|–|√|√|√|√|
|200 GB|–|√|√|√|√|
|250 GB|–|√ (S)|√ (S)|√|√|
|300 GB|–|√|√|√|√|
|400 GB|Nicht zutreffend|√|√|√|√|
|500 GB|–|√|√|√ (S)|√|
|750 GB|–|√|√|√|√|
|1024 GB|–|√|√|√|√ (S)|
|Von 1024 GB bis 4096 GB in Inkrementen von 256 GB*|–|–|–|–|√|

\* P11 und P15 ermöglichen, dass die Größe von MAXSIZE bis zu 4 TB beträgt, wobei 1024 GB die Standardgröße darstellt. P11 und P15 können bis zu 4 TB des enthaltenen Speichers ohne Aufpreis verwenden. Im Premium-Tarif ist MAXSIZE mit einer Größe von mehr als 1 TB derzeit in den folgenden Regionen verfügbar: USA, Osten 2; USA, Westen; US Gov Virginia; Europa, Westen; Deutschland, Mitte; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten. Zusätzliche Informationen bezüglich der Ressourcenbeschränkungen für das DTU-Modell finden Sie unter [DTU-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits).

Der MAXSIZE-Wert für das DTU-Modell muss – wenn angegeben – ein gültiger Wert sein, der in der Tabelle oben für die festgelegte Dienstebene angezeigt wird.

**V-Kern-Modell**

**Universell – bereitgestellte Computekapazität – Gen4 (Teil 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|

**Universell – bereitgestellte Computekapazität – Gen4 (Teil 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Maximale Datengröße (GB)|1536|3072|3072|3072|4096|4096|

**Universell – bereitgestellte Computekapazität – Gen5 (Teil 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|1536|

**Universell – bereitgestellte Computekapazität – Gen5 (Teil 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|

**Universell – bereitgestelltes Computing – Fsv2-Serie (Teil 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Maximale Datengröße (GB)|1024|1024|1024|1024|1536|1536|

**Universell – bereitgestelltes Computing – Fsv2-Serie (Teil 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Maximale Datengröße (GB)|1536|1536|3072|3072|4096|

**Universell – serverlose Computekapazität – Gen5 (Teil 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Max. virtuelle Kerne|1|2|4|6|8|

**Universell – serverlose Computekapazität – Gen5 (Teil 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|Max. virtuelle Kerne|10|12|14|16|

**Universell – serverlose Computekapazität – Gen5 (Teil 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Max. virtuelle Kerne|18|20|24|32|40|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen4 (Teil 1)**

|Computegröße (Dienstziel)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen4 (Teil 2)**

|Computegröße (Dienstziel)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen5 (Teil 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|1536|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen5 (Teil 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|

**Unternehmenskritisch – bereitgestelltes Computing – M-Serie (Teil 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Maximale Datengröße (GB)|512|640|768|896|1024|1152|

**Unternehmenskritisch – bereitgestelltes Computing – M-Serie (Teil 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Maximale Datengröße (GB)|1280|1536|2048|4096|4096|

Wenn kein `MAXSIZE`-Wert bei Verwendung des vCore-Modells festgelegt ist, beträgt die Standardgröße 32 GB. Zusätzliche Informationen bezüglich der Ressourcenbeschränkungen für das V-Kern-Modell finden Sie unter [V-Kern-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits).

Die folgenden Regeln gelten für das MAXSIZE-Argument und das EDITION-Argument:

- Wenn EDITION angegeben ist, MAXSIZE jedoch nicht, wird der Standardwert für die Edition verwendet. Wenn EDITION beispielsweise auf die Standard Edition festgelegt und MAXSIZE nicht angegeben ist, wird MAXSIZE automatisch auf 250 MB festgelegt.
- Wenn weder MAXSIZE noch EDITION angegeben sind, wird EDITION auf „Universell“ (S0) und MAXSIZE auf 32 GB festgelegt.

MODIFY (SERVICE_OBJECTIVE = \<service-objective>)   
Gibt die Computegröße (Dienstziel) an. Im folgenden Beispiel wird das Dienstziel einer Premium-Datenbank in `P6` geändert:

```sql
ALTER DATABASE current
    MODIFY (SERVICE_OBJECTIVE = 'P6');
```

SERVICE_OBJECTIVE   

- **Bei einzelnen und in einem Pool zusammengefassten Datenbanken**

  - Gibt die Computegröße (Dienstziel) an. Als Dienstziele sind die folgenden Werte verfügbar: `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Für einzelne Datenbanken in der serverlosen Computeebene**

  - Gibt die Computegröße (Dienstziel) an. Als Dienstziele sind die folgenden Werte verfügbar: `GP_S_Gen5_1` `GP_S_Gen5_2` `GP_S_Gen5_4` `GP_S_Gen5_6` `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Bei einzelnen Datenbanken im Diensttarif „Hyperscale“**

  - Gibt die Computegröße (Dienstziel) an. Als Dienstziele sind die folgenden Werte verfügbar: `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Dienstzielbeschreibungen und weitere Informationen zu Größe, Editionen und Dienstzielkombinationen finden Sie unter [Dienstebenen und Leistungsstufen von Azure SQL-Datenbank](/azure/azure-sql/database/purchasing-models), [DTU-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits) und [V-Kern-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits). Die Unterstützung für PRS-Dienstziele wurde entfernt. Wenn Sie Fragen haben, wenden Sie sich an den E-Mail-Alias premium-rs@microsoft.com.

MODIFY (SERVICE_OBJECTIVE = ELASTIC\_POOL (name = \<elastic_pool_name>)   
Wenn Sie eine vorhandene Datenbank zu einem Pool für elastische Datenbanken hinzufügen möchten, müssen Sie das SERVICE_OBJECTIVE der Datenbank auf ELASTIC_POOL festlegen und den Namen des Pools für elastische Datenbanken angeben. Sie können mit dieser Option auch die Datenbank in einen anderen Pool für elastische Datenbanken innerhalb desselben Servers ändern. Weitere Informationen finden Sie unter [Erstellen und Verwalten eines Pools für elastische Datenbanken von SQL-Database](/azure/azure-sql/database/elastic-pool-overview). Wenn Sie eine Datenbank aus einem Pool für elastische Datenbanken entfernen möchten, legen Sie mithilfe von ALTER DATABASE das SERVICE_OBJECTIVE-Objekt auf eine einzelne Computegröße (Dienstziel) der Datenbank fest.

> [!NOTE]
> Datenbanken im Diensttarif „Hyperscale“ können nicht zu einem Pool für elastische Datenbanken hinzugefügt werden.

ADD SECONDARY ON SERVER \<partner_server_name>   
Erstellt eine georeplizierte sekundäre Datenbank mit dem gleichen Namen auf einem Partnerserver. Dabei wird die lokale Datenbank in eine georeplizierte primäre Datenbank umgewandelt und mit dem asynchronen Replizieren von Daten von der primären in die neue sekundäre Datenbank begonnen. Der Befehl schlägt fehl, wenn auf dem sekundären Server bereits eine Datenbank mit dem gleichen Namen vorhanden ist. Der Befehl wird in der Masterdatenbank auf dem Server ausgeführt, der Host der lokalen Datenbank ist, die zur primären Datenbank wird.

> [!IMPORTANT]
> Der Diensttarif „Hyperscale“ unterstützt derzeit keine Georeplikation.

> [!IMPORTANT]
> Standardmäßig wird die sekundäre Datenbank mit der gleichen Sicherungsspeicherredundanz wie die primäre Datenbank oder Quelldatenbank erstellt. Das Ändern der Sicherungsspeicherredundanz während des Erstellens einer sekundären Datenbank wird von T-SQL nicht unterstützt. 

WITH ALLOW_CONNECTIONS { **ALL** | NO }   
Wenn ALLOW_CONNECTIONS nicht angegeben ist, wird es standardmäßig auf ALL festgelegt. Wenn ALL festgelegt ist, handelt es sich um eine schreibgeschützte Datenbank, zu der alle Anmeldenamen mit den entsprechenden Berechtigungen eine Verbindung herstellen dürfen.

MIT SERVICE_OBJECTIVE { `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128` }

Wenn SERVICE_OBJECTIVE nicht angegeben ist, wird die sekundäre Datenbank auf derselben Dienstebene wie die primäre Datenbank erstellt. Wenn SERVICE_OBJECTIVE angegeben ist, wird die sekundäre Datenbank auf der angegebenen Ebene erstellt. Diese Option unterstützt die Erstellung georeplizierter sekundärer Datenbanken mit kostengünstigeren Servicelevels. Das angegebene SERVICE_OBJECTIVE muss sich in derselben Edition wie die Quelle befinden. So können Sie beispielsweise nicht „S0“ angeben, wenn es sich bei der Edition um eine Premium-Edition handelt.

ELASTIC_POOL (name = \<elastic_pool_name>): Wenn ELASTIC_POOL nicht angegeben ist, wird die sekundäre Datenbank nicht in einem Pool für elastische Datenbanken erstellt. Wenn ELASTIC_POOL angegeben ist, wird die sekundäre Datenbank im angegebenen Pool erstellt.

> [!IMPORTANT]
> Der Benutzer, der den Befehl ADD SECONDARY ausführt, muss DBManager auf dem primären Server, db_owner-Mitglied in der lokalen Datenbank und DBManager auf dem sekundären Server sein. Die IP-Clientadresse muss sowohl für den primären als auch für den sekundären Server der Zulassungsliste in den Firewallregeln hinzugefügt werden. Sollten unterschiedliche IP-Clientadressen vorliegen, muss exakt dieselbe IP-Clientadresse, die auf dem primären Server hinzugefügt wurde, auch auf dem sekundären Server hinzugefügt werden. Dieser Schritt ist erforderlich, bevor Sie den Befehl ADD SECONDARY ausführen, um die Georeplikation zu initiieren.

REMOVE SECONDARY ON SERVER \<partner_server_name>: Entfernt die angegebene georeplizierte sekundäre Datenbank vom angegebenen Server. Der Befehl wird in der Masterdatenbank auf dem Server ausgeführt, der Host der primären Datenbank ist.

> [!IMPORTANT]
> Der Benutzer, der den Befehl REMOVE SECONDARY ausführt, muss DBManager auf dem primären Server sein.

FAILOVER: Stuft die sekundäre Datenbank in einer Partnerschaft für die Georeplikation hoch, für die der Befehl ausgeführt wird, damit sie zur primären Datenbank wird, und stuft die aktuelle primäre Datenbank tiefer, sodass diese zur neuen sekundären Datenbank wird. Im Rahmen dieses Prozesses wird der Georeplikationsmodus vorübergehend vom asynchronen in den synchronen Modus umgeschaltet. Während des Failoverprozesses:

1. Die primäre Datenbank übernimmt keine neuen Transaktionen mehr.
2. Alle ausstehenden Transaktionen werden in der sekundären Datenbank geleert.
3. Die sekundäre Datenbank wird zur primären Datenbank und beginnt mit der asynchronen Replikation mit der alten primären/der neuen sekundären Datenbank.

Durch diese Sequenz wird sichergestellt, dass keine Daten verloren gehen. Der Zeitraum, in dem keine der beiden Datenbanken verfügbar ist, umfasst 0 bis 25 Sekunden. In dieser Zeit werden die Rollen gewechselt. Der gesamte Vorgang sollte nicht mehr als einer Minute in Anspruch nehmen. Wenn die primäre Datenbank bei der Ausgabe dieses Befehls nicht verfügbar ist, schlägt der Befehl fehl, und es wird eine Fehlermeldung angezeigt, dass die primäre Datenbank nicht verfügbar ist. Wenn der Failoverprozess nicht abgeschlossen wird und ins Stocken geraten zu sein scheint, können Sie den Befehl zur Erzwingung des Failovers verwenden und einen Datenverlust in Kauf nehmen. Anschließend, wenn eine Wiederherstellung der verloren gegangenen Daten erforderlich sein sollte, können Sie DevOps (CSS) zur Wiederherstellung der verloren gegangenen Daten aufrufen.

> [!IMPORTANT]
> Der Benutzer, der den Befehl FAILOVER ausführt, muss DBManager auf dem primären und dem sekundären Server sein.

FORCE_FAILOVER_ALLOW_DATA_LOSS: Stuft die sekundäre Datenbank in einer Partnerschaft für die Georeplikation hoch, für die der Befehl ausgeführt wird, damit sie zur primären Datenbank wird, und stuft die aktuelle primäre Datenbank tiefer, sodass diese zur neuen sekundären Datenbank wird. Verwenden Sie diesen Befehl nur dann, wenn die aktuelle primäre Datenbank nicht mehr verfügbar ist. Er wurde nur für die Notfallwiederherstellung erstellt, wenn die Wiederherstellung der Verfügbarkeit wesentlich und ein Datenverlust akzeptabel ist.

Während eines erzwungenen Failovers:

1. Die angegebene sekundäre Datenbank wird sofort zur primären Datenbank und beginnt, neue Transaktionen zu akzeptieren.
2. Wenn die ursprüngliche primäre Datenbank eine Verbindung zur neuen primären Datenbank wiederherstellen kann, wird eine inkrementelle Sicherung von der ursprünglichen primären Datenbank erstellt. Die ursprüngliche primäre Datenbank wird dann zu einer neuen sekundären Datenbank.
3. Zur Wiederherstellung der Daten aus dieser inkrementellen Sicherung der ursprünglichen primären Datenbank bindet der Benutzer DevOps/CSS ein.
4. Wenn es weitere sekundäre Datenbanken gibt, werden diese automatisch so neu konfiguriert, dass sie zu den sekundären Datenbanken der neuen primären Datenbank werden. Dieser Vorgang ist asynchron. Bis zum Abschluss des Vorgangs kann es zu einer Verzögerung kommen. Bis zum Abschluss der Neukonfiguration sind die sekundären Datenbanken weiterhin die sekundären Datenbanken der ursprünglichen primären Datenbank.

> [!IMPORTANT]
> Der Benutzer, der den Befehl `FORCE_FAILOVER_ALLOW_DATA_LOSS` ausführt, muss auf dem primären und sekundären Server die Rolle `dbmanager` inne haben.

## <a name="remarks"></a>Bemerkungen

Verwenden Sie [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md), um eine Datenbank zu entfernen.
Verwenden Sie [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md), um die Größe einer Datenbank zu reduzieren.

Die `ALTER DATABASE`-Anweisung muss im Autocommitmodus (dem Standardmodus für die Transaktionsverwaltung) ausgeführt werden und ist in einer expliziten oder impliziten Transaktion nicht zugelassen.

Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll enthält für jeden geleerten Cachespeicher im Plancache folgende informative Meldung: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

Der Prozedurcache wird auch im folgenden Szenario geleert: Sie führen mehrere Abfragen für eine Datenbank aus, die über Standardoptionen verfügt. Anschließend wird die Datenbank gelöscht.

## <a name="viewing-database-information"></a>Anzeigen von Datenbankinformationen

Sie können Katalogsichten, Systemfunktionen und gespeicherte Systemprozeduren verwenden, um Informationen zu Datenbanken, Dateien und Dateigruppen zurückzugeben.

## <a name="permissions"></a>Berechtigungen

Zum Ändern der Datenbank muss eine Anmeldung entweder die Prinzipalanmeldung auf Serverebene (vom Bereitstellungsprozess erstellt), ein Mitglied der Datenbankrolle `dbmanager` in der Masterdatenbank, ein Mitglied der Datenbankrolle `db_owner` in der aktuellen Datenbank oder `dbo` der Datenbank sein.

## <a name="examples"></a>Beispiele

### <a name="a-check-the-edition-options-and-change-them"></a>A. Überprüfen Sie die Bearbeitungsoptionen, und ändern Sie diese

Legt eine Edition und maximale Größe für die Datenbank db1 fest:

```sql
SELECT Edition = DATABASEPROPERTYEX('db1', 'EDITION'),
        ServiceObjective = DATABASEPROPERTYEX('db1', 'ServiceObjective'),
        MaxSizeInBytes =  DATABASEPROPERTYEX('db1', 'MaxSizeInBytes');

ALTER DATABASE [db1] MODIFY (EDITION = 'Premium', MAXSIZE = 1024 GB, SERVICE_OBJECTIVE = 'P15');
```

### <a name="b-moving-a-database-to-a-different-elastic-pool"></a>B. Verschieben einer Datenbank in einen anderen Pool für elastische Datenbanken

Verschiebt eine vorhandene Datenbank in einen Pool mit dem Namen pool1:

```sql
ALTER DATABASE db1
MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = pool1 ) ) ;
```

### <a name="c-add-a-geo-replication-secondary"></a>C. Hinzufügen einer sekundären Datenbank für eine Georeplikation

Erstellt die lesbare sekundäre Datenbank db1 auf dem Server `secondaryserver` der Datenbank db1 auf dem lokalen Server.

```sql
ALTER DATABASE db1
ADD SECONDARY ON SERVER secondaryserver
WITH ( ALLOW_CONNECTIONS = ALL )
```

### <a name="d-remove-a-geo-replication-secondary"></a>D: Entfernen einer sekundären Datenbank für eine Georeplikation

Entfernt die sekundäre Datenbank db1 auf dem Server `secondaryserver`.

```sql
ALTER DATABASE db1
REMOVE SECONDARY ON SERVER testsecondaryserver
```

### <a name="e-failover-to-a-geo-replication-secondary"></a>E. Failover für eine sekundäre Datenbank für eine Georeplikation

Stuft die sekundäre Datenbank db1 auf dem Server `secondaryserver` höher, damit sie bei Ausführung auf dem Server `secondaryserver` zur neuen primären Datenbank wird.

```sql
ALTER DATABASE db1 FAILOVER
```

### <a name="e-force-failover-to-a-geo-replication-secondary-with-data-loss"></a>E. Erzwingen eines Failovers für eine sekundäre Datenbank für eine Georeplikation mit Datenverlust

Erzwingt die sekundäre Datenbank „db1“ auf dem Server `secondaryserver`, damit diese bei Ausführung die primäre Datenbank auf dem Server `secondaryserver` wird, wenn der primäre Server nicht mehr verfügbar ist. Diese Option kann zu Datenverlust führen.

```sql
ALTER DATABASE db1 FORCE_FAILOVER_ALLOW_DATA_LOSS
```

### <a name="g-update-a-single-database-to-service-tier-s0-standard-edition-performance-level-0"></a>G. Aktualisieren einer einzelnen Datenbank auf Diensttarif S0 (Standard Edition, Leistungsstufe 0)

Der folgende Befehl führt ein Update für eine einzelne Datenbank auf die Standard Edition (Diensttarif) mit einer Computegröße (Dienstziel) von S0 und einer maximalen Größe von 250 GB durch.

```sql
ALTER DATABASE [db1] MODIFY (EDITION = 'Standard', MAXSIZE = 250 GB, SERVICE_OBJECTIVE = 'S0');
```

### <a name="h-update-the-backup-storage-redundancy-of-a-database"></a>H. Aktualisieren der Sicherungsspeicherredundanz einer Datenbank

In diesem Beispiel wird die Sicherungsspeicherredundanz einer Datenbank in Zonenredundanz aktualisiert. Die neue Einstellung wird bei allen zukünftigen Sicherungen dieser Datenbank verwendet. Dies gilt auch für Zeitpunktwiederherstellungs- und Langzeitaufbewahrungssicherungen (sofern konfiguriert). 

```sql
ALTER DATABASE db1 MODIFY BACKUP_STORAGE_REDUNDANCY = 'ZONE'
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE (Azure SQL-Datenbank)](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Systemdatenbanken](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::



&nbsp;

## <a name="overview-azure-sql-managed-instance"></a>Übersicht: Verwaltete Azure SQL-Instanz

Verwenden Sie diese Anweisung in [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)], um Datenbankoptionen festzulegen.

Aufgrund ihrer Länge wird die `ALTER DATABASE`-Syntax in mehrere Artikel aufgeteilt.

ALTER DATABASE   
Der aktuelle Artikel umfasst die Syntax und weitere Informationen zum Festlegen von Datei- und Dateigruppenoptionen, von Datenbankoptionen und des Datenbank-Kompatibilitätsgrads.  
  
[ALTER DATABASE-Optionen für Dateien und Dateigruppen](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md?&tabs=sqldbmi)   
Stellt die Syntax und weitere Informationen zum Hinzufügen und Entfernen von Dateien und Dateigruppen in einer Datenbank sowie zum Ändern der Datei- und Dateigruppenattribute bereit.  
  
[ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md?&tabs=sqldbmi)   
Stellt die Syntax und weitere Informationen zum Ändern der Datenbankattribute mithilfe der SET-Optionen von ALTER DATABASE bereit.  
  
[ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md?&tabs=sqldbmi)   
Stellt die Syntax und weitere Informationen für die SET-Optionen von ALTER DATABASE bereit, die sich auf die Datenbank-Kompatibilitätsgrade beziehen.  

## <a name="syntax"></a>Syntax

```syntaxsql
-- Azure SQL Database Syntax  
ALTER DATABASE { database_name | CURRENT }  
{
    MODIFY NAME = new_database_name
  | COLLATE collation_name
  | <file_and_filegroup_options>  
  | SET <option_spec> [ ,...n ]  
}  
[;]

<file_and_filegroup_options>::=  
  <add_or_modify_files>::=  
  <filespec>::=
  <add_or_modify_filegroups>::=  
  <filegroup_updatability_option>::=  

<option_spec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>  
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <temporal_history_retention>
  | <compatibility_level>
      { 150 | 140 | 130 | 120 | 110 | 100 | 90 }

}  

```

## <a name="arguments"></a>Argumente

*database_name*   
Der Name der Datenbank, die geändert werden soll.

CURRENT   
Legt fest, dass die zurzeit verwendete Datenbank geändert werden soll.

## <a name="remarks"></a>Bemerkungen

Verwenden Sie [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md), um eine Datenbank zu entfernen.
Verwenden Sie [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md), um die Größe einer Datenbank zu reduzieren.

Die `ALTER DATABASE`-Anweisung muss im Autocommitmodus (dem Standardmodus für die Transaktionsverwaltung) ausgeführt werden und ist in einer expliziten oder impliziten Transaktion nicht zugelassen.

Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. Für jeden geleerten Cachespeicher im Plancache enthält das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll folgende Meldung zur Information: „[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat für den "%2!s!"-Cachespeicher (Bestandteil des Plancache) %1!s! Leerungen des Cachespeichers gefunden, die von Datenbankwartungs- oder Neukonfigurierungsvorgängen ausgelöst wurden.“. Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

Der Plancache wird auch dann geleert, wenn mehrere Abfragen auf eine Datenbank angewendet werden, die Standardoptionen aufweist. Anschließend wird die Datenbank gelöscht.

## <a name="viewing-database-information"></a>Anzeigen von Datenbankinformationen

Sie können Katalogsichten, Systemfunktionen und gespeicherte Systemprozeduren verwenden, um Informationen zu Datenbanken, Dateien und Dateigruppen zurückzugeben.

## <a name="permissions"></a>Berechtigungen

Datenbanken können nur durch den Prinzipalanmeldenamen auf Serverebene (vom Bereitstellungsprozess erstellt) oder Mitglieder der Datenbankrolle `dbcreator` geändert werden.

> [!IMPORTANT]
> Der Datenbankbesitzer kann die Datenbank nur ändern, wenn er Mitglied der Rolle `dbcreator` ist.

## <a name="examples"></a>Beispiele

Die folgenden Beispiele zeigen, wie die automatische Optimierung festgelegt wird und wie eine Datei in einer verwalteten Instanz hinzugefügt wird.

```sql
ALTER DATABASE WideWorldImporters
  SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON)

ALTER DATABASE WideWorldImporters
  ADD FILE (NAME = 'data_17')
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE (Azure SQL-Datenbank)](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)[sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Systemdatenbanken](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Übersicht: Azure Synapse Analytics

In Azure Synapse ändert `ALTER DATABASE` den Namen, die maximale Größe oder das Dienstziel einer Datenbank.

Aufgrund ihrer Länge wird die `ALTER DATABASE`-Syntax in mehrere Artikel aufgeteilt.

[ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
Stellt die Syntax und weitere Informationen zum Ändern der Datenbankattribute mithilfe der SET-Optionen von ALTER DATABASE bereit.

## <a name="syntax"></a>Syntax

### <a name="dedicated-sql-pool"></a>[Dedizierter SQL-Pool](#tab/sqlpool)
```syntaxsql
ALTER DATABASE { database_name | CURRENT }
{
  MODIFY NAME = new_database_name
| MODIFY ( <edition_option> [, ... n] )
| SET <option_spec> [ ,...n ] [ WITH <termination> ]
}
[;]

<edition_option> ::=
      MAXSIZE = {
            250 | 500 | 750 | 1024 | 5120 | 10240 | 20480
          | 30720 | 40960 | 51200 | 61440 | 71680 | 81920
          | 92160 | 102400 | 153600 | 204800 | 245760
      } GB
      | SERVICE_OBJECTIVE = {
            'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500'
          | 'DW600' | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000'
          | 'DW3000' | 'DW6000' | 'DW500c' | 'DW1000c' | 'DW1500c'
          | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c' | 'DW6000c'
          | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
      }
```
### <a name="serverless-sql-pool"></a>[Serverloser SQL-Pool](#tab/sqlod)
```syntaxsql
ALTER DATABASE { database_name | Current } 
{ 
    COLLATE collation_name 
  | SET { <optionspec> [ ,...n ] } 
} 
[;] 

<optionspec> ::= 
{ 
    <auto_option> 
  | <sql_option> 
}  

<auto_option> ::= 
{ 
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] } 
  | AUTO_UPDATE_STATISTICS { ON | OFF } 
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF } 
} 

<sql_option> ::= 
{ 
    ANSI_NULL_DEFAULT { ON | OFF } 
  | ANSI_NULLS { ON | OFF } 
  | ANSI_PADDING { ON | OFF } 
  | ANSI_WARNINGS { ON | OFF } 
  | ARITHABORT { ON | OFF } 
  | COMPATIBILITY_LEVEL = { 140 | 130 | 120 | 110 | 100 } 
  | CONCAT_NULL_YIELDS_NULL { ON | OFF } 
  | NUMERIC_ROUNDABORT { ON | OFF } 
  | QUOTED_IDENTIFIER { ON | OFF } 
} 
```
---

## <a name="arguments"></a>Argumente

*database_name*   
Gibt den Namen der zu ändernden Datenbank an.

MODIFY NAME = *new_database_name*   
Benennt die Datenbank in den angegebenen Namen *new_database_name* um.

MAXSIZE   
Der Standardwert ist 245.760 GB (240 TB).

**Anwendungsbereich:** Optimiert für Compute Gen1

Der Wert für die maximal zulässige Größe der Datenbank Die Datenbank kann nicht größer sein als MAXSIZE.

**Anwendungsbereich:** Optimiert für Compute Gen2

Die maximal zulässige Größe für Rowstore-Daten in der Datenbank Daten, die in Rowstore-Tabellen, dem Deltastore eines Columnstore-Index oder einem nicht gruppierten Index für einen gruppierten Columnstore-Index gespeichert sind, können MAXSIZE nicht übersteigen. Daten, die im Columnstore-Format komprimiert sind, haben kein Größenlimit und werden nicht durch MAXSIZE beschränkt.

SERVICE_OBJECTIVE   
Gibt die Computegröße (Dienstziel) an. Weitere Informationen zu Dienstzielen für Azure Synapse finden Sie unter [Data Warehouse-Einheiten (DWUs)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="permissions"></a>Berechtigungen

Folgende Berechtigungen sind erforderlich:

- Der Prinzipalanmeldename auf Serverebene (der während des Bereitstellungsprozesses erstellt wurde) oder
- Mitgliedschaft in der `dbmanager`-Datenbankrolle

Der Datenbankbesitzer kann die Datenbank nur ändern, wenn er Mitglied der Rolle `dbmanager` ist.

## <a name="general-remarks"></a>Allgemeine Hinweise

Bei der aktuellen Datenbank muss es sich um eine andere Datenbank als die handeln, die Sie ändern. Deshalb muss **ALTER ausgeführt werden, während eine Verbindung zur Masterdatenbank besteht**.

Der Wert für COMPATIBILITY_LEVEL ist in SQL Analytics standardmäßig auf 130 festgelegt und kann nicht geändert werden. Weitere Informationen finden Sie unter [Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank](./alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> COMPATIBILITY_LEVEL gilt nur für bereitgestellte Ressourcen (bzw. Ressourcenpools).

## <a name="limitations-and-restrictions"></a>Einschränkungen

Für die Ausführung von `ALTER DATABASE` muss die Datenbank online sein und darf sich nicht im angehaltenen Zustand befinden.

Die Anweisung `ALTER DATABASE` muss im Autocommitmodus ausgeführt werden. Dabei handelt es sich um den Standardmodus für die Transaktionsverwaltung. Dies wird in den Verbindungseinstellungen festgelegt.

Die Anweisung `ALTER DATABASE` darf nicht Teil einer benutzerdefinierten Transaktion sein.

Sie können die Datenbanksortierung nicht ändern.

## <a name="examples"></a>Beispiele

Stellen Sie vor dem Ausführen dieser Beispiele sicher, dass es sich bei der Datenbank, die Sie ändern, nicht um die aktuelle Datenbank handelt. Bei der aktuellen Datenbank muss es sich um eine andere Datenbank als die handeln, die Sie ändern. Deshalb muss **ALTER ausgeführt werden, während eine Verbindung zur Masterdatenbank besteht**.

### <a name="a-change-the-name-of-the-database"></a>A. Ändern des Datenbanknamens

```sql
ALTER DATABASE AdventureWorks2012
MODIFY NAME = Northwind;
```

### <a name="b-change-max-size-for-the-database"></a>B. Ändern der maximalen Datenbankgröße

```sql
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB );
```

### <a name="c-change-the-compute-size-service-objective"></a>C. Ändern der Computegröße (Dienstziel)

```sql
ALTER DATABASE dw1 MODIFY ( SERVICE_OBJECTIVE= 'DW1200' );
```

### <a name="d-change-the-max-size-and-the-compute-size-service-objective"></a>D: Ändern der maximalen Größe und der Computegröße (Dienstziel)

```sql
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB, SERVICE_OBJECTIVE= 'DW1200' );
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE (Azure Synapse Analytics)](../../t-sql/statements/create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
- [Liste der Referenzartikel zu Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Übersicht: Analyseplattformsystem

Ändert die Optionen für die maximale Datenbankgröße für replizierte und verteilte Tabellen sowie für das Transaktionsprotokoll in PDW. Verwenden Sie diese Anweisung, um die Speicherplatzzuordnungen für eine Datenbank zu verwalten, wenn sich diese vergrößert oder verkleinert. Darüber hinaus wird in diesem Artikel die Syntax zum Festlegen von Datenbankoptionen in PDW beschrieben.

## <a name="syntax"></a>Syntax

```syntaxsql
-- Analytics Platform System
ALTER DATABASE database_name
    SET ( <set_database_options> | <db_encryption_option> )
[;]

<set_database_options> ::=
{
    AUTOGROW = { ON | OFF }
    | REPLICATED_SIZE = size [GB]
    | DISTRIBUTED_SIZE = size [GB]
    | LOG_SIZE = size [GB]
    | SET AUTO_CREATE_STATISTICS { ON | OFF }
    | SET AUTO_UPDATE_STATISTICS { ON | OFF }
    | SET AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<db_encryption_option> ::=
    ENCRYPTION { ON | OFF }
```

## <a name="arguments"></a>Argumente

*database_name*   
Der Name der Datenbank, die geändert werden soll. Um eine Liste von Datenbanken auf dem Gerät anzuzeigen, verwenden Sie [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

AUTOGROW = { ON | OFF }   
Aktualisiert die Option AUTOGROW. Wenn AUTOGROW ON ist, erhöht [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] automatisch den zugeordneten Speicherplatz für replizierte Tabellen und verteilte Tabellen sowie das Transaktionsprotokoll nach Bedarf, um den gesteigerten Speicheranforderungen gerecht zu werden. Wenn AUTOGROW OFF ist, gibt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] einen Fehler zurück, wenn replizierte Tabellen, verteilte Tabellen oder das Transaktionsprotokoll die Einstellung für die maximale Größe überschreiten.

REPLICATED_SIZE = *size* [GB]   
Gibt die neue maximale Anzahl von Gigabyte pro Computeknoten für die Speicherung aller replizierten Tabellen in der Datenbank an, die geändert werden. Wenn Sie Appliancespeicherplatz planen, müssen Sie REPLICATED_SIZE mit der Anzahl der Computeknoten in der Appliance multiplizieren.

DISTRIBUTED_SIZE = *size* [GB]   
Gibt die neue maximale Anzahl von Gigabyte pro Datenbank für die Speicherung aller verteilten Tabellen in der Datenbank an, die geändert werden. Die Größe wird auf alle Computeknoten der Appliance verteilt.

LOG_SIZE = *size* [GB]   
Gibt die neue maximale Anzahl von Gigabyte pro Datenbank für die Speicherung aller Transaktionsprotokolle in der Datenbank an, die geändert werden. Die Größe wird auf alle Computeknoten der Appliance verteilt.

ENCRYPTION { ON | OFF }   
Legt fest, ob die Datenbank verschlüsselt (ON) oder nicht verschlüsselt (OFF) werden soll. Die Verschlüsselung kann nur für [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] konfiguriert werden, wenn [sp_pdw_database_encryption](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md) auf **1** festgelegt wurde. Ein Datenbank-Verschlüsselungsschlüssel muss erstellt werden, bevor Transparent Data Encryption konfiguriert werden kann. Weitere Informationen zur Datenbankverschlüsselung finden Sie unter [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).

SET AUTO_CREATE_STATISTICS { ON | OFF }   
Ist die Option AUTO_CREATE_STATISTICS zum automatischen Erstellen von Statistiken aktiviert, erstellt der Abfrageoptimierer nach Bedarf Statistiken für einzelne Spalten im Abfrageprädikat, um Kardinalitätsschätzungen für den Abfrageplan zu verbessern. Diese Statistiken für einzelne Spalten werden für Spalten erstellt, die noch nicht über ein Histogramm in einem vorhandenen Statistikobjekt verfügen.

Bei neuen Datenbanken, die nach dem Upgrade auf AU7 erstellt wurden, ist die Option standardmäßig auf ON festgelegt. Bei Datenbanken, die vor dem Upgrade erstellt wurden, ist die Option standardmäßig auf OFF festgelegt.

Weitere Informationen zu Statistiken finden Sie unter [Statistik](../../relational-databases/statistics/statistics.md)

SET AUTO_UPDATE_STATISTICS { ON | OFF }   
Wenn die Option AUTO_UPDATE_STATISTICS zum automatischen Update von Statistiken aktiviert ist, stellt der Abfrageoptimierer fest, wann Statistiken veraltet sein könnten, und aktualisiert diese Statistiken, sobald sie von einer Abfrage verwendet werden. Statistiken sind veraltet, wenn die Datenverteilung in der Tabelle oder indizierten Sicht durch die Vorgänge INSERT, UPDATE, DELETE oder MERGE geändert wurde. Der Abfrageoptimierer stellt fest, wann Statistiken veraltet sein könnten, indem er die Anzahl der Datenänderungen seit des letzten Statistikupdates ermittelt und sie mit einem Schwellenwert vergleicht. Der Schwellenwert basiert auf der Anzahl von Zeilen in der Tabelle oder indizierten Sicht.

Bei neuen Datenbanken, die nach dem Upgrade auf AU7 erstellt wurden, ist die Option standardmäßig auf ON festgelegt. Bei Datenbanken, die vor dem Upgrade erstellt wurden, ist die Option standardmäßig auf OFF festgelegt.

Weitere Informationen zu Statistiken finden Sie unter [Statistik](../../relational-databases/statistics/statistics.md).

SET AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }   
Mit der AUTO_UPDATE_STATISTICS_ASYNC-Option für die asynchrone Statistikaktualisierung wird festgelegt, ob der Abfrageoptimierer die synchrone oder asynchrone Statistikaktualisierung verwendet. Die Option AUTO_UPDATE_STATISTICS_ASYNC gilt für Statistikobjekte, die für Indizes, einzelne Spalten in Abfrageprädikaten und mit der CREATE STATISTICS-Anweisung generierte Statistiken erstellt wurden.

Bei neuen Datenbanken, die nach dem Upgrade auf AU7 erstellt wurden, ist die Option standardmäßig auf ON festgelegt. Bei Datenbanken, die vor dem Upgrade erstellt wurden, ist die Option standardmäßig auf OFF festgelegt.

Weitere Informationen zu Statistiken finden Sie unter [Statistik](../../relational-databases/statistics/statistics.md).

## <a name="permissions"></a>Berechtigungen

Erfordert die Berechtigung `ALTER` für die Datenbank.

## <a name="error-messages"></a>Fehlermeldungen

Wenn „auto-stats“ deaktiviert ist und Sie versuchen, die Statistikeinstellungen zu ändern, gibt PDW die Fehlermeldung `This option is not supported in PDW` aus. Der Systemadministrator kann „auto-stats“ aktivieren, indem er den Featureschalter [AutoStatsEnabled](../../analytics-platform-system/appliance-feature-switch.md) aktiviert.

## <a name="general-remarks"></a>Allgemeine Hinweise

Die Werte für `REPLICATED_SIZE`, `DISTRIBUTED_SIZE` und `LOG_SIZE` können größer, gleich oder kleiner als die aktuellen Werte für die Datenbank sein.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Vergrößerungs- und Verkleinerungsvorgänge sind ungenau. Die resultierenden tatsächlichen Größen können von den Größenparametern abweichen.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] führt die ALTER DATABASE-Anweisung nicht als unteilbaren Vorgang aus. Wenn die Anweisung während der Ausführung abgebrochen wird, bleiben Änderungen, die bereits durchgeführt wurden, erhalten.

Die Statistikeinstellungen funktionieren nur, wenn der Administrator „auto-stats“ aktiviert hat. Wenn Sie Administrator sind, aktivieren oder deaktivieren Sie „auto-stats“ mithilfe des Featureschalters [AutoStatsEnabled](../../analytics-platform-system/appliance-feature-switch.md).

## <a name="locking-behavior"></a>Sperrverhalten

Führt eine gemeinsame Sperre für das DATABASE-Objekt durch. Sie können eine Datenbank, die von einem anderen Benutzer im Lese- oder Schreibmodus verwendet wird, nicht ändern. Dies schließt auch Sitzungen ein, die eine [USE](../language-elements/use-transact-sql.md)-Anweisung für die Datenbank ausgeführt haben.

## <a name="performance"></a>Leistung

Das Verkleinern einer Datenbank kann, abhängig von der Größe der tatsächlichen Daten innerhalb der Datenbank und der Menge der Fragmentierung auf dem Datenträger, viel Zeit und viele Systemressourcen in Anspruch nehmen. Das Verkleinern einer Datenbank könnte beispielsweise mehrere Stunden oder noch länger dauern.

## <a name="determining-encryption-progress"></a>Bestimmen des Verschlüsselungsfortschritts

Verwenden Sie die folgende Abfrage, um den Fortschritt der transparenten Datenverschlüsselung für die Datenbank als Prozentsatz zu bestimmen:

```sql
WITH
database_dek AS (
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id,
        dek.encryption_state, dek.percent_complete,
        dek.key_algorithm, dek.key_length, dek.encryptor_thumbprint,
        type
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek
    INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map
        ON dek.database_id = node_db_map.database_id
        AND dek.pdw_node_id = node_db_map.pdw_node_id
    LEFT JOIN sys.pdw_database_mappings AS db_map
        ON node_db_map .physical_name = db_map.physical_name
    INNER JOIN sys.dm_pdw_nodes nodes
        ON nodes.pdw_node_id = dek.pdw_node_id
    WHERE dek.encryptor_thumbprint <> 0x
),
dek_percent_complete AS (
    SELECT database_dek.database_id, AVG(database_dek.percent_complete) AS percent_complete
    FROM database_dek
    WHERE type = 'COMPUTE'
    GROUP BY database_dek.database_id
)
SELECT DB_NAME( database_dek.database_id ) AS name,
    database_dek.database_id,
    ISNULL(
       (SELECT TOP 1 dek_encryption_state.encryption_state
        FROM database_dek AS dek_encryption_state
        WHERE dek_encryption_state.database_id = database_dek.database_id
        ORDER BY (CASE encryption_state
            WHEN 3 THEN -1
            ELSE encryption_state
            END) DESC), 0)
        AS encryption_state,
dek_percent_complete.percent_complete,
database_dek.key_algorithm, database_dek.key_length, database_dek.encryptor_thumbprint
FROM database_dek
INNER JOIN dek_percent_complete
    ON dek_percent_complete.database_id = database_dek.database_id
WHERE type = 'CONTROL';
```

Ein umfangreiches Beispiel mit allen Schritten zur Implementierung von TDE finden Sie unter [Transparente Datenverschlüsselung (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).

## <a name="examples-sspdw"></a>Beispiele: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-altering-the-autogrow-setting"></a>A. Ändern der AUTOGROW-Einstellung

Legen Sie AUTOGROW für die Datenbank `CustomerSales` auf ON fest.

```sql
ALTER DATABASE CustomerSales
    SET ( AUTOGROW = ON );
```

### <a name="b-altering-the-maximum-storage-for-replicated-tables"></a>B. Ändern des maximalen Speicherplatzes für replizierte Tabellen

Im folgenden Beispiel wird die Speicherbegrenzung für replizierte Tabellen für die Datenbank `CustomerSales` auf 1 GB festgelegt. Dabei handelt es sich um die Speicherbegrenzung pro Computeknoten.

```sql
ALTER DATABASE CustomerSales
    SET ( REPLICATED_SIZE = 1 GB );
```

### <a name="c-altering-the-maximum-storage-for-distributed-tables"></a>C. Ändern des maximalen Speicherplatzes für verteilte Tabellen

 Im folgenden Beispiel wird die Speicherbegrenzung für verteilte Tabellen für die Datenbank `CustomerSales` auf 1000 GB (ein Terabyte) festgelegt. Dabei handelt es sich um die gemeinsame Speicherbegrenzung für alle Computeknoten der Appliance und nicht um die Speicherbegrenzung pro Computeknoten.

```sql
ALTER DATABASE CustomerSales
    SET ( DISTRIBUTED_SIZE = 1000 GB );
```

### <a name="d-altering-the-maximum-storage-for-the-transaction-log"></a>D: Ändern des maximalen Speicherplatzes für das Transaktionsprotokoll

 Im folgenden Beispiel wird die Datenbank `CustomerSales` so aktualisiert, dass die maximale Größe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Transaktionsprotokolls für die Appliance 10 GB beträgt.

```sql
ALTER DATABASE CustomerSales
    SET ( LOG_SIZE = 10 GB );
```

### <a name="e-check-for-current-statistics-values"></a>E. Überprüfung auf aktuelle Statistikwerte

Die folgende Abfrage gibt die aktuellen Statistikwerte für sämtliche Datenbanken zurück. Der Wert 1 bedeutet, dass das Feature aktiviert ist, und 0 (null) bedeutet, dass das Feature deaktiviert ist.

```sql
SELECT NAME,
    is_auto_create_stats_on,
    is_auto_update_stats_on,
    is_auto_update_stats_async_on
FROM sys.databases;
```

### <a name="f-enable-auto-create-and-auto-update-stats-for-a-database"></a>F. Aktivieren der automatischen Erstellung und des automatischen Updates von Statistiken für eine Datenbank

Mit der folgenden Anweisung können Sie Statistiken zur automatischen und asynchronen Erstellung und Aktualisierung für die Datenbank „CustomerSales“ aktivieren. Dadurch werden zur Erstellung hochwertiger Abfragepläne nach Bedarf einspaltige Statistiken erstellt und aktualisiert.

```sql
ALTER DATABASE CustomerSales
    SET AUTO_CREATE_STATISTICS ON;
ALTER DATABASE CustomerSales
    SET AUTO_UPDATE_STATISTICS ON;
ALTER DATABASE
    SET AUTO_UPDATE_STATISTICS_ASYNC ON;
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE DATABASE - Analytics Platform System](../../t-sql/statements/create-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
