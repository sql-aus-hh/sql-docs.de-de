---
description: Konfigurieren von TLS-Verbindungen auf einem Berichtsserver im einheitlichen Modus
title: Konfigurieren von TLS-Verbindungen auf einem Berichtsserver im einheitlichen Modus | Microsoft-Dokumentation
ms.date: 09/27/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Secure Sockets Layer (SSL)
ms.assetid: 212f2042-456a-4c0a-8d76-480b18f02431
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6d76229a17951c4b0bad35ee78e95a903bb590e3
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594966"
---
# <a name="configure-tls-connections-on-a-native-mode-report-server"></a>Konfigurieren von TLS-Verbindungen auf einem Berichtsserver im einheitlichen Modus

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Im einheitlichen Modus wird der HTTP-SSL-Dienst (Secure Sockets Layer, SSL) verwendet, um verschlüsselte Verbindungen mit einem Berichtsserver herzustellen. Transport Layer Security (TLS) wurde früher als Secure Sockets Layer (SSL) bezeichnet. Wenn in einem lokalen Zertifikatspeicher auf dem Berichtsservercomputer eine Zertifikatsdatei (CER-Datei) installiert ist, können Sie das Zertifikat an eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -URL-Reservierung binden, um Berichtsserververbindungen über einen verschlüsselten Kanal zu unterstützen.  
  
> [!TIP]  
>  Bei Verwendung des SharePoint-Modus von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] finden Sie weitere Informationen in der SharePoint-Dokumentation. Zum Beispiel: [Aktivieren von TLS für eine SharePoint 2010-Webanwendung](/archive/blogs/sowmyancs/how-to-enable-ssl-on-a-sharepoint-2010-web-application).  
  
 Da Internetinformationsdienste (IIS) auch HTTP-SSL verwenden, bestehen signifikante Probleme mit der Interoperabilität, die berücksichtigt werden müssen, wenn IIS und [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] auf demselben Computer ausgeführt werden. Überprüfen Sie im Abschnitt "Interoperabilitätsprobleme mit IIS", wie Sie diese Probleme beheben können.  
  
## <a name="server-certificate-requirements"></a>Anforderungen für Serverzertifikate  
 Auf Ihrem Computer muss ein Serverzertifikat installiert sein (Clientzertifikate werden nicht unterstützt). Reporting Services bietet keine Funktionalität für das Anfordern, Generieren, Herunterladen oder Installieren von Zertifikaten. Windows Server 2012 und höhere Versionen bieten ein Zertifikate-Snap-In, mit dem Sie ein Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle anfordern können.  
  
 Für Testzwecke können Sie ein Zertifikat lokal generieren. Wenn Sie das Hilfsprogramm **MakeCert** und den Beispielbefehl als Vorlage verwenden, geben Sie Ihren Servernamen als Host an, und entfernen Sie alle Zeilenumbrüche, bevor Sie den Befehl ausführen. Wenn Sie den Befehl in einem DOS-Fenster ausführen, müssen Sie die Puffergröße des Fensters erhöhen, damit der gesamte Befehl aufgenommen werden kann.  
  
 Wenn Sie IIS und [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] zusammen auf demselben Computer verwenden, können Sie mit der Konsolenanwendung „IIS-Manager“ das auf Ihrem Computer installierte Zertifikat abrufen. IIS-Manager enthält Optionen, mit denen Sie eine Zertifikatanforderungsdatei (CRT-Datei) erstellen und in ein Paket integrieren können, sodass sie nachfolgend von einer vertrauenswürdigen Zertifizierungsstelle verarbeitet werden kann. Die von Ihnen verwendete Zertifizierungsstelle generiert eine Zertifikatsdatei (CER-Datei), die sie an Sie zurücksendet. Über die IIS-Verwaltungskonsole können Sie die Zertifikatsdatei im lokalen Speicher installieren. Weitere Informationen finden Sie unter [Using SSL to Encrypt Confidential Data](/previous-versions/windows/it-pro/windows-server-2003/cc738495(v=ws.10)) auf der TechNet-Website.  
  
## <a name="interoperability-issues-with-iis"></a>Interoperabilitätsprobleme mit IIS  
 Wenn IIS auf demselben Computer vorhanden ist wie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], wirkt sich dies entscheidend auf die TLS-Verbindungen mit einem Berichtsserver aus:  
  
