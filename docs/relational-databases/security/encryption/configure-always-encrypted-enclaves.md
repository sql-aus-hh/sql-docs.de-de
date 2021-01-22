---
title: Konfigurieren und Verwenden von Always Encrypted mit Secure Enclaves | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Always Encrypted mit Secure Enclaves in SQL Server und Azure SQL-Datenbank konfigurieren und verwenden, was umfassendere Funktionalitäten für sensible Daten ermöglicht.
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
ms.openlocfilehash: 481493a50fdefc22f6eb4d77feb13cfc4848388d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534649"
---
# <a name="configure-and-use-always-encrypted-with-secure-enclaves"></a>Konfigurieren und Verwenden von Always Encrypted mit Secure Enclaves 

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) erweitert das vorhandene [Always Encrypted](always-encrypted-database-engine.md)-Feature um umfangreichere Funktionalitäten für sensible Daten und schützt gleichzeitig die vertraulichen Daten. Dieser Artikel führt allgemeine Aufgaben beim Konfigurieren und Verwenden des Features auf.

Ein Tutorial für den schnellen Einstieg mit Always Encrypted mit Secure Enclaves finden Sie unter

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md).
- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="set-up-the-secure-enclave-and-attestation"></a>Einrichten der Secure Enclave-Instanz und des Nachweises

Vor der Verwendung von Always Encrypted mit Secure Enclaves müssen Sie in der Konfiguration Ihrer Umgebung sicherstellen, dass die sichere Enklave für die Datenbank verfügbar ist. Außerdem müssen Sie einen [Enclave-Nachweis](always-encrypted-enclaves.md#secure-enclave-attestation) einrichten. 

Der Vorgang zum Einrichten Ihrer Umgebung hängt davon ab, ob Sie [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] oder [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] verwenden.

### <a name="set-up-the-secure-enclave-and-attestation-in-ssnoversion-md"></a>Einrichten der Secure Enclave-Instanz und des Nachweises in [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

Details finden Sie in den folgenden Artikeln:
- [Planen des Nachweises des Host-Überwachungsdiensts](./always-encrypted-enclaves-host-guardian-service-plan.md)
- [Bereitstellen des Host-Überwachungsdiensts für [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)
- [Registrieren des Computers mit dem Host-Überwachungsdienst](./always-encrypted-enclaves-host-guardian-service-register.md)
- [Konfigurieren der Secure Enclave-Instanz in SQL Server](always-encrypted-enclaves-configure-enclave-type.md)

### <a name="set-up-the-secure-enclave-and-attestation-in-sssdsfull"></a>Einrichten der Secure Enclave-Instanz und des Nachweises in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]

Details finden Sie in den folgenden Artikeln:
- [Planen im Hinblick auf Intel SGX-Enclaves und -Nachweis in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-plan)
- [Aktivieren von Intel SGX für Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-enable-sgx)
- [Konfigurieren von Azure Attestation für Ihren logischen Azure SQL-Datenbank-Server](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-configure-attestation)

## <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves
Weitere Informationen finden Sie in den folgenden Artikeln:
- [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves – Übersicht](always-encrypted-enclaves-manage-keys.md)
- [Bereitstellen Enclave-fähiger Schlüssel](always-encrypted-enclaves-provision-keys.md)
- [Rotieren Enclave-fähiger Schlüssel](always-encrypted-enclaves-rotate-keys.md)

## <a name="configure-columns-with-always-encrypted-with-secure-enclaves"></a>Konfigurieren von Spalten mit Always Encrypted mit Secure Enclaves
Weitere Informationen finden Sie in den folgenden Artikeln:
- [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves – Übersicht](always-encrypted-enclaves-configure-encryption.md)
- [Direkte Konfiguration der Spaltenverschlüsselung mit Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Aktivieren von Always Encrypted mit Secure Enclaves für vorhandene verschlüsselte Spalten](always-encrypted-enclaves-enable-for-encrypted-columns.md)

## <a name="run-transact-sql-statements-using-secure-enclaves"></a>Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves
Weitere Informationen finden Sie in den folgenden Artikeln:
- [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md)
- [Behandeln von häufig auftretenden Problemen bei Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-troubleshooting.md)

## <a name="create-and-use-indexes-on-enclave-enabled-columns"></a>Erstellen und Verwenden von Indizes für Enclave-fähige Spalten
Weitere Informationen finden Sie in den folgenden Artikeln:
- [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-create-use-indexes.md)
  
## <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves
Weitere Informationen finden Sie in den folgenden Artikeln:
- [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md)
