---
description: CREATE CREDENTIAL (Transact-SQL)
title: CREATE CREDENTIAL (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREDENTIAL_TSQL
- SQL13.SWB.CREDENTIAL.GENERAL.F1
- CREATE CREDENTIAL
- CREATE_CREDENTIAL_TSQL
- CREDENTIAL
dev_langs:
- TSQL
helpviewer_keywords:
- SECRET clause
- authentication [SQL Server], credentials
- CREATE CREDENTIAL statement
- credentials [SQL Server], CREATE CREDENTIAL statement
ms.assetid: d5e9ae69-41d9-4e46-b13d-404b88a32d9d
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b98affdc431a7e13f83ed152248e3abc1300bf76
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688801"
---
# <a name="create-credential-transact-sql"></a>CREATE CREDENTIAL (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Erstellt Anmeldeinformationen auf Serverebene. Anmeldeinformationen sind in einem Datensatz gespeichert, in dem die Authentifizierungsinformationen enthalten sind, die zum Herstellen einer Verbindung mit einer Ressource außerhalb von SQL Server erforderlich sind. Die meisten Anmeldeinformationen schließen einen Windows-Benutzer und ein Kennwort ein. Wenn Sie z.B. eine Datensicherung an einem beliebigen Speicherort speichern, erfordert SQL Server möglicherweise die Eingabe von besonderen Anmeldeinformationen, damit auf diesen Speicherort zugegriffen werden kann. Weitere Informationen finden Sie unter [Anmeldeinformationen (Datenbank-Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

> [!NOTE]
> Verwenden Sie [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md) zum Erstellen der Anmeldeinformationen auf Datenbankebene. Verwenden Sie Anmeldeinformationen auf Serverebene, wenn Sie dieselben Anmeldeinformationen für mehrere Datenbanken auf demselben Server verwenden müssen. Verwenden Sie datenbankbezogene Anmeldeinformationen, um die Datenbank portierbarer zu machen. Wenn eine Datenbank auf einen neuen Server verschoben wird, werden gleichzeitig auch diese datenbankbezogenen Anmeldeinformationen verschoben. Verwenden Sie datenbankbezogene Anmeldeinformationen in [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE CREDENTIAL credential_name
WITH IDENTITY = 'identity_name'
    [ , SECRET = 'secret' ]
        [ FOR CRYPTOGRAPHIC PROVIDER cryptographic_provider_name ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*credential_name* Gibt den Namen für die zu erstellenden Anmeldeinformationen an. *credential_name* darf nicht mit dem Nummernzeichen (#) beginnen. Systemanmeldeinformationen beginnen mit zwei Nummernzeichen (##). 

> [!IMPORTANT]
> Wenn eine Shared Access Signature (SAS) verwendet wird, muss dieser Name dem Containerpfad zugeordnet werden können, mit „https“ beginnen und einen Schrägstrich enthalten. Siehe [Beispiel D](#d-creating-a-credential-using-a-sas-token).

IDENTITY **='** _identity\_name_ **'** Gibt den Namen des Kontos an, das beim Herstellen einer Verbindung außerhalb des Servers verwendet wird. Wenn die Anmeldeinformationen zum Zugreifen auf Azure Key Vault verwendet werden, ist **IDENTITY** der Name des Schlüsseltresors. Weitere Informationen finden Sie unten im Beispiel C. Wenn die Anmeldeinformationen eine Shared Access Signature (SAS) verwenden, lautet **IDENTITY** *SHARED ACCESS SIGNATURE*. Siehe Beispiel D.

> [!IMPORTANT]
> Azure SQL-Datenbank unterstützt nur Azure Key Vault- und Shared Access Signature-Identitäten. Windows-Benutzeridentitäten werden nicht unterstützt.

SECRET **='** _secret_ **'** Gibt den geheimen Schlüssel an, der für die ausgehende Authentifizierung erforderlich ist.

Wenn die Anmeldeinformationen zum Zugreifen auf Azure Key Vault verwendet werden, müssen an das **SECRET**-Argument von **CREATE CREDENTIAL** die *\<Client ID>* (ohne Bindestriche) und der *\<Secret>* eines **Dienstprinzipals** in Azure Active Directory zusammen, ohne Leerzeichen dazwischen, übergeben werden. Weitere Informationen finden Sie unten im Beispiel C. Wenn die Anmeldeinformationen eine Shared Access Signature (SAS) verwenden, ist **SECRET** das freigegebene SAS-Token. Siehe Beispiel D. Informationen über das Erstellen einer gespeicherten Zugriffsrichtlinie und einer Shared Access Signature (SAS) in einem Azure-Container finden Sie unter [Lektion 1: Erstellen einer gespeicherten Zugriffsrichtlinie und von Speicher mit freigegebenem Zugriff](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#1---create-stored-access-policy-and-shared-access-storage).

OR CRYPTOGRAPHIC PROVIDER *cryptographic_provider_name* Gibt den Namen eines *EKM-Anbieters (Enterprise Key Management)* an. Weitere Informationen zum Verwalten von Schlüsseln finden Sie unter [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).

## <a name="remarks"></a>Bemerkungen

Falls für IDENTITY ein Windows-Benutzer angegeben ist, kann der geheime Bereich das Kennwort enthalten. Der geheime Bereich wird mithilfe des Diensthauptschlüssels verschlüsselt. Falls der Diensthauptschlüssel neu generiert wird, wird der geheime Bereich mithilfe des neuen Diensthauptschlüssels neu verschlüsselt.

Nach der Erstellung von Anmeldeinformationen können Sie diese einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen zuordnen, indem Sie [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) oder [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) verwenden. Ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldename kann nur einem Satz Anmeldeinformationen zugeordnet werden, ein Satz Anmeldeinformationen kann jedoch mehreren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen zugeordnet sein. Weitere Informationen finden Sie unter [Anmeldeinformationen &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md). Anmeldeinformationen auf Serverebene können nur einem Anmeldenamen und keinem Datenbankbenutzer zugeordnet werden. 

Informationen zu Anmeldeinformationen werden in der [sys.credentials](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)-Katalogsicht angezeigt.

Falls für den Anbieter kein mit einem Anmeldenamen verknüpfter Identitätsnachweis vorliegt, werden die dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto zugeordneten Anmeldeinformationen verwendet.

Einem Anmeldenamen können mehrere Anmeldeinformationen zugeordnet werden, solange sie für unterschiedliche Anbieter verwendet werden. Pro Anbieter und Anmeldung darf es jedoch nur einen zugeordneten Identitätsnachweis geben. Die gleichen Anmeldeinformationen können jedoch auch anderen Anmeldenamen zugeordnet werden.

## <a name="permissions"></a>Berechtigungen

Erfordert die **ALTER ANY CREDENTIAL**-Berechtigung.

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-credential-for-windows-identity"></a>A. Erstellen von Anmeldeinformationen für die Windows-Identität

Im folgenden Beispiel werden die Anmeldeinformationen namens `AlterEgo` erstellt. Die Anmeldeinformationen enthalten den Windows-Benutzer `Mary5` und ein Kennwort.

```sql
CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',
    SECRET = '<EnterStrongPasswordHere>';
GO
```

### <a name="b-creating-a-credential-for-ekm"></a>B. Erstellen von Anmeldeinformationen für EKM

Im folgenden Beispiel wird ein zuvor erstelltes Konto namens `User1OnEKM` auf einem EKM-Modul mithilfe der EKM-Verwaltungstools mit einem Standardkontotyp und Kennwort verwendet. Das **sysadmin**-Konto auf dem Server erstellt die Anmeldeinformationen, mit denen die Verbindung mit dem EKM-Konto hergestellt wird, und weist diese dem `User1` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konto zu:

```sql
CREATE CREDENTIAL CredentialForEKM
    WITH IDENTITY='User1OnEKM', SECRET='<EnterStrongPasswordHere>'
    FOR CRYPTOGRAPHIC PROVIDER MyEKMProvider;
GO

/* Modify the login to assign the cryptographic provider credential */
ALTER LOGIN User1
ADD CREDENTIAL CredentialForEKM;
```

### <a name="c-creating-a-credential-for-ekm-using-the-azure-key-vault"></a>C. Erstellen von Anmeldeinformationen für EKM unter Verwendung des Azure-Schlüsseltresors

Im folgenden Beispiel werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen erstellt, die das [!INCLUDE[ssDE](../../includes/ssde-md.md)] beim Zugreifen auf Azure Key Vault mit dem **SQL Server-Connector für Microsoft Azure Key Vault** verwenden soll. Ein vollständiges Beispiel zur Verwendung des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Connectors finden Sie unter [Erweiterbare Schlüsselverwaltung mit Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).

> [!IMPORTANT]
> Das **IDENTITY** -Argument von **CREATE CREDENTIAL** erfordert den Schlüsseltresornamen. An das **SECRET**-Argument von **CREATE CREDENTIAL** müssen die *\<Client ID>* (ohne Bindestriche) und der *\<Secret>* zusammen, ohne Leerzeichen dazwischen, übergeben werden.

 Im folgenden Beispiel wird die **Client-ID** (`EF5C8E09-4D2A-4A76-9998-D93440D8115D`) von den Bindestrichen bereinigt und als Zeichenfolge `EF5C8E094D2A4A769998D93440D8115D` eingegeben. Der **geheime Schlüssel** wird durch die Zeichenfolge *SECRET_DBEngine* dargestellt.

```sql
USE master;
CREATE CREDENTIAL Azure_EKM_TDE_cred
    WITH IDENTITY = 'ContosoKeyVault',
    SECRET = 'EF5C8E094D2A4A769998D93440D8115DSECRET_DBEngine'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;
```

Im folgenden Beispiel werden die gleichen Anmeldeinformationen unter Verwendung von Variablen für die Zeichenfolgen der **Client-ID** und des **geheimem Schlüssels** erstellt, die dann verkettet werden, um das **SECRET**-Argument zu bilden. Mit der **REPLACE**-Funktion werden die Bindestriche aus der Client-ID entfernt.

```sql
DECLARE @AuthClientId uniqueidentifier = 'EF5C8E09-4D2A-4A76-9998-D93440D8115D';
DECLARE @AuthClientSecret varchar(200) = 'SECRET_DBEngine';
DECLARE @pwd varchar(max) = REPLACE(CONVERT(varchar(36), @AuthClientId) , '-', '') + @AuthClientSecret;

EXEC ('CREATE CREDENTIAL Azure_EKM_TDE_cred
    WITH IDENTITY = 'ContosoKeyVault', SECRET = ''' + @PWD + '''
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;');
```

### <a name="d-creating-a-credential-using-a-sas-token"></a>D: Erstellen von Anmeldeinformationen mithilfe eines SAS-Token

**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] bis zur [aktuellen Version](../../sql-server/what-s-new-in-sql-server-2016.md) und für Azure SQL Managed Instance

Das folgende Beispiel erstellt SAS-Anmeldinformationen mit dem SAS-Token. Ein Tutorial zum Erstellen einer gespeicherten Zugriffsrichtlinie und einer SAS für einen Azure-Container und das anschließende Erstellen von Anmeldeinformationen mit dieser SAS finden Sie unter [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).

> [!IMPORTANT]
> Das **CREDENTIAL NAME**-Argument erfordert, dass der Name dem Containerpfad zugeordnet werden kann, mit „https“ beginnt und keinen nachgestellten Schrägstrich enthält. Das **IDENTITY**-Argument erfordert den Namen *SHARED ACCESS SIGNATURE*. Das **SECRET**-Argument erfordert das SAS-Token.
>
> Das **SHARED ACCESS SIGNATURE-Geheimnis** sollte kein vorangehendes **?** haben.

```sql
USE master
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] -- this name must match the container path, start with https and must not contain a trailing forward slash.
    WITH IDENTITY='SHARED ACCESS SIGNATURE' -- this is a mandatory string and do not change it.
    , SECRET = 'sharedaccesssignature' -- this is the shared access signature token
GO
```

### <a name="e-creating-a-credential-for-managed-identity"></a>E. Erstellen von Anmeldeinformationen für eine verwaltete Identität

Im folgenden Beispiel werden die Anmeldeinformationen erstellt, die die verwaltete Identität von Azure SQL oder Azure Synapse darstellt. In diesem Falls ist kein Kennwort und kein Geheimnis anwendbar.

```sql
CREATE CREDENTIAL ServiceIdentity WITH IDENTITY = 'Managed Identity';
GO
```

## <a name="see-also"></a>Weitere Informationen

- [Anmeldeinformationen &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [ALTER CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-credential-transact-sql.md)
- [DROP CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-credential-transact-sql.md)
- [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)
- [Lektion 2: Erstellen von SQL Server-Anmeldeinformationen mit einer Shared Access Signature (SAS)](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature)
- [Shared Access Signatures](/azure/storage/common/storage-sas-overview)