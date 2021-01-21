---
title: SQL Server-Verbindungspooling (ADO.NET)
description: Erfahren Sie, wie der Microsoft SqlClient-Datenanbieter für SQL Server den Zeitaufwand für das Öffnen von Verbindungen mithilfe des SQL Server-Verbindungspoolings minimiert, wodurch der Aufwand für neue Verbindungen reduziert wird.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 7e51d44e-7c4e-4040-9332-f0190fe36f07
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 9538368cdcc39d3a037c609fc79f65d5d3cb8e45
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596382"
---
# <a name="sql-server-connection-pooling-adonet"></a>SQL Server-Verbindungspooling (ADO.NET)

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Beim Herstellen einer Verbindung mit einem Datenbankserver müssen normalerweise mehrere zeitaufwändige Schritte ausgeführt werden. Es muss u. a. ein physischer Channel (z. B. ein Socket oder eine benannte Pipe) erstellt, der anfängliche Handshake durchgeführt, die Informationen der Verbindungszeichenfolge analysiert, die Verbindung vom Server authentifiziert und Überprüfungen zum Eintragen in die aktuelle Transaktion ausgeführt werden.

In der Praxis verwenden die meisten Anwendungen nur eine oder einige unterschiedliche Konfigurationen für Verbindungen. Dies bedeutet, dass beim Ausführen von Anwendungen viele identische Verbindungen wiederholt geöffnet und geschlossen werden. Um den Aufwand für das Herstellen von Verbindungen zu reduzieren, verwendet der Microsoft SqlClient-Datenanbieter für SQL Server eine Optimierungstechnik, die auch als *Verbindungspooling* bezeichnet wird.

Beim Verbindungspooling wird die Häufigkeit reduziert, mit der neue Verbindungen hergestellt werden müssen. Die *Poolfunktion* bleibt für die physische Verbindung zuständig. Er verwaltet Verbindungen, indem er eine Reihe aktiver Verbindungen für jede angegebene Verbindungskonfiguration beibehält. Sobald ein Benutzer `Open` für eine Verbindung aufruft, sucht der Pooler nach einer verfügbaren Verbindung im Pool. Wenn eine Verbindung in einem Pool verfügbar ist, gibt der Pooler die Verbindung an den Aufrufer zurück, anstatt eine neue Verbindung herzustellen. Wenn die Anwendung `Close` für eine Verbindung aufruft, wird diese vom Pooler an die aktiven Verbindungen im Pool zurückgegeben, anstatt diese zu beenden. Nachdem die Verbindung an den Pool zurückgegeben wurde, kann sie für den nächsten `Open`-Aufruf erneut verwendet werden.

Es können nur Verbindungen mit derselben Konfiguration zu einem Pool zusammengefasst werden. Der Microsoft SqlClient-Datenanbieter für SQL Server verwaltet mehrere Pools gleichzeitig, und zwar einen pro Konfiguration. Verbindungen werden durch Verbindungszeichenfolgen sowie bei Verwendung der integrierten Sicherheit durch Windows-Identitäten in Pools unterteilt. Verbindungen werden auch auf der Basis ihrer Eintragung in eine Transaktion zu einem Pool zusammengefasst. Bei Verwendung von <xref:Microsoft.Data.SqlClient.SqlConnection.ChangePassword%2A> wirkt sich die <xref:Microsoft.Data.SqlClient.SqlCredential>-Instanz auf den Verbindungspool aus. Verschiedene Instanzen von <xref:Microsoft.Data.SqlClient.SqlCredential> verwenden verschiedene Verbindungspools, auch wenn die Benutzer-ID und das Kennwort übereinstimmen.

