---
title: Erweiterbarkeitsframework-API
titleSuffix: SQL Server Language Extensions
description: Sie können das Erweiterbarkeitsframework verwenden, um Programmiersprachenerweiterungen für SQL Server zu schreiben. Bei der Erweiterbarkeitsframework-API für Microsoft SQL Server handelt es sich um eine API, die von einer Programmiersprachenerweiterung verwendet werden kann, um mit SQL Server zu interagieren und um Daten mit SQL Server auszutauschen.
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 244a8ce680704b0b25f9d0499e1a7802661d56cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471691"
---
# <a name="extensibility-framework-api-for-sql-server"></a>Erweiterbarkeitsframework-API für SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Sie können das Erweiterbarkeitsframework verwenden, um Programmiersprachenerweiterungen für SQL Server zu schreiben. Bei der Erweiterbarkeitsframework-API für Microsoft SQL Server handelt es sich um eine API, die von einer Programmiersprachenerweiterung verwendet werden kann, um mit SQL Server zu interagieren und um Daten mit SQL Server auszutauschen.

Als Ersteller einer Programmiersprachenerweiterung können Sie diese Referenz und die Open-Source-Programmiersprachenerweiterungen verwenden, um ein Verständnis dafür zu erwerben, wie Sie die API zum Schreiben eigener Programmiersprachenerweiterungen verwenden können. Den Quellcode für die Programmiersprachenerweiterungen finden Sie unter [aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions).

Im Folgenden finden Sie die Syntax- und Argumentinformationen zu allen API-Funktionen.

## <a name="return-value"></a>Rückgabewert

Alle Funktionen geben einen *SQLRETURN*-Parameter zurück. Wenn der Wert nicht *SQL_SUCCESS* lautet, wird er als Fehler behandelt, und die Ausführung des Skripts wird angehalten.

## <a name="standard-output"></a>Standardausgabe

Alle Ausgaben der Erweiterung zur Standardausgabe oder zu Fehlerstreams werden in der Protokolldatei der Sitzung und ggf. zu SQL Server nachverfolgt. Dasselbe gilt für alles, was auf der Registerkarte zu SSMS-Meldungen angezeigt wird.


## <a name="init"></a>Init

Diese Funktion wird nur einmal aufgerufen und zum Initialisieren der Laufzeit für die Ausführung verwendet. 

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>Argumente

*ExtensionParams*  
\[Eingabe\] Eine NULL-terminierte Zeichenfolge, die den `PARAMETERS`-Wert enthält, der während [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) oder [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md) bereitgestellt wurde

*ExtensionParamsLength*  
\[Eingabe\] Die Länge von *ExtensionParams* in Byte (ohne das Zeichen der NULL-Terminierung)

*ExtensionPath*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die den absoluten Pfad zum Installationsverzeichnis der Erweiterung enthält

*ExtensionPathLength*  
\[Eingabe\] Die Länge von *ExtensionPath* in Byte (ohne das Zeichen der NULL-Terminierung)

*PublicLibraryPath*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die den absoluten Pfad zum Verzeichnis der öffentlichen externen Bibliotheken für diese externe Programmiersprache enthält

*PublicLibraryPathLength*  
\[Eingabe\] Die Länge von *PublicLibraryPath* in Byte (ohne das Zeichen der NULL-Terminierung)

*PrivateLibraryPath*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die den absoluten Pfad zum Verzeichnis der öffentlichen externen Bibliotheken für diesen Benutzer und diese externe Programmiersprache enthält

*PrivateLibraryPathLength*  
\[Eingabe\] Die Länge von *PrivateLibraryPath* in Byte (ohne das Zeichen der NULL-Terminierung)

## <a name="initsession"></a>InitSession

Diese Funktion wird einmal pro Sitzung aufgerufen und initialisiert sitzungsspezifische Einstellungen.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*Skript*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) enthält

*ScriptLength*  
\[Eingabe\] Die Länge von *ScriptScript* in Byte (ohne das Zeichen der NULL-Terminierung)

