---
description: cdc.change_tables (Transact-SQL)
title: CDC.change_tables (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc.change_tables
- cdc.change_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.change_tables
ms.assetid: 3525a5f5-8d8b-46a8-b334-4b7cd9fb7c21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28da42d6fbf03a01dc1c0719bd62a6270cb6525e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091507"
---
# <a name="cdcchange_tables-transact-sql"></a>cdc.change_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro Änderungstabelle in der Datenbank zurück. Eine Änderungstabelle wird erstellt, wenn Change Data Capture für eine Quelltabelle aktiviert ist. Es wird empfohlen, die Systemtabellen nicht direkt abzufragen. Führen Sie stattdessen die gespeicherte Prozedur [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) aus.  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID der Änderungstabelle. Ist innerhalb einer Datenbank eindeutig.|  
|**version**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] gibt diese Spalte immer den Wert 0 zurück.|  
|**source_object_id**|**int**|ID der Quelltabelle, für die Change Data Capture aktiviert ist.|  
|**capture_instance**|**sysname**|Name der Aufzeichnungsinstanz, der zur Benennung von instanzspezifischen Nachverfolgungsobjekten verwendet wird. Standardmäßig wird der Name aus dem Quell Schema Namen und dem Quell Tabellennamen im Format *schemaname_sourcename* abgeleitet.|  
|**start_lsn**|**binary(10)**|Protokollfolgenummer (Log Sequence Number, LSN), die den unteren Endpunkt zum Abfragen der in der Änderungstabelle enthaltenen Änderungsdaten darstellt.<br /><br /> NULL = Der untere Endpunkt wurde nicht erstellt.|  
|**end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Für [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] gibt diese Spalte immer NULL zurück.|  
|**supports_net_changes**|**bit**|Unterstützung zum Abfragen von Nettoänderungen ist für die Änderungstabelle aktiviert.|  
|**has_drop_pending**|**bit**|Der Aufzeichnungsprozess hat die Benachrichtigung erhalten, dass die Quelltabelle gelöscht wurde.|  
|**role_name**|**sysname**|Name der Daten Bank Rolle, die verwendet wird, um den Zugriff auf Änderungs Daten zu ändern.<br /><br /> NULL = Eine Rolle wird nicht verwendet.|  
|**index_name**|**sysname**|Name des Indexes, mit dessen Hilfe Zeilen in der Quelltabelle eindeutig identifiziert werden. **index_name** ist entweder der Name des Primärschlüssel Indexes der Quell Tabelle oder der Name eines eindeutigen Indexes, der beim Aktivieren Change Data Capture in der Quell Tabelle angegeben wurde.<br /><br /> NULL = Bei der Aktivierung von Change Data Capture verfügte die Quelltabelle über keinen Primärschlüssel, und es wurde auch kein eindeutiger Index angegeben.<br /><br /> Hinweis: Wenn Change Data Capture für eine Tabelle aktiviert ist, in der ein Primärschlüssel vorhanden ist, verwendet das Change Data Capture Feature den Index, unabhängig davon, ob NET-Änderungen aktiviert sind oder nicht. Nachdem Change Data Capture aktiviert wurde, kann der Primärschlüssel nicht mehr geändert werden. Besitzt die Tabelle keinen Primärschlüssel, können Sie immer noch Change Data Capture aktivieren, jedoch nur dann, wenn Nettoänderungen auf false festgelegt wurden. Nachdem Change Data Capture aktiviert wurde, können Sie einen Primärschlüssel erstellen. Sie können den Primärschlüssel auch ändern, denn Change Data Capture verwendet den Primärschlüssel nicht.|  
|**filegroup_name**|**sysname**|Name der Dateigruppe, in der sich die Änderungstabelle befindet.<br /><br /> NULL = Die Änderungstabelle befindet sich in der Standarddateigruppe der Datenbank.|  
|**create_date**|**datetime**|Datum, an dem die Quelltabelle aktiviert wurde.|  
|**partition_switch**|**bit**|Gibt an, ob der **Switch Partition** -Befehl von **ALTER TABLE** für eine Tabelle ausgeführt werden kann, die für Change Data Capture aktiviert ist. 0 bedeutet, dass der Partitionswechsel blockiert wird. Für nicht partitionierte Tabellen wird stets 1 zurückgegeben.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
  
