---
description: SET NUMERIC_ROUNDABORT (Transact-SQL)
title: SET NUMERIC_ROUNDABORT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NUMERIC_ROUNDABORT
- SET_NUMERIC_ROUNDABORT_TSQL
- SET NUMERIC_ROUNDABORT
- NUMERIC_ROUNDABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- precision [SQL Server], rounded expressions
- expressions [SQL Server], rounding
- NUMERIC_ROUNDABORT
- SET NUMERIC_ROUNDABORT statement
ms.assetid: d20e74f1-b8da-466c-b180-9d8a8b851a77
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6cac574403a39550d7f838544a9d88b42f8f9fdf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100740"
---
# <a name="set-numeric_roundabort-transact-sql"></a>SET NUMERIC_ROUNDABORT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gibt an, welche Fehlerberichtsstufe generiert wird, wenn beim Runden in einem Ausdruck Genauigkeitsverluste entstehen.  
  
![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Artikellinksymbol") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntax

```syntaxsql

SET NUMERIC_ROUNDABORT { ON | OFF }
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
Wenn SET NUMERIC_ROUNDABORT auf ON festgelegt ist, wird ein Fehler generiert, wenn ein Genauigkeitsverlust in einem Ausdruck aufgetreten ist. Wenn OFF festgelegt ist, werden keine Fehlermeldungen generiert, wenn ein Genauigkeitsverlust auftritt. Das Ergebnis wird auf die Genauigkeit der Spalte oder Variable gerundet, die das Ergebnis speichert.  
  
Ein Genauigkeitsverlust entsteht, wenn Sie versuchen einen Wert mit fester Genauigkeit in einer Spalte oder Variablen mit geringerer Genauigkeit zu speichern.  
  
Wenn SET NUMERIC_ROUNDABORT auf ON festgelegt ist, bestimmt SET ARITHABORT den Schweregrad des generierten Fehlers. Die folgende Tabelle zeigt die Auswirkungen dieser beiden Einstellungen im Falle eines Genauigkeitsverlusts.  
  
|Einstellung|SET NUMERIC_ROUNDABORT ON|SET NUMERIC_ROUNDABORT OFF|
|-------------|--------------------------------|---------------------------------|
|SET ARITHABORT ON|Fehler wird generiert; es werden keine Ergebnisse zurückgegeben.|Keine Fehler oder Warnungen, Ergebnis wird gerundet.|  
|SET ARITHABORT OFF|Warnung wird zurückgegeben; Ausdruck gibt NULL zurück.|Keine Fehler oder Warnungen, Ergebnis wird gerundet.|  

Die Einstellung von SET NUMERIC_ROUNDABORT wird zur Ausführungszeit und nicht zur Analysezeit festgelegt.

SET NUMERIC_ROUNDABORT muss beim Erstellen oder Ändern von Indizes für berechnete Spalten oder indizierte Sichten auf OFF festgelegt sein. Wenn SET NUMERIC_ROUNDABORT auf ON festgelegt ist, schlagen die folgenden Anweisungen in Tabellen mit Indizes für berechnete Spalten oder indizierte Sicht fehl:

- CREATE 
- UPDATE 
- INSERT 
- Delete 

Weitere Informationen zu den erforderlichen Einstellungen der SET-Option mit indizierten Sichten und Indizes für berechnete Spalten finden Sie in den [Überlegungen zum Verwenden von SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements).
  
Um die aktuelle Einstellung anzuzeigen, führen Sie die folgende Abfrage aus:
  
```sql
DECLARE @NUMERIC_ROUNDABORT VARCHAR(3) = 'OFF';  
IF ( (8192 & @@OPTIONS) = 8192 ) SET @NUMERIC_ROUNDABORT = 'ON';  
SELECT @NUMERIC_ROUNDABORT AS NUMERIC_ROUNDABORT;  
  
```  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel werden zwei Werte gezeigt, die auf vier Dezimalstellen genau sind. Diese werden zu einer Variablen hinzugefügt und in dieser gespeichert, die auf zwei Dezimalstellen genau ist. Die Ausdrücke zeigen die Auswirkungen der unterschiedlichen `SET NUMERIC_ROUNDABORT`- und `SET ARITHABORT`-Einstellungen.  
  
```sql
-- SET NOCOUNT to ON,   
-- SET NUMERIC_ROUNDABORT to ON, and SET ARITHABORT to ON.  
SET NOCOUNT ON;  
PRINT 'SET NUMERIC_ROUNDABORT ON';  
PRINT 'SET ARITHABORT ON';  
SET NUMERIC_ROUNDABORT ON;  
SET ARITHABORT ON;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to ON and SET ARITHABORT to OFF.  
PRINT 'SET NUMERIC_ROUNDABORT ON';  
PRINT 'SET ARITHABORT OFF';  
SET NUMERIC_ROUNDABORT ON;  
SET ARITHABORT OFF;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to OFF and SET ARITHABORT to ON.  
PRINT 'SET NUMERIC_ROUNDABORT OFF';  
PRINT 'SET ARITHABORT ON';  
SET NUMERIC_ROUNDABORT OFF;  
SET ARITHABORT ON;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234 ;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
  
-- SET NUMERIC_ROUNDABORT to OFF and SET ARITHABORT to OFF.  
PRINT 'SET NUMERIC_ROUNDABORT OFF';  
PRINT 'SET ARITHABORT OFF';  
SET NUMERIC_ROUNDABORT OFF;  
SET ARITHABORT OFF;  
GO  
DECLARE @result DECIMAL(5, 2),  
   @value_1 DECIMAL(5, 4),   
   @value_2 DECIMAL(5, 4);  
SET @value_1 = 1.1234;  
SET @value_2 = 1.1234;  
SELECT @result = @value_1 + @value_2;  
SELECT @result;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
[SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
[SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md)  
  
  
