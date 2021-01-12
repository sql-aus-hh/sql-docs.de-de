---
description: DROP FULLTEXT CATALOG (Transact-SQL)
title: DROP FULLTEXT CATALOG (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_FULLTEXT_CATALOG_TSQL
- DROP FULLTEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- dropping full-text catalogs
- removing full-text catalogs
- full-text catalogs [SQL Server], removing
- deleting full-text catalogs
- DROP FULLTEXT CATALOG statement
ms.assetid: b54efb0b-400b-49ce-923b-ce20a2a12255
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 77801da7447453ead175f760c711cc087d573419
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091955"
---
# <a name="drop-fulltext-catalog-transact-sql"></a>DROP FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt einen Volltextkatalog aus einer Datenbank. Sie müssen alle Volltextindizes löschen, die dem Katalog zugeordnet sind, bevor Sie den Katalog löschen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
DROP FULLTEXT CATALOG catalog_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *catalog_name*  
 Der Name des Katalogs, der entfernt werden soll. Falls *catalog_name* nicht vorhanden ist, gibt [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Fehler zurück und führt den DROP-Vorgang nicht aus. Die Dateigruppe des Volltextkatalogs darf nicht mit OFFLINE oder READONLY gekennzeichnet sein. Andernfalls schlägt der Befehl fehl.  
  
## <a name="permissions"></a>Berechtigungen  
 Benutzer müssen über die DROP-Berechtigung für den Volltextkatalog verfügen oder Mitglieder der festen Datenbankrollen **db_owner** oder **db_ddladmin** sein.  
  
## <a name="see-also"></a>Siehe auch  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [ALTER FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [Volltextsuche](../../relational-databases/search/full-text-search.md)  
  
  
