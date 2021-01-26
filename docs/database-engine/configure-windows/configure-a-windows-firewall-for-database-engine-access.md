---
title: Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Windows-Firewall so konfigurieren, dass Clientcomputer durch die Firewall auf eine Instanz von SQL Server-Datenbank-Engine zugreifen können.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], firewall systems
- firewall systems, [Database Engine]
- security [SQL Server], firewalls
ms.assetid: 0093b43c-c6b5-4574-9b30-3a0e91e1a1f9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 03f4a469465339664e56aae784c3b734f85ba9b4
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765696"
---
# <a name="configure-a-windows-firewall-for-database-engine-access"></a>Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


  In diesem Thema wird beschrieben, wie eine Windows-Firewall für Datenbank-Engine-Zugriff in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe des SQL Server-Konfigurations-Managers konfiguriert wird. Durch Firewallsysteme kann der nicht autorisierte Zugriff auf Computerressourcen verhindert werden. Um über eine Firewall auf eine Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] zugreifen zu können, müssen Sie die Firewall auf dem Computer mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entsprechend konfigurieren.  
  
 Weitere Informationen zu den Standardeinstellungen der Windows-Firewall und eine Beschreibung der TCP-Ports, die sich auf das [!INCLUDE[ssDE](../../includes/ssde-md.md)], Analysis Services, Reporting Services und Integration Services auswirken, finden Sie unter [Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md). Es gibt zahlreiche verschiedene Firewallsysteme auf dem Markt. Informationen zu den für Ihr System relevanten Einstellungen finden Sie in der Firewalldokumentation.  
  
 Die wichtigsten Schritte zum Konfigurieren des Zugriffs werden im Folgenden beschrieben:  
  
