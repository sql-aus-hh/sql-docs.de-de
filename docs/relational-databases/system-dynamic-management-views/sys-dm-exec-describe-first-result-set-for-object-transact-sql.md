---
description: sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
title: sys.dm_exec_describe_first_result_set_for_object (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_describe_first_result_set_for_object_TSQL
- sys.dm_exec_describe_first_result_set_for_object
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set_for_object catalog view
ms.assetid: 63b0fde7-95d7-4ad7-a219-a9feacf1bd89
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 490f2197f1399bd89b83a920085829c96a4ed52c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101638"
---
# <a name="sysdm_exec_describe_first_result_set_for_object-transact-sql"></a>sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Diese dynamische Verwaltungsfunktion nimmt einen @object_id als Parameter an und beschreibt die ersten Ergebnis Metadaten für das Modul mit dieser ID. Der @object_id angegebene kann die ID einer [!INCLUDE[tsql](../../includes/tsql-md.md)] gespeicherten Prozedur oder eines- [!INCLUDE[tsql](../../includes/tsql-md.md)] Auslösers sein. Wenn es sich um die ID eines beliebigen anderen Objekts (z. B. einer Sicht, Tabelle, Funktion oder CLR-Prozedur) handelt, wird in den Fehlerspalten des Ergebnisses ein Fehler angegeben.  
  
 **sys.dm_exec_describe_first_result_set_for_object** hat dieselbe resultsetdefinition wie [sys.dm_exec_describe_first_result_set &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) und ähnelt [sp_describe_first_result_set &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.dm_exec_describe_first_result_set_for_object   
    ( @object_id , @include_browse_information )  
```  
  
## <a name="arguments"></a>Argumente  
 *\@object_id*  
 Der einer @object_id [!INCLUDE[tsql](../../includes/tsql-md.md)] gespeicherten Prozedur oder eines- [!INCLUDE[tsql](../../includes/tsql-md.md)] Auslösers. @object_id ist vom Typ **int**.  
  
 *\@include_browse_information*  
 @include_browse_information ist vom Typ **Bit**. Bei 1 werden alle Abfragen so analysiert, als ob die FOR BROWSE-Option in der Abfrage enthalten wäre. Gibt zusätzliche Schlüsselspalten und Quelltabelleninformationen zurück.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
 Diese allgemeinen Metadaten werden in den Ergebnismetadaten als Resultset mit einer Zeile für jede Spalte zurückgegeben. Jede Zeile beschreibt den Typ und die NULL-Zulässigkeit der Spalte in dem Format, das im folgenden Abschnitt beschriebenen wird. Wenn die erste Anweisung nicht für alle Steuerelementpfade vorhanden ist, wird ein Resultset mit 0 Zeilen zurückgegeben.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Gibt an, ob es sich um eine zusätzliche Spalte für Zwecke der Informationssuche handelt, die nicht im Resultset angezeigt wird.|  
|**column_ordinal**|**int**|Enthält die Ordnungsposition der Spalte im Resultset. Die Position der ersten Spalte wird mit 1 angegeben.|  
|**name**|**sysname**|Enthält den Namen der Spalte, wenn ein Name bestimmt werden kann. Andernfalls NULL.|  
|**is_nullable**|**bit**|Enthält den Wert 1, wenn die Spalte NULL-Werte zulässt, 0, wenn die Spalte keine NULL-Werte zulässt, und -1, wenn nicht ermittelt werden kann, ob die Spalte NULL-Werte zulässt.|  
|**system_type_id**|**int**|Enthält die system_type_id des Datentyps der Spalte, wie in sys. types angegeben. Bei CLR-Typen wird von dieser Spalte der Wert 240 zurückgegeben, obwohl von der system_type_name-Spalte NULL zurückgegeben wird.|  
|**system_type_name**|**nvarchar(256)**|Enthält den Namen des Datentyps. Enthält Argumente, die für den Datentyp der Spalte angegeben wurden (z. B. Länge, Genauigkeit, Skala). Wenn der Datentyp ein benutzerdefinierter Aliastyp ist, wird der zugrunde liegende Systemtyp hier angegeben. Bei einem benutzerdefinierten CLR-Typ wird NULL in dieser Spalte zurückgegeben.|  
|**max_length**|**smallint**|Maximale Länge (in Byte) für die Spalte.<br /><br /> -1 = der Spaltendatentyp ist **varchar (max)**, **nvarchar (max)**, **varbinary (max)** oder **XML**.<br /><br /> Bei **Text** Spalten ist der **max_length** Wert 16 oder der Wert, der durch **sp_tableoption ' Text in row '** festgelegt wird.|  
|**precision**|**tinyint**|Die Genauigkeit der Spalte, wenn sie auf numerischen Werten basiert. Andernfalls wird 0 zurückgegeben.|  
|**scale**|**tinyint**|Die Skalierung der Spalte, wenn sie auf numerischen Werten basiert. Andernfalls wird 0 zurückgegeben.|  
|**collation_name**|**sysname**|Name der Sortierung der Spalte, wenn diese zeichenbasiert ist. Andernfalls wird NULL zurückgegeben.|  
|**user_type_id**|**int**|Enthält bei CLR- und Aliastypen die user_type_id des Datentyps der Spalte, wie in sys.types angegeben. Andernfalls NULL.|  
|**user_type_database**|**sysname**|Enthält bei CLR- und Aliastypen den Namen der Datenbank, in der der Typ definiert wurde. Andernfalls NULL.|  
|**user_type_schema**|**sysname**|Enthält bei CLR- und Aliastypen den Namen des Schemas, in dem der Typ definiert wurde. Andernfalls NULL.|  
|**user_type_name**|**sysname**|Enthält bei CLR- und Aliastypen den Namen des Typs. Andernfalls NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Gibt bei CLR-Typen den Namen der Assembly und der Klasse zurück, die den Typ definieren. Andernfalls NULL.|  
|**xml_collection_id**|**int**|Enthält die xml_collection_id des Datentyps für die Spalte, wie in sys.columns angegeben. Diese Spalte gibt NULL zurück, wenn der zurückgegebene Typ keiner XML-Schemaauflistung zugeordnet ist.|  
|**xml_collection_database**|**sysname**|Enthält die Datenbank, in der die XML-Schemaauflistung definiert ist, die diesem Typ zugeordnet wurde. Diese Spalte gibt NULL zurück, wenn der zurückgegebene Typ keiner XML-Schemaauflistung zugeordnet ist.|  
|**xml_collection_schema**|**sysname**|Enthält das Schema, in dem die XML-Schemaauflistung definiert ist, die diesem Typ zugeordnet wurde. Diese Spalte gibt NULL zurück, wenn der zurückgegebene Typ keiner XML-Schemaauflistung zugeordnet ist.|  
|**xml_collection_name**|**sysname**|Enthält den Namen der XML-Schemaauflistung, die diesem Typ zugeordnet wurde. Diese Spalte gibt NULL zurück, wenn der zurückgegebene Typ keiner XML-Schemaauflistung zugeordnet ist.|  
|**is_xml_document**|**bit**|Gibt 1 zurück, wenn der zurückgegebene Datentyp XML ist und für diesen Typ garantiert ist, dass es sich um ein vollständiges XML-Dokument (einschließlich eines Stammknotens) handelt, nicht um ein XML-Fragment. Andernfalls wird 0 zurückgegeben.|  
|**is_case_sensitive**|**bit**|Gibt 1 zurück, wenn die Spalte von einem Zeichenfolgentyp ist, bei dem die Groß-/Kleinschreibung beachtet wird, andernfalls 0.|  
|**is_fixed_length_clr_type**|**bit**|Gibt 1 zurück, wenn die Spalte von einem CLR-Typ mit fester Länge ist, andernfalls 0.|  
|**source_server**|**sysname**|Der Name des ursprünglichen Servers, der von der Spalte in diesem Ergebnis zurückgegeben wurde (bei einem Remoteserver). Der Name wird wie in sys. Servers angezeigt.  Gibt NULL zurück, wenn die Spalte vom lokalen Server stammt oder der ursprüngliche Server nicht ermittelt werden konnte. Wird nur aufgefüllt, wenn die Suche nach Informationen erforderlich ist.|  
|**source_database**|**sysname**|Der Name der ursprünglichen Datenbank, die von der Spalte in diesem Ergebnis zurückgegeben wird. Gibt NULL zurück, wenn die Datenbank nicht ermittelt werden kann. Wird nur aufgefüllt, wenn die Suche nach Informationen erforderlich ist.|  
|**source_schema**|**sysname**|Der Name des ursprünglichen Schemas, das von der Spalte in diesem Ergebnis zurückgegeben wird. Gibt NULL zurück, wenn das Schema nicht bestimmt werden kann. Wird nur aufgefüllt, wenn die Suche nach Informationen erforderlich ist.|  
|**source_table**|**sysname**|Der Name der ursprünglichen Tabelle, die von der Spalte in diesem Ergebnis zurückgegeben wird. Gibt NULL zurück, wenn die Tabelle nicht bestimmt werden kann. Wird nur aufgefüllt, wenn die Suche nach Informationen erforderlich ist.|  
|**source_column**|**sysname**|Der Name der ursprünglichen Spalte, die von der Spalte in diesem Ergebnis zurückgegeben wird. Gibt NULL zurück, wenn die Spalte nicht bestimmt werden kann. Wird nur aufgefüllt, wenn die Suche nach Informationen erforderlich ist.|  
|**is_identity_column**|**bit**|Gibt 1 zurück, wenn die Spalte eine Identitätsspalte ist, andernfalls 0. Gibt NULL zurück, wenn nicht ermittelt werden kann, ob die Spalte eine Identitätsspalte ist.|  
|**is_part_of_unique_key**|**bit**|Gibt 1 zurück, wenn die Spalte Teil eines eindeutigen Index (einschließlich UNIQUE- und PRIMARY-Einschränkung) ist, andernfalls 0. Gibt NULL zurück, wenn nicht ermittelt werden kann, ob die Spalte Teil eines eindeutigen Indexes ist. Wird nur aufgefüllt, wenn die Suche nach Informationen erforderlich ist.|  
|**is_updateable**|**bit**|Gibt 1 zurück, wenn die Spalte aktualisiert werden kann, andernfalls 0. Gibt NULL zurück, wenn nicht ermittelt werden kann, ob die Spalte aktualisiert werden kann.|  
|**is_computed_column**|**bit**|Gibt 1 zurück, wenn die Spalte eine berechnete Spalte, andernfalls 0. Gibt NULL zurück, wenn nicht ermittelt werden kann, ob die Spalte eine berechnete Spalte ist.|  
|**is_sparse_column_set**|**bit**|Gibt 1 zurück, wenn die Spalte eine Sparsespalte ist, andernfalls 0. Gibt NULL zurück, wenn nicht ermittelt werden kann, ob die Spalte Teil eines Sparsespaltensatzes ist.|  
|**ordinal_in_order_by_list**|**smallint**|Position der Spalte in der ORDER BY-Liste. Gibt NULL zurück, wenn die Spalte nicht in der ORDER BY-Liste angezeigt wird oder die ORDER BY-Liste nicht eindeutig bestimmt werden kann.|  
|**order_by_list_length**|**smallint**|Die Länge der ORDER BY-Liste. Gibt NULL zurück, wenn keine ORDER BY-Liste vorhanden ist oder die ORDER BY-Liste nicht eindeutig bestimmt werden kann. Beachten Sie, dass dieser Wert für alle von sp_describe_first_result_set zurückgegebenen Zeilen gleich ist.|  
|**order_by_is_descending**|**smallint NULL**|Wenn ordinal_in_order_by_list nicht NULL ist, wird von der **order_by_is_descending**-Spalte die Richtung der ORDER BY-Klausel für diese Spalte gemeldet. Andernfalls wird NULL gemeldet.|  
|**error_number**|**int**|Enthält die von der Funktion zurückgegebene Fehlernummer. Enthält NULL, wenn in der Spalte kein Fehler aufgetreten ist.|  
|**error_severity**|**int**|Enthält den von der Funktion zurückgegebenen Schweregrad. Enthält NULL, wenn in der Spalte kein Fehler aufgetreten ist.|  
|**error_state**|**int**|Enthält die von der Funktion zurückgegebene Statusmeldung. Wenn kein Fehler ist aufgetreten ist, enthält die Spalte NULL.|  
|**error_message**|**nvarchar (4096)**|Enthält die von der Funktion zurückgegebene Meldung. Wenn kein Fehler aufgetreten ist, enthält die Spalte NULL.|  
|**error_type**|**int**|Enthält eine ganze Zahl, die den zurückgegebenen Fehler darstellt. Wird error_type_desc zugeordnet. Siehe Liste unter Hinweisen.|  
|**error_type_desc**|**nvarchar(60)**|Enthält eine kurze Zeichenfolge in Großbuchstaben, die den zurückgegebenen Fehler darstellt. Wird error_type zugeordnet. Siehe Liste unter Hinweisen.|  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Funktion verwendet den gleichen Algorithmus wie **sp_describe_first_result_set**. Weitere Informationen finden Sie unter [sp_describe_first_result_set &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 In der folgenden Tabelle werden die Fehlertypen und deren Beschreibungen aufgeführt.  
  
|error_type|error_type|BESCHREIBUNG|  
|-----------------|-----------------|-----------------|  
|1|MISC|Alle Fehler, die nicht anderweitig beschrieben sind.|  
|2|SYNTAX|Im Batch ist ein Syntaxfehler aufgetreten.|  
|3|CONFLICTING_RESULTS|Das Ergebnis konnte wegen eines Konflikts zwischen zwei möglichen ersten Anweisungen nicht ermittelt werden.|  
|4|DYNAMIC_SQL|Das Ergebnis konnte wegen einer dynamischen SQL-Abfrage nicht ermittelt werden, die potenziell das erste Ergebnis zurückgeben kann.|  
|5|CLR_PROCEDURE|Das Ergebnis konnte nicht ermittelt werden, da eine gespeicherte CLR-Prozedur potenziell das erste Ergebnis zurückgeben kann.|  
|6|CLR_TRIGGER|Das Ergebnis konnte wegen eines dynamischen CLR-Triggers nicht ermittelt werden, der potenziell das erste Ergebnis zurückgeben kann.|  
|7|EXTENDED_PROCEDURE|Das Ergebnis konnte nicht ermittelt werden, da eine erweiterte gespeicherte Prozedur potenziell das erste Ergebnis zurückgeben kann.|  
|8|UNDECLARED_PARAMETER|Das Ergebnis konnte nicht bestimmt werden, da der Datentyp von mindestens einer der Spalten des Resultsets potenziell von einem nicht deklarierten Parameter abhängt.|  
|9|RECURSION|Das Ergebnis konnte nicht ermittelt werden, da der Batch eine rekursive Anweisung enthält.|  
|10|TEMPORARY_TABLE|Das Ergebnis konnte nicht ermittelt werden, da der Batch eine temporäre Tabelle enthält und von **sp_describe_first_result_set** nicht unterstützt wird.|  
|11|UNSUPPORTED_STATEMENT|Das Ergebnis konnte nicht ermittelt werden, da der Batch eine Anweisung enthält, die von **sp_describe_first_result_set** (z. B., FETCH, REVERT usw.) nicht unterstützt wird.|  
|12|OBJECT_ID_NOT_SUPPORTED|Die @object_id an die Funktion über gegebene wird nicht unterstützt (d. h. keine gespeicherte Prozedur).|  
|13|OBJECT_ID_DOES_NOT_EXIST|Die @object_id an die Funktion über gegebene wurde im System Katalog nicht gefunden.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Berechtigung zum Ausführen des @tsql Arguments.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-metadata-with-and-without-browse-information"></a>A. Zurückgeben von Metadaten mit und ohne Suchinformationen  
 Im folgenden Beispiel wird eine gespeicherte Prozedur mit dem Namen TestProc2 erstellt, die zwei Resultsets zurückgibt. Dann wird im Beispiel veranschaulicht, dass **sys.dm_exec_describe_first_result_set** Informationen zum ersten Resultset in der Prozedur zurückgibt, mit und ohne Suchinformationen.  
  
```  
CREATE PROC TestProc2  
AS  
SELECT object_id, name FROM sys.objects ;  
SELECT name, schema_id, create_date FROM sys.objects ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 0) ;  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 1) ;  
GO  
```  
  
### <a name="b-combining-the-sysdm_exec_describe_first_result_set_for_object-function-and-a-table-or-view"></a>B. Kombinieren der sys.dm_exec_describe_first_result_set_for_object-Funktion mit einer Tabelle oder Sicht  
 Im folgenden Beispiel werden die System Katalog Sicht sys. Procedures und die **sys.dm_exec_describe_first_result_set_for_object** -Funktion verwendet, um Metadaten für die Resultsets aller gespeicherten Prozeduren in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] Datenbank anzuzeigen.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT p.name, r.*   
FROM sys.procedures AS p  
CROSS APPLY sys.dm_exec_describe_first_result_set_for_object(p.object_id, 0) AS r;  
GO  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_describe_first_result_set &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)  
  
  
