---
description: ALTER SERVICE (Transact-SQL)
title: ALTER SERVICE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER SERVICE
- ALTER_SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying services
- contracts [Service Broker], modifying
- ALTER SERVICE statement
- services [Service Broker], modifying
ms.assetid: 2b4608f7-bb2e-4246-aa29-b52c55995b3a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 746e1aa7215acd584e6512c3e39fb79014b7821b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100259"
---
# <a name="alter-service-transact-sql"></a>ALTER SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ändert einen vorhandenen Dienst.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql 
ALTER SERVICE service_name   
   [ ON QUEUE [ schema_name . ]queue_name ]   
   [ ( < opt_arg > [ , ...n ] ) ]  
[ ; ]  
  
<opt_arg> ::=  
   ADD CONTRACT contract_name | DROP CONTRACT contract_name  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *service_name*  
 Der Name des zu ändernden Diensts. Server-, Datenbank- und Schemaname können nicht angegeben werden.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Gibt die neue Warteschlange für diesen Dienst an. [!INCLUDE[ssSB](../../includes/sssb-md.md)] verschiebt alle Meldungen für diesen Dienst aus der aktuellen Warteschlange in die neue.  
  
 ADD CONTRACT *contract_name*  
 Gibt einen Vertrag an, der dem durch diesen Dienst verfügbar gemachten Vertragssatz hinzugefügt werden soll.  
  
 DROP CONTRACT *contract_name*  
 Gibt einen Vertrag an, der aus dem von diesem Dienst verfügbar gemachten Vertragssatz gelöscht werden soll. [!INCLUDE[ssSB](../../includes/sssb-md.md)] sendet eine Fehlermeldung die vorhandene Konversationen mit diesem Dienst, die diesen Vertrag verwenden.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn mithilfe der ALTER SERVICE-Anweisung ein Vertrag aus einem Dienst gelöscht wird, kann der Dienst kein Ziel für Konversationen mehr sein, die diesen Vertrag verwenden. Deshalb lässt [!INCLUDE[ssSB](../../includes/sssb-md.md)] keine neuen Konversationen mit dem Dienst für diesen Vertrag zu. Bestehende Konversationen, die den Vertrag verwenden, sind davon nicht betroffen.  
  
 Verwenden Sie die ALTER AUTHORIZATION-Anweisung, wenn Sie AUTHORIZATION für einen Dienst ändern möchten.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Berechtigung zum Ändern eines Diensts erhalten standardmäßig die Besitzer des Diensts, Mitglieder der festen Datenbankrollen **db_ddladmin** oder **db_owner** sowie Mitglieder der festen Serverrolle **sysadmin**.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-queue-for-a-service"></a>A. Ändern der Warteschlange für einen Dienst  
 Im folgenden Beispiel wird der `//Adventure-Works.com/Expenses`-Dienst so geändert, dass er die Warteschlange `NewQueue` verwendet.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE NewQueue ;  
```  
  
### <a name="b-adding-a-new-contract-to-the-service"></a>B. Hinzufügen eines neuen Vertrags zum Dienst  
 Im folgenden Beispiel wird der `//Adventure-Works.com/Expenses`-Dienst so geändert, dass Dialoge für den Vertrag `//Adventure-Works.com/Expenses` zugelassen sind.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="c-adding-a-new-contract-to-the-service-dropping-existing-contract"></a>C. Hinzufügen eines neuen Vertrags zum Dienst und Löschen des vorhandenen Vertrags  
 Im folgenden Beispiel wird der `//Adventure-Works.com/Expenses`-Dienst so geändert, dass Dialoge für den Vertrag `//Adventure-Works.com/Expenses/ExpenseProcessing` zugelassen sind und Dialoge für den Vertrag `//Adventure-Works.com/Expenses/ExpenseSubmission` nicht zugelassen sind.  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing],   
     DROP CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
