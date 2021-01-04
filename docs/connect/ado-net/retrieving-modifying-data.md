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
ms.openlocfilehash: c3cf3766ffc6c8acf6025b58aa0adbaafafa2188
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038958"
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

Der Artikel [Transaktionen und Parallelität](transactions-and-concurrency.md) befasst sich mit der Durchführung lokaler Transaktionen, verteilter Transaktionen und der Arbeit mit der optimistischen Nebenläufigkeit.

Im Artikel [Abrufen von Informationen zum Datenbankschema](retrieving-database-schema-information.md) wird das Abrufen verfügbarer Datenbanken oder Kataloge, Tabellen und Ansichten in einer Datenbank, für Tabellen vorliegende Einschränkungen und andere Schemainformationen aus einer Datenquelle beschrieben.

## <a name="see-also"></a>Weitere Informationen:

- [Datentypzuordnungen in ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server und ADO.NET](./sql/index.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
