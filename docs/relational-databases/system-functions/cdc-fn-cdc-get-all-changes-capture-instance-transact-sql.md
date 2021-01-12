---
description: CDC.fn_cdc_get_all_changes_ &lt; capture_instance &gt;  (Transact-SQL)
title: CDC.fn_cdc_get_all_changes_ &lt; capture_instance &gt;  (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_all_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_all_changes_<capture_instance>
ms.assetid: c6bad147-1449-4e20-a42e-b51aed76963c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8bb04e74ab2dd613173bf194fe4ca5412d79ac7e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095031"
---
# <a name="cdcfn_cdc_get_all_changes_ltcapture_instancegt--transact-sql"></a>CDC.fn_cdc_get_all_changes_ &lt; capture_instance &gt;  (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede auf die Quelltabelle innerhalb des angegebenen Bereichs der Protokollfolgenummer (Log Sequence Number, LSN) angewendete Änderung an. Wenn an einer Quellzeile während des Intervalls mehrere Änderungen vorgenommen wurden, wird jede Änderung im zurückgegebenen Resultset dargestellt. Zusätzlich zum Zurückgeben der Änderungsdaten stellen vier Metadatenspalten die Informationen bereit, die Sie zum Anwenden der Änderungen auf eine andere Datenquelle benötigen. Über Zeilenfilterungsoptionen werden der Inhalt der Metadatenspalten sowie die im Resultset zurückgegebenen Zeilen bestimmt. Wenn die Filteroption 'all' angegeben ist, verfügt jede Änderung über genau eine Zeile, um die Änderung zu identifizieren. Wenn die Option 'all update old' angegeben ist, werden die Updatevorgänge in zwei Zeilen dargestellt: Eine enthält die Werte der aufgezeichneten Spalten vor dem Update und die andere enthält die Werte der aufgezeichneten Spalten nach dem Update.  
  
 Diese Enumerationsfunktion wird zu dem Zeitpunkt erstellt, zu dem eine Quelltabelle für Change Data Capture aktiviert wird. Der Funktionsname wird abgeleitet und verwendet das Format **CDC.fn_cdc_get_all_changes_**_capture_instance_ , wobei *capture_instance* der für die Aufzeichnungs Instanz angegebene Wert ist, wenn die Quell Tabelle für Change Data Capture aktiviert ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
cdc.fn_cdc_get_all_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all update old  
}  
```  
  
## <a name="arguments"></a>Argumente  
 *from_lsn*  
 Der LSN-Wert, der den unteren Endpunkt des LSN-Bereichs darstellt, der im Resultset enthalten sein soll. *from_lsn* ist **Binär (10)**.  
  
 Im Resultset sind nur Zeilen in der Tabelle [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) Änderungs Tabelle mit einem Wert in **__ $ start_lsn** größer oder gleich *from_lsn* enthalten.  
  
 *to_lsn*  
 Der LSN-Wert, der den oberen Endpunkt des LSN-Bereichs darstellt, der im Resultset enthalten sein soll. *to_lsn* ist **Binär (10)**.  
  
 Im Resultset sind nur Zeilen in der Tabelle [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) Änderung mit einem Wert in **__ $ start_lsn** größer als oder gleich *from_lsn* und kleiner als oder gleich *to_lsn* enthalten.  
  
 <row_filter_option>:: = {alle | alle Updates alt}  
 Eine Option, die den Inhalt der Metadatenspalten sowie die im Resultset zurückgegebenen Zeilen bestimmt.  
  
 Eine der folgenden Optionen ist möglich:  
  
 alle  
 Gibt alle Änderungen innerhalb des angegebenen LSN-Bereichs zurück. Bei Änderungen aufgrund eines Updatevorgangs gibt diese Option nur die Zeilen zurück, die die neuen Werte enthalten, nachdem das Update angewendet wurde.  
  
 all update old  
 Gibt alle Änderungen innerhalb des angegebenen LSN-Bereichs zurück. Bei Änderungen aufgrund eines Update Vorgangs gibt diese Option sowohl die Zeile mit den Spaltenwerten vor dem Update als auch die Zeile zurück, die die Spaltenwerte nach dem Update enthält.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|Commit-LSN, die der Änderung zugeordnet ist, die die Commitreihenfolge der Änderung beibehält. Änderungen, für die ein Commit in derselben Transaktion ausgeführt wurde, verwenden denselben Commit-LSN-Wert.|  
|**__$seqval**|**binary(10)**|Sequenzwert, der verwendet wird, um Änderungen an einer Zeile innerhalb einer Transaktion zu sortieren.|  
|**__$operation**|**int**|Identifiziert den Vorgang der Datenbearbeitungssprache (Data Manipulation Language, DML), der erforderlich ist, um die Zeile der Änderungsdaten auf die Zieldatenquelle anzuwenden. Dabei kann es sich um eine der folgenden Methoden handeln:<br /><br /> 1 = Löschen<br /><br /> 2 = Einfügen<br /><br /> 3 = Update (die aufgezeichneten Spaltenwerte liegen vor dem Updatevorgang). Dieser Wert ist nur gültig, wenn die Zeilenfilteroption 'all update old' angegeben ist.<br /><br /> 4 = Update (die aufgezeichneten Spaltenwerte liegen nach dem Updatevorgang).|  
|**__$update_mask**|**varbinary(128)**|Eine Bitmaske mit einem Bit, das den einzelnen aufgezeichneten Spalten entspricht, die für die Aufzeichnungsinstanz identifiziert wurden. Für diesen Wert sind alle definierten Bits auf 1 festgelegt, wenn **__ $ Operation** = 1 oder 2 ist. Wenn **__ $ Operation** = 3 oder 4, werden nur die Bits, die den geänderten Spalten entsprechen, auf 1 festgelegt.|  
|**\<captured source table columns>**|Variiert|Bei den verbleibenden Spalten, die von der Funktion zurückgegeben werden, handelt es sich um die aufgezeichneten Spalten, die beim Erstellen der Aufzeichnungsinstanz identifiziert wurden. Wenn in der Liste der aufgezeichneten Spalten keine Spalten angegeben wurden, werden alle Spalten in der Quelltabelle zurückgegeben.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Server Rolle **sysadmin** oder **db_owner** festen Daten Bank Rolle. Für alle anderen Benutzer ist die SELECT-Berechtigung für alle aufgezeichneten Spalten in der Quelltabelle und, wenn eine Gatingrolle für die Aufzeichnungsinstanz definiert wurde, eine Mitgliedschaft in dieser Datenbankrolle erforderlich. Wenn der Aufrufer nicht über die Berechtigung zum Anzeigen der Quelldaten verfügt, gibt die Funktion den Fehler 229 zurück ("die SELECT-Berechtigung wurde für das Objekt ' fn_cdc_get_all_changes_... ', Datenbank ' \<DatabaseName> ', Schema ' CDC '." verweigert.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der angegebene LSN-Bereich nicht in den Zeitraum der Änderungsnachverfolgung für die Aufzeichnungsinstanz fällt, gibt die Funktion Fehler 208 zurück ("Für die 'cdc.fn_cdc_get_all_changes'-Prozedur oder -Funktion wurden zu wenig Argumente bereitgestellt").  
  
 Spalten des Datentyps **Image**, **Text** und **ntext** wird immer ein NULL-Wert zugewiesen, wenn **__ $ Operation** = 1 oder **__ $ Operation** = 3. Spalten vom Datentyp **varbinary (max)**, **varchar (max)** oder **nvarchar (max)** wird ein NULL-Wert zugewiesen, wenn **__ $ Operation** = 3, es sei denn, die Spalte wurde während des Updates geändert. Wenn **__ $ Operation** = 1, werden diesen Spalten zum Zeitpunkt der Löschung ihre Werte zugewiesen. Berechnete Spalten in einer Aufzeichnungsinstanz besitzen immer den Wert NULL.  
  
## <a name="examples"></a>Beispiele  
 Es stehen mehrere [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Vorlagen zur Verfügung, die zeigen, wie die Change Data Capture Abfragefunktionen verwendet werden. Diese Vorlagen sind im Menü **Ansicht** in verfügbar [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Weitere Informationen finden Sie unter [Vorlagen-Explorer](../../ssms/template/template-explorer.md).  
  
 Im folgenden Beispiel wird die `Enumerate All Changes for Valid Range Template` veranschaulicht. Darin werden mit der Funktion `cdc.fn_cdc_get_all_changes_HR_Department` alle derzeit verfügbaren Änderungen für die Aufzeichnungsinstanz `HR_Department` gemeldet, die für die Quelltabelle HumanResources.Department in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank definiert ist.  
  
```  
-- ========  
-- Enumerate All Changes for Valid Range Template  
-- ========  
USE AdventureWorks2012;  
GO  
  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HR_Department');  
SET @to_lsn   = sys.fn_cdc_get_max_lsn();  
SELECT * FROM cdc.fn_cdc_get_all_changes_HR_Department  
  (@from_lsn, @to_lsn, N'all');  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CDC.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)   
 [sys.sp_cdc_get_captured_columns &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [Über Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
