---
description: DENY (Serverberechtigungen) (Transact-SQL)
title: DENY (Serverberechtigungen) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- denying permissions [SQL Server], servers
- servers [SQL Server], permissions
- DENY statement, servers
ms.assetid: 68d6b2a9-c36f-465a-9cd2-01d43a667e99
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 290b7e30d198f18fc1d07049b80a0cea78673724
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688901"
---
# <a name="deny-server-permissions-transact-sql"></a>DENY (Serverberechtigungen) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Verweigert Berechtigungen für einen Server.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DENY permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS <grantor_principal> ]   
  
<grantee_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
  
<grantor_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *permission*  
 Gibt eine Berechtigung an, die für einen Server verweigert werden kann. Eine Liste der Berechtigungen finden Sie im Abschnitt zu den Hinweisen weiter unten in diesem Thema.  
  
 CASCADE  
 Gibt an, dass die Berechtigung für den angegebenen Prinzipal und für alle anderen Prinzipale verweigert wird, denen diese Berechtigung von diesem Prinzipal erteilt wurde. Dies ist erforderlich, wenn der Prinzipal die GRANT OPTION-Berechtigung besitzt. 
  
 TO \<server_principal>  
 Gibt den Prinzipal an, für den die Berechtigung verweigert wird.  
  
 AS \<grantor_principal>  
 Gibt den Prinzipal an, von dem der Prinzipal, der diese Abfrage ausführt, sein Recht zum Verweigern der Berechtigung ableitet.
Verwenden Sie die AS-Prinzipalklausel, um anzugeben, dass der Prinzipal, der die Berechtigung verweigert hat, ein anderer Prinzipal als die Person sein muss, die die Anweisung ausführt. Nehmen Sie beispielsweise an, dass die Benutzerin Mary der principal_id 12 und der Benutzer Raul der principal_id 15 entspricht. Mary führt `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;` aus. Daraufhin gibt die Tabelle „sys.database_permissions“ an, dass die „grantor_prinicpal_id“ der DENY-Anweisung „15“ (Raul) entspricht, obwohl die Anweisung tatsächlich von Benutzer 12 (Mary) ausgeführt wurde.
  
In dieser Anweisung impliziert die Verwendung von AS nicht die Fähigkeit, die Identität eines anderen Benutzers anzunehmen.    
  
 *SQL_Server_login*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an.  
  
 *SQL_Server_login_mapped_to_Windows_login*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, der einem Windows-Anmeldenamen zugeordnet ist.  
  
 *SQL_Server_login_mapped_to_Windows_group*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, der einer Windows-Gruppe zugeordnet ist.  
  
 *SQL_Server_login_mapped_to_certificate*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, der einem Zertifikat zugeordnet ist.  
  
 *SQL_Server_login_mapped_to_asymmetric_key*  
 Gibt einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen an, der einem asymmetrischen Schlüssel zugeordnet ist.  
  
 *server_role*  
 Gibt eine Serverrolle an.  
  
## <a name="remarks"></a>Bemerkungen  
 Berechtigungen im Serverbereich können nur verweigert werden, wenn master als aktuelle Datenbank verwendet wird.  
  
 Informationen zu Serverberechtigungen werden in der [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)-Katalogsicht und Informationen zu Serverprinzipalen in der [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)-Katalogsicht angezeigt. Informationen zur Mitgliedschaft von Serverrollen werden in der [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)-Katalogsicht angezeigt.  
  
 Ein Server stellt die höchste Ebene der Berechtigungshierarchie dar. Die spezifischsten und restriktivsten Berechtigungen, die für einen Server verweigert werden können, sind in der folgenden Tabelle aufgeführt.  
  
