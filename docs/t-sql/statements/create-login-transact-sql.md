---
description: CREATE LOGIN (Transact-SQL)
title: CREATE LOGIN (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/29/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_LOGIN_TSQL
- CREATE LOGIN
- LOGIN_TSQL
- LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- passwords [SQL Server], logins
- mapping logins [SQL Server]
- logins [SQL Server], creating
- CREATE LOGIN statement
- permissions [SQL Server], logins
- Windows domain accounts [SQL Server]
- re-hashing passwords
- certificates [SQL Server], logins
ms.assetid: eb737149-7c92-4552-946b-91085d8b1b01
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2fe84db5d96e21f9d6bf107f87624de4d5953b89
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643971"
---
# <a name="create-login-transact-sql"></a>CREATE LOGIN (Transact-SQL)

Erstellt eine Anmeldung für SQL Server, SQL-Datenbank, Azure Synapse Analytics oder Analytics Platform System-Datenbanken. Klicken Sie auf eine der folgenden Registerkarten, um Syntax, Argumente, Hinweise, Berechtigungen und Beispiele für eine bestimmte Version anzuzeigen.

CREATE LOGIN nimmt an Transaktionen teil. Wenn CREATE LOGIN innerhalb einer Transaktion ausgeführt wird, für die dann ein Rollback ausgeführt wird, wird ein Rollback für die Erstellung von Anmeldedaten ausgeführt. Die erstellten Anmeldedaten können erst verwendet werden, nachdem die Transaktion committet wurde, wenn sie innerhalb einer Transaktion ausgeführt werden.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure SQL-Datenbank](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL<br />Managed Instance](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for SQL Server
CREATE LOGIN login_name { WITH <option_list1> | FROM <sources> }

<option_list1> ::=
    PASSWORD = { 'password' | hashed_password HASHED } [ MUST_CHANGE ]
    [ , <option_list2> [ ,... ] ]

<option_list2> ::=
    SID = sid
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
    | CHECK_EXPIRATION = { ON | OFF}
    | CHECK_POLICY = { ON | OFF}
    | CREDENTIAL = credential_name

<sources> ::=
    WINDOWS [ WITH <windows_options>[ ,... ] ]
    | CERTIFICATE certname
    | ASYMMETRIC KEY asym_key_name
  
<windows_options> ::=
    DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Argumente

*login_name* Gibt den Anmeldenamen an, der erstellt wird. Es gibt vier Arten von Anmeldenamen: SQL Server-Anmeldungen, Windows-Anmeldenamen, Anmeldenamen mit zugeordneten Zertifikaten sowie Anmeldenamen mit zugeordneten asymmetrischen Schlüsseln. Wenn Sie Anmeldenamen erstellen, die von einem Windows-Domänenkonto zugeordnet werden, müssen Sie den Benutzeranmeldenamen bei Versionen vor Windows 2000 im Format [\<domainName>\\<login_name>] verwenden. Sie können keine UPN im Format login_name@DomainName verwenden. Ein Beispiel hierzu finden Sie unter „Beispiel D“ weiter unten in diesem Artikel. Anmeldenamen für die Authentifizierung sind vom Typ **sysname**. Sie müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen und dürfen keinen „ **\\** “ enthalten. Windows-Anmeldenamen können ' **\\** '-Zeichen enthalten. Anmeldenamen auf Grundlage von Active Directory-Benutzern sind auf Namen mit weniger als 21 Zeichen beschränkt.

PASSWORD **=** '*Kennwort*' Gilt nur für SQL Server-Anmeldungen. Gibt das Kennwort für den Anmeldenamen an, der erstellt wird. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md) und [Kennwortrichtlinie](../../relational-databases/security/password-policy.md). Ab SQL Server 2012 (11.x) werden gespeicherte Kennwortinformationen mithilfe der SHA-512-Komponente des mit Salt verschlüsselten Kennworts berechnet.

Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Kennwörter sollten immer mindestens acht Zeichen lang sein und dürfen 128 Zeichen nicht überschreiten. Kennwörter dürfen a-z, A-Z, 0-9 und die meisten nicht alphanumerischen Zeichen einschließen. Kennwörter dürfen keine einfachen Anführungszeichen oder *login_name* enthalten.

PASSWORD **=** *hashed\_password*: Gilt nur für das HASHED-Schlüsselwort. Gibt den Hashwert des Kennworts für den Anmeldenamen an, der erstellt wird.

HASHED Gilt nur für SQL Server-Anmeldenamen. Gibt an, dass das nach dem PASSWORD-Argument eingegebene Kennwort bereits einen Hashwert darstellt. Wenn diese Option nicht ausgewählt wird, wird aus der als Kennwort eingegebenen Zeichenfolge vor dem Speichern in der Datenbank ein Hashwert erstellt. Diese Option sollte nur verwendet werden, um Datenbanken von einem Server auf einen anderen zu migrieren. Verwenden Sie die HASHED-Option nicht, um neue Anmeldenamen zu erstellen. Die HASHED-Option kann nicht mit Hashes verwendet werden, die in SQL 7 oder einer früheren Version erstellt wurden.

MUST_CHANGE Gilt nur für SQL Server-Anmeldenamen. Wenn diese Option angegeben wird, wird der Benutzer von SQL Server zur Eingabe eines neuen Kennworts aufgefordert, wenn der neue Anmeldename zum ersten Mal verwendet wird.

CREDENTIAL **=** _credential\_name_ Die Anmeldeinformationen, die dem neuen SQL Server-Anmeldenamen zugeordnet werden sollen. Die Anmeldeinformationen müssen bereits auf dem Server vorhanden sein. Momentan verknüpft diese Option nur die Anmeldeeinformationen mit einem Anmeldenamen. Der Anmeldung eines Systemadministrators können keine Anmeldeinformationen zugeordnet werden.

SID = *sid* Wird verwendet, um eine Anmeldung neu zu erstellen. Gilt nur für Anmeldenamen für die SQL Server-Authentifizierung, nicht für Anmeldenamen für die Windows-Authentifizierung. Gibt die SID des neuen Anmeldenamens für die SQL Server-Authentifizierung an. Wenn diese Option nicht verwendet wird, weist SQL Server automatisch eine SID zu. Die SID-Struktur hängt von der SQL Server-Version ab. SID des Anmeldenamens für SQL Server: ein 16-Byte-Literalwert (**binary(16)** ) basierend auf einer GUID. Beispiel: `SID = 0x14585E90117152449347750164BA00A7`.

DEFAULT_DATABASE **=** _database_ Gibt die Standarddatenbank an, die dem Anmeldenamen zugewiesen werden soll. Wenn diese Option nicht enthalten ist, wird die Standarddatenbank als „master“ festgelegt.

