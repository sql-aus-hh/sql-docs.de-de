---
title: Erweiterbarkeitsarchitektur in SQL Server-Spracherweiterungen
titleSuffix: ''
description: Erfahren Sie mehr über die Erweiterbarkeitsarchitektur, die für SQL Server-Spracherweiterungen verwendet wird und Ihnen das Ausführen von externem Code in SQL Server ermöglicht. In SQL Server 2019 werden Java, Python und R unterstützt. Der Code wird in einer Language Runtime-Umgebung als Erweiterung der Hauptdatenbank-Engine ausgeführt.
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 344e5f35e35202b6a44a4932db8a27424fdb8753
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471801"
---
# <a name="extensibility-architecture-in-sql-server-language-extensions"></a>Erweiterbarkeitsarchitektur in SQL Server-Spracherweiterungen

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Erfahren Sie mehr über die Erweiterbarkeitsarchitektur, die für SQL Server-Spracherweiterungen verwendet wird und Ihnen das Ausführen von externem Code in SQL Server ermöglicht. In SQL Server 2019 werden Java, Python und R unterstützt. Der Code wird in einer Language Runtime-Umgebung als Erweiterung der Haupt-Datenbank-Engine ausgeführt.

## <a name="background"></a>Hintergrund

Der Zweck des Erweiterbarkeitsframeworks besteht darin, eine Schnittstelle zwischen SQL Server und externen Sprachen bereitzustellen. Durch das Ausführen einer vertrauenswürdigen Sprache innerhalb eines von SQL Server verwalteten sicheren Frameworks können Datenbankadministratoren die Sicherheit aufrechterhalten und gleichzeitig Datenanalysten den Zugriff auf Unternehmensdaten ermöglichen.

<!-- We need to get a diagram like the one below.
The following diagram visually describes opportunities and benefits of the extensible architecture.

  ![Goals of integration with SQL Server](../media/ml-service-value-add.png "Machine Learning Services Value Add")
-->

Jede unterstützte externe Sprache kann durch Aufrufen einer gespeicherten Prozedur ausgeführt werden, und die Ergebnisse werden als tabellarische Ergebnisse direkt an SQL Server zurückgegeben. Dies vereinfacht das Verwenden der externen Sprache aus einer beliebigen Anwendung, die eine SQL-Abfrage senden und die Ergebnisse verarbeiten kann.

## <a name="architecture-diagrams"></a>Architekturdiagramme

Die Architektur ist so konzipiert, dass externer Code in einem separaten Prozess von SQL Server ausgeführt wird, jedoch mit Komponenten, die intern die Kette von Anforderungen für Daten und Vorgänge auf SQL Server verwalten. 
  
  ***Komponentenarchitektur unter Windows:** _

  ![Komponentenarchitektur unter Windows](../media/generic-architecture-windows.png "Komponentenarchitektur unter Windows")
  
  _*_Komponentenarchitektur unter Linux:_*_
  
  ![Komponentenarchitektur unter Linux](../media/generic-architecture-linux.png "Komponentenarchitektur unter Linux")
  
Zu den Komponenten gehört ein *Launchpad*-Dienst, der zum Aufrufen externer Runtimes (z. B. Java) und bibliotheksspezifischer Logik für das Laden von Interpretern und Bibliotheken verwendet wird.

<a name="launchpad"></a>

## <a name="launchpad"></a>Launchpad

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] ist ein Dienst, der die Lebensdauer, Ressourcen und Sicherheitsgrenzen des externen Prozesses verwaltet, der für die Skriptausführung verantwortlich ist. Dies ähnelt der Art und Weise, wie die Volltextindizierung und der Abfragedienst einen separaten Host für die Verarbeitung von Volltextabfragen starten. Der Launchpad-Dienst kann nur vertrauenswürdige Startprogramme starten, die von Microsoft veröffentlicht oder durch Microsoft zertifiziert wurden, da sie die Anforderungen hinsichtlich der Leistung und der Ressourcenverwaltung erfüllen.

Der [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)]-Dienst wird unter **SQLRUserGroup** ausgeführt, das [AppContainers](/windows/desktop/secauthz/appcontainer-isolation) für die Ausführungsisolierung verwendet.

Für jede Instanz der Datenbank-Engine, der Sie SQL Server-Spracherweiterungen hinzugefügt haben, wird ein separater [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)]-Dienst erstellt. Es gibt einen Launchpad-Dienst pro Datenbank-Engine-Instanz. Wenn Sie also über mehrere Instanzen mit externer Skriptunterstützung verfügen, haben Sie für jede Instanz einen Launchpad-Dienst. Eine Datenbank-Engine-Instanz ist an den für sie erstellten Launchpad-Dienst gebunden. Alle Aufrufe externer Skripts in einer gespeicherten Prozedur oder in T-SQL führen dazu, dass der SQL Server-Dienst den für dieselbe Instanz erstellten Launchpad-Dienst aufruft.

Zum Ausführen von Aufgaben in einer bestimmten unterstützten Sprache erhält das Launchpad ein gesichertes Workerkonto aus dem Pool und startet einen Satellitenprozess zum Verwalten der externen Runtimes. Jeder Satellitenprozess erbt das Benutzerkonto des Launchpads und verwendet dieses Workerkonto für die Dauer der Skriptausführung. Wenn Skripts parallele Prozesse verwenden, werden sie unter demselben Einzelworkerkonto erstellt.

## <a name="communication-channels-between-components"></a>Kommunikationskanäle zwischen Komponenten

Im folgenden Abschnitt werden die Kommunikationsprotokolle zwischen Komponenten und Datenplattformen beschrieben.

+ **TCP/IP**

  Standardmäßig wird für die interne Kommunikation zwischen SQL Server und SQL Satellite TCP/IP verwendet.

+ **ODBC**

  Für die Kommunikation zwischen externen Data Science-Clients und einer SQL Server-Remoteinstanz wird ODBC verwendet. Das Konto, das Skriptaufträge an SQL Server übermittelt, muss über beide Berechtigungen zum Herstellen einer Verbindung mit der Instanz und zum Ausführen externer Skripts verfügen.

  Außerdem benötigt das Konto abhängig von der Aufgabe möglicherweise die folgenden Berechtigungen zum:

  + Lesen der vom Auftrag verwendeten Daten
  + Schreiben von Daten in Tabellen, z. B. beim Speichern von Ergebnissen in einer Tabelle
  + Erstellen von Datenbankobjekten, z. B. beim Speichern eines externen Skripts als Teil einer neuen gespeicherten Prozedur

  Wenn SQL Server als Computekontext für Skripts verwendet wird, die von einem Remoteclient ausgeführt werden, und die ausführbare Datei Daten aus einer externen Quelle abrufen muss, wird ODBC für den Rückschreibevorgang verwendet. SQL Server ordnet die Identität des Benutzers, der den Remotebefehl ausgibt, der Identität des Benutzers auf der aktuellen Instanz zu und führt den ODBC-Befehl mit den Anmeldeinformationen dieses Benutzers aus. Die Verbindungszeichenfolge, die zum Durchführen dieses ODBC-Aufruf erforderlich ist, wird vom Clientcode abgerufen.

+ **Weitere Protokolle**

  Prozesse, die möglicherweise in „Blöcken“ arbeiten oder Daten zurück an einen Remoteclient übertragen müssen, können auch das [XDF-Dateiformat](/machine-learning-server/r/concept-what-is-xdf) verwenden. Die tatsächliche Datenübertragung erfolgt über codierte Blobs.

## <a name="next-steps"></a>Nächste Schritte

+ [Was sind Spracherweiterungen?](../language-extensions-overview.md)