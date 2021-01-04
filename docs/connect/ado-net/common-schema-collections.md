---
title: Allgemeine Schemaauflistungen
description: In diesem Artikel werden alle allgemeinen Schemasammlungen beschrieben, die von allen von .NET verwalteten Anbietern unterstützt werden.
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f84cc2940e56116b9cef4600b21fe742f4ae2d07
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051322"
---
# <a name="common-schema-collections"></a>Allgemeine Schemaauflistungen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Die allgemeinen Schemasammlungen sind die Schemasammlungen, die von allen von .NET verwalteten Anbietern implementiert werden. Sie können einen von.NET verwalteten Anbieter abfragen, um die Liste der unterstützten Schemasammlungen zu ermitteln. Rufen Sie hierzu die **GetSchema**-Methode ohne Argumente oder mit dem Schemasammlungsnamen „MetaDataCollections“ auf. Dadurch wird <xref:System.Data.DataTable> mit einer Liste der unterstützten Schemaauflistungen, der Anzahl der von diesen Schemaauflistungen unterstützten Einschränkungen und der Anzahl der von diesen Schemaauflistungen verwendeten Bezeichnerteilen zurückgegeben. In diesen Auflistungen werden alle erforderlichen Spalten beschrieben. Anbieter können ggf. zusätzliche Spalten hinzufügen. Beispielsweise fügt der Microsoft SqlClient-Datenanbieter für SQL Server der Sammlung Restrictions (Einschränkungen) ParameterName hinzu.  

Wenn ein Anbieter den Wert einer erforderlichen Spalte nicht ermitteln kann, wird NULL zurückgegeben.  
  
Weitere Informationen zur Verwendung der **GetSchema**-Methoden finden Sie unter [GetSchema und Schemasammlungen](getschema-and-schema-collections.md).  

## <a name="metadatacollections"></a>MetaDataCollections  

Diese Schemasammlung stellt Informationen zu allen Schemasammlungen bereit, die vom Microsoft SqlClient-Datenanbieter für SQL Server unterstützt werden, mit dem derzeit eine Verbindung mit der Datenbank hergestellt wird.  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|CollectionName|Zeichenfolge|Hierbei handelt es sich um den Namen der Sammlung, der zum Zurückgeben der Sammlung an die **GetSchema**-Methode übergeben werden soll.|  
|NumberOfRestrictions|INT|Die Anzahl der Einschränkungen, die für die Auflistung angegeben werden können.|  
|NumberOfIdentifierParts|INT|Die Anzahl der Bestandteile im zusammengesetzten Bezeichner/Datenbank-Objektnamen. In SQL Server entspricht dies beispielsweise 3 für Tabellen und 4 für Spalten.|  

## <a name="datasourceinformation"></a>DataSourceInformation  

