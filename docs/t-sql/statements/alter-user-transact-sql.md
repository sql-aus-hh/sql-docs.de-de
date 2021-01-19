---
description: ALTER USER (Transact-SQL)
title: ALTER USER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 21314c02ddb58aabcee33257fc4d8d44f56b9da7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170572"
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)

Benennt einen Datenbankbenutzer um oder ändert sein Standardschema.

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for SQL Server

ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
```

## <a name="arguments"></a>Argumente

 *userName*: Gibt den Namen an, mit dem der Benutzer innerhalb dieser Datenbank identifiziert wird.

 LOGIN **=** _loginName_: Ordnet einen Benutzer einer anderen Anmeldung neu zu. Dazu wird die Sicherheits-ID (SID) des Benutzers in die SID der Anmeldung geändert.

 NAME **=** _newUserName_: Gibt den neuen Namen für diesen Benutzer an. *newUserName* darf in der aktuellen Datenbank noch nicht vorhanden sein.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL }: Gibt das erste Schema an, das vom Server beim Auflösen der Namen von Objekten für diesen Benutzer durchsucht wird. Wenn das Standardschema auf NULL festgelegt wird, wird ein Standardschema aus einer Windows-Gruppe entfernt. Die NULL-Option kann nicht in Verbindung mit einem Windows-Benutzer verwendet werden.

 PASSWORD **=** '*password*'  **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Gibt das Kennwort für den Benutzer an, der geändert wird. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.

> [!NOTE]
> Diese Option ist nur für enthaltene Benutzer verfügbar. Weitere Informationen finden Sie unter [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md) und [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Das aktuelle Benutzerkennwort, das durch '*password*' ersetzt wird. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. *OLD_PASSWORD* ist für die Änderung eines Kennworts erforderlich, wenn Sie nicht über die **ALTER ANY USER**-Berechtigung verfügen. Mit *OLD_PASSWORD* wird verhindert, dass Benutzer mit der **IMPERSONATION**-Berechtigung das Kennwort ändern.

> [!NOTE]
> Diese Option ist nur für enthaltene Benutzer verfügbar.

 DEFAULT_LANGUAGE **=** _{NONE \| \<lcid> \| \<language name> \| \<language alias>}_ **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

 Gibt eine Standardsprache an, die dem Benutzer zugewiesen werden soll. Wenn diese Option auf NONE festgelegt wird, wird die aktuelle Standardsprache der Datenbank als Standardsprache festgelegt. Wenn die Standardsprache der Datenbank später geändert wird, bleibt die Standardsprache des Benutzers unverändert. *DEFAULT_LANGUAGE* kann die lokale ID (lcid), der Name der Sprache oder der Sprachalias sein.

> [!NOTE]
> Diese Option kann nur in einer eigenständigen Datenbank und nur für eigenständige Benutzer angegeben werden.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **Gilt für**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Verhindert bei Massenkopiervorgängen kryptografische Metadatenüberprüfungen auf dem Server. Dadurch kann der Benutzer durch Massenkopiervorgänge Daten zwischen Tabellen oder Datenbanken austauschen, ohne dabei die Daten zu verschlüsseln. Der Standardwert ist OFF.

> [!WARNING]
> Die falsche Verwendung dieser Option kann zur Datenbeschädigung führen. Weitere Informationen finden Sie unter [Migrieren von durch Always Encrypted geschützten sensiblen Daten](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Bemerkungen

 Das Standardschema ist das erste Schema, das vom Server beim Auflösen der Namen von Objekten für diesen Datenbankbenutzer durchsucht wird. Wenn nicht anders angegeben, ist das Standardschema der Besitzer von Objekten, die von diesem Datenbankbenutzer erstellt werden.

 Wenn der Benutzer ein Standardschema hat, wird dieses Standardschema verwendet. Wenn der Benutzer kein Standardschema hat, aber Mitglied einer Gruppe mit einem Standardschema ist, wird das Standardschema der Gruppe verwendet. Wenn der Benutzer kein Standardschema hat und Mitglied von mehreren Gruppen ist, ist das Standardschema für den Benutzer das Schema der Windows-Gruppe mit der niedrigsten principal_id und explizit festgelegt. Wenn für einen Benutzer kein Standardschema bestimmt werden kann, wird das Schema **dbo** verwendet.

 DEFAULT_SCHEMA kann auf ein Schema festgelegt werden, das zurzeit nicht in der Datenbank vorhanden ist. Deshalb können Sie DEFAULT_SCHEMA einem Benutzer zuweisen, bevor das Schema erstellt wird.

 DEFAULT_SCHEMA kann nicht für einen Benutzer angegeben werden, der einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist.

> [!IMPORTANT]
> Der Wert von DEFAULT_SCHEMA wird ignoriert, wenn der Benutzer ein Mitglied der festen Serverrolle **sysadmin** ist. Alle Mitglieder der festen Serverrolle **sysadmin** verfügen über ein `dbo`-Standardschema .

 Sie können den Namen eines einer Windows-Anmeldung oder -Gruppe zugeordneten Benutzers ändern, wenn die SID des neuen Benutzernamens mit der SID in der Datenbank übereinstimmt. Diese Überprüfung trägt dazu bei, das Spoofing von Windows-Anmeldungen in der Datenbank zu verhindern.

 Mit der WITH LOGIN-Klausel kann ein Benutzer einer anderen Anmeldung neu zugeordnet werden. Benutzer ohne Anmeldenamen, einem Zertifikat zugeordnete Benutzer oder einem asymmetrischen Schlüssel zugeordnete Benutzer können mithilfe dieser Klausel nicht neu zugeordnet werden. Nur SQL-Benutzer und Windows-Benutzer (oder -Gruppen) können neu zugeordnet werden. Mit der WITH LOGIN-Klausel kann nicht der Benutzertyp geändert werden, beispielsweise kann ein Windows-Konto nicht in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen geändert werden.

 Der Name des Benutzers wird automatisch in den Anmeldenamen umbenannt, wenn die folgenden Voraussetzungen erfüllt sind.

- Der Benutzer ist ein Windows-Benutzer.

- Der Name ist ein Windows-Name (enthält einen umgekehrten Schrägstrich).

- Es wurde kein neuer Name angegeben.

- Der aktuelle Name unterscheidet sich vom Benutzernamen.

 Andernfalls wird der Benutzer nicht umbenannt, es sei denn, der Aufrufer ruft zusätzlich die NAME-Klausel auf.

Der Name eines Benutzers, der einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen, einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist, kann keinen umgekehrten Schrägstrich (\\) enthalten.

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sicherheit

> [!NOTE]
> Ein Benutzer mit **ALTER ANY USER**-Berechtigung kann das Standardschema jedes Benutzers ändern. Ein Benutzer mit einem geänderten Schema könnte dann versehentlich Daten aus der falschen Tabelle auswählen oder Code aus dem falschen Schema ausführen.

### <a name="permissions"></a>Berechtigungen

 Zum Ändern des Namens eines Benutzers ist die **ALTER ANY USER**-Berechtigung erforderlich.

 Um die Zielanmeldung eines Benutzers zu ändern, wird die **CONTROL**-Berechtigung für die Datenbank benötigt.

 Um den Benutzernamen eines Benutzers zu ändern, der über die **CONTROL**-Berechtigung für die Datenbank verfügt, ist die **CONTROL**-Berechtigung für die Datenbank erforderlich.

 Zum Ändern des Standardschemas oder der Sprache ist die **ALTER**-Berechtigung für den Benutzer erforderlich. Benutzer können ihr eigenes Standardschema oder ihre eigene Sprache ändern.

## <a name="examples"></a>Beispiele

Alle Beispiele werden in einer Benutzerdatenbank ausgeführt.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Ändern des Namens eines Datenbankbenutzers

 Im folgenden Beispiel wird der Name des Datenbankbenutzers `Mary5` in `Mary51` geändert.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Ändern des Standardschemas eines Benutzers

 Im folgenden Beispiel wird das Standardschema des Benutzers `Mary51` in `Purchasing` geändert.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Gleichzeitiges Ändern mehrerer Optionen

 Im folgenden Beispiel werden mehrere Optionen für einen Benutzer einer eigenständigen Datenbank in einer Anweisung geändert.

**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL-Datenbank \*_**
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL-Datenbank

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Azure SQL Database

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = schemaName
| LOGIN = loginName
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]
[;]

-- Azure SQL Database Update Syntax
ALTER USER userName
 WITH <set_item> [ ,...n ]
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- SQL Database syntax when connected to a federation member
ALTER USER userName
 WITH <set_item> [ ,... n ]
[;]

<set_item> ::=
 NAME = newUserName
```

