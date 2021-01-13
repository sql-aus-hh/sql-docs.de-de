---
title: Leistungsindikatoren in SqlClient
description: Verwenden Sie die Leistungsindikatoren des Microsoft SqlClient-Datenanbieters für SQL Server, um den Status Ihrer Anwendung und deren Verbindungsressourcen mithilfe des Windows-Leistungsmonitors oder programmgesteuert zu überwachen.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e9d8c2edb88a9ed50b47c761d3af8aec8016065a
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804291"
---
# <a name="performance-counters-in-sqlclient"></a>Leistungsindikatoren in SqlClient

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Mithilfe von <xref:Microsoft.Data.SqlClient>-Leistungsindikatoren können Sie den Status Ihrer Anwendung und die von ihr verwendeten Verbindungsressourcen überwachen. Für die Überwachung der Leistungsindikatoren kann der Windows-Leistungsmonitor verwendet werden, und die <xref:System.Diagnostics.PerformanceCounter>-Klasse im <xref:System.Diagnostics>-Namespace ermöglicht den programmgesteuerten Zugriff auf diese Indikatoren.

## <a name="available-performance-counters"></a>Verfügbare Leistungsindikatoren

Zurzeit sind 14 verschiedene Leistungsindikatoren für <xref:Microsoft.Data.SqlClient> verfügbar, wie in der folgenden Tabelle beschrieben.

