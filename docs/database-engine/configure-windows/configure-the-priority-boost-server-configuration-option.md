---
title: Konfigurieren der Serverkonfigurationsoption „Prioritätserhöhung“ | Microsoft-Dokumentation
description: Informationen zur Option „priority boost“ Hier erfahren Sie, wie Sie die Basispriorität für SQL Server im Scheduler von Windows 2008 oder Windows Server 2008 R2 festlegen.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- priority boost option
ms.assetid: 765f1e83-dd52-44fb-b0c8-1078f213607b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c8889fd32fc0d494260bf73e790cdb3a428201d4
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783626"
---
# <a name="configure-the-priority-boost-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Prioritätserhöhung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie die Konfigurationsoption **Prioritätserhöhung** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Verwenden Sie die Option **Prioritätserhöhung**, um anzugeben, ob [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit einer höheren Planungspriorität von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 2008 bzw. Windows 2008 R2 als andere Prozesse auf demselben Computer ausgeführt werden soll. Wenn Sie diese Option auf 1 festlegen, wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit der Basispriorität 13 im Windows 2008- oder Windows Server 2008 R2-Taskplaner ausgeführt. Der Standardwert ist 0, was der Basispriorität 7 entspricht.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Prioritätserhöhung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Prioritätserhöhung“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Wird die Priorität zu stark angehoben, können dadurch Ressourcen von wichtigen Betriebssystem- und Netzwerkfunktionen abgezogen werden, was zu Problemen beim Herunterfahren von SQL Server oder bei der Verwendung anderer Betriebssystemtasks auf dem Server führen kann.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-priority-boost-option"></a>So konfigurieren Sie die Option Prioritätserhöhung  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den Knoten **Prozessoren** .  
  
3.  Aktivieren Sie unter **Threads** das Kontrollkästchen **SQL Server-Priorität höher stufen** .  
  
4.  Schließen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], und starten Sie das Programm neu.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-priority-boost-option"></a>So konfigurieren Sie die Option Prioritätserhöhung  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um den Wert der Option `priority boost` auf `1`festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'priority boost', 1 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-priority-boost-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Prioritätserhöhung“  
 Der Server muss neu gestartet werden, bevor die Einstellung wirksam werden kann.  
  
## <a name="see-also"></a>Weitere Informationen  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
