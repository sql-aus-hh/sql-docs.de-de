---
title: SQL Server-Schemaauflistungen
description: In diesem Artikel werden die zusätzlichen vom Microsoft-SqlClient-Datenanbieter unterstützten Schemasammlungen für SQL Server beschrieben.
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e5a90fa57c4fa7478a965250fded7ceb92bf326c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051333"
---
# <a name="sql-server-schema-collections"></a>SQL Server-Schemaauflistungen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Der SqlClient-Datenanbieter für SQL Server von Microsoft unterstützt neben den gängigen Schemasammlungen auch zusätzliche Schemasammlungen. Die Schemaauflistungen sind je nach verwendeter SQL Server-Version verschieden. Rufen Sie die Methode **GetSchema** ohne Argumente oder mit dem Schemasammlungsnamen „MetaDataCollections“ auf, um eine Liste der unterstützten Schemasammlungen zu ermitteln. Dadurch wird <xref:System.Data.DataTable> mit einer Liste der unterstützten Schemaauflistungen, der Anzahl der von diesen Schemaauflistungen unterstützten Einschränkungen und der Anzahl der von diesen Schemaauflistungen verwendeten Bezeichnerteilen zurückgegeben.  

## <a name="databases"></a>Datenbanken  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|database_name|String|Der Name der Datenbank.|  
|dbid|Int16|Datenbank-ID|  
|create_date|Datetime|Erstellungsdatum der Datenbank.|  

## <a name="foreign-keys"></a>Fremdschlüssel  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|CONSTRAINT_CATALOG|String|Katalog, zu dem die Einschränkung gehört.|  
|CONSTRAINT_SCHEMA|String|Schema, das die Einschränkung enthält.|  
|CONSTRAINT_NAME|String|Name.|  
|TABLE_CATALOG|String|Tabellenname, zu dem diese Einschränkung gehört.|  
|TABLE_SCHEMA|String|Schema, das die Tabelle enthält.|  
|table_name|String|Tabellenname|  
|CONSTRAINT_TYPE|String|Einschränkungstyp Nur "FOREIGN KEY" zulässig.|  
|IS_DEFERRABLE|String|Gibt an, ob die Einschränkung verzögert werden kann. Gibt NO zurück.|  
|INITIALLY_DEFERRED|String|Gibt an, ob die Einschränkung anfangs verzögert werden kann. Gibt NO zurück.|  

## <a name="indexes"></a>Indizes  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|constraint_catalog|String|Katalog, zu dem dieser Index gehört.|  
|constraint_schema|String|Schema, das den Index enthält.|  
|constraint_name|String|Name des Indexes.|  
|table_catalog|String|Tabellenname, dem der Index zugeordnet ist.|  
|table_schema|String|Schema, das die Tabelle enthält, der der Index zugeordnet ist.|  
|table_name|String|Tabellenname.|  
|index_name|String|Indexname|  
|type_desc|String|Der Index weist einen der folgenden Typen auf:<br /><br /> – HEAP<br />– CLUSTERED<br />– NONCLUSTERED<br />– XML<br />– SPATIAL|  

## <a name="indexcolumns"></a>IndexColumns  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|constraint_catalog|String|Katalog, zu dem dieser Index gehört.|  
|constraint_schema|String|Schema, das den Index enthält.|  
|constraint_name|String|Name des Indexes.|  
|table_catalog|String|Tabellenname, dem der Index zugeordnet ist.|  
|table_schema|String|Schema, das die Tabelle enthält, der der Index zugeordnet ist.|  
|table_name|String|Tabellenname.|  
|column_name|String|Spaltenname, dem der Index zugeordnet ist.|  
|ordinal_position|Int32|Ordnungsposition der Spalte.|  
|KeyType|Byte|Der Objekttyp.|  
|index_name|String|Indexname| 

## <a name="procedures"></a>Prozeduren  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|String|Spezifischer Name für den Katalog.|  
|SPECIFIC_SCHEMA|String|Spezifischer Name des Schemas.|  
|SPECIFIC_NAME|String|Spezifischer Name des Katalogs.|  
|ROUTINE_CATALOG|String|Katalog, zu dem die gespeicherte Prozedur gehört.|  
|ROUTINE_SCHEMA|String|Schema, das die gespeicherte Prozedur enthält.|  
|ROUTINE_NAME|String|Name der gespeicherten Prozedur.|  
|ROUTINE_TYPE|String|Gibt PROCEDURE für gespeicherte Prozeduren und FUNCTION für Funktionen zurück.|  
|CREATED|Datetime|Zeitpunkt der Erstellung der Prozedur.|  
|LAST_ALTERED|Datetime|Zeitpunkt der letzten Änderung der Prozedur.| 

