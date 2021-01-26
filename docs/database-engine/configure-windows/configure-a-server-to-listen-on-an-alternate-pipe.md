---
title: Konfigurieren eines Servers für die Überwachung einer alternativen Pipe | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Named Pipe konfigurieren, an der SQL Server-Datenbank-Engine lauscht. Außerdem erfahren Sie, wie Sie eine Verbindung zwischen einer Clientanwendung und einer spezifischen Named Pipe herstellen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Named Pipes [SQL Server], configuring
- listening [SQL Server], pipes
- pipes [SQL Server], alternate
- alternate pipes [SQL Server]
ms.assetid: 914f7491-e2be-4b0d-b3aa-fe5409cdbafa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 70abda90ef4b7c047a4833baa792adeff60b8f12
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783224"
---
# <a name="configure-a-server-to-listen-on-an-alternate-pipe"></a>Konfigurieren eines Servers für die Überwachung einer alternativen Pipe
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie Sie einen Server konfigurieren können, um eine alternative Pipe in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe des SQL Server-Konfigurations-Managers überwachen zu können. Standardmäßig lauscht die Standardinstanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] an der Named Pipe \\\\.\pipe\sql\query. Benannte Instanzen von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] und [!INCLUDE[ssEW](../../includes/ssew-md.md)] überwachen andere Pipes.  
  
 Es gibt drei Möglichkeiten, mit einer Clientanwendung eine Verbindung mit einer bestimmten benannten Pipe herzustellen:  
  
-   Ausführen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser-Dienstes auf dem Server  
  
-   Erstellen eines Alias auf dem Client unter Angabe der benannten Pipe  
  
-   Programmieren Sie den Client so, dass die Verbindung mithilfe einer benutzerdefinierten Verbindungszeichenfolge hergestellt wird.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Verwenden des SQL Server-Konfigurations-Managers  
  
#### <a name="to-configure-the-named-pipe-used-by-the-sql-server-database-engine"></a>So konfigurieren Sie die von der SQL Server-Datenbank-Engine verwendete Named Pipe  
  
1.  Erweitern Sie im SQL Server-Konfigurations-Manager im Konsolenbereich **SQL Server-Netzwerkkonfiguration**, und klicken Sie dann auf **Protokolle für** *\<instance name>* .  
  
2.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **Named Pipes**, und klicken Sie dann auf **Eigenschaften**.  
  
3.  Geben Sie auf der Registerkarte **Protokoll** im Feld **Pipename** die Pipe ein, an der [!INCLUDE[ssDE](../../includes/ssde-md.md)] gelauscht werden soll, und klicken Sie dann auf **OK**.  
  
4.  Klicken Sie im linken Bereich auf **SQL Server-Dienste**.  
  
5.  Klicken Sie im Detailbereich mit der rechten Maustaste auf **SQL Server (** \<instance name> **)** , und klicken Sie dann auf **Neu starten**, um den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst zu beenden und neu zu starten.  
  
 Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine alternative Pipe überwacht, können Sie auf drei Arten mit einer Clientanwendung eine Verbindung zu einer bestimmten benannten Pipe herstellen:  
  
-   Ausführen des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser-Dienstes auf dem Server  
  
-   Erstellen eines Alias auf dem Client unter Angabe der benannten Pipe  
  
-   Programmieren Sie den Client so, dass die Verbindung mithilfe einer benutzerdefinierten Verbindungszeichenfolge hergestellt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen oder Löschen eines Serveralias für die Verwendung durch einen Client &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md)   
 [Server-Netzwerkkonfiguration](../../database-engine/configure-windows/server-network-configuration.md)  
  
  
