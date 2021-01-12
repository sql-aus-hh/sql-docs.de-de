---
description: CDC. &lt; capture_instance &gt; _CT (Transact-SQL)
title: CDC. &lt; capture_instance &gt; _CT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc
- cdc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.<capture_instance>_CT
ms.assetid: 979c8110-3c54-4e76-953c-777194bc9751
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7fa737644611f24d9d0858fd04066d3ba0571ee3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102726"
---
# <a name="cdcltcapture_instancegt_ct-transact-sql"></a>CDC. &lt; capture_instance &gt; _CT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die Änderungstabelle, die erstellt wird, wenn Change Data Capture für eine Quelltabelle aktiviert wird. Für jeden Einfüge- oder Löschvorgang, der in der Quelltabelle ausgeführt wird, gibt die Tabelle eine Zeile zurück, für jeden Updatevorgang in der Quelltabelle gibt sie zwei Zeilen zurück. Wird der Name der Änderungstabelle bei der Aktivierung der Quelltabelle nicht angegeben, wird der Name abgeleitet. Das Format des Namens ist CDC. *capture_instance* _CT, wobei *capture_instance* der Schema Name der Quell Tabelle und der Name der Quell Tabelle im Format *schema_table* ist. Wenn beispielsweise die Tabelle **Person. Address** in der **AdventureWorks** -Beispieldatenbank für Change Data Capture aktiviert ist, wird der Name der abgeleiteten Änderungs Tabelle **CDC.Person_Address_CT**.  
  
 Es wird empfohlen, **die Systemtabellen nicht direkt abzufragen**. Führen Sie stattdessen die [CDC.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) und [CDC.fn_cdc_get_net_changes_<capture_instance ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md) Funktionen aus.  
  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|Protokollfolgenummer (LSN, Log Sequence Number), die dem Commit für die Änderung zugeordnet wurde.<br /><br /> Alle Änderungen, für die ein Commit in derselben Transaktion ausgeführt wurde, verwenden dieselbe Commit-LSN. Wenn z. b. bei einem Löschvorgang in der Quell Tabelle zwei Zeilen entfernt werden, enthält die Änderungs Tabelle zwei Zeilen mit demselben Wert von **__ $ start_lsn** .|  
|**__ $ end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hat diese Spalte immer den Wert NULL.|  
|**__$seqval**|**binary(10)**|Sequenzwert, der verwendet wird, um die Zeilenänderungen innerhalb einer Transaktion zu sortieren.|  
|**__$operation**|**int**|Identifiziert den Vorgang der Datenbearbeitungssprache (Data Manipulation Language, DML), der der Änderung zugeordnet ist. Dabei kann es sich um eine der folgenden Methoden handeln:<br /><br /> 1 = Löschen<br /><br /> 2 = Einfügen<br /><br /> 3 = Aktualisieren (alte Werte)<br /><br /> Spaltendaten verfügen vor der Ausführung der UPDATE-Anweisung über Zeilenwerte.<br /><br /> 4 = Aktualisieren (neue Werte)<br /><br /> Spaltendaten verfügen nach der Ausführung der UPDATE-Anweisung über Zeilenwerte.|  
|**__$update_mask**|**varbinary(128)**|Eine Bitmaske, die auf den Spalten Ordnungen der Änderungs Tabelle basiert, die die geänderten Spalten identifiziert.|  
|*\<captured source table columns>*|Variiert|Bei den verbleibenden Spalten in der Änderungstabelle handelt es sich um die Spalten aus der Quelltabelle, die beim Erstellen der Aufzeichnungsinstanz als aufgezeichnete Tabellen identifiziert wurden. Wenn in der Liste der aufgezeichneten Spalten keine Spalten angegeben wurden, werden alle Spalten in der Quelltabelle in diese Tabelle aufgenommen.|  
|**__ $ command_id** |**int** |Verfolgt die Reihenfolge der Vorgänge innerhalb einer Transaktion. |  
  
## <a name="remarks"></a>Bemerkungen  

Die Spalte " `__$command_id` Spalte war" wurde in einem kumulativen Update in den Versionen 2012 bis 2016 eingeführt. Informationen zur Version und zum Herunterladen finden Sie im KB-Artikel 3030352 unter [Korrektur: die Änderungs Tabelle wird nach dem Aktivieren von Change Data Capture für eine Microsoft SQL Server Datenbank falsch für aktualisierte Zeilen angeordnet](https://support.microsoft.com/help/3030352/fix-the-change-table-is-ordered-incorrectly-for-updated-rows-after-you).  Weitere Informationen finden Sie unter unter [Brechung der CDC-Funktion nach dem Upgrade auf das neueste Cu für SQL Server 2012, 2014 und 2016](/archive/blogs/sql_server_team/cdc-functionality-may-break-after-upgrading-to-the-latest-cu-for-sql-server-2012-2014-and-2016).

## <a name="captured-column-data-types"></a>Datentypen von aufgezeichneten Spalten  
 Die in dieser Tabelle enthaltenen aufgezeichneten Spalten haben denselben Datentyp und Wert wie die entsprechenden Quellspalten. Hierbei gelten folgende Ausnahmen:  
  
-   **Timestamp** -Spalten werden als **Binary (8)** definiert.  
  
-   **Identitäts** Spalten werden entweder als **int** oder **bigint** definiert.  
  
 Die Werte in diesen Spalten sind jedoch mit den Quellspaltenwerten identisch.  
  
### <a name="large-object-data-types"></a>LOB (Large Object)-Datentypen  
 Spalten des Datentyps **Image**, **Text** und **ntext** wird immer ein **null** -Wert zugewiesen, wenn __ $ operation = 1 oder \_ \_ $Operation = 3. Spalten vom Datentyp **varbinary (max)**, **varchar (max)** oder **nvarchar (max)** wird ein **null** -Wert zugewiesen, wenn \_ \_ $Operation = 3, es sei denn, die Spalte wurde während des Updates geändert. Wenn \_ \_ $Operation = 1, werden diesen Spalten zum Zeitpunkt der Löschung ihre Werte zugewiesen. Berechnete Spalten, die in einer Aufzeichnungs Instanz enthalten sind, haben immer den Wert **null**.  
  
 Standardmäßig können einer aufgezeichneten Spalte in einer einzelnen Anweisung vom Typ INSERT, UPDATE, WRITETEXT oder UPDATETEXT maximal 65.536 Bytes oder 64 KB hinzugefügt werden. Verwenden Sie die [Server Konfigurations Option max text repl size konfigurieren](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) , um diese Größe für die Unterstützung größerer LOB-Daten zu erhöhen. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md).  
  
