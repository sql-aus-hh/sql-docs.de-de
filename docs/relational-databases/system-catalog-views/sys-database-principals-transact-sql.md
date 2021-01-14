---
description: sys.database_principals (Transact-SQL)
title: sys.database_principals (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_principals
- database_principals_TSQL
- sys.database_principals
- sys.database_principals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_principals catalog view
ms.assetid: 8cb239e9-eb8c-4109-9cec-0d35de95fa0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43fb4dff1730aa0d8e19d411838f76b965fbca01
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171992"
---
# <a name="sysdatabase_principals-transact-sql"></a>sys.database_principals (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden Sicherheitsprinzipal in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zurück.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der Name des Prinzipals, der innerhalb der Datenbank eindeutig ist.|  
|**principal_id**|**int**|Die ID des Prinzipals, die innerhalb der Datenbank eindeutig ist.|  
|**type**|**char(1)**|Prinzipaltyp:<br /><br /> A = Anwendungsrolle<br /><br /> C = Einem Zertifikat zugeordneter Benutzer<br /><br /> E = externer Benutzer aus Azure Active Directory<br /><br /> G = Windows-Gruppe<br /><br /> K = Einem asymmetrischen Schlüssel zugeordneter Benutzer<br /><br /> R = Datenbankrolle<br /><br /> S = SQL-Benutzer<br /><br /> U = Windows-Benutzer<br /><br /> X = externe Gruppe aus Azure Active Directory Gruppe oder Anwendungen|  
|**type_desc**|**nvarchar(60)**|Beschreibung des Prinzipaltyps.<br /><br /> APPLICATION_ROLE<br /><br /> CERTIFICATE_MAPPED_USER<br /><br /> EXTERNAL_USER<br /><br /> WINDOWS_GROUP<br /><br /> ASYMMETRIC_KEY_MAPPED_USER<br /><br /> DATABASE_ROLE<br /><br /> SQL_USER<br /><br /> WINDOWS_USER<br /><br /> EXTERNAL_GROUPS|  
|**default_schema_name**|**sysname**|Der Name, der verwendet werden soll, wenn der SQL-Name kein Schema angibt. NULL für Prinzipale, die nicht vom Typ S, U oder A sind.|  
|**create_date**|**datetime**|Der Zeitpunkt, zu dem der Prinzipal erstellt wurde.|  
|**modify_date**|**datetime**|Der Zeitpunkt, zu dem der Prinzipal zum letzten Mal geändert wurde.|  
|**owning_principal_id**|**int**|ID des Prinzipals, der der Besitzer dieses Prinzipals ist. Alle festgelegten Daten bankrollen befinden sich standardmäßig im Besitz von **dbo** .|  
|**sid**|**varbinary(85)**|Sicherheits-ID (SID) des Prinzipals.  NULL für SYS und INFORMATION SCHEMAS.|  
|**is_fixed_role**|**bit**|Falls 1, stellt diese Zeile einen Eintrag für eine der festen Datenbankrollen dar: db_owner, db_accessadmin, db_datareader, db_datawriter, db_ddladmin, db_securityadmin, db_backupoperator, db_denydatareader oder db_denydatawriter.|  
|**authentication_type**|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt den Authentifizierungstyp an. Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> 0: keine Authentifizierung<br />1: instanzauthentifizierung<br />2: Daten Bank Authentifizierung<br />3: Windows-Authentifizierung<br />4: Azure Active Directory Authentifizierung|  
|**authentication_type_desc**|**nvarchar(60)**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Beschreibung des Authentifizierungstyps. Im folgenden sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> Keine: keine Authentifizierung<br />Instanz: instanzauthentifizierung<br />Datenbank: Daten Bank Authentifizierung<br />Windows: Windows-Authentifizierung<br />Extern: Azure Active Directory Authentifizierung|  
|**default_language_name**|**sysname**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die Standardsprache für diesen Prinzipal an.|  
|**default_language_lcid**|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die Standard-LCID für diesen Prinzipal an.|  
|**allow_encrypted_value_modifications**|**bit**|**Gilt für:** [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Verhindert bei Massenkopiervorgängen kryptografische Metadatenüberprüfungen auf dem Server. Dies ermöglicht dem Benutzer das Massen Kopieren von Daten, die mit Always Encrypted verschlüsselt werden, zwischen Tabellen oder Datenbanken, ohne die Daten zu entschlüsseln. Der Standardwert ist OFF. |      
  
## <a name="remarks"></a>Bemerkungen  
 Die *PasswordLastSetTime* -Eigenschaften sind in allen unterstützten Konfigurationen von SQL Server verfügbar, die anderen Eigenschaften sind jedoch nur verfügbar, wenn SQL Server unter Windows Server 2003 oder höher ausgeführt wird und sowohl CHECK_POLICY als auch CHECK_EXPIRATION aktiviert sind. Weitere Informationen finden Sie unter Kenn [Wort Richtlinie](../../relational-databases/security/password-policy.md) .
Die Werte der principal_id werden möglicherweise wieder verwendet, wenn Prinzipale gelöscht wurden und daher nicht unbedingt immer zunehmen.
  
## <a name="permissions"></a>Berechtigungen  
 Jeder Benutzer kann den eigenen Benutzernamen, die Systembenutzer und die festen Datenbankrollen anzeigen. Zum Anzeigen anderer Benutzer ist ALTER ANY USER oder eine Berechtigung für den Benutzer erforderlich. Zum Anzeigen benutzerdefinierter Rollen ist ALTER ANY ROLE oder die Mitgliedschaft in der Rolle erforderlich.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>A: Auflisten aller Berechtigungen von Datenbankprinzipalen  
 Mit der folgenden Abfrage werden die Berechtigungen aufgelistet, die Datenbankprinzipalen ausdrücklich gewährt oder verweigert wurden.  
  
> [!IMPORTANT]  
>  Die Berechtigungen von festen Datenbankrollen werden nicht in sys.database_permissions aufgeführt. Daher können Datenbankprinzipale über zusätzliche Berechtigungen verfügen, die hier nicht aufgeführt werden.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>G. Auflisten von Berechtigungen für Schemaobjekte in einer Datenbank  
 Die folgende Abfrage verknüpft sys.database_principals und sys.database_permissions mit sys.objects und sys.schemas, um Berechtigungen aufzulisten, die bestimmten Schemaobjekten gewährt oder verweigert wurden.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-listing-all-the-permissions-of-database-principals"></a>C: Auflisten aller Berechtigungen von Daten Bank Prinzipale  
 Mit der folgenden Abfrage werden die Berechtigungen aufgelistet, die Datenbankprinzipalen ausdrücklich gewährt oder verweigert wurden.  
  
> [!IMPORTANT]  
>  Die Berechtigungen fester Daten bankrollen werden in nicht angezeigt `sys.database_permissions` . Daher können Datenbankprinzipale über zusätzliche Berechtigungen verfügen, die hier nicht aufgeführt werden.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="d-listing-permissions-on-schema-objects-within-a-database"></a>D: Auflisten von Berechtigungen für Schema Objekte in einer Datenbank  
 Mit der folgenden Abfrage `sys.database_principals` `sys.database_permissions` werden die Berechtigungen und in und aufgelistet, um `sys.objects` `sys.schemas` Berechtigungen aufzulisten, die bestimmten Schema Objekten erteilt oder verweigert wurden.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Eigenständige Datenbankbenutzer: Erstellen Sie Ihre Datenbank portabel](../../relational-databases/security/contained-database-users-making-your-database-portable.md)   
 [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview)  
  
