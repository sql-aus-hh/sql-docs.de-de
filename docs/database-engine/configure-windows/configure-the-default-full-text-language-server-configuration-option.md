---
title: Konfigurieren der Serverkonfigurationsoption „Volltext-Standardsprache“ | Microsoft-Dokumentation
description: In diesem Artikel wird die Option „Volltext-Standardsprache“ vorgestellt. Sie erfahren, wie Sie die Option konfigurieren, um die Standardsprache festzulegen, die SQL Server für Volltextindizes verwendet.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- default full-text language option
ms.assetid: 0fa8785b-0830-4a52-aff5-fcf8268b72fc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45e05505444d00607cae875b8dfd8bd07bf9190e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783240"
---
# <a name="configure-the-default-full-text-language-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Volltext-Standardsprache
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Volltext-Standardsprache** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Mit der Option **Volltext-Standardsprache** können Sie einen Standardsprachenwert für Volltextindizes angeben. Für alle Daten, die Volltextindizes aufweisen, wird eine linguistische Analyse ausgeführt, die von der Sprache der Daten abhängt. Der Standardwert für diese Option ist die Sprache des Servers. Bei einer lokalisierten Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wird die Option [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] default full-text language **vom** -Setup auf die Sprache des Servers festgelegt, falls eine geeignete Übereinstimmung vorhanden ist. Bei einer nicht lokalisierten Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wird Englisch für die Option **Standard-Volltextsprache** verwendet.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Empfehlungen](#Recommendations)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Volltext-Standardsprache mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Volltext-Standardsprache“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Der Wert der Option **Volltext-Standardsprache** wird in einem Volltextindex verwendet, wenn für eine Spalte keine Sprache über die Option LANGUAGE **language_term** in der CREATE FULLTEXT INDEX- bzw. der ALTER FULLTEXT INDEX-Anweisung angegeben wurde. Wenn die Volltext-Standardsprache nicht unterstützt wird oder das Sprachanalysepaket nicht verfügbar ist, schlägt die CREATE- oder ALTER-Operation fehl, und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt eine Fehlermeldung zurück, die besagt, dass die angegebene Sprache ungültig ist.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Diese Option ist eine erweiterte Option und sollte ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Experten geändert werden.  
  
-   Die Option **Volltext-Standardsprache** erfordert einen LCID-Wert. Eine Liste mit unterstützten LCIDs und den dazugehörigen Sprachen finden Sie unter [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)konfiguriert wird. Andere Sprachen können beispielsweise von unabhängigen Softwareherstellern verfügbar sein. Wenn keine spezielle Sprache gefunden wird, schaltet die Volltextsuch-Engine automatisch in die primäre Sprache.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>So konfigurieren Sie die Option Volltext-Standardsprache  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den **Erweitert** -Knoten.  
  
3.  Verwenden Sie unter Verschiedenes die Option **Volltext-Standardsprache** , um einen Wert für die Standardsprache für volltextindizierte Spalten anzugeben.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>So konfigurieren Sie die Option Volltext-Standardsprache  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um den Wert der Option `default full-text` auf Holländisch (`1043`) festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'default full-text language', 1043 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-default-full-text-language-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Volltext-Standardsprache“  
 Die Einstellung tritt ohne Neustarten des Servers sofort in Kraft.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