## <a name="data-definition-language-modifications"></a>Änderungen mithilfe der Datendefinitionssprache (Data Definition Language, DDL)  
 DDL-Änderungen an der Quell Tabelle (z. b. das Hinzufügen oder Löschen von Spalten) werden in der [CDC.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) Tabelle aufgezeichnet. Diese Änderungen werden nicht auf die Änderungstabelle angewendet. Die Definition der Änderungstabelle bleibt also konstant. Werden Zeilen in die Änderungstabelle eingefügt, werden während des Aufzeichnungsvorgangs diejenigen Spalten ignoriert, die nicht in der Liste der aufgezeichneten Spalten, die der Quelltabelle zugeordnet ist, aufgeführt sind. Falls in der Liste der aufgezeichneten Spalten eine Spalte angezeigt wird, die sich nicht mehr in der Quelltabelle befindet, wird dieser Spalte ein NULL-Wert zugewiesen.  
  
 Das Ändern des Datentyps einer Spalte in der Quell Tabelle wird auch in der [CDC.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) Tabelle aufgezeichnet. Durch diese Änderung wird die Definition der Änderungstabelle jedoch nicht geändert. Der Datentyp der aufgezeichneten Spalte in der Änderungstabelle wird geändert, wenn während des Aufzeichnungsvorgangs der Protokolldatensatz für die an der Quelltabelle vorgenommenen Änderungen gefunden wird.  
  
 Falls Sie den Datentyp einer aufgezeichneten Spalte in der Quelltabelle so ändern müssen, dass die Größe des Datentyps verringert wird, stellen Sie mithilfe des folgenden Verfahrens sicher, dass die entsprechende Spalte in der Änderungstabelle erfolgreich geändert werden kann.  
  
1.  Aktualisieren Sie in der Quelltabelle die Werte in der zu ändernden Spalte, sodass ihre Größe für die geplante Datentypgröße geeignet ist. Wenn Sie z. b. den Datentyp von **int** in **smallint** ändern, aktualisieren Sie die Werte auf eine Größe, die im **smallint** -Bereich (-32.768 bis 32.767) entspricht.  
  
2.  Führen Sie denselben Updatevorgang in der Änderungstabelle für die entsprechende Spalte aus.  
  
3.  Ändern Sie die Quelltabelle, indem Sie den neuen Datentyp angeben. Die Datentypänderung wird erfolgreich an die Änderungstabelle weitergegeben.  

## <a name="data-manipulation-language-modifications"></a>Änderungen mithilfe der Datenbearbeitungssprache (Data Manipulation Language, DDL)  
 Wenn in einer Change Data Capture-aktivierten Quelltabelle Einfüge-, Update- und Löschvorgänge ausgeführt werden, wird im Datenbanktransaktionsprotokoll ein Datensatz dieser DML-Vorgänge angezeigt. Der Change Data Capture Prozess ruft Informationen zu diesen Änderungen aus dem Transaktionsprotokoll ab und fügt der Änderungs Tabelle entweder eine oder zwei Zeilen hinzu, um die Änderung aufzuzeichnen. Die Einträge werden in derselben Reihenfolge in die Änderungstabelle eingefügt, in der sie an die Quelltabelle übergeben wurden, obwohl der Commit für Einträge in der Änderungstabelle normalerweise für Gruppen von Änderungen statt für einzelne Einträge ausgeführt werden muss.  
  
 Innerhalb des Änderungs Tabellen Eintrags wird die Spalte **__ $ start_lsn** verwendet, um die Commit-LSN aufzuzeichnen, die der Änderung in der Quell Tabelle zugeordnet ist, und die **Spalte __ $** * wird verwendet, um die Änderung innerhalb der Transaktion zu sortieren. In Kombination können diese Metadatenspalten verwendet werden, um sicherzustellen, dass die Reihenfolge der Änderungen beim Commit beibehalten wird. Da die Änderungsinformationen beim Aufzeichnungsprozess aus dem Transaktionsprotokoll abgerufen werden, werden die Einträge in der Änderungstabelle nicht synchron mit den entsprechenden Änderungen in der Quelltabelle angezeigt. Stattdessen werden die Änderungen asynchron angezeigt, nachdem die relevanten Änderungseinträge aus dem Transaktionsprotokoll vom Aufzeichnungsprozess verarbeitet wurden.  
  
 Bei Einfüge- und Löschvorgängen werden alle Bits in der Updatemaske festgelegt. Bei Updatevorgängen wird die Updatemaske sowohl in den alten als auch in den neuen Zeilen des Updates entsprechend den Spalten geändert, die während des Updates geändert wurden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.sp_cdc_enable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)  
  
