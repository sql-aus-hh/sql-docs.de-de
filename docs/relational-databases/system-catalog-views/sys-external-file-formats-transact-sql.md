---
description: sys.external_file_formats (Transact-SQL)
title: sys.external_file_formats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a89efb2c-0a3a-4b64-9284-6e93263e29ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 98b926499793c62817f884645366848c81bb4e4e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094631"
---
# <a name="sysexternal_file_formats-transact-sql"></a>sys.external_file_formats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jedes externe Dateiformat in der aktuellen Datenbank für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
 Enthält eine Zeile für jedes externe Dateiformat auf dem Server für [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|file_format_id|**int**|Die Objekt-ID für das externe Dateiformat.||  
|name|**sysname**|Der Name des Datei Formats. in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ist dies für die-Datenbank eindeutig. In [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ist dies für den Server eindeutig.||  
|format_type|**tinyint**|Der Datei Formattyp.|delimitedtext, rcfile, Orc, Parkett|  
|field_terminator|**nvarchar (10)**|Bei format_type = delimitedtext handelt es sich hierbei um das Feld Abschluss Zeichen.||  
|string_delimiter|**nvarchar (10)**|Bei format_type = delimitedtext ist dies das Zeichen folgen Trennzeichen.||  
|date_format|**nvarchar(50)**|Bei format_type = delimitedtext handelt es sich hierbei um das benutzerdefinierte Datums-und Uhrzeit Format.||  
|use_type_default|**bit**|Gibt für format_type = durch Trennzeichen getrennten Text an, wie fehlende Werte behandelt werden sollen, wenn polybase Daten aus HDFS-Text Dateien in importiert [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .|0-fehlende Werte als Zeichenfolge ' NULL ' speichern.<br /><br /> 1: speichert fehlende Werte als Spalten Standardwert.|  
|serde_method|**nvarchar(255)**|Bei format_type = rcfile ist dies die Serialisierungs-/Deserialisierungsmethode.||  
|row_terminator|**nvarchar (10)**|Bei format_type = delimitedtext handelt es sich hierbei um die Zeichenfolge, die jede Zeile in der externen Hadoop-Datei beendet.|Immer ' \n '.|  
|encoding|**nvarchar (10)**|Bei format_type = delimitedtext handelt es sich hierbei um die Codierungsmethode für die externe Hadoop-Datei.|Immer "utf8".|  
|data_compression|**nvarchar(255)**|Die Datenkomprimierungsmethode für die externen Daten.|Für format_type = delimitedtext:<br /><br /> -' org. Apache. Hadoop. IO. compress. defaultcodec '<br />-' org. Apache. Hadoop. IO. compress. gzipcodec '<br /><br /> Für format_type = rcfile:<br /><br /> -' org. Apache. Hadoop. IO. compress. defaultcodec '<br /><br /> Für format_type = Orc:<br /><br /> -' org. Apache. Hadoop. IO. compress. defaultcodec '<br />-' org. Apache. Hadoop. IO. compress. snappycodec '<br /><br /> Für format_type = Parkett:<br /><br /> -' org. Apache. Hadoop. IO. compress. gzipcodec '<br />-' org. Apache. Hadoop. IO. compress. snappycodec '|  
  
## <a name="permissions"></a>Berechtigungen  
 Die Sichtbarkeit der Metadaten in Katalogsichten ist auf sicherungsfähige Elemente eingeschränkt, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.external_data_sources &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [sys.external_tables &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)  
  
  
