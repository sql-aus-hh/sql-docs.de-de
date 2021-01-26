---
title: Konfigurieren der Serverkonfigurationsoption „Remotezugriff“ | Microsoft-Dokumentation
description: Lernen Sie Alternativen zur veralteten Option „Remotezugriff“ kennen. Sehen Sie sich weitere Ressourcen zur Behebung von Problemen mit SQL Server-Verbindungen an.
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], stored procedure execution
ms.assetid: f5de748d-1c55-4714-9661-38fe62e5095f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: afe6721bef097dfa941b5578b6d4287fc370c3f2
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783129"
---
# <a name="configure-the-remote-access-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Remotezugriff
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird das Feature "Remotezugriff" erläutert. Diese Konfigurationsoption ist eine ungünstige und veraltete Kommunikationsfunktion für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], die Sie vermutlich nicht verwenden sollten. Wenn Sie zu dieser Seite gelangt, weil Sie Probleme beim Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]haben, lesen Sie stattdessen eines der folgenden Themen:  
  
-   [Tutorial: Erste Schritte mit der Datenbank-Engine](../../relational-databases/tutorial-getting-started-with-the-database-engine.md)  
  
-   [Anmelden an SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md)  
  
-   [Herstellen einer Verbindung mit SQL Server, wenn Systemadministratoren gesperrt sind](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)  
  
-   [Herstellen einer Verbindung zu einem registrierten Server &#40;SQL Server Management Studio&#41;](../../ssms/register-servers/connect-to-a-registered-server-sql-server-management-studio.md)  
  
-   [Herstellen einer Verbindung mit einer beliebigen SQL Server-Komponente aus SQL Server Management Studio](../../ssms/f1-help/connect-to-any-sql-server-component-from-sql-server-management-studio.md)  
  
-   [Herstellen einer Verbindung mit der Datenbank-Engine mithilfe von sqlcmd](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md)  
  
-   [So beheben Sie Verbindungsfehler mit der SQL Server-Datenbank-Engine](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
 Programmierer finden möglicherweise die folgenden Themen nützlich:  
  
-   [Vorgehensweise: Verbinden mit SQL Server mithilfe der SQL-Authentifizierung in ASP.NET 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10))  
  
-   [Herstellen einer Verbindung zu einer Instanz von SQL Server](../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)  
  
-   [Vorgehensweise: Erstellen von Verbindungen zu SQL Server-Datenbanken](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))  
  
 **Der Hauptteil dieses Themas beginnt hier.**  
  
 In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Remotezugriff** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Mithilfe der Option **Remotezugriff** können Sie die Ausführung gespeicherter Prozeduren von lokalen Servern oder Remoteservern steuern, auf denen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt werden. Der Standardwert für diese Option ist 1. Damit wird die Berechtigung zum Ausführen lokal gespeicherter Prozeduren von Remoteservern aus oder zum Ausführen remote gespeicherter Prozeduren vom lokalen Server aus erteilt. Legen Sie die Option auf 0 fest, um zu verhindern, dass lokal gespeicherte Prozeduren von einem Remoteserver aus ausgeführt werden oder dass remote gespeicherte Prozeduren auf dem lokalen Server ausgeführt werden können.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Verwenden Sie stattdessen [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Remotezugriff mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Remotezugriff“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Die Option **Remotezugriff** gilt nur für Server, die mithilfe von [sp_addserver](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)hinzugefügt wurden, und wird aus Gründen der Abwärtskompatibilität bereitgestellt.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-access-option"></a>So konfigurieren Sie die Option Remotezugriff  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den Knoten **Verbindungen** .  
  
3.  Aktivieren oder deaktivieren Sie unter **Remoteserververbindungen** das Kontrollkästchen **Remoteverbindungen mit diesem Server zulassen** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-remote-access-option"></a>So konfigurieren Sie die Option Remotezugriff  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um den Wert der Option `remote access` auf `0`festzulegen.  
  
```sql  
EXEC sp_configure 'remote access', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-remote-access-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Remotezugriff“  
 Diese Einstellung wird erst wirksam, wenn Sie SQL Server neu starten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
