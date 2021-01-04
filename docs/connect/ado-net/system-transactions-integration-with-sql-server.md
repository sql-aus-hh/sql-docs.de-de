---
title: System.Transactions-Integration in SQL Server
description: In diesem Artikel wird die System.Transactions-Integration mit SQL Server für die Arbeit mit verteilten Transaktionen beschrieben.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: af0ba2865719a5388314a4ca695e09191cb56173
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051314"
---
# <a name="systemtransactions-integration-with-sql-server"></a>System.Transactions-Integration in SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET enthält ein neues Transaktionsframework, auf das über den <xref:System.Transactions>-Namespace zugegriffen werden kann. Dieses Framework stellt Transaktionen vollständig in .NET, einschließlich ADO.NET, integriert bereit.  
  
Neben den Verbesserungen in Bezug auf die Programmierbarkeit können <xref:System.Transactions> und ADO.NET auch zusammenarbeiten, um Optimierungen bei der Arbeit mit Transaktionen zu koordinieren. Eine heraufstufbare Transaktion ist eine kompakte (lokale) Transaktion, die automatisch bei Bedarf auf eine vollverteilte Transaktion höhergestuft werden kann.

Der Microsoft SqlClient-Datenanbieter für SQL Server unterstützt höherstufbare Transaktionen, wenn Sie mit SQL Server arbeiten. Eine heraufstufbare Transaktion ruft den zusätzlichen Aufwand einer verteilten Transaktion nur hervor, wenn dieser erforderlich ist. Höherstufbare Transaktionen sind automatisch und erfordern keinen Eingriff des Entwicklers.

## <a name="creating-promotable-transactions"></a>Erstellen von höherstufbaren Transaktionen

Der Microsoft SqlClient-Datenanbieter für SQL Server bietet Unterstützung für höherstufbare Transaktionen, die über die Klassen im <xref:System.Transactions>-Namespace verarbeitet werden. Heraufstufbare Transaktionen optimieren verteilte Transaktionen, indem sie das Erstellen einer verteilten Transaktion verzögern, bis diese benötigt wird. Wenn nur ein Ressourcen-Manager erforderlich ist, erfolgt keine verteilte Transaktion.

> [!NOTE]
> In einem teilweise vertrauenswürdigen Szenario wird die <xref:System.Transactions.DistributedTransactionPermission> benötigt, wenn eine Transaktion zu einer verteilten Transaktion heraufgestuft wird.

## <a name="promotable-transaction-scenarios"></a>Szenarios mit höherstufbaren Transaktionen

Heraufstufbare Transaktionen beanspruchen normalerweise erhebliche Systemressourcen und werden von MS DTC (Microsoft Distributed Transaction Coordinator) verwaltet, der alle Ressourcen-Manager integriert, auf die in der Transaktion zugegriffen wird. Eine höherstufbare Transaktion ist eine spezielle Art von <xref:System.Transactions>-Transaktion, die im Prinzip die Arbeit an eine einfache SQL Server-Transaktion delegiert. <xref:System.Transactions>, <xref:Microsoft.Data.SqlClient> und SQL Server koordinieren die für die Verarbeitung der Transaktion erforderliche Arbeit und stufen sie bei Bedarf auf eine vollständig verteilte Transaktion hoch.

Der Vorteil der Verwendung heraufstufbarer Transaktionen besteht darin, dass beim Öffnen einer Verbindung mit einer aktiven <xref:System.Transactions.TransactionScope> -Transaktion die Transaktion als kompakte Transaktion durchgeführt wird, wenn keine weiteren Verbindungen geöffnet sind und der zusätzliche Mehraufwand einer vollständig verteilten Transaktion vermieden werden kann.

### <a name="connection-string-keywords"></a>Schlüsselwörter für Verbindungszeichenfolgen

Die <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> -Eigenschaft unterstützt ein Schlüsselwort, `Enlist`, das angibt, ob der <xref:Microsoft.Data.SqlClient> Transaktionskontexte erkennt, und die Verbindung automatisch in einer verteilten Transaktion einträgt. Wenn `Enlist=true`, wird die Verbindung automatisch im aktuellen Transaktionskontext des öffnenden Threads eingetragen. Wenn `Enlist=false`, interagiert der `SqlClient` nicht mit einer verteilten Transaktion. Der Standardwert für `Enlist` ist "true". Wenn `Enlist` in der Verbindungszeichenfolge nicht angegeben ist, wird die Verbindung automatisch in einer verteilten Transaktion eingetragen, wenn eine solche zum Zeitpunkt des Öffnens der Verbindung erkannt wird.

Die `Transaction Binding` -Schlüsselwörter in einer <xref:Microsoft.Data.SqlClient.SqlConnection> -Verbindungszeichenfolge steuern die Zuordnung einer Verbindung mit einer eingetragenen `System.Transactions` -Transaktion. Diese ist auch verfügbar durch die <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> -Eigenschaft eines <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

In der folgenden Tabelle sind die möglichen Werte beschrieben.
  
|Stichwort|BESCHREIBUNG|  
|-------------|-----------------|  
|Implicit Unbind|Der Standardwert. Die Verbindung wird von der Transaktion getrennt, wenn diese endet, und wechselt damit zurück in den Autocommit-Modus.|
|Explicit Unbind|Die Verbindung bleibt so lange an die Transaktion gebunden, bis die Transaktion geschlossen wird. Die Verbindung schlägt fehl, wenn die zugehörige Transaktion nicht aktiv ist oder <xref:System.Transactions.Transaction.Current%2A>nicht entspricht.|

