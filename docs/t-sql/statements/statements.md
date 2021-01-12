---
title: Transact-SQL-Anweisungen
description: Transact-SQL-Anweisungen
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Alter_TSQL
dev_langs:
- TSQL
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 04/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 65f48884fff2fc5ce9fc018f8d79b3c2da5a4770
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079633"
---
# <a name="transact-sql-statements"></a>Transact-SQL-Anweisungen

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Eine SQL-Anweisung ist eine unteilbare Arbeitseinheit, die entweder vollständig erfolgreich ist oder vollständig erfolglos. Eine SQL-Anweisung ist ein Satz von Anweisungen, der aus Bezeichnern, Parametern, Variablen, Namen, Datentypen und reservierten SQL-Wörtern besteht, die erfolgreich kompiliert werden. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] erstellt eine *implizite* Transaktion für eine SQL-Anweisung, wenn ein `BeginTransaction`-Befehl nicht den Beginn einer Transaktion angibt. Wenn die Anweisung erfolgreich ist, führt [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] immer einen Commit für die implizite Transaktionen aus, und wenn der Befehl einen Fehler generiert, wird ein Rollback für die implizite Transaktion ausgeführt.  

Es gibt viele Arten von Anweisungen. Die wahrscheinlich wichtigste ist die [SELECT](../queries/select-transact-sql.md)-Anweisung, die Zeilen aus der Datenbank abruft und die Auswahl einer oder vieler Zeilen oder Spalten aus einer Tabelle oder zahlreichen Tabellen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht. In diesem Artikel werden die Kategorien der Anweisungen zusammengefasst, die mit Transact-SQL (T-SQL) ergänzend zur `SELECT`-Anweisung verwendet werden. Eine Auflistung aller Anweisungen finden Sie im linken Navigationsbereich.

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Die Backup- und Restore-Anweisungen stellen Möglichkeiten bereit, um Sicherungen zu erstellen und Wiederherstellungen aus Sicherungen vorzunehmen.  Weitere Informationen finden Sie in der [Übersicht zur Sicherung und Wiederherstellung](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).

## <a name="data-definition-language"></a>Datendefinitionssprache

Durch DDL-Anweisungen (Data Definition Language, Datendefinitionssprache) werden Datenstrukturen definiert. Verwenden Sie die Anweisungen, um Datenstrukturen in einer Datenbank zu erstellen, zu ändern oder zu löschen. Zu diesen Anweisungen gehören:

- ALTER
- Sortierungen
- CREATE
- DROP
- DISABLE TRIGGER
- ENABLE TRIGGER
- RENAME
- UPDATE STATISTICS
- TRUNCATE TABLE

## <a name="data-manipulation-language"></a>Datenbearbeitungssprache

Die DML (Data Manipulation Language, Datenbearbeitungssprache) wirkt sich auf die in der Datenbank gespeicherten Informationen aus. Verwenden Sie diese Anweisungen, um Zeilen in der Datenbank einzufügen, zu aktualisieren und zu verändern.

- BULK INSERT
- Delete
- INSERT
- SELECT
- UPDATE
- MERGE

## <a name="permissions-statements"></a>Berechtigungsanweisungen

Durch Berechtigungsanweisungen wird bestimmt, welche Benutzer und Konten auf Daten zugreifen und Vorgänge ausführen können. Weitere Informationen zur Authentifizierung und zum Zugriff finden Sie im [Sicherheitscenter](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).

## <a name="service-broker-statements"></a>Service Broker-Anweisungen

Bei Service Broker handelt es sich um ein Feature, das native Unterstützung für Messaging- und Warteschlangenanwendungen bereitstellt. Weitere Informationen finden Sie unter [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md).

## <a name="session-settings"></a>Sitzungseinstellungen

Durch SET-Anweisungen wird bestimmt, wie die aktuelle Sitzung ausgeführte Uhrzeiteinstellungen verarbeitet. Eine Übersicht finden Sie unter [SET-Anweisungen](set-statements-transact-sql.md).