*InputSchemaColumnsNumber*  
\[Eingabe\] Die Anzahl der Spalten im Resultset aus `@input_data_1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

*ParametersNumber*  
\[Eingabe\] Die Anzahl der Eingabeparameter aus `@params` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

*InputDataName*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die `@input_data_1_name` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) enthält

*InputDataNameLength*  
\[Eingabe\] Die Länge von *InputDataName* in Byte (ohne das Zeichen der NULL-Terminierung)

*OutputDataName*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die `@output_data_1_name` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) enthält

*OutputDataNameLength*  
\[Eingabe\] Die Länge von *OutputDataName* in Byte (ohne das Zeichen der NULL-Terminierung)

## <a name="initcolumn"></a>InitColumn

Initialisieren Sie die Informationen für eine bestimmte Spalte für eine bestimmte Sitzung.

Diese Funktion wird für jede Spalte im Resultset aus `@input_data_1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) aufgerufen.

Die Spaltenstruktur dieses Resultsets wird als *Eingabeschema* bezeichnet.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*ColumnNumber*  
\[Eingabe\] Eine ganze Zahl, die den Index dieser Spalte im Eingabeschema identifiziert. Spalten werden sequenziell in aufsteigender Reihenfolge nummeriert, beginnend mit 0.

*ColumnName*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die den Namen der Spalte enthält

*ColumnNameLength*  
\[Eingabe\] Die Länge von *ColumnName* in Byte (ohne das Zeichen der NULL-Terminierung)

*DataType*  
\[Eingabe\] Der ODBC-C-Typ, der den Datentyp dieser Spalte identifiziert

*ColumnSize*  
\[Eingabe\] Die maximale Größe der zugrunde liegenden Daten in dieser Spalte in Byte

Bei den Datentypen „SQL_C_CHAR“, „SQL_C_WCHAR“ und „SQL_C_BINARY“ geben Werte über 8000 an, dass diese Spalte LOB-Objekte mit Größen bis zu 2 GB darstellt.

*DecimalDigits*  
\[Eingabe\] Die Dezimalstellen der zugrunde liegenden Daten in dieser Spalte, wie unter [Dezimalstellen](../../odbc/reference/appendixes/decimal-digits.md) definiert

*NULL zulassen*  
\[Eingabe\] Ein Wert, der angibt, ob diese Spalte NULL-Werte enthalten kann. Mögliche Werte:

- SQL_NO_NULLS: Die Spalte kann keine NULL-Werte enthalten.
- SQL_NULLABLE: Die Spalte kann NULL-Werte enthalten.

*PartitionByNumber*  
\[Eingabe\] Ein Wert, der den Index dieser Spalte in der `@input_data_1_partition_by_columns`-Sequenz in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) angibt. Spalten werden sequenziell in aufsteigender Reihenfolge nummeriert, beginnend mit 0. Wenn diese Spalte in der Sequenz nicht enthalten ist, ist der Wert –1.

*OrderByNumber*  
\[Eingabe\] Ein Wert, der den Index dieser Spalte in der `@input_data_1_order_by_columns`-Sequenz in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) angibt. Spalten werden sequenziell in aufsteigender Reihenfolge nummeriert, beginnend mit 0. Wenn diese Spalte in der Sequenz nicht enthalten ist, ist der Wert –1.

## <a name="initparam"></a>InitParam

Initialisieren Sie die Informationen zu einem angegebenen Eingabeparameter für eine bestimmte Sitzung.

Diese Funktion wird für jeden Parameter aus `@params` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) aufgerufen.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*ParamNumber*  
\[Eingabe\] Eine ganze Zahl, die den Index dieses Parameters identifiziert. Parameter werden sequenziell in aufsteigender Reihenfolge nummeriert, beginnend mit 0.

*ParamName*  
\[Eingabe\] Eine NULL-terminierte UTF-8-Zeichenfolge, die den Namen des Parameters enthält

