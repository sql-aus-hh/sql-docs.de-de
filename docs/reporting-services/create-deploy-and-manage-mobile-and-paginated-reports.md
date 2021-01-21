---
title: Grundlegendes zu den SQL Server Reporting Services | Microsoft-Dokumentation
description: Erfahren Sie mehr über Tools und Dienste für mobile und paginierte Reporting Services-Berichte.
ms.date: 05/06/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services]
- SSRS
- reports [Reporting Services], about reports
- Reporting Services
- SQL Server Reporting Services
ms.assetid: b8d18d3d-9db0-43e7-8286-7b46cc3a37ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 08f9271c7b42e7db3d42ffadc111e9b86dd38e68
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596191"
---
# <a name="what-is-sql-server-reporting-services-ssrs"></a>Was ist SQL Server Reporting Services (SSRS)?

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

Interessieren Sie sich für Power BI-Berichtsserver? Weitere Informationen finden Sie unter [Was ist der Power BI-Berichtsserver?](/power-bi/report-server/get-started).

SQL Server Reporting Services (SSRS) bietet eine Reihe lokaler Tools und Dienste zum Erstellen, Bereitstellen und Verwalten mobiler und paginierter Berichte.

![Alle Varianten von SQL Server Reporting Services](../reporting-services/media/ss-reporting-services-all-together.png "Alle Varianten von SQL Server Reporting Services")

## <a name="create-deploy-and-manage-mobile-and-paginated-reports"></a>Erstellen, Verwalten und Bereitstellen von mobilen und paginierten Berichten

Mit SSRS gelangt die richtige Information zum richtigen Benutzer. Benutzer können die Berichte über einen Webbrowser oder per E-Mail auf einem mobilen Gerät lesen.

SQL Server Reporting Services bietet eine aktualisierte Sammlung von Produkten:

* **„Herkömmlich“ paginierte Berichte** sind auf den neuesten Stand gebracht, sodass Sie modern aussehende Berichte erstellen können, wozu Sie aktualisierte Tools und neue Funktionen nutzen.
* **Neue mobile Berichte** mit einem ansprechenden Layout, das sich entsprechend den verschiedenen Geräten und den verschiedenen Arten anpasst, wie Sie diese Geräte halten.
* **Ein modernes Webportal** , das Sie in jedem modernen Browser anzeigen können. Im neuen Portal können Sie mobile und paginierte Reporting Services-Berichte und KPIs organisieren und anzeigen. Sie können auch Excel-Arbeitsmappen im Portal speichern.

Lesen Sie weiter, um weitere Informationen zu jedem Aspekt zu erhalten.

### <a name="whats-new-in-reporting-services"></a>Neuigkeiten bei Reporting Services

Mit diesen Quellen sind Sie ständig über neue Funktionen in SQL Server Reporting Services auf dem Laufenden.