## <a name="arguments"></a>Argumente

 *userName*: Gibt den Namen an, mit dem der Benutzer innerhalb dieser Datenbank identifiziert wird.

 LOGIN **=** _loginName_: Ordnet einen Benutzer einer anderen Anmeldung neu zu. Dazu wird die Sicherheits-ID (SID) des Benutzers in die SID der Anmeldung geändert.

 Wenn ALTER USER die einzige Anweisung in einem SQL-Batch ist, unterstützt Azure SQL-Datenbank die WITH LOGIN-Klausel. Wenn ALTER USER nicht die einzige Anweisung in einem SQL-Batch ist oder in dynamischem SQL-Code ausgeführt wird, wird die WITH LOGIN-Klausel nicht unterstützt.

 NAME **=** _newUserName_: Gibt den neuen Namen für diesen Benutzer an. *newUserName* darf in der aktuellen Datenbank noch nicht vorhanden sein.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL }: Gibt das erste Schema an, das vom Server beim Auflösen der Namen von Objekten für diesen Benutzer durchsucht wird. Wenn das Standardschema auf NULL festgelegt wird, wird ein Standardschema aus einer Windows-Gruppe entfernt. Die NULL-Option kann nicht in Verbindung mit einem Windows-Benutzer verwendet werden.

 PASSWORD **=** '*password*'  **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Gibt das Kennwort für den Benutzer an, der geändert wird. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.

> [!NOTE]
> Diese Option ist nur für enthaltene Benutzer verfügbar. Weitere Informationen finden Sie unter [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md) und [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_ **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

 Das aktuelle Benutzerkennwort, das durch '*password*' ersetzt wird. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. *OLD_PASSWORD* ist für die Änderung eines Kennworts erforderlich, wenn Sie nicht über die **ALTER ANY USER**-Berechtigung verfügen. Mit *OLD_PASSWORD* wird verhindert, dass Benutzer mit der **IMPERSONATION**-Berechtigung das Kennwort ändern.

> [!NOTE]
> Diese Option ist nur für enthaltene Benutzer verfügbar.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ]  **Gilt für**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

 Verhindert bei Massenkopiervorgängen kryptografische Metadatenüberprüfungen auf dem Server. Dadurch kann der Benutzer durch Massenkopiervorgänge Daten zwischen Tabellen oder Datenbanken austauschen, ohne dabei die Daten zu verschlüsseln. Der Standardwert ist OFF.

