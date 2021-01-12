---
description: FileTableRootPath (Transact-SQL)
title: Filetablerootpath (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- FileTableRootPath_TSQL
- FileTableRootPath
dev_langs:
- TSQL
helpviewer_keywords:
- FileTableRootPath function
ms.assetid: 0cba908a-c85c-4b09-b16a-df1cb333c629
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f08273adcf261ecfdafaa70793d50b31360f7e40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093863"
---
# <a name="filetablerootpath-transact-sql"></a>FileTableRootPath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den UNC-Pfad auf der Stammebene für eine bestimmte FileTable oder die aktuelle Datenbank zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
FileTableRootPath ( [ '[schema_name.]FileTable_name' ], @option )  
```  
  
## <a name="arguments"></a>Argumente  
 *FileTable_name*  
 Der Name der FileTable. *FileTable_name* ist vom Typ **nvarchar**. Dies ist ein optionaler Parameter. Der Standardwert ist die aktuelle Datenbank. Die Angabe von *schema_name* ist ebenfalls optional. Sie können NULL für *FileTable_name* übergeben, um den Standardparameter Wert zu verwenden.  
  
 *\@andere*  
 Ein ganzzahliger Ausdruck, der definiert, wie die Serverkomponente des Pfads formatiert werden soll. die *\@ Option* kann einen der folgenden Werte aufweisen:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**0**|Gibt den in ein NetBIOS-Format konvertierten Servernamen zurück. Beispiel:<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Dies ist der Standardwert.|  
|**1**|Gibt den Servernamen ohne Konvertierung zurück. Beispiel:<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Gibt den vollständigen Serverpfad zurück. Beispiel:<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Rückgabetyp  
 **nvarchar(4000)**  
  
 Wenn die Datenbank zu einer Always on-Verfügbarkeits Gruppe gehört, gibt die **filetablerootpath** -Funktion anstelle des Computer namens den Namen des virtuellen Netzwerks (vnn) zurück.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Die **filetablerootpath** -Funktion gibt NULL zurück, wenn eine der folgenden Bedingungen zutrifft:  
  
-   Der Wert *FileTable_name* ist ungültig.  
  
-   Der Aufrufer hat keine ausreichende Berechtigung zum Verweisen auf die angegebene Tabelle oder die aktuelle Datenbank auf.  
  
-   Die FILESTREAM-Option von *database_directory* ist für die aktuelle Datenbank nicht festgelegt.  
  
 Weitere Informationen finden Sie unter [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
## <a name="best-practices"></a>Bewährte Methoden  
 Um Code und Anwendungen vom aktuellen Computer und von der Datenbank unabhängig zu halten, sollten Sie keinen Code schreiben, der auf absoluten Dateipfaden basiert. Rufen Sie stattdessen den vollständigen Pfad für eine Datei mit der Funktion **FileTableRootPath** und der Funktion **GetFileNamespacePath** zur Laufzeit ab, wie im folgenden Beispiel gezeigt. Die **GetFileNamespacePath** -Funktion gibt standardmäßig den relativen Pfad der Datei unter dem Stammpfad der Datenbank zurück.  
  
```sql  
USE MyDocumentDatabase;  
  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
@fullPath = varchar(1000);  
  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath()  
FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Die **filetablerootpath** -Funktion erfordert Folgendes:  
  
-   SELECT-Berechtigung für die FileTable, um den Stammpfad einer bestimmten FileTable abzurufen.  
  
-   **db_datareader** Berechtigung oder höher, um den Stammpfad für die aktuelle Datenbank zu erhalten.  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird gezeigt, wie die **filetablerootpath** -Funktion aufgerufen wird.  
  
```  
USE MyDocumentDatabase;  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase"  
SELECT FileTableRootPath();  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'dbo.MyFileTable');  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'MyFileTable');  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden von Verzeichnissen und Pfaden in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
