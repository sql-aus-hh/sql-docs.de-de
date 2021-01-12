---
description: DROP XML SCHEMA COLLECTION (Transact-SQL)
title: DROP XML SCHEMA COLLECTION (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP XML SCHEMA COLLECTION
- DROP_XML_SCHEMA_COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting XML schema collections
- XML schema collections [SQL Server], removing
- schema collections [SQL Server], removing
- removing XML schema collections
- dropping XML schema collections
- DROP XML SCHEMA COLLECTION statement
ms.assetid: d686f2f5-e03a-4ffe-a566-6036628f46f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 87ce71446016c59ee888648a6557b4cbc0575519
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102289"
---
# <a name="drop-xml-schema-collection-transact-sql"></a>DROP XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Löscht die gesamte XML-Schemaauflistung und alle zugehörigen Komponenten.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP XML SCHEMA COLLECTION [ relational_schema. ]sql_identifier  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*relational_schema*  
Identifiziert den Namen des relationalen Schemas. Wenn kein Name angegeben ist, wird das relationale Standardschema verwendet.  
  
*sql_identifier*  
Der Name der zu löschenden XML-Schemaauflistung.  
  
## <a name="remarks"></a>Bemerkungen  
Das Löschen einer XML-Schemaauflistung ist ein Transaktionsvorgang. Wenn Sie eine XML-Schemaauflistung innerhalb einer Transaktion löschen und später ein Rollback für die Transaktion ausführen, wird die XML-Schemaauflistung nicht gelöscht.  
  
Eine XML-Schemaauflistung, die verwendet wird, kann nicht gelöscht werden. Also darf für die zu löschende Auflistung keine der folgenden Bedingungen zutreffen:  
  
-   Sie darf keinem Parameter bzw. keiner Spalte vom Typ **xml** zugeordnet sein.  
  
-   Sie darf nicht in Tabelleneinschränkungen angegeben sein.  
  
-   In einer schemagebundenen Funktion oder gespeicherten Prozedur darf nicht darauf verwiesen werden. Beispielsweise sperrt die folgende Funktion die XML-Schemaauflistung `MyCollection`, weil die Funktion `WITH SCHEMABINDING` angibt. Wenn Sie dies entfernen, ist für XML SCHEMA COLLECTION keine Sperre vorhanden.  
  
    ```sql  
    CREATE FUNCTION dbo.MyFunction()  
    RETURNS int  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
       ...  
       DECLARE @x XML(MyCollection)  
       ...  
    END;  
    ```  
  
## <a name="permissions"></a>Berechtigungen  
Zum Löschen von XML SCHEMA COLLECTION ist die DROP-Berechtigung für die Auflistung erforderlich.  
  
## <a name="examples"></a>Beispiele  
Das folgende Beispiel zeigt, wie eine XML-Schemaauflistung entfernt wird.  
  
```sql  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [ALTER XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Anforderungen und Einschränkungen für XML-Schemaauflistungen auf dem Server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
