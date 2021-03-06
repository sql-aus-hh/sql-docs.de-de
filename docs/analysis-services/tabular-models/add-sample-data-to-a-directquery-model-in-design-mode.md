---
title: Hinzufügen von Beispieldaten zu einem DirectQuery-Modell im Entwurfsmodus | Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1ca4c4c2a00eed80e709602084cf5de427134977
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34041634"
---
# <a name="add-sample-data-to-a-directquery-model-in-design-mode"></a>Hinzufügen von Beispieldaten zu einem DirectQuery-Modell im Entwurfsmodus
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
 Im DirectQuery-Modus werden Tabellenpartitionen entweder zum Erstellen von Beispieldatenteilmengen verwenden, die während des Modellentwurfs genutzt werden, oder zum Erstellen von Alternativen einer vollständigen Datenansicht.
 
 Bei der Bereitstellung eines DirectQuery-Tabellenmodells ist nur eine Partition pro Tabelle erlaubt, und diese Partition muss bei Bedarf eine vollständige Datenansicht sein. Bei jeder zusätzlichen Partition handelt es sich entweder um einen Ersatz für eine vollständige Datenansicht oder um Beispieldaten. In diesem Thema wird das Erstellen einer Beispielpartition mit einer Teilmenge von Daten beschrieben.
 
 Standardmäßig enthält die Arbeitsdatenbank des Modells keine Daten, wenn ein Tabellenmodell im DirectQuery-Modus in SSDT erstellt wird. Es gibt eine Standardpartition für jede Tabelle, und diese Partition übermittelt alle Abfragen an die Datenquelle. 
  
Sie können jedoch eine kleinere Menge an Beispieldaten der Arbeitsdatenbank Ihres Modells hinzufügen, die zur Entwurfszeit verwendet wird. Beispieldaten werden über eine Abfrage auf eine Beispielpartition angegeben, die nur während des Entwurfs verwendet wird. Sie werden mit dem Modell im Arbeitsspeicher zwischengespeichert. Dies hilft Ihnen bei der Überprüfung von Modellierungsentscheidungen, ohne dabei die Datenquelle zu beeinträchtigen. Sie können Ihre Modellierungsentscheidungen mit dem Beispieldataset überprüfen, wenn Sie es mit **In Excel analysieren** in SQL Server Data Tools (SSDT) verwenden, oder es aus anderen Clientanwendungen stammt, die eine Verbindung mit Ihrer Arbeitsbereichsdatenbank herstellen können.  
  
> [!TIP]  
>  Selbst im DirectQuery-Modus bei einem leeren Modell können Sie sich immer für jede Tabelle ein kleines eingebautes Rowset anzeigen lassen. Klicken Sie in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]auf **Tabelle** > **Tabelleneigenschaften** , um das Dataset mit 50 Zeilen anzuzeigen.  
  
## <a name="create-a-sample-partition"></a>Erstellen einer Beispielpartition
 Diese Anweisungen gelten für tabellarische Modelle erstellt oder ein Upgrade auf Kompatibilitätsgrad 1200 oder höher. Modelle mit niedrigeren Kompatibilitätsgraden verwenden verschiedene Eigenschaften zum Abrufen zwischengespeicherter Daten. Zusätzliche Beschreibungen finden Sie unter [Aktivieren des DirectQuery-Modus in SSMS](../../analysis-services/tabular-models/enable-directquery-mode-in-ssms.md) .  
  
1.  Klicken Sie in SQL Server Data Tools unter „Diagramm“ oder „Datenansicht“ auf eine Faktentabelle, um deren Eigenschaftenseite zu öffnen. Faktentabellen geben die aggregierten numerischen Daten und die Measures in Ihrem Modell an. Sie verfügen möglicherweise über mehr als eine Faktentabelle.  
  
2.  Klicken Sie auf **Tabelle** > **Eigenschaften** , um das Dialogfeld „Partitionsverwaltung“ zu öffnen.  
  
    Beachten Sie, dass die Standardpartition **(DirectQuery) \<Tabellenname >**. Dies ist die vollständige Datenansicht. Löschen Sie diese Partition nicht! Diese Partition wird verwendet, wenn das Modell bereitgestellt wird.  
  
4.  Wählen Sie die Partition aus, und klicken Sie anschließend auf **Kopieren**.  

    Dadurch wird eine Kopie der Standardpartition erstellt. Diese Kopie enthält jedoch Beispieldaten, die Sie in einer Abfrage angeben. Beispiel:
  
     ![Ssas_tabularproject_copypartition](../../analysis-services/tabular-models/media/ssas-tabularproject-copypartition.jpg "Ssas_tabularproject_copypartition")  
  
5.  Wählen Sie die kopierte Partition aus, und klicken Sie anschließend auf die Schaltfläche **SQL-Abfrage-Editor** , um einen Filter hinzuzufügen. Reduzieren Sie die Größe Ihrer Beispieldaten während der Erstellung des Modells. Wenn Sie z.B. **FactInternetSales** aus AdventureWorksDW auswählen, kann Ihr Filter wie Folgt aussehen:  
  
    ```  
    SELECT [dbo].[FactInternetSales].* FROM [dbo].[FactInternetSales]  
    JOIN DimSalesTerritory as ST  
    ON ST.SalesTerritoryKey = FactInternetSales.SalesTerritoryKey  
    WHERE ST.SalesTerritoryGroup='North America';  
    ```  
  
6.  Klicken Sie auf **Überprüfen** , um die Syntaxfehler zu überprüfen.  
  
     Beachten Sie, dass im DirectQuery-Modus zusätzliche zu den Schaltflächen **Neu** , **Kopieren**und **Löschen** im Dialogfeld der Partition auch eine Umschaltfläche existiert, die alternativ **Als Beispiel festlegen** oder **Als DirectQuery festlegen**liest.  
  
     Nur eine Partition kann die DirectQuery-Partition sein. Sie können dies steuern, indem Sie eine Partition auswählen, die für die Tabelle definiert ist und anschließend auf **Als Beispiel festlegen**klicken.  
  
7.  Verarbeiten der Tabelle.  
  


  
  