## <a name="procedure-parameters"></a>Prozedurparameter  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|String|Katalogname der Prozedur, für die dies einen Parameter darstellt.|  
|SPECIFIC_SCHEMA|String|Schema, das die Prozedur enthält, zu der dieser Parameter gehört.|  
|SPECIFIC_NAME|String|Name der Prozedur, zu der dieser Parameter gehört.|  
|ORDINAL_POSITION|Int32|Die Ordnungsposition des Parameters, beginnend bei 1. Für den Rückgabewert einer Prozedur ist dies 0.|  
|PARAMETER_MODE|String|Gibt IN zurück, wenn es ein Eingabeparameter ist, OUT, wenn es ein Ausgabeparameter ist, und INOUT, wenn es ein Eingabe/Ausgabeparameter ist.|  
|IS_RESULT|String|Gibt YES zurück, wenn das Ergebnis der Prozedur angegeben wird, die eine Funktion darstellt. Andernfalls wird NO zurückgegeben.|  
|AS_LOCATOR|String|Gibt YES zurück, wenn der Parameter als Lokator deklariert wurde. Andernfalls wird NO zurückgegeben.|  
|PARAMETER_NAME|String|Der Name des Parameters. NULL, wenn er dem Rückgabewert einer Funktion entspricht.|  
|DATA_TYPE|String|Vom System bereitgestellter Datentyp|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Maximale Länge in Zeichen für binary-Datentypen oder Zeichendatentypen Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_OCTET_LENGTH|Int32|Maximale Länge in Bytes für binary-Datentypen oder Zeichendatentypen Andernfalls wird NULL zurückgegeben.|  
|COLLATION_CATALOG|String|Katalogname der Sortierung des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|COLLATION_SCHEMA|String|Gibt immer NULL zurück.|  
|COLLATION_NAME|String|Name der Sortierung des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|CHARACTER_SET_CATALOG|String|Katalogname des Zeichensatzes des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|CHARACTER_SET_SCHEMA|String|Gibt immer NULL zurück.|  
|CHARACTER_SET_NAME|String|Name des Zeichensatzes des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|NUMERIC_PRECISION|Byte|Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION_RADIX|Int16|Basis der Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_SCALE|Int32|Anzahl der Dezimalstellen für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|DATETIME_PRECISION|Int16|Genauigkeit in Bruchteilen von Sekunden, wenn der Parametertyp datetime oder smalldatetime ist. Andernfalls wird NULL zurückgegeben.|  
|INTERVAL_TYPE|String|NULL. Für die künftige Verwendung durch SQL Server reserviert.|  
|INTERVAL_PRECISION|Int16|NULL. Für die künftige Verwendung durch SQL Server reserviert.| 

## <a name="tables"></a>Tabellen  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|String|Katalog der Tabelle.|  
|TABLE_SCHEMA|String|Schema, das die Tabelle enthält.|  
|table_name|String|Tabellenname.|  
|TABLE_TYPE|String|Tabellentyp. VIEW oder BASE TABLE|  

## <a name="columns"></a>Spalten  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|String|Katalog der Tabelle.|  
|TABLE_SCHEMA|String|Schema, das die Tabelle enthält.|  
|table_name|String|Tabellenname.|  
|COLUMN_NAME|String|Spaltenname.|  
|ORDINAL_POSITION|Int32|Identifikationsnummer der Spalte|  
|COLUMN_DEFAULT|String|Standardwert der Spalte|  
|IS_NULLABLE|String|NULL-Zulässigkeit der Spalte. Wenn diese Spalte NULL zulässt, gibt die Spalte YES zurück. Andernfalls wird NO zurückgegeben.|  
|DATA_TYPE|String|Vom System bereitgestellter Datentyp|  
|CHARACTER_MAXIMUM_LENGTH|Int32 – Sql8, Int16 – Sql7|Maximale Länge (in Zeichen) für binäre Daten, Zeichendaten, Text- und Tmage-Daten Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_OCTET_LENGTH|Int32 – SQL8, Int16 – Sql7|Maximale Länge (in Bytes) für binäre Daten, Zeichendaten, Text- und Image-Daten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION|Byte ohne Vorzeichen|Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION_RADIX|Int16|Basis der Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_SCALE|Int32|Anzahl der Dezimalstellen für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|DATETIME_PRECISION|Int16|Untertypcode für den datetime-Datentyp und den SQL-92-Datentyp interval. Für andere Datentypen wird NULL zurückgegeben.|  
|CHARACTER_SET_CATALOG|String|Gibt "master" zurück, wodurch die Datenbank angegeben wird, in der der Zeichensatz enthalten ist, wenn die Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_SET_SCHEMA|String|Gibt immer NULL zurück.|  
|CHARACTER_SET_NAME|String|Gibt den eindeutigen Namen für den Zeichensatz zurück, wenn diese Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls wird NULL zurückgegeben.|  
|COLLATION_CATALOG|String|Gibt master zurück, wodurch die Datenbank angegeben wird, in der die Sortierung definiert ist, wenn die Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls ist diese Spalte NULL.|  
|IS_FILESTREAM|String|YES, wenn die Spalte über das FILESTREAM-Attribut verfügt.<br /><br /> NO, wenn die Spalte nicht über das FILESTREAM-Attribut verfügt.|  
|IS_SPARSE|String|YES, wenn die Spalte eine Sparsespalte ist.<br /><br /> NO, wenn die Spalte keine Sparsespalte ist.|  
|IS_COLUMN_SET|String|YES, wenn die Spalte eine Spaltensatzspalte ist.<br /><br /> NO, wenn die Spalte keine Spaltensatzspalte ist.|  

