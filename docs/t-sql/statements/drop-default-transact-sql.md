---
description: DROP DEFAULT (Transact-SQL)
title: DROP DEFAULT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_DEFAULT_TSQL
- DROP DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- DROP DEFAULT statement
- defaults [SQL Server], removing
ms.assetid: d2d3af25-8877-46ba-95d9-1844961d97ee
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 43c75b4462f8a725e5ce06d4d5eb545000bc8bb7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095780"
---
# <a name="drop-default-transact-sql"></a>DROP DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Entfernt einen oder mehrere benutzerdefinierte Standardwerte aus der aktuellen Datenbank.  
  
> [!IMPORTANT]
>  DROP DEFAULT wird in der nächsten Version von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr unterstützt. Verwenden Sie DROP DEFAULT nicht bei neuen Entwicklungsarbeiten, und planen Sie die Änderung von Anwendungen, die DROP DEFAULT derzeit verwenden. Verwenden Sie stattdessen Standarddefinitionen, die mit dem DEFAULT-Schlüsselwort von [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) oder [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) erstellt werden können.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DROP DEFAULT [ IF EXISTS ] { [ schema_name . ] default_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Löscht die Standardeinstellung nur, wenn diese bereits vorhanden ist.  
  
 *schema_name*  
 Der Name des Schemas, zu dem der Standardwert gehört.  
  
 *default_name*  
 Der Name eines vorhandenen Standardwerts. Führen Sie **sp_help** aus, um eine Liste von vorhandenen Standardwerten anzuzeigen. Standardwerte müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen. Das Angeben des Standardschemanamens ist optional.  
  
## <a name="remarks"></a>Hinweise  
 Bevor ein Standardwert gelöscht wird, muss die Bindung des Standardwerts durch Ausführen von **sp_unbindefault** aufgehoben werden, wenn der Standardwert aktuell an eine Spalte oder an einen Aliasdatentyp gebunden ist.  
  
 Nachdem ein Standardwert aus einer Spalte gelöscht wurde, die NULL-Werte zulässt, wird NULL an dessen Stelle eingefügt, wenn Zeilen hinzugefügt und keine Werte explizit angegeben werden. Nachdem ein Standardwert einer Spalte gelöscht wurde, in der keine NULL-Werte zulässig sind, wird eine Fehlermeldung zurückgegeben, wenn Zeilen hinzugefügt und keine Werte explizit angegeben werden. Diese Zeilen werden später als Teil des typischen Verhaltens der INSERT-Anweisung hinzugefügt.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Ausführen von DROP DEFAULT muss ein Benutzer mindestens über die ALTER-Berechtigung für das Schema verfügen, zu dem der Standardwert gehört.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-dropping-a-default"></a>A. Löschen eines Standardwerts  
 Wenn ein Standardwert nicht an eine Spalte oder an einen Aliasdatentyp gebunden ist, kann er einfach mithilfe von DROP DEFAULT gelöscht werden. Im folgenden Beispiel wird der vom Benutzer erstellte Standardwert `datedflt` entfernt.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
         WHERE name = 'datedflt'   
            AND type = 'D')  
   DROP DEFAULT datedflt;  
GO  
```  
  
 Ab [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] können Sie folgende Syntax verwenden.  
  
```sql  
DROP DEFAULT IF EXISTS datedflt;  
GO  
```  
  
### <a name="b-dropping-a-default-that-has-been-bound-to-a-column"></a>B. Löschen eines Standardwerts, der an eine Spalte gebunden war  
 Im folgenden Beispiel wird die Bindung des Standardwerts an die `EmergencyContactPhone`-Spalte in der `Contact`-Tabelle aufgehoben und dann der Standardwert `phonedflt` gelöscht.  
  
```sql  
USE AdventureWorks2012;  
GO  
   BEGIN   
      EXEC sp_unbindefault 'Person.Contact.Phone'  
      DROP DEFAULT phonedflt  
   END;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
