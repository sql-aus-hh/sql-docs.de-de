---
description: sp_server_info (Transact-SQL)
title: sp_server_info (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_server_info
- sp_server_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_server_info
ms.assetid: 2dc2c262-3cfa-4a84-8127-3632ba583543
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b8460daf65826290942f28fe65af6e5fa9f9b416
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765773"
---
# <a name="sp_server_info-transact-sql"></a>sp_server_info (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine Liste von Attributnamen und entsprechenden Werten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], das Datenbankgateway oder die zugrunde liegende Datenquelle zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_server_info [[@attribute_id = ] 'attribute_id']  
```  
  
## <a name="arguments"></a>Argumente  
`[ @attribute_id = ] 'attribute_id'` Die ganzzahlige ID des Attributs. *attribute_id* ist vom Datentyp **int** und hat den Standardwert NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 Keine  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**ATTRIBUTE_ID**|**int**|ID-Nummer des Attributs.|  
|**ATTRIBUTE_NAME**|**varchar (** 60 **)**|Der Attributname.|  
|**ATTRIBUTE_VALUE**|**varchar (** 255 **)**|Aktuelle Einstellung des Attributs.|  
  
 Die Attribute sind in der folgenden Tabelle aufgeführt. [!INCLUDE[msCoName](../../includes/msconame-md.md)] ODBC-Client Bibliotheken verwenden derzeit die Attribute **1**, **2**, **18**, **22** und **500** zur Verbindungszeit.  
  
|ATTRIBUTE_ID|Beschreibung in ATTRIBUTE_NAME|ATTRIBUTE_VALUE|  
|-------------------|---------------------------------|----------------------|  
|**1**|DBMS_NAME|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**2**|DBMS_VER|Microsoft [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] *xxxx*  -  *x. xx. xxxx*<br/><br> Zum Beispiel, `Microsoft SQL Server 2017 - 14.0.3257.3`|  
|**10**|OWNER_TERM|owner|  
|**11**|TABLE_TERM|table|  
|**12**|MAX_OWNER_NAME_LENGTH|128|  
|**13**|TABLE_LENGTH<br /><br /> Gibt die maximale Anzahl der Zeichen für einen Tabellennamen an.|128|  
|**14**|MAX_QUAL_LENGTH<br /><br /> Gibt die maximale Länge des Namens für einen Tabellenqualifizierer an (der erste Teil eines dreiteiligen Tabellennamens).|128|  
|**15**|COLUMN_LENGTH<br /><br /> Gibt die maximale Anzahl der Zeichen für einen Spaltennamen an.|128|  
|**16**|IDENTIFIER_CASE<br /><br /> Gibt die benutzerdefinierten Namen (die Namen von Tabellen, Spalten, gespeicherten Prozeduren) in der Datenbank an (Groß- und Kleinschreibung der Objekte in den Systemkatalogen).|SENSITIVE|  
|**17**|TX_ISOLATION<br /><br /> Gibt die Ausgangsisolationsstufe des Servers für Transaktionen an, die einer in SQL-92 definierten Isolationsstufe entspricht.|2|  
|**Jahren**|COLLATION_SEQ<br /><br /> Gibt die Sortierung des Zeichensatzes für diesen Server an.|charset=iso_1 sort_order=dictionary_iso charset_num=1 sort_order_num=51|  
|**19**|SAVEPOINT_SUPPORT<br /><br /> Gibt an, ob das zugrunde liegende DBMS benannte Sicherungspunkte unterstützt.|J|  
|**20**|MULTI_RESULT_SETS<br /><br /> Gibt an, ob die zugrunde liegende Datenbank oder das Gateway selbst mehrere Resultsets unterstützt (mehrere Anweisungen können über das Gateway gesendet werden, wobei mehrere Resultsets an den Client zurückgegeben werden).|J|  
|**22**|ACCESSIBLE_TABLES<br /><br /> Gibt an, ob das Gateway in **sp_tables** nur Tabellen, Sichten usw. zurückgibt, auf die der aktuelle Benutzer zugreifen kann (d. h. der Benutzer, der mindestens über SELECT-Berechtigungen für die Tabelle verfügt).|J|  
|**100**|USERID_LENGTH<br /><br /> Gibt die maximal zulässige Anzahl der Zeichen für einen Benutzernamen an.|128|  
|**101**|QUALIFIER_TERM<br /><br /> Gibt den DBMS-Herstellerausdruck für einen Tabellenqualifizierer an (der erste Teil eines dreiteiligen Tabellennamens).|database|  
|**102**|NAMED_TRANSACTIONS<br /><br /> Gibt an, ob das zugrunde liegende DBMS benannte Transaktionen unterstützt.|J|  
|**103**|SPROC_AS_LANGUAGE<br /><br /> Gibt an, ob gespeicherte Prozeduren als Sprachereignisse ausgeführt werden können.|J|  
|**104**|ACCESSIBLE_SPROC<br /><br /> Gibt an, ob das Gateway in **sp_stored_procedures** nur gespeicherte Prozeduren zurückgibt, die vom aktuellen Benutzer ausführbare Dateien sind.|J|  
|**105**|MAX_INDEX_COLS<br /><br /> Gibt die maximal zulässige Anzahl der Spalten eines Index für das DBMS an.|16|  
|**106**|RENAME_TABLE<br /><br /> Gibt an, ob Tabellen umbenannt werden können.|J|  
|**107**|RENAME_COLUMN<br /><br /> Gibt an, ob Spalten umbenannt werden können.|J|  
|**108**|DROP_COLUMN<br /><br /> Gibt an, ob Spalten gelöscht werden können.|J|  
|**109**|INCREASE_COLUMN_LENGTH<br /><br /> Gibt an, ob Spalten vergrößert werden können.|J|  
|**110**|DDL_IN_TRANSACTION<br /><br /> Gibt an, ob DDL-Anweisungen in Transaktionen zulässig sind.|J|  
|**111**|DESCENDING_INDEXES<br /><br /> Gibt an, ob absteigende Indizes unterstützt werden.|J|  
|**112**|SP_RENAME<br /><br /> Gibt an, ob gespeicherte Prozeduren umbenannt werden können.|J|  
|**113**|REMOTE_SPROC<br /><br /> Gibt an, ob gespeicherte Prozeduren über die remote gespeicherten Prozedurfunktionen in DB-Library ausgeführt werden können.|J|  
|**500**|SYS_SPROC_VERSION<br /><br /> Gibt die Katalogversion der derzeit implementierten gespeicherten Prozeduren an.|Aktuelle Versionsnummer|  
  
## <a name="remarks"></a>Hinweise  
 **sp_server_info** gibt eine Teilmenge der Informationen zurück, die von **SQLGetInfo** in ODBC bereitgestellt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert SELECT-Berechtigung für das Schema.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Katalog Prozeduren &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