## <a name="allcolumns"></a>AllColumns  

 Die AllColumns-Schemasammlung wird zur Unterstützung von Sparsespalten verwenden. AllColumns hat die gleichen Einschränkungen und das resultierende DataTable-Schema wie die Columns-Schemaauflistung. Der einzige Unterschied besteht darin, dass AllColumns Spaltensatzspalten einschließt, die nicht in der Columns-Schemaauflistung enthalten sind. In der folgenden Liste werden diese Spalten beschrieben.  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|String|Katalog der Tabelle.|  
|TABLE_SCHEMA|String|Schema, das die Tabelle enthält.|  
|table_name|String|Tabellenname.|  
|COLUMN_NAME|String|Spaltenname.|  
|ORDINAL_POSITION|Int32|Identifikationsnummer der Spalte|  
|COLUMN_DEFAULT|String|Standardwert der Spalte|  
|IS_NULLABLE|String|NULL-Zulässigkeit der Spalte. Wenn diese Spalte NULL zulässt, gibt die Spalte YES zurück. Andernfalls wird NO zurückgegeben.|  
|DATA_TYPE|String|Vom System bereitgestellter Datentyp|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Maximale Länge (in Zeichen) für binäre Daten, Zeichendaten, Text- und Tmage-Daten Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_OCTET_LENGTH|Int32|Maximale Länge (in Bytes) für binäre Daten, Zeichendaten, Text- und Image-Daten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION|Byte ohne Vorzeichen|Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION_RADIX|Int16|Basis der Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_SCALE|Int32|Anzahl der Dezimalstellen für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|DATETIME_PRECISION|Int16|Untertypcode für den datetime-Datentyp und den SQL-92-Datentyp interval. Für andere Datentypen wird NULL zurückgegeben.|  
|CHARACTER_SET_CATALOG|String|Gibt "master" zurück, wodurch die Datenbank angegeben wird, in der der Zeichensatz enthalten ist, wenn die Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_SET_SCHEMA|String|Gibt immer NULL zurück.|  
|CHARACTER_SET_NAME|String|Gibt den eindeutigen Namen für den Zeichensatz zurück, wenn diese Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls wird NULL zurückgegeben.|  
|COLLATION_CATALOG|String|Gibt master zurück, wodurch die Datenbank angegeben wird, in der die Sortierung definiert ist, wenn die Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls ist diese Spalte NULL.|  
|IS_FILESTREAM|String|YES, wenn die Spalte über das FILESTREAM-Attribut verfügt.<br /><br /> NO, wenn die Spalte nicht über das FILESTREAM-Attribut verfügt.|  
|IS_SPARSE|String|YES, wenn die Spalte eine Sparsespalte ist.<br /><br /> NO, wenn die Spalte keine Sparsespalte ist.|  
|IS_COLUMN_SET|String|YES, wenn die Spalte eine Spaltensatzspalte ist.<br /><br /> NO, wenn die Spalte keine Spaltensatzspalte ist.|  

## <a name="columnsetcolumns"></a>ColumnSetColumns