DEFAULT_LANGUAGE **=** _language_ Gibt die Standardsprache an, die dem Anmeldenamen zugewiesen werden soll. Wenn diese Option nicht enthalten ist, wird die aktuelle Standardsprache des Servers als Standardsprache festgelegt. Wenn die Standardsprache des Servers zu einem späteren Zeitpunkt geändert wird, wird die Standardsprache des Anmeldenamens nicht geändert.

CHECK_EXPIRATION **=** { ON | **OFF** } Gilt nur für SQL Server-Anmeldenamen. Gibt an, ob die Richtlinie für das Ablaufen von Kennwörtern für diesen Anmeldenamen erzwungen werden soll. Der Standardwert ist OFF.

CHECK_POLICY **=** { **ON** | OFF } Gilt nur für SQL Server-Anmeldenamen. Gibt an, dass die Windows-Kennwortrichtlinien des Computers, auf dem SQL Server ausgeführt wird, für diesen Anmeldenamen erzwungen werden sollen. Der Standardwert ist ON.

Wenn die Windows-Richtlinie sichere Kennwörter erfordert, müssen Kennwörter mindestens drei der folgenden vier Eigenschaften aufweisen:

- Ein Großbuchstabe (A-Z).
- Ein Kleinbuchstabe (a-z).
- Eine Ziffer (0-9).
- Ein nicht alphanumerisches Zeichen, z. B. ein Leerzeichen, _, @, *, ^,%, !, $, # oder &.

WINDOWS Gibt an, dass der Anmeldename einem Windows-Anmeldenamen zugeordnet wird.

CERTIFICATE *certname* Gibt den Namen eines Zertifikats an, das diesem Anmeldenamen zugeordnet werden soll. Dieses Zertifikat muss bereits in der Masterdatenbank vorhanden sein.

ASYMMETRIC KEY *asym_key_name* Gibt den Namen eines asymmetrischen Schlüssels an, der diesem Anmeldenamen zugeordnet werden soll. Dieser Schlüssel muss bereits in der Masterdatenbank vorhanden sein.

## <a name="remarks"></a>Bemerkungen

- Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.
- Das vorherige Erstellen von Hashwerten für Kennwörter wird nur unterstützt, wenn Sie SQL Server-Anmeldenamen erstellen.
- Falls MUST_CHANGE angegeben wird, müssen CHECK_EXPIRATION und CHECK_POLICY auf ON festgelegt werden. Andernfalls erzeugt die Anweisung einen Fehler.
- Die Kombination aus CHECK_POLICY = OFF und CHECK_EXPIRATION = ON wird nicht unterstützt.
- Wenn CHECK_POLICY auf OFF festgelegt ist, wird *lockout_time* zurückgesetzt, und CHECK_EXPIRATION wird auf OFF festgelegt.

> [!IMPORTANT]
> CHECK_EXPIRATION und CHECK_POLICY werden nur unter Windows Server 2003 und höher erzwungen. Weitere Informationen finden Sie unter [Password Policy](../../relational-databases/security/password-policy.md).