*ParamNameLength*  
\[Eingabe\] Die Länge von *ParamName* in Byte (ohne das Zeichen der NULL-Terminierung)

*DataType*  
\[Eingabe\] Der ODBC-C-Typ, der den Datentyp dieses Parameters identifiziert

*ParamSize*  
\[Eingabe\] Die maximale Größe der zugrunde liegenden Daten in diesem Parameter in Byte

Bei den Datentypen „SQL_C_CHAR“, „SQL_C_WCHAR“ und „SQL_C_BINARY“ geben Werte über 8000 an, dass dieser Parameter LOB-Objekte mit Größen bis zu 2 GB darstellt.

*DecimalDigits*  
\[Eingabe\] Die Dezimalstellen der zugrunde liegenden Daten in diesem Parameter, wie unter [Dezimalstellen](../../odbc/reference/appendixes/decimal-digits.md) definiert

*ParamValue*  
\[Eingabe\] Ein Zeiger auf einen Puffer, der den Wert des Parameters enthält

*StrLen_or_Ind*  
\[Eingabe\] Eine ganze Zahl, die die Länge von *ParamValue* in Byte angibt, oder „SQL_NULL_DATA“, um anzugeben, dass die Daten NULL ergeben

„StrLen_or_Ind\[col\]“ kann ignoriert werden, wenn eine Spalte keine NULL-Werte zulässt und keinen der folgenden Datentypen darstellt: „SQL_C_CHAR“, „SQL_C_WCHAR“ und „SQL_C_BINARY“, „SQL_C_NUMERIC“ oder „SQL_C_TYPE_TIMESTAMP“. Andernfalls wird auf ein gültiges Array mit \[RowsNumber\]-Elementen verwiesen, wobei jedes Element seine Längen- oder NULL-Indikatordaten enthält.

*InputOutputType*  
\[Eingabe\] Der Typ des Parameters. Das *InputOutputType*-Argument ist einer der folgenden Werte:

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>Execute

Führen Sie das `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) aus.

Diese Funktion kann mehrmals aufgerufen werden, einmal pro Steam-Block und pro Partition in `@input_data_1_partition_by_columns` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Syntax

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*RowsNumber*  
\[Eingabe\] Die Anzahl der Zeilen in *Data*

*Daten*  
\[Eingabe\] Ein zweidimensionales Array, das das Resultset von `@input_data_1` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) enthält.

Die Gesamtanzahl der Spalten ist der Wert *InputSchemaColumnsNumber*, der beim [InitSession](#initsession)-Aufruf abgerufen wurde. Jede Spalte enthält *RowsNumber*-Elemente, die gemäß dem Spaltentyp aus [InitColumn](#initcolumn) interpretiert werden sollten.

Elemente, die in *StrLen_or_Ind* als NULL angegeben werden, sind nicht garantiert gültig und sollten ignoriert werden.

*StrLen_or_Ind*  
\[Eingabe\] Ein zweidimensionales Array, das den Längen-/NULL-Indikator für jeden Wert in *Data* enthält. Mögliche Werte für jede Zelle:

- n, wobei n > 0: Dieser Wert gibt die Länge der Daten in Bytes an.
- SQL_NULL_DATA: Dieser Wert gibt einen NULL-Wert an.

Die Gesamtanzahl der Spalten ist der Wert *InputSchemaColumnsNumber*, der beim [InitSession](#initsession)-Aufruf abgerufen wurde. Jede Spalte enthält *RowsNumber*-Elemente, die gemäß dem Spaltentyp aus [InitColumn](#initcolumn) interpretiert werden sollten.

„StrLen_or_Ind\[col\]“ kann ignoriert werden, wenn eine Spalte keine NULL-Werte zulässt und keinen der folgenden Datentypen darstellt: „SQL_C_CHAR“, „SQL_C_WCHAR“ und „SQL_C_BINARY“, „SQL_C_NUMERIC“ oder „SQL_C_TYPE_TIMESTAMP“. Andernfalls wird auf ein gültiges Array mit *RowsNumber*-Elementen verwiesen, wobei jedes Element seine Längen- oder NULL-Indikatordaten enthält.

*OutputSchemaColumnsNumber*  
\[Ausgabe\] Zeiger auf einen Puffer, in den die Anzahl der Spalten im erwarteten Resultset von `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) zurückgegeben werden soll