> [!WARNING]
> Die falsche Verwendung dieser Option kann zur Datenbeschädigung führen. Weitere Informationen finden Sie unter [Migrieren von durch Always Encrypted geschützten sensiblen Daten](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Bemerkungen

 Das Standardschema ist das erste Schema, das vom Server beim Auflösen der Namen von Objekten für diesen Datenbankbenutzer durchsucht wird. Wenn nicht anders angegeben, ist das Standardschema der Besitzer von Objekten, die von diesem Datenbankbenutzer erstellt werden.

 Wenn der Benutzer ein Standardschema hat, wird dieses Standardschema verwendet. Wenn der Benutzer kein Standardschema hat, aber Mitglied einer Gruppe mit einem Standardschema ist, wird das Standardschema der Gruppe verwendet. Wenn der Benutzer kein Standardschema hat und Mitglied von mehreren Gruppen ist, ist das Standardschema für den Benutzer das Schema der Windows-Gruppe mit der niedrigsten principal_id und explizit festgelegt. Wenn für einen Benutzer kein Standardschema bestimmt werden kann, wird das Schema **dbo** verwendet.

 DEFAULT_SCHEMA kann auf ein Schema festgelegt werden, das zurzeit nicht in der Datenbank vorhanden ist. Deshalb können Sie DEFAULT_SCHEMA einem Benutzer zuweisen, bevor das Schema erstellt wird.

 DEFAULT_SCHEMA kann nicht für einen Benutzer angegeben werden, der einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist.

> [!IMPORTANT]
> Der Wert von DEFAULT_SCHEMA wird ignoriert, wenn der Benutzer ein Mitglied der festen Serverrolle **sysadmin** ist. Alle Mitglieder der festen Serverrolle **sysadmin** verfügen über ein `dbo`-Standardschema .

 Sie können den Namen eines einer Windows-Anmeldung oder -Gruppe zugeordneten Benutzers ändern, wenn die SID des neuen Benutzernamens mit der SID in der Datenbank übereinstimmt. Diese Überprüfung trägt dazu bei, das Spoofing von Windows-Anmeldungen in der Datenbank zu verhindern.

 Mit der WITH LOGIN-Klausel kann ein Benutzer einer anderen Anmeldung neu zugeordnet werden. Benutzer ohne Anmeldenamen, einem Zertifikat zugeordnete Benutzer oder einem asymmetrischen Schlüssel zugeordnete Benutzer können mithilfe dieser Klausel nicht neu zugeordnet werden. Nur SQL-Benutzer und Windows-Benutzer (oder -Gruppen) können neu zugeordnet werden. Mit der WITH LOGIN-Klausel kann nicht der Benutzertyp geändert werden, beispielsweise kann ein Windows-Konto nicht in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen geändert werden.

 Der Name des Benutzers wird automatisch in den Anmeldenamen umbenannt, wenn die folgenden Voraussetzungen erfüllt sind.

- Der Benutzer ist ein Windows-Benutzer.

- Der Name ist ein Windows-Name (enthält einen umgekehrten Schrägstrich).

- Es wurde kein neuer Name angegeben.

- Der aktuelle Name unterscheidet sich vom Benutzernamen.

 Andernfalls wird der Benutzer nicht umbenannt, es sei denn, der Aufrufer ruft zusätzlich die NAME-Klausel auf.

Der Name eines Benutzers, der einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen, einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist, kann keinen umgekehrten Schrägstrich (\\) enthalten.

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sicherheit

> [!NOTE]
> Ein Benutzer mit **ALTER ANY USER**-Berechtigung kann das Standardschema jedes Benutzers ändern. Ein Benutzer mit einem geänderten Schema könnte dann versehentlich Daten aus der falschen Tabelle auswählen oder Code aus dem falschen Schema ausführen.

### <a name="permissions"></a>Berechtigungen

 Zum Ändern des Namens eines Benutzers ist die **ALTER ANY USER**-Berechtigung erforderlich.

 Um die Zielanmeldung eines Benutzers zu ändern, wird die **CONTROL**-Berechtigung für die Datenbank benötigt.

 Um den Benutzernamen eines Benutzers zu ändern, der über die **CONTROL**-Berechtigung für die Datenbank verfügt, ist die **CONTROL**-Berechtigung für die Datenbank erforderlich.

 Zum Ändern des Standardschemas oder der Sprache ist die **ALTER**-Berechtigung für den Benutzer erforderlich. Benutzer können ihr eigenes Standardschema oder ihre eigene Sprache ändern.

## <a name="examples"></a>Beispiele

Alle Beispiele werden in einer Benutzerdatenbank ausgeführt.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Ändern des Namens eines Datenbankbenutzers

 Im folgenden Beispiel wird der Name des Datenbankbenutzers `Mary5` in `Mary51` geändert.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Ändern des Standardschemas eines Benutzers

 Im folgenden Beispiel wird das Standardschema des Benutzers `Mary51` in `Purchasing` geändert.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Gleichzeitiges Ändern mehrerer Optionen

 Im folgenden Beispiel werden mehrere Optionen für einen Benutzer einer eigenständigen Datenbank in einer Anweisung geändert.

```sql
ALTER USER Philip
WITHNAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per';
GO
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

## <a name="syntax"></a>Syntax

> [!IMPORTANT]
> Nur die folgenden Optionen werden für Azure SQL Managed Instance unterstützt, wenn sie auf Benutzer mit Azure AD-Konten angewendet werden: `DEFAULT_SCHEMA = { schemaName | NULL }` und `DEFAULT_LANGUAGE = { NONE | lcid | language name | language alias }`.
> </br> </br> Es wurde eine neue Syntaxerweiterung hinzugefügt, um die Neuzuordnung von Benutzern in einer Datenbank, die zu Azure SQL Managed Instance migriert wurde, zu unterstützen. Mit der ALTER USER-Syntax können Datenbankbenutzern in einer im Verbund konfigurierten und synchronisierten Azure AD-Domäne Azure AD-Anmeldenamen zugeordnet werden.

```syntaxsql
-- Syntax for SQL Managed Instance
ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
| ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]

