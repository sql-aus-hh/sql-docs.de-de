---
title: Lokale Transaktionen
description: In diesem Artikel wird die Durchführung von Transaktionen für eine Datenbank mit dem Microsoft SqlClient-Datenanbieter für SQL Server beschrieben.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a310ab409c2300eb31d1e3c0e58b7ebe32096bfd
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051305"
---
# <a name="local-transactions"></a>Lokale Transaktionen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Transaktionen werden in ADO.NET verwendet, wenn mehrere Aufgaben miteinander verbunden werden sollen, damit sie als eine einzige Verarbeitungseinheit ausgeführt werden können. Stellen Sie sich z.&#160;B. vor, dass eine Anwendung zwei Aufgaben ausführt. Zum einen aktualisiert sie eine Tabelle mit Bestellinformationen. Zum anderen aktualisiert sie eine Tabelle mit Bestandsinformationen und bucht die bestellten Artikel ab. Wenn bei einem der Tasks ein Fehler auftritt, wird für beide Updates ein Rollback ausgeführt.  

## <a name="determining-the-transaction-type"></a>Ermitteln des Transaktionstyps

Eine Transaktion wird als lokale Transaktion angesehen, wenn sie über nur eine Phase verfügt und direkt von der Datenbank verarbeitet wird. Als verteilte Transaktion wird eine Transaktion angesehen, wenn sie von einem Transaktionsmonitor koordiniert wird und für die Transaktionsauflösung ausfallsichere Mechanismen verwendet werden (z. B. Zweiphasencommit).

Der Microsoft SqlClient-Datenanbieter für SQL Server verfügt über ein eigenes <xref:Microsoft.Data.SqlClient.SqlTransaction>-Objekt zum Durchführen von lokalen Transaktionen in SQL Server-Datenbanken. Andere .NET-Datenanbieter stellen ebenfalls eigene `Transaction`-Objekte bereit. Darüber hinaus gibt es eine <xref:System.Data.Common.DbTransaction>-Klasse zum Schreiben von anbieterunabhängigem Code, der Transaktionen erfordert.

> [!NOTE]
> Transaktionen sind am effizientesten, wenn sie auf dem Server durchgeführt werden. Wenn Sie mit einer SQL Server-Datenbank arbeiten, die häufig explizite Transaktionen verwendet, empfiehlt es sich, die Transaktionen unter Verwendung der BEGIN TRANSACTION-Anweisung von Transact-SQL als gespeicherte Prozeduren zu schreiben.

## <a name="performing-a-transaction-using-a-single-connection"></a>Durchführen einer Transaktion mit einer einzelnen Verbindung 

In ADO.NET können Sie Transaktionen mit dem `Connection`-Objekt steuern. Zum Initiieren einer lokalen Transaktion steht Ihnen die `BeginTransaction`-Methode zur Verfügung. Nachdem Sie eine Transaktion gestartet haben, können Sie mit der `Transaction`-Eigenschaft eines `Command`-Objekts einen Befehl in dieser Transaktion eintragen. Dann können Sie für Änderungen an der Datenquelle auf Grundlage des Erfolgs oder Fehlschlags der Komponenten der Transaktion einen Commit oder Rollback ausführen.

> [!NOTE]
> Die `EnlistDistributedTransaction`-Methode darf nicht für eine lokale Transaktion verwendet werden.

Der Gültigkeitsbereich der Transaktion ist auf die Verbindung beschränkt. Im folgenden Beispiel wird eine explizite Transaktion ausgeführt, die aus zwei separaten Befehlen im `try`-Block besteht. Die Befehle führen INSERT-Anweisungen für die `Production.ScrapReason`-Tabelle in der AdventureWorks-Beispieldatenbank für SQL Server aus. Ein Commit wird hierfür nur ausgeführt, wenn keine Ausnahmen ausgelöst werden. Der Code im `catch`-Block führt ein Rollback der Transaktion aus, wenn eine Ausnahme ausgelöst wird. Wenn die Transaktion abgebrochen oder die Verbindung geschlossen wird, bevor die Transaktion beendet wurde, wird automatisch ein Rollback für die Transaktion ausgeführt.

## <a name="example"></a>Beispiel  

 Führen Sie diese Schritte aus, um eine Transaktion auszuführen:

1. Rufen Sie die <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>-Methode des <xref:Microsoft.Data.SqlClient.SqlConnection>-Objekts auf, um den Start der Transaktion festzulegen. Die <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>-Methode gibt einen Verweis auf die Transaktion zurück. Dieser Verweis wird den <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekten zugeordnet, die in der Transaktion eingetragen sind.

2. Weisen Sie das `Transaction`-Objekt der <xref:Microsoft.Data.SqlClient.SqlCommand.Transaction%2A>-Eigenschaft des auszuführenden <xref:Microsoft.Data.SqlClient.SqlCommand> zu. Wenn ein Befehl für eine Verbindung mit einer aktiven Transaktion ausgeführt wird und das `Transaction`-Objekt nicht der `Transaction`-Eigenschaft des `Command`-Objekts zugewiesen wurde, wird eine Ausnahme ausgelöst.

3. Führen Sie die erforderlichen Befehle aus.

4. Rufen Sie die <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A>-Methode des <xref:Microsoft.Data.SqlClient.SqlTransaction>-Objekts auf, um die Transaktion abzuschließen, oder rufen Sie die <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A>-Methode auf, um die Transaktion zu beenden. Wenn die Verbindung vor dem Ausführen der <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A>-Methode oder der <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A>-Methode geschlossen oder verworfen wurde, wird für die Transaktion ein Rollback ausgeführt.

Im folgenden Codebeispiel wird die Transaktionslogik unter Verwendung des Microsoft SqlClient-Datenanbieters für SQL Server veranschaulicht.  

[!code-csharp[SqlTransactionLocal#1](~/../sqlclient/doc/samples/SqlTransactionLocal.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Transaktionen und Parallelität](transactions-and-concurrency.md)
- [Verteilte Transaktionen](distributed-transactions.md)
- [System.Transactions-Integration in SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
