---
description: 'Sortierungsfunktionen: COLLATIONPROPERTY (Transact-SQL)'
title: COLLATIONPROPERTY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COLLATIONPROPERTY_TSQL
- COLLATIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], properties
- COLLATIONPROPERTY function
ms.assetid: f5029e74-a1db-4f69-b0f5-5ee920c3311d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b56940c6b0c3f8bacc193a1cfe7149117dc3d20
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98089058"
---
# <a name="collation-functions---collationproperty-transact-sql"></a>Sortierungsfunktionen: COLLATIONPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Diese Funktion gibt die angeforderte Eigenschaft einer angegebenen Sortierung zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
COLLATIONPROPERTY( collation_name , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*collation_name*  
Der Name der Sortierung. Das Argument *collation_name* weist den Datentyp **nvarchar(128)** auf und verfügt über keinen Standardwert.
  
*property*  
Die Eigenschaft der Sortierung. Das Argument *property* weist den Datentyp **varchar(128)** auf und kann einen der folgenden Werte besitzen:
  
|Eigenschaftenname|BESCHREIBUNG|  
|---|---|
|**CodePage**|Nicht-Unicode-Codepage der Sortierung. Dies ist der Zeichensatz, der für **varchar**-Daten verwendet wird. Informationen zum Übersetzen dieser Werte und zu ihren Zeichenzuordnungen finden Sie unter [Appendix G DBCS/Unicode Mapping Tables (Anhang G: Tabellen zur DBCS-/Unicode-Zuordnung)](/previous-versions/cc194886(v=msdn.10)) und [Appendix H Code Pages (Anhang H: Codepages)](/previous-versions/cc195051(v=msdn.10)).<br /><br />Basisdatentyp: **int**|  
|**LCID**|Windows-Gebietsschemabezeichner (Locale Identifier, LCID) der Sortierung. Dies ist die Kultur, die für die Sortierung und die Vergleichsregeln verwendet wird. Informationen zum Übersetzen dieser Werte erhalten Sie unter [LCID Structure (LCID-Struktur)](/openspecs/windows_protocols/ms-lcid/63d3d639-7fd2-4afb-abbe-0d5b5551eef8). Sie müssen jedoch zunächst eine Konvertierung in **varbinary** vornehmen.<br /><br />Basisdatentyp: **int**|  
|**ComparisonStyle**|Die Windows-Vergleichsart der Sortierung. Gibt 0 für binäre Sortierungen zurück. Sowohl (\_BIN) als auch (\_BIN2) werden ebenfalls zurückgegeben, wenn nach allen Eigenschaften unterschieden wird – (\_CS\_AS\_KS\_WS) und (\_CS\_AS\_KS\_WS\_SC) und (\_CS\_AS\_KS\_WS\_VSS). Bitmaskenwerte:<br /><br /> Groß-/Kleinschreibung ignorieren: 1<br /><br /> Akzente ignorieren: 2<br /><br /> Kana ignorieren: 65536<br /><br /> Breite ignorieren: 131072<br /><br /> Hinweis: Die Option \_VSS (Unterscheidung nach Variierungsauswahlzeichen) wird in diesem Wert nicht dargestellt, obwohl sie Auswirkungen auf das Vergleichsverhalten hat.<br /><br />Basisdatentyp: **int**|  
|**Version**|Die Version der Sortierung. Gibt einen Wert zwischen 0 und 3 zurück.<br /><br /> Sortierungen, in deren Namen die Zahl 140 enthalten ist, geben 3 zurück.<br /><br /> Sortierungen, in deren Namen die Zahl 100 enthalten ist, geben 2 zurück.<br /><br /> Sortierungen, in deren Namen die Zahl 90 enthalten ist, geben 1 zurück.<br /><br /> Alle anderen Sortierungen geben 0 zurück.<br /><br />Basisdatentyp: **tinyint**|  
  
## <a name="return-types"></a>Rückgabetypen
**sql_variant**
  
## <a name="examples"></a>Beispiele  
  
```sql
SELECT COLLATIONPROPERTY('Traditional_Spanish_CS_AS_KS_WS', 'CodePage');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1252   
```  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
```sql
SELECT COLLATIONPROPERTY('Traditional_Spanish_CS_AS_KS_WS', 'CodePage')  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1252   
```  
  
## <a name="see-also"></a>Weitere Informationen
[sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
  
