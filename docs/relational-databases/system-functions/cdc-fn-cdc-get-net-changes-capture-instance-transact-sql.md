---
description: CDC.fn_cdc_get_net_changes_ &lt; capture_instance &gt; (Transact-SQL)
title: CDC.fn_cdc_get_net_changes_ &lt; capture_instance &gt; (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_net_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_net_changes_<capture_instance>
ms.assetid: 43ab0d1b-ead4-471c-85f3-f6c4b9372aab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7dee462b6b4a66b61d790bb06673d36f0907ef73
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099698"
---
# <a name="cdcfn_cdc_get_net_changes_ltcapture_instancegt-transact-sql"></a>CDC.fn_cdc_get_net_changes_ &lt; capture_instance &gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine netzwerkänderungzeile für jede Quellzeile zurück, die innerhalb des angegebenen LSN-Bereichs (Log Sequence Number) geändert wurde.  
  
 **Warten Sie, was ist eine LSN?** Jeder Datensatz im [SQL Server Transaktionsprotokoll](../logs/the-transaction-log-sql-server.md) wird eindeutig durch eine Protokoll Folge Nummer (Log Sequence Number, LSN) identifiziert. LSNs sind so angeordnet, dass LSN2, wenn größer als LSN1 ist, die durch den Protokolldaten Satz, auf den LSN2 verwiesen wird, **nach** der durch die Protokolldaten Satz-LSN beschriebenen Änderung aufgetreten ist.  
  
 Die LSN eines Protokolldaten Satzes, bei dem ein erhebliches Ereignis aufgetreten ist, kann zum Erstellen richtiger Wiederherstellungs Sequenzen hilfreich sein. Da LSNs geordnet sind, können Sie Sie auf Gleichheit und Ungleichheit (d \<, > . h.,, =, \<=, > =) vergleichen. Solche Vergleiche sind beim Erstellen von Wiederherstellungssequenzen hilfreich.  
  
 Wenn eine Quellzeile mehrere Änderungen während des LSN-Bereichs aufweist, wird eine einzelne Zeile, die den endgültigen Inhalt der Zeile wieder gibt, von der unten beschriebenen Enumerationsfunktion zurückgegeben. Wenn eine Transaktion z. b. eine Zeile in die Quell Tabelle einfügt und eine nachfolgende Transaktion innerhalb des LSN-Bereichs eine oder mehrere Spalten in dieser Zeile aktualisiert, gibt die Funktion nur **eine** Zeile zurück, die die aktualisierten Spaltenwerte enthält.  
  
 Diese Enumerationsfunktion wird erstellt, wenn eine Quelltabelle für Change Data Capture aktiviert wird und die Nettonachverfolgung angegeben ist. Um die Nettonachverfolgung zu aktivieren, muss die Quelltabelle einen Primärschlüssel oder einen eindeutigen Index aufweisen. Der Funktionsname wird abgeleitet und verwendet das Format cdc.fn_cdc_get_net_changes_ *capture_instance*. dabei ist *capture_instance* der Wert, der für die Aufzeichnungs Instanz angegeben wurde, als die Quell Tabelle für Change Data Capture aktiviert war. Weitere Informationen finden Sie unter [sys.sp_cdc_enable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
cdc.fn_cdc_get_net_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all with mask  
 | all with merge  
}  
```  
  
## <a name="arguments"></a>Argumente  
 *from_lsn*  
 Die LSN, die den unteren Endpunkt des LSN-Bereichs darstellt, der im Resultset enthalten sein soll. *from_lsn* ist **Binär (10)**.  
  
 Im Resultset sind nur Zeilen in der Tabelle [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) Änderungs Tabelle mit einem Wert in __ $ start_lsn größer oder gleich *from_lsn* enthalten.  
  
 *to_lsn*  
 Die LSN, die den hohen Endpunkt des LSN-Bereichs darstellt, der im Resultset enthalten sein soll. *to_lsn* ist **Binär (10)**.  
  
 Im Resultset sind nur Zeilen in der Tabelle [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) Änderung mit einem Wert in __ $ start_lsn kleiner oder gleich *from_lsn* oder *gleich to_lsn enthalten* .  
  
 *<row_filter_option>* :: = {all | all with mask | all with Merge}  
 Eine Option, die den Inhalt der Metadatenspalten sowie die im Resultset zurückgegebenen Zeilen bestimmt. Eine der folgenden Optionen ist möglich:  
  
 alle  
 Gibt die LSN der letzten Änderung an der Zeile und den Vorgang zurück, der zum Anwenden der Zeile in den Metadatenspalten __ $ start_lsn und $Operation erforderlich ist \_ \_ . Die Spalte \_ \_ $Update _Mask ist immer NULL.  
  
 all with mask  
 Gibt die LSN der letzten Änderung an der Zeile und den Vorgang zurück, der zum Anwenden der Zeile in den Metadatenspalten __ $ start_lsn und $Operation erforderlich ist \_ \_ . Wenn ein Aktualisierungs Vorgang ( \_ \_ $Operation = 4) zurückgibt, werden die aufgezeichneten Spalten, die im Update geändert wurden, in dem Wert gekennzeichnet, der in $Update _Mask zurückgegeben wird \_ \_ .  
  
 all with merge  
 Gibt die LSN der letzten Zeilenänderung in den Metadatenspalten __$start_lsn zurück. Die Spalte \_ \_ $Operation ist einer von zwei Werten: 1 für DELETE und 5, um anzugeben, dass der zum Anwenden der Änderung erforderliche Vorgang entweder eine Einfügung oder ein Update ist. Die Spalte \_ \_ $Update _Mask ist immer NULL.  
  
 Da die Bestimmung des präzisen Vorgangs für eine bestimmte Änderung die Logik der Abfragekomplexität erhöht, soll diese Option die Abfrageleistung in den Fällen verbessern, in denen zur Angabe des für die Änderungsanwendung notwendigen Vorgangs nur darauf hingewiesen werden muss, dass es sich entweder um einen Einfügevorgang oder um einen Updatevorgang handelt, und die explizite Unterscheidung der beiden Vorgänge nicht notwendig ist. Diese Option eignet sich besonders für Zielumgebungen, in denen Mergevorgänge direkt verfügbar sind, z. B. in einer [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]-Umgebung.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|__$start_lsn|**binary(10)**|LSN, die dem Commit für die Änderung zugeordnet wurde.<br /><br /> Alle Änderungen, für die ein Commit in derselben Transaktion ausgeführt wurde, verwenden dieselbe Commit-LSN. Wenn beispielsweise ein Aktualisierungs Vorgang in der Quell Tabelle zwei Spalten in zwei Zeilen ändert, enthält die Änderungs Tabelle vier Zeilen, die jeweils denselben __ $ start_lsnvalue.|  
|__$operation|**int**|Identifiziert den Vorgang der Datenbearbeitungssprache (Data Manipulation Language, DML), der erforderlich ist, um die Zeile der Änderungsdaten auf die Zieldatenquelle anzuwenden.<br /><br /> Wenn der Wert des row_filter_option-Parameters 'all' oder 'all with mask' ist, kann der Wert in dieser Spalte einen der folgenden Werte annehmen:<br /><br /> 1 = Löschen<br /><br /> 2 = Einfügen<br /><br /> 4 = Aktualisieren<br /><br /> Wenn der Wert des row_filter_option-Parameters 'all with merge' ist, kann der Wert in dieser Spalte einen der folgenden Werte annehmen:<br /><br /> 1 = Löschen|  
|__$update_mask|**varbinary(128)**|Eine Bitmaske mit einem Bit, das den einzelnen aufgezeichneten Spalten entspricht, die für die Aufzeichnungsinstanz identifiziert wurden. Für diesen Wert sind alle definierten Bits auf 1 festgelegt, wenn __$operation = 1 oder 2 ist. Wenn \_ \_ $Operation = 3 oder 4, werden nur die Bits, die den geänderten Spalten entsprechen, auf 1 festgelegt.|  
|*\<captured source table columns>*|Variiert|Bei den von der Funktion zurückgegebenen verbleibenden Spalten handelt es sich um die Spalten aus der Quelltabelle, die beim Erstellen der Aufzeichnungsinstanz als aufgezeichnete Spalten identifiziert wurden. Wenn in der Liste der aufgezeichneten Spalten keine Spalten angegeben wurden, werden alle Spalten in der Quelltabelle zurückgegeben.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Serverrolle sysadmin oder in der festen Datenbankrolle db_owner. Für alle anderen Benutzer ist die SELECT-Berechtigung für alle aufgezeichneten Spalten in der Quelltabelle und, wenn eine Gatingrolle für die Aufzeichnungsinstanz definiert wurde, eine Mitgliedschaft in dieser Datenbankrolle erforderlich. Wenn der Aufrufer nicht über die Berechtigungen zum Anzeigen der Quelldaten verfügt, gibt die Funktion Fehler 208 (Ungültiger Objektname) zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der angegebene LSN-Bereich nicht in den Zeitraum der Änderungsnachverfolgung der Aufzeichnungsinstanz fällt, gibt die Funktion Fehler 208 (Ungültiger Objektname) zurück.

 Änderungen am eindeutigen Bezeichner einer Zeile bewirken, dass fn_cdc_get_net_changes den anfänglichen Update-Befehl mit dem Befehl delete und then INSERT anzeigt.  Dieses Verhalten ist erforderlich, um den Schlüssel vor und nach der Änderung zu überprüfen.
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die-Funktion verwendet `cdc.fn_cdc_get_net_changes_HR_Department` , um die Netto Änderungen zu melden, die an der Quell Tabelle `HumanResources.Department` während eines bestimmten Zeitintervalls vorgenommen wurden.  
  
 Zuerst wird die `GETDATE`-Funktion verwendet, um den Anfang des Zeitintervalls zu markieren. Nachdem mehrere DML-Anweisungen auf die Quelltabelle angewendet wurden, wird die `GETDATE`-Funktion erneut aufgerufen, um das Ende des Zeitintervalls zu identifizieren. Die Funktion [sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) wird dann verwendet, um das Zeitintervall einem Change Data Capture Abfrage Bereich zuzuordnen, der von LSN-Werten festgelegt wird. Schließlich wird die `cdc.fn_cdc_get_net_changes_HR_Department`-Funktion abgefragt, um die Nettoänderungen an der Quelltabelle für das Zeitintervall zu erhalten. Beachten Sie, dass die eingefügte und anschließend gelöschte Zeile nicht in dem von der Funktion zurückgegebenen Resultset aufgeführt wird. Der Grund dafür ist, dass eine in einem Abfragefenster zuerst hinzugefügte und dann gelöschte Zeile keine Nettoänderung in der Quelltabelle für das Intervall erzeugt. Bevor Sie dieses Beispiel ausführen, müssen Sie zuerst Beispiel B in [sys.sp_cdc_enable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)ausführen.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @begin_time datetime, @end_time datetime, @from_lsn binary(10), @to_lsn binary(10);  
-- Obtain the beginning of the time interval.  
SET @begin_time = GETDATE() -1;  
-- DML statements to produce changes in the HumanResources.Department table.  
INSERT INTO HumanResources.Department (Name, GroupName)  
VALUES (N'MyDept', N'MyNewGroup');  
  
UPDATE HumanResources.Department  
SET GroupName = N'Resource Control'  
WHERE GroupName = N'Inventory Management';  
  
DELETE FROM HumanResources.Department  
WHERE Name = N'MyDept';  
  
-- Obtain the end of the time interval.  
SET @end_time = GETDATE();  
-- Map the time interval to a change data capture query range.  
SET @from_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than or equal', @begin_time);  
SET @to_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);  
  
-- Return the net changes occurring within the query window.  
SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@from_lsn, @to_lsn, 'all');  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CDC.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [Über Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