## <a name="using-transactionscope"></a>Verwenden von "TransactionScope"

Die <xref:System.Transactions.TransactionScope>-Klasse bewirkt, dass ein Codeblock transaktional wird, indem sie Verbindungen implizit in einer verteilten Transaktion einträgt. Sie müssen die <xref:System.Transactions.TransactionScope.Complete%2A> -Methode am Ende des <xref:System.Transactions.TransactionScope> -Blocks vor dem Verlassen aufrufen. Beim Verlassen des Blocks wird die <xref:System.Transactions.TransactionScope.Dispose%2A> -Methode aufgerufen. Wenn eine Ausnahme aufgerufen wurde, die den Code veranlasst, den Bereich zu verlassen, wird die Transaktion als abgebrochen angesehen.

Es wird empfohlen, einen `using` -Block zu verwenden, um sicherzustellen, dass <xref:System.Transactions.TransactionScope.Dispose%2A> für das <xref:System.Transactions.TransactionScope> -Objekt aufgerufen wird, wenn der verwendete Block beendet wird. Durch das Fehlschlagen von Commits oder Rollbacks ausstehender Transaktionen kann die Leistung entscheidend beeinträchtigt werden, da das Zeitlimit für <xref:System.Transactions.TransactionScope> eine Minute beträgt. Wenn Sie keine `using`-Anweisung verwenden, müssen Sie alle Arbeiten in einem `Try`-Block ausführen und die <xref:System.Transactions.TransactionScope.Dispose%2A>-Methode im `Finally`-Block explizit aufrufen.

Wenn innerhalb des <xref:System.Transactions.TransactionScope>eine Ausnahme auftritt, wird die Transaktion als inkonsistent markiert und abgebrochen. Sie wird zurückgesetzt, wenn der <xref:System.Transactions.TransactionScope> verworfen wird. Wenn keine Ausnahme auftritt, wird für teilnehmende Transaktionen ein Commit ausgeführt.

> [!NOTE]
> Die `TransactionScope`-Klasse erstellt <xref:System.Transactions.Transaction.IsolationLevel%2A> standardmäßig mit dem Wert `Serializable`. Je nach Ihrer Anwendung kann es vorteilhaft sein, die Isolationsstufe herabzusetzen, um ein hohes Konfliktpotenzial in der Anwendung zu vermeiden.

> [!NOTE]
> Es wird empfohlen, dass Sie nur Update-, Einfüge- und Löschvorgänge in verteilten Transaktionen durchführen, da diese erhebliche Datenbankressorcen beanspruchen. Select-Anweisungen können Datenbankressourcen unnötigerweise blockieren, und in einigen Szenarien kann es erforderlich sein, Transaktionen für Select-Vorgänge zu verwenden. Arbeiten, die nicht mit der Datenbank zusammenhängen, sollten außerhalb des Bereichs der Transaktion durchgeführt werden, außer wenn andere transaktive Ressourcen-Manager verwendet werden.
Obwohl eine Ausnahme innerhalb des Bereichs der Transaktion dazu führt, dass die Transaktion keinen Commit ausführt, verfügt die <xref:System.Transactions.TransactionScope> -Klasse über keine Funktion zum Zurücksetzen von Änderungen, die vom Code außerhalb des Bereichs der Transaktion selbst durchgeführt wurden. Wenn Sie beim Zurücksetzen der Transaktion Maßnahmen ergreifen müssen, müssen Sie Ihre eigene Implementierung der <xref:System.Transactions.IEnlistmentNotification> -Schnittstelle schreiben und in der Transaktion explizit eintragen.

## <a name="example"></a>Beispiel

Das Arbeiten mit <xref:System.Transactions> setzt einen Verweis auf "System.Transactions.dll" voraus.

Die folgende Funktion zeigt das Erstellen einer heraufstufbaren Transaktion für zwei verschiedene SQL Server-Instanzen, die von zwei verschiedenen <xref:Microsoft.Data.SqlClient.SqlConnection> -Objekten dargestellt werden, die ihrerseits von einem <xref:System.Transactions.TransactionScope> -Block umschlossen sind.

Mit dem Code unten wird der <xref:System.Transactions.TransactionScope>-Block mit einer `using`-Anweisung erstellt und die erste Verbindung geöffnet, wodurch die Transaktion automatisch in <xref:System.Transactions.TransactionScope> eingetragen wird.

Die Transaktion wird anfänglich als einfache Transaktion, nicht als vollständig verteilte Transaktion, eingetragen. Die zweite Verbindung wird nur dann im <xref:System.Transactions.TransactionScope> eingetragen, wenn der Befehl in der ersten Verbindung keine Ausnahme auslöst. Wenn die zweite Verbindung geöffnet wird, wird die Transaktion automatisch auf eine vollständig verteilte Transaktion hochgestuft.

Später wird die <xref:System.Transactions.TransactionScope.Complete%2A>-Methode aufgerufen, die die Transaktion nur dann committet, wenn keine Ausnahmen ausgelöst wurden. Wenn an einem beliebigen Punkt im <xref:System.Transactions.TransactionScope> -Block eine Ausnahme ausgelöst wurde, wird `Complete` nicht aufgerufen, und die verteilte Transaktion wird zurückgenommen, sobald der <xref:System.Transactions.TransactionScope> am Ende seines `using` -Blocks verfügbar gemacht wird.

[!code-csharp[SqlTransactionScope#1](~/../sqlclient/doc/samples/SqlTransactionScope.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Transaktionen und Parallelität](transactions-and-concurrency.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
