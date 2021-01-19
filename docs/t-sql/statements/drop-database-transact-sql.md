---
description: DROP DATABASE (Transact-SQL)
title: DROP DATABASE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP DATABASE
- DROP_DATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], deleting
- removing databases
- database snapshots [SQL Server], removing
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- DROP DATABASE statement
- database removal [SQL Server], DROP DATABASE statement
ms.assetid: 477396a9-92dc-43c9-9b97-42c8728ede8e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cef57db413b9035926e8c05446cd761cb937d538
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172112"
---
# <a name="drop-database-transact-sql"></a>DROP DATABASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Entfernt eine oder mehrere Benutzerdatenbanken oder Datenbank-Momentaufnahmen aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
-- SQL Server Syntax
DROP DATABASE [ IF EXISTS ] { database_name | database_snapshot_name } [ ,...n ] [;]
```

```syntaxsql
-- Azure SQL Database, Azure Synapse Analytics and Analytics Platform System Syntax
DROP DATABASE database_name [;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*IF EXISTS*
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]bis [aktuelle Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).

Löscht die Datenbank nur, wenn diese bereits vorhanden ist.

*database_name* Gibt den Namen der zu entfernenden Datenbank an. Zum Anzeigen einer Liste von Datenbanken verwenden Sie die [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht.

*database_snapshot_name*
**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Gibt den Namen der zu entfernenden Datenbankmomentaufnahme an.

## <a name="general-remarks"></a>Allgemeine Hinweise

Eine Datenbank kann unabhängig von ihrem Status (offline, schreibgeschützt, fehlerverdächtig usw.) gelöscht werden. Zur Anzeige des aktuellen Status einer Datenbank verwenden Sie die **sys.databases**-Katalogsicht.

Eine gelöschte Datenbank kann nur neu erstellt werden, indem eine Sicherungskopie wiederhergestellt wird. Datenbankmomentaufnahmen können nicht gesichert und somit nicht wiederhergestellt werden.

Vor dem Löschen einer Datenbank sollte die [Masterdatenbank](../../relational-databases/databases/master-database.md) gesichert werden.

Beim Löschen einer Datenbank wird die Datenbank von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz entfernt, und die von der Datenbank verwendeten physischen Datenträgerdateien werden gelöscht. Wenn die Datenbank oder eine ihrer Dateien beim Löschen offline ist, werden die Datenträgerdateien nicht gelöscht. Diese Dateien können manuell mit dem Windows-Explorers gelöscht werden. Verwenden Sie [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md), um eine Datenbank vom aktuellen Server zu entfernen, ohne dass die Dateien aus dem Dateisystem gelöscht werden.

> [!WARNING]
> Sie können zwar eine Datenbankdatei entfernen, der FILE_SNAPSHOT-Sicherungen zugeordnet sind, jedoch werden keine Datenbankdateien gelöscht, denen Momentaufnahmen zugeordnet sind, um zu vermeiden, dass die Sicherungen, die auf die Datenbankdatei verweisen, ungültig gemacht werden. Die Datei wird zwar abgeschnitten, aber nicht physisch gelöscht, damit die FILE_SNAPSHOT-Sicherungen vollständig erhalten bleiben. Weitere Informationen finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). **Gilt für**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658).

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Wenn Sie eine Datenbankmomentaufnahme löschen, wird diese aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz gelöscht. Außerdem werden die Sparsedateien des physischen NTFS-Dateisystems gelöscht, die von der Momentaufnahme verwendet werden. Informationen zum Verwenden von Sparsedateien für Datenbankmomentaufnahmen finden Sie unter [Datenbankmomentaufnahmen](../../relational-databases/databases/database-snapshots-sql-server.md). Durch das Löschen einer Datenbankmomentaufnahme wird der Plancache für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gelöscht. Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. Für jeden geleerten Cachespeicher im Plancache enthält das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll folgende Meldung zur Information: „[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat für den "%2!s!"-Cachespeicher (Bestandteil des Plancache) %1!s! Leerungen des Cachespeichers gefunden, die von Datenbankwartungs- oder Neukonfigurierungsvorgängen ausgelöst wurden.“. Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

## <a name="interoperability"></a>Interoperabilität

### <a name="sql-server"></a>SQL Server

Zum Löschen einer für die Transaktionsreplikation oder die Mergereplikation veröffentlichten Datenbank bzw. einer von der Mergereplikation abonnierten Datenbank müssen Sie zunächst die Replikation von der Datenbank entfernen. Wenn eine Datenbank beschädigt ist oder die Replikation nicht zuerst entfernt werden kann, oder wenn beides zutrifft, können Sie die Datenbank dennoch mithilfe von ALTER DATABASE löschen. Sie legen hierzu den Status der Datenbank auf offline fest und löschen diese dann.

Wenn die Datenbank für den Protokollversand verwendet wird, müssen Sie den Protokollversand vor dem Löschen der Datenbank entfernen. Weitere Informationen finden Sie unter [Informationen zum Protokollversand](../../database-engine/log-shipping/about-log-shipping-sql-server.md).

## <a name="limitations-and-restrictions"></a>Einschränkungen

[Systemdatenbanken](../../relational-databases/databases/system-databases.md) können nicht gelöscht werden.

Die DROP DATABASE-Anweisung muss im Autocommitmodus ausgeführt werden und ist in einer expliziten oder impliziten Transaktion nicht zulässig. Der Autocommitmodus ist der Standardmodus für die Transaktionsverwaltung.

Sie können eine aktuell verwendete Datenbank nicht löschen. Dies bezieht sich auf eine von einem beliebigen Benutzer für Schreib- oder Lesevorgänge geöffnete Datei. Zum Entfernen von Benutzern aus der Datenbank können Sie z.B. die Datenbank mithilfe von ALTER DATABASE auf SINGLE_USER festlegen.

> [!WARNING]
> Bei diesem Ansatz können allerdings Fehler auftreten, da die erste von vielen Verbindungen, die von einem beliebigen Thread hergestellt werden, immer einen SINGLE_USER-Thread erhält, wodurch die Verbindung fehlschlägt. In SQL-Server ist keine Komponente zum Entfernen von Datenbanken unter Last integriert.

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Alle Datenbankmomentaufnahmen in einer Datenbank müssen vor dem Löschen der Datenbank gelöscht werden.

Wenn eine Datenbank gelöscht wird, die für Stretch Database aktiviert ist, werden keine Remotedaten gelöscht. Wenn Sie die Remotedaten löschen möchten, müssen Sie dies manuell tun.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Sie müssen mit der master-Datenbank verbunden sein, um eine Datenbank zu löschen.

 Die DROP DATABASE-Anweisung muss die einzige Anweisung in einem SQL-Batch sein, und es darf jeweils nur eine Datenbank gelöscht werden.

### [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

Sie müssen mit der master-Datenbank verbunden sein, um eine Datenbank zu löschen.

Die DROP DATABASE-Anweisung muss die einzige Anweisung in einem SQL-Batch sein, und es darf jeweils nur eine Datenbank gelöscht werden.

## <a name="permissions"></a>Berechtigungen

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Erfordert die **CONTROL**-Berechtigung für die Datenbank, die **ALTER ANY DATABASE**-Berechtigung oder die Mitgliedschaft in der festen Datenbankrolle **db_owner**.

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Datenbanken können nur durch den Prinzipalanmeldenamen auf Serverebene (vom Bereitstellungsprozess erstellt) oder Mitglieder der Datenbankrolle **dbmanager** gelöscht werden.

### [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Erfordert die **CONTROL**-Berechtigung für die Datenbank, die **ALTER ANY DATABASE**-Berechtigung oder die Mitgliedschaft in der festen Datenbankrolle **db_owner**.

## <a name="examples"></a>Beispiele

### <a name="a-dropping-a-single-database"></a>A. Löschen einer einzelnen Datenbank

Im folgenden Beispiel wird die Datenbank `Sales` entfernt.

```sql
DROP DATABASE Sales;
```

### <a name="b-dropping-multiple-databases"></a>B. Löschen mehrerer Datenbanken

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Im folgenden Beispiel wird jede der aufgelisteten Datenbanken entfernt.

```sql
DROP DATABASE Sales, NewSales;
```

### <a name="c-dropping-a-database-snapshot"></a>C. Löschen einer Datenbankmomentaufnahme

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

Im folgenden Beispiel wird eine Datenbankmomentaufnahme mit dem Namen `sales_snapshot0600` entfernt, ohne dass sich dies auf die Quelldatenbank auswirkt.

```sql
DROP DATABASE sales_snapshot0600;
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
