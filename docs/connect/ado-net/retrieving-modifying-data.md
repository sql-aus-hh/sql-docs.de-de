---
title: Abrufen und Ändern von Daten
description: In .NET dient der SqlClient-Datenanbieter für SQL Server von Microsoft als Brücke zwischen einer Anwendung und einer Datenquelle, um Daten zu lesen und zu aktualisieren.
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4275b7de0f31d03aa36ef31d8801fcdc0e9ec853
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771542"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>Abrufen und Ändern von Daten in ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Eine der wichtigsten Funktionen einer Datenbankanwendung ist das Herstellen einer Verbindung mit einer Datenquelle und das Abrufen der darin enthaltenen Daten. Der SqlClient-Datenanbieter dient als Brücke zwischen einer Anwendung und einer Datenquelle, die das Ausführen von Befehlen sowie das Abrufen von Daten mithilfe von **DataReader** oder **DataAdapter** ermöglicht. Eine Hauptfunktion jeder Datenbank ist die Fähigkeit, die in ihr gespeicherten Daten zu aktualisieren. Im Microsoft SqlClient-Datenanbieter für SQL Server werden zum Aktualisieren von Daten **DataAdapter**, die Objekte <xref:System.Data.DataSet> und **Command** und u. U. auch Transaktionen verwendet.

## <a name="in-this-section"></a>In diesem Abschnitt

[Herstellen einer Verbindung mit Datenquellen](connecting-to-data-source.md)  
Beschreibt, wie eine Verbindung zu einer Datenquelle hergestellt wird und wie mit Verbindungsereignissen gearbeitet wird.

[Verbindungszeichenfolgen](connection-strings.md)  
Enthält Themen, die verschiedene Aspekte der Verwendung von Verbindungszeichenfolgen beschreiben, einschließlich der Schlüsselwörter für Verbindungszeichenfolgen und Sicherheitsinformationen sowie das Speichern und Abrufen von Verbindungszeichenfolgen.

[Verbindungspooling](connection-pooling.md)  
Beschreibt Verbindungspooling für den Microsoft SqlClient-Datenanbieter für SQL Server

[Befehle und Parameter](commands-parameters.md)  
Enthält Themen, in denen beschrieben wird, wie Befehlen und Befehlsgeneratoren erstellt und Parameter konfiguriert werden und welche Vorgehensweise beim Ausführen von Befehlen zum Abrufen und Bearbeiten von Daten ausgeführt werden muss.

["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)  
Enthält Themen, in denen DataReader, DataAdapter, Parameter und die Vorgehensweise bei DataAdapter-Ereignissen und beim Ausführen von Batchvorgängen beschrieben werden.

[Transaktionen und Parallelität](transactions-and-concurrency.md)  
Enthält Themen, in denen beschrieben wird, wie lokale Transaktionen und verteilte Transaktionen ausgeführt werden und wie Sie mit vollständiger Parallelität arbeiten.

[Abrufen von Datenbankschemainformationen](retrieving-database-schema-information.md)  
Beschreibt, wie verfügbare Datenbanken oder Kataloge, Tabellen und Ansichten in einer Datenbank, für Tabellen vorhandene Einschränkungen und andere Schemainformationen aus einer Datenquelle abgerufen werden.

[DbProviderFactories](dbproviderfactories.md)  
Beschreibt das Anbietermodell für Zuordnungsinstanzen und veranschaulicht, wie die Basisklassen im `System.Data.Common`-Namespace verwendet werden.  

[Abrufen von Identity- oder Autonumber-Werten](retrieve-identity-or-autonumber-values.md)  
Stellt ein Beispiel für die Zuordnung der für eine **identity**-Spalte in einer SQL Server-Tabelle generierten Werte zu einer Spalte einer eingefügten Zeile in einer Tabelle bereit. Erläutert das Zusammenführen von Identitätswerten in einer `DataTable`.  
  
[Abrufen von Binärdaten](retrieve-binary-data.md)  
Beschreibt, wie Binärdaten oder große Datenstrukturen mithilfe von `CommandBehavior` abgerufen werden. `SequentialAccess` zum Ändern des Standardverhaltens einer `DataReader`-Instanz.  
  
[Ändern von Daten mit gespeicherten Prozeduren](modify-data-with-stored-procedures.md)  
Beschreibt, wie mit Eingabe- und Ausgabeparametern von gespeicherten Prozeduren eine Zeile in eine Datenbank eingefügt wird, wodurch ein neuer Identitätswert zurückgegeben wird.  

[Datenablaufverfolgung in SqlClient](data-tracing.md)  
Beschreibt, wie der Microsoft SqlClient-Datenanbieter für SQL Server integrierte Funktionen zur Datenablaufverfolgung bereitstellt.  
  
[Leistungsindikatoren in SqlClient](performance-counters.md)  
Beschreibt die verfügbaren Leistungsindikatoren für die Microsoft SqlClient-Datenanbieter für SQL Server.  
  
[Asynchrone Programmierung](asynchronous-programming.md)  
Beschreibt die Unterstützung der Microsoft SqlClient-Datenanbieter für SQL Server für die asynchrone Programmierung.  
  
[SqlClient-Streamingunterstützung](sqlclient-streaming-support.md)  
Erläutert, wie Sie Anwendungen schreiben, die Daten aus SQL Server streamen, ohne sie vollständig in den Arbeitsspeicher zu laden.  

## <a name="see-also"></a>Weitere Informationen:

- [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server und ADO.NET](./sql/index.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