## <a name="getresultcolumn"></a>GetResultColumn

Rufen Sie die Informationen zu einer angegebenen Ausgabespalte für eine bestimmte Sitzung ab.

Diese Funktion wird für jede Spalte im Resultset aus `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) aufgerufen.
Die Spaltenstruktur dieses Resultsets wird als das „Ausgabeschema“ bezeichnet.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*ColumnNumber*  
\[Eingabe\] Eine ganze Zahl, die den Index dieser Spalte im Ausgabeschema identifiziert. Spalten werden sequenziell in aufsteigender Reihenfolge nummeriert, beginnend mit 0.

*DataType*  
\[Ausgabe\] Ein Zeiger auf den Puffer, der den ODBC-C-Typ enthält, der den Datentyp dieser Spalte identifiziert

*ColumnSize*  
\[Ausgabe\] Ein Zeiger auf einen Puffer, der die maximale Größe der zugrunde liegenden Daten in dieser Spalte in Byte enthält

*DecimalDigits*  
\[Ausgabe\] Ein Zeiger auf einen Puffer, der die Dezimalstellen der zugrunde liegenden Daten in dieser Spalte enthält, wie unter [Dezimalstellen](../../odbc/reference/appendixes/decimal-digits.md) definiert. Wenn die Anzahl von Dezimalstellen nicht bestimmt werden kann oder nicht anwendbar ist, wird der Wert verworfen.

*NULL zulassen*  
\[Ausgabe\] Ein Zeiger auf einen Puffer, der einen Wert enthält, der angibt, ob diese Spalte NULL-Werte enthalten kann. Mögliche Werte:

- SQL_NO_NULLS: Die Spalte kann keine NULL-Werte enthalten.
- SQL_NULLABLE: Die Spalte kann NULL-Werte enthalten.

Wenn andere Werte übermittelt werden, wird die Ausführung angehalten.

## <a name="getresults"></a>GetResults

Rufen Sie das Resultset aus der Ausführung von `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ab.

