---
description: Aktivieren von Always Encrypted mit Secure Enclaves für vorhandene verschlüsselte Spalten
title: Aktivieren von Always Encrypted mit Secure Enclaves für vorhandene verschlüsselte Spalten | Microsoft-Dokumentation
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
ms.openlocfilehash: 03a57949624897b2cd2e0552892803c21a408b52
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534809"
---
# <a name="enable-always-encrypted-with-secure-enclaves-for-existing-encrypted-columns"></a>Aktivieren von Always Encrypted mit Secure Enclaves für vorhandene verschlüsselte Spalten 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

In diesem Artikel wird beschrieben, wie Sie die Funktionalität für Always Encrypted mit Secure Enclaves entsperren und Enclave-Berechnungen für vorhandene verschlüsselte Spalten aktivieren.  

Wenn Sie über Spalten verfügen, die mit nicht Enclave-fähigen Schlüsseln verschlüsselt wurden, können Sie die Spalten mit Enclave-fähigen Schlüsseln verschlüsseln. Dies ermöglicht Ihnen die Verwenden von Secure Enclaves in Abfragen für Ihre Spalten.

Sie können Enclave-Berechnungen für vorhandene verschlüsselte Spalten abhängig von den folgenden Optionen auf verschiedene Weisen aktivieren:

- **Bereich/Granularität:** Möchten Sie die Enclave-Funktionalität für eine Teilmenge von Spalten oder für alle mit einem bestimmten Spaltenhauptschlüssel geschützten Spalten aktivieren?
- **Datengröße:** Wie groß sind die Tabellen mit den Spalten, die Sie für die Enclave aktivieren möchten?
- Möchten Sie auch den Verschlüsselungstyp für Ihre Spalte(n) ändern? Denken Sie daran, dass nur die Verschlüsselung nach dem Zufallsprinzip umfangreiche Berechnungen (Musterabgleich, Vergleichsoperatoren) unterstützt. Wenn Ihre Spalte mit deterministischer Verschlüsselung verschlüsselt ist, müssen Sie sie auch mit der Verschlüsselung nach dem Zufallsprinzip neu verschlüsseln, um umfassende Berechnungen zu ermöglichen.

In den folgenden Abschnitten werden die drei Ansätze zum Aktivieren von Enclaves für vorhandene Spalten beschrieben.

## <a name="method-1-rotate-the-column-master-key-to-replace-it-with-an-enclave-enabled-column-master-key"></a>Methode 1: Rotieren des Spaltenhauptschlüssels zum Ersetzen durch einen Enclave-fähigen Spaltenhauptschlüssel
Durch das Ersetzen eines vorhandenen Spaltenhauptschlüssels (der nicht Enclave-fähig ist) durch einen neuen, Enclave-fähigen Spaltenhauptschlüssels werden jegliche Spaltenverschlüsselungsschlüssel (die dem Spaltenhauptschlüssel zugeordnet sind) ebenfalls Enclave-fähig.

- Vorteile:
  - Erfordert keine erneute Verschlüsselung von Daten. Daher ist dies in der Regel der schnellste Ansatz. Dieser Ansatz wird bei Spalten mit großen Datenmengen empfohlen, die bereits für umfangreiche Berechnungen verwendet werden können und die deterministische Verschlüsselung nutzen.
  - Kann die Enclave-Funktionalität für mehrere Spalten skalierbar aktivieren. Durch das Ersetzen des Spaltenhauptschlüssels durch den Enclave-fähigen Spaltenhauptschlüssel werden alle Spaltenverschlüsselungsschlüssel und alle dem ursprünglichen Spaltenhauptschlüssel zugeordneten verschlüsselten Spalten Enclave-fähig.
  
- Nachteile:
  - Das Ändern der Verschlüsselungstyps von einer deterministischen zu einer zufälligen Verschlüsselung wird nicht unterstützt. Obwohl die direkte Verschlüsselung für Spalten ermöglicht wird, die mit deterministischer Verschlüsselung verschlüsselt sind, werden umfangreiche Berechnungen nicht ermöglicht. Sie müssen die Spalten erneut mit der zufälligen Verschlüsselung verschlüsseln.
  - Sie können nicht einige der Spalten selektiv konvertieren, die einem bestimmten Spaltenhauptschlüssel zugeordnet sind.
  - Es kommt zu überflüssigem Schlüsselverwaltungsaufwand. Sie müssen einen neuen Spaltenhauptschlüssel erstellen und für Anwendungen zur Verfügung stellen, die die betroffenen Spalten abfragen.

