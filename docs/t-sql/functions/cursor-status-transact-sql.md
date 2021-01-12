---
description: CURSOR_STATUS (Transact-SQL)
title: CURSOR_STATUS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURSOR_STATUS
- CURSOR_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], cursors
- CURSOR_STATUS function
- cursors [SQL Server], status information
ms.assetid: 3a4a840e-04f8-43bd-aada-35d78c3cb6b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 853cae9252c43a176d208b7208f2ca595366a151
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093690"
---
# <a name="cursor_status-transact-sql"></a>CURSOR_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Für einen bestimmten Parameter gibt `CURSOR_STATUS` an, ob eine Cursordeklaration einen Cursor und ein Resultset zurückgegeben hat oder nicht.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CURSOR_STATUS   
     (  
          { 'local' , 'cursor_name' }   
          | { 'global' , 'cursor_name' }   
          | { 'variable' , 'cursor_variable' }   
     )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
'local'  
Gibt eine Konstante an, die kennzeichnet, dass die Cursorquelle ein lokaler Cursorname ist.
  
'*cursor_name*'  
Der Name des Cursors. Ein Cursorname muss den [Regeln für Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.
  
'global'  
Gibt eine Konstante an, die kennzeichnet, dass die Quelle des Cursors ein globaler Cursorname ist.
  
'variable'  
Gibt eine Konstante an, die kennzeichnet, dass die Quelle des Cursors eine lokale Variable ist.
  
'*cursor_variable*'  
Der Name der Cursorvariablen. Eine Cursorvariable muss mithilfe des **cursor**-Datentyps definiert werden.
  
## <a name="return-types"></a>Rückgabetypen
**smallint**
  
|Rückgabewert|Cursorname|Cursorvariable|  
|---|---|---|
|1|Das Resultset des Cursors hat mindestens eine Zeile.<br /><br /> Für INSENSITIVE- und KEYSET-Cursor hat das Resultset mindestens eine Zeile.<br /><br /> Für dynamische Cursor kann das Resultset keine, eine oder mehrere Zeilen haben.|Der für diese Variable zugeordnete Cursor ist geöffnet.<br /><br /> Für INSENSITIVE- und KEYSET-Cursor hat das Resultset mindestens eine Zeile.<br /><br /> Für dynamische Cursor kann das Resultset keine, eine oder mehrere Zeilen haben.|  
|0|Das Resultset des Cursors ist leer.*|Der dieser Variable zugeordnete Cursor ist geöffnet, das Resultset ist jedoch definitiv leer.*|  
|-1|Der Cursor ist geschlossen.|Der dieser Variable zugeordnete Cursor ist geschlossen.|  
|-2|Nicht zutreffend|Eine der folgenden Möglichkeiten trifft zu:<br /><br /> Die zuvor aufgerufene Prozedur hat dieser OUTPUT-Variablen keinen Cursor zugewiesen.<br /><br /> Die zuvor zugewiesene Prozedur hat dieser OUTPUT-Variablen einen Cursor zugewiesen, aber der Cursor war bei Abschluss der Prozedur geschlossen. Deshalb wird die Zuordnung des Cursors aufgehoben, und der Cursor wird nicht an die aufrufende Prozedur zurückgegeben.<br /><br /> Der deklarierten Cursorvariablen wird kein Cursor zugewiesen.|  
|-3|Ein Cursor mit dem angegebenen Namen ist nicht vorhanden.|Eine Cursorvariable mit dem angegebenen Namen ist nicht vorhanden, oder ihr wurde, wenn sie vorhanden ist, noch kein Cursor zugeordnet.|  
  
* Dynamische Cursor geben dieses Ergebnis nie zurück.
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird die `CURSOR_STATUS`-Funktion verwendet, um den Status eines Cursors mitzuteilen: nach seiner Deklaration, nachdem er geöffnet und nachdem er geschlossen wurde.
  
```sql
CREATE TABLE #TMP  
(  
   ii INT  
)  
GO  
  
INSERT INTO #TMP(ii) VALUES(1)  
INSERT INTO #TMP(ii) VALUES(2)  
INSERT INTO #TMP(ii) VALUES(3)  
  
GO  
  
--Create a cursor.  
DECLARE cur CURSOR  
FOR SELECT * FROM #TMP  
  
--Display the status of the cursor before and after opening  
--closing the cursor.  
  
SELECT CURSOR_STATUS('global','cur') AS 'After declare'  
OPEN cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Open'  
CLOSE cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Close'  
  
--Remove the cursor.  
DEALLOCATE cur  
  
--Drop the table.  
DROP TABLE #TMP  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
After declare
---------------
-1  
  
After Open
----------
1  
  
After Close
-----------
-1
```  
  
## <a name="see-also"></a>Weitere Informationen
[Cursorfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)  
[Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