Diese Funktion kann mehrmals aufgerufen werden, einmal pro Steam-Block und pro Partition in `@input_data_1_partition_by_columns` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Syntax

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER**_   StrLen_or_Ind
);
```

### <a name="arguments"></a>Argumente

SessionId  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*RowsNumber*  
\[Ausgabe\] Ein Zeiger auf einen Puffer, der die Anzahl der Zeilen in *Data* enthält

*Daten*  
\[Ausgabe\] Ein Zeiger auf ein zweidimensionales Array, das von der Erweiterung zugeordnet wurde und das Resultset von `@script` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) enthält.

Die Gesamtanzahl der Spalten sollte dem Wert *OutputSchemaColumnsNumber* entsprechen, der beim [Execute](#execute)-Aufruf abgerufen wurde. Jede Spalte sollte *RowsNumber*-Elemente enthalten, die gemäß dem Spaltentyp aus [GetResultColumn](#getresultcolumn) interpretiert werden sollten.

*StrLen_or_Ind*  
\[Ausgabe\] Ein Zeiger auf ein zweidimensionales Array, das von der Erweiterung zugeordnet wurde und den Längen-/NULL-Indikator für jeden Wert in *Data* enthält. Mögliche Werte für jede Zelle:

- n, wobei n > 0: Dieser Wert gibt die Länge der Daten in Bytes an.
- SQL_NULL_DATA: Dieser Wert gibt einen NULL-Wert an.

Die Gesamtanzahl der Spalten sollte dem Wert *OutputSchemaColumnsNumber* entsprechen, der beim [Execute](#execute)-Aufruf abgerufen wurde. Jede Spalte enthält *RowsNumber*-Elemente, die gemäß dem Spaltentyp aus [GetResultColumn](#getresultcolumn) interpretiert werden sollten.

„StrLen_or_Ind\[col\]“ wird ignoriert, wenn eine Spalte keine NULL-Werte zulässt und keinen der folgenden Datentypen darstellt: „SQL_C_CHAR“, „SQL_C_WCHAR“ und „SQL_C_BINARY“ [Datumsangaben hinzufügen]. Andernfalls wird auf ein gültiges Array mit *RowsNumber*-Elementen verwiesen, wobei jedes Element seine Längen- oder NULL-Indikatordaten enthält.

## <a name="getoutputparam"></a>GetOutputParam

Rufen Sie die Informationen zu einem angegebenen Ausgabeparameter für eine bestimmte Sitzung ab.

Diese Funktion wird für jeden Parameter aus `@params` in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) aufgerufen, der mit „OUTPUT“ (Ausgabe) gekennzeichnet ist.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*ParamValue*  
\[Ausgabe\] Ein Zeiger auf einen Puffer, der den Wert des Parameters enthält

*StrLen_or_Ind* \[Ausgabe\] Ein Zeiger auf einen Puffer, der eine ganze Zahl enthält, die die Länge von *ParamValue* in Byte angibt, oder „SQL_NULL_DATA“, um anzugeben, dass die Daten NULL ergeben

## <a name="getinterfaceversion"></a>GetInterfaceVersion

Rufen Sie die Schnittstellenversion ab.
Diese Funktion gibt eine ganze Zahl zurück, die die Schnittstellenversion der Erweiterung darstellt. Unterstützte Werte:
1. Bei Version 1 handelt es sich um die erste API-Version. Sie wird unter SQL Server 2019 RTM unterstützt.
1. Version 2 bietet zusätzliche Unterstützung für die InstallExternalLibrary- und die UninstallExternalLibrary-API und wird ab SQL Server 2019 CU3 unterstützt.                            

### <a name="syntax"></a>Syntax

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

Bereinigen Sie sitzungsspezifische Informationen.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

## <a name="cleanup"></a>Cleanup

Bereinigen Sie globale, freigegebene Informationen (z. B. JVM).

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

Dieser Wert ruft Telemetriedaten (Schlüssel-Wert-Paare) aus der Erweiterung ab. Die Funktion ist optional und erfordert keine Implementierung. Die Telemetriedaten werden von der dynamischen Verwaltungssicht (DMV) `dm_db_external_script_execution_stats` verfügbar gemacht.

Es gibt einen Zähler mit dem Namen „script_executions“, der vom Framework gesendet wird. Dieser Name sollte von der Erweiterung nicht verwendet werden.

Bei jedem Telemetrieeintrag handelt es sich um ein Schlüssel-Wert-Paar. Bei den Schlüsseln handelt es sich um Zeichenfolgen, die Werte sind ganze 64-Bit-Zahlen, also Zähler. Daher besteht die Ausgabe aus zwei logischen Arrays: den Namen und den entsprechenden Zählern. Bei jedem Array handelt es sich um eine Ausgabe.

Die Länge jedes Arrays ist *RowsNumber*, bei dem es sich um eine Ausgabe handelt. Die erste logische Ausgabe enthält Zeiger auf Zeichenfolgen und wird daher durch zwei Arrays dargestellt: *CounterNames* (die eigentlichen Zeichenfolgendaten) und *CounterNamesLength* (die Länge jeder Zeichenfolge). Die zweite logische Ausgabe wird im Zeiger *CounterValues* gespeichert.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>Argumente

*SessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*TaskId*  
\[Eingabe\] Eine ganze Zahl, die diesen Ausführungsprozess eindeutig identifiziert

Wenn in [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) `@parallel = 1` gilt, liegt dieser Wert zwischen 0 und dem Grad der Parallelität der Abfrage.

*RowsNumber*  
\[Ausgabe\] Die Anzahl von Schlüssel-Wert-Paaren

*CounterNames*  
\[Ausgabe\] Die Zeichenfolgendaten, die die Schlüssel enthalten

*CounterNamesLength*  
\[Ausgabe\] Die Länge jeder Schlüsselzeichenfolge

*CounterValues*  
\[Ausgabe\] Die Daten ganzer 64-Bit-Zahlen, die die Werte enthalten

## <a name="installexternallibrary"></a>InstallExternalLibrary

Dieser Wert installiert eine Bibliothek. Die Funktion ist optional und erfordert keine Implementierung. Die Standardimplementierung besteht darin, den Inhalt der Bibliothek (siehe [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)) in eine Datei am richtigen Speicherort zu kopieren. Der Dateiname ist der Bibliotheksname.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Argumente

*SetupSessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*LibraryName*  
\[Eingabe\] Der Bibliotheksname

*LibraryNameLength*  
\[Eingabe\] Die Länge des Bibliotheksnamens

*LibraryFile*  
\[Eingabe\] Der Pfad (als Zeichenfolge) zu der Bibliotheksdatei, die den von [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) angegebenen binären Inhalt enthält

*LibraryFileLength*  
\[Eingabe\] Die Länge der „LibraryFile“-Zeichenfolge

*LibraryInstallDirectory:*  
\[Eingabe\] Das Stammverzeichnis, in dem die Bibliothek installiert werden soll

*LibraryInstallDirectoryLength*  
\[Eingabe\] Die Länge der „LibraryInstallDirectory“-Zeichenfolge

*LibraryError*  
\[Ausgabe\] Ein optionaler Ausgabeparameter. Wenn während der Installation der Bibliothek ein Fehler aufgetreten ist, verweist „LibraryError“ auf eine Zeichenfolge, die den Fehler beschreibt.

*LibraryErrorLength*  
\[Ausgabe\] Die Länge der „LibraryError“-Zeichenfolge

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

Dieser Wert deinstalliert eine Bibliothek. Die Funktion ist optional und erfordert keine Implementierung. Die Standardimplementierung besteht darin, die von der Standardimplementierung von „InstallExternalLibrary“ ausgeführte Aktion rückgängig zu machen. Die Standardimplementierung löscht den Inhalt der Datei *LibraryName* unter *LibraryInstallDirectory*.

### <a name="syntax"></a>Syntax

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Argumente

*SetupSessionId*  
\[Eingabe\] Die GUID, die diese Skriptsitzung eindeutig identifiziert

*LibraryName*  
\[Eingabe\] Der Bibliotheksname

*LibraryNameLength*  
\[Eingabe\] Die Länge des Bibliotheksnamens

*LibraryFile*  
\[Eingabe\] Der Pfad (als Zeichenfolge) zu der Bibliotheksdatei, die den von „CREATE EXTERNAL LIBRARY“ angegebenen binären Inhalt enthält

*LibraryFileLength*  
\[Eingabe\] Die Länge der „LibraryFile“-Zeichenfolge

*LibraryInstallDirectory*  
\[Eingabe\] Das Stammverzeichnis, in dem die Bibliothek installiert werden soll

*LibraryInstallDirectoryLength*  
\[Eingabe\] Die Länge der „LibraryInstallDirectory“-Zeichenfolge

*LibraryError*  
\[Ausgabe\] Die „LibraryError“-Zeichenfolge

*LibraryErrorLength*  
\[Ausgabe\] Die Länge der „LibraryError“-Zeichenfolge

## <a name="next-steps"></a>Nächste Schritte

- [Microsoft-Erweiterbarkeits-SDK für Java für SQL Server](../how-to/extensibility-sdk-java-sql-server.md)
- [Benutzerdefinierte Python-Runtime](../../machine-learning/install/custom-runtime-python.md)
- [Benutzerdefinierte R-Runtime](../../machine-learning/install/custom-runtime-r.md).