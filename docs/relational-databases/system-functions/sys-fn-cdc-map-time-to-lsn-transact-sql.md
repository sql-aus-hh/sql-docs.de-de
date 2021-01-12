---
description: sys.fn_cdc_map_time_to_lsn (Transact-SQL)
title: sys.fn_cdc_map_time_to_lsn (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_map_time_to_lsn
- fn_cdc_map_time_to_lsn_TSQL
- sys.fn_cdc_map_time_to_lsn_TSQL
- fn_cdc_map_time_to_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_map_time_to_lsn
- sys.fn_cdc_map_time_to_lsn
ms.assetid: 6feb051d-77ae-4c93-818a-849fe518d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 38f07d3d3c46a46bc18f84d54b14809c201ee219
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094961"
---
# <a name="sysfn_cdc_map_time_to_lsn-transact-sql"></a>sys.fn_cdc_map_time_to_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den Wert der Protokollfolgenummer (LSN) für die angegebene Zeit in der **start_lsn** -Spalte der [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) -Systemtabelle zurück. Sie können diese Funktion verwenden, um DateTime-Bereiche systematisch dem LSN-basierten Bereich zuzuordnen, der von den Change Data Capture-Enumerationsfunktionen [CDC.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) und [CDC.fn_cdc_get_net_changes_ ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)<capture_instance>, um Datenänderungen innerhalb dieses Bereichs zurückzugeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.fn_cdc_map_time_to_lsn ( '<relational_operator>', tracking_time )  
  
<relational_operator> ::=  
{  largest less than  
 | largest less than or equal  
 | smallest greater than  
 | smallest greater than or equal  
}  
```  
  
## <a name="arguments"></a>Argumente  
 **"**<relational_operator>**"** {größte kleiner als das größte kleiner als oder gleich kleinste größer als das \| \| \| kleinste größer als oder gleich.  
 Wird verwendet, um einen eindeutigen LSN-Wert in der **cdc.lsn_time_mapping** -Tabelle mit zugeordnetem **tran_end_time** -Wert zu identifizieren, der der Beziehung entspricht, wenn er mit dem Wert *tracking_time* verglichen wird.  
  
 *relational_operator* ist **nvarchar(30)**  
  
 *tracking_time*  
 Der datetime-Wert, mit dem verglichen werden soll. *tracking_time* ist **datetime**  
  
## <a name="return-type"></a>Rückgabetyp  
 **binary(10)**  
  
## <a name="remarks"></a>Bemerkungen  
 Das folgende Szenario veranschaulicht, wie **sys.fn_cdc_map_time_lsn** verwendet werden kann, um datetime-Bereiche LSN-Bereichen zuzuordnen. Angenommen, ein Consumer möchte täglich Änderungsdaten extrahieren. In diesem Fall interessiert sich der Consumer nur für die Änderungen an einem bestimmten Tag bis einschließlich Mitternacht. Die Untergrenze des Zeitbereichs wäre bis Mitternacht des vorangehenden Tags (nicht einschließlich Mitternacht). Die Obergrenze wäre bis einschließlich Mitternacht des bestimmten Tags. Im folgenden Beispiel wird gezeigt, wie Sie die Funktion **sys.fn_cdc_map_time_to_lsn** verwenden können, um diesen zeitbasierten Bereich dem LSN-basierten Bereich zuzuordnen, der von den Change Data Capture-Enumerationsfunktionen benötigt wird, um alle Änderungen innerhalb dieses Bereichs zurückzugeben.  
  
 `DECLARE @begin_time datetime, @end_time datetime, @begin_lsn binary(10), @end_lsn binary(10);`  
  
 `SET @begin_time = '2007-01-01 12:00:00.000';`  
  
 `SET @end_time = '2007-01-02 12:00:00.000';`  
  
 `SELECT @begin_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than', @begin_time);`  
  
 `SELECT @end_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);`  
  
 `SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@begin_lsn, @end_lsn, 'all` `');`  
  
 Der relationale '`smallest greater than`'-Operator wird verwendet, um auf die Änderungen einzuschränken, die nach Mitternacht am vorangehenden Tag aufgetreten sind. Wenn mehrere Einträge mit unterschiedlichen LSN-Werten den Wert **tran_end_time** verwenden, der als untere Grenze in der [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) -Tabelle identifiziert ist, gibt die Funktion den kleinsten LSN-Wert zurück, um sicherzustellen, dass alle Einträge enthalten sind. Für die obere Grenze wird der relationale-Operator `largest less than or equal to` verwendet, um sicherzustellen, dass der Bereich alle Einträge für den Tag enthält, einschließlich der-Werte, die als **tran_end_time** Wert Mitternacht enthalten. Wenn mehrere Einträge mit unterschiedlichen LSN-Werten den Wert **tran_end_time** verwenden, der als obere Grenze identifiziert ist, wird die Funktion den größten LSN-Wert zurückgeben, um sicherzustellen, dass alle Einträge enthalten sind.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **public** -Rolle.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die `sys.fn_cdc_map_time_lsn` -Funktion verwendet, um zu bestimmen, ob es in der [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) -Tabelle Zeilen mit einem **tran_end_time** -Wert gibt, der größer oder gleich Mitternacht ist. Mit dieser Abfrage kann beispielsweise bestimmt werden, ob der Aufzeichnungsprozess bereits die Änderungen verarbeitet hat, für die bis Mitternacht des vorangehenden Tags ein Commit ausgeführt wurde, sodass mit dem Extrahieren von Änderungsdaten für diesen Tag fortgefahren werden kann.  
  
```  
DECLARE @extraction_time datetime, @lsn binary(10);  
SET @extraction_time = '2007-01-01 12:00:00.000';  
SELECT @lsn = sys.fn_cdc_map_time_to_lsn ('smallest greater than or equal', @extraction_time);  
IF @lsn IS NOT NULL  
BEGIN  
<some action>  
END  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CDC.lsn_time_mapping &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)   
 [sys.fn_cdc_map_lsn_to_time &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md)   
 [CDC.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