Diese Schemasammlung stellt Informationen zu Datenquellen bereit, mit denen der Microsoft SqlClient-Datenanbieter für SQL Server derzeit verbunden ist.  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|CompositeIdentifierSeparatorPattern|Zeichenfolge|Der reguläre Ausdruck, der den Trennzeichen zum Trennen der Bestandteile in einem zusammengesetzten Bezeichner entspricht. Beispiel: „\\“. (für SQL Server).<br /><br /> Ein zusammengesetzter Bezeichner wird in der Regel für einen Datenbankobjektnamen verwendet, z. B. „pubs.dbo.authors“ oder „pubs\@dbo.authors“.<br /><br /> Verwenden Sie für SQL Server den regulären Ausdruck „\\.“. |  
|DataSourceProductName|Zeichenfolge|Hierbei handelt es sich um den Namen des Produkts, auf das der Anbieter zugreift, z. B. „SQLServer“.|  
|DataSourceProductVersion|Zeichenfolge|Gibt die Version des Produkts, auf das durch den Anbieter zugegriffen wird, im systemeigenen Format der Datenquellen an, nicht im Microsoft-Format.<br /><br /> In einigen Fällen sind die Werte von "DataSourceProductVersion" und "DataSourceProductVersionNormalized" identisch. |  
|DataSourceProductVersionNormalized|Zeichenfolge|Eine normalisierte Version der Datenquelle, damit sie mithilfe von `String.Compare()` verglichen werden kann. Das Format ist für alle Versionen des Anbieters konsistent, um zu verhindern, dass Version 10 zwischen Version 1 und Version 2 einsortiert wird.<br /><br /> SQL Server verwendet z. B. das Microsoft-Standardformat „nn.nn.nnnn“.<br /><br /> In einigen Fällen sind die Werte von DataSourceProductVersion und DataSourceProductVersionNormalized identisch. |  
|GroupByBehavior|<xref:System.Data.Common.GroupByBehavior>|Gibt die Beziehung zwischen den Spalten in einer GROUP BY-Klausel und den nicht zusammengesetzten Spalten in der Auswahlliste an.|  
|IdentifierPattern|Zeichenfolge|Ein regulärer Ausdruck, der einem Bezeichner entspricht und über einen Wert verfügt, der den Bezeichner darstellt. Beispiel: "[A-Za-z0-9_#$]".|  
|IdentifierCase|<xref:System.Data.Common.IdentifierCase>|Gibt an, ob die Groß- und Kleinschreibung bei nicht in Anführungszeichen stehenden Bezeichnern berücksichtigt werden soll.|  
|OrderByColumnsInSelect|bool|Gibt an, ob Spalten in einer ORDER BY-Klausel in der Auswahlliste vorhanden sein müssen. Der Wert "true" gibt an, dass die Spalten in der Auswahlliste vorhanden sein müssen. Der Wert "false" gibt an, dass sie nicht in der Auswahlliste vorhanden sein müssen.|  
|ParameterMarkerFormat|Zeichenfolge|Eine Formatzeichenfolge, die die Formatierung des Parameters darstellt.<br /><br /> Wenn benannte Parameter von der Datenquelle unterstützt werden, muss sich der erste Platzhalter in dieser Zeichenfolge an der Stelle befinden, an der der Parametername formatiert wird.<br /><br /> Wenn die Datenquelle beispielsweise erwartet, dass Parameter benannt sind und das Präfix „:“ aufweisen, wäre dies: „:{0}“. Bei der Formatierung dieses Beispiels mit dem Parameternamen "p1" lautet die resultierende Zeichenfolge also ":p1".<br /><br /> Wenn die Datenquelle erwartet, dass Parameter das Präfix „\@“ aufweisen, dieses jedoch bereits in den Namen enthalten ist, wäre die Zeichenfolge „{0}“. Das Ergebnis der Formatierung eines Parameters mit dem Namen „\@p1“ wäre dann einfach „\@p1“.<br /><br /> Für Datenquellen, die keine benannten Parameter und stattdessen die Verwendung des Zeichens „?“ erwarten, kann für die Formatzeichenfolge einfach „?“ angegeben werden. Dadurch wird der Parametername ignoriert. |  
|ParameterMarkerPattern|Zeichenfolge|Ein regulärer Ausdruck, der einer Parametermarkierung entspricht. Er verfügt (sofern vorhanden) über einen Wert, der dem Parameternamen entspricht.<br /><br /> Wenn beispielsweise benannte Parameter mit einem vorangestellten „\@“-Zeichen unterstützt werden, das in den Parameternamen eingeschlossen wird, lautet die Zeichenfolge „(\@[A-Za-z0-9_$#]*)“.<br /><br /> Wenn benannte Parameter jedoch mit einem vorangestellten „:“-Zeichen unterstützt werden, das nicht Teil des Parameternamens ist, lautet die Zeichenfolge „:([A-Za-z0-9_$#]\*)“.<br /><br /> Wenn die Datenquelle keine benannten Parameter unterstützt, lautet die Zeichenfolge natürlich einfach „?“.|  
|ParameterNameMaxLength|INT|Die maximale Länge eines Parameternamens in Zeichen. In Visual Studio werden im Falle der Unterstützung von Parameternamen 30 Zeichen als Mindestwert für die maximale Länge erwartet.<br /><br /> Wenn benannte Parameter von der Datenquelle nicht unterstützt werden, gibt diese Eigenschaft Null (0) zurück.|  
|ParameterNamePattern|Zeichenfolge|Ein regulärer Ausdruck, der den gültigen Parameternamen entspricht. Je nach Datenquelle sind die Regeln bezüglich der für Parameternamen zulässigen Zeichen verschieden.<br /><br /> In Visual Studio wird im Falle der Unterstützung von Parameternamen erwartet, dass die Zeichen "\p{Lu}\p{Ll}\p{Lt}\p{Lm}\p{Lo}\p{Nl}\p{Nd}" die in jedem Fall unterstützte Gruppe von für Parameternamen gültigen Zeichen darstellen.|  
|QuotedIdentifierPattern|Zeichenfolge|Ein regulärer Ausdruck, der einem Bezeichner in Anführungszeichen entspricht und über einen Wert verfügt, der den Bezeichner ohne Anführungszeichen darstellt. Wenn in der Datenquelle beispielsweise doppelte Anführungszeichen zum Identifizieren von Bezeichnern in Anführungszeichen verwendet wurden, lautet die Zeichenfolge: „(([^\\"]&#124;\\"\\")*)“.|  
|QuotedIdentifierCase|<xref:System.Data.Common.IdentifierCase>|Gibt an, ob die Groß- und Kleinschreibung bei Bezeichnern in Anführungszeichen berücksichtigt werden muss.|  
|StatementSeparatorPattern|Zeichenfolge|Ein regulärer Ausdruck, der dem Trennzeichen für Anweisungen entspricht.|  
|StringLiteralPattern|Zeichenfolge|Ein regulärer Ausdruck, der einem Zeichenfolgenliteral entspricht und über einen Wert verfügt, der das Literal darstellt. Wenn in der Datenquelle beispielsweise einfache Anführungszeichen zum Identifizieren von Zeichenfolgen verwendet wurden, lautet die Zeichenfolge: „('([^']&#124;'')*')“.|  
|SupportedJoinOperators|<xref:System.Data.Common.SupportedJoinOperators>|Gibt an, welche SQL-Joinanweisungen von der Datenquelle unterstützt werden.|  

## <a name="datatypes"></a>DataTypes  

Diese Schemasammlung stellt Informationen zu den von der Datenbank, mit der der Microsoft SqlClient-Datenanbieter für SQL Server derzeit verbunden ist, unterstützten Datentypen bereit.  

|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|TypName|Zeichenfolge|Der anbieterspezifische Datentypname.|  
|ProviderDbType|INT|Hierbei handelt es sich um den anbieterspezifischen Typwert, der beim Angeben des Typs eines Parameters verwendet werden sollte, z. B. SqlDbType.Money.|  
|ColumnSize|long|Die Länge einer nicht numerischen Spalte oder eines nicht numerischen Parameters bezieht sich entweder auf die maximale oder auf die für diesen Typ vom Anbieter definierte Länge.<br /><br /> Bei Zeichendaten ist dies die maximale oder definierte Länge in Einheiten, entsprechend der Definition in der Datenquelle. <br /><br /> Bei Datum/Uhrzeit-Datentypen ist dies die Länge der Zeichenfolgendarstellung (dabei wird von der maximal zulässigen Genauigkeit der Sekundenbruchteil-Komponente ausgegangen).<br /><br /> Wenn der Datentyp numerisch ist, ist dies die obere Grenze der maximalen Genauigkeit des Datentyps.|  
|CreateFormat|Zeichenfolge|Formatzeichenfolge, die darstellt, wie diese Spalte einer Datendefinitionsanweisung (z. B. CREATE TABLE) hinzugefügt wird. Jedes Element im CreateParameter-Array muss durch eine "Parametermarkierung" in der Formatzeichenfolge dargestellt werden.<br /><br /> Für den SQL-Datentyp DECIMAL ist eine Angabe zur Genauigkeit und zur Dezimalstellenanzahl erforderlich. In diesem Fall lautet die Formatzeichenfolge „DECIMAL({0},{1})“.|  
|CreateParameters|Zeichenfolge|Die Erstellungsparameter, die beim Erstellen einer Spalte dieses Datentyps angegeben werden müssen. Die Erstellungsparameter sind in der Zeichenfolge durch ein Komma getrennt in der Reihenfolge aufgelistet, in der sie bereitgestellt werden sollen.<br /><br /> Für den SQL-Datentyp DECIMAL ist eine Angabe zur Genauigkeit und zur Dezimalstellenanzahl erforderlich. In diesem Fall müssen die Erstellungsparameter die Zeichenfolge "Genauigkeit, Dezimalstellenanzahl" enthalten.<br /><br /> In einem Textbefehl zum Erstellen einer DECIMAL-Spalte mit einer Genauigkeit von 10 und einer Dezimalstellenanzahl von 2 lautet der Wert der CreateFormat-Spalte möglicherweise „DECIMAL({0},{1})“ und die vollständige Typspezifikation wäre dann „DECIMAL(10,2)“.|  
|DataType|Zeichenfolge|Hierbei handelt es sich um den Namen des .NET-Typs des Datentyps.|  
|IsAutoincrementable|bool|true – Die Werte dieses Datentyps können automatisch erhöht werden.<br /><br /> false – Die Werte dieses Datentyps können nicht automatisch erhöht werden.<br /><br /> Beachten Sie, dass auf diese Weise lediglich angegeben wird, ob eine Spalte dieses Datentyps automatisch erhöht werden kann, und nicht, dass alle Spalten dieses Typs automatisch erhöht werden.|  
|IsBestMatch|bool|true: Der Datentyp weist die höchste Übereinstimmung unter allen Datentypen im Datenspeicher auf und ist der .NET-Datentyp, der durch den Wert in der DataType-Spalte angegeben wird.<br /><br /> false – Der Datentyp stellt nicht die höchste Übereinstimmung dar.<br /><br /> Für jede Gruppe von Zeilen, in der der Wert der DataType-Spalte derselbe ist, wird die IsBestMatch-Spalte nur in einer Zeile auf "true" festgelegt.|  
|IsCaseSensitive|bool|true – Bei dem Datentyp handelt es sich um einen Zeichentyp, und die Groß- und Kleinschreibung muss berücksichtigt werden.<br /><br /> true – Bei dem Datentyp handelt es sich nicht um einen Zeichentyp, und die Groß- und Kleinschreibung muss nicht berücksichtigt werden.|  
|IsFixedLength|bool|true – Die von der DLL (Data Definition Language) erstellten Spalten dieses Datentyps weisen eine feste Länge auf.<br /><br /> false – Die von der DLL (Data Definition Language) erstellten Spalten dieses Datentyps weisen eine variable Länge auf.<br /><br /> DBNull.Value – Es ist nicht bekannt, ob dieses Feld vom Anbieter einer Spalte mit fester oder variabler Länge zugeordnet wird.|  
|IsFixedPrecisionScale|bool|true – Der Datentyp verfügt über eine feste Genauigkeit und Dezimalstellenanzahl.<br /><br /> false – Der Datentyp verfügt nicht über eine feste Genauigkeit und Dezimalstellenanzahl.|  
|IsLong|bool|true – Der Datentyp enthält sehr lange Daten. Die Definition hierfür ist anbieterspezifisch.<br /><br /> false – Der Datentyp enthält keine sehr langen Daten.|  
|IsNullable|bool|true – Der Datentyp lässt NULL-Werte zu.<br /><br /> false – Der Datentyp lässt keine NULL-Werte zu.<br /><br /> DBNull.Value – Es ist nicht bekannt, ob der Datentyp NULL-Werte zulässt.|  
|IsSearchable|bool|true – Der Datentyp kann in WHERE-Klauseln mit beliebigen Operatoren außer dem LIKE-Prädikat verwendet werden.<br /><br /> FALSE – Der Datentyp kann nicht in WHERE-Klauseln mit beliebigen Operatoren außer dem LIKE-Prädikat verwendet werden.|  
|IsSearchableWithLike|bool|TRUE – Der Datentyp kann mit dem LIKE-Prädikat verwendet werden.<br /><br /> FALSE – Der Datentyp kann nicht mit dem LIKE-Prädikat verwendet werden.|  
|IsUnsigned|bool|true – Der Datentyp hat kein Vorzeichen.<br /><br /> false – Der Datentyp hat ein Vorzeichen.<br /><br /> DBNull.Value – Nicht zutreffend für den Datentyp.|  
|MaximumScale|short|Wenn es sich beim Typindikator um einen numerischen Typ handelt, ist dies die maximal zulässige Anzahl von Ziffern rechts vom Dezimaltrennzeichen. Andernfalls ist dies DBNull.Value.|  
|MinimumScale|short|Wenn es sich beim Typindikator um einen numerischen Typ handelt, ist dies die minimal zulässige Anzahl von Ziffern rechts vom Dezimaltrennzeichen. Andernfalls ist dies DBNull.Value.|  
|IsConcurrencyType|bool|true – Der Datentyp wird immer dann von der Datenbank aktualisiert, wenn die Zeile geändert wird und sich der Wert der Spalte von allen vorherigen Werten unterscheidet.<br /><br /> false – Der Datentyp wird von der Datenbank nicht bei jeder Änderung der Zeile aktualisiert.<br /><br /> DBNull.Value – Die Datenbank unterstützt diese Art von Datentyp nicht.|  
|IsLiteralSupported|bool|true – Der Datentyp kann als Literal ausgedrückt werden.<br /><br /> true – Der Datentyp kann nicht als Literal ausgedrückt werden.|  
|LiteralPrefix|Zeichenfolge|Das auf ein angegebenes Literal angewendete Präfix.|  
|LiteralSuffix|Zeichenfolge|Das auf ein angegebenes Literal angewendete Suffix.|  

## <a name="restrictions"></a>Beschränkungen 

Diese Schemasammlung stellt Informationen zu den Einschränkungen bereit, die vom Microsoft SqlClient-Datenanbieter für SQL Server unterstützt werden, mit dem derzeit eine Verbindung mit der Datenbank hergestellt wird.  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|CollectionName|Zeichenfolge|Der Name der Auflistung, auf die diese Einschränkungen angewendet werden.|  
|RestrictionName|Zeichenfolge|Der Name der Einschränkung in der Auflistung.|  
|RestrictionDefault|Zeichenfolge|Ignoriert.|  
|RestrictionNumber|INT|Die tatsächliche Position in den Auflistungseinschränkungen, an der sich diese bestimmte Einschränkung befindet.|  

## <a name="reservedwords"></a>ReservedWords  

Diese Schemasammlung stellt Informationen zu den für die Datenbank, mit der der Microsoft SqlClient-Datenanbieter für SQL Server derzeit verbunden ist, reservierten Wörtern bereit.  
  
|ColumnName|DataType|BESCHREIBUNG|  
|----------------|--------------|-----------------|  
|ReservedWord|Zeichenfolge|Hierbei handelt es sich um ein anbieterspezifisches reserviertes Wort.|  

## <a name="see-also"></a>Weitere Informationen:

- [Abrufen von Datenbankschema-Informationen](retrieving-database-schema-information.md)
- [GetSchema und Schemasammlungen](getschema-and-schema-collections.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
