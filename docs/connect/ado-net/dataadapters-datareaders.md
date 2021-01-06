---
title: "\"DataAdapters\" und \"DataReaders\""
description: Hier erfahren Sie mehr über die DataReader-Klasse, die Daten aus einer Datenbank abruft, sowie über die DataAdapter-Klasse, die Daten aus einer Datenquelle abruft und eine DataSet-Klasse auffüllt, im Microsoft SqlClient-Datenanbieter für SQL Server.
ms.date: 11/30/2020
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3a252ca44683c062e531081f0b625a5e2359d236
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771386"
---
# <a name="dataadapters-and-datareaders"></a>"DataAdapters" und "DataReaders"

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Mit der **DataReader**-Klasse im Microsoft SqlClient-Datenanbieter für SQL Server können Sie einen schreibgeschützten Vorwärtsdatenstrom aus einer Datenbank abrufen. Die Ergebnisse werden bei der Ausführung der Abfrage zurückgegeben und im Netzwerkpuffer auf dem Client gespeichert, bis Sie sie mit der **Read**-Methode der **DataReader**-Klasse anfordern. Mit der **DataReader**-Klasse kann die Leistung der Anwendung gesteigert werden, indem die Daten abgefragt werden, sobald sie verfügbar sind, und (standardmäßig) immer nur eine Zeile im Arbeitsspeicher gespeichert wird, wodurch der Systemmehraufwand verringert wird.

Ein <xref:System.Data.Common.DataAdapter> wird zum Abrufen von Daten aus einer Datenquelle und zum Auffüllen von Tabellen in einem <xref:System.Data.DataSet> verwendet. Mit dem `DataAdapter` werden außerdem im `DataSet` vorgenommene Änderungen für die Datenquelle übernommen. Die `DataAdapter`-Klasse verwendet das `Connection`-Objekt des Microsoft SqlClient-Datenanbieters für SQL Server zum Herstellen einer Verbindung mit einer Datenquelle und `Command`-Objekte zum Abrufen von Daten aus der Datenquelle und Auflösen von Änderungen an der Datenquelle.

.NET enthält ein <xref:System.Data.Common.DbDataReader>- und ein <xref:System.Data.Common.DbDataAdapter>-Objekt, der Microsoft SqlClient-Datenanbieter für SQL Server ein <xref:Microsoft.Data.SqlClient.SqlDataReader>- und ein <xref:Microsoft.Data.SqlClient.SqlDataAdapter>-Objekt.

## <a name="in-this-section"></a>In diesem Abschnitt

[Abrufen von Daten durch einen DataReader](retrieve-data-by-datareader.md)  
Beschreibt das ADO.NET-**DataReader**-Objekt und seine Verwendung zum Zurückgeben eines Ergebnisdatenstroms aus einer Datenquelle

[Auffüllen eines Datasets aus einem DataAdapter](populate-dataset-from-dataadapter.md)  
Beschreibt die Vorgehensweise beim Füllen eines `DataSet` mit Tabellen, Spalten und Zeilen mit einem `DataAdapter`.

[DataAdapter-Parameter](dataadapter-parameters.md)  
Beschreibt die Verwendung von Parametern mit den Befehlseigenschaften eines `DataAdapter`, einschließlich des Zuordnens der Inhalte einer Spalte in einem `DataSet` zu einem Befehlsparameter.

[Hinzufügen von vorhandenen Einschränkungen zu einem DataSet](add-existing-constraints-to-dataset.md)  
Beschreibt das Hinzufügen vorhandener Einschränkungen zu einem `DataSet`.

[DataAdapter-, DataTable-und DataColumn-Zuordnungen](dataadapter-datatable-datacolumn-mappings.md)  
Beschreibt das Einrichten von `DataTableMappings` und `ColumnMappings` für einen `DataAdapter`.

[Paging durch ein Abfrageergebnis](paging-through-query-result.md)  
Enthält ein Beispiel für das Anzeigen der Ergebnisse einer Abfrage als Datenseiten.

[Aktualisieren von Datenquellen mit DataAdapters](update-data-sources-with-dataadapters.md)  
Beschreibt das Verwenden eines `DataAdapter`, um Änderungen in einem `DataSet` in der Datenbank zu aktualisieren.

[Verarbeiten von DataAdapter-Ereignissen](handle-dataadapter-events.md)  
Beschreibt `DataAdapter`-Ereignisse und deren Verwendung.

[Batchvorgänge mit DataAdapters](batch-operations-using-dataadapters.md)  
Beschreibt, wie die Anwendungsleistung verbessert werden kann, indem die Anzahl von Roundtrips zu SQL Server beim Anwenden von Updates aus dem `DataSet` reduziert wird.

## <a name="see-also"></a>Weitere Informationen

- [Herstellen einer Verbindung mit Datenquellen](connecting-to-data-source.md)
- [Befehle und Parameter](commands-parameters.md)
- [Transaktionen und Parallelität](transactions-and-concurrency.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