-- Users or groups that are migrated as federated and synchronized with Azure AD have the following syntax:

/** Applies to Windows users that were migrated and have the following user names:
- Windows user <domain\user>
- Windows group <domain\MyWindowsGroup>
- Windows alias <MyWindowsAlias>
**/

ALTER USER userName
 { WITH <set_item> [ ,...n ] | FROM EXTERNAL PROVIDER }
[;]

<set_item> ::=
 NAME = newUserName
| DEFAULT_SCHEMA = { schemaName | NULL }
| LOGIN = loginName
| DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }
```

## <a name="arguments"></a>Argumente

 *userName*: Gibt den Namen an, mit dem der Benutzer innerhalb dieser Datenbank identifiziert wird.

 LOGIN **=** _loginName_: Ordnet einen Benutzer einer anderen Anmeldung neu zu. Dazu wird die Sicherheits-ID (SID) des Benutzers in die SID der Anmeldung geändert.

 Wenn ALTER USER die einzige Anweisung in einem SQL-Batch ist, unterstützt Azure SQL-Datenbank die WITH LOGIN-Klausel. Wenn ALTER USER nicht die einzige Anweisung in einem SQL-Batch ist oder in dynamischem SQL-Code ausgeführt wird, wird die WITH LOGIN-Klausel nicht unterstützt.

 NAME **=** _newUserName_: Gibt den neuen Namen für diesen Benutzer an. *newUserName* darf in der aktuellen Datenbank noch nicht vorhanden sein.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL }: Gibt das erste Schema an, das vom Server beim Auflösen der Namen von Objekten für diesen Benutzer durchsucht wird. Wenn das Standardschema auf NULL festgelegt wird, wird ein Standardschema aus einer Windows-Gruppe entfernt. Die NULL-Option kann nicht in Verbindung mit einem Windows-Benutzer verwendet werden.

 PASSWORD **=** '*password*'

 Gibt das Kennwort für den Benutzer an, der geändert wird. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.

> [!NOTE]
> Diese Option ist nur für enthaltene Benutzer verfügbar. Weitere Informationen finden Sie unter [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md) und [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).

 OLD_PASSWORD **=** _'oldpassword'_

 Das aktuelle Benutzerkennwort, das durch '*password*' ersetzt wird. Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. *OLD_PASSWORD* ist für die Änderung eines Kennworts erforderlich, wenn Sie nicht über die **ALTER ANY USER**-Berechtigung verfügen. Mit *OLD_PASSWORD* wird verhindert, dass Benutzer mit der **IMPERSONATION**-Berechtigung das Kennwort ändern.

> [!NOTE]
> Diese Option ist nur für enthaltene Benutzer verfügbar.

 DEFAULT_LANGUAGE **=** _{ NONE | \<lcid> | \<language name> | \<language alias> }_

 Gibt eine Standardsprache an, die dem Benutzer zugewiesen werden soll. Wenn diese Option auf NONE festgelegt wird, wird die aktuelle Standardsprache der Datenbank als Standardsprache festgelegt. Wenn die Standardsprache der Datenbank später geändert wird, bleibt die Standardsprache des Benutzers unverändert. *DEFAULT_LANGUAGE* kann die lokale ID (lcid), der Name der Sprache oder der Sprachalias sein.

> [!NOTE]
> Diese Option kann nur in einer eigenständigen Datenbank und nur für eigenständige Benutzer angegeben werden.

 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ON | **OFF**]

 Verhindert bei Massenkopiervorgängen kryptografische Metadatenüberprüfungen auf dem Server. Dadurch kann der Benutzer durch Massenkopiervorgänge Daten zwischen Tabellen oder Datenbanken austauschen, ohne dabei die Daten zu verschlüsseln. Der Standardwert ist OFF.

> [!WARNING]
> Die falsche Verwendung dieser Option kann zur Datenbeschädigung führen. Weitere Informationen finden Sie unter [Migrieren von durch Always Encrypted geschützten sensiblen Daten](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="remarks"></a>Bemerkungen

 Das Standardschema ist das erste Schema, das vom Server beim Auflösen der Namen von Objekten für diesen Datenbankbenutzer durchsucht wird. Wenn nicht anders angegeben, ist das Standardschema der Besitzer von Objekten, die von diesem Datenbankbenutzer erstellt werden.

 Wenn der Benutzer ein Standardschema hat, wird dieses Standardschema verwendet. Wenn der Benutzer kein Standardschema hat, aber Mitglied einer Gruppe mit einem Standardschema ist, wird das Standardschema der Gruppe verwendet. Wenn der Benutzer kein Standardschema hat und Mitglied von mehreren Gruppen ist, ist das Standardschema für den Benutzer das Schema der Windows-Gruppe mit der niedrigsten principal_id und explizit festgelegt. Wenn für einen Benutzer kein Standardschema bestimmt werden kann, wird das Schema **dbo** verwendet.

 DEFAULT_SCHEMA kann auf ein Schema festgelegt werden, das zurzeit nicht in der Datenbank vorhanden ist. Deshalb können Sie DEFAULT_SCHEMA einem Benutzer zuweisen, bevor das Schema erstellt wird.

 DEFAULT_SCHEMA kann nicht für einen Benutzer angegeben werden, der einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist.

> [!IMPORTANT]
> Der Wert von DEFAULT_SCHEMA wird ignoriert, wenn der Benutzer ein Mitglied der festen Serverrolle **sysadmin** ist. Alle Mitglieder der festen Serverrolle **sysadmin** verfügen über ein `dbo`-Standardschema .

 Sie können den Namen eines einer Windows-Anmeldung oder -Gruppe zugeordneten Benutzers ändern, wenn die SID des neuen Benutzernamens mit der SID in der Datenbank übereinstimmt. Diese Überprüfung trägt dazu bei, das Spoofing von Windows-Anmeldungen in der Datenbank zu verhindern.

 Mit der WITH LOGIN-Klausel kann ein Benutzer einer anderen Anmeldung neu zugeordnet werden. Benutzer ohne Anmeldenamen, einem Zertifikat zugeordnete Benutzer oder einem asymmetrischen Schlüssel zugeordnete Benutzer können mithilfe dieser Klausel nicht neu zugeordnet werden. Nur SQL-Benutzer und Windows-Benutzer (oder -Gruppen) können neu zugeordnet werden. Mit der WITH LOGIN-Klausel kann nicht der Benutzertyp geändert werden, beispielsweise kann ein Windows-Konto nicht in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen geändert werden. Die einzige Ausnahme ist das Umwandeln eines Windows-Benutzers in einen Azure AD-Benutzer.

> [!NOTE]
> Die folgenden Regeln gelten nicht für Windows-Benutzer von Azure SQL Managed Instance, da das Erstellen von Windows-Konten für Azure SQL Managed Instance nicht unterstützt wird. Die WITH LOGIN-Option kann nur verwendet werden, wenn Azure AD-Anmeldenamen vorhanden sind.

 Der Name des Benutzers wird automatisch in den Anmeldenamen umbenannt, wenn die folgenden Voraussetzungen erfüllt sind.

- Der Benutzer ist ein Windows-Benutzer.

- Der Name ist ein Windows-Name (enthält einen umgekehrten Schrägstrich).

- Es wurde kein neuer Name angegeben.

- Der aktuelle Name unterscheidet sich vom Benutzernamen.

 Andernfalls wird der Benutzer nicht umbenannt, es sei denn, der Aufrufer ruft zusätzlich die NAME-Klausel auf.

Der Name eines Benutzers, der einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen, einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist, kann keinen umgekehrten Schrägstrich (\\) enthalten.

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

### <a name="remarks-for-windows-users-in-sql-on-premises-migrated-to-azure-sql-managed-instance"></a>Hinweise für Windows-Benutzer in lokalen SQL-Instanzen nach einer Migration zu Azure SQL Managed Instance

Diese Hinweise gelten für die Authentifizierung als Windows-Benutzer, die mit Azure AD im Verbund konfiguriert und synchronisiert wurden.

> [!NOTE]
> Der Azure AD-Administrator für Azure SQL Managed Instance-Funktionen ändert sich nach der Erstellung. Weitere Informationen finden Sie unter [Neue Azure AD-Administratorfunktionen für verwaltete Instanzen](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

- Die Validierung von Windows-Benutzern oder -Gruppen, die Azure AD zugeordnet sind, erfolgt standardmäßig über die Graph-API in allen Versionen der für Migrationszwecke verwendeten ALTER USER-Syntax.
- Lokale Benutzer mit Aliasnamen, d.h. die einen anderen Namen als das ursprüngliche Windows-Konto verwenden, behalten den Aliasnamen bei.
- Bei der Azure AD-Authentifizierung gilt der LOGIN-Parameter nur für Azure SQL Managed Instance und kann nicht mit SQL-Datenbank verwendet werden.
- Verwenden Sie zum Anzeigen von Anmeldenamen für Azure AD-Prinzipale den folgenden Befehl: `select * from sys.server_principals`.
- Überprüfen Sie, ob der angegebene Typ des Anmeldenamens `E` oder `X` ist.
- Die PASSWORD-Option kann nicht für Azure AD-Benutzer verwendet werden.
- In allen Migrationsfällen werden die Rollen und Berechtigungen von Windows-Benutzern oder -Gruppen automatisch an die neuen Azure AD-Benutzer oder -Gruppen übertragen.
- Eine neue Syntaxerweiterung (**FROM EXTERNAL PROVIDER**) steht zur Verfügung, um Windows-Benutzer und -Gruppen aus lokalen SQL-Instanzen in Azure AD-Benutzer und -Gruppen zu ändern. Die Windows-Domäne muss im Verbund mit Azure AD konfiguriert sein, und alle Windows-Domänenmitglieder müssen bei der Verwendung dieser Erweiterung in Azure AD vorhanden sein. Die **FROM EXTERNAL PROVIDER**-Syntax gilt für Azure SQL Managed Instance und sollte verwendet werden, wenn Windows-Benutzer keine Konten für die ursprüngliche SQL-Instanz besitzen und eigenständigen Azure AD-Datenbankbenutzern zugeordnet werden müssen.
- In diesem Fall kann der zulässige Benutzername wie folgt lauten:
- Ein Widows-Benutzer (_Domäne\Benutzer_).
- Eine Windows-Gruppe (_MeineWindows-Gruppe_).
- Ein Windows-Alias (_MeinWindows-Alias_).
- Das Ergebnis des ALTER-Befehls ersetzt den alten Benutzernamen durch den entsprechenden Namen, der in Azure AD basierend auf der ursprünglichen SID des alten Benutzernamens gefunden wurde. Der geänderte Name wird ersetzt und in den Metadaten der Datenbank gespeichert:
- (_Domäne\Benutzer_) wird durch „Azure AD user@domain.com„ ersetzt.
- (_Domäne\\MeineWindows-Gruppe_) wird durch „Azure AD-Gruppe“ ersetzt.
- (_MeinWindows-Alias_) bleibt unverändert, aber die SID dieses Benutzers wird in Azure AD geprüft.

> [!NOTE]
> Wenn die in objectID konvertierte SID des ursprünglichen Benutzers in Azure AD nicht gefunden werden kann, tritt beim ALTER USER-Befehl ein Fehler auf.

- Verwenden Sie zum Anzeigen der geänderten Benutzer den folgenden Befehl: `select * from sys.database_principals`
- Überprüfen Sie, ob der angegebene Typ des Benutzers `E` oder `X` ist.
- Wenn NAME zum Migrieren von Windows-Benutzern zu Azure AD-Benutzern verwendet wird, gelten die folgenden Einschränkungen:
- Es muss ein gültiger Wert für LOGIN angegeben werden.
- NAME wird in Azure AD überprüft und kann nur folgenden Wert enthalten:
- Name von LOGIN.
- Alias: Der Name darf nicht in Azure AD vorhanden sein.
- In allen anderen Fällen schlägt die Syntax fehl.

## <a name="security"></a>Sicherheit

> [!NOTE]
> Ein Benutzer mit **ALTER ANY USER**-Berechtigung kann das Standardschema jedes Benutzers ändern. Ein Benutzer mit einem geänderten Schema könnte dann versehentlich Daten aus der falschen Tabelle auswählen oder Code aus dem falschen Schema ausführen.

### <a name="permissions"></a>Berechtigungen

 Zum Ändern des Namens eines Benutzers ist die **ALTER ANY USER**-Berechtigung erforderlich.

 Um die Zielanmeldung eines Benutzers zu ändern, wird die **CONTROL**-Berechtigung für die Datenbank benötigt.

 Um den Benutzernamen eines Benutzers zu ändern, der über die **CONTROL**-Berechtigung für die Datenbank verfügt, ist die **CONTROL**-Berechtigung für die Datenbank erforderlich.

 Zum Ändern des Standardschemas oder der Sprache ist die **ALTER**-Berechtigung für den Benutzer erforderlich. Benutzer können ihr eigenes Standardschema oder ihre eigene Sprache ändern.

## <a name="examples"></a>Beispiele

Alle Beispiele werden in einer Benutzerdatenbank ausgeführt.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Ändern des Namens eines Datenbankbenutzers

 Im folgenden Beispiel wird der Name des Datenbankbenutzers `Mary5` in `Mary51` geändert.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Ändern des Standardschemas eines Benutzers

 Im folgenden Beispiel wird das Standardschema des Benutzers `Mary51` in `Purchasing` geändert.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

### <a name="c-changing-several-options-at-once"></a>C. Gleichzeitiges Ändern mehrerer Optionen

 Im folgenden Beispiel werden mehrere Optionen für einen Benutzer einer eigenständigen Datenbank in einer Anweisung geändert.

```sql
ALTER USER Philip
WITH NAME = Philipe
, DEFAULT_SCHEMA = Development
, PASSWORD = 'W1r77TT98%ab@#' OLD_PASSWORD = 'New Devel0per'
, DEFAULT_LANGUAGE= French ;
GO
```

### <a name="d-map-the-user-in-the-database-to-an-azure-ad-login-after-migration"></a>D: Zuordnen des Benutzers in der Datenbank zu einem Azure AD-Anmeldenamen nach der Migration

Im folgenden Beispiel wird der Benutzer `westus/joe` einem Azure AD-Benutzer `joe@westus.com` neu zugeordnet. Dieses Beispiel gilt für Anmeldenamen, die bereits in der verwalteten Instanz vorhanden sind. Dies muss durchgeführt werden, nachdem Sie eine Datenbankmigration zu Azure SQL Managed Instance abgeschlossen haben und wenn Sie die Azure AD-Anmeldung für die Authentifizierung verwenden möchten.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com
```

