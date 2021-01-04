---
title: Verteilte Transaktionen
description: In diesem Artikel wird die Durchführung verteilter Transaktionen in ADO.NET beschrieben.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 77ed8486f8b059f12fe7178826d81a743a97bbc4
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559232"
---
# <a name="distributed-transactions"></a>Verteilte Transaktionen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Bei einer Transaktion handelt es sich u. a. um mehrere zusammenhängende Aufgaben, die entweder in ihrer Gesamtheit erfolgreich abgeschlossen werden (Commit) oder nicht (Abort). Bei einer *verteilten Transaktion* handelt es sich um eine Transaktion, die sich auf verschiedene Ressourcen auswirkt. Damit ein Commit einer verteilten Transaktion erfolgreich ausgeführt werden kann, müssen alle Beteiligten gewährleisten, dass jede vorgenommene Datenänderung dauerhaft ist. Änderungen müssen auch im Fall von Systemausfällen oder anderen unvorhergesehenen Ereignissen dauerhaft sein. Sobald auch nur ein Beteiligter dies nicht gewährleisten kann, kann die gesamte Transaktion nicht ausgeführt werden, und sämtliche im Zuge der Transaktion durchgeführten Datenänderungen werden in einem Rollback zurückgenommen. 

> [!NOTE]
> Wenn Sie versuchen, einen Commit oder Rollback für eine Transaktion auszuführen, wenn ein `DataReader` gestartet wird und die Transaktion aktiv ist, wird eine Ausnahme ausgelöst.

## <a name="working-with-systemtransactions"></a>Arbeiten mit System.Transactions

In .NET werden verteilte Transaktionen über die API im <xref:System.Transactions>-Namespace verwaltet. Die <xref:System.Transactions>-API delegiert die Behandlung verteilter Transaktionen an einen Transaktionsmonitor wie den MS DTC (Microsoft Distributed Transaction Coordinator), wenn mehrere ständige Ressourcen-Manager beteiligt sind. Weitere Informationen finden Sie unter [Grundlagen zu Transaktionen](/dotnet/framework/data/transactions/transaction-fundamentals).

Die Unterstützung der Auflistung in einer verteilten Transaktion mithilfe der `EnlistTransaction`-Methode, die eine Verbindung in einer <xref:System.Transactions.Transaction>-Instanz auflistet, ist eine neue Funktion in ADO.NET 2.0. In früheren Versionen von ADO.NET wurde die explizite Eintragung in verteilte Transaktionen mithilfe der `EnlistDistributedTransaction`-Methode einer Verbindung für das Eintragen einer Verbindung in eine <xref:System.EnterpriseServices.ITransaction>-Instanz durchgeführt, die aus Gründen der Abwärtskompatibilität unterstützt wird. Weitere Informationen zu Enterprise Services-Transaktionen finden Sie unter [Interoperabilität mit Enterprise Services- und COM+-Transaktionen](/dotnet/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions).

