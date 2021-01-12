---
title: SET IDENTITY_INSERT (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Anweisung SET IDENTITY_INSERT Bei Festlegung auf ON können explizite Werte in die Identitätsspalte einer Tabelle eingefügt werden.
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET IDENTITY_INSERT
- SET_IDENTITY_INSERT_TSQL
- IDENTITY_INSERT_TSQL
- IDENTITY_INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY_INSERT option
- SET IDENTITY_INSERT statement
- identity values [SQL Server], explicit values
- identity columns [SQL Server], explicit values
ms.assetid: a5dd49f2-45c7-44a8-b182-e0a5e5c373ee
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: 0a2d8e7cb4625d4b1ddcd35473e16f9bcad1d810
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093372"
---
# <a name="set-identity_insert-transact-sql"></a>SET IDENTITY_INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Ermöglicht das Einfügen expliziter Werte in die Identitätsspalte einer Tabelle.  

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET IDENTITY_INSERT [ [ database_name . ] schema_name . ] table_name { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *database_name*  
 Name der Datenbank, in der sich die angegebene Tabelle befindet.  
  
 *schema_name*  
 Der Name des Schemas, zu dem die Tabelle gehört.  
  
 *table_name*  
 Name einer Tabelle mit einer Identitätsspalte.  
  
## <a name="remarks"></a>Bemerkungen  
 Die IDENTITY_INSERT-Eigenschaft kann in einer Sitzung zu jedem Zeitpunkt nur für eine einzige Tabelle auf ON festgelegt sein. Wenn diese Eigenschaft bereits für eine Tabelle auf ON festgelegt ist und eine SET IDENTITY_INSERT ON-Anweisung für eine andere Tabelle ausgegeben wird, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Fehlermeldung zurück, die besagt, dass SET IDENTITY_INSERT bereits den Wert ON hat, und die angibt, für welche Tabelle der Wert ON festgelegt ist.  
  
 Wenn der eingefügte Wert größer als der aktuelle Identitätswert für die Tabelle ist, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] automatisch den neu eingefügten Wert als aktuellen Identitätswert.  
  
 Die Einstellung von SET IDENTITY_INSERT wird beim Ausführen bzw. zur Laufzeit festgelegt, nicht beim Analysieren.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer muss der Besitzer der Tabelle sein oder die ALTER-Berechtigung für die Tabelle besitzen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine Tabelle mit einer Identitätsspalte erstellt. Es zeigt, wie mithilfe der `SET IDENTITY_INSERT`-Einstellung eine aufgrund einer `DELETE`-Anweisung entstandene Lücke in den Identitätswerten gefüllt werden kann.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create tool table.  
CREATE TABLE dbo.Tool(  
   ID INT IDENTITY NOT NULL PRIMARY KEY,   
   Name VARCHAR(40) NOT NULL  
);  
GO  
-- Inserting values into products table.  
INSERT INTO dbo.Tool(Name)   
VALUES ('Screwdriver')  
        , ('Hammer')  
        , ('Saw')  
        , ('Shovel');  
GO  
  
-- Create a gap in the identity values.  
DELETE dbo.Tool  
WHERE Name = 'Saw';  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
  
-- Try to insert an explicit ID value of 3;  
-- should return an error:
-- An explicit value for the identity column in table 'AdventureWorks2012.dbo.Tool' can only be specified when a column list is used and IDENTITY_INSERT is ON.
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
-- SET IDENTITY_INSERT to ON.  
SET IDENTITY_INSERT dbo.Tool ON;  
GO  
  
-- Try to insert an explicit ID value of 3.  
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
-- Drop products table.  
DROP TABLE dbo.Tool;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENTITY &#40;Eigenschaft&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SET-Anweisungen &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
