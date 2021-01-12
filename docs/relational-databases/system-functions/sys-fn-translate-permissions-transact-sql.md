---
description: sys.fn_translate_permissions (Transact-SQL)
title: sys.fn_translate_permissions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_translate_permissions
- sys.fn_translate_permissions_TSQL
- fn_translate_permissions
- fn_translate_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions bitmask [SQL Server]
- sys.fn_translate_permissions function
- fn_translate_permissions function
ms.assetid: ac97121f-2bd0-4f71-8e45-42c8584edbc5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e4d00e0b945012cb8a1b583921328009d12a34ad
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094938"
---
# <a name="sysfn_translate_permissions-transact-sql"></a>sys.fn_translate_permissions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Übersetzt die von der SQL-Ablaufverfolgung zurückgegebene Bitmaske von Berechtigungen in eine Tabelle von Berechtigungsnamen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_translate_permissions ( level , perms )  
```  
  
## <a name="arguments"></a>Argumente  
 *level*  
 Die Art eines sicherungsfähigen Elements, für die die Berechtigung übernommen wird. *Ebene* ist vom Datentyp **nvarchar (60)**.  
  
 *perms*  
 Eine Bitmaske, die in der Berechtigungsspalte zurückgegeben wird. *perms* ist **varbinary (16)**.  
  
## <a name="returns"></a>Gibt zurück  
 **Tabelle**  
  
## <a name="remarks"></a>Bemerkungen  
 Der in der Spalte **Berechtigungen** einer SQL-Ablauf Verfolgung zurückgegebene Wert ist eine ganzzahlige Darstellung einer Bitmaske, die von verwendet wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um effektive Berechtigungen zu berechnen. Jede der 25 Arten sicherungsfähiger Elemente verfügt über einen eigenen Satz Berechtigungen mit entsprechenden numerischen Werten. **sys.fn_translate_permissions** übersetzt diese Bitmaske in eine Tabelle von Berechtigungs Namen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="example"></a>Beispiel  
 Die folgende-Abfrage verwendet `sys.fn_builtin_permissions` , um die Berechtigungen anzuzeigen, die für Zertifikate gelten, und verwendet dann `sys.fn_translate_permissions` , um die Ergebnisse der Berechtigungs Bitmaske zurückzugeben.  
  
```  
SELECT * FROM sys.fn_builtin_permissions('CERTIFICATE');  
SELECT '0001' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0001);  
SELECT '0010' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0010);  
SELECT '0011' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0011);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berechtigungen &#40;Datenbank-Engine&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
