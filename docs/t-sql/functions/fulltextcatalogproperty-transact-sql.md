---
description: FULLTEXTCATALOGPROPERTY (Transact-SQL)
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
author: cawrites
ms.author: chadam
ms.openlocfilehash: 57e53c26b7f1797daf6a1ccc53f7578eabda734a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087327"
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt Informationen zu Volltextkatalog-Eigenschaften in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] zurück.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
  
> [!NOTE]  
>  Die folgenden Eigenschaften werden in einem künftigen Release von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr unterstützt: **LogSize** und **PopulateStatus**. Vermeiden Sie die Verwendung dieser Eigenschaften in Neuentwicklungen, und planen Sie die Änderung von Anwendungen, die diese Eigenschaften derzeit verwenden.  
  
_catalog\_name_  
Ein Ausdruck, der den Namen des Volltextkatalogs enthält.  
  
_property_  
Ein Ausdruck, der den Namen der Volltext-Katalogeigenschaft enthält. In der folgenden Tabelle finden Sie eine Liste der Eigenschaften und eine Beschreibung der zurückgegebenen Informationen.  
  
|Eigenschaft|BESCHREIBUNG|  
|--------------|-----------------|  
|**AccentSensitivity**|Einstellung für die Unterscheidung nach Akzent.<br /><br /> 0 = Keine Unterscheidung nach Akzent<br /><br /> 1 = Unterscheidung nach Akzent|  
|**IndexSize**|Logische Größe des Volltextkatalogs in Megabyte (MB). Enthält die Größe des semantischen Schlüsselausdrucks und von Dokumentähnlichkeitsindizes.<br /><br /> Weitere Informationen finden Sie unter "Hinweise" weiter unten in diesem Thema.|  
|**ItemCount**|Anzahl der indizierten Elemente einschließlich aller Volltext-, Schlüsselausdrucks- und Dokumentähnlichkeitsindizes in einem Katalog.|  
|**LogSize**|Wird nur aus Gründen der Abwärtskompatibilität unterstützt. Es wird immer 0 zurückgegeben.<br /><br /> Größe (in Bytes) der kombinierten Gruppe von Fehlerprotokollen, die mit einem Volltextkatalog des [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search-Diensts verbunden sind.|  
|**MergeStatus**|Gibt an, ob eine Masterzusammenführung ausgeführt wird.<br /><br /> 0 = Masterzusammenführung wird nicht ausgeführt<br /><br /> 1 = Masterzusammenführung wird ausgeführt|  
|**PopulateCompletionAge**|Anzahl von Sekunden, die zwischen dem 01.01.1990, 00:00:00 Uhr, und der Beendigung des letzten Auffüllens des Volltextindex verstrichen sind.<br /><br /> Wird nur für vollständige und inkrementelle Durchforstungsvorgänge aktualisiert. Gibt 0 zurück, wenn keine Auffüllung aufgetreten ist.|  
|**PopulateStatus**|0 = Im Leerlauf<br /><br /> 1 = Vollständiges Auffüllen wird ausgeführt<br /><br /> 2 = Angehalten<br /><br /> 3 = Gedrosselt<br /><br /> 4 = Wird wiederhergestellt<br /><br /> 5 = Herunterfahren<br /><br /> 6 = Inkrementelles Auffüllen wird ausgeführt<br /><br /> 7 = Index wird erstellt<br /><br /> 8 = Der Datenträger ist voll. Angehalten.<br /><br /> 9 = Änderungsprotokollierung|  
|**UniqueKeyCount**|Anzahl der eindeutigen Schlüssel im Volltextkatalog.|  
|**ImportStatus**|Gibt an, ob der Volltextkatalog importiert wird.<br /><br /> 0 = Der Volltextkatalog wird nicht importiert.<br /><br /> 1 = Der Volltextkatalog wird importiert.|  
  
## <a name="return-types"></a>Rückgabetypen  
**int**  
  
## <a name="exceptions"></a>Ausnahmen  
Gibt NULL zurück, wenn ein Fehler auftritt oder ein Aufrufer nicht über die Berechtigungen zum Anzeigen des Objekts verfügt.  
  
In [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] kann ein Benutzer nur die Metadaten von sicherungsfähigen Elementen anzeigen. Diese sicherungsfähigen Elemente sind solche Elemente, die der Benutzer besitzt oder für die ihm Berechtigungen erteilt wurden. Daher können integrierte Funktionen, die Metadaten ausgeben (z.B. FULLTEXTCATALOGPROPERTY) NULL zurückgeben, wenn dem Benutzer für das Objekt keine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md).  
  
## <a name="remarks"></a>Bemerkungen  
FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') prüft, wie in [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md) dargestellt, nur Fragmente mit dem Status 4 oder 6. Diese Fragmente sind ein Teil des logischen Index. Daher gibt die **IndexSize**-Eigenschaft nur die logische Indexgröße zurück. 

Während eines Indexmerge könnte die tatsächliche Indexgröße jedoch doppelt so groß sein wie die logische Größe. Um die tatsächliche Größe zu ermitteln, die von einem Volltextindex während eines Merge beansprucht wird, verwenden Sie die gespeicherte Systemprozedur [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md). Diese Prozedur prüft alle Fragmente, die einem Volltextindex zugeordnet sind. 

Die Volltextauffüllung schlägt möglicherweise fehl, wenn Sie das Wachstum der Volltextkatalogdatei einschränken und nicht genügend Speicherplatz für den Mergeprozess zulassen. In diesem Fall gibt FULLTEXTCATALOGPROPERTY ('_catalog\_name_', '**IndexSize**') 0 zurück, und der folgende Fehler wird in das Volltextprotokoll geschrieben:  
  
`Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
Es ist wichtig, dass Anwendungen nicht in einer Schleife warten und die **PopulateStatus**-Eigenschaft auf Leerlauf überprüfen. Leerlauf bedeutet hier, dass das Auffüllen beendet wurde. Dadurch werden CPU-Zyklen von der Datenbank und von Volltextsuchprozessen abgezogen und Timeoutfehler verursacht. Es ist in der Regel besser, die entsprechende **PopulateStatus**-Eigenschaft auf Tabellenebene und **TableFullTextPopulateStatus** in der OBJECTPROPERTYEX-Systemfunktion zu überprüfen. Diese und andere neue Volltexteigenschaften in OBJECTPROPERTYEX stellen Informationen mit höherer Granularität zur Volltextindizierung von Tabellen bereit. Weitere Informationen finden Sie unter [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel wird die Anzahl der volltextindizierten Elemente in einem Volltextkatalog mit dem Namen `Cat_Desc` zurückgegeben.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
[FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
[Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/metadata-functions-transact-sql.md)   
[sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
