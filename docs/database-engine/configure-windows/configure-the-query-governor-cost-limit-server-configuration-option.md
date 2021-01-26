---
title: Konfigurieren der Serverkonfigurationsoption „Kostenbeschränkung der Abfragekontrolle“ | Microsoft-Dokumentation
description: Informationen zur Option „query governor cost limit“ Erfahren Sie, wie Sie die Ausführung von Abfragen begrenzen.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- queries [SQL Server], time to execute
- query governor cost limit option [SQL Server]
- time [SQL Server], query run time
ms.assetid: e7b8f084-1052-4133-959b-cebf4add790f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7004a0a7b7322fc939b5df21d40c389dc4a3dc6a
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783659"
---
# <a name="configure-the-query-governor-cost-limit-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Kostenbeschränkung der Abfragekontrolle
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Kostenbeschränkung der Abfragekontrolle** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Die Option zur Kostenbeschränkung legt eine Obergrenze für die geschätzten zulässigen Kosten für die Ausführung einer bestimmten Abfrage fest. Die Abfragekosten sind eine abstrakte Zahl, die vom Abfrageoptimierer basierend auf geschätzten Ausführungsanforderungen wie CPU-Zeit, Arbeitsspeichernutzung und Datenträger-E/A ermittelt wird. Sie beziehen sich auf eine geschätzte Zeit in Sekunden, die für das Ausführen einer Abfrage bei einer bestimmten Hardwarekonfiguration benötigt wird. Diese abstrakte Zahl entspricht nicht der Zeit, die für die vollständige Ausführung einer Abfrage auf der ausgeführten Instanz erforderlich ist. Sie sollte als relatives Measure behandelt werden. Der Standardwert für diese Option ist 0, mit dem die Abfragekontrolle deaktiviert wird. Durch Festlegen des Werts auf 0 können alle Abfragen ohne zeitliche Begrenzung ausgeführt werden. Wenn Sie einen nicht negativen Wert ungleich Null angeben, lässt die Abfragekontrolle die Ausführung von Abfragen nicht zu, deren geschätzte Kosten über diesem Wert liegen.   
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Empfehlungen](#Recommendations)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Kostenbeschränkung der Abfragekontrolle mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Kostenbeschränkung der Abfragekontrolle“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Diese Option ist eine erweiterte Option und sollte ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Experten geändert werden.  
  
-   Wenn Sie den Wert der Option Kostenbeschränkung der Abfragekontrolle jeweils für eine Verbindung ändern möchten, können Sie dazu die [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md) -Anweisung verwenden.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-query-governor-cost-limit-option"></a>So konfigurieren Sie die Option Kostenbeschränkung der Abfragekontrolle  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf die Seite **Verbindungen** .  
  
3.  Aktivieren bzw. deaktivieren Sie das Kontrollkästchen **Abfragekontrolle verwenden, um Abfragen mit langer Ausführungszeit zu verhindern** .  
  
     Wenn Sie dieses Kontrollkästchen aktivieren, geben Sie im Feld unten einen positiven Wert ein. Dieser wird von der Abfragekontrolle verwendet, um das Ausführen von Abfragen zu verhindern, deren geschätzte Kosten diesen Wert überschreiten.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-query-governor-cost-limit-option"></a>So konfigurieren Sie die Option Kostenbeschränkung der Abfragekontrolle  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Dieses Beispiel zeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um den Wert der Option `query governor cost limit` auf eine Obergrenze von `120` Bytes für die geschätzten Kosten festzulegen.
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'query governor cost limit', 120 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-query-governor-cost-limit-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Kostenbeschränkung der Abfragekontrolle“  
 Die Einstellung tritt ohne Neustarten des Servers sofort in Kraft.  
  
## <a name="see-also"></a>Weitere Informationen  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
