---
description: sys.dm_db_persisted_sku_features (Transact-SQL)
title: sys.dm_db_persisted_sku_features (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_persisted_sku_features_TSQL
- sys.dm_db_persisted_sku_features
- dm_db_persisted_sku_features_TSQL
- dm_db_persisted_sku_features
dev_langs:
- TSQL
helpviewer_keywords:
- editions [SQL Server]
- sys.dm_db_persisted_sku_features dynamic management view
ms.assetid: b4b29e97-b523-41b9-9528-6d4e84b89e09
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9221515e0c8a6c0704f0a036d851241d849d5f3d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101664"
---
# <a name="sysdm_db_persisted_sku_features-transact-sql"></a>sys.dm_db_persisted_sku_features (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Einige Features von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ändern die Art und Weise, [!INCLUDE[ssDE](../../includes/ssde-md.md)] in der Informationen in den Datenbankdateien gespeichert werden. Diese Funktionen sind nicht in allen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Datenbank, die diese Funktionen enthält, kann nicht in eine Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verschoben werden, die sie nicht unterstützt. Verwenden Sie die dynamische Verwaltungs Sicht sys.dm_db_persisted_sku_features, um Editions spezifische Funktionen aufzulisten, die in der aktuellen Datenbank aktiviert sind.
  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher).
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|feature_name|**sysname**|Externer Name der Funktion, die in der aktuellen Datenbank aktiviert ist, aber nicht in allen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt wird. Diese Funktion muss entfernt werden, bevor die Datenbank zu allen verfügbaren Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migriert werden kann.|  
|feature_id|**int**|Funktions-ID, die der Funktion zugeordnet ist. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW DATABASE STATE-Berechtigung für die Datenbank.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn von der Datenbank keine Features verwendet werden, die möglicherweise durch eine bestimmte Edition eingeschränkt sind, gibt die Sicht keine Zeilen zurück.  
  
 sys.dm_db_persisted_sku_features können die folgenden Funktionen zur Daten Bank Änderung auflisten, die auf bestimmte Editionen beschränkt sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   **Changecapture**: gibt an, dass eine Datenbank Change Data Capture aktiviert ist. Um Change Data Capture zu entfernen, verwenden Sie die gespeicherte Prozedur [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) . Weitere Informationen finden Sie unter [Informationen zu Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
-   **Columnstoreindex**: gibt an, dass mindestens eine Tabelle über einen columnstore--Index verfügt. Damit eine Datenbank in eine Edition von verschoben werden kann, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dieses Feature nicht unterstützt, verwenden Sie die Anweisung [Drop Index](../../t-sql/statements/drop-index-transact-sql.md) oder [Alter Index](../../t-sql/statements/alter-index-transact-sql.md) , um den columnstore--Index zu entfernen. Weitere Informationen finden Sie unter [columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
    **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher).  
  
-   **Komprimierung**: gibt an, dass mindestens eine Tabelle oder ein Index die Datenkomprimierung oder das vardecimal--Speicherformat verwendet. Damit eine Datenbank in eine Edition von verschoben werden kann, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die diese Funktion nicht unterstützt, verwenden Sie die [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) -oder [Alter Index](../../t-sql/statements/alter-index-transact-sql.md) -Anweisung, um die Datenkomprimierung zu entfernen. Um das vardecimal-Speicherformat zu entfernen, verwenden Sie die sp_tableoption-Anweisung. Weitere Informationen finden Sie unter [Data Compression](../../relational-databases/data-compression/data-compression.md).  
  
-   **Multiplefscontainers**: gibt an, dass für die Datenbank mehrere FILESTREAM-Container verwendet werden. Die Datenbank verfügt über eine FILESTREAM-Datei Gruppe mit mehreren Containern (Dateien). Weitere Informationen finden Sie unter [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
-   **Inmemoryoltp**: gibt an, dass die Datenbank In-Memory OLTP verwendet. Die Datenbank verfügt über eine MEMORY_OPTIMIZED_DATA-Dateigruppe. Weitere Informationen finden Sie unter [In-Memory OLTP &#40;Arbeitsspeicheroptimierung&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher). 
  
-   **Aufteilung.** Gibt an, dass die Datenbank partitionierte Tabellen, partitionierte Indizes, partitionierte Schemas oder Partitionsfunktionen enthält. Damit eine Datenbank in eine andere Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als die Enterprise oder Developer Edition verlagert werden kann, genügt es nicht, die Tabelle dahingehend abzuändern, dass sie sich in einer einzigen Partition befindet. Sie müssen die partitionierte Tabelle entfernen. Wenn die Tabelle Daten enthält, verwenden Sie SWITCH PARTITION, um jede Partition in eine nicht partitionierte Tabelle zu konvertieren. Löschen Sie dann die partitionierte Tabelle, das Partitionsschema und die Partitionsfunktion.  
  
-   **TransparentDataEncryption.** Gibt an, dass eine Datenbank mit transparenter Datenverschlüsselung verschlüsselt wird. Um die transparente Datenverschlüsselung zu entfernen, verwenden Sie die ALTER DATABASE-Anweisung. Weitere Informationen finden Sie unter [Transparente Datenverschlüsselung &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  

> [!NOTE]
> Ab [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Service Pack 1 sind diese Features mit Ausnahme von **transparentdataencryption.** sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für mehrere Editionen verfügbar und nicht nur für die Enterprise oder Developer Edition.

 Wenn Sie feststellen möchten, ob in einer Datenbank Funktionen verwendet werden, die nur in bestimmten Editionen verfügbar sind, führen Sie die folgende Anweisung in der Datenbank aus:  
  
```sql  
SELECT feature_name FROM sys.dm_db_persisted_sku_features;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [Editionen und unterstützten Funktionen von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Editionen und unterstützten Funktionen von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