Die ColumnSetColumns-Schemasammlung wird zur Unterstützung von Sparsespalten verwendet. Die ColumnSetColumns-Schemaauflistung gibt das Schema für alle Spalten in einem Spaltensatz zurück. In der folgenden Liste werden diese Spalten beschrieben.  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|String|Katalog der Tabelle.|  
|TABLE_SCHEMA|String|Schema, das die Tabelle enthält.|  
|table_name|String|Tabellenname.|  
|COLUMN_NAME|String|Spaltenname.|  
|ORDINAL_POSITION|Int32|Identifikationsnummer der Spalte|  
|COLUMN_DEFAULT|String|Standardwert der Spalte|  
|IS_NULLABLE|String|NULL-Zulässigkeit der Spalte. Wenn diese Spalte NULL zulässt, gibt die Spalte YES zurück. Andernfalls wird NO zurückgegeben.|  
|DATA_TYPE|String|Vom System bereitgestellter Datentyp|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Maximale Länge (in Zeichen) für binäre Daten, Zeichendaten, Text- und Tmage-Daten Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_OCTET_LENGTH|Int32|Maximale Länge (in Bytes) für binäre Daten, Zeichendaten, Text- und Image-Daten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION|Byte ohne Vorzeichen|Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_PRECISION_RADIX|Int16|Basis der Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|NUMERIC_SCALE|Int32|Anzahl der Dezimalstellen für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|DATETIME_PRECISION|Int16|Untertypcode für den datetime-Datentyp und den SQL-92-Datentyp interval. Für andere Datentypen wird NULL zurückgegeben.|  
|CHARACTER_SET_CATALOG|String|Gibt "master" zurück, wodurch die Datenbank angegeben wird, in der der Zeichensatz enthalten ist, wenn die Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls wird NULL zurückgegeben.|  
|CHARACTER_SET_SCHEMA|String|Gibt immer NULL zurück.|  
|CHARACTER_SET_NAME|String|Gibt den eindeutigen Namen für den Zeichensatz zurück, wenn diese Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls wird NULL zurückgegeben.|  
|COLLATION_CATALOG|String|Gibt master zurück, wodurch die Datenbank angegeben wird, in der die Sortierung definiert ist, wenn die Spalte den Typ Zeichendaten oder Textdaten aufweist. Andernfalls ist diese Spalte NULL.|  
|IS_FILESTREAM|String|YES, wenn die Spalte über das FILESTREAM-Attribut verfügt.<br /><br /> NO, wenn die Spalte nicht über das FILESTREAM-Attribut verfügt.|  
|IS_SPARSE|String|YES, wenn die Spalte eine Sparsespalte ist.<br /><br /> NO, wenn die Spalte keine Sparsespalte ist.|  
|IS_COLUMN_SET|String|YES, wenn die Spalte eine Spaltensatzspalte ist.<br /><br /> NO, wenn die Spalte keine Spaltensatzspalte ist.|  

## <a name="users"></a>Benutzer  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|uid|Int16|In dieser Datenbank eindeutiger Benutzername. 1 ist der Datenbankbesitzer.|  
|user_name|String|Benutzername oder Gruppenname, eindeutig innerhalb dieser Datenbank.|  
|createdate|Datetime|Datum, an dem das Konto hinzugefügt wurde.|  
|updatedate|Datetime|Datum, an dem das Konto zuletzt geändert wurde.| 

## <a name="views"></a>Sichten  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|String|Katalog der Ansicht.|  
|TABLE_SCHEMA|String|Schema, das die Ansicht enthält.|  
|table_name|String|Ansichtsname.|  
|CHECK_OPTION|String|WITH CHECK OPTION-Typ. Ist CASCADE, wenn die ursprüngliche Ansicht mit der WITH CHECK OPTION erstellt wurde. Andernfalls wird NONE zurückgegeben.|  
|IS_UPDATABLE|String|Gibt an, ob die Sicht aktualisierbar ist. Es wird immer NO zurückgegeben.|  

## <a name="viewcolumns"></a>ViewColumns  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|VIEW_CATALOG|String|Katalog der Ansicht.|  
|VIEW_SCHEMA|String|Schema, das die Ansicht enthält.|  
|VIEW_NAME|String|Ansichtsname.|  
|TABLE_CATALOG|String|Katalog der Tabelle, die dieser Ansicht zugeordnet ist.|  
|TABLE_SCHEMA|String|Schema, das die Tabelle enthält, die dieser Ansicht zugeordnet ist.|  
|table_name|String|Name der Tabelle, die der Ansicht zugeordnet ist. Basistabelle.|  
|COLUMN_NAME|String|Spaltenname.|  

## <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|assembly_name|String|Der Name der Datei für die Assembly.|  
|udt_name|String|Der Klassenname für die Assembly.|  
|version_major|Object|Nummer der Hauptversion.|  
|version_minor|Object|Nummer der Nebenversion.|  
|version_build|Object|Buildnummer.|  
|version_revision|Object|Revisionsnummer.|  
|culture_info|Object|Die diesem UDT zugeordneten Kulturinformationen.|  
|public_key|Object|Der von dieser Assembly verwendete öffentliche Schlüssel.|  
|is_fixed_length|Boolean|Gibt an, ob die Länge des Typs immer mit max_length übereinstimmt.|  
|max_length|Int16|Maximale Länge des Typs in Byte.|  
|Create_Date|Datetime|Datum, an dem die Assembly erstellt/registriert wurde.|  
|Permission_set_desc|String|Der angezeigte Name für die Berechtigungen/Sicherheitsebene der Assembly.|  

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen von Informationen zum Datenbankschema](retrieving-database-schema-information.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