### <a name="e-map-an-old-windows-user-in-the-database-without-a-login-in-azure-sql-managed-instance-to-an-azure-ad-user"></a>E. Zuordnen eines alten Windows-Benutzers in der Datenbank ohne eine Anmeldung zu einem Azure AD-Benutzer in Azure SQL Managed Instance

Im folgenden Beispiel wird der Benutzer `westus/joe` ohne Anmeldename einem Azure AD-Benutzer `joe@westus.com` neu zugeordnet. Der Verbundbenutzer muss in Azure AD vorhanden sein.

```sql
ALTER USER [westus/joe] FROM EXTERNAL PROVIDER
```

### <a name="f-map-the-user-alias-to-an-existing-azure-ad-login"></a>F. Zuordnen des Benutzeralias zu einem vorhandenen Azure AD-Anmeldenamen

Im folgenden Beispiel wird der Benutzername `westus\joe` dem Namen `joe_alias` neu zugeordnet. Der entsprechende Azure AD-Anmeldename in diesem Fall lautet `joe@westus.com`.

```sql
ALTER USER [westus/joe] WITH LOGIN = joe@westus.com, name= joe_alias
```

### <a name="g-map-a-windows-group-that-was-migrated-in-azure-sql-managed-instance-to-an-azure-ad-group"></a>G. Zuordnen einer Windows-Gruppe zu einer Azure AD-Gruppe nach einer Migration zu Azure SQL Managed Instance

