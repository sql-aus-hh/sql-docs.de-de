---
description: DROP SYNONYM (Transact-SQL)
title: DROP SYNONYM (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SYNONYM
- DROP_SYNONYM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting synonyms
- synonyms [SQL Server], removing
- removing synonyms
- DROP SYNONYM statement
- dropping synonyms
ms.assetid: 23578932-e4de-4c39-a5a0-ce45139c4269
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2a3dc4b5974059062815000f1671d1fa425137a7
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688879"
---
# <a name="drop-synonym-transact-sql"></a>DROP SYNONYM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt ein Synonym aus einem angegebenen Schema.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP SYNONYM [ IF EXISTS ] [ schema. ] synonym_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](../../sql-server/what-s-new-in-sql-server-2016.md))
  
 Löscht das Synonym nur, wenn dieses bereits vorhanden ist.  
  
 *schema*  
 Gibt das Schema an, in dem das Synonym vorhanden ist. Wird kein Schema angegeben, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Standardschema des aktuellen Benutzers.  
  
 *synonym_name*  
 Der Name des Synonyms, das gelöscht werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Verweise auf Synonyme sind nicht an ein Schema gebunden. Deshalb können Sie ein Synonym jederzeit löschen. Verweise auf gelöschte Synonyme werden erst zur Laufzeit gefunden.  
  
 In dynamischem SQL können Synonyme erstellt und gelöscht werden. Außerdem kann auf sie verwiesen werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Löschen eines Synonyms muss ein Benutzer mindestens eine der folgenden Bedingungen erfüllen. Folgende Voraussetzungen müssen erfüllt sein:  
  
-   Der Benutzer muss der aktuelle Besitzer eines Synonyms sein.  
  
-   Der Benutzer muss ein Berechtigter für CONTROL für ein Synonym sein.  
  
-   Der Benutzer muss ein Berechtigter sein, der über die ALTER SCHEMA-Berechtigung für das enthaltene Schema verfügt.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird zuerst das Synonym `MyProduct` erstellt und dann gelöscht.  
  
```sql  
USE tempdb;  
GO  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
-- Drop synonym MyProduct.  
USE tempdb;  
GO  
DROP SYNONYM MyProduct;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
