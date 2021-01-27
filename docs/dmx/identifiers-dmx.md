---
description: Bezeichner (DMX)
title: Bezeichner (DMX) | Microsoft-Dokumentation
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: d802cb37a972eb498b485d7253a82b8445f4566d
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813475"
---
# <a name="identifiers-dmx"></a>Bezeichner (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Alle Objekte in [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] müssen über einen Bezeichner verfügen. Der Name eines Objekts ist dessen Bezeichner. Server, Datenbanken und Datenbankenobjekte (Datenquellen, Datenquellensichten, Cubes, Dimensionen, Miningmodelle usw.) haben Bezeichner.  
  
 In Data Mining-Erweiterungen (DMX) gibt es zwei Klassen von Bezeichnern:  
  
-   [Reguläre Bezeichner](#RegularIdentifiers)  
  
-   [Begrenzungsbezeichner](#DelimitedIdentifiers)  
  
 Ein Objektbezeichner wird erstellt, wenn Sie das Objekt definieren. Sie verwenden den Bezeichner dann dazu, auf das Objekt zu verweisen. Ein Bezeichner kann bis zu 100 Zeichen lang sein.  
  
##  <a name="regular-identifiers"></a><a name="RegularIdentifiers"></a> Reguläre Bezeichner  
 Reguläre Bezeichner in DMX entsprechen den [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]-Regeln für das Format von Bezeichnern. Für einen regulären Bezeichner sind in DMX keine Begrenzungszeichen erforderlich. Im folgenden finden Sie ein Beispiel für eine DMX-Anweisung, die einen regulären, nicht Begrenzungs Bezeichner verwendet:  
  
```  
SELECT * FROM Clustering.CONTENT;  
```  
  
### <a name="rules-for-regular-identifiers"></a>Regeln für reguläre Bezeichner  
 Regeln für das Format eines regulären Bezeichners:  
  
1.  Das erste Zeichen eines regulären Bezeichners muss eines der folgenden Zeichen sein:  
  
    -   Ein Buchstabe gemäß Definition durch den Unicode-Standard 2,0. Dazu gehören die lateinischen Zeichen von a bis z und von A bis Z sowie Buchstaben aus anderen Sprachen.  
  
    -   Ein Unterstrich (_)  
  
2.  Im Anschluss daran können die folgenden Zeichen verwendet werden:  
  
    -   Im Unicode-Standard 2,0 definierte Buchstaben.  
  
    -   Dezimalzahlen aus dem lateinischen Grundalphabet oder anderen nationalen Schriften.  
  
    -   Ein Unterstrich (_)  
  
3.  Der Bezeichner darf kein reserviertes DMX-Wort sein. Bei reservierten Wörtern wird in DMX nicht nach Groß-/Kleinschreibung unterschieden. Weitere Informationen finden Sie unter [reservierte Schlüsselwörter &#40;DMX-&#41;](../dmx/reserved-keywords-dmx.md).  
  
4.  Der Bezeichner darf keine eingebetteten Leerzeichen oder Sonderzeichen enthalten.  
  
 Wenn Sie in DMX-Anweisungen Bezeichner verwenden, die nicht diesen Regeln entsprechen, müssen Sie diese Bezeichner in eckige Klammern setzen.  
  
##  <a name="delimited-identifiers"></a><a name="DelimitedIdentifiers"></a> Begrenzungs Bezeichner  
 Begrenzungsbezeichner sind in eckige Klammern ([ ]) eingeschlossen.  Das folgende Beispiel besteht aus einer DMX-Anweisung mit einem Begrenzungsbezeichner, der diesen Regeln entspricht.  
  
```  
SELECT * FROM [Marketing_Clusters].CONTENT;  
```  
  
 Ein Bezeichner, der nicht den Formatregeln regulärer Bezeichner entspricht, muss immer begrenzt sein. Das folgende Beispiel besteht aus einer DMX-Anweisung mit einem Begrenzungsbezeichner, der ein Leerzeichen enthält:  
  
```  
SELECT * FROM [Targeted Mailing].CONTENT;  
```  
  
 Verwenden Sie Begrenzungsbezeichner in folgenden Zusammenhängen:  
  
-   Wenn Sie reservierte Wörter für Objektnamen oder Teile von Objektnamen verwenden.  
  
     Es empfiehlt sich, keine reservierten Schlüsselwörter als Objektnamen zu verwenden. Datenbanken, die Sie von früheren Versionen von aktualisieren, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] enthalten möglicherweise Bezeichner, die Wörter enthalten, die in der früheren Version von nicht reserviert waren, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] für die jedoch reservierte Wörter sind [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Bis Sie den Namen des Objekts ggf. ändern, können Sie mit einem Begrenzungsbezeichner auf ein solches Objekt verweisen.  
  
-   Wenn Sie Zeichen verwenden, die nicht als qualifizierte Bezeichner aufgeführt sind.  
  
     In [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] können Sie in einem Begrenzungsbezeichner jedes Zeichen verwenden, das zur aktuellen Codepage gehört. Die unbedachte Verwendung von Sonderzeichen in einem Objektnamen kann allerdings dazu führen, dass DMX-Anweisungen schwierig zu lesen und zu pflegen sind.  
  
### <a name="rules-for-delimited-identifiers"></a>Regeln für Begrenzungsbezeichner  
 Regeln für das Format von Begrenzungsbezeichnern:  
  
1.  Begrenzungsbezeichner können die gleiche Anzahl von Zeichen enthalten wie reguläre Bezeichner (von 1 bis 100 Zeichen, wobei die Begrenzungszeichen nicht gezählt werden).  
  
2.  Der Textkörper eines Bezeichners kann jede Kombination von Zeichen der aktuellen Codepage enthalten, einschließlich der Begrenzungszeichen selbst. Enthält der Textkörper eines Bezeichners selbst Begrenzungszeichen, kann ein spezieller Schritt erforderlich sein:  
  
    -   Enthält der Textkörper des Bezeichners eine linke eckige Klammer ([), ist kein zusätzlicher Schritt erforderlich.  
  
    -   Enthält der Textkörper des Bezeichners eine rechte eckige Klammer (]), müssen Sie zwei rechte eckige Klammern angeben, um die Klammer in der Codepage darzustellen.  
  
### <a name="delimiting-identifiers-with-multiple-parts"></a>Begrenzen von mehrteiligen Bezeichnern  
 Wenn Sie vollqualifizierte Objektnamen verwenden, ist es u. U. erforderlich, mehr als einen der Bezeichner, aus denen sich der Objektname zusammensetzt, zu begrenzen. Sie müssen jeden Bezeichner einzeln begrenzen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Data Mining-Erweiterungen &#40;DMX-&#41; Referenz](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Elemente](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Funktionsreferenz](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Operator Verweis](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41;-Anweisungs Referenz](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining-Erweiterungen &#40;DMX-&#41; Syntax Konventionen](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Allgemeine Vorhersagefunktionen &#40;DMX-&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Struktur und Verwendung von DMX-Vorhersage Abfragen](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Grundlegendes zur SELECT-Anweisung (DMX)](../dmx/understanding-the-dmx-select-statement.md)  
  
  