-   Wenn IIS installiert ist, muss der World Wide Web-(W3SVC)-Dienst immer ausgeführt werden. Der HTTP-SSL-Dienst stellt eine Abhängigkeit zu IIS her, wenn er erkennt, dass IIS ausgeführt wird. Dies bedeutet, dass der W3SVC-Dienst ausgeführt werden muss, wenn IIS und [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] auf demselben Computer installiert sind und Sie Berichtsserver-URLs für TLS-Verbindungen konfigurieren.  
  
-   Durch das Deinstallieren von IIS kann der Dienst zu einer TLS-gebundenen Berichtsserver-URL vorübergehend unterbrochen werden. Aus diesem Grund wird empfohlen, den Computer neu zu starten, nachdem Sie IIS deinstalliert haben.  
  
     Der Computer muss neu gestartet werden, um alle TLS-Sitzungen aus dem Cache zu löschen. Einige Betriebssysteme speichern TLS-Sitzungen bis zu 10 Stunden im Cache. Aus diesem Grund funktioniert eine https://-URL auch, nachdem die TLSL-Bindung in HTTP.SYS aus der URL-Reservierung entfernt wurde. Durch den Neustart des Computers werden alle geöffneten Verbindungen geschlossen, die den Kanal verwenden.  
  
## <a name="bind-tls-to-a-reporting-services-url-reservation"></a>Binden von TLS an eine URL-Reservierung für Reporting Services  
 In den folgenden Schritten wird nicht erläutert, wie Sie Zertifikate anfordern, generieren, herunterladen oder installieren. Sie müssen bereits ein Zertifakt installiert haben, das verwendet werden kann. Es bleibt Ihnen überlassen, welche Zertifikatseigenschaften Sie angeben, welche Zertifizierungsstelle Sie verwenden und mit welchen Tools und Hilfsprogrammen Sie das Zertifikat anfordern und installieren.  
  
 Sie können das Zertifikat mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool binden. Wenn das Zertifikat im lokalen Computerspeicher korrekt installiert ist, wird es vom [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool erkannt und in der Liste **SSL-Zertifikate** auf den Seiten **Webdienst-URL** und **Webportal-URL** angezeigt.  
  
### <a name="to-configure-a-report-server-url-for-tls"></a>So konfigurieren Sie eine Berichtsserver-URL für TLS  
  
1.  Starten Sie das Reporting Services-Konfigurationstool, und stellen Sie eine Verbindung mit dem Berichtsserver her.  
  
2.  Klicken Sie auf **Webdienst-URL**.  
  
3.  Erweitern Sie die Liste der TLS/SSL-Zertifikate. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] erkennt Serverauthentifizierungszertifikate im lokalen Speicher. Wenn Sie ein Zertifikat installiert haben, das in der Liste nicht angezeigt wird, müssen Sie den Dienst eventuell neu starten. Verwenden Sie die Schaltflächen **Stopp** und **Start** auf der Seite **Berichtsserverstatus** im Konfigurationstool für die Reporting Services, um den Dienst neu zu starten (oberste Seite).  
  
4.  Wählen Sie das Zertifikat aus.  
  
5.  Klicken Sie auf **Anwenden**.  
  
6.  Klicken Sie auf die URL, um zu überprüfen, ob sie funktioniert.  
  
 Damit Sie die URL testen können, muss die Berichtsserverdatenbank konfiguriert sein. Erstellen Sie gegebenenfalls die Berichtsserverdatenbank, bevor Sie die URL testen.  
  
 URL-Reservierungen für die Webportal-URL und die Berichtsserver-Webdienst-URL werden unabhängig voneinander konfiguriert. Wenn Sie auch den Webportalzugriff über einen TLS-verschlüsselten Kanal konfigurieren möchten, fahren Sie mit den folgenden Schritten fort:  
  
1.  Greifen Sie auf die **Webportal-URL** zu.
  
2.  Wählen Sie **Erweitert** aus.  
  
3.  Klicken Sie unter **Mehrere HTTPS-Identitäten für das aktuell ausgewählte Reporting Services-Feature** auf **Hinzufügen**.  
  
4.  Wählen Sie das Zertifikat aus, und klicken Sie nacheinander auf **OK** und **Anwenden**.  
  
5.  Testen Sie die URL, um zu überprüfen, ob sie funktioniert.  
  