* [Neuigkeiten bei Reporting Services](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)
* [SQL Server Reporting Services Team Blog (Blog des SQL Server Reporting Services-Teams)](/archive/blogs/sqlrsteamblog/)
* Der [YouTube-Kanal „Guy in a Cube“](https://www.youtube.com/channel/UCFp1vaKzpfvoGai0vE5VJ0w)

## <a name="paginated-reports"></a>Paginierte Berichte

![Bild eines paginierten Berichts auf einem Desktopbildschirm und auf einem Tablet](../reporting-services/media/ssrs-paginated-reports.png)

Reporting Services ist „traditionellen“ paginierten Berichten zugeordnet, die optimal zum Drucken von Dokumenten mit festem Layout, wie z. B. PDF- und Word-Dateien, geeignet sind.

Diese BI-Kernarbeitslast gibt es nach wie vor, weshalb wir sie modernisiert haben. Sie können nun mithilfe des Berichts-Generators oder des Berichts-Designers in [SQL Server Data Tools (SSDT)](../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md) modern gestaltete Berichte mit aktualisierten neuen Funktionen erstellen.

* Wir haben die Standardstile und -farbpaletten aktualisiert, d. h., Sie erstellen Berichte standardmäßig mit einem neuen minimalistischen Stil.
* Wir haben den Bereich „Parameter“ aktualisiert, sodass Sie Parameter nach Wunsch beliebig anordnen können.
* Sie können in neue Formate, etwa PowerPoint, exportieren. Visualisierungen mit Reporting Services in PowerPoint stellen keine einfachen Screenshots dar, sie sind vielmehr dynamisch und können bearbeitet werden.
* Sie können eine hybride Power BI/Reporting Services-Umgebung erstellen: Statt Ihre lokalen Reporting Services-Berichte in Power BI neu zu erstellen, können Sie visuelle Objekte aus diesen Berichten an Ihre Power BI-Dashboards anheften. Anschließend können Sie alle Elemente an einem Ort in Ihrem Power BI-Dashboard überwachen.

## <a name="mobile-reports"></a>Mobile Berichte

![Bild eines mobilen Berichts auf einem Desktopbildschirm und auf einem Tablet](../reporting-services/media/ssrs-mobile-reports.png)

Mobile Geräte haben zu einem anderen Arbeitsverhalten geführt, was bedeutet, dass Menschen von heute andere Anforderungen an Berichte haben. Die Berichtsdarstellung mit festem Layout funktioniert nicht sehr gut für Tablets und Smartphones. Eine Darstellung, die für einen breiten PC-Bildschirm konzipiert ist, ergibt nicht die optimale Darstellung auf einem kleinen Smartphonebildschirm, der nicht nur kleiner ist, sondern auch eine Ausrichtung im Hoch- oder Querformat hat.

Für diese sehr unterschiedlichen Bildschirmformate benötigen Sie ein dynamisches Layout, das sich an die jeweilige Bildschirmgröße und -ausrichtung anpasst. Dafür haben wir einen neuen Berichtstyp hinzugefügt: mobile Berichte, die auf der Datazen-Technologie basieren, die wir vor über einem Jahr erworben und in das Produkt integriert haben. Sie können Ihre vorhandene Datazen Berichte über den [SQL Server Migration Assistant for Datazen](https://www.microsoft.com/download/details.aspx?id=53128)zu Reporting Services migrieren.

Sie erstellen diese mobilen Berichte in der neuen App [Publisher für mobile Berichte](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) . Dann können Sie in den nativen [Power BI-Apps für mobile Geräte](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) für Windows 10, iOS, Android und HTML5 auf Ihre Daten in Power BI, in der Cloud oder in SSRS zugreifen.

Während Sie eine Visualisierung erstellen, generiert der Publisher für mobile Berichte automatisch Beispieldaten. Mit dieser Funktion können Sie gleich sehen, wie die Visualisierung Ihrer Daten aussieht und welche Art von Daten zur jeweiligen Visualisierung passt.

## <a name="web-portal"></a>Webportal

![Bild eines Laptops mit angezeigtem Webportal](../reporting-services/media/ssrs-web-portal.png)

Benutzer des einheitlichen Modus von Reporting Services erleben ein modernes Webportal, das in den meisten modernen Browsern anzeigt wird. Sie können über das neue Portal auf all Ihre mobilen, paginierten Reporting Services-Berichte und KPIs zugreifen. KPIs können wichtige Geschäftsmetriken auf einen Blick im Browser zum Vorschein bringen, ohne dass ein Bericht geöffnet werden muss.

Das neue Webportal ist eine vollständig neu geschriebene Version des Berichts-Managers. Es ist nun eine einseitige, standardbasierte HTML5-App, für die moderne Browser optimiert sind: Microsoft Edge, Internet Explorer 10 und 11, Chrome, Firefox, Safari und alle weiteren wichtigen Browser.

Die Inhalte des Webportals sind nach Typ strukturiert:

* Paginierte Berichte
* Mobile Berichte 
* KPIs (Key Performance Indicators)
* Excel-Arbeitsmappen
* Freigegebene Datasets
* Freigegebene Datenquellen

Sie können die Berichte dort in der herkömmlichen Ordnerhierarchie sicher speichern und verwalten. Kennzeichnen Sie Ihre Favoritenberichte für den Schnellzugriff. Mit den entsprechenden Berechtigungen können Sie SSRS-Inhalte verwalten.

Und Sie können im neuen Webportal weiterhin Berichtsverarbeitung planen, bei Bedarf auf Berichte zugreifen sowie veröffentlichte Berichte abonnieren.

Weitere Informationen zum Webportal finden Sie [hier](../reporting-services/web-portal-ssrs-native-mode.md).

::: moniker range="=sql-server-2016"

## <a name="reporting-services-in-sharepoint-integrated-mode"></a>Reporting Services im integrierten SharePoint-Modus

Sie veröffentlichen Berichte in Reporting Services im integrierten SharePoint-Modus. Sie können die Verarbeitung von Berichten planen, nach Bedarf auf Begriffe zugreifen, veröffentlichte Berichte abonnieren und Berichten in andere Programme wie Microsoft Excel exportieren. Erstellen Sie Datenwarnungen für Berichte, die auf einer SharePoint-Website veröffentlicht werden, und lassen Sie sich bei Berichtsdatenänderungen per E-Mail benachrichtigen.  

Weitere Informationen über [Reporting Services-Berichtsserver im integrierten SharePoint-Modus](../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md).

::: moniker-end

## <a name="ssrsnoversion-programming-features"></a>[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] -Programmierfeatures

Mit den [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]-Programmierfunktionen können Sie die Berichtsfunktionen erweitern und anpassen. Mithilfe der SSRS-APIs können Sie die Daten- und Berichtsverarbeitung in benutzerdefinierte Anwendungen integrieren oder sie darin erweitern.

Weitere Informationen finden Sie in der [Reporting Services-Entwicklerdokumentation](../reporting-services/reporting-services-developer-documentation.md).

## <a name="next-steps"></a>Nächste Schritte

* [Installieren von Reporting Services](../reporting-services/install-windows/install-reporting-services.md)
* [Download der neuesten SQL Server-Datatools](../ssdt/download-sql-server-data-tools-ssdt.md)
* [Install Report Builder (Installieren des Berichts-Generators)](../reporting-services/install-windows/install-report-builder.md)

* Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)