- Aus Zertifikaten oder asymmetrischen Schlüsseln erstellte Anmeldenamen werden nur zum Signieren von Code verwendet. Sie können nicht verwendet werden, um eine Verbindung mit SQL Server herzustellen. Sie können einen Anmeldenamen anhand eines Zertifikats oder eines asymmetrischen Schlüssels nur erstellen, wenn das Zertifikat oder der asymmetrische Schlüssel bereits in der Masterdatenbank vorhanden ist.
- Ein Skript zum Übertragen von Anmeldenamen finden unter [Übertragen von Benutzernamen und Kennwörtern zwischen Instanzen von SQL Server](https://support.microsoft.com/kb/918992).
- Durch das Erstellen eines Anmeldenamens wird der neue Anmeldename automatisch aktiviert, und diesem Anmeldenamen wird die **CONNECT SQL**-Berechtigung auf Serverebene gewährt.
- Der [Authentifizierungsmodus](../../relational-databases/security/choose-an-authentication-mode.md) muss mit dem Anmeldetyp übereinstimmen, damit der Zugriff gewährt wird.
- Informationen zum Entwerfen eines Berechtigungssystems finden Sie unter [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).

## <a name="permissions"></a>Berechtigungen

- Nur Benutzer mit **ALTER ANY LOGIN**-Berechtigung auf dem Server oder einer Mitgliedschaft bei der festen Serverrolle **securityadmin** können Anmeldenamen erstellen. Weitere Informationen finden Sie unter [Rollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#groups-and-roles) und unter [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).
- Falls die Option **CREDENTIAL** verwendet wird, ist auch die **ALTER ANY CREDENTIAL**-Berechtigung auf dem Server erforderlich.

## <a name="after-creating-a-login"></a>Nach dem Erstellen eines Anmeldenamens

Nach dem Erstellen eines Anmeldenamens kann mit dem Namen eine Verbindung mit SQL Server hergestellt werden, er verfügt jedoch nur über die der Rolle **public** gewährten Berechtigungen. Ziehen Sie die Ausführung einiger der folgenden Aktivitäten in Betracht.

- Erstellen Sie zum Herstellen einer Verbindung mit einer Datenbank einen Datenbankbenutzer für den Anmeldenamen. Weitere Informationen finden Sie unter [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md).
- Erstellen sie eine benutzerdefinierte Serverrolle mit [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md). Verwenden Sie **ALTER SERVER ROLE** ... **ADD MEMBER**, um der benutzerdefinierten Serverrolle den neuen Anmeldenamen hinzuzufügen. Weitere Informationen finden Sie unter [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) und unter [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).
- Fügen Sie den Anmeldenamen mit **sp_addsrvrolemember** einer festen Serverrolle hinzu. Weitere Informationen finden Sie unter [Rollen auf Serverebene](../../relational-databases/security/authentication-access/server-level-roles.md) und unter [sp_addsrvrolemember](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).
- Gewähren Sie mit der **GRANT**-Anweisung Berechtigungen auf Serverebene für den neuen Anmeldenamen oder für eine Rolle, die den Anmeldenamen enthält. Weitere Informationen finden Sie unter [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-login-with-a-password"></a>A. Erstellen einer Anmeldung mit einem Kennwort

Im folgenden Beispiel wird ein Anmeldename für einen bestimmten Benutzer erstellt, und es wird ein Kennwort zugewiesen.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-with-a-password-that-must-be-changed"></a>B. Erstellen einer Anmeldung mit einem zu ändernden Kennwort

Im folgenden Beispiel wird ein Anmeldename für einen bestimmten Benutzer erstellt, und es wird ein Kennwort zugewiesen. Die Option `MUST_CHANGE` erfordert, dass Benutzer dieses Kennwort ändern, wenn sie das erste Mal eine Verbindung mit dem Server herstellen.

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'
    MUST_CHANGE, CHECK_EXPIRATION = ON;
GO
```

> [!NOTE]
> Die Option MUST_CHANGE kann nicht verwendet werden, wenn CHECK_EXPIRATION auf OFF festgelegt ist.

### <a name="c-creating-a-login-mapped-to-a-credential"></a>C. Erstellen eines Anmeldeinformationen zugeordneten Anmeldenamens

Im folgenden Beispiel wird unter Verwendung des Benutzers der Anmeldename für einen bestimmten Benutzer erstellt. Dieser Anmeldename wird den Anmeldeinformationen zugeordnet.

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>',
    CREDENTIAL = <credentialName>;
GO
```

### <a name="d-creating-a-login-from-a-certificate"></a>D: Erstellen eines Anmeldenamens aus einem Zertifikat

Im folgenden Beispiel wird der Anmeldename für eine bestimmte Benutzer-ID von einem Zertifikat in der Masterdatenbank erstellt.

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

```sql
USE MASTER;
CREATE CERTIFICATE <certificateName>
    WITH SUBJECT = '<login_name> certificate in master database',
    EXPIRY_DATE = '12/05/2025';
GO
CREATE LOGIN <login_name> FROM CERTIFICATE <certificateName>;
GO
```

### <a name="e-creating-a-login-from-a-windows-domain-account"></a>E. Erstellen eines Anmeldenamens von einem Windows-Domänenkonto

Im folgenden Beispiel wird ein Anmeldename von einem Windows-Domänenkonto erstellt.

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

```sql
CREATE LOGIN [<domainName>\<login_name>] FROM WINDOWS;
GO
```

### <a name="f-creating-a-login-from-a-sid"></a>F. Erstellen eines Anmeldenamens aus einer SID

Im folgenden Beispiel wird ein Anmeldename für die SQL Server-Authentifizierung erstellt und die SID des Anmeldenamens festgelegt.

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';
SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

Die Abfrage hier gibt 0x241C11948AEEB749B0D22646DB1A19F2 als SID zurück. Die Abfrage gibt einen anderen Wert zurück. Die folgenden Anweisungen löschen den Anmeldenamen und erstellen dann erneut die Anmeldung. Verwenden Sie die SID aus der vorherigen Abfrage.

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

### <a name="g-creating-a-login-with-multiple-arguments"></a>G. Erstellen einer Anmeldung mit mehreren Argumenten

Im folgenden Beispiel sehen Sie, wie Sie mehrere Argumente mit Kommas zwischen den einzelnen Argumenten als eine Zeichenfolge verbinden.

```sql
CREATE LOGIN [MyUser]
WITH PASSWORD = 'MyPassword',
DEFAULT_DATABASE = MyDatabase,
CHECK_POLICY = OFF,
CHECK_EXPIRATION = OFF ;
```

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Kennwortrichtlinie](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Erstellen eines Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure SQL-Datenbank \*_**
    :::column-end:::
    :::column:::
        [Azure SQL<br />Managed Instance](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL-Datenbank

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Azure SQL Database
CREATE LOGIN login_name
 { WITH <option_list> }

<option_list> ::=
    PASSWORD = { 'password' }
    [ , SID = sid ]
```

## <a name="arguments"></a>Argumente

*login_name* Gibt den Anmeldenamen an, der erstellt wird. Einzelne und Pooldatenbanken in Azure SQL-Datenbank und Datenbanken in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] unterstützen nur SQL-Anmeldungen. Verwenden Sie die [CREATE USER](create-user-transact-sql.md)-Anweisung, um Konten für Azure Active Directory-Benutzer oder Benutzeranmeldungen, die keiner Anmeldung zugewiesen sind, zu erstellen. Weitere Informationen finden Sie unter [Verwalten von Anmeldungen in Azure SQL-Datenbank](/azure/sql-database/sql-database-manage-logins).

PASSWORD **='** password* *'* Gibt das Kennwort für den SQL-Anmeldenamen an, der erstellt wird. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md) und [Kennwortrichtlinie](../../relational-databases/security/password-policy.md). Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] werden gespeicherte Kennwortinformationen mithilfe der SHA-512-Komponente des mit Salt verschlüsselten Kennworts berechnet.

Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Kennwörter sollten immer mindestens acht Zeichen lang sein und dürfen 128 Zeichen nicht überschreiten. Kennwörter dürfen a-z, A-Z, 0-9 und die meisten nicht alphanumerischen Zeichen einschließen. Kennwörter dürfen keine einfachen Anführungszeichen oder *login_name* enthalten.

SID = *sid* Wird verwendet, um eine Anmeldung neu zu erstellen. Gilt nur für Anmeldenamen für die SQL Server-Authentifizierung, nicht für Anmeldenamen für die Windows-Authentifizierung. Gibt die SID des neuen Anmeldenamens für die SQL Server-Authentifizierung an. Wenn diese Option nicht verwendet wird, weist SQL Server automatisch eine SID zu. Die SID-Struktur hängt von der SQL Server-Version ab. Bei SQL-Datenbank ist dies normalerweise ein 32-Byte-Literalwert (**binary(32)** ) bestehend aus `0x01060000000000640000000000000000` plus 16 Byte, die eine GUID darstellen. Beispiel: `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.

## <a name="remarks"></a>Bemerkungen

- Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.
- Durch das Erstellen eines Anmeldenamens wird der neue Anmeldename automatisch aktiviert, und diesem Anmeldenamen wird die **CONNECT SQL**-Berechtigung auf Serverebene gewährt.

> [!IMPORTANT]
> Unter [Verwalten von Anmeldungen in Azure SQL-Datenbank](/azure/sql-database/sql-database-manage-logins) finden Sie weitere Informationen zum Arbeiten mit Anmeldungen und Benutzern in Azure SQL-Datenbank.

## <a name="login"></a>Anmeldename

### <a name="sql-database-logins"></a>Anmeldenamen für SQL-Datenbanken

Die **CREATE LOGIN**-Anweisung muss die einzige Anweisung in einem Batch sein.

Bei einigen Methoden zum Herstellen einer Verbindung mit SQL-Datenbank, z. B. **sqlcmd**, müssen Sie den SQL-Datenbank-Servernamen mit der Notation *\<login>* @ *\<server>* an den Anmeldenamen in der Verbindungszeichenfolge anfügen. Wenn Ihr Anmeldename beispielsweise `login1` und der vollqualifizierte Name des Servers von SQL-Datenbank `servername.database.windows.net` lautet, sollte der Parameter *username* der Verbindungszeichenfolge `login1@servername` lauten. Da die Gesamtlänge des *username*-Parameters 128 Zeichen beträgt, ist *login_name* auf 127 Zeichen abzüglich der Länge des Servernamens beschränkt. Im Beispiel darf `login_name` nur 117 Zeichen lang sein, da `servername` 10 Zeichen enthält.

In SQL-Datenbank müssen Sie mit der Masterdatenbank verbunden sein und über die entsprechenden Berechtigungen verfügen, um eine Anmeldung zu erstellen. Weitere Informationen finden Sie unter [Erstellen zusätzlicher Anmeldungen und Benutzer mit Verwaltungsberechtigungen](/azure/sql-database/sql-database-manage-logins#create-additional-logins-and-users-having-administrative-permissions).

Mithilfe von SQL Server-Regeln können Sie einen Anmeldenamen für die SQL Server-Authentifizierung im Format \<loginname>@\<servername> erstellen. Wenn der [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Servername **myazureserver** und der Anmeldename **myemail@live.com** lauten, müssen Sie den Anmeldenamen als **myemail@live.com@myazureserver** bereitstellen.

In SQL-Datenbank werden Anmeldedaten, die für die Authentifizierung einer Verbindung und für Firewallregeln auf Serverebene erforderlich sind, über einen gewissen Zeitraum in jeder Datenbank gespeichert. Dieser Cache wird regelmäßig aktualisiert. Führen Sie [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md) aus, um eine Aktualisierung des Authentifizierungscache zu erzwingen und sicherzustellen, dass eine Datenbank über die aktuelle Version der Tabelle mit Anmeldenamen verfügt.

## <a name="permissions"></a>Berechtigungen

Neue Anmeldenamen können nur mit den Anmeldenamen des Serverebenenprinzipals (im Bereitstellungsprozess erstellt) oder von Mitgliedern der `loginmanager`-Datenbankrolle in der Masterdatenbank erstellt werden. Weitere Informationen finden Sie unter [Erstellen zusätzlicher Anmeldungen und Benutzer mit Verwaltungsberechtigungen](/azure/sql-database/sql-database-manage-logins#create-additional-logins-and-users-having-administrative-permissions).

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-login-with-a-password"></a>A. Erstellen einer Anmeldung mit einem Kennwort

Im folgenden Beispiel wird ein Anmeldename für einen bestimmten Benutzer erstellt, und es wird ein Kennwort zugewiesen.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-from-a-sid"></a>B. Erstellen eines Anmeldenamens aus einer SID

Im folgenden Beispiel wird ein Anmeldename für die SQL Server-Authentifizierung erstellt und die SID des Anmeldenamens festgelegt.

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

Die Abfrage hier gibt 0x241C11948AEEB749B0D22646DB1A19F2 als SID zurück. Die Abfrage gibt einen anderen Wert zurück. Die folgenden Anweisungen löschen den Anmeldenamen und erstellen dann erneut die Anmeldung. Verwenden Sie die SID aus der vorherigen Abfrage.

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Kennwortrichtlinie](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Erstellen eines Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL-Datenbank](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\*Azure SQL<br />Managed Instance\*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Azure SQL Managed Instance
CREATE LOGIN login_name [FROM EXTERNAL PROVIDER] { WITH <option_list> [,..]}

<option_list> ::=
    PASSWORD = {'password'}
    | SID = sid
    | DEFAULT_DATABASE = database
    | DEFAULT_LANGUAGE = language
```

## <a name="arguments"></a>Argumente

*login_name* Bei Verwendung mit der Klausel **FROM EXTERNAL PROVIDER** legt der Anmeldename den Azure AD-Prinzipal fest, bei dem es sich um einen Azure AD-Benutzer, eine -Gruppe oder eine -Anwendung handelt. Andernfalls stellt der Anmeldename den Namen der erstellten SQL Server-Anmeldung dar.

FROM EXTERNAL PROVIDER </br>
Legt fest, dass der Anmeldename zur Azure AD-Authentifizierung dient.

PASSWORD **=** '*password*' Gibt das Kennwort für den SQL-Anmeldenamen an, der erstellt wird. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md) und [Kennwortrichtlinie](../../relational-databases/security/password-policy.md). Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] werden gespeicherte Kennwortinformationen mithilfe der SHA-512-Komponente des mit Salt verschlüsselten Kennworts berechnet.

Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Kennwörter sollten immer mindestens 10 Zeichen lang sein und dürfen 128 Zeichen nicht überschreiten. Kennwörter dürfen a-z, A-Z, 0-9 und die meisten nicht alphanumerischen Zeichen einschließen. Kennwörter dürfen keine einfachen Anführungszeichen oder *login_name* enthalten.

SID **=** *sid*: Wird verwendet, um eine Anmeldung neu zu erstellen. Gilt nur für SQL Server-Authentifizierungsanmeldungen. Gibt die SID des neuen Anmeldenamens für die SQL Server-Authentifizierung an. Wenn diese Option nicht verwendet wird, weist SQL Server automatisch eine SID zu. Die SID-Struktur hängt von der SQL Server-Version ab. Bei SQL-Datenbank ist dies normalerweise ein 32-Byte-Literalwert (**binary(32)** ) bestehend aus `0x01060000000000640000000000000000` plus 16 Byte, die eine GUID darstellen. Beispiel: `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.

## <a name="remarks"></a>Bemerkungen

- Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.
- Eine neue Syntax für die Erstellung von Serverebenenprinzipalen für Azure AD-Konten wurde eingeführt (**FROM EXTERNAL PROVIDER**).
- Wenn **FROM EXTERNAL PROVIDER** festgelegt wird:

  - „login_name“ muss einem vorhandenen Azure AD-Konto (Benutzer, Gruppe oder Anwendung) entsprechen, auf das in Azure AD über die aktuelle verwaltete Azure SQL-Datenbank-Instanz zugegriffen werden kann. Für Azure AD-Prinzipale erfordert die CREATE LOGIN-Syntax:
    - „UserPrincipalName“ des Azure AD-Objekts für Azure AD-Benutzer.
    - „DisplayName“ des Azure AD-Objekts für Azure AD-Gruppen und Azure AD-Anwendungen.
  - Die Option **PASSWORD** kann nicht verwendet werden.
- Wenn die Klausel **FROM EXTERNAL PROVIDER** ausgelassen wird, wird standardmäßig eine SQL Server-Anmeldung erstellt.
- Azure AD-Anmeldenamen werden in „sys.server_principals“ aufgeführt. Für Anmeldenamen, die Azure AD-Benutzern zugeordnet sind, ist der Wert der Typspalte auf **E** und „type_desc“ ist auf **EXTERNAL_LOGIN** festgelegt. Für Anmeldenamen, die Azure AD-Gruppen zugeordnet sind, ist der Wert der Typspalte auf **X** und „type_desc“ ist auf **EXTERNAL_GROUP** festgelegt.
- Ein Skript zum Übertragen von Anmeldenamen finden unter [Übertragen von Benutzernamen und Kennwörtern zwischen Instanzen von SQL Server](https://support.microsoft.com/kb/918992).
- Durch das Erstellen eines Anmeldenamens wird der neue Anmeldename automatisch aktiviert, und diesem Anmeldenamen wird die **CONNECT SQL**-Berechtigung auf Serverebene gewährt.

> [!IMPORTANT]
> Unter [Verwalten von Anmeldungen in Azure SQL-Datenbank](/azure/sql-database/sql-database-manage-logins) finden Sie weitere Informationen zum Arbeiten mit Anmeldungen und Benutzern in Azure SQL-Datenbank.

## <a name="logins-and-permissions"></a>Anmeldenamen und Berechtigungen

Neue Anmeldenamen können nur mit den Anmeldenamen des Serverebenenprinzipals (im Bereitstellungsprozess erstellt) oder von Mitgliedern der Datenbankrollen `securityadmin` oder `sysadmin` in der Masterdatenbank erstellt werden. Weitere Informationen finden Sie unter [Rollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#groups-and-roles) und unter [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).

Die folgenden Standardberechtigungen werden neu erstellten Azure AD-Anmeldenamen in der Masterdatenbank gewährt: **CONNECT SQL** und **VIEW ANY DATABASE**.

### <a name="sql-managed-instance-logins"></a>SQL Managed Instance-Anmeldungen

- Erfordert eine **ALTER ANY LOGIN**-Berechtigung auf dem Server oder eine Mitgliedschaft bei einer der festen Serverrollen `securityadmin` oder `sysadmin`. Dieser Befehl kann nur mit einem Azure AD-Konto (Azure Active Directory) mit der **ALTER ANY LOGIN**-Berechtigung auf dem Server oder mit einer Mitgliedschaft bei einer dieser Serverrollen ausgeführt werden.
- Wenn es sich bei der Anmeldung um einen SQL-Prinzipal handelt, können nur Anmeldungen, die der Rolle `sysadmin` angehören, den Befehl „create“ verwenden, um Anmeldungen für ein Azure AD-Konto zu erstellen.
- Muss im gleichen Verzeichnis, das auch für die verwaltete Azure SQL-Datenbank-Instanz verwendet wird, ein Azure AD-Mitglied sein.

## <a name="after-creating-a-login"></a>Nach dem Erstellen eines Anmeldenamens

> [!NOTE]
> Der Azure AD-Administrator für Azure SQL Managed Instance-Funktionen ändert sich nach der Erstellung. Weitere Informationen finden Sie unter [Neue Azure AD-Administratorfunktionen für verwaltete Instanzen](/azure/sql-database/sql-database-aad-authentication-configure#new-azure-ad-admin-functionality-for-mi).

Nach dem Erstellen einer Anmeldung kann diese zum Herstellen einer Verbindung mit einer verwalteten Instanz verwendet werden. Der Anmeldung werden jedoch nur die Berechtigungen gewährt, die der Rolle **public** zugewiesen sind. Ziehen Sie die Ausführung einiger der folgenden Aktivitäten in Betracht.

- Informationen zum Erstellen eines Azure AD-Benutzers aus Azure AD-Anmeldeinformationen finden Sie unter [CREATE USER](../../t-sql/statements/create-user-transact-sql.md).
- Verwenden Sie für die Erteilung von Berechtigungen für einen Benutzer in einer Datenbank die Anweisung **ALTER SERVER ROLE** ... **ADD MEMBER**-Anweisung zum Hinzufügen des Benutzer zu einer der integrierten Datenbankrollen oder einer benutzerdefinierten Rolle oder zum Erteilen von Berechtigungen für den Benutzer über die direkte Verwendung der Anweisung [GRANT](../../t-sql/statements/grant-transact-sql.md). Weitere Informationen finden Sie unter [Benutzer ohne Administratorrechte](/azure/sql-database/sql-database-manage-logins#non-administrator-users) und in den Artikeln zu den Anweisungen [Zusätzliche Administratorrollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)- und [GRANT](grant-transact-sql.md)-Anweisung.
- Wenn Sie serverweite Berechtigungen erteilen möchten, erstellen Sie in der Masterdatenbank einen Datenbankbenutzer, und verwenden Sie die Anweisung **ALTER SERVER ROLE** ... **ADD MEMBER**-Anweisung zum Hinzufügen des Benutzers zu einer der administrativen Serverrollen. Weitere Informationen finden Sie unter [Rollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#groups-and-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) und unter [Serverrollen](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles).
  - Verwenden Sie den folgenden Befehl, um einem Azure AD-Anmeldenamen die Rolle `sysadmin` zu gewähren: `ALTER SERVER ROLE sysadmin ADD MEMBER [AzureAD_Login_name]`
- Gewähren Sie mit der **GRANT**-Anweisung Berechtigungen auf Serverebene für den neuen Anmeldenamen oder für eine Rolle, die den Anmeldenamen enthält. Weitere Informationen finden Sie unter [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="limitations"></a>Einschränkungen

- Das Festlegen eines Azure AD-Anmeldenamens, der einer Azure AD-Gruppe zugeordnet ist, als Besitzer der Datenbank wird nicht unterstützt.
- Identitätswechsel von Azure AD-Prinzipalen auf Serverebene mit anderen Azure AD-Prinzipalen werden unterstützt, z.B. in der [EXECUTE AS](execute-as-transact-sql.md)-Klausel.
- Nur SQL-Serverebenenprinzipale (Anmeldungen), die Teil der `sysadmin`-Rolle sind, können die folgenden Vorgänge für Azure AD-Prinzipale ausführen:
  - EXECUTE AS USER
  - EXECUTE AS LOGIN
- Aus einem anderen Azure AD-Verzeichnis importierte externe Benutzer (Gastbenutzer) können nicht direkt als Azure AD-Administratoren für Azure SQL Managed Instance mithilfe des Azure-Portals konfiguriert werden. Binden Sie die externen Benutzer stattdessen in eine Gruppe mit Azure AD-Sicherheit ein, und konfigurieren Sie die Gruppe als Instanzadministrator. Sie können PowerShell oder die Azure CLI verwenden, um Einzelgastbenutzer als Instanzadministrator festzulegen.
- Anmeldenamen werden nicht auf die sekundäre Instanz in einer Failovergruppe repliziert. Sie werden in der Masterdatenbank gespeichert, bei der es sich um eine Systemdatenbank handelt. Sie wird daher nicht georepliziert. Der Benutzer muss einen Anmeldenamen mit derselben SID auf der sekundären Instanz erstellen, um dieses Problem zu beheben.

```SQL
-- Code to create login on the secondary instance
CREATE LOGIN foo WITH PASSWORD = '<enterStrongPasswordHere>', SID = <login_sid>;
```

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-login-with-a-password"></a>A. Erstellen einer Anmeldung mit einem Kennwort

Im folgenden Beispiel wird ein Anmeldename für einen bestimmten Benutzer erstellt, und es wird ein Kennwort zugewiesen.

 ```sql
 CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
 GO
 ```

### <a name="b-creating-a-login-from-a-sid"></a>B. Erstellen eines Anmeldenamens aus einer SID

 Im folgenden Beispiel wird ein Anmeldename für die SQL Server-Authentifizierung erstellt und die SID des Anmeldenamens festgelegt.

 ```sql
 CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

 SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
 GO
 ```

Die Abfrage hier gibt 0x241C11948AEEB749B0D22646DB1A19F2 als SID zurück. Die Abfrage gibt einen anderen Wert zurück. Die folgenden Anweisungen löschen den Anmeldenamen und erstellen dann erneut die Anmeldung. Verwenden Sie die SID aus der vorherigen Abfrage.

 ```sql
 DROP LOGIN TestLogin;
 GO

 CREATE LOGIN TestLogin
 WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

 SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
 GO
 ```

### <a name="c-creating-a-login-for-a-local-azure-ad-account"></a>C. Erstellen eines Anmeldenamens für ein lokales Azure AD-Konto

 Im folgenden Beispiel wird ein Anmeldename für das Azure AD-Konto joe@myaad.onmicrosoft.com erstellt, das in der Azure AD-Instanz *myaad* vorhanden ist.

```sql
CREATE LOGIN [joe@myaad.onmicrosoft.com] FROM EXTERNAL PROVIDER
GO
```

### <a name="d-creating-a-login-for-a-federated-azure-ad-account"></a>D: Erstellen eines Anmeldenamens für ein lokales Azure AD-Verbundkonto

 Im folgenden Beispiel wird ein Anmeldename für das Azure AD-Verbundkonto bob@contoso.com erstellt, das in der Azure AD-Instanz *contoso* vorhanden ist. Der Benutzer „Bob“ kann auch ein Gastbenutzer sein.

```sql
CREATE LOGIN [bob@contoso.com] FROM EXTERNAL PROVIDER
GO
```

### <a name="e-creating-a-login-for-an-azure-ad-group"></a>E. Erstellen eines Anmeldenamens für eine Azure AD-Gruppe

 Im folgenden Beispiel wird ein Anmeldename für die Azure AD-Gruppe *mygroup* erstellt, die in der Azure AD-Instanz *myaad* vorhanden ist.

```sql
CREATE LOGIN [mygroup] FROM EXTERNAL PROVIDER
GO
```

### <a name="f-creating-a-login-for-an-azure-ad-application"></a>F. Erstellen eines Anmeldenamens für eine Azure AD-Anwendung

Im folgenden Beispiel wird ein Anmeldename für die Azure AD-Anwendung *myapp* erstellt, die in der Azure AD-Instanz *myaad* vorhanden ist.

```sql
CREATE LOGIN [myapp] FROM EXTERNAL PROVIDER
```

### <a name="g-check-newly-added-logins"></a>G. Überprüfen neu hinzugefügter Anmeldenamen

Führen Sie den folgenden T-SQL-Befehl aus, um neu hinzugefügte Anmeldenamen zu überprüfen:

```sql
SELECT *
FROM sys.server_principals;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Kennwortrichtlinie](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Erstellen eines Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL-Datenbank](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL<br />Managed Instance](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-login-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Azure Synapse Analytics
CREATE LOGIN login_name
 { WITH <option_list> }

<option_list> ::=
    PASSWORD = { 'password' }
    [ , SID = sid ]
```

## <a name="arguments"></a>Argumente

*login_name* Gibt den Anmeldenamen an, der erstellt wird. SQL Analytics in Azure Synapse unterstützt nur SQL-Anmeldungen. Um Konten für Azure Active Directory-Benutzer zu erstellen, verwenden Sie die [CREATE USER](create-user-transact-sql.md)-Anweisung.

PASSWORD **='** password* *'* Gibt das Kennwort für den SQL-Anmeldenamen an, der erstellt wird. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md) und [Kennwortrichtlinie](../../relational-databases/security/password-policy.md). Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] werden gespeicherte Kennwortinformationen mithilfe der SHA-512-Komponente des mit Salt verschlüsselten Kennworts berechnet.

Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Kennwörter sollten immer mindestens acht Zeichen lang sein und dürfen 128 Zeichen nicht überschreiten. Kennwörter dürfen a-z, A-Z, 0-9 und die meisten nicht alphanumerischen Zeichen einschließen. Kennwörter dürfen keine einfachen Anführungszeichen oder *login_name* enthalten.

 SID = *sid* Wird verwendet, um eine Anmeldung neu zu erstellen. Gilt nur für Anmeldenamen für die SQL Server-Authentifizierung, nicht für Anmeldenamen für die Windows-Authentifizierung. Gibt die SID des neuen Anmeldenamens für die SQL Server-Authentifizierung an. Wenn diese Option nicht verwendet wird, weist SQL Server automatisch eine SID zu. Die SID-Struktur hängt von der SQL Server-Version ab. Bei SQL Analytics ist dies ein 32-Byte-Literalwert (**binary(32)** ), bestehend aus `0x01060000000000640000000000000000` plus 16 Bytes, die eine GUID darstellen. Beispiel: `SID = 0x0106000000000064000000000000000014585E90117152449347750164BA00A7`.

## <a name="remarks"></a>Bemerkungen

- Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.
- Ein Skript zum Übertragen von Anmeldenamen finden unter [Übertragen von Benutzernamen und Kennwörtern zwischen Instanzen von SQL Server](https://support.microsoft.com/kb/918992).
- Durch das Erstellen eines Anmeldenamens wird der neue Anmeldename automatisch aktiviert, und diesem Anmeldenamen wird die **CONNECT SQL**-Berechtigung auf Serverebene gewährt.
- Der [Authentifizierungsmodus](../../relational-databases/security/choose-an-authentication-mode.md) muss mit dem Anmeldetyp übereinstimmen, damit der Zugriff gewährt wird.
- Informationen zum Entwerfen eines Berechtigungssystems finden Sie unter [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).

## <a name="logins"></a>Anmeldungen

Die **CREATE LOGIN**-Anweisung muss die einzige Anweisung in einem Batch sein.

Beim Herstellen einer Verbindung mit Azure Synapse mithilfe von Tools wie **sqlcmd** müssen Sie den SQL Analytics-Servernamen mit der Notation *\<login>* @ *\<server>* an den Anmeldenamen in der Verbindungszeichenfolge anfügen. Wenn Ihr Anmeldename beispielsweise `login1` und der vollqualifizierte Name des SQL Analytics-Servers `servername.database.windows.net` lautet, sollte der Parameter *username* der Verbindungszeichenfolge `login1@servername` lauten. Da die Gesamtlänge des *username*-Parameters 128 Zeichen beträgt, ist *login_name* auf 127 Zeichen abzüglich der Länge des Servernamens beschränkt. Im Beispiel darf `login_name` nur 117 Zeichen lang sein, da `servername` 10 Zeichen enthält.

Um eine Anmeldung erstellen zu können, müssen Sie mit der Masterdatenbank verbunden sein.

Mithilfe von SQL Server-Regeln können Sie einen Anmeldenamen für die SQL Server-Authentifizierung im Format \<loginname>@\<servername> erstellen. Wenn der [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Servername **myazureserver** und der Anmeldename **myemail@live.com** lauten, müssen Sie den Anmeldenamen als **myemail@live.com@myazureserver** bereitstellen.

Anmeldedaten, die für die Authentifizierung einer Verbindung und Firewallregeln auf Serverebene erforderlich sind, werden vorübergehend in jeder Datenbank gespeichert. Dieser Cache wird regelmäßig aktualisiert. Führen Sie [DBCC FLUSHAUTHCACHE](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md) aus, um eine Aktualisierung des Authentifizierungscache zu erzwingen und sicherzustellen, dass eine Datenbank über die aktuelle Version der Tabelle mit Anmeldenamen verfügt.

Weitere Informationen zu Anmeldungen finden Sie unter [Verwalten von Datenbanken und Anmeldungen](/azure/sql-database/sql-database-manage-logins).

## <a name="permissions"></a>Berechtigungen

Neue Anmeldenamen können nur mit den Anmeldenamen des Serverebenenprinzipals (im Bereitstellungsprozess erstellt) oder von Mitgliedern der `loginmanager`-Datenbankrolle in der Masterdatenbank erstellt werden. Weitere Informationen finden Sie unter [Rollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#groups-and-roles) und unter [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).

## <a name="after-creating-a-login"></a>Nach dem Erstellen eines Anmeldenamens

Nach dem Erstellen einer Anmeldung kann diese eine Verbindung mit Azure Synapse herstellen, verfügt jedoch nur über die der Rolle **public** erteilten Berechtigungen. Ziehen Sie die Ausführung einiger der folgenden Aktivitäten in Betracht.

- Erstellen Sie zum Herstellen einer Verbindung mit einer Datenbank einen Datenbankbenutzer für den Anmeldenamen. Weitere Informationen finden Sie unter [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md).
- Verwenden Sie für die Erteilung von Berechtigungen für einen Benutzer in einer Datenbank die Anweisung **ALTER SERVER ROLE** ... **ADD MEMBER**-Anweisung zum Hinzufügen des Benutzer zu einer der integrierten Datenbankrollen oder einer benutzerdefinierten Rolle oder zum Erteilen von Berechtigungen für den Benutzer über die direkte Verwendung der Anweisung [GRANT](grant-transact-sql.md). Weitere Informationen finden Sie unter [Benutzer ohne Administratorrechte](/azure/sql-database/sql-database-manage-logins#non-administrator-users) und in den Artikeln zu den Anweisungen [Zusätzliche Administratorrollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md)- und [GRANT](grant-transact-sql.md)-Anweisung.
- Wenn Sie serverweite Berechtigungen erteilen möchten, erstellen Sie in der Masterdatenbank einen Datenbankbenutzer, und verwenden Sie die Anweisung **ALTER SERVER ROLE** ... **ADD MEMBER**-Anweisung zum Hinzufügen des Benutzers zu einer der administrativen Serverrollen. Weitere Informationen finden Sie unter [Rollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#groups-and-roles), [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md) und unter [Serverrollen](/azure/sql-database/sql-database-manage-logins#additional-server-level-administrative-roles).

- Gewähren Sie mit der **GRANT**-Anweisung Berechtigungen auf Serverebene für den neuen Anmeldenamen oder für eine Rolle, die den Anmeldenamen enthält. Weitere Informationen finden Sie unter [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-login-with-a-password"></a>A. Erstellen einer Anmeldung mit einem Kennwort

Im folgenden Beispiel wird ein Anmeldename für einen bestimmten Benutzer erstellt, und es wird ein Kennwort zugewiesen.

```sql
CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-login-from-a-sid"></a>B. Erstellen eines Anmeldenamens aus einer SID

 Im folgenden Beispiel wird ein Anmeldename für die SQL Server-Authentifizierung erstellt und die SID des Anmeldenamens festgelegt.

```sql
CREATE LOGIN TestLogin WITH PASSWORD = 'SuperSecret52&&';

SELECT name, sid FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

Die Abfrage hier gibt 0x241C11948AEEB749B0D22646DB1A19F2 als SID zurück. Die Abfrage gibt einen anderen Wert zurück. Die folgenden Anweisungen löschen den Anmeldenamen und erstellen dann erneut die Anmeldung. Verwenden Sie die SID aus der vorherigen Abfrage.

```sql
DROP LOGIN TestLogin;
GO

CREATE LOGIN TestLogin
WITH PASSWORD = 'SuperSecret52&&', SID = 0x241C11948AEEB749B0D22646DB1A19F2;

SELECT * FROM sys.sql_logins WHERE name = 'TestLogin';
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Kennwortrichtlinie](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Erstellen eines Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-login-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL-Datenbank](create-login-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure SQL<br />Managed Instance](create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-login-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
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
CREATE LOGIN loginName { WITH <option_list1> | FROM WINDOWS }

<option_list1> ::=
    PASSWORD = { 'password' } [ MUST_CHANGE ]
    [ , <option_list> [ ,... ] ]
  
<option_list> ::=
      CHECK_EXPIRATION = { ON | OFF}
    | CHECK_POLICY = { ON | OFF}
```

## <a name="arguments"></a>Argumente

*login_name* Gibt den Anmeldenamen an, der erstellt wird. Es gibt vier Arten von Anmeldenamen: SQL Server-Anmeldungen, Windows-Anmeldenamen, Anmeldenamen mit zugeordneten Zertifikaten sowie Anmeldenamen mit zugeordneten asymmetrischen Schlüsseln. Wenn Sie Anmeldenamen erstellen, die von einem Windows-Domänenkonto zugeordnet werden, müssen Sie den Benutzeranmeldenamen bei Versionen vor Windows 2000 im Format [\<domainName>\\<login_name>] verwenden. Sie können keine UPN im Format login_name@DomainName verwenden. Ein Beispiel hierzu finden Sie unter „Beispiel D“ weiter unten in diesem Artikel. Anmeldenamen für die Authentifizierung sind vom Typ **sysname**. Sie müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen und dürfen keinen „ **\\** “ enthalten. Windows-Anmeldenamen können ' **\\** '-Zeichen enthalten. Anmeldenamen auf Grundlage von Active Directory-Benutzern sind auf Namen mit weniger als 21 Zeichen beschränkt.

PASSWORD **='** _Kennwort_' Gilt nur für SQL Server-Anmeldungen. Gibt das Kennwort für den Anmeldenamen an, der erstellt wird. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Sichere Kennwörter](../../relational-databases/security/strong-passwords.md) und [Kennwortrichtlinie](../../relational-databases/security/password-policy.md). Ab SQL Server 2012 (11.x) werden gespeicherte Kennwortinformationen mithilfe der SHA-512-Komponente des mit Salt verschlüsselten Kennworts berechnet.

Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden. Kennwörter sollten immer mindestens acht Zeichen lang sein und dürfen 128 Zeichen nicht überschreiten. Kennwörter dürfen a-z, A-Z, 0-9 und die meisten nicht alphanumerischen Zeichen einschließen. Kennwörter dürfen keine einfachen Anführungszeichen oder *login_name* enthalten.

MUST_CHANGE Gilt nur für SQL Server-Anmeldenamen. Wenn diese Option angegeben wird, wird der Benutzer von SQL Server zur Eingabe eines neuen Kennworts aufgefordert, wenn der neue Anmeldename zum ersten Mal verwendet wird.

CHECK_EXPIRATION **=** { ON | **OFF** } Gilt nur für SQL Server-Anmeldenamen. Gibt an, ob die Richtlinie für das Ablaufen von Kennwörtern für diesen Anmeldenamen erzwungen werden soll. Der Standardwert ist OFF.

CHECK_POLICY **=** { **ON** | OFF } Gilt nur für SQL Server-Anmeldenamen. Gibt an, dass die Windows-Kennwortrichtlinien des Computers, auf dem SQL Server ausgeführt wird, für diesen Anmeldenamen erzwungen werden sollen. Der Standardwert ist ON.

Wenn die Windows-Richtlinie sichere Kennwörter erfordert, müssen Kennwörter mindestens drei der folgenden vier Eigenschaften aufweisen:

- Ein Großbuchstabe (A-Z).
- Ein Kleinbuchstabe (a-z).
- Eine Ziffer (0-9).
- Ein nicht alphanumerisches Zeichen, z. B. ein Leerzeichen, _, @, *, ^,%, !, $, # oder &.

WINDOWS Gibt an, dass der Anmeldename einem Windows-Anmeldenamen zugeordnet wird.

## <a name="remarks"></a>Bemerkungen

- Bei Kennwörtern wird nach Groß- und Kleinschreibung unterschieden.
- Falls MUST_CHANGE angegeben wird, müssen CHECK_EXPIRATION und CHECK_POLICY auf ON festgelegt werden. Andernfalls erzeugt die Anweisung einen Fehler.
- Die Kombination aus CHECK_POLICY = OFF und CHECK_EXPIRATION = ON wird nicht unterstützt.
- Wenn CHECK_POLICY auf OFF festgelegt ist, wird *lockout_time* zurückgesetzt, und CHECK_EXPIRATION wird auf OFF festgelegt.

> [!IMPORTANT]
> CHECK_EXPIRATION und CHECK_POLICY werden nur unter Windows Server 2003 und höher erzwungen. Weitere Informationen finden Sie unter [Password Policy](../../relational-databases/security/password-policy.md).

- Ein Skript zum Übertragen von Anmeldenamen finden unter [Übertragen von Benutzernamen und Kennwörtern zwischen Instanzen von SQL Server](https://support.microsoft.com/kb/918992).
- Durch das Erstellen eines Anmeldenamens wird der neue Anmeldename automatisch aktiviert, und diesem Anmeldenamen wird die **CONNECT SQL**-Berechtigung auf Serverebene gewährt.
- Informationen zum Entwerfen eines Berechtigungssystems finden Sie unter [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).

## <a name="permissions"></a>Berechtigungen

Nur Benutzer mit **ALTER ANY LOGIN**-Berechtigung auf dem Server oder einer Mitgliedschaft bei der festen Serverrolle **securityadmin** können Anmeldenamen erstellen. Weitere Informationen finden Sie unter [Rollen auf Serverebene](/azure/sql-database/sql-database-manage-logins#groups-and-roles) und unter [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).

## <a name="after-creating-a-login"></a>Nach dem Erstellen eines Anmeldenamens

Nach dem Erstellen einer Anmeldung kann diese eine Verbindung mit Azure Synapse Analytics herstellen, verfügt jedoch nur über die der Rolle **public** erteilten Berechtigungen. Ziehen Sie die Ausführung einiger der folgenden Aktivitäten in Betracht.

- Erstellen Sie zum Herstellen einer Verbindung mit einer Datenbank einen Datenbankbenutzer für den Anmeldenamen. Weitere Informationen finden Sie unter [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md).
- Erstellen sie eine benutzerdefinierte Serverrolle mit [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md). Verwenden Sie **ALTER SERVER ROLE** ... **ADD MEMBER**, um der benutzerdefinierten Serverrolle den neuen Anmeldenamen hinzuzufügen. Weitere Informationen finden Sie unter [CREATE SERVER ROLE](../../t-sql/statements/create-server-role-transact-sql.md) und unter [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).
- Fügen Sie den Anmeldenamen mit **sp_addsrvrolemember** einer festen Serverrolle hinzu. Weitere Informationen finden Sie unter [Rollen auf Serverebene](../../relational-databases/security/authentication-access/server-level-roles.md) und unter [sp_addsrvrolemember](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).
- Gewähren Sie mit der **GRANT**-Anweisung Berechtigungen auf Serverebene für den neuen Anmeldenamen oder für eine Rolle, die den Anmeldenamen enthält. Weitere Informationen finden Sie unter [GRANT](../../t-sql/statements/grant-transact-sql.md).

## <a name="examples"></a>Beispiele

### <a name="g-creating-a-sql-server-authentication-login-with-a-password"></a>G. Erstellen einer SQL Server-Authentifizierungsanmeldung mit einem Kennwort

Im folgenden Beispiel wird der Anmeldename `Mary7` mit dem Kennwort `A2c3456` erstellt.

```sql
CREATE LOGIN Mary7 WITH PASSWORD = 'A2c3456$#' ;
```

### <a name="h-using-options"></a>H. Using-Optionen

Im folgenden Beispiel wird der Anmeldename `Mary8` mit Kennwörtern und einigen optionalen Argumenten erstellt.

```sql
CREATE LOGIN Mary8 WITH PASSWORD = 'A2c3456$#' MUST_CHANGE,
CHECK_EXPIRATION = ON,
CHECK_POLICY = ON;
```

### <a name="i-creating-a-login-from-a-windows-domain-account"></a>I. Erstellen eines Anmeldenamens von einem Windows-Domänenkonto

Im folgenden Beispiel wird ein Anmeldename von einem Windows-Domänenkonto mit dem Namen `Mary` in der Domäne `Contoso` erstellt.

```sql
CREATE LOGIN [Contoso\Mary] FROM WINDOWS;
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)
- [Principals](../../relational-databases/security/authentication-access/principals-database-engine.md)
- [Kennwortrichtlinie](../../relational-databases/security/password-policy.md)
- [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md)
- [DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [Erstellen eines Anmeldenamens](../../relational-databases/security/authentication-access/create-a-login.md)

---

::: moniker-end
