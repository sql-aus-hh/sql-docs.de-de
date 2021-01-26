---
title: Konfigurieren der Serverkonfigurationsoption „Netzwerkpaketgröße“ | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Option „network packet size“ verwenden, um die Paketgröße festzulegen, die SQL Server verwendet, wenn Anforderungen und Ergebnisse zwischen Clients und Servern übertragen werden.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- default packet size
- size [SQL Server], packets
- packets [SQL Server], size
- network packet size option
ms.assetid: 236985bf-fc4a-4a57-98f7-a71ef977fd7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8cb8e2b3377c4b90e61e9b65225d3d350bfa9897
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783624"
---
# <a name="configure-the-network-packet-size-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Netzwerkpaketgröße
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Netzwerkpaketgröße** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Mit der Option **Netzwerkpaketgröße** können Sie die Paketgröße (in Byte) festlegen, die innerhalb des gesamten Netzwerks verwendet wird. Pakete sind die Datenabschnitte fester Größe, die Anforderungen und Ergebnisse zwischen Clients und Servern übertragen. Die Standardpaketgröße beträgt 4.096 Bytes.  
  
> [!NOTE]  
>  Sie sollten die Paketgröße nur dann ändern, wenn Sie sicher sind, dass die Leistung dadurch verbessert werden kann. Für die meisten Anwendungen empfiehlt sich die Standardpaketgröße.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Empfehlungen](#Recommendations)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie die Option Netzwerkpaketgröße mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Nachverfolgung:**  [Nach dem Konfigurieren der Option „Netzwerkpaketgröße“](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Die maximale Netzwerkpaketgröße für verschlüsselte Verbindungen beträgt 16.383 Bytes.  
  
> [!NOTE]  
> Wenn MARS aktiviert ist, fügt der SMUX-Anbieter dem Paket vor der TLS-Verschlüsselung einen 16-Byte-Header hinzu, wodurch die maximale Netzwerkpaketgröße auf 16368 Bytes verringert wird.
   
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Diese Option ist eine erweiterte Option und sollte ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Experten geändert werden.  
  
-   Wenn eine Anwendung Massenkopiervorgänge ausführt oder große Mengen an Daten vom Typ text oder image sendet bzw. empfängt, kann eine über der Standardgröße liegende Paketgröße die Effizienz verbessern, da weniger Netzwerklesevorgänge und -schreibvorgänge ausgeführt werden. Sendet und empfängt eine Anwendung wenige Informationen, können Sie die Paketgröße auf 512 Bytes festlegen. Dies ist für die meisten Datenübertragungen ausreichend.  
  
-   Bei Systemen, die verschiedene Netzwerkprotokolle verwenden, sollten Sie die Option „Netzwerkpaketgröße“ auf die Größe festlegen, die das am häufigsten verwendete Protokoll vorgibt. Wenn Netzwerkprotokolle größere Pakete unterstützen, kann durch Netzwerkpaketgröße die Netzwerkleistung verbessert werden. Clientanwendungen können diesen Wert überschreiben.  
  
-   Sie können auch die Funktionen von OLE DB, ODBC (Open Database Connectivity) und der DB-Library aufrufen, um eine Änderung der Paketgröße anzufordern. Wenn der Server die angeforderte Paketgröße nicht unterstützt, sendet [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Warnmeldung an den Client. Unter gewissen Umständen führt das Ändern der Paketgröße möglicherweise zu einem Kommunikationsverbindungsfehler, wie beispielsweise:  
  
     `Native Error: 233, no process is on the other end of the pipe.`  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss einem Benutzer die ALTER SETTINGS-Berechtigung auf Serverebene erteilt worden sein. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-the-network-packet-size-option"></a>So konfigurieren Sie die Option Netzwerkpaketgröße  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den **Erweitert** -Knoten.  
  
3.  Wählen Sie unter **Netzwerk** einen Wert im Feld **Netzwerkpaketgröße** aus.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-network-packet-size-option"></a>So konfigurieren Sie die Option Netzwerkpaketgröße  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um den Wert der Option `network packet size` auf `6500` Byte festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'network packet size', 6500 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)angezeigt oder konfiguriert wird.  
  
##  <a name="follow-up-after-you-configure-the-network-packet-size-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Netzwerkpaketgröße“  
 Die Einstellung tritt ohne Neustarten des Servers sofort in Kraft.  
  
## <a name="see-also"></a>Weitere Informationen  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