Bei <xref:System.Transactions>-Transaktionen mit dem Microsoft SqlClient-Datenanbieter für SQL Server für eine SQL Server-Datenbank wird automatisch eine einfache <xref:System.Transactions.Transaction>-Klasse verwendet. Die Transaktion kann anschließend bei Bedarf zu einer vollständig verteilten Transaktion heraufgestuft werden. Weitere Informationen finden Sie unter [System.Transactions-Integration mit SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="automatically-enlisting-in-a-distributed-transaction"></a>Automatisches Eintragen in eine verteilte Transaktion

Die automatische Eintragung ist die Standard- (und bevorzugte) Methode zum Integrieren von ADO.NET-Verbindungen in die `System.Transactions`. Ein Verbindungsobjekt trägt sich automatisch in eine vorhandene verteilte Transaktion ein, wenn es feststellt, dass eine Transaktion aktiv ist. Bei `System.Transaction` bedeutet dies, dass `Transaction.Current` nicht NULL ist. Die automatische Transaktionseintragung erfolgt beim Öffnen der Verbindung. Dies erfolgt nicht anschließend, auch wenn ein Befehl innerhalb des Gültigkeitsbereichs einer Transaktion ausgeführt wird. Sie können die automatische Eintragung in vorhandene Transaktionen deaktivieren, indem Sie `Enlist=false` als Verbindungszeichenfolgen-Parameter für die <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>-Eigenschaft angeben.

## <a name="manually-enlisting-in-a-distributed-transaction"></a>Manuelles Eintragen in eine verteilte Transaktion

Wenn die automatische Eintragung deaktiviert ist oder Sie eine Transaktion eintragen müssen, die nach dem Öffnen der Verbindung gestartet wurde, können Sie sie in eine vorhandene verteilte Transaktion eintragen, indem Sie die `EnlistTransaction`-Methode des <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekts für den Microsoft SqlClient-Datenanbieter für SQL Server verwenden. Durch das Eintragen in eine vorhandene verteilte Transaktion wird sichergestellt, dass bei einem Commit oder Rollback der Transaktion auch die Änderungen einbezogen werden, die vom Code an der Datenquelle vorgenommen wurden.

Das Eintragen in verteilte Transaktionen eignet sich besonders beim Pooling von Geschäftsobjekten. Wenn sich ein Geschäftsobjekt mit einer hergestellten Verbindung in einem Pool befindet, erfolgt die automatische Transaktionseintragung nur, wenn diese Verbindung hergestellt wird. Wenn mehrere Transaktionen unter Verwendung des Geschäftsobjekts im Pool ausgeführt werden, wird die für das Objekt hergestellte Verbindung nicht automatisch in neu initialisierte Transaktionen eingetragen. In diesem Fall können Sie die automatische Transaktionseintragung für die Verbindung deaktivieren und die Verbindung mithilfe der `EnlistTransaction`-Methode in Transaktionen eintragen.

`EnlistTransaction` akzeptiert nur ein Argument vom Typ <xref:System.Transactions.Transaction>, das einen Verweis auf die vorhandene Transaktion darstellt. Nach dem Aufrufen der `EnlistTransaction`-Methode der Verbindung werden alle Änderungen, die mit der Verbindung an der Datenquelle vorgenommen wurden, in die Transaktion eingetragen. Beim Übergeben eines NULL-Werts wird die Verbindung aus der aktuellen verteilten Transaktionseintragung ausgetragen. Beachten Sie, dass die Verbindung vor dem Aufrufen der `EnlistTransaction`-Methode hergestellt werden muss.

> [!NOTE]
> Nachdem eine Verbindung ausdrücklich in eine Transaktion eingetragen wurde, kann sie solange nicht ausgetragen oder in eine andere Transaktion eingetragen werden, bis die erste Transaktion beendet wurde.

> [!CAUTION]
> Die `EnlistTransaction`-Methode löst eine Ausnahme aus, wenn die Verbindung bereits mithilfe ihrer <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>-Methode eine Transaktion begonnen hat. Wenn es sich bei der Transaktion jedoch um eine lokale Transaktion handelt, die an der Datenquelle begonnen wurde (z. B. die explizite Ausführung der BEGIN TRANSACTION-ANWEISUNG mithilfe eines <xref:Microsoft.Data.SqlClient.SqlCommand>), führt die `EnlistTransaction`-Methode einen Rollback der lokalen Transaktion durch und trägt sich selbst wie angefordert in die vorhandene verteilte Transaktion ein. Sie werden nicht über den Rollback der lokalen Transaktion benachrichtigt und müssen nicht begonnene lokale Transaktionen mithilfe der <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>-Methode verwalten. Wenn Sie den Microsoft SqlClient-Datenanbieter für SQL Server zusammen mit SQL Server verwenden, wird bei einem Eintragsversuch eine Ausnahme ausgelöst. Alle anderen Fälle bleiben unerkannt.  

## <a name="promotable-transactions-in-sql-server"></a>Höherstufbare Transaktionen in SQL Server

SQL Server unterstützt heraufstufbare Transaktionen, bei denen eine lokale kompakte Transaktion nur bei Bedarf automatisch zu einer verteilten Transaktion heraufgestuft werden kann. Eine heraufstufbare Transaktion ruft den zusätzlichen Aufwand einer verteilten Transaktion nur hervor, wenn dieser erforderlich ist. Weitere Informationen und ein Codebeispiel finden Sie unter [System.Transactions-Integration mit SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="configuring-distributed-transactions"></a>Konfigurieren von verteilten Transaktionen

 Möglicherweise müssen Sie MS DTC über das Netzwerk aktivieren, um verteilte Transaktionen zu verwenden. Wenn die Windows-Firewall aktiviert ist, müssen Sie dem MS DTC-Dienst die Erlaubnis erteilen, das Netzwerk zu verwenden, oder den MS DTC-Port öffnen.  
  
## <a name="see-also"></a>Weitere Informationen:

- [Transaktionen und Parallelität](transactions-and-concurrency.md)
- [System.Transactions-Integration in SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