Durch Verbindungspooling kann die Leistung und Skalierbarkeit einer Anwendung wesentlich erhöht werden. Verbindungspooling ist im Microsoft SqlClient-Datenanbieter für SQL Server standardmäßig aktiviert. Verbindungen werden beim Öffnen und Schließen in der Anwendung vom Pooler optimiert, außer wenn dies explizit deaktiviert wurde. Sie können auch mehrere Modifizierer für Verbindungszeichenfolgen angeben, um das Verhalten des Verbindungspoolings zu steuern. Weitere Informationen finden Sie unter **Steuern des Verbindungspoolings mit Schlüsselwörtern für Verbindungszeichenfolgen** weiter unten in diesem Thema.

> [!IMPORTANT]
> Wenn Verbindungspooling aktiviert ist und ein Timeout- oder anderer Anmeldefehler auftritt, wird eine Ausnahme ausgelöst. Nachfolgende Verbindungsversuche schlagen in den nächsten **5** Sekunden (`blocking period`) fehl. Wenn die Anwendung versucht, innerhalb der Sperrfrist eine Verbindung herzustellen, wird die erste Ausnahme erneut ausgelöst. Nachfolgende Verbindungsfehler nach Ablauf eines Sperrzeitraums führen zu einem neuen Sperrzeitraum, der doppelt so lang ist wie der vorherige Sperrzeitraum, bis zu einem *Höchstwert von **1** Minute*.

> [!NOTE]
> Der Mechanismus für `blocking period` gilt nicht standardmäßig für Azure SQL Server. Dieses Verhalten kann, außer bei *.NET Standard*, durch Ändern der Eigenschaft <xref:Microsoft.Data.SqlClient.PoolBlockingPeriod> in <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString> geändert werden.

## <a name="pool-creation-and-assignment"></a>Poolerstellung und -zuweisung

Wenn eine Verbindung erstmals hergestellt wird, wird ein Verbindungspool basierend auf einem exakt übereinstimmenden Algorithmus erstellt. Damit wird der Pool der Verbindungszeichenfolge in der Verbindung zugewiesen. Jedem Verbindungspool wird eine eindeutige Verbindungszeichenfolge zugeordnet. Wenn beim Öffnen einer neuen Verbindung die Verbindungszeichenfolge nicht genau mit einem vorhanden Pool übereinstimmt, wird ein neuer Pool erstellt.

> [!NOTE]
> Verbindungen werden pro _Prozess_, pro _Anwendungsdomäne_, pro _Verbindungszeichenfolge_ und bei Verwendung der integrierten Sicherheit pro _Windows-Identität_ in einem Pool zusammengefasst. Verbindungszeichenfolgen müssen ebenfalls exakt übereinstimmen. Schlüsselwörter, die für dieselbe Verbindung in einer anderen Reihenfolge bereitgestellt werden, werden in separaten Pools zusammengefasst.

> [!NOTE]
> Wenn die `MinPoolSize` nicht in der Verbindungszeichenfolge oder als 0 (null) angegeben ist, wird der Pool nach einer bestimmten Zeit der Inaktivität geschlossen. Wenn die angegebene `MinPoolSize` größer als 0 (null) ist, wird der Verbindungspool erst gelöscht, nachdem die `AppDomain` entladen und der Prozess beendet wurde. Für die Pflege inaktiver oder leerer Pools ist ein minimaler Systemmehraufwand erforderlich.

> [!NOTE]
> Der Pool wird bei schwerwiegenden Fehlern (z. B. einem Failover) automatisch gelöscht.

Im folgenden C#-Beispiel werden drei neue <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekte erstellt. Es sind aber nur zwei Verbindungspools erforderlich, um diese Objekte zu verwalten. Beachten Sie, dass die erste und zweite Verbindungszeichenfolge sich durch den Wert unterscheiden, der `Initial Catalog` zugewiesen ist.  


