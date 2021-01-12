---
description: sys.database_audit_specification_details (Transact-SQL)
title: sys.database_audit_specification_details (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_audit_specification_details
- sys.database_audit_specification_details_TSQL
- sys.database_audit_specification_details
- database_audit_specification_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_audit_specification_details catalog view
ms.assetid: 03fc60a9-1696-4109-b15e-a50046310859
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 80eb02546225f3378c2ca8f139f3282e4651ea24
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102830"
---
# <a name="sysdatabase_audit_specification_details-transact-sql"></a>sys.database_audit_specification_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält Informationen über die Datenbank-Überwachungsspezifikationen in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Überwachung auf einer Serverinstanz für alle Datenbanken. Weitere Informationen finden Sie unter [SQL Server Audit &#40;Datenbank-Engine&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md). Um eine Liste aller audit_action_id und deren Namen zu erhalten, Fragen Sie [sys.dm_audit_actions &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)ab.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_specification_id**|**int**|ID der Überwachungsspezifikation.|  
|**audit_action_id**|**int**|ID der Überwachungsaktion.|  
|**audit_action_name**|**Vom Datentyp sysname**|Name der Überwachungsaktion oder Überwachungsaktionsgruppe|  
|**Klasse**|**int**|Identifiziert die Klasse des überwachten Objekts.|  
|**class_**|**Nvarchar (60)**|Beschreibung der Klasse des überwachten Objekts:<br /><br /> - SCHEMA<br /><br /> - TABLE|  
|**major_id**|**int**|Haupt-ID des überwachten Objekts, z. B. eine Tabellen-ID einer Tabellenüberwachungsaktion.|  
|**minor_id**|**Int**|Sekundäre, entsprechend der Klasse interpretierte ID des überwachten Objekts, z. B. eine Spalten-ID einer Tabellenüberwachungsaktion.|  
|**audited_principal_id**|**int**|Prinzipal, der überwacht wird.|  
|**audited_result**|**Nvarchar (60)**|Ergebnisse der Überwachungsaktion:<br /><br /> - SUCCESS AND FAILURE - SUCCESS<br /><br /> - FAILURE|  
|**is_group**|**Trate**|Zeigt an, ob das Objekt eine Gruppe ist:<br /><br /> 0 – Keine Gruppe<br /><br /> 1 – Gruppe|  
  
## <a name="permissions"></a>Berechtigungen  
 Prinzipale mit den Berechtigungen **ALTER ANY DATABASE Audit** oder **View Definition** , der **dbo** -Rolle und Mitglieder der **db_owners** Fixed-Daten Bank Rolle haben Zugriff auf diese Katalog Sicht. Außerdem darf dem Prinzipal die **View Definition** -Berechtigung nicht verweigert werden.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Erstellen einer Serverüberwachung und einer Serverüberwachungsspezifikation](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
