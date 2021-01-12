---
description: COLLATE (Transact-SQL)
title: COLLATE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLLATE
- COLLATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], COLLATE clause
- COLLATE clause
ms.assetid: 76763ac8-3e0d-4bbb-aa53-f5e7da021daa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d456379700ba48513105569a8486cf6f7a615ce3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098454"
---
# <a name="collate-transact-sql"></a>COLLATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definiert eine Sortierung einer Datenbank oder Tabellenspalte, oder einen Umwandlungsvorgang für eine Sortierung, wenn er für einen Zeichenfolgenausdruck angewendet wird. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Wenn Sie während der Datenbankerstellung keine Sortierung angeben, wird der Datenbank die Standardsortierung der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugewiesen. Wenn Sie während der Tabellenspaltenerstellung keine Sortierung angeben, wird der Spalte die Standardsortierung der Datenbank zugewiesen.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
COLLATE { <collation_name> | database_default }
<collation_name> :: =
    { Windows_collation_name } | { SQL_collation_name }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*collation_name* Der Name der Sortierung, die auf den Ausdruck, die Spaltendefinition oder die Datenbankdefinition angewendet werden soll. *collation_name* kann nur ein bestimmter *Windows_collation_name* oder ein *SQL_collation_name* sein. *collation_name* muss ein Literalwert sein. *collation_name* kann nicht durch eine Variable oder einen Ausdruck dargestellt werden.

*Windows_collation_name* ist der Name für einen [Windows-Sortierungsnamen](../../t-sql/statements/windows-collation-name-transact-sql.md).

*SQL_collation_name* ist der Name für einen [SQL Server-Sortierungsnamen](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

**database_default** Bewirkt, dass die COLLATE-Klausel die Sortierung der aktuellen Datenbank erbt.

## <a name="remarks"></a>Hinweise

Die COLLATE-Klausel kann auf mehreren Ebenen angegeben werden. Diese umfassen Folgendes:

1. Erstellen oder Ändern einer Datenbank.

    Sie können die COLLATE-Klausel der Anweisungen `CREATE DATABASE` oder `ALTER DATABASE` verwenden, um die Standardsortierung der Datenbank anzugeben. Sie können eine Sortierung auch dann angeben, wenn Sie eine Datenbank mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] erstellen. Wenn Sie keine Sortierung angeben, wird der Datenbank die Standardsortierung der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugewiesen.

    > [!NOTE]
    > Nur-Unicode-Sortierungen von Windows können nur mit der COLLATE-Klausel verwendet werden, um Sortierungen auf die Datentypen **nchar**, **nvarchar** und **ntext** bei Daten auf Spalten- und Ausdrucksebene anzuwenden. Diese können nicht mit der COLLATE-Klausel verwendet werden, um die Sortierung einer Datenbank oder Serverinstanz zu definieren oder zu ändern.

2. Erstellen oder Ändern einer Tabellenspalte.

    Sie können mithilfe der COLLATE-Klausel der Anweisungen `CREATE TABLE` oder `ALTER TABLE` für jede Zeichenfolgenspalte eine Sortierung angeben. Sie können eine Sortierung auch dann angeben, wenn Sie eine Tabelle mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] erstellen. Wenn Sie keine Sortierung angeben, wird der Spalte die Standardsortierung der Datenbank zugewiesen.

    Sie können außerdem die Option `database_default` in der COLLATE-Klausel verwenden, um anzugeben, dass für eine Spalte einer temporären Tabelle anstelle von **tempdb** die Standardsortierung der aktuellen Benutzerdatenbank für die Verbindung verwendet wird.

3. Umwandeln der Sortierung eines Ausdrucks.

    Sie können die COLLATE-Klausel verwenden, um einen Zeichenausdruck auf eine bestimmte Sortierung anzuwenden. Zeichenliteralen und Variablen wird die Standardsortierung der aktuellen Datenbank zugewiesen. Spaltenverweisen wird die Definitionssortierung der Spalte zugewiesen.

Die Sortierung eines Bezeichners hängt von der Ebene ab, auf der er definiert ist. Bezeichnern von Objekten auf Instanzebene, wie z. B. Anmeldenamen und Datenbanknamen, wird die Standardsortierung der Instanz zugewiesen. Bezeichnern von Objekten innerhalb einer Datenbank, wie z. B. Tabellen, Sichten und Spaltennamen, wird die Standardsortierung der Datenbank zugewiesen. Beispielsweise können in einer Datenbank mit einer Sortierung mit Unterscheidung nach Groß-/Kleinschreibung zwei Tabellen mit gleichen Namen, die sich nur durch verschiedene Groß-/Kleinschreibung unterscheiden, erstellt werden; in einer Datenbank mit einer Sortierung ohne Unterscheidung nach Groß-/Kleinschreibung ist dies jedoch nicht möglich. Weitere Informationen finden Sie unter [Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md).

