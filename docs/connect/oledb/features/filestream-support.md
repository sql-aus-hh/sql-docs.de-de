---
title: FILESTREAM-Unterstützung | Microsoft-Dokumentation
description: SQL Server unterstützt die erweiterte FILESTREAM-Funktion, mit der Sie große binäre Werte entweder über SQL Server oder das Dateisystem speichern und abrufen können.
ms.custom: ''
ms.date: 09/13/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- FILESTREAM [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server [FILESTREAM support]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1fcc048d51186289d13cbe8918b5328f8c604f8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766135"
---
# <a name="filestream-support-in-ole-db-driver-for-sql-server"></a>FILESTREAM-Unterstützung im OLE DB-Treiber für SQL Server
[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Ab [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] unterstützt der OLE DB-Treiber für SQL Server die erweiterte FILESTREAM-Funktion. Beispiele finden Sie unter [FILESTREAM und OLE DB](../../oledb/ole-db-how-to/filestream/filestream-and-ole-db.md).  

Die FILESTREAM-Funktion bietet eine Möglichkeit, große binäre Werte zu speichern und entweder über [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] oder durch direkten Zugriff auf das Windows-Dateisystem darauf zuzugreifen. Ein großer Binärwert ist ein Wert, der größer als 2 Gigabyte (GB) ist. Weitere Informationen zur verbesserten FILESTREAM-Unterstützung finden Sie unter [FILESTREAM &#40;SQL Server&#41;](../../../relational-databases/blob/filestream-sql-server.md).  
  
Wenn eine Datenbankverbindung geöffnet wird, wird **\@\@TEXTSIZE** standardmäßig auf –1 („unbegrenzt“) festgelegt.  
  
Es ist auch möglich, mit Windows-Dateisystem-APIs auf FILESTREAM-Spalten zuzugreifen und diese zu aktualisieren.  
  
Weitere Informationen finden Sie unter [Zugreifen auf FILESTREAM-Daten mit OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md).  
  
## <a name="querying-for-filestream-columns"></a>Abfragen von FILESTREAM-Spalten  
Schemarowsets in OLE DB geben nicht an, ob eine Spalte eine FILESTREAM-Spalte ist. ITableDefinition in OLE DB kann nicht verwendet werden, um eine FILESTREAM-Spalte zu erstellen.    
  
Mithilfe der **is_filestream**-Spalte der [sys.columns](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)-Katalogsicht können Sie FILESTREAM-Spalten erstellen oder ermitteln, welche der vorhandenen Spalten FILESTREAM-Spalten sind.  
  
Es folgt ein Beispiel:  
  
```sql  
-- Create a table with a FILESTREAM column.  
CREATE TABLE Bob_01 (GuidCol1 uniqueidentifier ROWGUIDCOL NOT NULL UNIQUE DEFAULT NEWID(), IntCol2 int, varbinaryCol3 varbinary(max) FILESTREAM);  
  
-- Find FILESTREAM columns.  
SELECT name FROM sys.columns WHERE is_filestream=1;  
  
-- Determine whether a column is a FILESTREAM column.  
SELECT is_filestream FROM sys.columns WHERE name = 'varbinaryCol3' AND object_id IN (SELECT object_id FROM sys.tables WHERE name='Bob_01');  
```  
  
## <a name="down-level-compatibility"></a>Kompabilität mit früheren Versionen  
Wenn der Client mit dem OLE DB-Treiber für SQL Server kompiliert wurde und die Anwendung eine Verbindung mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] herstellt ([!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] oder später), ist das Verhalten von **varbinary(max)** mit dem vom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client in [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] eingeführten Verhalten. Das heißt, die Maximalgröße der zurückgegebenen Daten ist auf 2 GB beschränkt. Ergebniswerte, die größer als 2 GB sind, werden abgeschnitten, und es wird die Warnung „Zeichenfolgendaten werden rechts abgeschnitten“ zurückgegeben. 
  
Wenn Datentypkompatibilität auf 80 festgelegt wird, ist das Clientverhalten mit dem Verhalten von Clients früherer Versionen konsistent.  
  
Bei Clients, die SQLOLEDB oder andere Anbieter verwenden, die vor [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] veröffentlicht wurden, wird **varbinary(max)** dem Datentyp „image“ zugeordnet.  
  
## <a name="comments"></a>Kommentare
- Eine Anwendung verwendet **DBTYPE_IUNKNOWN** in Parameter- und Ergebnisbindungen, um **varbinary(max)** -Werte zu senden und zu empfangen, die größer als 2 GB sind. Der Anbieter muss für Parameter IUnknown::QueryInterface für ISequentialStream und für Ergebnisse abrufen, die ISequentialStream zurückgeben.  

-  Für OLE DB wird die auf ISequentialStream-Werte bezogene Überprüfung gelockert. Wenn *wType* in der **DBBINDING**-Struktur **DBTYPE_IUNKNOWN** ist, kann die Längenüberprüfung deaktiviert werden, indem **DBPART_LENGTH** aus *dwPart* ausgelassen wird oder die Länge der Daten (bei Offset *obLength* im Datenpuffer) auf ~0 festgelegt wird. In diesem Fall überprüft der Provider die Länge des Werts nicht und fordert alle Daten an (oder gibt sie zurück), die über den Datenstrom zur Verfügung stehen. Diese Änderung wird auf alle LOB-Typen (Large Object) und auf XML angewendet, allerdings nur, wenn eine Verbindung zu [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]-Servern (oder höher) besteht. Auf diese Weise erhalten Entwickler eine höhere Flexibilität, während sie die Konsistenz und Abwärtskompatibilität für vorhandene Anwendungen und Downlevelserver beibehalten.  Diese Änderung wirkt sich auf alle Schnittstellen aus, die Daten übertragen (hauptsächlich IRowset::GetData, ICommand::Execute und IRowsetFastLoad::InsertRow).
 

## <a name="see-also"></a>Weitere Informationen  
 [OLE DB-Treiber für SQL Server-Features](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