1.  Konfigurieren Sie [!INCLUDE[ssDE](../../includes/ssde-md.md)] für die Verwendung eines bestimmten TCP/IP-Ports. In der Standardinstanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] wird Port 1433 verwendet. Sie können diese Einstellung jedoch ändern. Der vom [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendete Port ist im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll aufgeführt. Instanzen von [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], [!INCLUDE[ssEW](../../includes/ssew-md.md)] und benannte Instanzen von [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwenden dynamische Ports. Informationen zum Konfigurieren dieser Instanzen zum Verwenden eines bestimmten Ports finden Sie unter [Konfigurieren eines Servers zur Überwachung eines bestimmten TCP-Ports &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
2.  Konfigurieren Sie die Firewall so, dass autorisierten Benutzern oder Computern der Zugriff auf diesen Port gewährt wird.  
  
> [!NOTE]  
>  Mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser-Dienst können Benutzer ohne Kenntnis der Portnummer Verbindungen mit Instanzen von [!INCLUDE[ssDE](../../includes/ssde-md.md)] herstellen, die nicht den Port 1433 überwachen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser verwenden möchten, müssen Sie UDP-Port 1434 öffnen. Um eine möglichst sichere Umgebung zu erzielen, lassen Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser-Dienst beendet, und konfigurieren Sie die Clients so, dass sie Verbindungen mithilfe der Portnummer herstellen müssen.  
  
> [!NOTE]  
>  Standardmäßig wird die Windows-Firewall von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows aktiviert. Damit wird Port 1433 geschlossen, um zu verhindern, dass Internetcomputer Verbindungen mit einer Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Ihrem Computer herstellen. Verbindungen zur Standardinstanz über TCP/IP sind nicht möglich, außer wenn Sie Port 1433 erneut öffnen. Die grundlegenden Schritte für die Konfiguration der Windows-Firewall werden in den folgenden Verfahren beschrieben. Weitere Informationen finden Sie in der Windows-Dokumentation.  
  
 Als Alternative zum Konfigurieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für das Lauschen an einem festen Port und zum Öffnen des Ports können Sie die ausführbare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datei (Sqlservr.exe) als Ausnahme in die Liste der blockierten Programme aufnehmen. Verwenden Sie diese Methode, wenn Sie weiterhin dynamische Ports verwenden möchten. Auf diese Weise kann nur auf eine einzige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugegriffen werden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff mithilfe von:**  
  
     [SQL Server-Konfigurations-Manager](#SSMSProcedure)  
  
## <a name="before-you-begin"></a>Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
 Das Öffnen von Ports in der Firewall kann dazu führen, dass der Server böswilligen Angriffen ausgesetzt ist. Daher sollten Sie Ports grundsätzlich nur dann öffnen, wenn Sie sicher sind, dass Sie das Konzept von Firewallsystemen verstanden haben. Weitere Informationen finden Sie unter [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Verwenden des SQL Server-Konfigurations-Managers  
 Gilt für: Windows Vista, Windows 7 und Windows Server 2008  
  
 Anhand der folgenden Prozeduren wird die Windows-Firewall unter Verwendung des MMC-Snap-Ins (Microsoft Management Console) "Windows-Firewall mit erweiterter Sicherheit" konfiguriert. Von der Windows-Firewall mit erweiterter Sicherheit wird nur das aktuelle Profil konfiguriert. Weitere Informationen über die Windows-Firewall mit erweiterter Sicherheit finden Sie unter [Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
#### <a name="to-open-a-port-in-the-windows-firewall-for-tcp-access"></a>So öffnen Sie einen Port in der Windows-Firewall für den TCP-Zugriff  
  
1.  Klicken Sie im Menü **Start** auf **Ausführen**, geben Sie **WF.msc** ein, und klicken Sie anschließend auf **OK**.  
  
2.  Klicken Sie im linken Bereich von **Windows-Firewall mit erweiterter Sicherheit** mit der rechten Maustaste auf **Eingehende Regeln**, und klicken Sie anschließend im Aktionsbereich auf **Neue Regel** .  
  
3.  Wählen Sie im Dialogfeld **Regeltyp** die Option **Port** aus, und klicken Sie anschließend auf **Weiter**.  
  
4.  Wählen Sie im Dialogfeld **Protokoll und Ports** die Option **TCP** aus. Wählen Sie **Bestimmte lokale Ports** aus, und geben Sie anschließend die Portnummer der Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)]ein, z.B. **1433** für die Standardinstanz. Klicken Sie auf **Weiter**.  
  
5.  Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie anschließend auf **Weiter**.  
  
6.  Wählen Sie im Dialogfeld **Profil** beliebige Profile aus, die die Verbindungsumgebung des Computers beschreiben, wenn Sie eine Verbindung zum [!INCLUDE[ssDE](../../includes/ssde-md.md)]herstellen möchten, und klicken Sie dann auf **Weiter**.  
  
7.  Geben Sie im Dialogfeld **Name** einen Namen und eine Beschreibung für diese Regel ein, und klicken Sie dann auf **Fertig stellen**.  
  
#### <a name="to-open-access-to-sql-server-when-using-dynamic-ports"></a>So ermöglichen Sie den Zugriff auf SQL Server bei der Verwendung von dynamischen Ports  
  
1.  Klicken Sie im Menü **Start** auf **Ausführen**, geben Sie **WF.msc** ein, und klicken Sie anschließend auf **OK**.  
  
2.  Klicken Sie im linken Bereich von **Windows-Firewall mit erweiterter Sicherheit** mit der rechten Maustaste auf **Eingehende Regeln**, und klicken Sie anschließend im Aktionsbereich auf **Neue Regel** .  
  
3.  Wählen Sie im Dialogfeld **Regeltyp** die Option **Programm** aus, und klicken Sie anschließend auf **Weiter**.  
  
4.  Wählen Sie im Dialogfeld **Programm** die Option **Dieser Programmpfad** aus. Klicken Sie auf **Durchsuchen**, navigieren Sie zu der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , auf die Sie durch die Firewall hindurch zugreifen möchten, und klicken Sie dann auf **Öffnen**. Standardmäßig befindet sich [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Verzeichnis **C:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Sqlservr.exe**. Klicken Sie auf **Weiter**.  
  
5.  Wählen Sie im Dialogfeld **Aktion** die Option **Verbindung zulassen** aus, und klicken Sie anschließend auf **Weiter**.  
  
6.  Wählen Sie im Dialogfeld **Profil** beliebige Profile aus, die die Verbindungsumgebung des Computers beschreiben, wenn Sie eine Verbindung zum [!INCLUDE[ssDE](../../includes/ssde-md.md)]herstellen möchten, und klicken Sie dann auf **Weiter**.  
  
7.  Geben Sie im Dialogfeld **Name** einen Namen und eine Beschreibung für diese Regel ein, und klicken Sie dann auf **Fertig stellen**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vorgehensweise: Konfigurieren von Firewalleinstellungen (Azure SQL-Datenbank)](/azure/azure-sql/database/firewall-configure)  
  
