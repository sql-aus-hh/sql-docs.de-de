---
description: '&#x40;&#x40;CURSOR_ROWS (Transact-SQL)'
title: '@@CURSOR_ROWS (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@CURSOR_ROWS'
- '@@CURSOR_ROWS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@CURSOR_ROWS function'
- cursors [SQL Server], last-opened
- last-opened cursor
- asynchronous cursors [SQL Server]
ms.assetid: 31bd7a97-7f28-42a8-ba24-24d16d22973d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7df1f0df488313e5d13afe829ec25710477ffbcf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097200"
---
# <a name="x40x40cursor_rows-transact-sql"></a>&#x40;&#x40;CURSOR_ROWS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt die Anzahl der kennzeichnenden Zeilen zurück, die sich aktuell im letzten für die Verbindung geöffneten Cursor befinden. Zur Verbesserung der Leistung kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] große statische und keysetgesteuerte Cursor asynchron auffüllen. `@@CURSOR_ROWS` kann aufgerufen werden, um zu bestimmen, dass die Anzahl der einen Cursor kennzeichnenden Zeilen zum Zeitpunkt des @@CURSOR_ROWS-Aufrufs abgerufen wird.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
@@CURSOR_ROWS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
**integer**
  
## <a name="return-value"></a>Rückgabewert  
  
|Rückgabewert|BESCHREIBUNG|  
|---|---|
|-*m*|Der Cursor wird asynchron aufgefüllt. Der zurückgegebene Wert (-*m*) entspricht der aktuellen Anzahl der Zeilen im Keyset.|  
|-1|Der Cursor ist dynamisch. Da dynamische Cursor alle Änderungen widerspiegeln, ändert sich die Anzahl der Zeilen, die den Cursor kennzeichnen, ständig. Für den Cursor werden nicht unbedingt alle kennzeichnenden Zeilen abgerufen.|  
|0|Es wurden keine Cursor geöffnet, keine Zeilen kennzeichnen den zuletzt geöffneten Cursor, oder der zuletzt geöffnete Cursor wurde geschlossen oder seine Zuordnung aufgehoben.|  
|*n*|Der Cursor ist vollständig aufgefüllt. Der zurückgegebene Wert (*n*) entspricht der Gesamtanzahl der Zeilen im Cursor.|  
  
## <a name="remarks"></a>Hinweise  
`@@CURSOR_ROWS` gibt eine negative Zahl zurück, wenn der letzte Cursor asynchron geöffnet wurde. Keysetgesteuerte oder statische Cursor werden asynchron geöffnet, wenn der Wert für den Cursorschwellenwert „sp_configure“ größer als 0 (null) und die Anzahl der Zeilen im Cursorresultset größer als der Cursorschwellenwert ist.
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird zunächst ein Cursor deklariert und dann `SELECT` verwendet, um den Wert von `@@CURSOR_ROWS` anzuzeigen. Die Einstellung hat den Wert `0`, bevor der Cursor geöffnet wird, und hat dann den Wert `-1`, um zu kennzeichnen, dass das Keyset des Cursors asynchron aufgefüllt wird.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT @@CURSOR_ROWS;  
DECLARE Name_Cursor CURSOR FOR  
SELECT LastName ,@@CURSOR_ROWS FROM Person.Person;  
OPEN Name_Cursor;  
FETCH NEXT FROM Name_Cursor;  
SELECT @@CURSOR_ROWS;  
CLOSE Name_Cursor;  
DEALLOCATE Name_Cursor;  
GO             
```  
  
Im Folgenden werden die Resultsets aufgeführt.
  
```
-----------
0  
```

```
LastName
---------------
Sanchez
```

```
-----------
-1
```  
  
## <a name="see-also"></a>Weitere Informationen
[Cursorfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)  
[OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)
  
  
