---
description: ALTER ENDPOINT (Transact-SQL)
title: ALTER ENDPOINT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER ENDPOINT
- ALTER_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ENDPOINT statement
- modifying endpoints
- endpoints [SQL Server], modifying
ms.assetid: 70f35566-30cf-47c6-8394-dfe5d71629d3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 361f91c88f597b18324d56ac3d331899bd73881c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099552"
---
# <a name="alter-endpoint-transact-sql"></a>ALTER ENDPOINT (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Ermöglicht das Ändern eines vorhandenen Endpunktes auf die folgenden Arten:  
  
-   Durch Hinzufügen einer neuen Methode zu einem vorhandenen Endpunkt.  
  
-   Durch Ändern oder Löschen einer vorhandenen Methode aus dem Endpunkt.  
  
-   Durch Ändern der Eigenschaften eines Endpunkts.  
  
> [!NOTE]  
>  In diesem Abschnitt werden die Syntax und die Argumente beschrieben, die für ALTER ENDPOINT spezifisch sind. Beschreibungen der Argumente, die sowohl CREATE ENDPOINT als auch ALTER ENDPOINT gemeinsam sind, finden Sie unter [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 Systemeigene XML-Webdienste (SOAP/HTTP-Endpunkte) wurden ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] entfernt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
ALTER ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
[ AS { TCP } ( <protocol_specific_items> ) ]  
[ FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_items>  
        ) ]  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( 4-part-ip ) | ( "ip_address_v6" ) ]  
)  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
      WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]  
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ , ENCRYPTION = { DISABLED   
       |   
         {{SUPPORTED | REQUIRED }   
       [ ALGORITHM { RC4 | AES | AES RC4 | RC4 AES } ] }   
   ]  
  
  [ , MESSAGE_FORWARDING = {ENABLED | DISABLED} ]  
  [ , MESSAGE_FORWARD_SIZE = forwardSize  
)  
  
<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
      WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]  
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ , ENCRYPTION = { DISABLED   
       |   
         {{SUPPORTED | REQUIRED }   
       [ ALGORITHM { RC4 | AES | AES RC4 | RC4 AES } ] }   
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
  
> [!NOTE]  
>  Die folgenden Argumente gelten nur für ALTER ENDPOINT. Beschreibungen der restlichen Argumente finden Sie unter [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 **AS** { **TCP** }  
 Sie können das Transportprotokoll mit **ALTER ENDPOINT** nicht ändern.  
  
 **AUTHORIZATION** *login*  
 Die **AUTHORIZATION**-Option ist in **ALTER ENDPOINT** nicht verfügbar. Besitz kann nur zugewiesen werden, wenn der Endpunkt erstellt wird.  
  
 **FOR** { **TSQL** | **SERVICE_BROKER** | **DATABASE_MIRRORING** }  
 Sie können den Nutzlasttyp mit **ALTER ENDPOINT** nicht ändern.  
  
## <a name="remarks"></a>Bemerkungen  
 Geben Sie bei Verwendung von ALTER ENDPOINT nur die zu aktualisierenden Parameter an. Alle Eigenschaften eines vorhandenen Endpunktes bleiben unverändert, es sei denn, Sie ändern diese explizit.  
  
 ENDPOINT DDL-Anweisungen können nicht innerhalb einer Benutzertransaktion ausgeführt werden.  
  
 Weitere Informationen zum Auswählen eines Verschlüsselungsalgorithmus zur Verwendung mit einem Endpunkt finden Sie unter [Auswählen eines Verschlüsselungsalgorithmus](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
> [!NOTE]  
>  Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höheren Versionen kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.  
>   
>  RC4 ist ein relativ schwacher Algorithmus, und AES ist ein relativ starker Algorithmus. AES ist jedoch deutlich langsamer als RC4. Wenn Sicherheit für Sie eine höhere Priorität besitzt als Geschwindigkeit, ist es empfehlenswert, AES zu verwenden.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer muss ein Mitglied der festen Serverrolle **sysadmin** sein, er muss der Besitzer des Endpunktes sein, oder ihm muss die ALTER ANY ENDPOINT-Berechtigung erteilt worden sein.  
  
 Sie können den Besitz für einen vorhandenen Endpunkt ändern, indem Sie die ALTER AUTHORIZATION-Anweisung verwenden. Weitere Informationen finden Sie unter [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Weitere Informationen finden Sie unter [GRANT (Endpunktberechtigungen) &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [DROP ENDPOINT (Transact-SQL)](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
