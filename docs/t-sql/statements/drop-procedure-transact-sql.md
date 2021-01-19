---
description: DROP PROCEDURE (Transact-SQL)
title: DROP PROCEDURE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP PROCEDURE
- DROP_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing stored procedures
- dropping procedure groups
- deleting stored procedures
- deleting procedure groups
- DROP PROCEDURE statement
- dropping stored procedures
- stored procedures [SQL Server], removing
- removing procedure groups
ms.assetid: 1c2d7235-7b9b-4336-8f17-429e7d82c2c3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c810d1e57b3e2cfb092e1091da0d95b082eb8c2d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172472"
---
# <a name="drop-procedure-transact-sql"></a>DROP PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Entfernt eine oder mehrere gespeicherte Prozeduren oder Prozedurgruppen aus der aktuellen Datenbank in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP { PROC | PROCEDURE } [ IF EXISTS ] { [ schema_name. ] procedure } [ ,...n ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP { PROC | PROCEDURE } { [ schema_name. ] procedure_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Löscht die Standardprozedur nur, wenn diese bereits vorhanden ist.  
  
 *schema_name*  
 Der Name des Schemas, zu dem die Prozedur gehört. Ein Servername oder Datenbankname kann nicht angegeben werden.  
  
 *procedure*  
 Der Name der gespeicherten Prozedur bzw. der gespeicherten Prozedurgruppe, die entfernt werden soll. Einzelne Prozeduren innerhalb einer nummerierten Prozedurgruppe können nicht gelöscht werden. Es wird die gesamte Prozedurgruppe gelöscht.  
  
## <a name="best-practices"></a>Bewährte Methoden  
 Überprüfen Sie abhängige Objekte, und ändern Sie diese Objekte entsprechend, bevor Sie eine gespeicherte Prozedur löschen. Das Löschen einer gespeicherten Prozedur kann Fehler bei abhängigen Objekten und Skripts verursachen, wenn diese Objekte nicht aktualisiert werden. Weitere Informationen finden Sie unter [Anzeigen der Abhängigkeiten einer gespeicherten Prozedur](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
## <a name="metadata"></a>Metadaten  
 Fragen Sie die **sys.objects**-Katalogsicht ab, um eine Liste der vorhandenen Prozeduren anzuzeigen. Fragen Sie zur Anzeige der Prozedurdefinitionen die **sys.sql_modules**-Katalogsicht ab.  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die **CONTROL**-Berechtigung für die Prozedur oder die **ALTER**-Berechtigung für das Schema, zu dem die Prozedur gehört, oder die Mitgliedschaft in der festen Serverrolle **db_ddladmin**.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die gespeicherte Prozedur `dbo.uspMyProc` aus der aktuellen Datenbank entfernt.  
  
```sql  
DROP PROCEDURE dbo.uspMyProc;  
GO  
```  
  
 Im folgenden Beispiel werden mehrere gespeicherte Prozeduren aus der aktuellen Datenbank entfernt.  
  
```sql  
DROP PROCEDURE dbo.uspGetSalesbyMonth, dbo.uspUpdateSalesQuotes, dbo.uspGetSalesByYear;  
```  
  
 Im folgenden Beispiel wird die gespeicherte Prozedur `dbo.uspMyProc` entfernt, wenn diese vorhanden ist. Es wird jedoch kein Fehler ausgelöst, wenn die Prozedur nicht vorhanden ist. Die Syntax ist in [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] neu.  
  
```sql  
DROP PROCEDURE IF EXISTS dbo.uspMyProc;  
GO  
```  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Löschen einer gespeicherten Prozedur](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)  
  
  


