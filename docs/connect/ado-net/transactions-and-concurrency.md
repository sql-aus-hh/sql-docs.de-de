---
title: Transaktionen und Parallelität
description: In diesem Artikel wird die Verwendung des SqlClient-Datenanbieters von Microsoft für SQL Server mit Transaktionen und Parallelität beschrieben.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a30a3d3184c411fc0b54c0e26330a8fb138ffdae
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051300"
---
# <a name="transactions-and-concurrency"></a>Transaktionen und Parallelität

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Eine Transaktion besteht aus einem einzelnen Befehl oder einer Gruppe von Befehlen, die als Paket ausgeführt werden. Mit Transaktionen können Sie mehrere Operationen in einem Arbeitsschritt zusammenfassen. Wenn an einer Stelle in der Transaktion ein Fehler auftritt, kann für alle Updates ein Rollback zum Zustand vor der Transaktion ausgeführt werden.

Eine Transaktion muss zur Gewährleistung der Datenkonsistenz die folgenden vier Kriterien, die so genannten ACID-Kriterien, erfüllen: Atomicity (Atomarität), Consistency (Konsistenz), Isolation und Durability (Dauerhaftigkeit). Die meisten relationalen Datenbanksysteme (z. B. Microsoft SQL Server) unterstützen Transaktionen, indem sie für Update-, Einfüge- oder Löschvorgänge, die durch eine Clientanwendung ausgeführt werden, entsprechende Sperr-, Protokollierungs- und Transaktionsverwaltungsfunktionen bereitstellen.

> [!NOTE]
> Transaktionen, an denen mehrere Ressourcen beteiligt sind, können die Parallelität senken, wenn Sperren zu lang gehalten werden. Halten Sie Transaktionen daher so kurz wie möglich.  

Wenn an einer Transaktion mehrere Tabellen in derselben Datenbank oder auf demselben Server beteiligt sind, bieten explizite Transaktionen in gespeicherten Prozeduren oft eine bessere Leistung. Transaktionen in gespeicherten SQL Server-Prozeduren können Sie mithilfe der Transact-SQL-Anweisungen `BEGIN TRANSACTION`, `COMMIT TRANSACTION` und `ROLLBACK TRANSACTION` erstellen. Weitere Informationen finden Sie in der SQL Server-Onlinedokumentation.

Transaktionen, an denen verschiedene Ressourcen-Manager beteiligt sind, z. B. Transaktionen zwischen SQL Server und Oracle, erfordern eine verteilte Transaktion.

## <a name="in-this-section"></a>In diesem Abschnitt

 [Lokale Transaktionen](local-transactions.md)  
 Zeigt, wie Transaktionen für eine Datenbank ausgeführt werden können.  
  
 [Verteilte Transaktionen](distributed-transactions.md)  
 Beschreibt die Ausführung von verteilten Transaktionen in ADO.NET.  
  
 [System.Transactions-Integration in SQL Server](system-transactions-integration-with-sql-server.md)  
 In diesem Artikel wird die <xref:System.Transactions>-Integration mit SQL Server für die Arbeit mit verteilten Transaktionen beschrieben.  
  
 Im Artikel [Optimistische Nebenläufigkeit](optimistic-concurrency.md) werden die optimistische und pessimistische Nebenläufigkeit und die Vorgehensweise zum Prüfen auf Verstöße gegen die Parallelität beschrieben.  

## <a name="see-also"></a>Weitere Informationen:

- [Grundlagen zu Transaktionen](/dotnet/framework/data/transactions/transaction-fundamentals)
- [Herstellen einer Verbindung mit einer Datenquelle](connecting-to-data-source.md)
- [Befehle und Parameter](commands-parameters.md)
- ["DataAdapters" und "DataReaders"](dataadapters-datareaders.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
