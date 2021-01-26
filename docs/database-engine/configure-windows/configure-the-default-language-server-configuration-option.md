---
title: Konfigurieren der Serverkonfigurationsoption „Standardsprache“ | Microsoft-Dokumentation
description: Hier lernen Sie die Option „Standardsprache“ kennen. Sie erfahren, wie Sie die Option konfigurieren, um die Standardsprache festzulegen, die SQL Server für alle neu erstellten Anmeldungen verwendet.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- default language option
ms.assetid: c08c26d8-5a62-487e-a4ee-4c529e4f9287
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 05bf39e18f7efaf8c4f47ca857824189906977ca
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783244"
---
# <a name="configure-the-default-language-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Standardsprache
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Standardsprache** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Verwenden Sie die Option **Standardsprache** , um die Standardsprache für alle neu erstellten Anmeldenamen anzugeben. Geben Sie zum Festlegen der Standardsprache den **langid** -Wert der gewünschten Sprache an. Sie können den **langid** -Wert abrufen, indem Sie die **sys.syslanguages** -Kompatibilitätssicht abfragen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Empfehlungen](#Recommendations)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Standardsprache mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Standardsprache“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Die Standardsprache für einen Anmeldenamen kann mit CREATE LOGIN oder ALTER LOGIN überschrieben werden. Die Sprache für den Anmeldenamen der Sitzung ist die Standardsprache für die Sitzung, es sei denn, dieser Wert wird für die jeweilige Sitzung mithilfe der Open Database Connectivity (ODBC)- oder OLE DB-APIs überschrieben. Beachten Sie, dass Sie die Option **Standardsprache** nur auf eine Sprachen-ID festlegen können, die in [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) (0-32) definiert ist. Bei Verwendung von eigenständigen Datenbanken kann eine Standardsprache mit CREATE DATABASE oder ALTER DATABASE für eine Datenbank und mit CREATE USER oder ALTER USER für Benutzer eigenständiger Datenbanken festgelegt werden. Beim Festlegen von Standardsprachen in einer enthaltenen Datenbank wird der **langid** -Wert, der Sprachenname oder ein in **sys.syslanguages** aufgelistetes Sprachalias akzeptiert.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-default-language-option"></a>So konfigurieren Sie die Option Standardsprache  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf die Registerkarte **Erweitert**.  
  
3.  Wählen Sie im Feld **Standardsprache** die Sprache aus, in der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Systemmeldungen anzeigen soll.  
  
     Die Standardsprache ist Deutsch.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-default-language-option"></a>So konfigurieren Sie die Option Standardsprache  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um die Option `default language` auf Französisch (`2`) festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'default language', 2 ;  
GO  
RECONFIGURE ;  
GO  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-default-language-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Standardsprache“  
 Die Einstellung tritt ohne Neustarten des Servers sofort in Kraft.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
