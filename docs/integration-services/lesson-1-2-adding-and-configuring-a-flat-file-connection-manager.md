---
description: 'Lektion 1.2: Hinzufügen und Konfigurieren eines Verbindungs-Managers für Flatfiles'
title: 'Schritt 2: Hinzufügen und Konfigurieren eines Verbindungs-Managers für Flatfiles | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9a77dd32-d8c2-4961-ad37-2a971f9d6043
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 57bf9bd3863e5486c890088324641a5100581a02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099224"
---
# <a name="lesson-1-2-add-and-configure-a-flat-file-connection-manager"></a>Lektion 1.2: Hinzufügen und Konfigurieren eines Verbindungs-Managers für Flatfiles

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In dieser Aufgabe fügen Sie einen Verbindungs-Manager für Flatfiles zum von Ihnen erstellten Paket hinzu. Mithilfe eines Verbindungs-Managers für Flatfiles können von einem Paket Daten aus einer Flatfile extrahiert werden. Mithilfe des Verbindungs-Managers für Flatfiles können Sie den Namen und Speicherort der Datei, die Gebietsschema- und Codepage sowie das Dateiformat einschließlich der Spaltentrennzeichen angeben, die angewendet werden sollen, wenn vom Paket Daten aus der Flatfile extrahiert werden. Zusätzlich können Sie die Datentypen für einzelne Spalten manuell angeben oder das Dialogfeld **Spaltentypen vorschlagen** verwenden, um die Spalten extrahierter Daten automatisch [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Datentypen zuzuordnen.  
  
Sie müssen einen neuen Verbindungs-Manager für Flatfiles für jedes Dateiformat erstellen, mit dem Sie arbeiten. Da in diesem Tutorial Daten aus mehreren Flatfiles extrahiert werden, die alle dasselbe Datenformat aufweisen, müssen Sie für das Beispielpaket nur einen Verbindungs-Manager für Flatfiles hinzufügen und konfigurieren.  
  
In dieser Lektion konfigurieren Sie die folgenden Eigenschaften in Ihrem Verbindungs-Manager für Flatfiles:  
  
-   **Spaltennamen:** Da die Flatfile keine Spaltennamen aufweist, werden vom Verbindungs-Manager für Flatfiles Standardspaltennamen erstellt. Diese Standardnamen sind nicht sinnvoll, wenn der Zweck jeder Spalte identifiziert werden soll. Ändern Sie die Standardnamen so, dass sie mit der Faktentabelle übereinstimmen, in die die Flatfiledaten geladen werden sollen.  
  
-   **Datenzuordnungen:** Die Datentypenzuordnungen, die Sie für den Verbindungs-Manager für Flatfiles angeben, werden von allen Flatfile-Datenquellenkomponenten verwendet, die auf diesen Verbindungs-Manager verweisen. Sie können diese Datentypen entweder mithilfe des Verbindungs-Managers für Flatfiles manuell zuordnen oder das Dialogfeld **Spaltentypen vorschlagen** verwenden. In dieser Aufgabe werden die vorgeschlagenen Zuordnungen im Dialogfeld **Spaltentypen vorschlagen** angezeigt. Sie erstellen dann manuell die erforderlichen Zuordnungen im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles**.  
  
> [!NOTE]
> Der Verbindungs-Manager für Flatfiles stellt Gebietsschemainformationen zur Datendatei bereit. Wenn Ihr Computer nicht für die Verwendung der Regionsoption **Englisch (USA)** konfiguriert ist, müssen Sie zusätzliche Eigenschaften im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** festlegen.  
  
## <a name="add-a-flat-file-connection-manager-to-the-ssis-package"></a>Hinzufügen eines Flatfile-Verbindungs-Managers zum SSIS-Paket  
  
1.  Klicken Sie im Bereich **Projektmappen-Explorer** mit der rechten Maustaste auf **Verbindungs-Manager**, und wählen Sie **Neuer Verbindungs-Manager** aus.
1. Wählen Sie im Dialogfeld **SSIS-Verbindungs-Manager hinzufügen** die Option **FLATFILE** und dann **Hinzufügen** aus.
  
2.  Geben Sie im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** für **Name des Verbindungs-Managers** den Namen **Sample Flat File Source Data** (Beispiel-Flatfilequelldaten) ein.  
  
3.  Wählen Sie **Durchsuchen** aus.  
  
4.  Suchen Sie im Dialogfeld **Öffnen** die Datei **SampleCurrencyData.txt** auf Ihrem Computer.  
  
5.  Deaktivieren Sie das Kontrollkästchen **Spaltennamen in der ersten Datenzeilen**.  
  
### <a name="set-locale-sensitive-properties"></a>Festlegen gebietsschemabezogener Eigenschaften  
  
1.  Wählen Sie im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** die Option **Allgemein** aus.  
  
2.  Legen Sie **Gebietsschema** auf **Englisch (USA)** und **Codepage** auf **1252** fest.  
  
### <a name="rename-columns-in-the-flat-file-connection-manager"></a>Umbenennen von Spalten im Verbindungs-Manager für Flatfiles  
  
1.  Wählen Sie im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** die Option **Erweitert** aus.  
  
2.  Nehmen Sie im Eigenschaftenbereich die folgenden Änderungen vor:  
  
    -   Ändern Sie die **Column 0** -Nameneigenschaft in **AverageRate**.  
  
    -   Ändern Sie die **Column 1** -Nameneigenschaft in **CurrencyID**.  
  
    -   Ändern Sie die **Column 2** -Nameneigenschaft in **CurrencyDate**.  
  
    -   Ändern Sie die **Column 3** -Nameneigenschaft in **EndOfDayRate**.  
  
### <a name="remap-column-data-types"></a>Neuzuordnung von Spaltendatentypen  
  
Standardmäßig sind alle vier Spalten auf einen Zeichenfolgendatentyp [DT_STR] mit einer **OutputColumnWidth** von 50 festgelegt.  

1.  Wählen Sie im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** die Option **Typen vorschlagen** aus.  
  
    [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] schlägt automatisch geeignete Datentypen auf Basis der ersten 200 Datenzeilen vor. Sie können diese Vorschlagsoptionen auch ändern, um mehr oder weniger Daten auszuwerten, den Standarddatentyp für ganzzahlige oder boolesche Daten anzugeben oder Leerstellen zum Auffüllen von Zeichenfolgenspalten hinzuzufügen.  
  
    Nehmen Sie vorerst keine Änderungen an den Optionen im Dialogfeld **Spaltentypen vorschlagen** vor, und wählen Sie **OK** aus, damit von [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] Datentypen für Spalten vorgeschlagen werden. Anschließend kehren Sie durch diese Aktion zum Bereich **Erweitert** im Dialogfeld **Verbindungs-Manager-Editor für Flatfiles** zurück, in dem Sie die von [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]vorgeschlagenen Spaltendatentypen anzeigen können. Wenn Sie alternativ **Abbrechen** auswählen, werden keine Vorschläge zu Spaltenmetadaten gemacht, und der Standardtyp für Zeichenfolgendaten (DT_STR) wird verwendet.  
  
    In diesem Lernprogramm werden von [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] die Datentypen vorgeschlagen, die in der zweiten Spalte der folgenden Tabelle für die Daten aus der Datei SampleCurrencyData.txt angezeigt werden. Die vierte Spalte enthält die Datentypen, die für die Spalten im Ziel erforderlich sind, die in einem späteren Schritt definiert werden.  
  
    |Flatfilespalte|Vorgeschlagener Typ|Zielspalte|Zieltyp|  
    |--------------------|------------------|----------------------|--------------------|  
    |AverageRate|float [DT_R4]|FactCurrencyRate.AverageRate|float|  
    |CurrencyID|string [DT_STR]|DimCurrency.CurrencyAlternateKey|nchar(3)|  
    |CurrencyDate|date [DT_DATE]|DimDate.FullDateAlternateKey|date|  
    |EndOfDayRate|float [DT_R4]|FactCurrencyRate.EndOfDayRate|float|  
  
    Der für die Spalte **CurrencyID** vorgeschlagene Datentyp ist inkompatibel mit dem Datentyp des Felds in der Zieltabelle. Da `DimCurrency.CurrencyAlternateKey` vom Datentyp nchar (3) ist, muss **CurrencyID** von string [DT_STR] in Unicode string [DT_WSTR] geändert werden. Zusätzlich ist das Feld `DimDate.FullDateAlternateKey` als date-Datentyp definiert. Deshalb muss der Typ für **CurrencyDate** von date [DT_Date] in database date [DT_DBDATE] geändert werden.  
  
2.  Wählen Sie in der Liste die Spalte **CurrencyID** aus. Ändern Sie im Eigenschaftenbereich den Datentyp der Spalte **CurrencyID** von string [DT_STR] in Unicode string [DT_WSTR].  
  
3.  Ändern Sie im Eigenschaftenbereich den Datentyp der Spalte **CurrencyDate** von date [DT_DATE] in database date [DT_DBDATE].  
  
4.  Klicken Sie auf **OK**.  
  
## <a name="go-to-next-task"></a>Weiter zur nächsten Aufgabe
[Schritt 3: Hinzufügen und Konfigurieren eines OLE DB-Verbindungs-Managers](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Verbindungs-Manager für Flatfiles](../integration-services/connection-manager/flat-file-connection-manager.md)  
[SQL Server Integration Services-Datentypen](../integration-services/data-flow/integration-services-data-types.md)  
  
  
  
