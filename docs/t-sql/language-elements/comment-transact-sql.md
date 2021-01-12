---
description: -- (Kommentar) (Transact-SQL)
title: -- (Kommentar) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- --_TSQL
- Comment
- --
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- remarks [SQL Server]
- -- (comment character)
- comments [SQL Server]
ms.assetid: 676ea8c2-52c1-4ef6-9354-320f1a091153
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 152276ea9879065e1ffbe809197e82959172dac9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100405"
---
# <a name="---comment-transact-sql"></a>-- (Kommentar) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt vom Benutzer bereitgestellten Text an. Kommentare können in einer eigenen Zeile eingefügt werden, geschachtelt am Ende einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehlszeile oder innerhalb einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung. Der Kommentar wird vom Server nicht ausgewertet.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- text_of_comment  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *text_of_comment*  
 Die Zeichenfolge mit dem Kommentartext.  
  
## <a name="remarks"></a>Bemerkungen  
Verwenden Sie zwei Bindestriche ( **--** ) für einzeilige oder geschachtelte Kommentare. Kommentare, die mit **--** eingefügt werden, werden von einer neuen Zeile beendet, die durch ein Wagenrücklaufzeichen (U+000A), ein Zeilenvorschubzeichen (U+000D) oder eine Kombination der beiden Zeichen angegeben ist. Es gibt keine Maximallänge für Kommentare. In der folgenden Tabelle sind die Tastenkombinationen aufgeführt, die Sie verwenden können, um Text als Kommentar zu kennzeichnen oder auszukommentieren.
  
|Aktion|Standard|  
|------------|--------------|  
|Umwandeln des markierten Texts in einen Kommentar|STRG+K, STRG+C|  
|Kommentierung des ausgewählten Texts entfernen|STRG+K, STRG+U|  
  
 Weitere Informationen zu diesen Tastenkombinationen finden Sie unter [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
 Mehrzeilige Kommentare finden Sie unter [Slash Star &#40;Block Comment&#41; &#40;Transact-SQL&#41; (Schrägstrich – (Blockkommentar) (Transact-SQL))](../../t-sql/language-elements/slash-star-comment-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden die Kommentarzeichen -- verwendet.  
  
```sql  
-- Choose the AdventureWorks2012 database.  
USE AdventureWorks2012;  
GO  
-- Choose all columns and all rows from the Address table.  
SELECT *  
FROM Person.Address  
ORDER BY PostalCode ASC; -- We do not have to specify ASC because   
-- that is the default.  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Control-of-Flow Language &#40;Transact-SQL&#41; (Sprachkonstrukte zur Ablaufsteuerung (Transact-SQL))](~/t-sql/language-elements/control-of-flow.md)  
  
  
