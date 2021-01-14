---
description: sp_dbcmptlevel (Transact-SQL)
title: sp_dbcmptlevel (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_dbcmptlevel
- sp_dbcmptlevel_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbcmptlevel
ms.assetid: 508c686d-2bd4-41ba-8602-48ebca266659
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fbf4875ec03cc961e696ae5f39bd0e29abccb4e9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168387"
---
# <a name="sp_dbcmptlevel-transact-sql"></a>sp_dbcmptlevel (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Legt für bestimmte Verhalten der Datenbank fest, dass sie mit der angegebenen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kompatibel sein müssen.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Verwenden Sie stattdessen [ALTER DATABASE-Kompatibilitäts Grad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dbcmptlevel [ [ @dbname = ] name ]   
    [ , [ @new_cmptlevel = ] version ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @dbname = ] name` Der Name der Datenbank, für die der Kompatibilitäts Grad geändert werden soll. Datenbanknamen müssen den Regeln für Bezeichner entsprechen. *name* ist vom Datentyp **sysname** und hat den Standardwert NULL.  
  
`[ @new_cmptlevel = ] version` Die Version von, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit der die Datenbank kompatibel gemacht werden soll. die *Version* ist vom Datentyp **tinyint**. der Standardwert ist NULL. Folgende Werte sind zulässig:  
  
 **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Wenn keine Parameter angegeben werden oder der *Name* -Parameter nicht angegeben ist, gibt **sp_dbcmptlevel** einen Fehler zurück.  
  
 Wenn *Name* ohne *Version* angegeben wird, [!INCLUDE[ssDE](../../includes/ssde-md.md)] gibt eine Meldung zurück, die den aktuellen Kompatibilitäts Grad der angegebenen Datenbank anzeigt.  
  
## <a name="remarks"></a>Bemerkungen  
 Eine Beschreibung der Kompatibilitäts Grade finden Sie unter [ALTER DATABASE-Kompatibilitäts Grad &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Prozedur kann nur vom Datenbankbesitzer, von Mitgliedern der festen Server Rolle **sysadmin** und der festen Daten Bank Rolle **db_owner** (wenn Sie die aktuelle Datenbank ändern) ausgeführt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank-Engine gespeicherter Prozeduren &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Reserved Keywords &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