Variablen, GOTO-Marken, temporär gespeicherte Prozeduren und temporäre Tabellen können erstellt werden, wenn der Verbindungskontext einer Datenbank zugeordnet ist. Anschließend kann darauf verwiesen werden, wenn zum Kontext einer anderen Datenbank gewechselt wurde. Die Bezeichner von Variablen, GOTO-Marken, temporär gespeicherten Prozeduren und temporären Tabellen befinden sich in der Standardsortierung der Serverinstanz.

Die COLLATE-Klausel gilt nur für die Datentypen **char**, **varchar**, **text**, **nchar**, **nvarchar** und **ntext**.

COLLATE verwendet *collate_name*, um den Namen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierung oder der Windows-Sortierung anzugeben, die auf den Ausdruck, die Spaltendefinition oder die Datenbankdefinition angewendet werden soll. *collation_name* kann nur ein bestimmter *Windows_collation_name* oder ein *SQL_collation_name* sein und der Parameter muss einen Literalwert enthalten. *collation_name* kann nicht durch eine Variable oder einen Ausdruck dargestellt werden.

Sortierungen werden außer beim Setup in der Regel durch den Sortierungsnamen identifiziert. Geben Sie beim Setup stattdessen den Sortierungskennzeichner (das Gebietsschema) für Windows-Sortierungen und dann die Sortierungsoptionen an, z.B. Unterscheidung nach Groß-/Kleinschreibung oder Akzenten.

Sie können die [fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)-Systemfunktion ausführen, um eine Liste aller gültigen Namen für Windows- und SQL Server-Sortierungen abzurufen:

```sql
SELECT name, description
FROM fn_helpcollations();
```

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt nur Codepages, die vom zugrunde liegenden Betriebssystem unterstützt werden. Wenn Sie eine Aktion ausführen, die von Sortierungen abhängt, muss die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierung, die von dem Objekt verwendet wird, auf das verwiesen wird, eine Codepage verwenden, die vom Betriebssystem des Computers unterstützt wird. Mögliche Aktionen sind:

- Angeben einer Standardsortierung für eine Datenbank, wenn Sie die Datenbank erstellen oder ändern.
- Angeben einer Sortierung für eine Spalte, wenn Sie eine Tabelle erstellen oder ändern.
- Beim Wiederherstellen oder Anfügen einer Datenbank müssen die Standardsortierung der Datenbank und die Sortierungen aller Spalten und Parameter, die zur Datenbank gehören und den Datentyp **char**, **varchar** oder **text** aufweisen, vom Betriebssystem unterstützt werden.

> [!NOTE]
> Codepageübersetzungen werden für die Datentypen **char** und **varchar**, nicht jedoch für den **text**-Datentyp unterstützt. Datenverlust während der Codepageübersetzung wird nicht gemeldet.
>
> Wenn die angegebene Sortierung oder die Sortierung, die von dem Objekt verwendet wird, auf das verwiesen wird, eine Codepage verwendet, die nicht von Windows unterstützt wird, zeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Fehler an.

## <a name="examples"></a>Beispiele

### <a name="a-specifying-collation-during-a-select"></a>A. Angeben einer Sortierung beim Verwenden von SELECT

Im folgenden Beispiel wird eine einfache Tabelle erstellt, und 4 Zeilen werden eingefügt. Dann werden im Beispiel zwei Sortierungen bei der Auswahl von Daten aus der Tabelle angewendet, um zu zeigen, wie `Chiapas` unterschiedlich sortiert wird.

```sql
CREATE TABLE Locations
(Place varchar(15) NOT NULL);
GO
INSERT Locations(Place) VALUES ('Chiapas'),('Colima')
                             , ('Cinco Rios'), ('California');
GO
--Apply an typical collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Latin1_General_CS_AS_KS_WS ASC;
GO
-- Apply a Spanish collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Traditional_Spanish_ci_ai ASC;
GO
```

Dies sind die Ergebnisse der ersten Abfrage.

```output
Place
-------------
California
Chiapas
Cinco Rios
Colima
```

Dies sind die Ergebnisse der zweiten Abfrage.

```output
Place
-------------
California
Cinco Rios
Colima
Chiapas
```

### <a name="b-additional-examples"></a>B. Weitere Beispiele

Zusätzliche Beispiele zur Verwendung von **COLLATE** finden Sie unter [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md#examples) in Beispiel **G. Erstellen einer Datenbank und Angeben eines Sortierungsnamens und von Optionen**, und unter [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#alter_column) in Beispiel **V. Ändern der Spaltensortierung**.

## <a name="see-also"></a>Siehe auch

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md)
- [Rangfolge von Sortierungen](../../t-sql/statements/collation-precedence-transact-sql.md)
- [Konstanten](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [Tabellendatentyp](../../t-sql/data-types/table-transact-sql.md)
