---
description: sys.dm_db_objects_disabled_on_compatibility_level_change (Transact-SQL)
title: sys.dm_db_objects_disabled_on_compatibility_level_change (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_objects_disabled_on_compatibility_level_change
- dm_db_objects_disabled_on_compatibility_level_change_TSQL
- sys.dm_db_objects_disabled_on_compatibility_level_change
- sys.dm_db_objects_disabled_on_compatibility_level_change_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_objects_disabled_on_compatibility_level_change catalog view
ms.assetid: a5d70064-0330-48b9-b853-01eba50755d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4e5092468b8a6beebb9d5149d215b39f47db0818
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100026"
---
# <a name="spatial-data---sysdm_db_objects_disabled_on_compatibility_level_change"></a>Räumliche Daten-sys.dm_db_objects_disabled_on_compatibility_level_change
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Führt die Indizes und die Einschränkungen auf, die in Folge der Änderung des Kompatibilitätsgrads in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]deaktiviert werden. Indizes und Einschränkungen, die persistierte berechnete Spalten enthalten, deren Ausdrücke räumliche UDTs verwenden, werden nach einem Upgrade oder einer Änderung des Kompatibilitätsgrads deaktiviert. Bestimmen Sie die Auswirkungen einer Änderung des Kompatibilitätsgrads mithilfe dieser dynamischen Verwaltungsfunktion.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```sql  
sys.dm_db_objects_disabled_on_compatibility_level_change ( compatibility_level )   
```  
  
##  <a name="arguments"></a><a name="Arguments"></a>Argumente  
 *compatibility_level*  
 **int** -Wert, der den Kompatibilitätsgrad angibt, den Sie festlegen möchten.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**class**|**int**|1 = Einschränkungen<br /><br /> 7 = Indizes und Heaps|  
|**class_desc**|**nvarchar(60)**|OBJECT oder COLUMN für Einschränkungen<br /><br /> INDEX für Indizes und Heaps|  
|**major_id**|**int**|OBJECT ID der Einschränkungen<br /><br /> OBJECT ID der Tabelle, die Indizes und Heaps enthält|  
|**minor_id**|**int**|NULL für Einschränkungen<br /><br /> Index_id für Indizes und Heaps|  
|**gkeit**|**nvarchar(60)**|Beschreibung der Abhängigkeit, die bewirkt, dass die Einschränkung oder der Index deaktiviert wird. Die gleichen Werte werden auch in den Warnungen verwendet, die während des Upgrades ausgelöst werden. Einige Beispiele dafür sind:<br /><br /> "space" für eine systeminterne Funktion<br /><br /> "geometry" für einen System-UDT<br /><br /> "geography::Parse" für eine Methode eines System-UDTs|  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Persistente berechnete Spalten, die systeminterne Funktionen verwenden, werden bei einer Änderung des Kompatibilitätsgrads deaktiviert. Darüber hinaus werden persistierte berechnete Spalten, die eine Geometry-Methode oder Geography-Methode verwenden, beim Upgrade einer Datenbank deaktiviert.  
  
### <a name="which-functions-cause-persisted-computed-columns-to-be-disabled"></a>Welcher Funktionen haben eine Deaktivierung von persistierten berechneten Spalten zur Folge?  
 Wenn die folgenden Funktionen im Ausdruck einer persistierten berechneten Spalte verwendet werden, bewirken sie, dass Indizes und Einschränkungen, die auf diese Spalten verweisen, bei einer Änderung des Kompatibilitätsgrads von 80 auf 90 deaktiviert werden:  
  
-   **IsNumeric**  
  
 Wenn die folgenden Funktionen im Ausdruck einer persistierten berechneten Spalte verwendet werden, bewirken sie, dass Indizes und Einschränkungen, die auf diese Spalten verweisen, bei einer Änderung des Kompatibilitätsgrads von 100 auf 110 oder höher deaktiviert werden:  
  
