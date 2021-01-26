---
title: Anzeigen oder Konfigurieren von Verbindungsoptionen für Remoteserver (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Verbindungsoptionen für Remoteserver auf Serverebene anzeigen oder konfigurieren. Zu diesem Zweck können Sie SQL Server Management Studio oder Transact-SQL verwenden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], connection options
- servers [SQL Server], remote
- connections [SQL Server], remote servers
ms.assetid: 356d3e6b-8514-4bd2-a683-9de147949b2b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7505c7ce47f0804f2c9cf83951a5d47847edf18b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783024"
---
# <a name="view-or-configure-remote-server-connection-options-sql-server"></a>Anzeigen oder Konfigurieren von Verbindungsoptionen für Remoteserver (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie Verbindungsoptionen für Remoteserver auf Serverebene in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]anzeigen oder konfigurieren können.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **So zeigen Sie Verbindungsoptionen für Remoteserver an oder konfigurieren sie mit**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren von Verbindungsoptionen für Remoteserver](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Für das Ausführen von **sp_serveroption** sind ALTER ANY LINKED SERVER-Berechtigungen auf dem Server erforderlich.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-view-or-configure-remote-server-connection-options"></a>So zeigen Sie Verbindungsoptionen für Remoteserver an oder konfigurieren sie  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Klicken Sie im Dialogfeld **Eigenschaften von SQL Server - \<**_server_name_**>** auf **Verbindungen**.  
  
3.  Überprüfen Sie auf der Seite **Verbindungen** die Einstellungen für **Remoteserververbindungen** , und ändern Sie sie gegebenenfalls.  
  
4.  Wiederholen Sie Schritt 1 bis 3 auf dem zweiten Server des Remoteserverpaares.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-view-remote-server-connection-options"></a>So zeigen Sie Verbindungsoptionen für Remoteserver an  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel werden von [sp_helpserver](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md) Informationen über alle Remoteserver zurückgegeben.  
  
```sql  
USE master;  
GO  
EXEC sp_helpserver ;  
```  
  
#### <a name="to-configure-remote-server-connection-options"></a>So konfigurieren Sie Verbindungsoptionen für Remoteserver  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_serveroption](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md) zum Konfigurieren eines Remoteservers verwendet wird. In diesem Beispiel wird ein Remoteserver, der einer anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz ( `SEATTLE3`) entspricht, so konfiguriert, dass seine Sortierung mit der Sortierung der lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz kompatibel ist.  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
##  <a name="follow-up-after-you-configure-remote-server-connection-options"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren von Verbindungsoptionen für Remoteserver  
 Der Remoteserver muss angehalten und neu gestartet werden, damit die Einstellung wirksam werden kann.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Remoteserver](../../database-engine/configure-windows/remote-servers.md)   
 [Verbindungsserver &#40;Datenbank-Engine&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)   
 [sp_linkedservers (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_helpserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)  
  
  
