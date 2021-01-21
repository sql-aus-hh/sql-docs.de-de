---
title: 'Mobile Berichte mit SQL Server: End-to-End-Vorgehensweise'
description: Hier erfahren Sie, wie Sie mobile Berichte im Publisher für mobile Berichte von SQL Server erstellen, Berichte im Reporting Services-Webportal speichern und Berichte in mobilen Power BI-Apps anzeigen.
ms.date: 12/07/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: e198575e-b154-4342-b944-2bf19ec49bfd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df09e4168790a4607de19348f50e76a4fbd0f383
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596543"
---
# <a name="sql-server-mobile-reports-end-to-end-walk-through"></a>Mobile Berichte mit SQL Server: End-to-End-Vorgehensweise
Exemplarische Vorgehensweise zum Erstellen mobiler Berichte für sämtliche Bildschirmgrößen mit [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-long.md)] im [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] -Webportal und ihrer Anzeige in mobilen Power BI-Apps.

Das Tool bietet eine Entwurfsoberfläche mit anpassbaren Rasterzeilen und -spalten und flexiblen Elementen für mobile Berichte. Verbinden Sie sich mit einer Vielzahl lokaler Datenquellen, oder laden Sie Excel-Arbeitsmappen hoch, um mobile Berichte zu erstellen. Speichern Sie Ihre Berichte in einem [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal, und zeigen Sie sie in einem Browser oder mobilen Power BI-Apps an.  
  
In diesem Artikel wird Folgendes beschrieben:   
  
- Erstellen einer freigegebenen Datenquelle und eines Datasets im [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal mithilfe der AdventureWorks-Datenbank als Beispieldatenquelle  
- Erstellen eines mobilen Reporting Services-Berichts in [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]  
- Veröffentlichen des mobilen Berichts im [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal  
- Anzeigen des mobilen Berichts in der mobilen Power BI-App  
  
## <a name="before-we-start"></a>Voraussetzungen  
Um folgen zu können, benötigen Sie diese Produkte:  
  
* Zum Erstellen von Datenquellen und KPIs sowie zum Veröffentlichen von Datasets und mobilen Berichten benötigen Sie Zugriff auf einen [Reporting Services-Berichtsserver im einheitlichen Modus](../install-windows/install-reporting-services-native-mode-report-server.md).  
* Zum Erstellen freigegebener Datasets müssen Sie den [Berichts-Generator installieren](../install-windows/install-report-builder.md).  
* Zum Erstellen mobiler Berichte [installieren Sie den Publisher für mobile Berichte von SQL Server](../reporting-services-features-and-tasks-ssrs.md).  
* [AdventureWorks-Beispieldatenbanken](https://github.com/Microsoft/sql-server-samples/releases).  
*  ODER: Wide World Importers-Beispieldatenbank (WWI), die auf der Seite [Microsoft SQL Server-Beispiele](../../samples/sql-samples-where-are.md) verfügbar ist.
* Führen Sie zum Anzeigen von Ergebnissen die folgenden Schritte aus: 
  *   [Registrieren beim Power BI-Dienst](https://go.microsoft.com/fwlink/?LinkID=513879) und
  *  [Herunterladen der mobilen Power BI-App](/power-bi/consumer/mobile/mobile-apps-for-mobile-devices) auf Ihr Mobilgerät: iOS, Android-Smartphone oder Windows 10-Gerät.  

  
## <a name="create-a-shared-data-source"></a>Erstellen einer freigegebenen Datenquelle  
  
Sie können eine freigegebene Datenquelle für Ihre mobilen Berichte anhand der Datenquellen erstellen, die Reporting Services unterstützt. [Eine Liste unterstützter Datenquellen finden Sie hier](../report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
1. Klicken Sie in Ihrem [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal auf **Neu** > **Datenquelle**.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
3. Geben Sie Ihre Datenquelleninformationen ein, und klicken Sie auf **OK**.  
  
    Datenquellen werden nicht standardmäßig im Portal angezeigt.    
   
5. Klicken Sie zum Anzeigen von Datenquellen auf **Anzeigen** > **Datenquelle**.  
  
   ![PBI_SSMRP_DisplayDataSources](../../reporting-services/mobile-reports/media/pbi-ssmrp-displaydatasources.png)  
   
6. Nun wird die Datenquelle im [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Portal angezeigt.  
  
   ![PBI_SSMRP_PortlDataSource](../../reporting-services/mobile-reports/media/pbi-ssmrp-portldatasource.png)  
  
Erfahren Sie mehr über [freigegebene Datenquellen in Reporting Services](../report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
   
## <a name=""></a><a name="shared-dataset">Erstellen eines freigegebenen Datasets</a>  
  
Verwenden Sie ein vorhandenes [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Clienttool, z. B. Berichts-Designer in [!INCLUDE[ssBIDevStudioFull_md](../../includes/ssbidevstudiofull-md.md)], um das freigegebene Dataset zu erstellen.  In dieser exemplarischen Vorgehensweise wird [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]verwendet. [Installieren Sie Berichts-Generator](../install-windows/install-report-builder.md), oder starten Sie das Tool in Ihrem Webportal. Sie erstellen drei Datasets, und zwar jeweils eine für den KPI-Wert, den KPI-Trend und eine mit weiteren Feldern für den mobilen Reporting Services-Bericht.     
  
1. Klicken Sie in Ihrem [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal auf **Neu** > **Paginierter Bericht** , um [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]zu starten.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)   
2. Klicken Sie auf **Neues Dataset**.  
  
   ![PBI_SSMRP_RBNewDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-rbnewdataset.png)  
   
3. Klicken Sie auf **Andere Datenquellen durchsuchen**.  
   
4. Geben Sie in das Feld „Name“ den Namen des Servers, auf dem Sie die Datenquelle gespeichert hat, in diesem Format ein:   
   
   Name: https://*localhost*/ReportServer  
   Elemente vom Typ: Datenquellen (*.rsds)  
   
5. Klicken Sie auf **Öffnen**, und navigieren Sie zur Datenquelle, die Sie auf diesem Server erstellt haben.  
   
6. Wählen Sie die Datenquelle aus, und klicken Sie erneut auf **Öffnen** .    
  
7. Entwerfen Sie das Dataset im [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_RB_QueryDesignr600](../../reporting-services/mobile-reports/media/pbi-ssmrp-rb-querydesignr600.png)  
   
8. Wenn Sie fertig sind, speichern Sie das Dataset auf dem [!INCLUDE[PRODUCT_NAME](../../includes/ssrs.md)] -Berichtsserver.    
   
Nun können Sie das Dataset als Grundlage für Ihre KPIs und mobilen Berichte nutzen.  Sie können mehrere Datasets für die gleiche Datenquelle erstellen. Sie können auch mehrere KPIs und mobile Berichte für diese freigegebene Datasets erstellen.   
  
## <a name=""></a><a name="create-KPI">Erstellen eines KPI</a>  
Sie erstellen KPIs direkt im [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal.    
  
1. Klicken Sie im Webportal rechts oben auf **Neu** > **Neuer KPI**.   
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
      
   Auf dem Bildschirm zum Erstellen von KPIs können Sie Werte manuell eingeben oder ein freigegebenes Dataset verwenden.    
2. Ändern Sie **Wert** von **Manuell festlegen** in **Datasetfeld**.  
   
   ![PBI_SSMRP_KPI_DatasetField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpi-datasetfield.png)  
   
3. Klicken Sie im Feld **Datasetfeld auswählen** auf die Auslassungspunkte ( **...** ), und wählen Sie ein Dataset aus dem vorherigen Schritt aus.  
   
   ![PBI_SSMRP_KPIPickDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickdataset.png)  
   
4. Wählen Sie das Feld im Dataset.    
   
   ![PBI_SSMRP_KPIPickField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickfield.png)  
     
5. Wählen Sie die gewünschte Aggregation. KPIs werden nur als eine Zahl angezeigt, sodass das Feld aggregiert wird, um diese Zahl anzuzeigen.

   ![Screenshot des Abschnitts „Feld aus "AWSalesYTD" auswählen“, in dem der „Aggregation“-Abschnitt „Durchschnitt“ angezeigt wird](../../reporting-services/mobile-reports/media/reporting-services-kpi-pick-aggregation.png)

6. Klicken Sie auf **OK**.

7. Klicken Sie im Feld **Trendsatz** auf **Datasettrend**.  
  
6. Klicken Sie im Feld **Datasettrend auswählen** auf die Auslassungspunkte ( **...** ).  
   
7. Wählen Sie ein Feld aus, und klicken Sie auf **OK**.  

   ![PBI_SSMRP_KPIPickTrend](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipicktrend.png)  
  
8. Benennen Sie den KPI, wählen Sie eine Visualisierung, und klicken Sie auf **Erstellen**.   
  
   Der KPI wird im [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal angezeigt.  
   
    ![PBI_SSMRP_NewKPI](../../reporting-services/mobile-reports/media/pbi-ssmrp-newkpi.png)  
    
## <a name=""></a><a name="create-mobile-report">Erstellen eines mobilen Berichts in Reporting Services</a>  
   
Zum Erstellen eines mobilen Reporting Services-Berichts [installieren Sie Publisher für mobile Berichte von SQL Server](../reporting-services-features-and-tasks-ssrs.md)oder starten das Tool im [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] -Webportal. 

Beim ersten Öffnen von [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]sehen Sie einen leeren Zeichenbereich, in dem Sie Ihren mobilen Bericht erstellen können. Sie können nach Wunsch zuerst mit den visuellen Elementen oder mit Ihren Daten beginnen. Wenn Sie die visuellen Elemente zuerst erstellen, generiert [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] automatisch simulierte Daten, die an den Bericht gebunden sind, und ändert sich dynamisch, sobald Sie Ihre Optionen für die visuellen Elemente ändern. Versuchen Sie es selbst.   
  
## <a name="start-with-the-visuals"></a>Beginnen mit den visuellen Elementen  
  
1. Klicken Sie in Ihrem [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal auf **Neu** > **Mobiler Bericht** , um [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]zu starten.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)

   [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] öffnet das Raster des Masterlayouts.  
  
2. Wechseln Sie auf der Registerkarte **Layout** zum Abschnitt „Diagramme“.  
  
   ![PBI_SSMRP_LayoutTabCharts2](../../reporting-services/mobile-reports/media/pbi-ssmrp-layouttabcharts2.png)  
  
2. Ziehen Sie die **Strukturzuordnung** auf das Raster, und ziehen Sie die rechte untere Ecke auf eine Breite von drei Spalten und eine Höhe von drei Zeilen.  
  
   ![PBI_SSMRP_TreeMap](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemap.png)  
  
3. Sie können ihre visuellen Eigenschaften im unteren Bereich anzeigen. Sie müssen möglicherweise seitwärts scrollen, um alle anzuzeigen.   
  
   ![PBI_SSMRP_TreeMapVisProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapvisprops.png)  
  
4. Nachdem Sie das visuelle Element „Strukturzuordnung“ ausgewählt haben, wählen Sie links oben die Registerkarte **Daten** aus.   
  
   Nun sehen Sie die simulierten Felder und Werte, die [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] generiert hat, und erkennen, was die Größe und Farbe in der Strukturzuordnung darstellen.  
  
   ![PBI_SSMRP_TreeMapDataProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapdataprops.png)  
  
6. Klicken Sie auf die Registerkarte **Layout**.  
  
7. Klicken Sie auf das Rädchen „Optionen“ ![PBI_SSMRP_Cog](../../reporting-services/mobile-reports/media/pbi-ssmrp-cog.png) rechts oben in der Strukturzuordnung, um das enthaltene Menü einzublenden.   
  
   ![PBI_SSMRP_OptionsWheel](../../reporting-services/mobile-reports/media/pbi-ssmrp-optionswheel.png)  
  
8. Klicken Sie auf den Pfeil in der Mitte des Rads, um es zu schließen.  
  
## <a name="add-your-own-data"></a>Hinzufügen Ihrer eigenen Daten  
  
1. Wechseln Sie zur Registerkarte **Daten** .    
   
2. Um Ihre eigenen Daten hinzuzufügen, klicken Sie rechts oben auf **Daten hinzufügen** , und navigieren Sie zu Ihrer Daten.    
  
3. Sie können Daten aus einer lokalen Excel-Arbeitsmappe nutzen, doch in diesem Fall stammen sie aus dem freigegebenen Dataset in Ihrem [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal. Die Meldung „Server hinzugefügt“ wird angezeigt.  
  
4. Wählen Sie den Server und dann das Dataset aus, das Sie erstellt haben.  
   
3. Zurück auf der Registerkarte **Daten** ändern Sie im Bereich **Dateneigenschaften** die Eigenschaften **Größe repräsentiert**, **Farbe repräsentiert** u. a. in Felder in Ihren eigenen Daten. 
   
   *  **Größe repräsentiert**, **Farbe repräsentiert** und **Benutzerdefinierter Mittenwert** müssen Felder mit numerischen Werten sein. 
   *  **Gruppieren nach** ist eine Kategorie, also ein Textfeld.
   
   ![Screenshot des Abschnitts „Dateneigenschaften“](../../reporting-services/mobile-reports/media/ssrs-mobile-report-data-properties.png)
   
6. Wählen Sie **Vorschau** aus, um die mit Ihren Daten aktualisierte Strukturzuordnung anzuzeigen.  

## <a name="add-a-gauge-to-your-mobile-report"></a>Hinzufügen eines Messgeräts zu Ihrem mobilen Bericht

Lassen Sie uns ein Messgerät hinzufügen, um anhand desselben Datasets den bisherigen Jahresumsatz mit dem letztjährigen Umsatz zu vergleichen.

1. Ziehen Sie auf der Registerkarte „Layout“ ein halbes Rad auf den Zeichenbereich. Platzieren Sie es unter der Strukturzuordnung, und ziehen Sie die rechte untere Ecke auf eine Breite von drei Quadraten und eine Höhe von zwei Quadraten. 

2. Es wird erneut mit simulierten Daten begonnen. 

   Beachten Sie, dass in **Visuelle Eigenschaften** die Angaben **Höhere Werte sind besser** und **Deltabezeichnung** standardmäßig ein **Prozent von Ziel** darstellen. Standardmäßig ist **Bereichsabgrenzungen** angegeben, was Sie ändern können. Doch für den Moment war es das.

   ![Screenshot des „Bereiche festlegen“-Abschnitts der visuellen Eigenschaften des mobilen Berichts mit Radsymbol](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-visual-properties.png)
   
3. Wählen Sie auf der Registerkarte **Daten** die Tabelle mit Ihren Daten, dann das Feld **Hauptwert** und das Feld aus, das Sie in **Vergleichswert** vergleichen möchten.

4. Sie können unterschiedliche Aggregationen wählen, um eine Zahl für **Hauptwert** und eine für **Vergleichswert** zurückzugeben. Standardmäßig ist der Wert eine Summe.

   ![Screenshot der Optionen für den „Vergleichswert“](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-sum.png)

5. Wählen Sie **Vorschau** aus, um das Ergebnis zu prüfen. 

   ![Screenshot der Vorschau des mobilen Berichts mit Radsymbol](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-preview.png)

## <a name="add-a-selection-list-as-a-filter"></a>Hinzufügen einer Auswahlliste als Filter

Auswahllisten fungieren wie Slicer in Power BI und Excel. Wir können eine hinzufügen, um die anderen visuellen Elemente im mobilen Bericht zu filtern.

1. Ziehen Sie auf der Registerkarte **Layout** eine Auswahlliste rechts neben die Strukturzuordnung und dann die rechte untere Ecke, um sie mit einer Breite von zwei Quadraten und der Höhe des Zeichenbereichs (fünf Quadrate) zu versehen. 

   ![Screenshot der Auswahlliste für den mobilen Bericht](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list.png)

2. Legen Sie auf der Registerkarte **Daten** unter **Dateneigenschaften** die Felder **Schlüssel** und **Bezeichnungen** auf ein Feld in den Daten fest, anhand dessen Sie filtern möchten.

   ![Screenshot des „Dateneigenschaften“-Abschnitts der Auswahlliste für den mobilen Bericht](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list-data-properties.png)
   
## <a name="create-a-mobile-report-for-phones"></a>Erstellen eines mobilen Berichts für Smartphones  
  
Nachdem Sie visuelle Elemente im Masterlayout erstellt haben, können Sie einen mobilen Bericht mit einem Layout erstellen, das spezifisch für Benutzer von Smartphones optimiert ist.    
  
1. Klicken Sie rechts oben auf das Canvas-Symbol und dann auf **Telefon**.  
  
2. Auf der Registerkarte „Layout“ sehen Sie unter **Steuerelementinstanzen** die beiden Diagramme, die Sie erstellt haben.   
  
3. Ziehen Sie die Strukturzuordnung auf den Zeichenbereich „Telefon“ und auf eine Breite von vier Spalten und eine Höhe von drei Zeilen.  
  

## <a name="save-your-mobile-report"></a>Speichern des mobilen Berichts  
Sie können den Bericht lokal oder in einem [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] -Webportal speichern. Bei einer lokalen Speicherung speichert [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] den Bericht mit zwischengespeicherten Daten, damit Sie ihn öffnen und weiter bearbeiten können. Sie können ihn aber nicht auf einem mobilen Gerät anzeigen.   
  
1. Klicken Sie links oben auf der Symbol „Speichern“.   
   
2. Um den Bericht mit anderen zu teilen und auf einem mobilen Gerät anzuzeigen, klicken Sie auf **Auf Server speichern**.  
  
3. Navigieren Sie auf dem Server zum Ordner, in dem Sie Ihren mobilen Bericht speichern möchten.  
  
4. Klicken Sie auf **Ordner wählen** > **Speichern**.  
  
   Sie erhalten eine Meldung zur Bestätigung, dass der Bericht gespeichert wurde.  
    
   Nach dem Speichern können Sie zum Portal zurückkehren und Ihren mobilen Bericht in einer Miniaturansicht anzeigen.   
    
5. Tippen Sie auf die Miniaturansicht, um den Bericht im Webportal anzuzeigen.  
  
## <a name="view-your-report-on-the-web-portal"></a>Anzeigen des Berichts im Webportal

  
## <a name="view-your-report-on-a-mobile-device"></a>Anzeigen des Berichts auf einem mobilen Gerät   
  
Führen Sie zum Anzeigen Ihres [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] -Berichts zunächst diese Schritte aus:

*  [Registrieren beim Power BI-Dienst](https://go.microsoft.com/fwlink/?LinkID=513879), wenn Sie noch kein Konto haben.
*  [Herunterladen der mobile Power BI-App](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) auf Ihr mobiles Gerät.  

### <a name="view-your-mobile-report"></a>Anzeigen des mobilen Berichts
  
1.  Öffnen Sie die Power BI-App auf Ihrem mobilen Gerät, und melden Sie sich an.  
    
2.  Um Ihre mobilen Reporting Services-Berichte und KPIs anzuzeigen, tippen Sie auf **Reporting Services**.  
![PBI_iPad_GetStartedSm](../../reporting-services/mobile-reports/media/pbi-ipad-getstartedsm.png)  
  
3. Tippen Sie links oben auf das Symbol „Optionen“ ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) und dann auf **Mit Server verbinden**.  
  
   ![PBI_iPad_SSMRP_ConnectCrop](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcrop.png)  
  
4. Benennen Sie den Server, und geben Sie die Adresse des Servers, Ihre E-Mail-Adresse und das Kennwort im folgenden Format ein:  
  
   ![PBI_iPad_SSMRP_ConnectContoso](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcontoso.png)   
  
5.  Nun wird den Server auf der linken Navigationsleiste angezeigt.  
  
    ![PBI_iPad_SSMRP_LeftNavBiggr](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-leftnavbiggr.png)  
      
> [!TIP]
> Sie können jederzeit auf das Symbol „Optionen“ ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) tippen, um zwischen Ihren mobilen Reporting Services-Berichten im Reporting Services-Webportal und Ihren Dashboards im Power BI-Dienst zu wechseln.  

  
## <a name="view-kpis-and-mobile-reports-in-the-power-bi-app"></a>Anzeigen von KPIs und mobilen Berichten in der Power BI-App  
  
Tippen Sie auf die Registerkarte **KPIs** oder **Mobile Berichte** .   
  
![PBI_iPad_SSMRP_Portal](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-portal.png)  
  
- Tippen Sie auf einen KPI, um ihn im Fokusmodus anzuzeigen.  
  
    ![PBI_iPad_SSMRP_Tile](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-tile.png)  
  
- Tippen Sie auf einen mobilen Bericht, um ihn in der Power BI-App zu öffnen und mit ihm zu interagieren.  
  
Die KPIs und mobilen Berichte werden in denselben Ordnern angezeigt, in denen sie sich im Reporting Services-Webportal befinden.   
  
## <a name="see-also"></a>Weitere Informationen  
 
-  Informationen zu iOS- und Android-Geräten finden Sie unter [Anzeigen lokaler Berichte und KPIs eines Berichtsservers in den mobilen Power BI-Apps](/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports).
-  Informationen zu iOS- und Android-Geräten finden Sie unter [Anzeigen von mobilen SSRS-Berichten (Reporting Services) und -KPIs in der mobilen Power BI-App für Windows 10](https://powerbi.microsoft.com/documentation/powerbi-mobile-win10-kpis-mobile-reports/).    
  
