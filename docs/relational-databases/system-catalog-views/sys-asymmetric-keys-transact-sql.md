---
description: sys.asymmetric_keys (Transact-SQL)
title: sys.asymmetric_keys (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- asymmetric_keys
- sys.asymmetric_keys_TSQL
- asymmetric_keys_TSQL
- sys.asymmetric_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.asymmetric_keys catalog view
ms.assetid: bbca796a-9bb5-4a62-9ca8-1d255984553d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cf926f7c3d3ab84a41ed142dd8ff1bef7357af96
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091532"
---
# <a name="sysasymmetric_keys-transact-sql"></a>sys.asymmetric_keys (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine Zeile für jeden asymmetrischen Schlüssel zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Name des Schlüssels. Ist in der Datenbank eindeutig.|  
|**principal_id**|**int**|ID des Datenbankprinzipals, der der Besitzer des Schlüssels ist.|  
|**asymmetric_key_id**|**int**|ID des Schlüssels. Ist in der Datenbank eindeutig.|  
|**pvt_key_encryption_type**|**char(2)**|Die Verschlüsselungsart des Schlüssels.<br /><br /> NA = Nicht verschlüsselt<br /><br /> MK = Schlüssel ist mit dem Hauptschlüssel verschlüsselt<br /><br /> PW = Schlüssel ist mit einem benutzerdefinierten Kennwort verschlüsselt<br /><br /> SK = Schlüssel ist mit dem Diensthauptschlüssel verschlüsselt|  
|**pvt_key_encryption_type_desc**|**nvarchar(60)**|Die Beschreibung der Verschlüsselungsart des privaten Schlüssels.<br /><br /> NO_PRIVATE_KEY<br /><br /> ENCRYPTED_BY_MASTER_KEY<br /><br /> ENCRYPTED_BY_PASSWORD<br /><br /> ENCRYPTED_BY_SERVICE_MASTER_KEY|  
|**Fingerabdruck**|**varbinary(32)**|SHA-1-Hash des Schlüssels. Der Hash ist global eindeutig.|  
|**projiziert**|**char(2)**|Der mit dem Schlüssel verwendete Algorithmus.<br /><br /> 1R = 512-Bit-RSA<br /><br /> 2R = 1024-Bit-RSA<br /><br /> 3R = 2048-Bit-RSA|  
|**algorithm_desc**|**nvarchar(60)**|Beschreibung des mit dem Schlüssel verwendeten Algorithmus.<br /><br /> RSA_512<br /><br /> RSA_1024<br /><br /> RSA_2048|  
|**key_length**|**int**|Bitlänge des Schlüssels.|  
|**sid**|**varbinary(85)**|Anmelde-SID für diesen Schlüssel. Bei Extensible Key Management-Schlüsseln ist dieser Wert NULL.|  
|**string_sid**|**nvarchar(128)**|Zeichenfolgendarstellung der Anmelde-SID des Schlüssels. Bei Extensible Key Management-Schlüsseln ist dieser Wert NULL.|  
|**public_key**|**varbinary(max)**|Öffentlicher Schlüssel.|  
|**attested_by**|**nvarchar(260)**|Nur zur Verwendung durch das System.|  
|**provider_type**|**nvarchar(120)**|Der Typ des Kryptografieanbieters:<br /><br /> CRYPTOGRAPHIC PROVIDER = Extensible Key Management-Schlüssel<br /><br /> NULL = Nicht-Extensible Key Management-Schlüssel|  
|**cryptographic_provider_guid**|**uniqueidentifier**|Der GUID für den Kryptografieanbieter. Bei Schlüsseln, die nicht der erweiterbaren Schlüsselverwaltung entsprechen, ist dieser Wert NULL.|  
|**cryptographic_provider_algid**|**sql_variant**|Die Algorithmus-ID für den Kryptografieanbieter. Bei Schlüsseln, die nicht der erweiterbaren Schlüsselverwaltung entsprechen, ist dieser Wert NULL.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitskatalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Erweiterbare Schlüsselverwaltung &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
  
