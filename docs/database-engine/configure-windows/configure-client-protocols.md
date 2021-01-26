---
title: Konfigurieren von Clientprotokollen | Microsoft-Dokumentation
description: Hier finden Sie verschiedene Möglichkeiten zum Konfigurieren der Protokolle, die von Clientanwendungen in SQL Server verwendet werden. Zu den unterstützten Protokollen gehören TCP/IP, Named Pipes und Shared Memory.
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- default protocols
- network protocols [SQL Server], client configuration
- TCP/IP [SQL Server], client protocols
- disabling client protocols
- ordering protocols [SQL Server]
- protocols [SQL Server], order for client computers
- configure client protocols
- client protocols [SQL Server]
- protocols [SQL Server], client configuration
- default protocols, client
ms.assetid: 3dfa2702-ba65-43b4-a777-6727846e133a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6651a3c0d5985e3fa848c3fd02cc0971cb87db0c
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783302"
---
# <a name="configure-client-protocols"></a>Konfigurieren von Clientprotokollen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird die Konfiguration von Clientprotokollen beschrieben, die mithilfe des [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Konfigurations-Managers von Clientanwendungen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt die Clientkommunikation mit TCP/IP und dem Named Pipes-Protokoll. Auch das Shared Memory-Protokoll steht zur Verfügung, wenn der Client eine Verbindung mit einer [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz auf demselben Computer herstellt. Zum Auswählen des Protokolls stehen drei allgemeine Methoden zur Verfügung.  
  
-   Das Konfigurieren aller Clientanwendungen für dasselbe Netzwerkprotokoll durch Festlegen der Protokollreihenfolge im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager.  
  
-   Das Konfigurieren einer einzelnen Clientanwendung zum Verwenden eines anderen Netzwerkprotokolls durch Erstellen eines Alias. Weitere Informationen finden Sie unter [Erstellen oder Löschen eines Serveralias für die Verwendung durch einen Client &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
-   In einigen Clientanwendungen, beispielsweise sqlcmd.exe, kann das Protokoll als Teil der Verbindungszeichenfolge angegeben werden. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der Datenbank-Engine mithilfe von sqlcmd](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md).  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Verwenden des SQL Server-Konfigurations-Managers  
  
###  <a name="to-enable-or-disable-a-client-protocol"></a><a name="EnableDisable"></a> So aktivieren oder deaktivieren Sie ein Clientprotokoll  
  
1.  Erweitern Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager **SQL Server Native Client-Konfiguration**, klicken Sie mit der rechten Maustaste auf **Clientprotokolle**, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Um ein Protokoll zu aktivieren, klicken Sie auf das gewünschte Protokoll im Feld **Deaktivierte Protokolle** , und klicken Sie dann auf **Aktivieren**.  
  
3.  Um ein Protokoll zu deaktivieren, klicken Sie auf das gewünschte Protokoll im Feld **Aktivierte Protokolle** , und klicken Sie dann auf **Deaktivieren**.  
  
###  <a name="to-change-the-default-protocol-or-the-protocol-order-for-client-computers"></a><a name="ChangeDefault"></a> So ändern Sie das Standardprotokoll oder die Protokollreihenfolge für Clientcomputer  
  
1.  Erweitern Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager **SQL Server Native Client-Konfiguration**, klicken Sie mit der rechten Maustaste auf **Clientprotokolle**, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Soll die Reihenfolge der Protokolle beim Aufbauen einer Verbindung mit **geändert werden, klicken Sie im Feld** Aktivierte Protokolle **auf** Nach oben **bzw.** Nach unten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Das obere Protokoll im Feld **Aktivierte Protokolle** ist das Standardprotokoll.  
  
    > [!IMPORTANT]  
    >  Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager erstellt Registrierungseinträge für die Serveraliaskonfigurationen und die Standard-Netzwerkbibliothek des Clients. Die Anwendung installiert jedoch weder die Clientnetzwerkbibliotheken noch die Netzwerkprotokolle von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clientnetzwerkbibliotheken werden im Rahmen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Setup installiert. Die Netzwerkprotokolle werden im Rahmen von Microsoft Windows Setup installiert (oder über die Anwendung **Netzwerk** in der **Systemsteuerung**). Als Teil von Windows Setup steht möglicherweise kein spezielles Netzwerkprotokoll zur Verfügung. Weitere Informationen zum Installieren dieser Netzwerkprotokolle finden Sie in der Dokumentation des Herstellers.  
  
###  <a name="to-configure-a-client-to-use-tcpip"></a><a name="Configure"></a> So konfigurieren Sie einen Client für die Verwendung von TCP/IP  
  
1.  Erweitern Sie im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager **SQL Server Native Client-Konfiguration**, klicken Sie mit der rechten Maustaste auf **Clientprotokolle**, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Klicken Sie im Feld **Aktivierte Protokolle** auf die Pfeile, um die Reihenfolge zu ändern, in der mit den Protokollen versucht werden soll, eine Verbindung zu SQL Server herzustellen. Das obere Protokoll im Feld **Aktivierte Protokolle** ist das Standardprotokoll.  
  
 Das Shared Memory-Protokoll wird getrennt durch Aktivieren des Kästchens **Shared Memory-Protokoll aktivieren** aktiviert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren des Timeouts für Remoteanmeldungen (Serverkonfigurationsoption)](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md)  
  
