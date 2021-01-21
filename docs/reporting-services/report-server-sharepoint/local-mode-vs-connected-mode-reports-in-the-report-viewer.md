---
title: Berichte im lokalen Modus im Vergleich zu Berichten im verbundenen Modus in der Berichtanzeige | Microsoft-Dokumentation
description: SQL Server Reporting Services-Berichte können entweder für die Ausführung im lokalen Modus oder im verbundenen Modus konfiguriert werden. Hier erfahren Sie mehr über die verschiedenen Modi.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: beeba27a4186aa8ea19401284b8e1f3c4c7f6990
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597199"
---
# <a name="local-mode-vs-connected-mode-reports-in-the-report-viewer"></a>Berichte im lokalen Modus im Vergleich zu Berichten im verbundenen Modus in der Berichtanzeige

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichte können so konfiguriert werden, dass sie entweder im *lokalen Modus* oder im *verbundenen Modus* ausgeführt werden, der einen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver nutzt. Sie können stattdessen mithilfe des Berichts-Viewers Berichte aus SharePoint direkt rendern, sofern die Datenerweiterung die Berichterstellung im lokalen Modus unterstützt. Dieser Ansatz wird als *lokaler Modus* bezeichnet. In früheren Versionen von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]musste die SharePoint-Farm mit einem im SharePoint-Modus konfigurierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver verbunden sein, damit Berichte vom Steuerelement des Berichts-Viewers gerendert werden konnten. Dieser Ansatz wird als *Remotemodus* oder *verbundener Modus* bezeichnet.  

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.

 Im *lokalen Modus* gibt es keinen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver. Sie müssen das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-In für SharePoint-Produkte installieren, während jedoch kein [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver erforderlich ist. Im lokalen Modus können Benutzer Berichte anzeigen, haben aber keinen Zugriff auf serverseitige Funktionen wie Abonnements und Datenwarnungen.  

## <a name="local-mode-vs-connected-mode-and-supported-extensions"></a>Lokaler Modus vs. verbundener Modus und unterstützte Erweiterungen

 **Lokaler Modus:** Bei Datenerweiterungen, die den lokalen Modus unterstützen, können Berichte des Berichts-Viewers direkt über SharePoint gerendert werden. Im *lokalen Modus* gibt es keinen [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver. Sie müssen das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Add-In für SharePoint-Produkte installieren, während jedoch kein [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver erforderlich ist. Im lokalen Modus können Benutzer Berichte anzeigen, haben aber **keinen** Zugriff auf serverseitige Funktionen wie Abonnements und Datenwarnungen.  
  
 Für den **verbundenen Modus**, auch *Remotemodus* genannt, ist ein mit der SharePoint-Farm verbundener [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver im SharePoint-Modus erforderlich, damit Berichte vom Steuerelement des Berichts-Viewers gerendert werden können.  
  
 Die folgende Liste enthält die Datenverarbeitungserweiterungen, die lokale Modusberichterstellung unterstützen:  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access 2010-Berichtserweiterung. Weitere Informationen zu Access Services finden Sie unter [Verwenden von Access Services mit SQL Reporting Services: Installieren des SQL Server 2008 R2 Reporting Services-Add-Ins (SharePoint Server 2010)](/previous-versions/office/sharepoint-server-2010/ee662542(v=office.14)).  
  
-   Die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -SharePoint-Listendatenerweiterung. Weitere Informationen zur SharePoint-Listendatenerweiterung finden Sie unter [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
  
 Benutzerdefinierte Datenverarbeitungserweiterungen können auch für den lokalen Modus entwickelt werden. Weitere Informationen finden Sie unter [Implementing a Data Processing Extension](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
 Der lokale Modus unterstützt das Rendern von Berichten mit einer eingebetteten oder freigegebenen Datenquelle von einer RSDS-Datei. Sie können den Bericht oder dessen zugeordnete Datenquelle jedoch nicht verwalten. In diesem Fall wird ein Fehler mit dem Hinweis ausgegeben, dass der Vorgang im lokalen Modus nicht unterstützt wird. Das Verwalten von Datenquellen auf der SharePoint-Website wird nur im verbundenen Modus unterstützt.  
  
> [!NOTE]  
>  Wie in früheren Versionen ist es nicht möglich, Benutzernamen und Kennwörter in die RSDS-Datei einzubetten.  
  
## <a name="configure-local-mode-and-access-services-with-sharepoint-2013"></a>Konfigurieren des lokalen Modus und von Access Services mit SharePoint 2013

 Sie können die SharePoint 2013-Farm so konfigurieren, dass vorhandene Access 2010-Webdatenbanken und der lokale [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Modus unterstützt werden. Weitere Informationen finden Sie unter [Einrichten und Konfigurieren von Access Services 2010 für Webdatenbanken in SharePoint Server 2013](https://technet.microsoft.com/library/ee748653\(office.15\).aspx).  
  
 Es ist nicht möglich, neue Access-Webdatenbanken für SharePoint 2013 zu erstellen. Access 2013 verwendet den neuen Datenbanktyp *Access Web App* , den Sie in Access erstellen und dann als SharePoint-App in einem Webbrowser verwenden und mit anderen gemeinsam nutzen.  
  
 Weitere Informationen finden Sie unter folgenden Themen.  
  
-   [Neues in Access 2013](https://office.microsoft.com/access-help/what-s-new-in-access-2013-HA102809500.aspx) (https://office.microsoft.com/access-help/what-s-new-in-access-2013-HA102809500.aspx).  
  
-   [Grundlegende Aufgaben für eine Access-App](https://office.microsoft.com/access-help/basic-tasks-for-an-access-app-HA102840210.aspx?CTT=5&origin=HA102809500) (https://office.microsoft.com/access-help/basic-tasks-for-an-access-app-HA102840210.aspx?CTT=5&origin=HA102809500).  
  
## <a name="configure-local-mode-reporting-with-sharepoint-2010"></a>Konfigurieren der Berichterstellung im lokalen Modus mit SharePoint 2010

 Der lokale Modus erfordert den ASP.NET-Sitzungsstatus. Durch die Installation von Access-Diensten wird der ASP.NET-Sitzungstatus aktiviert. Sie können die Aktivierung auch mit PowerShell durchführen.  
  
1.  Öffnen Sie die SharePoint 2010-Verwaltungsshell.  
  
2.  Geben Sie folgenden Befehl ein:  
  
    ```  
    - Enable-SPSessionStateService  
    ```  
  
3.  Geben bei Aufforderung den Namen der Datenbank ein.  
  
4.  Führen Sie eine IIS-Rücksetzung aus.  
  
 Weitere Informationen finden Sie unter [Verwenden von Access Services mit SQL Reporting Services: Installieren des SQL Server 2008 R2 Reporting Services-Add-Ins (SharePoint Server 2010)](/previous-versions/office/sharepoint-server-2010/ee662542(v=office.14)) und [Enable-SPSessionStateService](https://technet.microsoft.com/library/ff607857\(v=office.15\).aspx).  
  
## <a name="connected-mode"></a>Modus „Verbunden“

 Die neuesten Informationen zur Verwendung der ADS-Erweiterung mit verbundenem Modus in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] finden Sie unter [Access Services-Bericht in SharePoint-Website zeigt einen Fehler in der ADS-Datenerweiterung](https://social.technet.microsoft.com/wiki/contents/articles/25298.access-services-report-in-sharepoint-site-shows-error-in-data-extension-ads.aspx).  
  
## <a name="see-also"></a>Weitere Informationen

 [Von Reporting Services unterstützte Datenquellen](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)