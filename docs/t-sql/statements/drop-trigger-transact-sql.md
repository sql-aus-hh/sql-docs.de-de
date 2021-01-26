---
description: DROP TRIGGER (Transact-SQL)
title: DROP TRIGGER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP TRIGGER
- DROP_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- renaming triggers
- triggers [SQL Server], removing
- DDL triggers, removing
- DROP TRIGGER statement
- deleting triggers
- dropping triggers
- removing triggers
- DML triggers, removing
ms.assetid: 092d0d71-9f1e-4e38-a1c4-2487adfa5b4e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6ef0daca8b2c593c5d451c25471cef73cf8034fa
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688945"
---
# <a name="drop-trigger-transact-sql"></a>DROP TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt einen oder mehrere DML- oder DDL-Trigger aus der aktuellen Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
DROP TRIGGER [ IF EXISTS ] [schema_name.]trigger_name [ ,...n ] [ ; ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE or UPDATE statement (DDL Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON { DATABASE | ALL SERVER }   
[ ; ]  
  
-- Trigger on a LOGON event (Logon Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON ALL SERVER  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](../../sql-server/what-s-new-in-sql-server-2016.md), [!INCLUDE[sssds](../../includes/sssds-md.md)])  
  
 Entfernt den Trigger nur, wenn dieser bereits vorhanden ist.  
  
 *schema_name*  
 Der Name des Schemas, zu dem ein DML-Trigger gehört. DML-Trigger werden auf das Schema der Tabelle oder der Sicht begrenzt, in denen sie erstellt werden. *schema_name* kann für DDL- oder LOGON-Trigger nicht angegeben werden.  
  
 *trigger_name*  
 Ist der Name des zu entfernenden Triggers. Um eine Liste von gerade erstellten Triggern anzuzeigen, verwenden Sie [sys.server_assembly_modules](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) oder [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md).  
  
 DATABASE  
 Gibt den Bereich des DDL-Triggers für die aktuelle Datenbank an. DATABASE muss angegeben werden, wenn es auch beim Erstellen oder Ändern des Triggers angegeben wurde.  
  
 ALL SERVER  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt den Bereich des DDL-Triggers für den aktuellen Server an. ALL SERVER muss angegeben werden, wenn es auch beim Erstellen oder Ändern des Triggers angegeben wurde. ALL SERVER gilt auch für LOGON-Trigger.  
  
> [!NOTE]  
>  Diese Option ist in einer enthaltenen Datenbank nicht verfügbar.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können einen DML-Trigger entfernen, indem Sie ihn löschen oder die Triggertabelle löschen. Beim Löschen einer Tabelle werden auch alle zugeordneten Trigger gelöscht.  
  
 Wird ein Trigger gelöscht, werden die Informationen zum Trigger aus den Katalogsichten **sys.objects**, **sys.triggers** und **sys.sql_modules** entfernt.  
  
 Mehrere DDL-Trigger können nur über die DROP TRIGGER-Anweisung gelöscht werden, wenn alle Trigger mithilfe identischer ON-Klauseln erstellt wurden.  
  
 Verwenden Sie DROP TRIGGER und CREATE TRIGGER, um einen Trigger umzubenennen. Wenn Sie die Definition eines Triggers ändern möchten, verwenden Sie ALTER TRIGGER.  
  
 Weitere Informationen zum Bestimmen von Abhängigkeiten für einen bestimmten Trigger finden Sie unter [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md), [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) und [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 Weitere Informationen zum Anzeigen des Texts des Triggers finden Sie unter [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)und [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
 Weitere Informationen zum Anzeigen einer Liste mit bereits vorhandenen Triggern finden Sie unter [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) und [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Löschen eines DML-Triggers ist die ALTER-Berechtigung für die Tabelle oder Sicht erforderlich, in der der Trigger definiert wurde.  
  
 Zum Löschen eines DDL-Triggers, der mit einem Serverbereich (ON ALL SERVER) definiert ist, oder eines LOGON-Triggers ist die CONTROL SERVER-Berechtigung auf dem Server erforderlich. Um einen mit einem Datenbankbereich definierten DDL-Trigger (ON DATABASE) zu löschen, ist die ALTER ANY DATABASE DDL TRIGGER-Berechtigung in der aktuellen Datenbank erforderlich.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-dropping-a-dml-trigger"></a>A. Löschen eines DML-Triggers  
 Im folgenden Beispiel wird der `employee_insupd`-Trigger in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank gelöscht. (Ab [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] können Sie die Syntax DROP TRIGGER IF EXISTS verwenden.)  
  
```sql  
IF OBJECT_ID ('employee_insupd', 'TR') IS NOT NULL  
   DROP TRIGGER employee_insupd;  
```  
  
### <a name="b-dropping-a-ddl-trigger"></a>B. Löschen eines DDL-Triggers  
 Im folgenden Beispiel wird der DDL-Trigger `safety` gelöscht.  
  
> [!IMPORTANT]  
>  Da DDL-Trigger nicht schemabezogen sind und deshalb nicht in der **sys.objects**-Katalogsicht angezeigt werden, kann die OBJECT_ID-Funktion nicht für Abfragen verwendet werden, über die festgestellt werden soll, ob DDL-Trigger in der Datenbank vorhanden sind. Objekte, die keine Bereiche als Schemas besitzen, müssen mithilfe der entsprechenden Katalogsicht abgerufen werden. Für DDL-Trigger verwenden Sie **sys.triggers**.  
  
```sql  
DROP TRIGGER safety  
ON DATABASE;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Abrufen von Informationen zu DML-Triggern](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