|Serverberechtigung|Impliziert durch die Serverberechtigung|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|CREATE AVAILABILITY GROUP<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] bis [aktuelle Version](../../sql-server/what-s-new-in-sql-server-2016.md)).|CONTROL SERVER|  
|SHUTDOWN|CONTROL SERVER|  
|UNSAFE ASSEMBLY|CONTROL SERVER|  
|VIEW ANY DATABASE|VIEW ANY DEFINITION|  
|VIEW ANY DEFINITION|CONTROL SERVER|  
|VIEW SERVER STATE|ALTER SERVER STATE|  
  
 Die folgenden drei Serverberechtigungen wurden in [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] hinzugefügt.  
  
 **CONNECT ANY DATABASE**-Berechtigung  
 Erteilen Sie die **CONNECT ANY DATABASE**-Berechtigung einem Anmeldenamen, der eine Verbindung mit allen derzeit vorhandenen Datenbanken und allen zukünftig erstellten neuen Datenbanken herstellen muss. Gewährt keine Berechtigung für Datenbanken außer der Berechtigung zum Herstellen der Verbindung. Kombinieren Sie diese Berechtigung mit **SELECT ALL USER SECURABLES** oder **VIEW SERVER STATE**, um einem Überwachungsprozess das Anzeigen aller Daten oder aller Datenbankstatus in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu ermöglichen.  
  
 **IMPERSONATE ANY LOGIN**-Berechtigung  
 Wenn die Berechtigung erteilt wird, kann ein Prozess der mittleren Ebene beim Herstellen der Verbindung mit Datenbanken die Identität des Kontos von Clients annehmen, die eine Verbindung mit ihm herstellen. Wenn die Berechtigung verweigert wird, kann verhindert werden, dass ein Anmeldename mit hohen Privilegien die Identität anderer Anmeldenamen annimmt. Beispielsweise kann verhindert werden, dass ein Anmeldename mit einer **CONTROL SERVER**-Berechtigung die Identität anderer Anmeldenamen annimmt.  
  
 **SELECT ALL USER SECURABLES**-Berechtigung  
 Wenn sie erteilt wird, kann ein Anmeldename, z. B. ein Auditor, Daten in allen Datenbanken anzeigen, mit denen der Benutzer eine Verbindung herstellen kann. Wenn sie verweigert wird, wird der Zugriff auf alle Objekte, außer auf die Objekte im **sys**-Schema, verhindert.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CONTROL SERVER-Berechtigung oder den Besitz des sicherungsfähigen Elements. Falls die AS-Klausel verwendet wird, muss der angegebene Prinzipal der Besitzer des sicherungsfähigen Elements sein, für das Berechtigungen verweigert werden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-denying-connect-sql-permission-to-a-sql-server-login-and-principals-to-which-the-login-has-regranted-it"></a>A. Verweigern der CONNECT SQL-Berechtigung für einen SQL Server-Anmeldenamen und für Prinzipale, denen der Anmeldename diese Berechtigung erteilt hat  
 Im folgenden Beispiel wird die `CONNECT SQL`-Berechtigung für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen `Annika` und für die Prinzipale verweigert, denen diese Benutzerin die Berechtigung erteilt hat.  
  
```sql  
USE master;  
DENY CONNECT SQL TO Annika CASCADE;  
GO  
```  
  
### <a name="b-denying-create-endpoint-permission-to-a-sql-server-login-using-the-as-option"></a>B. Verweigern der CREATE ENDPOINT-Berechtigung für einen SQL Server-Anmeldenamen mit der AS-Option  
 Im folgenden Beispiel wird die `CREATE ENDPOINT`-Berechtigung für den Benutzer `ArifS` verweigert. Im Beispiel wird die `AS`-Option verwendet, um `MandarP` als Prinzipal anzugeben, von dem der ausführende Prinzipal die Berechtigung zum Erteilen oder Verweigern der Berechtigung ableitet.  
  
```sql  
USE master;  
DENY CREATE ENDPOINT TO ArifS AS MandarP;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY (Serverberechtigungen) (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE (Serverberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [Berechtigungshierarchie &#40;Datenbank-Engine &#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