Im folgenden Beispiel wird die alte lokale Gruppe `westus\mygroup` einer Azure AD-Gruppe `mygroup` in der verwalteten Instanz neu zugeordnet. Die Gruppe muss in Azure AD vorhanden sein.

```sql
ALTER USER [westus\mygroup] WITH LOGIN = mygroup
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)
- [Tutorial: Migrieren von lokalen SQL Server-Benutzern und -Gruppen unter Windows zu SQL Managed Instance mithilfe der DDL-Syntax von T-SQL](/azure/sql-database/tutorial-managed-instance-azure-active-directory-migration)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-user-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Azure Synapse

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Argumente

 *userName*: Gibt den Namen an, mit dem der Benutzer innerhalb dieser Datenbank identifiziert wird.

 LOGIN **=** _loginName_: Ordnet einen Benutzer einer anderen Anmeldung neu zu. Dazu wird die Sicherheits-ID (SID) des Benutzers in die SID der Anmeldung geändert.

 Wenn ALTER USER die einzige Anweisung in einem SQL-Batch ist, unterstützt Azure SQL-Datenbank die WITH LOGIN-Klausel. Wenn ALTER USER nicht die einzige Anweisung in einem SQL-Batch ist oder in dynamischem SQL-Code ausgeführt wird, wird die WITH LOGIN-Klausel nicht unterstützt.

 NAME **=** _newUserName_: Gibt den neuen Namen für diesen Benutzer an. *newUserName* darf in der aktuellen Datenbank noch nicht vorhanden sein.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL }: Gibt das erste Schema an, das vom Server beim Auflösen der Namen von Objekten für diesen Benutzer durchsucht wird. Wenn das Standardschema auf NULL festgelegt wird, wird ein Standardschema aus einer Windows-Gruppe entfernt. Die NULL-Option kann nicht in Verbindung mit einem Windows-Benutzer verwendet werden.

## <a name="remarks"></a>Bemerkungen

 Das Standardschema ist das erste Schema, das vom Server beim Auflösen der Namen von Objekten für diesen Datenbankbenutzer durchsucht wird. Wenn nicht anders angegeben, ist das Standardschema der Besitzer von Objekten, die von diesem Datenbankbenutzer erstellt werden.

 Wenn der Benutzer ein Standardschema hat, wird dieses Standardschema verwendet. Wenn der Benutzer kein Standardschema hat, aber Mitglied einer Gruppe mit einem Standardschema ist, wird das Standardschema der Gruppe verwendet. Wenn der Benutzer kein Standardschema hat und Mitglied von mehreren Gruppen ist, ist das Standardschema für den Benutzer das Schema der Windows-Gruppe mit der niedrigsten principal_id und explizit festgelegt. Wenn für einen Benutzer kein Standardschema bestimmt werden kann, wird das Schema **dbo** verwendet.

 DEFAULT_SCHEMA kann auf ein Schema festgelegt werden, das zurzeit nicht in der Datenbank vorhanden ist. Deshalb können Sie DEFAULT_SCHEMA einem Benutzer zuweisen, bevor das Schema erstellt wird.

 DEFAULT_SCHEMA kann nicht für einen Benutzer angegeben werden, der einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist.

> [!IMPORTANT]
> Der Wert von DEFAULT_SCHEMA wird ignoriert, wenn der Benutzer ein Mitglied der festen Serverrolle **sysadmin** ist. Alle Mitglieder der festen Serverrolle **sysadmin** verfügen über ein `dbo`-Standardschema .

 Mit der WITH LOGIN-Klausel kann ein Benutzer einer anderen Anmeldung neu zugeordnet werden. Benutzer ohne Anmeldenamen, einem Zertifikat zugeordnete Benutzer oder einem asymmetrischen Schlüssel zugeordnete Benutzer können mithilfe dieser Klausel nicht neu zugeordnet werden. Nur SQL-Benutzer und Windows-Benutzer (oder -Gruppen) können neu zugeordnet werden. Mit der WITH LOGIN-Klausel kann nicht der Benutzertyp geändert werden, beispielsweise kann ein Windows-Konto nicht in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen geändert werden.

 Der Name des Benutzers wird automatisch in den Anmeldenamen umbenannt, wenn die folgenden Voraussetzungen erfüllt sind.

- Es wurde kein neuer Name angegeben.

- Der aktuelle Name unterscheidet sich vom Benutzernamen.

 Andernfalls wird der Benutzer nicht umbenannt, es sei denn, der Aufrufer ruft zusätzlich die NAME-Klausel auf.

Der Name eines Benutzers, der einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen, einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist, kann keinen umgekehrten Schrägstrich (\\) enthalten.

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sicherheit

> [!NOTE]
> Ein Benutzer mit **ALTER ANY USER**-Berechtigung kann das Standardschema jedes Benutzers ändern. Ein Benutzer mit einem geänderten Schema könnte dann versehentlich Daten aus der falschen Tabelle auswählen oder Code aus dem falschen Schema ausführen.

### <a name="permissions"></a>Berechtigungen

 Zum Ändern des Namens eines Benutzers ist die **ALTER ANY USER**-Berechtigung erforderlich.

 Um die Zielanmeldung eines Benutzers zu ändern, wird die **CONTROL**-Berechtigung für die Datenbank benötigt.

 Um den Benutzernamen eines Benutzers zu ändern, der über die **CONTROL**-Berechtigung für die Datenbank verfügt, ist die **CONTROL**-Berechtigung für die Datenbank erforderlich.

 Zum Ändern des Standardschemas oder der Sprache ist die **ALTER**-Berechtigung für den Benutzer erforderlich. Benutzer können ihr eigenes Standardschema oder ihre eigene Sprache ändern.

## <a name="examples"></a>Beispiele

Alle Beispiele werden in einer Benutzerdatenbank ausgeführt.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Ändern des Namens eines Datenbankbenutzers

 Im folgenden Beispiel wird der Name des Datenbankbenutzers `Mary5` in `Mary51` geändert.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Ändern des Standardschemas eines Benutzers

Im folgenden Beispiel wird das Standardschema des Benutzers `Mary51` in `Purchasing` geändert.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-user-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-user-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-user-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-user-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Analyseplattformsystem

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Analytics Platform System

ALTER USER userName
 WITH <set_item> [ ,...n ]

<set_item> ::=
 NAME = newUserName
 | LOGIN = loginName
 | DEFAULT_SCHEMA = schema_name
[;]
```

