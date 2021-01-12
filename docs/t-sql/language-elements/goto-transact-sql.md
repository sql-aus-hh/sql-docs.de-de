---
description: GOTO (Transact-SQL)
title: GOTO (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GOTO
- GOTO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- skipping statements
- Transact-SQL statements, skipping
- labels [SQL Server]
- statements [SQL Server], skipping
- GOTO statement
ms.assetid: 589b6f8e-dc80-416f-9e74-48bed5337f58
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9d47b0b4b847e6cb3a6fee75a6b1ea561f1c57f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100340"
---
# <a name="goto-transact-sql"></a>GOTO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Verändert den Kontrollfluss, sodass die Ausführung an einer Marke fortgesetzt wird. Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung oder -Anweisungen, die GOTO folgen, werden ausgelassen, und die Verarbeitung wird an der Marke fortgesetzt. Sie können GOTO-Anweisungen und Marken überall in einer Prozedur, in einem Batch oder einem Anweisungsblock verwenden. GOTO-Anweisungen können geschachtelt werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Define the label:   
label:   
Alter the execution:  
GOTO label   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *label*  
 Ist der Punkt, an dem die Ausführung beginnt, wenn GOTO auf diese Marke zielt. Bezeichnungen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen. Sie können Marken auch als Kommentarmethode verwenden, unabhängig davon, ob GOTO verwendet wird.  
  
## <a name="remarks"></a>Bemerkungen  
 GOTO kann innerhalb von bedingten Anweisungen zur Ablaufsteuerung, Anweisungsblöcken oder Prozeduren vorhanden sein, nicht aber an Marken außerhalb des Batches. Sie können mit GOTO zu einer Marke verzweigen, die vor oder nach GOTO definiert ist.  
  
## <a name="permissions"></a>Berechtigungen  
 GOTO-Berechtigungen erhalten standardmäßig alle gültigen Benutzer.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird gezeigt, wie Sie `GOTO` als Verzweigungsmechanismus verwenden können.  
  
```  
DECLARE @Counter int;  
SET @Counter = 1;  
WHILE @Counter < 10  
BEGIN   
    SELECT @Counter  
    SET @Counter = @Counter + 1  
    IF @Counter = 4 GOTO Branch_One --Jumps to the first branch.  
    IF @Counter = 5 GOTO Branch_Two  --This will never execute.  
END  
Branch_One:  
    SELECT 'Jumping To Branch One.'  
    GOTO Branch_Three; --This will prevent Branch_Two from executing.  
Branch_Two:  
    SELECT 'Jumping To Branch Two.'  
Branch_Three:  
    SELECT 'Jumping To Branch Three.';  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Control-of-Flow Language &#40;Transact-SQL&#41; (Sprachkonstrukte zur Ablaufsteuerung (Transact-SQL))](~/t-sql/language-elements/control-of-flow.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [BREAK &#40;Transact-SQL&#41;](../../t-sql/language-elements/break-transact-sql.md)   
 [CONTINUE &#40;Transact-SQL&#41;](../../t-sql/language-elements/continue-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [WAITFOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/waitfor-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
  
