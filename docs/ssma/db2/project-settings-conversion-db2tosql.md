---
description: Projekteinstellungen (Konvertierung) (DB2ToSQL)
title: Projekteinstellungen (Konvertierung) (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 538c93cf-c5bb-43d5-b758-186d9fb00c19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f39965e87911f7fef8d50c0b0888d552a451a90
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172652"
---
# <a name="project-settings-conversion-db2tosql"></a>Projekteinstellungen (Konvertierung) (DB2ToSQL)
Die Seite Konvertierung des Dialog Felds **Projekteinstellungen** enthält Einstellungen, die anpassen, wie SSMA die DB2-Syntax in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax konvertiert.  
  
Der Bereich Konvertierung ist in den Dialogfeldern **Projekteinstellungen** und **Standard Projekteinstellungen** verfügbar:  
  
-   Wenn Sie Einstellungen für alle SSMA-Projekte angeben möchten, klicken Sie **im Menü Extras** auf **Standard Projekteinstellungen**, wählen Sie den Migrations Projekttyp aus, für den die Einstellungen in der Dropdown Liste **Migrations Ziel Version** angezeigt oder geändert werden müssen, klicken Sie dann unten im linken Bereich auf **Allgemein** , und klicken Sie dann auf **Konvertierung**.  
  
-   Um Einstellungen für das aktuelle Projekt anzugeben, klicken Sie **im Menü Extras** auf **Projekteinstellungen**, klicken Sie dann unten im linken Bereich auf **Allgemein** , und klicken Sie dann auf **Konvertierung**.  
  
## <a name="conversion-messages"></a>Konvertierungs Meldungen  
  
### <a name="generate-messages-about-issues-applied"></a>Meldungen zu den angewendeten Problemen generieren  
Gibt an, ob SSMA während der Konvertierung Informationsmeldungen generiert, diese im Ausgabebereich anzeigt und Sie dem konvertierten Code hinzufügt.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Nein  
  
**Vollständiger Modus:** Nein  
  
## <a name="miscellaneous-options"></a>Sonstige Optionen  
  
### <a name="cast-rownum-expressions-as-integers"></a>Umwandeln von rowNum-Ausdrücken als ganze Zahlen  
Wenn SSMA rowNum-Ausdrücke konvertiert, wird der Ausdruck in eine Top-Klausel konvertiert, gefolgt vom Ausdruck. Das folgende Beispiel zeigt rowNum in einer DB2 DELETE-Anweisung:  
  
`DELETE FROM Table1`  
  
`WHERE ROWNUM < expression and Field1 >= 2`  
  
Das folgende Beispiel zeigt das Ergebnis [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
`DELETE TOP (expression-1)`  
  
`FROM Table1`  
  
`WHERE Field1>=2`  
  
Der obere Wert erfordert, dass der Ausdruck für die Top-Klauseln eine ganze Zahl ergibt. Wenn die Ganzzahl negativ ist, erzeugt die Anweisung einen Fehler.  
  
-   Wenn Sie " **Ja**" auswählen, wandelt SSMA den Ausdruck in eine ganze Zahl um.  
  
-   Wenn Sie **Nein** auswählen, markiert SSMA alle nicht ganzzahligen Ausdrücke als Fehler im konvertierten Code.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standard-/Vollmodus:** Nein  
  
**Optimistischer Modus:** Zwar  
  
### <a name="default-schema-mapping"></a>Standard Schema Zuordnung  
Diese Einstellung gibt an, wie DB2-Schemas SQL Server Schemas zugeordnet werden. In dieser Einstellung sind zwei Optionen verfügbar:  
  
1.  **Schema in Datenbank:** In diesem Modus wird das DB2-Schema ' Sch1 ' standardmäßig dem dbo-SQL Server Schema in SQL Server Datenbank ' Sch1 ' zugeordnet.  
  
2.  **Schema zu Schema:** In diesem Modus wird das DB2-Schema "Sch1" standardmäßig in "Sch1" SQL Server Schema in der standardmäßigen SQL Server Datenbank, die im Verbindungs Dialogfeld bereitgestellt wird, zugeordnet.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Schema in Datenbank  
  
### <a name="conversion-ways-of-merge-statement"></a>Konvertierungs Methoden der MERGE-Anweisung  
  
-   Wenn Sie **mithilfe der INSERT-, Update-, DELETE-Anweisung** auswählen, konvertiert SSMA die Fusion-Anweisung in INSERT-, Update-und DELETE-Anweisungen.  
  
-   Wenn Sie **mithilfe der MERGE-Anweisung** auswählen, konvertiert SSMA die Fusion-Anweisung in in eine MERGE-Anweisung [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!WARNING]  
> Diese Projekt Einstellungs Option ist nur in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014 verfügbar.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Verwenden der MERGE-Anweisung  
  
### <a name="convert-calls-to-subprograms-that-use-default-arguments"></a>Konvertieren von Aufrufen in Unterprogramme, die Standardargumente verwenden  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Funktionen unterstützen das Weglassen von Parametern im Funktions aufzurufen nicht. Außerdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützen Funktionen und Prozeduren keine Ausdrücke als Standardparameter Werte.  
  
-   Wenn Sie " **Ja** " und einen Funktions aufzurufenden Parameter auswählen, fügt SSMA das Schlüsselwort **default** in die Funktion ein und ruft an der korrekten Position auf. Anschließend wird der-Befehl mit einer Warnung markiert.  
  
-   Wenn Sie **Nein** auswählen, werden die Funktionsaufrufe von SSMA als Fehler markiert.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-count-function-to-count_big"></a>Count-Funktion in COUNT_BIG konvertieren  
Wenn die count-Funktionen wahrscheinlich Werte größer als 2.147.483.647 zurückgeben (2<sup>31</sup>-1), sollten Sie die Funktionen in COUNT_BIG konvertieren.  
  
-   Wenn Sie " **Ja**" auswählen, konvertiert SSMA alle Verwendungsmöglichkeiten von "count" in "COUNT_BIG".  
  
-   Wenn Sie **Nein** auswählen, werden die Funktionen als Anzahl beibehalten. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gibt einen Fehler zurück, wenn die Funktion einen Wert zurückgibt, der größer als 2<sup>31</sup>-1 ist.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standard-/Vollmodus:** Zwar  
  
**Optimistischer Modus:** Nein  
  
### <a name="convert-forall-statement-to-while-statement"></a>Anweisung "ForAll" in While-Anweisung konvertieren  
Definiert, wie SSMA ForAll-Schleifen für PL/SQL-Auflistungs Elemente behandelt.  
  
-   Wenn Sie **Ja** auswählen, erstellt SSMA eine while-Schleife, in der Sammlungs Elemente nacheinander abgerufen werden.  
  
-   Wenn Sie **Nein** auswählen, generiert SSMA mithilfe der Nodes ()-Methode ein Rowset aus der Auflistung und verwendet es als einzelne Tabelle. Dies ist effizienter, macht den Ausgabe Code jedoch weniger lesbar.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Nein  
  
**Vollständiger Modus:** Zwar  
  
### <a name="convert-foreign-keys-with-set-null-referential-action-on-column-that-is-not-null"></a>Konvertieren von Fremdschlüsseln mit referenziellen Set NULL-Aktion für Spalte, die nicht NULL ist  
DB2 ermöglicht das Erstellen von Foreign Key-Einschränkungen, bei denen eine SET NULL-Aktion nicht ausgeführt werden konnte, da NULL-Werte in der referenzierten Spalte nicht zulässig sind. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine solche Fremdschlüssel Konfiguration ist nicht zulässig.  
  
-   Wenn Sie **Ja** auswählen, werden referenzielle Aktionen von SSMA wie in DB2 generiert. Sie müssen jedoch manuelle Änderungen vornehmen, bevor Sie die Einschränkung in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können z. b. keine Aktion anstelle von NULL festlegen auswählen.  
  
-   Wenn Sie **Nein** auswählen, wird die Einschränkung als Fehler markiert.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Nein  
  
### <a name="convert-function-calls-to-procedure-calls"></a>Konvertieren von Funktionsaufrufen in Prozedur Aufrufe  
Einige DB2-Funktionen sind als autonome Transaktionen definiert oder enthalten Anweisungen, die in nicht gültig sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In diesen Fällen erstellt SSMA eine Prozedur und eine Funktion, bei der es sich um einen Wrapper für die Prozedur handelt. Die konvertierte Funktion Ruft die implementierende Prozedur auf.  
  
SSMA kann Aufrufe der Wrapper Funktion in Aufrufe der Prozedur konvertieren. Dadurch wird mehr lesbarer Code erstellt, und die Leistung kann verbessert werden. Der Kontext lässt ihn jedoch nicht immer zu. Beispielsweise ist es nicht möglich, einen Funktions aufrufin der Auswahlliste durch einen Prozedur Befehl zu ersetzen. SSMA bietet einige Optionen, um die gängigen Fälle abzudecken:  
  
-   Wenn Sie " **immer**" auswählen, versucht SSMA, Wrapper Funktionsaufrufe in Prozedur Aufrufe zu konvertieren. Wenn der aktuelle Kontext diese Konvertierung nicht zulässt, wird eine Fehlermeldung erzeugt. Auf diese Weise werden keine Funktionsaufrufe im generierten Code verbleiben.  
  
-   Wenn Sie nach **Möglichkeit** auswählen, wird von SSMA nur dann ein Wechsel zu Prozedur aufrufen durchführt, wenn die Funktion über Ausgabeparameter verfügt. Wenn das Verschieben nicht möglich ist, wird das Ausgabe Attribut des Parameters entfernt. In allen anderen Fällen verlässt SSMA Funktionsaufrufe.  
  
-   Wenn Sie **Never** auswählen, werden alle Funktionsaufrufe von SSMA als Funktionsaufrufe belassen. Manchmal ist diese Auswahl aufgrund von Leistungsgründen möglicherweise nicht akzeptabel.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Wenn möglich  
  
### <a name="convert-lock-table-statements"></a>Konvertieren von LOCK TABLE-Anweisungen  
SSMA kann viele LOCK TABLE-Anweisungen in Tabellen Hinweise konvertieren. SSMA kann keine LOCK TABLE-Anweisungen konvertieren, die Partition, SUBPARTITION, @dblink und nowait-Klauseln enthalten, und kennzeichnet solche Anweisungen mit Konvertierungs Fehlermeldungen.  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA unterstützte LOCK TABLE-Anweisungen in Tabellen Hinweise.  
  
-   Wenn Sie **Nein** auswählen, markiert SSMA alle LOCK TABLE-Anweisungen mit Konvertierungs Fehlermeldungen.  
  
In der folgenden Tabelle wird gezeigt, wie SSMA die DB2-Sperr Modi konvertiert:  
  
|DB2-Sperrmodus|SQL Server-Tabellen Hinweis|  
|-|-|  
|Zeilen Freigabe|ROWLOCK, HOLDLOCK|  
|Zeilen exklusiv|ROWLOCK, xlock, HOLDLOCK|  
|Freigabe Update = Zeilen Freigabe|ROWLOCK, HOLDLOCK|  
|FREIGEBEN|TABLOCK, HOLDLOCK|  
|Freigabe von Zeilen exklusiv|TABLOCK, xlock, HOLDLOCK|  
|Ausschließliche|TABLOCKX, HOLDLOCK|  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-open-for-statements-for-ref-cursor-out-parameters"></a>Konvertieren von Open for-Anweisungen für Ref Cursor out-Parameter  
In DB2 kann die Open-for-Anweisung verwendet werden, um ein Resultset an den out-Parameter eines subprogramms vom Typ Ref Cursor zurückzugeben. In werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeicherte Prozeduren direkt die Ergebnisse von SELECT-Anweisungen zurückgeben.  
  
SSMA kann viele Open-for-Anweisungen in SELECT-Anweisungen konvertieren.  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA die Open-for-Anweisung in eine SELECT-Anweisung, die das Resultset an den Client zurückgibt.  
  
-   Wenn Sie **Nein** auswählen, generiert SSMA eine Fehlermeldung im konvertierten Code und im Ausgabebereich.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-record-as-a-list-of-separates-variables"></a>Datensatz als Liste von getrennten Variablen konvertieren  
SSMA kann DB2-Datensätze in trennt Variablen und in XML-Variablen mit spezifischer Struktur konvertieren.  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA den Datensatz nach Möglichkeit in eine Liste mit getrennten Variablen.  
  
-   Wenn Sie **Nein** auswählen, konvertiert SSMA den Datensatz in XML-Variablen mit einer bestimmten Struktur.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-substr-function-calls-to-substring-function-calls"></a>Konvertieren von substr-Funktionsaufrufen in Teil Zeichenfolgen-Funktionsaufrufe  
SSMA kann in Abhängigkeit von der Anzahl von Parametern DB2-substr-Funktionsaufrufe in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Teil Zeichenfolgen** -Funktionsaufrufe konvertieren. Wenn SSMA einen substr-Funktions Aufrufwert nicht konvertieren kann oder die Anzahl von Parametern nicht unterstützt wird, konvertiert SSMA den substr-Funktions aufrufin einen benutzerdefinierten SSMA-Funktions Aufruf.  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA substr-Funktionsaufrufe, die drei Parameter verwenden, in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Teil Zeichenfolge**. Andere substr-Funktionen werden konvertiert, um die benutzerdefinierte SSMA-Funktion aufzurufen.  
  
-   Wenn Sie **Nein** auswählen, konvertiert SSMA den substr-Funktions aufrufin einen benutzerdefinierten SSMA-Funktions Aufruf.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Zwar  
  
**Vollständiger Modus:** Nein  
  
### <a name="convert-subtypes"></a>Konvertieren von Untertypen  
SSMA kann PL/SQL-Untertypen auf zwei Arten konvertieren:  
  
-   Wenn Sie " **Ja**" auswählen, wird von SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein benutzerdefinierter Typ aus einem Untertyp erstellt und für jede Variable dieses unter Typs verwendet.  
  
-   Wenn Sie **Nein** auswählen, ersetzt SSMA alle Quell Deklarationen des unter Typs durch den zugrunde liegenden Typ und konvertiert das Ergebnis wie gewohnt. In diesem Fall werden in keine weiteren Typen erstellt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Nein  
  
### <a name="convert-synonyms"></a>Synonyme konvertieren  
Synonyme für die folgenden DB2-Objekte können zu migriert werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Tabellen und Objekttabellen  
  
-   Sichten und Objektsichten  
  
-   Gespeicherte Prozeduren und Funktionen  
  
-   Materialisierte Sichten  
  
Synonyme für die folgenden DB2-Objekte können durch direkte Verweise auf die Objekte ersetzt werden:  
  
-   Sequenzen  
  
-   Pakete  
  
-   Java-Klassen-Schemaobjekte  
  
-   Benutzerdefinierte Objekttypen  
  
Andere Synonyme können nicht migriert werden. SSMA generiert Fehlermeldungen für das Synonym und alle Verweise, die das Synonym verwenden.  
  
-   Wenn Sie " **Ja**" auswählen, erstellt SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entsprechend den vorherigen Listen Synonyme und direkte Objekt Verweise.  
  
-   Wenn Sie **Nein** auswählen, werden direkte Objekt Verweise für alle hier aufgeführten Synonyme von SSMA erstellt.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-to_chardate-format"></a>Konvertieren TO_CHAR (Datum, Format)  
SSMA kann DB2-TO_CHAR (Datum, Format) in Prozeduren aus der sysdb-Datenbank konvertieren.  
  
-   Wenn Sie die **Verwendung TO_CHAR_DATE Funktion** auswählen, konvertiert SSMA die TO_CHAR (Date, Format) in TO_CHAR_DATE Funktion, wobei die englische Sprache für die Konvertierung verwendet wird.  
  
-   Wenn Sie die **Verwendung TO_CHAR_DATE_LS-Funktion (NLS Care)** auswählen, konvertiert SSMA die TO_CHAR (Date, Format) in TO_CHAR_DATE_LS Funktion mithilfe der Sitzungs Sprache für die Konvertierung.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Verwenden der TO_CHAR_DATE-Funktion  
  
**Vollständiger Modus:** Verwenden der TO_CHAR_DATE_LS-Funktion (NLS-Sorgfalt)  
  
### <a name="convert-transaction-processing-statements"></a>Konvertieren von Transaktions Verarbeitungsanweisungen  
SSMA kann DB2-Transaktions Verarbeitungsanweisungen konvertieren:  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA DB2 Transaction Processing-Anweisungen in- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Anweisungen.  
  
-   Wenn Sie **Nein** auswählen, markiert SSMA die Transaktions Verarbeitungsanweisungen als Konvertierungs Fehler.  
  
> [!NOTE]  
> DB2 öffnet Transaktionen implizit. Um dieses Verhalten in zu emulieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , müssen Sie BEGIN TRANSACTION Anweisungen manuell hinzufügen, wenn Ihre Transaktionen gestartet werden sollen. Alternativ können Sie den Befehl SET IMPLICIT_TRANSACTIONS on am Anfang der Sitzung ausführen. SSMA fügt SET IMPLICIT_TRANSACTIONS automatisch hinzu, wenn Unterroutinen in Autonome Transaktionen umgerechnet werden.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="emulate-db2-null-behavior-in-order-by-clauses"></a>Emulieren des DB2-NULL-Verhaltens in order by  
NULL-Werte werden in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und DB2 anders angeordnet:  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind NULL-Werte die niedrigsten Werte in einer geordneten Liste. In einer aufsteigenden Liste werden zuerst NULL-Werte angezeigt.  
  
-   In DB2 sind NULL-Werte die höchsten Werte in einer geordneten Liste. Standardmäßig werden NULL-Werte zuletzt in einer aufsteigenden Reihen folgen Liste angezeigt.  
  
-   DB2 verfügt über NULLS First-und NULLS Last-Klauseln, mit denen Sie die NULL-Reihenfolge der Nullen ändern können.  
  
SSMA kann DB2 Order by-Verhalten emulieren, indem auf NULL-Werte überprüft wird. Anschließend werden zuerst nach NULL-Werten in der angegebenen Reihenfolge sortiert und dann nach anderen Werten sortiert.  
  
-   Wenn Sie " **Ja**" auswählen, konvertiert SSMA die DB2-Anweisung auf eine Weise, die DB2-Reihenfolge nach Verhalten emuliert.  
  
-   Wenn Sie **Nein** auswählen, werden DB2-Regeln von SSMA ignoriert und eine Fehlermeldung generiert, wenn die NULLS First-Klausel und die NULLS-Klausel (letzte) erkannt werden.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Nein  
  
**Vollständiger Modus:** Zwar  
  
### <a name="emulate-row-count-exceptions-in-select"></a>Zeilen Anzahl Ausnahmen in SELECT emulieren  
Wenn eine SELECT-Anweisung mit einer INTO-Klausel keine Zeilen zurückgibt, löst DB2 eine NO_DATA_FOUND Ausnahme aus. Wenn die-Anweisung zwei oder mehr Zeilen zurückgibt, wird die TOO_MANY_ROWS Ausnahme ausgelöst. Die konvertierte Anweisung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt keine Ausnahme aus, wenn die Zeilen Anzahl von einem abweicht.  
  
-   Wenn Sie " **Ja**" auswählen, fügt SSMA nach jeder SELECT-Anweisung den db_error_exact_one_row_check der sysdb-Prozedur hinzu. Mit diesem Verfahren werden die NO_DATA_FOUND-und TOO_MANY_ROWS Ausnahmen emuliert. Dies ist die Standardeinstellung und ermöglicht es, das DB2-Verhalten so nah wie möglich zu reproduzieren. Sie sollten immer **Ja** auswählen, wenn der Quellcode Ausnahmehandler enthält, die diese Fehler verarbeiten. Beachten Sie Folgendes: Wenn die SELECT-Anweisung innerhalb einer benutzerdefinierten Funktion auftritt, wird dieses Modul in eine gespeicherte Prozedur konvertiert, da das Ausführen gespeicherter Prozeduren und das Auslassen von Ausnahmen nicht mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Funktions Kontext kompatibel ist.  
  
-   Wenn Sie **Nein** auswählen, werden keine Ausnahmen generiert. Dies kann hilfreich sein, wenn SSMA eine benutzerdefinierte Funktion konvertiert und Sie eine Funktion in beibehalten möchten. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="generate-error-for-dbms_sqlparse"></a>Fehler beim Generieren des DBMS_SQL. Analysieren  
  
-   Wenn Sie **Fehler** auswählen, generiert SSMA bei der Konvertierung DBMS_SQL einen Fehler. Analysieren.  
  
-   Wenn Sie die Option **Warnung** auswählen, generiert SSMA beim Konvertierungs DBMS_SQL eine Warnung. Analysieren.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zeit  
  
### <a name="generate-rowid-column"></a>ROWID-Spalte generieren  
Wenn SSMA Tabellen in erstellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , kann eine ROWID-Spalte erstellt werden. Beim Migrieren von Daten erhält jede Zeile einen neuen uniqueidentifier-Wert, der von der Funktion "netwid ()" generiert wird.  
  
-   Wenn Sie **Ja** auswählen, wird die ROWID-Spalte für alle Tabellen erstellt, und es werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] GUIDs beim Einfügen von Werten generiert. Wählen Sie immer **Ja** aus, wenn Sie beabsichtigen, den SSMA-Tester zu verwenden.  
  
-   Wenn Sie **Nein** auswählen, werden den Tabellen keine ROWID-Spalten hinzugefügt.  
  
-   **ROWID-Spalte für Tabellen mit Triggern** hinzufügen ROWID für die Tabellen, die Trigger enthalten.  
  
> [!WARNING]  
> Standardeinstellung im Fall von SQL Server 2005, SQL Server 2008 und SQL Server 2012 und 2014 ist die **ROWID-Spalte für Tabellen mit Triggern hinzufügen**.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** ROWID-Spalte für Tabellen mit Triggern hinzufügen  
  
**Vollständiger Modus:** Zwar  
  
### <a name="generate-unique-index-on-rowid-column"></a>Eindeutigen Index für ROWID-Spalte generieren  
Gibt an, ob SSMA eine eindeutige Index Spalte für die von ROWID generierte Spalte generiert. Wenn die Option auf "yes" festgelegt ist, wird ein eindeutiger Index generiert. wenn der Wert auf "No" festgelegt ist, wird der eindeutige Index nicht in der ROWID-Spalte generiert.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="local-modules-conversion"></a>Konvertieren von lokalen Modulen  
Definiert den Typ des untergeordneten DB2-Unterprogramms (deklariert in einer eigenständigen gespeicherten Prozedur oder Funktion).  
  
-   Wenn Sie **Inline** auswählen, werden die aufrufenden aufrufenden Subprogramme durch den Text ersetzt.  
  
-   Wenn Sie **gespeicherte Prozeduren** auswählen, wird das untergeordnete Programm in eine gespeicherte Prozedur SQL Server konvertiert, und die zugehörigen Aufrufe werden bei diesem Prozedur Aufruf ersetzt.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Es  
  
### <a name="use-isnull-in-string-concatenation"></a>Verwenden von IsNull bei der Zeichen folgen Verkettung  
DB2 und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geben andere Ergebnisse zurück, wenn Zeichen folgen Verkettungen NULL-Werte einschließen. DB2 behandelt den NULL-Wert wie einen leeren Zeichensatz. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt NULL zurück.  
  
-   Wenn Sie **Ja** auswählen, ersetzt SSMA das DB2-Verkettungs Zeichen (| |) durch das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verkettungs Zeichen (+). SSMA überprüft auch die Ausdrücke auf beiden Seiten der Verkettung auf NULL-Werte.  
  
-   Wenn Sie " **Nein**" auswählen, ersetzt SSMA die Verkettungs Zeichen, überprüft jedoch nicht, ob NULL-Werte enthalten sind.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
### <a name="use-isnull-in-replace-function-calls"></a>Verwenden von IsNull in Replace-Funktionsaufrufen  
Die IsNull-Anweisung wird in Replace-Funktionsaufrufen zum Emulieren des DB2-Verhaltens verwendet. Die folgenden Optionen sind für diese Einstellung vorhanden:  
  
-   YES  
  
-   Nein  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Nein  
  
**Vollständiger Modus:** Zwar  
  
### <a name="use-isnull-in-concat-function-calls"></a>Verwenden von IsNull in Concat-Funktionsaufrufen  
Die IsNull-Anweisung wird in Concat-Funktionsaufrufen zum Emulieren des DB2-Verhaltens verwendet. Die folgenden Optionen sind für diese Einstellung vorhanden:  
  
-   YES  
  
-   Nein  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Nein  
  
**Vollständiger Modus:** Zwar  
  
### <a name="use-native-convert-function-when-possible"></a>Verwenden Sie nach Möglichkeit die native Convert-Funktion.  
  
-   Wenn Sie " **Ja**" auswählen, konvertiert SSMA die TO_CHAR (Date, Format) nach Möglichkeit in eine native Convert-Funktion.  
  
-   Wenn Sie **Nein** auswählen, konvertiert SSMA die TO_CHAR (Date, Format) in TO_CHAR_DATE oder TO_CHAR_DATE_LS (Sie wird durch die Optionen "Convert TO_CHAR (Date, Format)" definiert).  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer Modus:** Zwar  
  
**Vollständiger Modus:** Nein  
  
### <a name="use-selectfor-xml-when-converting-selectinto-for-record-variable"></a>Verwenden Sie SELECT... FOR XML beim Umrechnen von SELECT... INTO für Daten Satz Variable  
Gibt an, ob ein XML-Resultset generiert werden soll, wenn Sie eine Daten Satz Variable auswählen.  
  
-   Wenn Sie **Ja** auswählen, gibt die SELECT-Anweisung XML zurück.  
  
-   Wenn Sie **Nein** auswählen, gibt die SELECT-Anweisung ein Resultset zurück.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Nein  
  
## <a name="returning-clause-conversion"></a>Zurückgeben der Klauseln Konvertierung  
  
### <a name="convert-returning-clause-in-delete-statement-to-output"></a>Konvertieren der Rückgabe Klausel in der DELETE-Anweisung in die Ausgabe  
DB2 bietet eine Rückgabe Klausel als Möglichkeit, um gelöschte Werte sofort abzurufen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Funktionalität mit der OUTPUT-Klausel bereit.  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA Rückgabe Klauseln in DELETE-Anweisungen in Ausgabe Klauseln. Da Trigger für eine Tabelle Werte ändern können, kann der zurückgegebene Wert in anders sein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als in DB2.  
  
-   Wenn Sie **Nein** auswählen, generiert SSMA vor DELETE-Anweisungen eine SELECT-Anweisung, um zurückgegebene Werte abzurufen.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-returning-clause-in-insert-statement-to-output"></a>Konvertieren der Rückgabe Klausel in der INSERT-Anweisung in die Ausgabe  
DB2 stellt eine Rückgabe Klausel als Möglichkeit bereit, eingefügte Werte sofort abzurufen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Funktionalität mit der OUTPUT-Klausel bereit.  
  
-   Wenn Sie " **Ja**" auswählen, konvertiert SSMA eine Rückgabe Klausel in einer INSERT-Anweisung in "Output". Da Trigger für eine Tabelle Werte ändern können, kann der zurückgegebene Wert in anders sein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als in DB2.  
  
-   Wenn Sie **Nein** auswählen, emuliert SSMA die DB2-Funktionalität durch Einfügen und anschließende Auswahl von Werten aus einer Verweis Tabelle.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
### <a name="convert-returning-clause-in-update-statement-to-output"></a>Konvertieren der Rückgabe Klausel in der Update-Anweisung in die Ausgabe  
DB2 stellt eine Rückgabe Klausel als Möglichkeit bereit, um aktualisierte Werte sofort abzurufen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt diese Funktionalität mit der OUTPUT-Klausel bereit.  
  
-   Wenn Sie **Ja** auswählen, konvertiert SSMA Rückgabe Klauseln in Update-Anweisungen in Ausgabe Klauseln. Da Trigger für eine Tabelle Werte ändern können, kann der zurückgegebene Wert in anders sein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als in DB2.  
  
-   Wenn Sie **Nein** auswählen, generiert SSMA SELECT-Anweisungen nach Update-Anweisungen, um zurückgegebene Werte abzurufen.  
  
Wenn Sie im Feld **Modus** einen Konvertierungsmodus auswählen, wendet SSMA die folgende Einstellung an:  
  
**Standardmodus/optimistischer/Vollmodus:** Zwar  
  
## <a name="sequence-conversion"></a>Sequenz Konvertierung  
  
### <a name="convert-sequence-generator"></a>Sequence Generator konvertieren  
In DB2 können Sie eine Sequenz verwenden, um eindeutige Bezeichner zu generieren.  
  
SSMA kann Sequenzen in Folgendes konvertieren.  
  
-   Mit SQL Server Sequence Generator (diese Option ist nur bei der Umstellung auf SQL Server 2012 und SQL Server 2014) verfügbar.  
  
-   Verwenden des SSMA Sequence Generators.  
  
-   Verwenden der Spalten Identität.  
  
Die Standardoption bei der Umstellung auf SQL Server 2012 oder SQL Server 2014 besteht in der Verwendung des SQL Server Sequence Generators. SQL Server 2012 und SQL Server 2014 unterstützt jedoch nicht das Abrufen des aktuellen Sequenz Werts (z. b. der DB2 Sequence currval-Methode). Anleitungen zum Migrieren der currval-Methode der DB2-Sequenz finden Sie auf der SSMA-Teamblog-Website  
  
SSMA bietet auch eine Option zum Konvertieren der DB2-Sequenz in den SSMA-Sequenz Emulator. Dies ist die Standardoption, wenn Sie in SQL Server vor 2012 konvertieren.  
  
Schließlich können Sie die einer Spalte in der Tabelle zugewiesene Sequenz auch in SQL Server Identitäts Werte konvertieren. Sie müssen die Zuordnung zwischen den Sequenzen zu einer Identitäts Spalte auf der DB2- **Tabellen** Registerkarte angeben.  
  
### <a name="convert-currval-outside-triggers"></a>Currval-externe Trigger konvertieren  
Nur sichtbar, wenn der Convert Sequence Generator mithilfe der **Spalten Identität** auf festgelegt ist. Da DB2-Sequenzen aus Tabellen getrennte Objekte sind, verwenden viele Tabellen, die Sequenzen verwenden, einen---------------- SSMA kommentiert diese Anweisungen und markiert sie als Fehler, wenn der Auskommentierung Fehler erzeugt.  
  
-   Wenn Sie **Ja** auswählen, markiert SSMA alle Verweise auf externe Trigger für die konvertierte Sequenz currval mit einer Warnung.  
  
-   Wenn Sie **Nein** auswählen, markiert SSMA alle Verweise auf externe Trigger für die konvertierte Sequenz currval mit einem Fehler.  
  
## <a name="see-also"></a>Weitere Informationen  
[Referenz zur Benutzeroberfläche &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
