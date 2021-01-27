---
description: sys.sp_cdc_disable_db (Transact-SQL)
title: sys.sp_cdc_disable_db (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cdc_disable_db
- sys.sp_cdc_disable_db_TSQL
- sp_cdc_disable_db_TSQL
- sys.sp_cdc_disable_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_disable_db
- sys.sp_cdc_disable_db
- change data capture [SQL Server], disabling databases
ms.assetid: 420fb99e-e60f-445b-b568-da96471f1e8f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 297ec414919235696fd25ed3c6e9bc0ca9f88602
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812826"
---
# <a name="syssp_cdc_disable_db-transact-sql"></a>sys.sp_cdc_disable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deaktiviert Change Data Capture für die aktuelle Datenbank. Change Data Capture ist nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```sql  
sys.sp_cdc_disable_db  
```  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 **sys.sp_cdc_disable_db** deaktiviert Change Data Capture für alle Tabellen in der Datenbank, die derzeit aktiviert sind. Alle Systemobjekte, die sich auf Change Data Capture beziehen, z. B. Änderungstabellen, Aufträge, gespeicherte Prozeduren und Funktionen, werden gelöscht. Die **is_cdc_enabled** Spalte für den Datenbankeintrag in der [sys. Database](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) -Katalog Sicht wird auf 0 festgelegt.  
  
> [!NOTE]  
>  Wenn zum Zeitpunkt der Deaktivierung von Change Data Capture viele Aufzeichnungsinstanzen für die Datenbank definiert sind, kann eine Transaktion mit langer Ausführungszeit dazu führen, dass die Ausführung von sys.sp_cdc_disable_db fehlschlägt. Dieses Problem kann vermieden werden, indem die einzelnen Aufzeichnungsinstanzen vor der Ausführung von sys.sp_cdc_disable_db mithilfe von sys.sp_cdc_disable_table deaktiviert werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** .  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird Change Data Capture für die Datenbank `AdventureWorks2012` deaktiviert.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_disable_db;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.sp_cdc_enable_db &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md)   
 [sys.sp_cdc_disable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)  
  
