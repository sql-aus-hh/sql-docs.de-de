---
title: DISCOVER_TRACE_EVENT_CATEGORIES-Rowset | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- analysis-services
- docset-sql-devref
ms.topic: reference
ms.assetid: 1ad74fd2-4740-469d-85b5-abf0171737fd
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: 4d5771be15bf30b7cd5d478281ff9d859c354f4d
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48140156"
---
# <a name="discovertraceeventcategories-rowset"></a>DISCOVER_TRACE_EVENT_CATEGORIES-Rowset
  Zeigt die Liste der Ereigniskategorien an, die vom Ablaufverfolgungsanbieter unterstützt werden.  
  
 **Gilt für:** tabellarische und mehrdimensionale Modelle  
  
## <a name="rowset-columns"></a>Rowsetspalten  
 Die `DISCOVER_TRACE_EVENT_CATEGORIES` Rowset enthält die folgenden Spalten.  
  
|Spaltenname|Typindikator|Länge|Description|  
|-----------------|--------------------|------------|-----------------|  
|`Data`|`DBTYPE_WSTR`||Enthält eine codierte XML-Zeichenfolge, die Ereigniskategorieinformationen zum Ablaufverfolgungsanbieter beschreibt, einschließlich Kategoriename, Typ und Beschreibung. Der Typ ist eine Zeichenfolge, die den Typ der Ereigniskategorie angibt. Folgende Enumerationswerte sind möglich:<br /><br /> -0 = Normal<br />– 1 = signifikante<br />– 2 = Fehler|  
  
 Dieses Schemarowset ist nicht sortiert.  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Verwenden von ADOMD.NET zum Zurückgeben des Rowsets  
 Wenn Sie Metadaten mithilfe von ADOMD.NET und des Schemarowsets abrufen, können Sie entweder die GUID verwenden oder eine Referenz für ein Schemarowsetobjekt in der GetSchemaDataSet-Methode herstellen. Weitere Informationen finden Sie unter [Working with Schema Rowsets in ADOMD.NET](../../../relational-databases/native-client-ole-db-rowsets/rowsets.md).  
  
 Die folgende Tabelle enthält die GUID und die Zeichenfolgenwerte, die dieses Rowset identifizieren.  
  
|Argument|value|  
|--------------|-----------|  
|GUID|a07ccd19-8148-11d0-87bb-00c04fc33942|  
|Zeichenfolge|DISCOVER_TRACE_EVENT_CATEGORIES|  
  
## <a name="see-also"></a>Siehe auch  
 [XML for Analysis – Schemarowsets](xml-for-analysis-schema-rowsets.md)  
  
  
