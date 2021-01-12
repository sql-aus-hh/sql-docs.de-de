---
description: sys.query_store_plan (Transact-SQL)
title: sys.query_store_plan (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- QUERY_STORE_PLAN_TSQL
- SYS.QUERY_STORE_PLAN
- SYS.QUERY_STORE_PLAN_TSQL
- QUERY_STORE_PLAN
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_plan catalog view
- sys.query_store_plan catalog view
ms.assetid: b4d05439-6360-45db-b1cd-794f4a64935e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b13c6f42eb967c6ef5d01e189860c9de3204646
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095467"
---
# <a name="sysquery_store_plan-transact-sql"></a>sys.query_store_plan (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Enthält Informationen zu jedem Ausführungsplan, der einer Abfrage zugeordnet ist.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|
|**plan_id**|**bigint**|Der Primärschlüssel.|  
|**query_id**|**bigint**|Fremdschlüssel. Joins mit [sys.query_store_query &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md).|  
|**plan_group_id**|**bigint**|ID der Plan Gruppe. Cursor Abfragen erfordern in der Regel mehrere Pläne (Auffüllen und Abrufen). Auffüllen und Abrufen von Plänen, die zusammen kompiliert werden, befinden sich in derselben Gruppe.<br /><br /> 0 bedeutet, dass der Plan nicht in einer Gruppe ist.|  
|**engine_version**|**nvarchar(32)**|Die Version der Engine, mit der der Plan im Format **"Major. Minor. Build. Revision"** kompiliert wird.|  
|**compatibility_level**|**smallint**|Datenbank-Kompatibilitäts Grad der Datenbank, auf die in der Abfrage verwiesen wird.|  
|**query_plan_hash**|**Binär (8)**|MD5-Hash des einzelnen Plans.|  
|**query_plan**|**nvarchar(max)**|Showplan XML für den Abfrageplan.|  
|**is_online_index_plan**|**bit**|Der Plan wurde während einer Online Indexerstellung verwendet. <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**is_trivial_plan**|**bit**|Der Plan ist ein trivialer Plan (Ausgabe in Phase 0 des Abfrage Optimierers). <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**is_parallel_plan**|**bit**|Der Plan ist parallel. <br/>**Hinweis:** Azure Synapse Analytics gibt immer eins (1) zurück.|  
|**is_forced_plan**|**bit**|Der Plan wird als erzwungen gekennzeichnet, wenn der Benutzer gespeicherte Prozeduren **sys.sp_query_store_force_plan** ausführt. Das Erzwingen von Mechanismen *garantiert nicht* , dass genau dieser Plan für die Abfrage verwendet wird, auf die von **query_id** verwiesen wird. Die Plan Erzwingung bewirkt, dass die Abfrage erneut kompiliert wird, und erzeugt in der Regel genau denselben oder einen ähnlichen Plan für den Plan, auf den **plan_id** verweist Wenn die Plan Erzwingung nicht erfolgreich ist, wird **force_failure_count** inkrementiert und **last_force_failure_reason** mit der Fehlerursache aufgefüllt. <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**is_natively_compiled**|**bit**|Der Plan umfasst nativ kompilierte Speicher optimierte Prozeduren. (0 = false, 1 = true). <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**force_failure_count**|**bigint**|Gibt an, wie oft das Erzwingen dieses Plans fehlgeschlagen ist. Sie kann nur inkrementiert werden, wenn die Abfrage neu kompiliert wird (*nicht bei jeder Ausführung*). Der Wert wird jedes Mal auf 0 zurückgesetzt, wenn **is_plan_forced** von **false** in **true** geändert wird. <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**last_force_failure_reason**|**int**|Ursache für Fehler beim Erzwingen des Plans.<br /><br /> 0: kein Fehler, andernfalls Fehlernummer des Fehlers, der die Erzwingung fehlgeschlagen ist<br /><br /> 8637: ONLINE_INDEX_BUILD<br /><br /> 8683: INVALID_STARJOIN<br /><br /> 8684: TIME_OUT<br /><br /> 8689: NO_DB<br /><br /> 8690: HINT_CONFLICT<br /><br /> 8691: SETOPT_CONFLICT<br /><br /> 8694: DQ_NO_FORCING_SUPPORTED<br /><br /> 8698: NO_PLAN<br /><br /> 8712: NO_INDEX<br /><br /> 8713: VIEW_COMPILE_FAILED<br /><br /> \<other value>: GENERAL_FAILURE <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**last_force_failure_reason_desc**|**nvarchar(128)**|Die Textbeschreibung der last_force_failure_reason_desc.<br /><br /> ONLINE_INDEX_BUILD: die Abfrage versucht, Daten zu ändern, während die Ziel Tabelle über einen Index verfügt, der Online erstellt wird.<br /><br /> INVALID_STARJOIN: der Plan enthält eine ungültige starjoinspezifikation.<br /><br /> TIME_OUT: der Optimierer hat die Anzahl zulässiger Vorgänge beim Suchen nach dem durch den erzwungenen Plan angegebenen Plan überschritten.<br /><br /> NO_DB: eine im Plan angegebene Datenbank ist nicht vorhanden.<br /><br /> HINT_CONFLICT: die Abfrage kann nicht kompiliert werden, weil der Plan Konflikte mit einem Abfrage Hinweis verursacht.<br /><br /> DQ_NO_FORCING_SUPPORTED: die Abfrage kann nicht ausgeführt werden, da der Plan Konflikte mit der Verwendung von verteilten Abfragen oder voll Text Vorgängen verursacht.<br /><br /> NO_PLAN: der Abfrage Prozessor konnte keinen Abfrage Plan erzeugt werden, da der erzwungene Plan nicht überprüft werden konnte, damit er für die Abfrage gültig ist.<br /><br /> NO_INDEX: der im Plan angegebene Index ist nicht mehr vorhanden.<br /><br /> VIEW_COMPILE_FAILED: der Abfrageplan konnte aufgrund eines Problems in einer indizierten Sicht, auf die im Plan verwiesen wird, nicht erzwungen werden.<br /><br /> GENERAL_FAILURE: allgemeiner Erzwingungs Fehler (wird mit den oben genannten Gründen nicht abgedeckt) <br/>**Hinweis:** Azure Synapse Analytics gibt immer *keine* zurück.|  
|**count_compiles**|**bigint**|Planen der Kompilierungs Statistik.|  
|**initial_compile_start_time**|**datetimeoffset**|Planen der Kompilierungs Statistik.|  
|**last_compile_start_time**|**datetimeoffset**|Planen der Kompilierungs Statistik.|  
|**last_execution_time**|**datetimeoffset**|Die letzte Ausführungszeit bezieht sich auf die letzte Endzeit der Abfrage/des Plans.|  
|**avg_compile_duration**|**float**|Planen der Kompilierungs Statistik. <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**last_compile_duration**|**bigint**|Planen der Kompilierungs Statistik. <br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**plan_forcing_type**|**int**|Der Plan Erzwingungs Typ.<br /><br />0: keine<br /><br />1: manuell<br /><br />2: automatisch|  
|**plan_forcing_type_desc**|**nvarchar(60)**|Die Text Beschreibung der plan_forcing_type.<br /><br />None: kein Plan Erzwingung<br /><br />Manuell: Plan erzwungen von Benutzer<br /><br />Auto: Plan erzwungen durch automatische Optimierung|  

## <a name="plan-forcing-limitations"></a>Einschränkungen des Erzwingens von Plänen
Der Abfragedatenspeicher verfügt über eine Mechanismus, der den Abfrageoptimierer dazu zwingen kann, einen bestimmten Ausführungsplan zu verwenden. Es gibt allerdings Einschränkungen, die dazu führen können, dass das Erzwingen eines Plans verhindert wird. 

Erstens, wenn der Plan die folgenden Konstruktionen enthält:
* INSERT BULK-Anweisung
* einen Verweis auf eine externe Tabelle
* eine verteilte Abfrage oder Volltextvorgänge
* globale Abfragen 
* Dynamische oder Keysetcursor 
* eine ungültige Sternverknüpfungsspezifikation 

> [!NOTE]
> Azure SQL-Datenbank und SQL Server 2019 Support Plan Erzwingung für statische und schnelle Forward-Cursors.

Zweitens, wenn Objekte, die der Plan verwendet, nicht mehr zur Verfügung stehen:
* eine Datenbank (wenn die Datenbank, aus der der Plan kommt, nicht mehr existiert)
* ein Index (nicht mehr vorhanden oder deaktiviert)

Und drittens, bei Problemen mit dem Plan selbst:
* Er darf nicht abgefragt werden.
* Der Abfrageoptimierer hat die Zahl an zulässigen Vorgängen überschritten.
* Die XML des Plans ist ungültig formuliert.

## <a name="permissions"></a>Berechtigungen  
 Erfordert die **View Database State** -Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.database_query_store_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
  
  
