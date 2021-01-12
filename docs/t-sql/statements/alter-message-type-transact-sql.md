---
description: ALTER MESSAGE TYPE (Transact-SQL)
title: ALTER MESSAGE TYPE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_MESSAGE_TYPE_TSQL
- ALTER MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER MESSAGE TYPE statement
- modifying message types
- message types [Service Broker], modifying
ms.assetid: 98c94176-2bdf-4725-b4bc-d33b6b14817d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c8b1cbf21f5875ee32174a37f29dfe4a32fbf195
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092133"
---
# <a name="alter-message-type-transact-sql"></a>ALTER MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ändert die Eigenschaften eines Nachrichtentyps.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql 
ALTER MESSAGE TYPE message_type_name  
   VALIDATION =  
    {  NONE   
     | EMPTY   
     | WELL_FORMED_XML   
     | VALID_XML WITH SCHEMA COLLECTION schema_collection_name }  
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *message_type_name*  
 Der Name des Nachrichtentyps, der geändert werden soll. Server-, Datenbank- und Schemaname können nicht angegeben werden.  
  
 VALIDATION  
 Gibt an, wie [!INCLUDE[ssSB](../../includes/sssb-md.md)] den Nachrichtentext für Nachrichten von diesem Typ überprüft.  
  
 Keine  
 Es wird keine Überprüfung ausgeführt. Der Nachrichtentext kann beliebige Daten enthalten oder kann NULL sein.  
  
 EMPTY  
 Der Nachrichtentext muss NULL sein.  
  
 WELL_FORMED_XML  
 Der Nachrichtentext muss wohlgeformte XML-Daten enthalten.  
  
 VALID_XML_WITH_SCHEMA = *schema_collection_name*  
 Der Nachrichtentext muss XML-Daten enthalten, die einem Schema in der angegebenen Schemaauflistung entsprechen. *schema_collection_name* muss der Name einer vorhandenen XML-Schemaauflistung sein.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Ändern der Überprüfung eines Nachrichtentyps hat auf Nachrichten, die bereits an eine Warteschlange übermittelt wurden, keine Auswirkungen.  
  
 Verwenden Sie die ALTER AUTHORIZATION-Anweisung, wenn Sie AUTHORIZATION für einen Nachrichtentyp ändern möchten.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Ändern eines Nachrichtentyps wird standardmäßig Mitgliedern der festen Datenbankrollen **db_ddladmin** oder **db_owner** sowie Mitgliedern der festen Serverrolle **sysadmin** erteilt.  
  
 Wenn in der ALTER MESSAGE TYPE-Anweisung eine Schemaauflistung angegeben ist, muss der Benutzer, der die Anweisung ausführt, über die REFERENCES-Berechtigung in der angegebenen Schemaauflistung verfügen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Nachrichtentyp `//Adventure-Works.com/Expenses/SubmitExpense` geändert, sodass der Nachrichtentext ein wohlgeformtes XML-Dokument enthalten muss.  
  
```sql  
ALTER MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = WELL_FORMED_XML ;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
