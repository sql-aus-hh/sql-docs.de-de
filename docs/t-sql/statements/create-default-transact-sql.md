---
description: CREATE DEFAULT (Transact-SQL)
title: CREATE DEFAULT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_DEFAULT_TSQL
- CREATE DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], default
- default objects
- CREATE DEFAULT statement
- objects [SQL Server], creating
- DEFAULT definition
ms.assetid: 08475db4-7d90-486a-814c-01a99d783d41
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7dd06b9daff476b26667c0ef5900f34325b05f7c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100887"
---
# <a name="create-default-transact-sql"></a>CREATE DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Erstellt ein Objekt, das als Standardwert bezeichnet wird. Wenn ein Standardwert an eine Spalte oder einen Aliasdatentyp gebunden ist, gibt er den Wert an, der in diese Spalte (oder im Fall eines Aliasdatentyps in alle Spalten) eingefügt werden soll, wenn beim Einfügen nicht explizit ein Wert angegeben ist.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen Standarddefinitionen, die mithilfe des DEFAULT-Schlüsselworts von ALTER TABLE oder CREATE TABLE erstellt werden.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
CREATE DEFAULT [ schema_name . ] default_name   
AS constant_expression [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*schema_name*  
 Der Name des Schemas, zu dem der Standardwert gehört.  
  
*default_name*  
 Der Name des Standardwerts. Namen für Standardwerte müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen. Das Angeben des Standardbesitzernamens ist optional.  
  
*constant_expression*  
Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md), der nur konstante Werte enthält (nicht zulässig sind Namen von Spalten oder anderen Datenbankobjekten). Sie können jede Konstante, jede integrierte Funktion oder jeden mathematischen Ausdruck verwenden, außer solchen, die Aliasdatentypen enthalten. Benutzerdefinierte Funktionen können nicht verwendet werden. Setzen Sie Zeichen- und Datumskonstanten in einfache Anführungszeichen (**'**). Bei Integer-, Währungs- und Gleitkommakonstanten sind keine Anführungszeichen erforderlich. Binären Daten muss 0x vorangestellt werden, und Währungsdaten muss das Dollarzeichen ($) vorangestellt werden. Der Standardwert muss mit dem Datentyp der Spalte kompatibel sein.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können den Namen eines Standardwerts nur in der aktuellen Datenbank erstellen. Innerhalb einer Datenbank müssen die Namen für Standardwerte für jedes Schema eindeutig sein. Wenn Sie einen Standardwert erstellen, binden Sie ihn mit **sp_bindefault** an eine Spalte oder einen Aliasdatentyp.  
  
 Falls der Standardwert mit der Spalte, an die er gebunden ist, nicht kompatibel ist, erzeugt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Versuch, den Standardwert einzufügen, eine Fehlermeldung. N/V kann z.B. nicht als Standardwert für **numeric**-Spalten verwendet werden.  
  
 Falls der Standardwert zu lang für die Spalte ist, an die er gebunden ist, wird er gekürzt.  
  
 CREATE DEFAULT-Anweisungen können nicht mit anderen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen in einem einzelnen Batch kombiniert werden.  
  
 Ein Standardwert muss gelöscht werden, bevor Sie einen neuen mit gleichem Namen erstellen. Bevor der Standardwert gelöscht wird, muss seine Bindung durch Ausführen von **sp_unbindefault** aufgehoben werden.  
  
 Falls einer Spalte sowohl ein Standardwert als auch eine Regel zugeordnet ist, darf der Standardwert nicht diese Regel verletzen. Ein Standardwert, der gegen eine Regel verstößt, wird nicht eingefügt. Bei jedem Versuch, einen solchen Standardwert einzufügen, erzeugt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Fehlermeldung.  
  
 Ist ein Standardwert an eine Spalte gebunden, wird er unter folgenden Umständen eingefügt:  
  
-   Es wird kein Wert explizit eingefügt.  
  
-   Das DEFAULT VALUES- oder DEFAULT-Schlüsselwort wird mit INSERT verwendet, um Standardwerte einzufügen.  
  
 Falls Sie beim Erstellen einer Spalte NOT NULL angeben und keinen Standardwert für sie erstellen, wird jedes Mal eine Fehlermeldung erzeugt, wenn ein Benutzer keinen Eintrag in dieser Spalte vornimmt. In der folgenden Tabelle wird die Beziehung zwischen dem Vorhandensein eines Standardwerts und der Definition einer Spalte als NULL oder NOT NULL verdeutlicht. Das jeweilige Ergebnis geht aus den Einträgen in der Tabelle hervor.  
  
|Spaltendefinition|Kein Eintrag, kein Standardwert|Kein Eintrag, Standardwert|Eingabe von NULL, kein Standardwert|Eingabe von NULL, Standardwert|  
|-----------------------|--------------------------|-----------------------|----------------------------|-------------------------|  
|**NULL**|NULL|default|NULL|NULL|  
|**NOT NULL**|Fehler|default|error|error|  
  
 Um einen Standardwert umzubenennen, verwenden Sie **sp_rename**. Um einen Bericht über einen Standardwert zu erhalten, verwenden Sie **sp_help**.  
  
## <a name="permissions"></a>Berechtigungen  
 Zur Verwendung von CREATE DEFAULT benötigt ein Benutzer zumindest die CREATE DEFAULT-Berechtigung für die aktuelle Datenbank sowie die ALTER-Berechtigung für das Schema, in dem der Standardwert erstellt wird.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-simple-character-default"></a>A. Erstellen eines einfachen Zeichenstandards  
 Im folgenden Beispiel wird ein Zeichenstandardwert namens `unknown` erstellt.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DEFAULT phonedflt AS 'unknown';  
```  
  
### <a name="b-binding-a-default"></a>B. Binden eines Standards  
 Im folgenden Beispiel wird der in Beispiel A erstellte Standardwert an eine Spalte gebunden. Der Standardwert tritt nur dann in Kraft, wenn kein Eintrag in der `Phone`-Spalte der `Contact`-Tabelle angegeben ist. 
 
 > [!Note] 
 >  Das Auslassen eines Eintrags unterscheidet sich vom expliziten Angeben von NULL in einer INSERT-Anweisung.  
  
 Da kein Standardwert namens `phonedflt` vorhanden ist, tritt bei der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ein Fehler auf. Dieses Beispiel dient nur zur Veranschaulichung.  
  
```sql  
USE AdventureWorks2012;  
GO  
sp_bindefault 'phonedflt', 'Person.PersonPhone.PhoneNumber';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