## <a name="arguments"></a>Argumente

 *userName*: Gibt den Namen an, mit dem der Benutzer innerhalb dieser Datenbank identifiziert wird.

 LOGIN **=** _loginName_: Ordnet einen Benutzer einer anderen Anmeldung neu zu. Dazu wird die Sicherheits-ID (SID) des Benutzers in die SID der Anmeldung geändert.

 Wenn ALTER USER die einzige Anweisung in einem SQL-Batch ist, unterstützt Azure SQL-Datenbank die WITH LOGIN-Klausel. Wenn ALTER USER nicht die einzige Anweisung in einem SQL-Batch ist oder in dynamischem SQL-Code ausgeführt wird, wird die WITH LOGIN-Klausel nicht unterstützt.

 NAME **=** _newUserName_: Gibt den neuen Namen für diesen Benutzer an. *newUserName* darf in der aktuellen Datenbank noch nicht vorhanden sein.

 DEFAULT_SCHEMA **=** { *schemaName* | NULL }: Gibt das erste Schema an, das vom Server beim Auflösen der Namen von Objekten für diesen Benutzer durchsucht wird. Wenn das Standardschema auf NULL festgelegt wird, wird ein Standardschema aus einer Windows-Gruppe entfernt. Die NULL-Option kann nicht in Verbindung mit einem Windows-Benutzer verwendet werden.

