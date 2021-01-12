---
description: ALTER REMOTE SERVICE BINDING (Transact-SQL)
title: ALTER REMOTE SERVICE BINDING (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER REMOTE SERVICE BINDING
- ALTER_REMOTE_SERVICE_BINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote service bindings [Service Broker], modifying
- ALTER REMOTE SERVICE BINDING statement
- modifying remote service bindings
ms.assetid: ee620b4a-9375-4eaa-a016-69916c9e1e68
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c671b412d5d9ce34c34fdc49e16215dafc8ec83e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092114"
---
# <a name="alter-remote-service-binding-transact-sql"></a>ALTER REMOTE SERVICE BINDING (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert den einer Remotedienstbindung zugeordneten Benutzer oder ändert die Einstellung der Bindung für die anonyme Authentifizierung.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ALTER REMOTE SERVICE BINDING binding_name   
   WITH [ USER = <user_name> ] [ , ANONYMOUS = { ON | OFF } ]   
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *binding_name*  
 Der Name der zu ändernden Remotedienstbindung. Server-, Datenbank- und Schemaname können nicht angegeben werden.  
  
 WITH USER = \<*user_name>*  
 Gibt den Datenbankbenutzer an, der das Zertifikat besitzt, das dem Remotedienst für diese Bindung zugeordnet ist. Der öffentliche Schlüssel dieses Zertifikats wird für die Verschlüsselung und Authentifizierung von Nachrichten verwendet, die mit dem Remotedienst ausgetauscht werden.  
  
 ANONYMOUS  
 Gibt an, ob die anonyme Authentifizierung bei der Kommunikation mit dem Remotedienst verwendet wird. Ist ANONYMOUS = ON, wird die anonyme Authentifizierung verwendet und die Anmeldeinformationen des lokalen Benutzers werden nicht an den Remotedienst übertragen. Ist ANONYMOUS = OFF, werden Benutzeranmeldeinformationen übertragen. Wird diese Klausel nicht angegeben, ist die Standardeinstellung OFF.  
  
## <a name="remarks"></a>Bemerkungen  
 Der öffentliche Schlüssel des Zertifikats, das *user_name* zugeordnet ist, wird zur Authentifizierung von Nachrichten an den Remotedienst und zur Verschlüsselung eines Sitzungsschlüssels verwendet, der dann zur Verschlüsselung der Konversation verwendet wird. Das Zertifikat für *user_name* muss dem Zertifikat für eine Anmeldung in der Datenbank entsprechen, die als Host für den Remotedienst dient.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Ändern einer Remotedienstbindung erhalten standardmäßig Besitzer der Remotedienstbindung, Mitglieder der festen Datenbankrolle **db_owner** und Mitglieder der festen Serverrolle **sysadmin**.  
  
 Der Benutzer, der die ALTER REMOTE SERVICE BINDING-Anweisung ausführt, muss berechtigt sein, die Identität des in der Anweisung angegebenen Benutzers anzunehmen.  
  
 Verwenden Sie die ALTER AUTHORIZATION-Anweisung, wenn Sie AUTHORIZATION für eine Remotedienstbindung ändern möchten.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Remotedienstbindung `APBinding` geändert, sodass Nachrichten mithilfe der Zertifikate aus dem Konto `SecurityAccount` verschlüsselt werden.  
  
```sql  
ALTER REMOTE SERVICE BINDING APBinding  
    WITH USER = SecurityAccount ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [DROP REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/drop-remote-service-binding-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
