---
description: sys.fn_cdc_get_min_lsn (Transact-SQL)
title: sys.fn_cdc_get_min_lsn (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_get_min_lsn
- fn_cdc_get_min_lsn
- fn_cdc_get_min_lsn_TSQL
- sys.fn_cdc_get_min_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_min_lsn
- sys.fn_cdc_get_min_lsn
ms.assetid: bd49e28a-128b-4f6b-8545-6a2ec3f4afb3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34f941c959d56e4526a73bd186e18cafeda43660
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099669"
---
# <a name="sysfn_cdc_get_min_lsn-transact-sql"></a>sys.fn_cdc_get_min_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den start_lsn Spaltenwert für die angegebene Aufzeichnungs Instanz aus der [CDC.change_tables](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md) Systemtabelle zurück. Dieser Wert stellt den unteren Endpunkt des Gültigkeitsintervalls für die Aufzeichnungsinstanz dar.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_cdc_get_min_lsn ( 'capture_instance_name' )  
```  
  
## <a name="arguments"></a>Argumente  
 **"** *capture_instance_name* **"**  
 Der Name der Aufzeichnungsinstanz. *capture_instance_name* ist vom **Datentyp vom Datentyp sysname**.  
  
## <a name="return-types"></a>Rückgabetypen  
 **binary(10)**  
  
## <a name="remarks"></a>Bemerkungen  
 Gibt 0x00000000000000000000 zurück, wenn die Aufzeichnungsinstanz nicht vorhanden oder der Aufrufer nicht berechtigt ist, auf die Änderungsdaten zuzugreifen, die der Aufzeichnungsinstanz zugeordnet sind.  
  
 Diese Funktion dient gewöhnlich zum Identifizieren des unteren Endpunkts der Change Data Capture-Zeitachse, die einer Aufzeichnungsinstanz zugeordnet ist. Außerdem können Sie mit dieser Funktion vor der Anforderung von Änderungsdaten überprüfen, ob die Endpunkte eines Abfragebereichs innerhalb der Zeitachse der Aufzeichnungsinstanz liegen. Es ist wichtig, solche Überprüfungen durchzuführen, da sich der niedrige Endpunkt einer Aufzeichnungs Instanz ändert, wenn ein Bereinigung für die Änderungs Tabellen ausgeführt wird. Falls ein längerer Zeitraum zwischen den Anforderungen von Änderungsdaten vergeht, könnte sogar ein niedriger Endpunkt, der auf den oberen Endpunkt der vorherigen Anforderung von Änderungsdaten festgelegt wurde, außerhalb der aktuellen Zeitachse liegen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle sysadmin oder in der festen Datenbankrolle db_owner. Für alle anderen Benutzer ist die SELECT-Berechtigung für alle aufgezeichneten Spalten in der Quelltabelle und, wenn eine Gatingrolle für die Aufzeichnungsinstanz definiert wurde, eine Mitgliedschaft in dieser Datenbankrolle erforderlich.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-the-minimum-lsn-value-for-a-specified-capture-instance"></a>A. Zurückgeben des kleinsten LSN-Werts für eine angegebene Aufzeichnungsinstanz  
 Im folgenden Beispiel wird der kleinste LSN-Wert für die Aufzeichnungsinstanz `HumanResources_Employee` in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben.  
  
```  
USE AdventureWorks2-12;  
GO  
SELECT sys.fn_cdc_get_min_lsn ('HumanResources_Employee')AS min_lsn;  
  
```  
  
### <a name="b-verifying-the-low-endpoint-of-a-query-range"></a>B. Überprüfen des unteren Endpunkts eines Abfragebereichs  
 Im folgenden Beispiel wird der kleinste von `sys.fn_cdc_get_min_lsn` zurückgegebene LSN-Wert verwendet, um zu überprüfen, ob der vorgeschlagene untere Endpunkt für eine Abfrage von Änderungsdaten für die aktuelle Zeitachse der Aufzeichnungsinstanz `HumanResources_Employee` gültig ist. In diesem Beispiel wird davon ausgegangen, dass die LSN des vorherigen oberen Endpunkts für die Aufzeichnungsinstanz gespeichert wurde und für die Festlegung der `@save_to_lsn`-Variablen verfügbar ist. Für dieses Beispiel wird `@save_to_lsn` auf 0x000000000000000000 festgelegt, um die Ausführung des Fehlerbehandlungsabschnitts zu erzwingen.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @min_lsn binary(10), @from_lsn binary(10),@save_to_lsn binary(10), @to_lsn binary(10);  
-- Sets @save_to_lsn to the previous high endpoint saved from the last change data request.  
SET @save_to_lsn = 0x000000000000000000;  
-- Sets the upper endpoint for the query range to the current maximum LSN.  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
-- Sets the @min_lsn parameter to the current minimum LSN for the capture instance.  
SET @min_lsn = sys.fn_cdc_get_min_lsn ('HumanResources_Employee');  
-- Sets the low endpoint for the query range to the LSN that follows the previous high endpoint.  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
-- Tests to verify the low endpoint is valid for the current capture instance.  
IF (@from_lsn < @min_lsn)  
    BEGIN  
        RAISERROR('Low endpoint of the request interval is invalid.', 16, -1);  
    END  
ELSE  
-- Return the changes occurring within the query range.  
    SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee(@from_lsn, @to_lsn, 'all');  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.fn_cdc_get_max_lsn &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md)   
 [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)  
  
  