Informationen zum Rotieren eines Spaltenhauptschlüssels finden Sie unter [Rotieren Enclave-fähiger Schlüssel](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-2-rotate-the-column-master-key-and-re-encrypt-columns-using-randomized-encryption-in-place"></a>Methode 2: Rotieren des Spaltenhauptschlüssels und erneutes Verschlüsseln von Spalten mithilfe direkter Verschlüsselung nach dem Zufallsprinzip
Diese Methode umfasst die Ausführung von Methode 1 als ersten Schritt und die erneute Verschlüsselung der Spalten. Die Spalten nutzen zunächst die deterministische Verschlüsselung und werden dann mit zufälliger Verschlüsselung erneut verschlüsselt, um umfangreiche Abfragen zu ermöglichen.

- Vorteile:
  - Die Neuverschlüsselung der Daten erfolgt direkt. Diese Methode wird empfohlen, wenn Sie umfangreiche Abfragen für verschlüsselte Spalten aktivieren müssen, die derzeit die deterministische Verschlüsselung nutzen und große Datenmengen enthalten. Der erste Schritt (die Rotation des Spaltenhauptschlüssels) ermöglicht die direkte Verschlüsselung von Spalten mithilfe der deterministischen Verschlüsselung, und der zweite Schritt (die erneute Verschlüsselung der Spalten) kann direkt durchgeführt werden.
  - Kann die Enclave-Funktionalität für mehrere Spalten skalierbar aktivieren.
  
- Nachteile:
  - Sie können nicht einige der Spalten selektiv konvertieren, die einem bestimmten Spaltenhauptschlüssel zugeordnet sind.
  - Es kommt zu überflüssigem Schlüsselverwaltungsaufwand. Sie müssen einen neuen Spaltenhauptschlüssel erstellen und für Anwendungen zur Verfügung stellen, die die betroffenen Spalten abfragen.

Informationen zum Rotieren eines Spaltenhauptschlüssels und zum direkten erneuten Verschlüsseln für die Rotation eines Spaltenverschlüsselungsschlüssels finden Sie unter [Rotieren Enclave-fähiger Schlüssel](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-3-re-encrypt-a-selected-column-with-an-enclave-enabled-column-encryption-key-on-the-client-side"></a>Method 3: Erneutes Verschlüsseln einer ausgewählten Spalte mit einem Enclave-fähigen Spaltenverschlüsselungsschlüssel auf der Clientseite
Diese Methode umfasst die erneute Verschlüsselung eines Enclave-fähigen Spaltenverschlüsselungsschlüssels, um umfangreiche Abfragen mit zufälliger Verschlüsselung zu ermöglichen. Da der aktuelle Spaltenverschlüsselungsschlüssel nicht Enclave-fähig ist, können Sie die Spalte nicht direkt erneut verschlüsseln. Verwenden Sie den Always Encrypted-Assistenten oder das Cmdlet „Set-SqlColumnEncryption“, um die Spalten außerhalb der Datenbank erneut zu verschlüsseln.

- Vorteile:
  - Das selektive Aktivieren der Enclave-Funktionalität (direkte Verschlüsselung und umfangreiche Abfragen, wenn Sie die Spalte mit zufälliger Verschlüsselung erneut verschlüsseln) wird für eine Spalte oder eine kleine Teilmenge der Spalten ermöglicht.
  - In nur einem Schritt können umfangreiche Berechnungen für Spalten aktiviert werden.
  - Es ist nicht erforderlich, einen neuen Spaltenhauptschlüssel zu erstellen, sodass die Auswirkungen auf Anwendungen geringer sind.
  
- Nachteile:
  - Das Tool verschiebt die Daten aus der Datenbank heraus, um sie zu verschlüsseln, was einige Zeit beanspruchen kann und anfällig für Netzwerkfehler ist.

Weitere Informationen zum Rotieren einer Spaltenverschlüsselung mithilfe eines clientseitigen Tools finden Sie unter [Rotieren von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) und [Rotieren von Always Encrypted-Schlüsseln mithilfe von PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Nächste Schritte
- [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md)
- [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md)