## <a name="remarks"></a>Bemerkungen

 Das Standardschema ist das erste Schema, das vom Server beim Auflösen der Namen von Objekten für diesen Datenbankbenutzer durchsucht wird. Wenn nicht anders angegeben, ist das Standardschema der Besitzer von Objekten, die von diesem Datenbankbenutzer erstellt werden.

 Wenn der Benutzer ein Standardschema hat, wird dieses Standardschema verwendet. Wenn der Benutzer kein Standardschema hat, aber Mitglied einer Gruppe mit einem Standardschema ist, wird das Standardschema der Gruppe verwendet. Wenn der Benutzer kein Standardschema hat und Mitglied von mehreren Gruppen ist, ist das Standardschema für den Benutzer das Schema der Windows-Gruppe mit der niedrigsten principal_id und explizit festgelegt. Wenn für einen Benutzer kein Standardschema bestimmt werden kann, wird das Schema **dbo** verwendet.

 DEFAULT_SCHEMA kann auf ein Schema festgelegt werden, das zurzeit nicht in der Datenbank vorhanden ist. Deshalb können Sie DEFAULT_SCHEMA einem Benutzer zuweisen, bevor das Schema erstellt wird.

 DEFAULT_SCHEMA kann nicht für einen Benutzer angegeben werden, der einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist.

> [!IMPORTANT]
> Der Wert von DEFAULT_SCHEMA wird ignoriert, wenn der Benutzer ein Mitglied der festen Serverrolle **sysadmin** ist. Alle Mitglieder der festen Serverrolle **sysadmin** verfügen über ein `dbo`-Standardschema .

 Mit der WITH LOGIN-Klausel kann ein Benutzer einer anderen Anmeldung neu zugeordnet werden. Benutzer ohne Anmeldenamen, einem Zertifikat zugeordnete Benutzer oder einem asymmetrischen Schlüssel zugeordnete Benutzer können mithilfe dieser Klausel nicht neu zugeordnet werden. Nur SQL-Benutzer und Windows-Benutzer (oder -Gruppen) können neu zugeordnet werden. Mit der WITH LOGIN-Klausel kann nicht der Benutzertyp geändert werden, beispielsweise kann ein Windows-Konto nicht in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen geändert werden.

 Der Name des Benutzers wird automatisch in den Anmeldenamen umbenannt, wenn die folgenden Voraussetzungen erfüllt sind.

- Es wurde kein neuer Name angegeben.

- Der aktuelle Name unterscheidet sich vom Benutzernamen.

 Andernfalls wird der Benutzer nicht umbenannt, es sei denn, der Aufrufer ruft zusätzlich die NAME-Klausel auf.

Der Name eines Benutzers, der einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen, einem Zertifikat oder einem asymmetrischen Schlüssel zugeordnet ist, kann keinen umgekehrten Schrägstrich (\\) enthalten.

> [!CAUTION]
> [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]

## <a name="security"></a>Sicherheit

> [!NOTE]
> Ein Benutzer mit **ALTER ANY USER**-Berechtigung kann das Standardschema jedes Benutzers ändern. Ein Benutzer mit einem geänderten Schema könnte dann versehentlich Daten aus der falschen Tabelle auswählen oder Code aus dem falschen Schema ausführen.

### <a name="permissions"></a>Berechtigungen

 Zum Ändern des Namens eines Benutzers ist die **ALTER ANY USER**-Berechtigung erforderlich.

 Um die Zielanmeldung eines Benutzers zu ändern, wird die **CONTROL**-Berechtigung für die Datenbank benötigt.

 Um den Benutzernamen eines Benutzers zu ändern, der über die **CONTROL**-Berechtigung für die Datenbank verfügt, ist die **CONTROL**-Berechtigung für die Datenbank erforderlich.

 Zum Ändern des Standardschemas oder der Sprache ist die **ALTER**-Berechtigung für den Benutzer erforderlich. Benutzer können ihr eigenes Standardschema oder ihre eigene Sprache ändern.

## <a name="examples"></a>Beispiele

Alle Beispiele werden in einer Benutzerdatenbank ausgeführt.

### <a name="a-changing-the-name-of-a-database-user"></a>A. Ändern des Namens eines Datenbankbenutzers

 Im folgenden Beispiel wird der Name des Datenbankbenutzers `Mary5` in `Mary51` geändert.

```sql
ALTER USER Mary5 WITH NAME = Mary51;
GO
```

### <a name="b-changing-the-default-schema-of-a-user"></a>B. Ändern des Standardschemas eines Benutzers
 Im folgenden Beispiel wird das Standardschema des Benutzers `Mary51` in `Purchasing` geändert.

```sql
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)
- [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)
- [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)

::: moniker-end
