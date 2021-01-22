---
description: Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves
title: Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 4799cd725dce4eb8300717b8c89d601e9915f7d2
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534829"
---
# <a name="create-and-use-indexes-on-columns-using-always-encrypted-with-secure-enclaves"></a>Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In diesem Artikel wird beschrieben, wie Indizes für Spalten erstellt und verwendet werden, die mit Enclave-fähigen Spaltenverschlüsselungsschlüsseln mit [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) verschlüsselt wurden.

Always Encrypted mit Secure Enclaves unterstützt Folgendes:
- Gruppierte und nicht gruppierte Indizes für Spalten, die mit deterministischer Verschlüsselung und Enclave-fähigen Schlüsseln verschlüsselt wurden.
  - Indizes dieser Art werden anhand des Chiffretexts sortiert. Bei Indizes dieser Art müssen keine besonderen Aspekte berücksichtigt werden. Sie können sie auf die gleiche Weise verwalten und verwenden wie Indizes für Spalten, die mit deterministischer Verschlüsselung und nicht Enclave-fähigen Schlüsseln (wie bei Always Encrypted) verschlüsselt wurden. 
- Nicht gruppierte Indizes für Spalten, die mit einer Verschlüsselung nach Zufallsprinzip und Enclave-fähigen Schlüsseln verschlüsselt wurden.
  - Die Schlüsselwerte in der Indexdatenstruktur (B-Struktur) werden anhand ihrer Klartextwerte verschlüsselt und sortiert. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung](always-encrypted-enclaves.md#indexes-on-enclave-enabled-columns).

> [!NOTE]
> In den übrigen Abschnitten dieses Artikels werden nicht gruppierte Indizes für Spalten beschrieben, die mit einer Verschlüsselung nach Zufallsprinzip und Enclave-fähigen Schlüsseln verschlüsselt wurden.

Da ein Index in einer Spalte mit zufälliger Verschlüsselung und einem Enclave-fähigen Spaltenverschlüsselungsschlüssel verschlüsselte (Chiffretext) Daten enthält, die nach Klartext sortiert werden, muss die SQL Server-Engine die Enclave für jeden Vorgang verwenden, bei dem ein Index erstellt, aktualisiert oder durchsucht wird. Hierzu zählen folgende Vorgänge:

- Erstellen oder Neuerstellen eines Index.
- Einfügen, Aktualisieren oder Löschen einer Zeile in einer Tabelle (die eine indizierte/verschlüsselte Spalte enthält), wodurch das Einfügen und/oder Entfernen eines Indexschlüssels in den/aus dem Index ausgelöst wird.
- Ausführen von `DBCC`-Befehlen, bei denen eine Integritätsprüfung der Indizes durchgeführt wird, z. B. [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) oder [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).
- Datenbankwiederherstellung (z. B. nach einem Ausfall von SQL Server und Neustarts), wenn SQL Server Änderungen am Index rückgängig machen muss (weitere Details finden Sie weiter unten).

Bei allen oben genannten Vorgängen benötigt die Enclave den Spaltenverschlüsselungsschlüssel für die indizierte Spalte. Der Schlüssel wird zum Entschlüsseln der Indexschlüssel benötigt. Im Allgemeinen kann die Enclave einen Spaltenverschlüsselungsschlüssel auf zwei Arten abrufen:
- Direkt über die Clientanwendung.
- Aus dem Cache des Spaltenverschlüsselungsschlüssels.

## <a name="invoke-indexing-operations-with-column-encryption-keys-provided-directly-by-the-client"></a>Aufrufen von Indizierungsvorgänge mit direkt vom Client bereitgestellten Spaltenverschlüsselungsschlüsseln
Damit diese Methode zum Aufrufen von Indizierungsvorgängen funktioniert, muss die Anwendung (wie etwa ein Tool wie SQL Server Management Studio (SSMS)), die eine Abfrage ausgibt, die einen Vorgang auf einem Index auslöst, folgende Vorgaben erfüllen:

- Sie muss eine Verbindung zur Datenbank mit Always Encrypted und Enclave-Berechnungen herstellen, die für die Datenbankverbindung aktiviert sind.
- Die Anwendung muss Zugriff auf den Spaltenhauptschlüssel haben, der den Spaltenverschlüsselungsschlüssel für die indizierte Spalte schützt.

Sobald die SQL Server-Engine die Anwendungsabfrage analysiert und feststellt, dass ein Index in einer verschlüsselten Spalte aktualisiert werden muss, um die Abfrage auszuführen, weist sie den Clienttreiber an, der Enclave den erforderlichen Spaltenverschlüsselungsschlüssel über einen sicheren Kanal bereitzustellen. Dies ist genau derselbe Mechanismus wie der, der zum Bereitstellen des Spaltenverschlüsselungsschlüssels für die Enclave zur Verarbeitung von anderen Abfragen verwendet wird, die keinen Indizes verwenden. Beispiele: direkte Verschlüsselung oder Abfragen, die einen Musterabgleich und Bereichsvergleiche verwenden.

Diese Methode ist sinnvoll, um sicherzustellen, dass das Vorhandensein von Indizes in verschlüsselten Spalten für Anwendungen, die bereits mit der Datenbank verbunden sind, transparent ist, wobei Always Encrypted- und Enclave-Berechnungen aktiviert sind. Die Anwendungsverbindung kann die Enclave für die Abfrageverarbeitung verwenden. Nachdem Sie einen Index für eine Spalte erstellt haben, stellt der Treiber in Ihrer Anwendung der Enclave transparent Spaltenverschlüsselungscodes für Indizierungsvorgänge zur Verfügung. Das Erstellen von Indizes kann die Anzahl der Abfragen erhöhen, bei denen die Anwendung die Spaltenverschlüsselungsschlüssel an die Enclave senden muss.

Wenn Sie diese Methode verwenden möchten, befolgen Sie die allgemeine Anleitung zum Ausführen von Anweisungen mithilfe einer Secure Enclave unter [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md).

Schritt-für-Schritt-Anweisungen zur Verwendung dieser Methode finden Sie im [Tutorial: Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).

## <a name="invoke-indexing-operations-using-cached-column-encryption-keys"></a>Aufrufen der Indizierungsvorgänge mit zwischengespeicherten Spaltenverschlüsselungsschlüsseln

Sobald eine Clientanwendung einen Spaltenverschlüsselungsschlüssel zur Verarbeitung aller Abfragen, die Enclave-Berechnungen erfordern, an die Enclave sendet, speichert die Enclave den Spaltenverschlüsselungsschlüssel in einem internen Cache. Dieser Cache befindet sich innerhalb der Enclave und ist von außen nicht zugänglich.

Wenn die gleiche oder eine andere Clientanwendung, die von demselben oder einem anderen Benutzer verwendet wird, einen Vorgang für einen Index auslöst, ohne die erforderliche Spaltenverschlüsselung direkt bereitzustellen, sucht die Enclave im Cache nach dem Spaltenverschlüsselungsschlüssel. Infolgedessen ist der Vorgang auf dem Index erfolgreich, obwohl die Clientanwendung den Schlüssel nicht bereitstellt hat.

Damit diese Methode zum Aufrufen von Indizierungsvorgängen funktioniert, muss die Anwendung eine Verbindung zur Datenbank herstellen, ohne dass für die Verbindung Always Encrypted aktiviert ist, und der erforderliche Spaltenverschlüsselungsschlüssel muss im Cache innerhalb der Enclave vorhanden sein.

Diese Methode zum Aufrufen von Vorgängen wird nur für Abfragen unterstützt, die keine Spaltenverschlüsselungsschlüssel für andere Vorgänge benötigen, die sich nicht auf Indizes beziehen. Beispielsweise muss eine Anwendung, die eine Zeile mit einer `INSERT`-Anweisung in eine Tabelle einfügt, die eine verschlüsselte Spalte enthält, eine Verbindung zur Datenbank herstellen, wobei Always Encrypted in der Verbindungszeichenfolge aktiviert ist und sie Zugriff auf die Schlüssel haben muss, unabhängig davon, ob die verschlüsselte Spalte einen Index aufweist oder nicht.

Diese Methode ist in folgenden Fällen sinnvoll:
 - Sie können sicherstellen, dass das Vorhandensein von Indizes in Enclave-fähigen Spalten mit zufälliger Verschlüsselung für Anwendungen und Benutzer, die keinen Zugriff auf die Schlüssel und Daten im Klartext haben, transparent ist. 
 - Dadurch wird sichergestellt, dass die Erstellung eines Indexes für eine verschlüsselte Spalte keine vorhandene Abfragen blockiert. Wenn eine Anwendung eine Abfrage für eine Tabelle mit verschlüsselten Spalten ausgibt, ohne auf die Schlüssel zugreifen zu müssen, kann die Anwendung ohne Zugriff auf die Schlüssel weiter ausgeführt werden, nachdem ein DBA einen Index erstellt hat. Stellen Sie sich beispielsweise eine Anwendung vor, die die folgende Abfrage für die im vorherigen Beispiel verwendete Tabelle **Employees** ausführt, die verschlüsselte Spalten enthält. Der DBA hat für keine verschlüsselte Spalte einen Index erstellt.

   ```sql
   DELETE FROM [dbo].[Employees] WHERE [EmployeeID] = 1;
   GO
   ```

   Wenn die Anwendung die Abfrage über eine Verbindung ohne aktivierte Always Encrypted- und Enclave-Berechnungen sendet, wird die Abfrage erfolgreich durchgeführt. Die Abfrage löst keine Berechnungen für verschlüsselte Spalten aus. Nachdem ein DBA einen Index für alle verschlüsselten Spalten erstellt hat, löst die Abfrage das Entfernen von Indexschlüsseln aus Indizes aus. Die Enclave benötigt in dieser Situation die Spaltenverschlüsselungsschlüssel. Die Anwendung kann diese Abfrage jedoch weiterhin über dieselbe Verbindung ausführen, sofern ein Datenbesitzer die Spaltenverschlüsselungsschlüssel an die Enclave weitergegeben hat.

 - Sie können damit bei der Verwaltung von Indizes eine Rollentrennung realisieren, da sie es DBAs ermöglicht, Indizes auf verschlüsselten Spalten zu erstellen und zu ändern, ohne Zugriff auf sensible Daten zu haben. 

> [!TIP] 
> Mit [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md) können Sie problemlos alle Enclave-fähigen Spaltenverschlüsselungsschlüssel senden, die für Indizes für die Enclave verwendet werden, und den Schlüsselcache auffüllen.

Schritt-für-Schritt-Anweisungen zur Verwendung dieser Methode finden Sie im [Tutorial: Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md). 

## <a name="next-steps"></a>Nächste Schritte
- [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md)

## <a name="see-also"></a>Weitere Informationen  
- [Tutorial: Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).
- [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md)
