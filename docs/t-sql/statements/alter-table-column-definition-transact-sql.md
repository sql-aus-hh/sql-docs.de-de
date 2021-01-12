---
description: ALTER TABLE column_definition (Transact-SQL)
title: column_definition (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- column_definition
- column_definition_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column_definition
- ALTER TABLE statement
- column properties [SQL Server]
- column definitions [SQL Server]
ms.assetid: a1742649-ca29-4d9b-9975-661cdbf18f78
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3a0724503ed6843972730afc5179c0d6ba9f18a1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100257"
---
# <a name="alter-table-column_definition-transact-sql"></a>ALTER TABLE column_definition (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Gibt die Eigenschaften einer Spalte an, die mithilfe von [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) einer Tabelle hinzugefügt wird.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
column_name <data_type>  
[ FILESTREAM ]  
[ COLLATE collation_name ]   
[ NULL | NOT NULL ]  
[   
    [ CONSTRAINT constraint_name ] DEFAULT constant_expression [ WITH VALUES ]   
    | IDENTITY [ ( seed , increment ) ] [ NOT FOR REPLICATION ]   
]  
[ ROWGUIDCOL ]   
[ SPARSE ]   
[ ENCRYPTED WITH  
  ( COLUMN_ENCRYPTION_KEY = key_name ,  
      ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,   
      ALGORITHM =  'AEAD_AES_256_CBC_HMAC_SHA_256'   
  ) ]  
[ MASKED WITH ( FUNCTION = ' mask_function ') ]  
[ <column_constraint> [ ...n ] ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
[ CONSTRAINT constraint_name ]   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH FILLFACTOR = fillfactor    
          | WITH ( < index_option > [ , ...n ] )   
        ]   
        [ ON { partition_scheme_name ( partition_column_name )   
            | filegroup | "default" } ]  
  | [ FOREIGN KEY ]   
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *column_name*  
 Der Name der Spalte, die geändert, hinzugefügt oder gelöscht werden soll. *column_name* kann zwischen 1 und 128 Zeichen aufweisen. Bei neuen Spalten, die mit einem timestamp-Datentyp erstellt wurden, ist *column_name* nicht erforderlich. Wenn *column_name* nicht für eine Spalte vom Datentyp **timestamp** angegeben ist, wird der Name **timestamp** verwendet.  
  
 [ _type_schema_name_ **.** ] *type_name*  
 Der Datentyp für die hinzugefügte Spalte und das Schema, zu dem er gehört.  
  
 *type_name* kann Folgendes sein:  
  
-   Ein [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp.  
  
-   Ein Aliasdatentyp, der auf einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp basiert. Aliasdatentypen müssen mithilfe von CREATE TYPE erstellt werden, damit sie in einer Tabellendefinition verwendet werden können.  
  
-   Ein benutzerdefinierter [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Datentyp und das Schema, zu dem er gehört. Ein benutzerdefinierter [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Datentyp muss mithilfe von CREATE TYPE erstellt werden, bevor er in einer Tabellendefinition verwendet werden kann.  
  
 Wenn *type_schema_name* nicht angegeben ist, verweist [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf *type_name* in der folgenden Reihenfolge:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp.  
  
-   Das Standardschema des aktuellen Benutzers in der aktuellen Datenbank  
  
-   Das **dbo** -Schema in der aktuellen Datenbank  
  
*precision*  
 Die Genauigkeit für den angegebenen Datentyp. Weitere Informationen über gültige Genauigkeitswerte finden Sie unter [Genauigkeit, Dezimalstellen und Länge &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
*scale*  
 Die Dezimalstellen für den angegebenen Datentyp. Weitere Informationen zu gültigen Dezimalstellenwerten finden Sie unter [Genauigkeit, Dezimalstellen und Länge &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
**max**  
 Gilt nur für die Datentypen **varchar**, **nvarchar** und **varbinary**. Diese Datentypen werden zum Speichern von Zeichen- und Binärdaten mit einer Länge von 2^31 Byte und Unicode-Daten mit einer Länge von 2^30 Byte verwendet.  
  
**CONTENT**  
 Gibt an, dass jede Instanz des **xml**-Datentyps in *column_name* mehrere allgemeine Elemente enthalten kann. CONTENT gilt nur für den **xml**-Datentyp und kann nur angegeben werden, wenn *xml_schema_collection* ebenfalls angegeben ist. Fehlt die Angabe, ist CONTENT das Standardverhalten.  
  
DOCUMENT  
 Gibt an, dass jede Instanz des **xml**-Datentyps in *column_name* nur ein allgemeines Element enthalten kann. DOCUMENT gilt nur für den **xml**-Datentyp und kann nur angegeben werden, wenn *xml_schema_collection* ebenfalls angegeben ist.  
  
 *xml_schema_collection*  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gilt nur für den **xml**-Datentyp zum Verknüpfen einer XML-Schemaauflistung mit diesem Typ. Vor der Typisierung einer **xml**-Spalte mit einem Schema muss das Schema zuerst mithilfe von [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) in der Datenbank erstellt werden.  
  
FILESTREAM  
 Gibt optional das FILESTREAM-Speicherattribut für eine Spalte an, die einen *type_name* mit dem Wert **varbinary(max)** hat.  
  
 Wenn FILESTREAM für eine Spalte angegeben wird, muss die Tabelle auch eine Spalte mit dem Datentyp **uniqueidentifier** aufweisen, der das ROWGUIDCOL-Attribut enthält. Diese Spalte darf keine NULL-Werte zulassen und muss eine UNIQUE- oder eine PRIMARY KEY-Einschränkung für einzelne Spalten enthalten. Der GUID-Wert für die Spalte muss entweder beim Einfügen von Daten von einer Anwendung oder durch eine DEFAULT-Einschränkung mit der NEWID ()-Funktion bereitgestellt werden.  
  
 Die Spalte ROWGUIDCOL kann nicht gelöscht, und die zugehörigen Einschränkungen können nicht geändert werden, wenn für die Tabelle eine FILESTREAM-Spalte definiert ist. Die Spalte ROWGUIDCOL kann nur gelöscht werden, nachdem die letzte FILESTREAM-Spalte gelöscht wurde.  
  
 Wenn das FILESTREAM-Speicherattribut für eine Spalte angegeben wird, werden alle Werte dieser Spalte in einem FILESTREAM-Datencontainer des Dateisystems gespeichert.  
  
 Ein Beispiel, das die Verwendung von Spaltendefinitionen veranschaulicht, finden Sie unter [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
COLLATE *collation_name*  
 Gibt die Sortierung der Spalte an. Wenn keine Sortierung angegeben ist, wird der Spalte die Standardsortierung der Datenbank zugewiesen. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Eine Liste und weitere Informationen finden Sie unter [Name der Windows-Sortierung &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) und [SQL Server-Sortierungsname &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 Mit der COLLATE-Klausel können Sie nur die Sortierungen von Spalten der Datentypen **char**, **varchar**, **nchar** und **nvarchar** angeben.  
  
 Weitere Informationen zur COLLATE-Klausel finden Sie unter [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 NULL | NOT NULL  
 Bestimmt, ob Nullwerte in der Spalte zulässig sind. NULL ist genau genommen keine Einschränkung, kann jedoch wie NOT NULL verwendet werden.  
  
[ CONSTRAINT *constraint_name* ]  
 Gibt den Anfang einer DEFAULT-Wertdefinition an. Um die Kompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufrechtzuerhalten, ist es möglich, einer DEFAULT-Definition einen Einschränkungsnamen zuzuweisen. *contraint_name* muss den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen, wobei der Name außerdem nicht mit einem Nummernzeichen (#) beginnen darf. Wenn *contraint_name* nicht angegeben wird, wird der DEFAULT-Definition ein vom System generierter Name zugewiesen.  
  
DEFAULT  
 Ein Schlüsselwort, das den Standardwert für die Spalte angibt. DEFAULT-Definitionen können verwendet werden, um Werte für eine neue Spalte in den vorhandenen Datenzeilen bereitzustellen. DEFAULT-Definitionen können nicht auf **timestamp**-Spalten oder Spalten mit einer IDENTITY-Eigenschaft angewendet werden. Wenn ein Standardwert für einen benutzerdefinierten Spaltentyp angegeben wird, muss dieser Typ eine implizite Konvertierung von *constant_expression* in den benutzerdefinierten Typ unterstützen.  
  
*constant_expression*  
 Ein Literalwert, ein NULL-Wert oder eine Systemfunktion, der bzw. die als Standardwert für die Spalte verwendet wird. Wenn die Implementierung des Datentyps zusammen mit einer Spalte verwendet wird, die als benutzerdefinierter Datentyp von [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] definiert ist, muss die Implementierung des Datentyps sie eine implizite Konvertierung von *constant_expression* in den benutzerdefinierten Datentyp unterstützen.  
  
WITH VALUES   
 Beim Hinzufügen einer Spalte UND einer DEFAULT-Beschränkung wird, wenn die Spalte NULL-Werte mit WITH VALUES erlaubt, für bestehende Zeilen der Wert der neuen Spalte auf den in DEFAULT *constant_expression* angegebenen Wert gesetzt. Wenn die hinzugefügte Spalte NULL-Werte nicht zulässt, wird der Wert der Spalte immer auf den im DEFAULT *constant expression* angegebenen Wert gesetzt. Ab SQL Server 2012 kann dies auch ein Metadatenvorgang sein [Hinzufügen von NOT NULL-Spalten als Onlinevorgang](alter-table-transact-sql.md#adding-not-null-columns-as-an-online-operation).
Wenn dieser verwendet wird, hat dies keine Auswirkung wenn die verknüpfte Spalte ist nicht ebenfalls hinzugefügt wird.
 
 Gibt an, dass der in DEFAULT *constant_expression* angegebene Wert in einer neuen Spalte gespeichert wird, die vorhandenen Zeilen hinzugefügt wird. Wenn die hinzugefügte Spalte NULL-Werte zulässt und WITH VALUES angegeben ist, wird der Standardwert in der neuen, zu vorhandenen Zeilen hinzugefügten Spalte gespeichert. Ist WITH VALUES für Spalten, die NULL-Werte zulassen, nicht angegeben, wird der Wert NULL in der neuen Spalte in vorhandenen Zeilen gespeichert. Wenn die neue Spalte keine NULL-Werte zulässt, wird der Standardwert in neuen Zeilen gespeichert, unabhängig davon, ob WITH VALUES angegeben ist.  
  
IDENTITY  
 Gibt an, dass es sich bei der neuen Spalte um eine Identitätsspalte handelt. Von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] wird ein eindeutiger, inkrementeller Wert für die Spalte bereitgestellt. Wenn Sie vorhandenen Tabellen Bezeichnerspalten hinzufügen, werden die ID-Nummern mit den Ausgangswerten und den inkrementellen Werten den vorhandenen Zeilen der Tabelle hinzugefügt. Die Reihenfolge, in der die Zeilen aktualisiert werden, ist nicht sichergestellt. Auch für alle neu hinzugefügten Zeilen werden ID-Nummern generiert.  
  
 Identitätsspalten werden in der Regel in Verbindung mit PRIMARY KEY-Einschränkungen verwendet, um als eindeutiger Zeilenbezeichner für die Tabelle zu dienen. Die IDENTITY-Eigenschaft kann folgenden Spalten zugewiesen werden: **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** oder **numeric(p,0)** . Es kann nur eine Identitätsspalte pro Tabelle erstellt werden. Das DEFAULT-Schlüsselwort und gebundene Standardwerte können bei einer Identitätsspalte nicht verwendet werden. Entweder müssen sowohl Ausgangswert als auch Inkrement oder keines von beiden angegeben werden. Wenn keine Angabe gemacht wird, ist der Standardwert (1,1).  
  
> [!NOTE]  
>  Es ist nicht möglich, einer vorhandenen Tabellenspalte die IDENTITY-Eigenschaft hinzuzufügen.  
  
 Das Hinzufügen einer Identitätsspalte zu einer veröffentlichten Spalte wird nicht unterstützt, da dies beim Replizieren der Spalte auf den Abonnenten zu einer Nichtkonvergenz führen kann. Die Werte in der Identitätsspalte auf dem Verleger richten sich nach der Ordnung, in der die Zeilen für die betreffende Tabelle physisch gespeichert sind. Die Zeilen sind auf dem Abonnenten möglicherweise anders gespeichert, sodass der Wert für die Identitätsspalte für dieselben Zeilen variieren kann.  
  
 Verwenden Sie [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md), um die IDENTITY-Eigenschaft einer Spalte zu deaktivieren, indem das Einfügen expliziter Werte ermöglicht wird.  
  
*seed*  
 Der Wert, der für die erste in die Tabelle geladene Zeile verwendet wird.  
  
*increment*  
 Der Inkrementwert, der zum Identitätswert der zuvor geladenen Zeile addiert wird.  
  
NOT FOR REPLICATION  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Kann für die IDENTITY-Eigenschaft angegeben werden. Wenn diese Klausel für die IDENTITY-Eigenschaft angegeben wird, werden Werte in Identitätsspalten nicht inkrementiert, wenn Replikations-Agents Einfügevorgänge ausführen.  
  
ROWGUIDCOL  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt an, dass die Spalte eine Spalte mit für alle Zeilen global eindeutigen Bezeichnern ist. ROWGUIDCOL kann nur einer **uniqueidentifier**-Spalte zugewiesen werden, und nur eine **uniqueidentifier**-Spalte pro Tabelle kann als ROWGUIDCOL-Spalte gekennzeichnet werden. ROWGUIDCOL kann keinen Spalten des benutzerdefinierten Datentyps zugewiesen werden.  
  
 ROWGUIDCOL erzwingt keine Eindeutigkeit der in der Spalte gespeicherten Werte. ROWGUIDCOL generiert auch nicht automatisch Werte für neue Zeilen, die in die Tabelle eingefügt werden. Verwenden Sie entweder die NEWID-Funktion in INSERT-Anweisungen, oder geben Sie die NEWID-Funktion als Standard für die Spalte an, um eindeutige Werte für jede Spalte zu generieren. Weitere Informationen finden Sie unter [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md) und [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
SPARSE  
 Gibt an, dass die Spalte eine Sparsespalte ist. Der Speicher für Sparsespalten ist für NULL-Werte optimiert. Spalten mit geringer Dichte können nicht als NOT NULL festgelegt werden. Weitere Einschränkungen und Informationen zu Sparsespalten finden Sie unter [Verwenden von Sparsespalten](../../relational-databases/tables/use-sparse-columns.md).  
  
\<column_constraint>  
 Die Definitionen der Spalteneinschränkungsargumente finden Sie unter [column_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-constraint-transact-sql.md).  
  
 ENCRYPTED WITH  
 Gibt Verschlüsselungsspalten mit dem Feature [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) an.  
  
 COLUMN_ENCRYPTION_KEY = *key_name*  
 Gibt Spaltenverschlüsselungsschlüssel an. Weitere Informationen finden Sie unter [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md).  
  
ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED }  
 Die **deterministische Verschlüsselung** verwendet eine Methode, die immer denselben verschlüsselten Wert für jeden angegebenen Klartextwert generiert. Die Verwendung der deterministischen Verschlüsselung ermöglicht die Suche mit einer Gleichheitsüberprüfung, das Gruppieren und das Verknüpfen von Tabellen mit Gleichheitsjoins, basierend auf verschlüsselten Werten. Jedoch erlaubt sie nicht autorisierten Benutzern möglicherweise, Informationen zu verschlüsselten Werten zu erraten, indem sie die Muster in den verschlüsselten Spalten untersuchen. Das Verknüpfen zweier Tabellen mit deterministisch verschlüsselten Spalten ist nur möglich, wenn die Spalten mit demselben Spaltenverschlüsselungsschlüssel verschlüsselt sind. Die deterministische Verschlüsselung muss eine Spaltensortierung mit einer binary2-Sortierreihenfolge für Zeichenspalten verwenden.  
  
 Die **zufällige Verschlüsselung** verwendet eine Methode, die Daten in einer weniger vorhersagbaren Weise verschlüsselt. Die Verschlüsselung nach dem Zufallsprinzip ist zwar sicherer, verhindert jedoch jegliche Berechnungen und Indizierungen in verschlüsselten Spalten, es sei denn, Ihre SQL Server-Instanz unterstützt [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).
  
 Wenn Sie Always Encrypted (ohne Secure Enclaves) verwenden, verwenden Sie die deterministische Verschlüsselung für Spalten, die mit Parametern oder Gruppierungsparametern durchsucht werden sollen (z.B. eine Behörden-ID-Nummer). Verwenden Sie die zufällige Datenverschlüsselung für Daten (z.B. Kreditkartennummern), die nicht mit anderen Datensätzen gruppiert oder in Jointabellen verwendet werden und nach denen nicht gesucht wird, wenn andere Spalten verwendet werden (z.B. Transaktionsnummern), um die Zeile zu suchen, die die betreffende verschlüsselte Spalte enthält.  

 Wenn Sie Always Encrypted mit Secure Enclaves verwenden, wird eine Verschlüsselung nach dem Zufallsprinzip empfohlen.  
  
 Spalten müssen einen qualifizierenden Datentyp aufweisen.  
  
ALGORITHM  
**Gilt für:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] und höher, [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
Muss **'AEAD_AES_256_CBC_HMAC_SHA_256'** sein.  
  
 Weitere Informationen einschließlich Featureeinschränkungen finden Sie unter [Always Encrypted &#40;Datenbank-Engine&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md).  
  
   
ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
 **Gilt für:** [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] und höher, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Gibt eine dynamische Datenmaske an. *mask_function* ist der Name der Maskierungsfunktion mit den entsprechenden Parametern. Die folgenden Funktionen stehen zur Verfügung:  
  
-   default()  
  
-   email()  
  
-   partial()  
  
-   random()  
  
 Weitere Informationen zu Funktionsparametern finden Sie im Artikel zur [dynamischen Datenmaskierung](../../relational-databases/security/dynamic-data-masking.md).  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn eine Spalte hinzugefügt wird, die über einen **uniqueidentifier**-Datentyp verfügt, kann sie mit einem Standardwert definiert werden, der die NEWID()-Funktion verwendet, um die eindeutigen Bezeichnerwerte in der neuen Spalte für jede vorhandene Zeile in der Tabelle anzugeben.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] erzwingt keine Reihenfolge für die Angabe von DEFAULT, IDENTITY, ROWGUIDCOL oder Spalteneinschränkungen in einer Spaltendefinition.  
  
 Die ALTER TABLE-Anweisung ergibt einen Fehler, wenn die Größe der Datenzeile durch Hinzufügen der Spalte 8060 Bytes überschreitet.  
  
## <a name="examples"></a>Beispiele  
 Beispiele finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
