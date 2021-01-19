---
description: DROP SEQUENCE (Transact-SQL)
title: DROP SEQUENCE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SEQUENCE
- DROP_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SEQUENCE statement
- sequence number object, dropping
ms.assetid: c25772d3-61af-4aa7-b58b-a6f67a793e3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0513019d95994607cf3db97c0fe9f72f8c3e44e0
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171622"
---
# <a name="drop-sequence-transact-sql"></a>DROP SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt ein Sequenzobjekt aus der aktuellen Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP SEQUENCE [ IF EXISTS ] { database_name.schema_name.sequence_name | schema_name.sequence_name | sequence_name } [ ,...n ]  
 [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Löscht die Sequenz nur, wenn diese bereits vorhanden ist.  
  
 *database_name*  
 Der Name der Datenbank, in der das Sequenzobjekt erstellt wurde.  
  
 *schema_name*  
 Der Name des Schemas, zu dem das Sequenzobjekt gehört.  
  
 *sequence_name*  
 Der Name der Sequenz, die gelöscht werden soll. Der Typ ist **sysname**.  
  
## <a name="remarks"></a>Hinweise  
 Sequenzobjekte weisen keine andauernde Beziehung zur generierten Zahl auf. Daher können Sequenzobjekte gelöscht werden, auch wenn die generierte Zahl noch verwendet wird.  
  
 Ein Sequenzobjekt kann gelöscht werden, während eine gespeicherte Prozedur oder ein Trigger darauf verweisen, da es nicht schemagebunden ist. Ein Sequenzobjekt kann nicht gelöscht werden, wenn als Standardwert in einer Tabelle darauf verwiesen wird. Das Objekt, das auf die Sequenz verweist, ist in der Fehlermeldung aufgeführt.  
  
 Um alle Sequenzobjekte in der Datenbank aufzulisten, führen Sie die folgende Anweisung aus.  
  
```sql  
SELECT sch.name + '.' + seq.name AS [Sequence schema and name]   
    FROM sys.sequences AS seq  
    JOIN sys.schemas AS sch  
        ON seq.schema_id = sch.schema_id ;  
GO  
```  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER- oder CONTROL-Berechtigung für das Schema.  
  
### <a name="audit"></a>Audit  
 Überwachen Sie **SCHEMA_OBJECT_CHANGE_GROUP**, um **DROP SEQUENCE** zu überwachen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird das Sequenzobjekt `CountBy1` aus der aktuellen Datenbank entfernt.  
  
```sql  
DROP SEQUENCE CountBy1 ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Sequenznummern](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