|Leistungsindikator|BESCHREIBUNG|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|Anzahl der Verbindungen, die pro Sekunde mit einem Datenbankserver hergestellt wurden|  
|`HardDisconnectsPerSecond`|Anzahl der Verbindungen mit einem Datenbankserver, die pro Sekunde getrennt wurden|  
|`NumberOfActiveConnectionPoolGroups`|Anzahl der eindeutigen Verbindungspoolgruppen, die aktiv sind. Dieser Indikator wird von der Anzahl der eindeutigen Verbindungszeichenfolgen gesteuert, die in der Anwendungsdomäne (AppDomain) gefunden werden.|  
|`NumberOfActiveConnectionPools`|Gesamtzahl der Verbindungspools|  
|`NumberOfActiveConnections`|Anzahl der aktiven Verbindungen, die gerade genutzt werden **Hinweis:**  Dieser Leistungsindikator ist standardmäßig nicht aktiviert. Informationen zum Aktivieren dieses Leistungsindikators finden Sie weiter unten unter [Aktivieren von Indikatoren, die standardmäßig deaktiviert sind](#ActivatingOffByDefault).|  
|`NumberOfFreeConnections`|Anzahl der Verbindungen, die für die Verwendung in den Verbindungspools verfügbar sind **Hinweis:**  Dieser Leistungsindikator ist standardmäßig nicht aktiviert. Informationen zum Aktivieren dieses Leistungsindikators finden Sie weiter unten unter [Aktivieren von Indikatoren, die standardmäßig deaktiviert sind](#ActivatingOffByDefault).|  
|`NumberOfInactiveConnectionPoolGroups`|Anzahl der eindeutigen Verbindungspoolgruppen, die zum Löschen gekennzeichnet sind. Dieser Indikator wird von der Anzahl der eindeutigen Verbindungszeichenfolgen gesteuert, die in der Anwendungsdomäne (AppDomain) gefunden werden.|  
|`NumberOfInactiveConnectionPools`|Anzahl der inaktiven Verbindungspools, die in letzter Zeit keine Aktivitäten zu verzeichnen hatten und darauf warten, gelöscht zu werden|  
|`NumberOfNonPooledConnections`|Anzahl der aktiven Verbindungen, die sich nicht in einem Pool befinden|  
|`NumberOfPooledConnections`|Anzahl der aktiven Verbindungen, die von der Verbindungspooling-Infrastruktur verwaltet werden|  
|`NumberOfReclaimedConnections`|Anzahl der Verbindungen, die durch die Garbage Collection (automatische Speicherbereinigung) wieder verfügbar gemacht wurden, bei denen die Anwendung weder `Close` noch `Dispose` aufgerufen hat. **Hinweis:** Das nicht explizite Schließen oder Freigeben von Verbindungen beeinträchtigt die Leistung.|  
|`NumberOfStasisConnections`|Anzahl der Verbindungen, die derzeit den Abschluss einer Aktion erwarten und die daher für die Verwendung durch Ihre Anwendung nicht zur Verfügung stehen|  
|`SoftConnectsPerSecond`|Anzahl der aktiven Verbindungen, die aus dem Verbindungspool herausgezogen werden. **Hinweis:**  Dieser Leistungsindikator ist standardmäßig nicht aktiviert. Informationen zum Aktivieren dieses Leistungsindikators finden Sie weiter unten unter [Aktivieren von Indikatoren, die standardmäßig deaktiviert sind](#ActivatingOffByDefault).|  
|`SoftDisconnectsPerSecond`|Anzahl der aktiven Verbindungen, die an den Verbindungspool zurückgegeben werden **Hinweis:**  Dieser Leistungsindikator ist standardmäßig nicht aktiviert. Informationen zum Aktivieren dieses Leistungsindikators finden Sie weiter unten unter [Aktivieren von Indikatoren, die standardmäßig deaktiviert sind](#ActivatingOffByDefault).|  

### <a name="connection-pool-groups-and-connection-pools"></a>Verbindungspoolgruppen und Verbindungspools

Bei der Verwendung der Windows-Authentifizierung (integrierte Sicherheit) müssen die folgenden beiden Leistungsindikatoren überwacht werden: `NumberOfActiveConnectionPoolGroups` und `NumberOfActiveConnectionPools`. Dies liegt daran, dass Verbindungspoolgruppen eindeutigen Verbindungszeichenfolgen zugeordnet werden. Bei Verwendung der integrierten Sicherheit werden die Verbindungspools den Verbindungszeichenfolgen zugeordnet, und zusätzlich werden separate Pools für einzelne Windows-Identitäten erstellt. Wenn Fred und Julie z. B. innerhalb derselben Anwendungsdomäne beide die `"Data Source=MySqlServer;Integrated Security=true"`-Verbindungszeichenfolge verwenden, wird für die Verbindungszeichenfolge eine Verbindungspoolgruppe erstellt, und es werden zwei zusätzliche Pools erstellt: einer für Fred und einer für Julie. Wenn John und Martha eine Verbindungszeichenfolge mit einer identischen SQL Server-Anmeldung (`"Data Source=MySqlServer;User Id=<myUserID>;Password=<myPassword>"`) verwenden, wird nur ein Pool für die **<myUserID>** -Identität erstellt.

<a name="ActivatingOffByDefault"></a>

### <a name="activate-off-by-default-counters"></a>Aktivieren von Indikatoren, die standardmäßig deaktiviert sind

Die Leistungsindikatoren `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond` und `SoftConnectsPerSecond` sind standardmäßig deaktiviert. Fügen Sie zum Aktivieren dieser Indikatoren in der Konfigurationsdatei der Anwendung folgende Informationen hinzu:

```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  

## <a name="retrieve-performance-counter-values"></a>Abrufen von Leistungsindikatorwerten

Die folgende Konsolenanwendung zeigt, wie Sie in Ihrer Anwendung Leistungsindikatorwerte abrufen können. Damit Informationen für alle Leistungsindikatoren des Microsoft SqlClient-Datenanbieters für SQL Server zurückgegeben werden, müssen die Verbindungen geöffnet und aktiv sein.

> [!NOTE]
> In diesem Beispiel wird die [**AdventureWorks**-Beispieldatenbank](../../samples/adventureworks-install-configure.md) verwendet. Die im Beispielcode angegebenen Verbindungszeichenfolgen setzen voraus, dass die Datenbank auf dem lokalen Computer installiert und verfügbar ist, und dass Sie Anmeldungen erstellt haben, die mit den in den Verbindungszeichenfolgen angegebenen übereinstimmen. Falls Ihr Server mit den Standardsicherheitseinstellungen konfiguriert ist, die nur die Windows-Authentifizierung zulassen, müssen Sie die SQL Server-Anmeldungen u. U. erst aktivieren. Passen Sie die Verbindungszeichenfolgen an Ihre Umgebung an.

### <a name="example"></a>Beispiel

[!code-csharp[SqlClient_PerformanceCounter#1](~/../sqlclient/doc/samples/SqlClient_PerformanceCounter.cs#1)]

## <a name="see-also"></a>Weitere Informationen

- [Herstellen einer Verbindung mit Datenquellen](connecting-to-data-source.md)
- [Laufzeit-Profilerstellung](/dotnet/framework/debug-trace-profile/runtime-profiling)
- [Einführung in die Überwachung von Leistungsschwellenwerten](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
