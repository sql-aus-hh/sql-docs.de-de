---
description: sys.query_store_query_text (Transact-SQL)
title: sys.query_store_query_text (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SYS.QUERY_STORE_QUERY_TEXT
- QUERY_STORE_QUERY_TEXT
- SYS.QUERY_STORE_QUERY_TEXT_TSQL
- QUERY_STORE_QUERY_TEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_store_query_text catalog view
- query_store_query_text catalog view
ms.assetid: f7032fa0-7c16-4492-bb82-685806c63a8c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee84228b126765ac48b128024d41bf6f8318c148
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093111"
---
# <a name="sysquery_store_query_text-transact-sql"></a>sys.query_store_query_text (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Enthält den [!INCLUDE[tsql](../../includes/tsql-md.md)] Text und das SQL-Handle der Abfrage.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**query_text_id**|**bigint**|Der Primärschlüssel.|  
|**query_sql_text**|**nvarchar(max)**|Der SQL-Text der Abfrage, wie er vom Benutzer bereitgestellt wird. Schließt Leerzeichen, Hinweise und Kommentare ein. Kommentare und Leerzeichen vor und nach dem Abfragetext werden ignoriert. Kommentare und Leerzeichen im Text werden nicht ignoriert.|  
|**statement_sql_handle**|**vabor (64)**|SQL-Handle der einzelnen Abfrage.|  
|**is_part_of_encrypted_module**|**bit**|Der Abfragetext ist Teil eines verschlüsselten Moduls.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**has_restricted_text**|**bit**|Der Abfragetext enthält ein Kennwort oder andere nicht unter sprechenswerte Wörter.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **View Database State** -Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.database_query_store_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