[!code-csharp[SqlConnection_Pooling#1](~/../sqlclient/doc/samples/SqlConnection_Pooling.cs#1)]

## <a name="add-connections"></a>Hinzufügen von Verbindungen

Für jede eindeutige Verbindungszeichenfolge wird ein Verbindungspool erstellt. Wenn ein Pool erstellt wird, werden mehrere Verbindungsobjekte erstellt und dem Pool hinzugefügt wird, sodass die minimalen Anforderungen für die Größe eines Pools erfüllt sind. Verbindungen werden dem Pool nach Bedarf bis zur angegebenen maximalen Poolgröße (**100 ist die Standardeinstellung**) hinzugefügt. Verbindungen werden wieder für den Pool freigegeben, wenn sie geschlossen oder verworfen werden.

Wenn ein <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekt angefordert wird, wird es aus dem Pool abgerufen, wenn eine verwendbare Verbindung verfügbar ist. Eine Verbindung ist dann verwendbar, wenn sie nicht verwendet wird, einen übereinstimmenden Transaktionskontext aufweist oder keinem Transaktionskontext zugeordnet ist sowie über eine gültige Verknüpfung zum Server verfügt.

Die Verbindungspoolfunktion erfüllt diese Verbindungsanforderungen, indem Verbindungen erneut zugewiesen werden, sobald sie wieder für den Pool freigegeben werden. Wenn die maximale Poolgröße erreicht ist und keine verwendbare Verbindung verfügbar ist, wird die Anforderung in die Warteschlange gestellt. Die Poolfunktion versucht dann, alle Verbindungen freizugeben, bis das Zeitlimit (**Standardwert: 15 Sekunden**) erreicht ist. Wenn der Pooler die Anforderung nicht erfüllt, bevor das Zeitlimit für die Verbindung überschritten ist, wird eine Ausnahme ausgelöst.

> [!CAUTION]
> Es ist unbedingt zu empfehlen, die Verbindung nach Verwendung stets zu schließen, damit sie in den Pool zurückgegeben wird. Verwenden Sie dazu die Methode `Close` oder `Dispose` des `Connection`-Objekts, oder öffnen Sie alle Verbindungen innerhalb einer `using`-Anweisung in C# oder innerhalb einer `Using`-Anweisung in Visual Basic. Verbindungen, die nicht explizit geschlossen werden, werden möglicherweise dem Pool nicht hinzugefügt bzw. nicht an den Pool zurückgegeben. Weitere Informationen finden Sie unter [using-Anweisung](/dotnet/csharp/language-reference/keywords/using-statement) oder [Gewusst wie:  Freigeben einer Systemressource](/dotnet/visual-basic/programming-guide/language-features/control-flow/how-to-dispose-of-a-system-resource) für Visual Basic.

> [!NOTE]
> Rufen Sie nicht `Close` oder `Dispose` für eine `Connection`, einen `DataReader` oder ein anderes verwaltetes Objekt in der `Finalize`-Methode der Klasse auf. Geben Sie in einer Finalize-Methode nur nicht verwaltete Ressourcen frei, die der Klasse direkt gehören. Wenn die Klasse keine nicht verwalteten Ressourcen besitzt, definieren Sie in der Klasse keine `Finalize`-Methode. Weitere Informationen finden Sie unter [Garbage Collection](/dotnet/standard/garbage-collection/index).

Weitere Informationen zu den Ereignissen im Zusammenhang mit dem Öffnen und Schließen von Verbindungen finden Sie in der SQL Server-Dokumentation unter [Audit Login (Ereignisklasse)](../../relational-databases/event-classes/audit-login-event-class.md) und [Audit Logout (Ereignisklasse)](../../relational-databases/event-classes/audit-logout-event-class.md).

## <a name="remove-connections"></a>Entfernen von Verbindungen

Die Verbindungspoolfunktion entfernt eine Verbindung aus dem Pool, nachdem sie für ca. **4-8** Minuten nicht verwendet oder festgestellt wurde, dass die Verbindung mit dem Server unterbrochen wurde.

> [!NOTE]
> Eine getrennte Verbindung kann erst erkannt werden, nachdem versucht wurde, mit dem Server zu kommunizieren. Wenn eine Verbindung gefunden wird, die nicht mehr mit dem Server verbunden ist, wird sie als ungültig markiert. Ungültige Verbindungen werden erst aus dem Verbindungspool entfernt, nachdem sie geschlossen oder freigegeben wurden.

Wenn eine Verbindung mit einem nicht mehr vorhandenen Server besteht, kann diese Verbindung aus dem Pool genommen werden, ohne dass die Verbindungspoolfunktion die unterbrochene Verbindung gefunden und als ungültig markiert hat. Dies liegt daran, dass durch den Mehraufwand beim Überprüfen, ob eine Verbindung noch gültig ist, die Vorteile eines Poolers umgangen werden, da eine weitere Schleife zum Server auftritt. In diesem Fall wird bei der ersten Verwendung der Verbindung festgestellt, dass die Verbindung unterbrochen wurde, und es wird eine Ausnahme ausgelöst.

## <a name="clear-the-pool"></a>Leeren des Pools

Der Microsoft SqlClient-Datenanbieter für SQL Server bietet nun zwei neue Methoden zum Leeren des Pools: <xref:Microsoft.Data.SqlClient.SqlConnection.ClearAllPools%2A> und <xref:Microsoft.Data.SqlClient.SqlConnection.ClearPool%2A>. `ClearAllPools` löscht die Verbindungspools für einen angegebenen Anbieter, und `ClearPool` löscht den Verbindungspool, der einer bestimmten Verbindung zugeordnet ist.

> [!NOTE]
> Wenn Verbindungen während des Aufrufs verwendet werden, werden diese entsprechend markiert. Sie werden nach dem Beenden verworfen und nicht an den Pool zurückgegeben.

## <a name="transaction-support"></a>Transaktionsunterstützung

Verbindungen werden aus dem Pool entnommen und basierend auf dem Transaktionskontext zugewiesen. Sofern `Enlist=false` in der Verbindungszeichenfolge angegeben ist, wird durch den Verbindungspool gewährleistet, dass die Verbindung im <xref:System.Transactions.Transaction.Current%2A>-Kontext eingetragen wird. Wenn eine Verbindung geschlossen und mit einer eingetragenen `System.Transactions`-Transaktion an den Pool zurückgegeben wird, wird sie so reserviert, dass bei der nächsten Anforderung für diesen Verbindungspool mit der gleichen `System.Transactions`-Transaktion die gleiche Verbindung zurückgegeben wird, soweit diese verfügbar ist. Wenn eine solche Anforderung ausgegeben wird und keine Verbindungen in einem Pool verfügbar sind, wird eine Verbindung vom nicht transaktiven Teil des Pools erstellt und eingetragen. Wenn in keinem Bereich des Pools Verbindungen verfügbar sind, wird eine neue Verbindung erstellt und eingetragen.

Wenn eine Verbindung geschlossen wird, wird sie an den Pool und an den entsprechenden Teilbereich auf der Grundlage des Transaktionskontexts zurückgegeben. Sie können die Verbindung daher trennen, ohne einen Fehler zu generieren, auch wenn eine verteilte Transaktion noch aussteht. So haben Sie die Möglichkeit, die verteilte Transaktion zu einem späteren Zeitpunkt durchzuführen oder abzubrechen.

## <a name="control-connection-pooling-with-connection-string-keywords"></a>Steuerung des Verbindungspoolings mit Schlüsselwörtern für die Verbindungszeichenfolge

Die `ConnectionString`-Eigenschaft des <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekts unterstützt Schlüssel-Wert-Paare für Verbindungszeichenfolgen, mit denen das Verhalten der Verbindungspoolinglogik angepasst werden kann. Weitere Informationen finden Sie unter <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="pool-fragmentation"></a>Poolfragmentierung

Poolfragmentierung ist ein Problem, das häufig bei Webanwendungen auftritt, bei denen die Anwendung eine große Anzahl von Pools erstellen kann, die erst nach der Beendigung des Prozesses freigegeben werden. Dabei bleibt eine große Anzahl von Verbindungen geöffnet. Dies benötigt viel Speicherplatz und verringert die Leistung.

### <a name="pool-fragmentation-due-to-integrated-security"></a>Poolfragmentierung aufgrund integrierter Sicherheit

Verbindungen werden entsprechend der Verbindungszeichenfolge und der Benutzeridentität zu Pools zusammengefasst. Daher erhalten Sie einen Pool pro Benutzer, wenn Sie für die Website die Standardauthentifizierung oder die Windows-Authentifizierung und eine Anmeldung mit integrierter Sicherheit verwenden. Obwohl dadurch die Leistungsfähigkeit nachfolgender Datenbankanforderungen für einen einzelnen Benutzer verbessert wird, kann der Benutzer von anderen Benutzern hergestellte Verbindungen nicht nutzen. Folglich wird pro Benutzer mindestens eine Verbindung mit dem Datenbankserver hergestellt. Dies ist ein Nebeneffekt einer bestimmten Webanwendungsarchitektur, den Entwickler gegen Sicherheits- und Überwachungsanforderungen abwägen müssen.

### <a name="pool-fragmentation-due-to-many-databases"></a>Poolfragmentierung aufgrund vieler Datenbanken

Viele Internetdienstanbieter hosten mehrere Websites auf einem einzigen Server. Sie verwenden u. U. eine einzige Datenbank, um eine Anmeldung mit Forms-Authentifizierung zu bestätigen, und stellen anschließend für diesen Benutzer oder diese Gruppe von Benutzern eine Verbindung mit einer bestimmten Datenbank her. Die Verbindung mit der Authentifizierungsdatenbank wird zu einem Pool zusammengefasst und kann von allen Benutzern verwendet werden. Es besteht jedoch für jede Datenbank ein eigener Verbindungspool, wodurch die Anzahl von Verbindungen mit dem Server erhöht wird.

Dies ist auch ein Nebeneffekt des Anwendungsentwurfs. Dieser Nebeneffekt kann relativ einfach vermieden werden, ohne dabei die Sicherheit beim Herstellen einer Verbindung mit SQL Server zu beeinträchtigen. Stellen Sie keine Verbindung mit einer separaten Datenbank für jeden Benutzer oder jede Gruppe her, sondern stellen Sie eine Verbindung mit derselben Datenbank auf dem Server her, und führen Sie anschließend die Transact-SQL-Anweisung USE aus, um zur gewünschten Datenbank zu wechseln.
 
Im folgenden Codefragment wird das Herstellen einer Anfangsverbindung mit der `master`-Datenbank und der anschließende Wechsel zur gewünschten Datenbank in der `databaseName`-Zeichenfolgenvariable veranschaulicht.

[!code-csharp[SqlConnection_Pooling_Use_Statement#1](~/../sqlclient/doc/samples/SqlConnection_Pooling_Use_Statement.cs#1)]

## <a name="application-roles-and-connection-pooling"></a>Anwendungsrollen und Verbindungspooling

Nachdem eine Anwendungsrolle von SQL Server durch Aufrufen der im System gespeicherten Prozedur `sp_setapprole` aktiviert wurde, kann der Sicherheitskontext dieser Verbindung nicht zurückgesetzt werden. Wenn jedoch das Verbindungspooling aktiviert ist, wird die Verbindung an den Verbindungspool zurückgegeben. Bei der erneuten Verwendung der an den Pool zurückgegebenen Verbindung wird dann ein Fehler ausgelöst.

### <a name="application-role-alternatives"></a>Alternativen zur Anwendungsrolle

Es wird empfohlen, die Sicherheitsmechanismen zu nutzen, die anstelle der Anwendungsrollen verwendet werden können.

## <a name="see-also"></a>Weitere Informationen

- [Verbindungspooling](connection-pooling.md)
- [SQL Server und ADO.NET](./sql/index.md)
- [Leistungsindikatoren in SqlClient](performance-counters.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)