-   **SOUNDEX**  
  
-   **Geografie:: GeomFromGML**  
  
-   **Geografie:: STGeomFromText**  
  
-   **Geography:: STLineFromText**  
  
-   **Geografie:: STPolyFromText**  
  
-   **Geografie:: STMPointFromText**  
  
-   **Geografie:: STMLineFromText**  
  
-   **Geografie:: STMPolyFromText**  
  
-   **Geografie:: STGeomCollFromText**  
  
-   **Geografie:: STGeomFromWKB**  
  
-   **Geografie:: STLineFromWKB**  
  
-   **Geografie:: STPolyFromWKB**  
  
-   **Geografie:: STMPointFromWKB**  
  
-   **Geografie:: STMLineFromWKB**  
  
-   **Geografie:: STMPolyFromWKB**  
  
-   **Geografie:: STUnion**  
  
-   **Geografie:: STIntersection**  
  
-   **Geografie:: STDifference**  
  
-   **Geografie:: STSymDifference**  
  
-   **Geografie:: STBuffer**  
  
-   **Geografie:: BufferWithTolerance**  
  
-   **Geography:: Analyse**  
  
-   **Geografie:: Reduce**  
  
### <a name="behavior-of-the-disabled-objects"></a>Verhalten der deaktivierten Objekte  
 **Indizes**  
  
 Wenn der gruppierte Index deaktiviert ist oder ein nicht gruppierter Index erzwungen wird, wird der folgende Fehler ausgelöst: "der Abfrage Prozessor kann keinen Plan durchführen, da der Index '%. \* ls ' für Tabelle oder Sicht '%. \* ls ' ist deaktiviert. " Um diese Objekte erneut zu aktivieren, erstellen Sie die Indizes nach dem Upgrade neu, indem Sie **Alter Index on... aufrufen. Neu erstellen**.  
  
 **Heaps**  
  
 Wenn eine Tabelle mit einem deaktivierten Heap verwendet wird, wird der folgende Fehler ausgelöst. Um diese Objekte erneut zu aktivieren, erstellen Sie nach dem Upgrade erneut, indem Sie **Alter Index all on... aufrufen. Neu erstellen**.  
  
```  
// ErrorNumber: 8674  
// ErrorSeverity: EX_USER  
// ErrorFormat: The query processor is unable to produce a plan because the table or view '%.*ls' is disabled.  
// ErrorCause: The table has a disabled heap.   
// ErrorCorrectiveAction: Rebuild the disabled heap to enable it.   
// ErrorInserts: table or view name   
// ErrorOwner: mtintor   
// ErrorFirstProduct: SQL11  
```  
  
 Wenn Sie versuchen, den Heap während eines Online Vorgangs neu zu erstellen, wird ein Fehler ausgelöst.  
  
 **CHECK-Einschränkungen und Fremdschlüssel**  
  
 Deaktivierte CHECK-Einschränkungen und Fremdschlüssel lösen keinen Fehler aus. Die Einschränkungen werden jedoch nicht erzwungen, wenn Zeilen geändert werden. Um diese Objekte erneut zu aktivieren, überprüfen Sie die Einschränkungen nach dem Upgrade durch Aufrufen von **ALTER TABLE... Check-Einschränkung**.  
  
 **Permanent berechnete Spalten**  
  
 Da eine Deaktivierung einzelner Spalten nicht möglich ist, wird die gesamte Tabelle deaktiviert, indem der gruppierte Index oder der Heap deaktiviert wird.  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird eine Abfrage für **sys.dm_db_objects_disabled_on_compatibility_level_change** angezeigt, durch die Objekte gesucht werden, auf die sich eine Änderung des Kompatibilitätsgrads in 120 auswirkt.  
  
```sql  
SELECT * FROM sys.dm_db_objects_disabled_on_compatibility_level_change(120);  
GO  
  
```  
  
  
