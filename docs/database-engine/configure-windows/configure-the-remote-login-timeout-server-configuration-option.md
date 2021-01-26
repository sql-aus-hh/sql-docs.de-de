---
title: Konfigurieren der Serverkonfigurationsoption „Timeout für Remoteanmeldung“ | Microsoft-Dokumentation
description: Information zur Option „remote login timeout“ Hier erfahren Sie, wie diese Option die Anzahl der Sekunden begrenzt, die SQL Server zum Herstellen einer Verbindung mit einem Remoteserver zuweist.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote login timeout option
ms.assetid: 077adebe-0e3f-42a5-a75e-5e6d04847e2b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ffc46323f8f3fb783b4a06836f66c535e145a04e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783117"
---
# <a name="configure-the-remote-login-timeout-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Timeout für Remoteanmeldungen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Timeout für Remoteanmeldung** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Mit der Option **Timeout für Remoteanmeldung** können Sie den Zeitraum in Sekunden angeben, wie lange gewartet werden soll, bevor ein Remoteanmeldeversuch einen Fehler erzeugt. Wenn Sie z. B versuchen, sich an einem Remoteserver anzumelden, und dieser Server derzeit nicht betriebsbereit ist, wird durch **Timeout für Remoteanmeldung** sichergestellt, dass Sie nicht unbegrenzt warten müssen, bis der Computer seine Anmeldeversuche beendet. Der Standardwert für diese Option ist 10 Sekunden. Bei einem Wert von 0 ist eine unbegrenzte Wartezeit zulässig.  
  
> [!NOTE]  
>  Der Standardwert für diese Option in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]ist 20 Sekunden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Timeout für Remoteanmeldung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Timeout für Remoteanmeldung“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Die Option **Timeout für Remoteanmeldung** wirkt sich auf Verbindungen mit OLE DB-Anbieter aus, die für heterogene Abfragen hergestellt wurden.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-login-timeout-option"></a>So konfigurieren Sie die Option Timeout für Remoteanmeldung  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den **Erweitert** -Knoten.  
  
3.  Wählen Sie unter **Netzwerk** einen Wert im Feld **Timeout für Remoteanmeldung** aus.  
  
     Sie können mithilfe der Option **Timeout für Remoteanmeldung** den Zeitraum in Sekunden angeben, wie lange gewartet werden soll, bevor für einen Remoteanmeldeversuch ein Fehler auftritt.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-remote-login-timeout-option"></a>So konfigurieren Sie die Option Timeout für Remoteanmeldung  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um den Wert der Option `remote login timeout` auf `35` Sekunden festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'remote login timeout', 35 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-remote-login-timeout-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Timeout für Remoteanmeldung“  
 Die Einstellung tritt ohne Neustarten des Servers sofort in Kraft.  
  
## <a name="see-also"></a>Weitere Informationen  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
