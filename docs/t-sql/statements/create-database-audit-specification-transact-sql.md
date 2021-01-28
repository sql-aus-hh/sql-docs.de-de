---
title: CREATE DATABASE AUDIT SPECIFICATION
description: Hier erfahren Sie, wie Sie die Spezifikation eines Datenbanküberwachungsobjekts mithilfe des SQL Server-Features für die Überwachung hinzufügen.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 01/03/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE DATABASE AUDIT
- DATABASE_AUDIT_SPECIFICATION_TSQL
- DATABASE AUDIT SPECIFICATION
- CREATE_DATABASE_AUDIT_SPECIFICATION_TSQL
- CREATE_DATABASE_AUDIT_TSQL
- CREATE DATABASE AUDIT SPECIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- database audit specification
- CREATE DATABASE AUDIT SPECIFICATION statement
ms.assetid: 0544da48-0ca3-4a01-ba4c-940e23dc315b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8d1f8288a0e09c3382c9c31420fb1da6083292f8
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783482"
---
# <a name="create-database-audit-specification-transact-sql"></a>CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Erstellt ein Datenbank-Überwachungsspezifikationsobjekt mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit-Funktionen. Weitere Informationen finden Sie unter [SQL Server Audit &#40;Datenbank-Engine&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    FOR SERVER AUDIT audit_name   
        [ { ADD ( { <audit_action_specification> | audit_action_group_name } )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      action [ ,...n ]ON [ class :: ] securable BY principal [ ,...n ]  
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *audit_specification_name*  
 Der Name der Überwachungsspezifikation.  
  
 *audit_name*  
 Der Name der Überwachung, auf die diese Spezifikation angewendet wird.  
  
 *audit_action_specification*  
 Die Spezifikation von Aktionen für sicherungsfähige Elemente durch Prinzipale, die in der Überwachung aufgezeichnet werden sollen.  
  
 *action*  
 Name von einer oder mehreren überwachbaren Aktionen auf Datenbankebene. Eine Liste der Überwachungsaktionsgruppen finden Sie unter [SQL Server Audit-Aktionsgruppen und -Aktionen](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Der Name von einer oder mehreren Gruppen von überwachbaren Aktionen auf Datenbankebene. Eine Liste der Überwachungsaktionsgruppen finden Sie unter [SQL Server Audit-Aktionsgruppen und -Aktionen](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Klassenname (sofern anwendbar) auf dem sicherungsfähigen Element.  
  
 *securable*  
 Eine Tabelle, Sicht oder ein anderes sicherungsfähiges Objekt in der Datenbank, auf das die Überwachungsaktion oder die Überwachungsaktionsgruppe angewendet werden soll. Weitere Informationen finden Sie unter [Securables](../../relational-databases/security/securables.md).  
  
 *principal*  
 Der Name des Datenbankprinzipals, auf den die Überwachungsaktion oder die Überwachungsaktionsgruppe angewendet werden soll. Verwenden Sie den Datenbankprinzipal `public`, um die Überwachung aller Datenbankprinzipale gewährleisten zu können. Weitere Informationen finden Sie unter [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH ( STATE = { ON | OFF } )  
 Aktiviert oder deaktiviert das Sammeln von Datensätzen durch die Überwachung für diese Überwachungsspezifikation.  
  
## <a name="remarks"></a>Bemerkungen  
 Datenbank-Überwachungsspezifikationen sind nicht sicherungsfähige Objekte, die sich in einer gegebenen Datenbank befinden. Wenn eine Datenbank-Überwachungsspezifikation erstellt wird, befindet sich diese in einem deaktivierten Zustand.  
  
## <a name="permissions"></a>Berechtigungen  
 Benutzer mit der `ALTER ANY DATABASE AUDIT`-Berechtigung können Datenbanküberwachungsspezifikationen erstellen und sie an eine beliebige Überwachung binden.  
  
 Nach Erstellung einer Datenbanküberwachungsspezifikation kann diese von Benutzern mit der `CONTROL SERVER`-Berechtigung oder dem `sysadmin`-Konto angezeigt werden.  
  
## <a name="examples"></a>Beispiele

### <a name="a-audit-select-and-insert-on-a-table-for-any-database-principal"></a>A. Überwachen von "AUSWÄHLEN" und "EINFÜGEN" in einer Tabelle für ein beliebiges Datenbankprinzipal 
 Im folgenden Beispiel wird zunächst eine Serverüberwachung mit dem Namen `Payrole_Security_Audit` und anschließend eine Datenbank-Überwachungsspezifikation mit dem Namen `Payrole_Security_Audit` erstellt, die die`SELECT` -Anweisungen und `INSERT`-Anweisungen des `dbo`-Benutzers für die `HumanResources.EmployeePayHistory`-Tabelle in der `AdventureWorks2012`-Datenbank überwacht.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.  
CREATE SERVER AUDIT Payrole_Security_Audit  
    TO FILE ( FILEPATH =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;  
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT Payrole_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
FOR SERVER AUDIT Payrole_Security_Audit  
ADD (SELECT , INSERT  
     ON HumanResources.EmployeePayHistory BY dbo )  
WITH (STATE = ON) ;  
GO  
``` 

### <a name="b-audit-any-dml-insert-update-or-delete-on-_all_-objects-in-the-_sales_-schema-for-a-specific-database-role"></a>B. Überwachen aller DML (EINFÜGEN, AKTUALISIEREN oder LÖSCHEN) für _alle_ Objekte im _sales_-Schema für eine bestimmte Datenbankrolle  
 Im folgenden Beispiel wird zunächst eine Serverüberwachung mit dem Namen `DataModification_Security_Audit` und anschließend eine Datenbank-Überwachungsspezifikation mit dem Namen `Audit_Data_Modification_On_All_Sales_Tables` erstellt, die die `INSERT`-Anweisungen, `UPDATE`-Anweisungen und `DELETE`-Anweisungen der Benutzer in einer neuen Datenbankrolle `SalesUK` für alle Objekte im `Sales`-Schema der `AdventureWorks2012` Datenbank überwacht.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.
-- Change the path to a path that the SQLServer Service has access to. 
CREATE SERVER AUDIT DataModification_Security_Audit  
    TO FILE ( FILEPATH = 
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ; 
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT DataModification_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
CREATE ROLE SalesUK
GO
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Data_Modification_On_All_Sales_Tables  
FOR SERVER AUDIT DataModification_Security_Audit  
ADD ( INSERT, UPDATE, DELETE  
     ON Schema::Sales BY SalesUK )  
WITH (STATE = ON) ;    
GO  
```  


## <a name="see-also"></a>Weitere Informationen  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [Erstellen einer Serverüberwachung und einer Serverüberwachungsspezifikation](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
