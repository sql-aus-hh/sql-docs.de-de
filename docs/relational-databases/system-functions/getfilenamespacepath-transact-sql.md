---
description: GetFileNamespacePath (Transact-SQL)
title: GetFilename NamespacePath (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- GetFileNamespacePath
- GetFileNamespacePath_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetFileNamespacePath function
ms.assetid: b393ecef-baa8-4d05-a268-b2f309fce89a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ecda7d44603636ff12eef955dd83e7659cb9dc7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097485"
---
# <a name="getfilenamespacepath-transact-sql"></a>GetFileNamespacePath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den UNC-Pfad für eine Datei bzw. ein Verzeichnis in einer FileTable zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<column-name>.GetFileNamespacePath(is_full_path, @option)  
```  
  
## <a name="arguments"></a>Argumente  
 *Spaltenname*  
 Der Spaltenname der VARBINARY(MAX) **file_stream** -Spalte in einer FileTable.  
  
 Der Wert von *column-name* muss ein gültiger Spaltenname sein. Es kann sich hierbei weder um einen Ausdruck noch um einen Wert handeln, der von einer Spalte eines anderen Datentyps konvertiert oder umgewandelt wurde.  
  
 *is_full_path*  
 Ein ganzzahliger Ausdruck, der angibt, ob ein relativer oder ein absoluter Pfad zurückgegeben werden soll. *is_full_path* kann einen der folgenden Werte aufweisen:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**0**|Gibt den relativen Pfad innerhalb des Verzeichnisses auf Datenbankebene zurück.<br /><br /> Dies ist der Standardwert.|  
|**1**|Gibt den vollständigen UNC-Pfad zurück, der mit `\\computer_name`beginnt.|  
  
 *\@andere*  
 Ein ganzzahliger Ausdruck, der definiert, wie die Serverkomponente des Pfads formatiert werden soll. die *\@ Option* kann einen der folgenden Werte aufweisen:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**0**|Gibt den in ein NetBIOS-Format konvertierten Servernamen zurück. Beispiel:<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Dies ist der Standardwert.|  
|**1**|Gibt den Servernamen ohne Konvertierung zurück. Beispiel:<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Gibt den vollständigen Serverpfad zurück. Beispiel:<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Rückgabetyp  
 **nvarchar(max)**  
  
 Wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz in einem Failovercluster gruppiert ist, ist der Computername, der als Teil dieses Pfads zurückgegeben wird, der virtuelle Hostname für die gruppierte Instanz.  
  
 Wenn die Datenbank zu einer Always on-Verfügbarkeits Gruppe gehört, gibt die **filetablerootpath** -Funktion anstelle des Computer namens den Namen des virtuellen Netzwerks (vnn) zurück.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Der von der **GetFileNamespacePath** -Funktion zurückgegebene Pfad ist ein logischer Verzeichnis- oder Dateipfad im folgenden Format:  
  
 `\\<machine>\<instance-level FILESTREAM share>\<database-level directory>\<FileTable directory>\...`  
  
 Dieser logische Pfad ist keine direkte Entsprechung eines physischen NTFS-Pfads. Er wird vom Dateisystem-Filtertreiber von FILESTREAM und vom FileStream-Agent in den physischen Pfad übersetzt. Durch diese Unterscheidung zwischen dem logischen und dem physischen Pfad kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Daten intern neu organisieren, ohne die Gültigkeit des Pfads zu beeinträchtigen.  
  
## <a name="best-practices"></a>Bewährte Methoden  
 Um Code und Anwendungen vom aktuellen Computer und von der Datenbank unabhängig zu halten, sollten Sie keinen Code schreiben, der auf absoluten Dateipfaden basiert. Rufen Sie stattdessen den vollständigen Pfad für eine Datei mit der Funktion **FileTableRootPath** und der Funktion **GetFileNamespacePath** zur Laufzeit ab, wie im folgenden Beispiel gezeigt. Die **GetFileNamespacePath** -Funktion gibt standardmäßig den relativen Pfad der Datei unter dem Stammpfad der Datenbank zurück.  
  
```sql  
USE MyDocumentDatabase;  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
  
@fullPath = varchar(1000);  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath() FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="remarks"></a>Bemerkungen  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird gezeigt, wie die **GetFileNamespacePath** -Funktion aufgerufen wird, um den UNC-Pfad für eine Datei oder ein Verzeichnis in einer FileTable abzurufen.  
  
```  
-- returns the relative path of the form "\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath() AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
  
-- returns "\\MyServer\MSSQLSERVER\MyDocumentDatabase\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath(1, Null) AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwenden von Verzeichnissen und Pfaden in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
