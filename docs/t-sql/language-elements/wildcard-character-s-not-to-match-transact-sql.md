---
title: '[^]-Platzhalter zum Ausschließen von Zeichen'
description: T-SQL-Platzhalter für Zeichen, die nicht gesucht werden sollen
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- wildcard
- '[^]_TSQL'
- '[^]'
- Not Match
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[^] (wildcard - character(s) not to match)'
ms.assetid: b970038f-f4e7-4a5d-96f6-51e3248c6aef
author: cawrites
ms.author: chadam
ms.openlocfilehash: dfaeb1d003fa5876cf924349fc8f701967d631db
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092086"
---
# <a name="-wildcard---characters-not-to-match-transact-sql"></a>\[^\] (Platzhalterzeichen – nicht zu suchende(s) Zeichen) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Findet alle einzelnen Zeichen, die nicht dem Bereich oder der Menge angehören, die zwischen den eckigen Klammern `[^]` angegeben ist. Diese Platzhalterzeichen können in Zeichenfolgenvergleichen verwendet werden, bei denen Mustervergleiche wie `LIKE` und `PATINDEX` durchgeführt werden. 
  
## <a name="examples"></a>Beispiele  
### <a name="a-simple-example"></a>A: Einfaches Beispiel   
 Im folgenden Beispiel wird der [^]-Operator verwendet, um ersten fünf Personen in der `Contact`-Tabelle zu suchen, deren Vorname mit `Al` beginnt und als dritten Buchstaben nicht den Buchstaben `a` hat.  
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP 5 FirstName, LastName  
FROM Person.Person  
WHERE FirstName LIKE 'Al[^a]%';  
```  
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
FirstName     LastName
---------     --------
Alex          Adams
Alexandra     Adams
Allison       Adams
Alisha        Alan
Alexandra     Alexander
```
### <a name="b-searching-for-ranges-of-characters"></a>B: Suchen nach Zeichenbereichen

In einer Reihe von Platzhaltern können einzelne Zeichen oder Zeichenbereiche sowie Kombinationen aus Zeichen und Bereichen angegeben werden. Im folgenden Beispiel wird mithilfe des [^]-Operators eine Zeichenfolge gesucht, die nicht mit einem Buchstaben oder einer Zahl beginnt.

```sql
SELECT [object_id], OBJECT_NAME(object_id) AS [object_name], name, column_id 
FROM sys.columns 
WHERE name LIKE '[^0-9A-z]%';
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
object_id     object_name   name    column_id
---------     -----------   ----    ---------
1591676718    JunkTable     _xyz    1
```
  
## <a name="see-also"></a>Weitere Informationen  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
 [% &#40;Wildcard - Character&#40;s&#41; to Match&#41; &#40;Transact-SQL&#41; (% (Platzhalterzeichen – zu suchende(s) Zeichen) (Transact-SQL))](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
  [&#91; &#93; &#40;Wildcard - Character&#40;s&#41; to Match&#41; &#40;Transact-SQL&#41; ([ ] (Platzhalterzeichen – zu suchende(s) Zeichen) (Transact-SQL))](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [\_ &#40;Wildcard - Match One Character&#41; &#40;Transact-SQL&#41; (_ (Platzhalterzeichen – einzelnes zu suchendes Zeichen) (Transact-SQL))](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
  
  
