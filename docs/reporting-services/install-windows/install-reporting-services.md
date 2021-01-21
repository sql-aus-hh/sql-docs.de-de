---
description: Installieren von SQL Server Reporting Services
title: Installieren von SQL Server Reporting Services | Microsoft-Dokumentation
ms.date: 12/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: a6c2dc6ae1a8711c3e78403b539a603c0cb36dcf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596989"
---
# <a name="install-sql-server-reporting-services"></a>Installieren von SQL Server Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

Die Installation von SQL Server Reporting Services umfasst Serverkomponenten zum Speichern von Berichtselementen, Rendern von Berichten und Verarbeiten von Abonnements sowie anderen Berichtsdiensten. 

::: moniker range=">=sql-server-ver15"
Laden Sie [SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122) im Microsoft Download Center herunter.

::: moniker-end

::: moniker range="=sql-server-2017"
Laden Sie [SQL Server 2017 Reporting Services](https://www.microsoft.com/download/details.aspx?id=55252) im Microsoft Download Center herunter.

::: moniker-end

> [!NOTE]
> Interessieren Sie sich für Power BI-Berichtsserver? Weitere Informationen finden Sie unter [Install Power BI Report Server (Installieren von Power BI-Berichtsserver)](https://powerbi.microsoft.com/documentation/reportserver-install-report-server/).
> 
> Führen Sie ein Upgrade oder eine Migration von SQL Server 2016 oder einer früheren Version von Reporting Services durch? Weitere Informationen dazu finden Sie unter [Upgrade und Migration von Reporting Services](upgrade-and-migrate-reporting-services.md).

## <a name="before-you-begin"></a>Voraussetzungen

Prüfen Sie vor der Installation von Reporting Services [Hardware and software requirements for installing SQL Server (Hardware- und Softwareanforderungen für die Installation von SQL Server)](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="install-your-report-server"></a>Installieren Ihres Berichtsservers

Die Installation des Berichtsservers ist unkompliziert. Zum Installieren der Dateien müssen nur wenige Schritte ausgeführt werden.

> [!NOTE]
> Während der Installation muss kein Server für eine SQL Server-Datenbank-Engine verfügbar sein. Allerdings benötigen Sie einen verfügbaren Server zur Konfiguration von Reporting Services nach der Installation.

1. Suchen Sie nach dem Speicherort von „SQLServerReportingServices.exe“, und starten Sie das Installationsprogramm.

2. Klicken Sie auf **Reporting Services installieren**.

3. Wählen Sie eine Edition aus, die Sie installieren möchten, und klicken Sie auf **Weiter**.

    In der Dropdownliste können Sie zwischen den kostenlosen Editionen „Evaluation“ und „Developer“ wählen.

    ![Die Editionen „Evaluation“ und „Developer“](media/install-reporting-services/report-server-install-edition-select.png)

    Geben Sie ansonsten einen Product Key ein. [So finden Sie den Product Key für SQL Server Reporting Services](find-reporting-services-product-key-ssrs.md)

4. Lesen und akzeptieren Sie die Lizenzbedingungen sowie sonstige weitere Bedingungen, und klicken Sie anschließend auf **Weiter**.

5. Zum Speichern der Berichtsserver-Datenbank muss eine Datenbank-Engine verfügbar sein. Klicken Sie auf **Weiter**, um nur den Berichtsserver zu installieren.

6. Geben Sie den Installationsort für den Berichtsserver an. Klicken Sie auf **Installieren**, um den Vorgang fortzusetzen.

    > [!NOTE]
    > Der Standardpfad lautet C:\Program Files\Microsoft SQL Server Reporting Services.

7. Wählen Sie nach dem erfolgreichen Setup **Berichtsserver konfigurieren** aus, um den Berichtsserver-Konfigurations-Manager zu starten.

## <a name="configure-your-report-server"></a>Konfigurieren Ihres Berichtsservers

Wenn Sie beim Setup auf **Berichtsserver konfigurieren** klicken, wird der **Konfigurations-Manager für Reporting Services** angezeigt. Weitere Informationen finden Sie unter [Report Server Configuration Manager (Konfigurations-Manager des Berichtsservers)](reporting-services-configuration-manager-native-mode.md).

Sie müssen eine [Berichtsserver-Datenbank erstellen](ssrs-report-server-create-a-report-server-database.md), um die erste Konfiguration von Reporting Services abzuschließen. Sie benötigen einen SQL Server-Datenbankserver, um diesen Schritt abzuschließen.

### <a name="creating-a-database-on-a-different-server"></a>Erstellen einer Datenbank auf einem anderen Server

Wenn Sie die Berichtsserver-Datenbank auf einem anderen Datenbankserver auf einem anderen Computer erstellen, müssen Sie die Anmeldeinformationen für das Dienstkonto für den Berichtsserver ändern, sodass diese vom Datenbankserver erkannt werden.

Standardmäßig verwendet der Berichtsserver das virtuelle Dienstkonto. Wenn Sie versuchen, eine Datenbank auf einem anderen Server zu erstellen, wird möglicherweise der folgende Fehler im Schritt „Anwenden von Verbindungsrechten“ ausgelöst:

`System.Data.SqlClient.SqlException (0x80131904): Windows NT user or group '(null)' not found. Check the name again.`

Sie können das Dienstkonto entweder in „Netzwerkdienst“ oder „Domänenkonto“ ändern, um den Fehler zu umgehen. Wenn das Dienstkonto in „Netzwerkdienst“ geändert wird, werden im Kontext des Computerkontos die Rechte für den Berichtsserver übernommen.

Weitere Informationen finden Sie unter [Configure the report server service account (Konfigurieren des Dienstkontos für den Berichtsserver)](configure-the-report-server-service-account-ssrs-configuration-manager.md).

## <a name="windows-service"></a>&Windows-Dienst

Als Teil der Installation wird ein Windows-Dienst erstellt. Dieser Dienst wird als **SQL Server Reporting Services** angezeigt. Der Name des Dienstes lautet **SQLServerReportingServices**.

## <a name="default-url-reservations"></a>Standard-URL-Reservierungen

URL-Reservierungen bestehen aus Präfix, Hostname, Port und virtuellem Verzeichnis:

|Teil|BESCHREIBUNG|
|----------|-----------------|
|Präfix|Das Standardpräfix ist http. Wenn Sie zuvor ein TLS-Zertifikat (Transport Layer Security, früher als Secure Sockets Layer, SSL, bezeichnet) installiert haben, versucht das Setup, die URL-Reservierungen zu erstellen, die das Präfix HTTPS verwenden.|
|Hostname|Der Standardhostname ist ein Platzhalter (+). Dieses gibt an, dass der Berichtsserver eine beliebige HTTP-Anforderung an den angegebenen Port für einen beliebigen Hostnamen akzeptiert, der für den Computer steht, einschließlich `https://<computername>/reportserver`, `https://localhost/reportserver` oder `https://<IPAddress>/reportserver.`.|
|Port|Der Standardport ist 80. Wenn Sie einen anderen Port als Port 80 verwenden, müssen Sie diesen explizit der URL hinzufügen, wenn Sie ein Webportal in einem Browserfenster öffnen.|
|Virtuelles Verzeichnis|Standardmäßig werden virtuelle Verzeichnisse im Format „ReportServer“ für den Berichtsserver-Webdienst und im Format „Reports“ für das Webportal erstellt. Beim Berichtsserver-Webdienst lautet der Standardname für das virtuelle Verzeichnis **reportserver**. Für das Webportal ist **reports** das virtuelle Standardverzeichnis.|

Ein Beispiel für die vollständige URL-Zeichenfolge könnte folgendermaßen aussehen:

- `https://+:80/reportserver` bietet Zugriff auf den Berichtsserver.

- `https://+:80/reports` bietet Zugriff auf das Webportal.

## <a name="firewall"></a>Firewall

Wenn Sie von einem Remotecomputer auf den Berichtsserver zugreifen, sollten Sie sicherstellen, dass Firewallregeln konfiguriert sind, wenn es eine Firewall gibt.

Sie müssen den TCP-Port öffnen, den Sie für die URLs Ihres Webdienstes und Ihres Webportals konfiguriert haben. Standardmäßig werden diese auf dem TCP-Port 80 konfiguriert.

## <a name="additional-configuration"></a>Zusätzliche Konfiguration

- Informationen zum Konfigurieren der Integration in den Power BI-Dienst finden Sie unter [Integrate with the Power BI service (Integration in den Power BI-Dienst)](power-bi-report-server-integration-configuration-manager.md). Dort erfahren Sie, wie Sie Berichtselemente an das Power BI-Dashboard heften können.

- Informationen zum Konfigurieren von E-Mails für Abonnementverarbeitungen finden Sie unter [E-Mail settings (E-Mail-Einstellungen)](e-mail-settings-reporting-services-native-mode-configuration-manager.md) und [E-Mail delivery in a report server (Übermitteln von E-Mails an einen Berichtsserver)](../subscriptions/e-mail-delivery-in-reporting-services.md).

- Informationen zum Konfigurieren des Webportals für das Abrufen und Verwalten von Berichten von einem Remotecomputer finden Sie unter [Configure a firewall for report server access (Konfigurieren einer Firewall zum Zugriff auf den Berichtsserver)](../report-server/configure-a-firewall-for-report-server-access.md) und [Configure a report server for remote administration (Konfigurieren eines Berichtsservers zur Remoteverwaltung)](../report-server/configure-a-report-server-for-remote-administration.md).

## <a name="related-information"></a>Verwandte Informationen

Informationen zur Installation von SQL Server Reporting Services im einheitlichen Modus finden Sie unter [Install Reporting Services native mode report server (Installieren des Reporting Services-Berichtsservers im einheitlichen Modus)](install-reporting-services-native-mode-report-server.md). 

::: moniker range="=sql-server-2016"

Informationen zur Installation von SQL Server 2016 Reporting Services (und Vorgängerversionen) im SharePoint-Integrationsmodus finden Sie unter [Install the first Report Server in SharePoint mode (Installieren des ersten Berichtsservers im SharePoint-Modus)](install-the-first-report-server-in-sharepoint-mode.md).

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den Berichtsserver installiert haben, beginnen Sie mit dem Erstellen von Berichten, und stellen Sie diese anschließend für ihren Berichtsserver bereit. Informationen zum Starten des Berichts-Generators finden Sie unter [Install Report Builder (Installieren des Berichts-Generators)](../../reporting-services/install-windows/install-report-builder.md).

[Laden Sie SQL Server Data Tools herunter](../../ssdt/download-sql-server-data-tools-ssdt.md), um Berichte mithilfe von SQL Server Data Tools zu erstellen.

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)