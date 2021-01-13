---
title: Datenablaufverfolgung in SqlClient
description: Beschreibt, wie der Microsoft SqlClient-Datenanbieter für SQL Server integrierte Funktionen zur Datenablaufverfolgung bereitstellt.
ms.date: 12/04/2020
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: fc8b5a7ca06af3c3e3ea83fcb747f79517e5ef7a
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744436"
---
# <a name="data-tracing-in-sqlclient"></a>Datenablaufverfolgung in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET bietet integrierte Funktionen zur Datenablaufverfolgung, die vom Microsoft SqlClient-Datenanbieter für SQL Server und SQL Server-Netzwerkprotokolle unterstützt werden.

Die Ablaufverfolgung von Aufrufen der Datenzugriffs-API kann bei der Diagnose der folgenden Probleme helfen:

- Fehlende Schemaübereinstimmung zwischen Clientprogramm und Datenbank.

- Nicht verfügbare Datenbank oder Probleme mit der Netzwerkbibliothek.

- Fehlerhafte SQL-Anweisungen (sowohl hart codiert als auch von einer Anwendung generiert).

- Fehlerhafte Programmierlogik.

- Probleme, die sich aus der Interaktion zwischen dem Microsoft SqlClient-Datenanbieter SQL Server und Ihren eigenen Komponenten ergeben.

Zur Unterstützung verschiedener Ablaufverfolgungstechnologien ist die Ablaufverfolgung erweiterbar, sodass Entwickler eine Ablaufverfolgung für Probleme auf jeder Ebene des Anwendungsstapels ausführen können. Der Microsoft SqlClient-Datenanbieter für SQL Server nutzt die Vorteile der generalisierten Ablaufverfolgungs- und Instrumentierungs-APIs.

Weitere Informationen zum Festlegen und Konfigurieren der verwalteten Ablaufverfolgung in .NET finden Sie unter [Datenzugriffs-Ablaufverfolgung](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)).

## <a name="access-diagnostic-information-in-the-extended-events-log"></a>Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse

Im Microsoft SqlClient-Datenanbieter für SQL Server vereinfacht die Datenzugriffs-Ablaufverfolgung ([Datenzugriffs-Ablaufverfolgung](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10))) die Zuordnung von Clientereignissen zu Diagnoseinformationen, z. B. Verbindungsfehler, aus dem Verbindungsringpuffer des Servers und Informationen zur Anwendungsleistung im Protokoll für erweiterte Ereignisse. Informationen dazu, wie Sie das Protokoll für erweiterte Ereignisse lesen, finden Sie unter [View Event Session Data](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110)).

Bei Verbindungsvorgängen sendet der Microsoft SqlClient-Datenanbieter für SQL Server eine Clientverbindungs-ID. Bei einem Verbindungsfehler haben Sie die Möglichkeit, auf den Konnektivitätsringpuffer zuzugreifen ([Konnektivitätsproblembehebung in SQL Server 2008 mit dem Konnektivitätsringpuffer](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) und über das Feld `ClientConnectionID` Diagnoseinformationen zum jeweiligen Verbindungsfehler abzurufen. Clientverbindungs-IDs werden nur im Ringpuffer protokolliert, wenn ein Fehler auftritt. Wenn vor dem Senden des prelogin-Pakets keine Verbindung hergestellt werden kann, wird keine Clientverbindungs-ID generiert. Die Clientverbindungs-ID ist eine 16-Byte-GUID. Sie finden die Clientverbindungs-ID auch in der Zielausgabe für erweiterte Ereignisse, wenn die `client_connection_id`-Aktion den Ereignissen in einer Sitzung für erweiterte Ereignisse hinzugefügt wurde. Sie können die Ablaufverfolgung für den Datenzugriff aktivieren, den Verbindungsbefehl erneut ausführen und das `ClientConnectionID`-Feld in der Datenzugriffs-Ablaufverfolgung beobachten, wenn Sie weitere Diagnoseinformationen zum Clienttreiber benötigen.

Sie können die Clientverbindungs-ID programmgesteuert abrufen, indem Sie die `SqlConnection.ClientConnectionID`-Eigenschaft verwenden.

> [!NOTE]
> Der Microsoft SqlClient-Datenanbieter für SQL Server unterstützt die Serverprozess-ID seit Version 2.1.0. Sie können sie programmgesteuert abrufen, indem Sie die Eigenschaft `SqlConnection.ServerProcessId` verwenden.

`ClientConnectionID` und `ServerProcessId` sind für ein <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekt verfügbar, das erfolgreich eine Verbindung hergestellt hat. Wenn ein Verbindungsfehler auftritt, ist `ClientConnectionID` u. U. über `SqlException.ToString` verfügbar.

Der Microsoft SqlClient-Datenanbieter für SQL Server sendet auch eine threadspezifische Aktivitäts-ID. Die Aktivitäts-ID wird in den Sitzungen für erweiterte Ereignisse aufgezeichnet, wenn die Sitzungen bei aktivierter TRACK_CAUSALITY-Option gestartet werden. Bei Leistungsproblemen mit einer aktiven Verbindung können Sie die Aktivitäts-ID aus der Datenzugriffs-Ablaufverfolgung des Clients abrufen (`ActivityID`-Feld) und dann die Aktivitäts-ID in der Ausgabe für die erweiterten Ereignisse suchen. Die Aktivitäts-ID in erweiterten Ereignissen ist eine 16-Byte-GUID (entspricht nicht der GUID für die Clientverbindungs-ID), an die eine 4-Byte-Sequenznummer angehängt ist. Die Sequenznummer steht für die Reihenfolge einer Anforderung innerhalb eines Threads und gibt die relative Reihenfolge des Batches und der RPC-Anweisungen für den Thread an. `ActivityID` wird derzeit optional für SQL-Batchanweisungen und RPC-Anforderungen gesendet, wenn die Ablaufverfolgung für den Datenzugriff aktiviert ist und das 18. Bit im Konfigurationswort der Datenzugriffs-Ablaufverfolgung ON lautet.

Die folgende SQL-Anweisung ist ein Beispiel, das Transact-SQL verwendet, um eine Sitzung für erweiterte Ereignisse zu starten, die in einem Ringpuffer gespeichert werden und die von einem Client in RPC- und Batch-Vorgängen gesendete Aktivitäts-ID aufzeichnen soll.

```sql
create event session MySession on server
add event connectivity_ring_buffer_recorded,
add event sql_statement_starting (action (client_connection_id)),
add event sql_statement_completed (action (client_connection_id)),
add event rpc_starting (action (client_connection_id)),
add event rpc_completed (action (client_connection_id))
add target ring_buffer with (track_causality=on)
```

## <a name="see-also"></a>Weitere Informationen:
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
