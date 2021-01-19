---
description: DROP VIEW (Transact-SQL)
title: DROP VIEW (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_VIEW_TSQL
- DROP VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- dropping views
- DROP VIEW statement
- deleting views
- indexed views [SQL Server], removing
- views [SQL Server], removing
- removing views
ms.assetid: 03cea355-e39c-46e1-b7db-8832038669dd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3eafbb102cbd58d6d9684f083e82078a8f578feb
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170392"
---
# <a name="drop-view-transact-sql"></a>DROP VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Entfernt eine oder mehrere Sichten aus der aktuellen Datenbank. DROP VIEW kann in indizierten Sichten ausgeführt werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure Synapse Analytics
  
DROP VIEW [ IF EXISTS ] [ schema_name . ] view_name [ ...,n ] [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DROP VIEW [ schema_name . ] view_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658), [!INCLUDE[sssds](../../includes/sssds-md.md)]).  
  
 Löscht die Sicht nur, wenn diese bereits vorhanden ist.  
  
 *schema_name*  
 Ist der Name des Schemas, zu dem die Sicht gehört.  
  
 *view_name*  
 Ist der Name der zu entfernenden Sicht.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie eine Sicht löschen, werden die Definition der Sicht sowie weitere Informationen zur Sicht aus dem Systemkatalog entfernt. Alle Berechtigungen für die Sicht werden ebenfalls gelöscht.  
  
 Eine mithilfe von DROP TABLE gelöschte Sicht in einer Tabelle muss explizit mit DROP VIEW gelöscht werden.  
  
 Beim Ausführen in einer indizierten Sicht löscht DROP VIEW automatisch alle Indizes der Sicht. Verwenden Sie [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md), um alle Indizes in einer Sicht anzuzeigen.  
  
 Wenn eine Abfrage über eine Sicht durchgeführt wird, überprüft [!INCLUDE[ssDE](../../includes/ssde-md.md)], ob alle Datenbankobjekte, auf die in der Anweisung verwiesen wird, vorhanden sind, ob sie im Kontext der Anweisung gültig sind und ob Datenänderungsanweisungen gegen die Regeln der Datenintegrität verstoßen. Schlägt eine Überprüfung fehl, wird eine Fehlermeldung zurückgegeben. Andernfalls wird die Aktion in eine Aktion für die zugrunde liegende Tabelle bzw. Tabellen übersetzt. Haben sich die zugrunde liegenden Tabellen oder Sichten seit der ursprünglichen Erstellung der Sicht geändert, kann es hilfreich sein, die Sicht zu löschen und neu zu erstellen.  
  
 Weitere Informationen zum Bestimmen von Abhängigkeiten für einen bestimmten Trigger finden Sie unter [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md).  
  
 Weitere Informationen zum Anzeigen des Sichttexts finden Sie unter [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **CONTROL**-Berechtigung für die Sicht, die **ALTER**-Berechtigung für das Schema, das die Sicht enthält, oder die Mitgliedschaft in der festen Serverrolle **db_ddladmin**.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-drop-a-view"></a>A. Löschen einer Sicht  
 Im folgenden Beispiel wird die `Reorder`-Sicht entfernt.  
  
```sql
DROP VIEW IF EXISTS dbo.Reorder ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
 
