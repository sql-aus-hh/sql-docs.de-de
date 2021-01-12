---
description: sys.dm_io_virtual_file_stats (Transact-SQL)
title: sys.dm_io_virtual_file_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_io_virtual_file_stats
- sys.dm_io_virtual_file_stats_TSQL
- sys.dm_io_virtual_file_stats
- dm_io_virtual_file_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_virtual_file_stats dynamic management function
ms.assetid: fa3e321f-6fe5-45ff-b397-02a0dd3d6b7d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7e5e19b8add37d3a544cfb75328f060d4551585
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101568"
---
# <a name="sysdm_io_virtual_file_stats-transact-sql"></a>sys.dm_io_virtual_file_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Gibt E/A-Statistiken für Daten- und Protokolldateien zurück. Diese dynamische Verwaltungs Sicht ersetzt die [fn_virtualfilestats](../../relational-databases/system-functions/sys-fn-virtualfilestats-transact-sql.md) Funktion.  
  
> [!NOTE]  
>  Um dies in aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_io_virtual_file_stats**. 

## <a name="syntax"></a>Syntax  
  
```  
-- Syntax for SQL Server and Azure SQL Database

sys.dm_io_virtual_file_stats (   
    { database_id | NULL },  
    { file_id | NULL }  
)  
```  

```  
-- Syntax for Azure Synapse Analytics

sys.dm_pdw_nodes_io_virtual_file_stats
```
  
## <a name="arguments"></a>Argumente  


 *database_id* | Normal

 **GILT FÜR:** SQL Server (ab 2008), Azure SQL-Datenbank

 Die ID der Datenbank. *database_id* ist vom Datentyp int und hat keinen Standardwert. Eine gültige Eingabe ist die ID einer Datenbank oder NULL. Wenn Sie NULL angeben, werden alle Datenbanken in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurückgegeben.  
  
 Die integrierte [DB_ID](../../t-sql/functions/db-id-transact-sql.md)-Funktion kann angegeben werden.  
  
*file_id* | Normal

**GILT FÜR:** SQL Server (ab 2008), Azure SQL-Datenbank
 
Die ID der Datei. *file_id* ist vom Datentyp int und hat keinen Standardwert. Eine gültige Eingabe ist die ID einer Datei oder NULL. Wenn Sie NULL angeben, werden alle Dateien in der Datenbank zurückgegeben.  
  
 Die integrierte Funktion [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) kann angegeben werden und verweist auf eine Datei in der aktuellen Datenbank.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|**Gilt nicht für:**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Datenbankname.</br></br>Bei Azure Synapse Analytics ist dies der Name der Datenbank, die auf dem Knoten gespeichert ist, der durch pdw_node_id identifiziert wird. Jeder Knoten verfügt über eine tempdb-Datenbank, die über 13 Dateien verfügt. Jeder Knoten verfügt auch über eine Datenbank pro Verteilung, und jede Verteilungs Datenbank verfügt über fünf Dateien. Wenn z. b. jeder Knoten vier Verteilungen enthält, werden in den Ergebnissen 20 Verteilungsdaten Bank Dateien pro pdw_node_id angezeigt. 
|**database_id**|**smallint**|ID der Datenbank.|  
|**file_id**|**smallint**|ID der Datei.|  
|**sample_ms**|**bigint**|Anzahl der Millisekunden seit dem Starten des Computers. Mit dieser Spalte können verschiedene Ausgaben dieser Funktion verglichen werden.</br></br>Der Datentyp ist " **int** " für [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**num_of_reads**|**bigint**|Die Anzahl der Lesevorgänge, die für die Datei ausgegeben wurden.|  
|**num_of_bytes_read**|**bigint**|Gesamtanzahl von Bytes, die aus dieser Datei gelesen wurden.|  
|**io_stall_read_ms**|**bigint**|Gesamtzeit in Millisekunden, die die Benutzer auf Lesevorgänge gewartet haben, die für die Datei ausgegeben wurden.|  
|**num_of_writes**|**bigint**|Anzahl von Schreibvorgängen, die für diese Datei ausgeführt wurden.|  
|**num_of_bytes_written**|**bigint**|Gesamtanzahl von Bytes, die in die Datei geschrieben wurden.|  
|**io_stall_write_ms**|**bigint**|Gesamtzeit in Millisekunden, die die Benutzer darauf gewartet haben, dass Schreibvorgänge für die Datei abgeschlossen werden.|  
|**io_stall**|**bigint**|Gesamtzeit in Millisekunden, die die Benutzer darauf gewartet haben, dass E/A-Vorgänge für die Datei abgeschlossen werden.|  
|**size_on_disk_bytes**|**bigint**|Anzahl von Bytes, die auf dem Datenträger für diese Datei verwendet werden. Für Sparsedateien ist dies die tatsächliche Anzahl von Bytes auf dem Datenträger, die für Datenbankmomentaufnahmen verwendet werden.|  
|**file_handle**|**varbinary**|Windows-Dateihandle für diese Datei.|  
|**io_stall_queued_read_ms**|**bigint**|**Gilt nicht für:**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] through [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br /> Die E/A-Gesamtlatenz, die durch die Ressourcenkontrolle für E/A-Lesevorgänge eingeführt wird. Lässt keine NULL-Werte zu. Weitere Informationen finden Sie unter [sys.dm_resource_governor_resource_pools &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).|  
|**io_stall_queued_write_ms**|**bigint**|**Gilt nicht für:**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] through [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br />  Die E/A-Gesamtlatenz, die durch die Ressourcenkontrolle für E/A-Schreibvorgänge eingeführt wird. Lässt keine NULL-Werte zu.|
|**pdw_node_id**|**int**|**Gilt für:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]</br></br>Der Bezeichner des Knotens für die Verteilung.
 
## <a name="remarks"></a>Bemerkungen
Die Indikatoren werden so initialisiert, dass Sie bei jedem Start des SQL Server-Dienstanbieter (MSSQLSERVER) leer sind.
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung. Weitere Informationen finden Sie unter [dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Beispiele  

### <a name="a-return-statistics-for-a-log-file"></a>A. Rückgabe Statistiken für eine Protokolldatei

**Gilt für:** SQL Server (beginnend mit 2008), Azure SQL-Datenbank

 Im folgenden Beispiel werden alle Statistiken für die Protokolldatei in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben.  
  
```sql  
SELECT * FROM sys.dm_io_virtual_file_stats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="b-return-statistics-for-file-in-tempdb"></a>B. Statistik für Datei in tempdb zurückgeben

**Gilt für:** Azure-Synapse-Analyse

```sql
SELECT * FROM sys.dm_pdw_nodes_io_virtual_file_stats 
WHERE database_name = 'tempdb' AND file_id = 2;

```

## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [In Beziehung stehende dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  