## <a name="how-certificate-bindings-are-stored"></a>Speichern von Zertifikatbindungen  
 Zertifikatsbindungen werden in HTTP.SYS gespeichert. Eine Darstellung der definierten Bindungen wird auch im Abschnitt **URLReservations** der Datei RSReportServer.config gespeichert. Die Einstellungen in der Konfigurationsdatei sind nur eine Darstellung der tatsächlichen, an anderer Stelle gespeicherten Werte. Ändern Sie die Werte in der Konfigurationsdatei nicht direkt. Die Konfigurationseinstellungen werden in der Datei erst angezeigt, wenn Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool oder den Berichtsserver-WMI-Anbieter (Windows Management Instrumentation) verwenden, um ein Zertifikat zu binden.  
  
> [!NOTE]  
>  Wenn Sie eine Bindung mit einem TLS/SSL-Zertifikat in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] konfigurieren und das Zertifikat später vom Computer entfernen möchten, müssen Sie sicherstellen, dass Sie die Bindung aus [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] entfernen, bevor Sie das Zertifikat vom Computer entfernen. Anderenfalls können Sie die Bindung mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool oder WMI nicht mehr entfernen, und die Fehlermeldung "Ungültiger Parameter" wird angezeigt. Wenn Sie das Zertifikat bereits vom Computer entfernt haben, können Sie das Tool Httpcfg.exe verwenden, um die Bindung aus HTTP.SYS zu entfernen. Weitere Informationen zu Httpcfg.exe finden Sie in der Windows-Produktdokumentation.  
  
 TLS-Bindungen stellen in Microsoft Windows eine freigegebene Ressource dar. Die vom Konfigurations-Manager für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] oder anderen Tools wie IIS-Manager vorgenommenen Änderungen können sich auf andere Anwendungen auf demselben Computer auswirken. Es empfiehlt sich, zum Bearbeiten von Bindungen dasselbe Tool zu verwenden, mit dem Sie die Bindungen erstellt haben.  Wenn Sie beispielsweise TLS-Bindungen mit dem Konfigurations-Manager erstellt haben, empfiehlt es sich, den Lebenszyklus der Bindungen mit dem Konfigurations-Manager zu verwalten. Wenn Sie Bindungen mit dem IIS-Manager erstellt haben, empfiehlt es sich, den Lebenszyklus der Bindungen mit dem IIS-Manager zu verwalten. Wenn IIS vor der Installation von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] auf dem Computer installiert wurde, empfiehlt es sich, die TLS-Konfiguration in IIS vor der Konfiguration von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] zu überprüfen.  
  
 Wenn Sie TLS-Bindungen für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] mithilfe des Berichtsserver-Konfigurations-Managers entfernen, funktioniert TLS möglicherweise nicht mehr für Websites auf einem Server, auf dem IIS (Internetinformationsdienste) ausgeführt wird, oder auf einem anderen HTTP.SYS-Server. Der Konfigurations-Manager für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] entfernt den folgenden Registrierungsschlüssel: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443**. Wenn dieser Registrierungsschlüssel entfernt wird, wird die TLS-Bindung für IIS ebenfalls entfernt. Ohne diese Bindung wird TLS nicht für das HTTPS-Protokoll bereitgestellt. Um dieses Problem zu diagnostizieren, verwenden Sie den IIS-Manager oder das Befehlszeilen-Hilfsprogramm „HTTPCFG.exe“. Um das Problem zu lösen, stellen Sie die TLS-Bindung für Ihre Websites mithilfe des IIS-Managers wieder her. Um dieses Problem in Zukunft zu vermeiden, verwenden Sie den IIS-Manager, um die TLS-Bindungen zu entfernen und dann die Bindung für die gewünschten Websites wiederherzustellen. Weitere Informationen finden Sie im Knowledge Base-Artikel [SSL funktioniert nach dem Entfernen einer SSL-Bindung nicht mehr (https://support.microsoft.com/kb/956209/n)](https://support.microsoft.com/kb/956209/n)).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Authentifizierung mit dem Berichtsserver](../../reporting-services/security/authentication-with-the-report-server.md)   
 [Konfigurieren und Verwalten eines Berichtsservers &#40;einheitlicher SSRS-Modus&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [RsReportServer.config-Konfigurationsdatei](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Konfigurieren von Berichtsserver-URLs &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
  
