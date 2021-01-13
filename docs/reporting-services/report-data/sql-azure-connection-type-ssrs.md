---
title: Azure SQL-Verbindungstyp | Microsoft-Dokumentation
description: Die Datenerweiterung „Azure SQL-Verbindung“ unterstützt mehrwertige Parameter, Serveraggregate und getrennt von der Verbindungszeichenfolge verwaltete Anmeldeinformationen.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.date: 02/15/2019
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 4dc1fcf4aa738a018274f65d7d707f6461b9c45c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878843"
---
# <a name="azure-sql-connection-type-ssrs"></a>Azure SQL-Verbindungstyp (SSRS)

[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] ist eine cloudbasierte, gehostete relationale Datenbank, die auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Technologien basiert. Wenn Sie Daten aus [!INCLUDE[ssSDS](../../includes/sssds-md.md)] in den Bericht einschließen möchten, benötigen Sie ein Dataset, das auf einer Berichtsdatenquelle vom Typ " [!INCLUDE[ssSDS](../../includes/sssds-md.md)]" basiert. Dieser integrierte Datenquellentyp basiert auf der [!INCLUDE[ssSDS](../../includes/sssds-md.md)] -Datenerweiterung. Verwenden Sie diesen Datenquellentyp, um eine Verbindung mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)]herzustellen und Daten abzurufen.  
  
Diese Datenerweiterung unterstützt mehrwertige Parameter, Serveraggregate und getrennt von der Verbindungszeichenfolge verwaltete Anmeldeinformationen.  
  
[!INCLUDE[ssSDS](../../includes/sssds-md.md)] mit einer vor Ort installierten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und dem Abrufen von Daten aus [!INCLUDE[ssSDS](../../includes/sssds-md.md)] vergleichbar. Die Vorgehensweise entspricht dem Abrufen von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> Legen Sie das Verbindungstimeout beim Öffnen einer Verbindung mit einer [!INCLUDE[ssSDS](../../includes/sssds-md.md)]auf 30 Sekunden fest.
  
Weitere Informationen finden Sie unter [Microsoft Azure SQL-Datenbank auf docs.microsoft.com](/azure/sql-database/).  
  
Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
## <a name="connection-string"></a><a name="Connection"></a> Verbindungszeichenfolge

Wenn Sie eine Verbindung mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] herstellen, stellen Sie eine Verbindung mit einem Datenbankobjekt in der Cloud her. Wie Onsitedatenbanken kann die gehostete Datenbank mehrere Schemas mit mehreren Tabellen, Sichten und gespeicherte Prozeduren enthalten. Sie geben das zu verwendende Datenbankobjekt im Abfrage-Designer an. Wenn Sie in der Verbindungszeichenfolge keine Datenbank angeben, wird eine Verbindung mit der Standarddatenbank hergestellt, die Ihnen vom Administrator zugewiesen wurde.  
  
Erfragen Sie bei Ihrem Datenbankadministrator die Verbindungsinformationen und die Anmeldeinformationen, die verwendet werden sollen, um eine Verbindung mit der Datenquelle herzustellen. In der Verbindungszeichenfolge im folgenden Beispiel wird eine gehostete Beispieldatenbank mit dem Namen AdventureWorks angegeben.  
  
```
Data Source=<host>;Initial Catalog=AdventureWorks; Encrypt=True;  
```
  
Außerdem verwenden Sie das Dialogfeld **Datenquelleneigenschaften**, um Anmeldeinformationen wie Benutzername und Kennwort anzugeben. Die Optionen `User Id` und `Password` werden automatisch an die Verbindungszeichenfolge angefügt, sie müssen nicht als Teil der Verbindungszeichenfolge eingegeben werden.  
  
Weitere Informationen und Beispiele für Verbindungszeichenfolgen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
## <a name="credentials"></a><a name="Credentials"></a> Anmeldeinformationen

Die Windows-Authentifizierung (integrierte Sicherheit) wird nicht unterstützt. Wenn Sie versuchen, mithilfe der Windows-Authentifizierung eine Verbindung mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] herzustellen, tritt ein Fehler auf. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] unterstützt nur die SQL Server-Authentifizierung (Benutzername und Kennwort). Benutzer müssen daher jedes Mal Anmeldeinformationen (Benutzername und Kennwort) eingeben, wenn sie eine Verbindung mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)]herstellen.  
  
Die Anmeldeinformationen müssen über Zugriffsberechtigungen für die Datenbank verfügen. Abhängig von der Abfrage benötigen Sie u. U. andere Berechtigungen, z. B. Berechtigungen für die Ausführung von gespeicherten Prozeduren und den Zugriff auf Tabellen und Sichten. Der Besitzer der externen Datenquelle muss entsprechende Anmeldeinformationen konfigurieren, die Lesezugriff auf die benötigten Datenbankobjekte gewähren.  
  
Auf einem Berichterstellungsclient sind die folgenden Optionen zum Angeben von Anmeldeinformationen verfügbar:  
  
- Verwendung eines gespeicherten Benutzernamens und eines gespeicherten Kennworts. Aktivieren Sie Optionen zur Verwendung der Anmeldeinformationen als Windows-Anmeldeinformationen, um den doppelten Sprung auszuhandeln, der auftritt, wenn sich die Datenbank mit den Berichtsdaten vom Berichtsserver unterscheidet. Sie können auch die Identität des authentifizierten Benutzers annehmen, nachdem die Verbindung mit der Datenquelle hergestellt wurde.  
  
- Anmeldeinformationen sind nicht erforderlich. Zur Verwendung dieser Option müssen Sie zuvor das Konto für die unbeaufsichtigte Ausführung auf dem Berichtsserver konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren des Kontos für die unbeaufsichtigte Ausführung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) oder [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
## <a name="queries"></a><a name="Query"></a> Abfragen

Mit einer Abfrage wird angegeben, welche Daten für ein Berichtsdataset abgerufen werden sollen. Die Feldauflistung für ein Dataset wird mit den Spalten aus dem Resultset einer Abfrage aufgefüllt. Wenn die Abfrage mehrere Resultsets zurückgibt, verarbeitet der Bericht nur das erste Resultset, das durch eine Abfrage abgerufen wird. Obwohl einige Unterschiede zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)]bestehen, z. B. die Größe unterstützter Datenbanken, ähnelt das Schreiben von Abfragen für [!INCLUDE[ssSDS](../../includes/sssds-md.md)]weitgehend dem Schreiben von Abfragen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken. Einige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen (z.B. BACKUP) werden in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] nicht unterstützt. Diese Anweisungen werden allerdings in Berichtsabfragen nicht verwendet. Weitere Informationen finden Sie unter [SQL Server-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md).  
  
Wenn Sie eine neue Abfrage erstellen oder eine vorhandene Abfrage öffnen, die im grafischen Abfrage-Designer dargestellt werden kann, ist der relationale Abfrage-Designer standardmäßig verfügbar. Zum Angeben einer Abfrage stehen Ihnen folgende Methoden zur Auswahl:  
  
- Erstellen Sie eine Abfrage interaktiv. Verwenden Sie den relationalen Abfrage-Designer, der eine hierarchische Ansicht der Tabellen, Sichten, gespeicherten Prozeduren und anderen Datenbankelemente nach Datenbankschema angeordnet anzeigt. Wählen Sie Spalten aus Tabellen oder Sichten aus, oder geben Sie gespeicherte Prozeduren oder Tabellenwertfunktionen an. Begrenzen Sie die Anzahl abzurufender Datenzeilen durch Angabe von Filterkriterien. Passen Sie den Filter an, wenn der Bericht ausgeführt wird, indem Sie die Parameteroption festlegen.  
  
- Geben Sie eine Abfrage ein, oder fügen Sie sie ein. Verwenden Sie den textbasierten Abfrage-Designer, um [!INCLUDE[tsql](../../includes/tsql-md.md)]-Text direkt einzugeben, um Abfragetext aus einer anderen Quelle einzufügen, um komplexe Abfragen einzugeben, die mit dem relationalen Abfrage-Designer nicht erstellt werden können, oder um abfragebasierte Ausdrücke einzugeben.  
  
- Importiert eine vorhandene Abfrage aus einer Datei oder einem Bericht. Verwenden Sie die Schaltfläche **Abfrage importieren** in einem Abfrage-Designer, um nach einer SQL- oder RDL-Datei zu suchen und eine Abfrage zu importieren.  
  
Der textbasierte Abfrage-Designer unterstützt die folgenden zwei Modi:  
  
- [Text](#QueryText) Geben Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehle zur Auswahl von Daten aus der Datenquelle ein.  
  
- [Gespeicherte Prozedur](#QueryStoredProcedure) Treffen Sie in einer Liste von gespeicherten Prozeduren eine Auswahl.  
  
Weitere Informationen finden Sie unter [Benutzeroberfläche des relationalen Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) und [Benutzeroberfläche des textbasierten Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
Der in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] verwendete grafische Abfrage-Designer bietet integrierte Unterstützung für das Gruppieren und für Aggregate, sodass Sie Abfragen schreiben können, durch die nur Zusammenfassungsdaten abgerufen werden. Die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Sprachfunktionen sind die GROUP BY-Klausel, das DISTINCT-Schlüsselwort und Aggregate wie SUM und COUNT. Der textbasierte Abfrage-Designer bietet vollständige Unterstützung für die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Sprache, einschließlich Gruppieren und Aggregate. Weitere Informationen über [!INCLUDE[tsql](../../includes/tsql-md.md)] finden Sie unter [Transact-SQL-Referenz &#40;Datenbank-Engine&#41;](../../t-sql/language-reference.md).  
  
### <a name="using-query-type-text"></a><a name="QueryText"></a> Verwenden des Abfragetyps "Text"

Im textbasierten Abfrage-Designer geben Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehle ein, um die Daten in einem Dataset zu definieren. Mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage werden z. B. die Namen aller Mitarbeiter ausgewählt, die Marketingassistenten sind:

```sql
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```

Klicken Sie auf der Symbolleiste auf die Schaltfläche **Ausführen** ( **!** ), um die Abfrage auszuführen und ein Resultset anzuzeigen.  
  
Um diese Abfrage zu parametrisieren, fügen Sie einen Abfrageparameter hinzu. Beispiel: Ändern Sie die WHERE-Klausel wie folgt:  

```sql
WHERE HumanResources.Employee.JobTitle = (@JobTitle)  
```

Bei der Ausführung der Abfrage werden automatisch Berichtsparameter erstellt, die den Abfrageparametern entsprechen. Weitere Informationen finden Sie unter [Abfrageparameter](#Parameters) weiter unten in diesem Thema.  
  
### <a name="using-query-type-storedprocedure"></a><a name="QueryStoredProcedure"></a> Verwenden des Abfragetyps "StoredProcedure"

Zum Angeben einer gespeicherte Prozedur für eine Datasetabfrage stehen Ihnen folgende Methoden zur Verfügung:  
  
- Legen Sie im Dialogfeld **Dataseteigenschaften** die Option **Gespeicherte Prozedur** fest. Wählen Sie aus der Dropdownliste mit gespeicherten Prozeduren und Tabellenwertfunktionen aus.  
  
- Wählen Sie im relationalen Abfrage-Designer im Bereich Datenbanksicht eine gespeicherte Prozedur oder Tabellenwertfunktion aus.  
  
- Wählen Sie im textbasierten Abfrage-Designer **StoredProcedure** auf der Symbolleiste aus.  
  
Nachdem Sie eine gespeicherte Prozedur oder Tabellenwertfunktion gewählt haben, können Sie die Abfrage ausführen. Sie werden zur Angabe von Eingabeparameterwerten aufgefordert. Bei der Ausführung der Abfrage werden automatisch Berichtsparameter erstellt, die den Eingabeparametern entsprechen. Weitere Informationen finden Sie unter [Abfrageparameter](#Parameters) weiter unten in diesem Thema.  
  
Nur das erste Resultset wird unterstützt, das für eine gespeicherte Prozedur abgerufen wird. Wenn eine gespeicherte Prozedur mehrere Resultsets zurückgibt, wird nur das erste Resultset verwendet.  
  
Falls eine gespeicherte Prozedur einen Parameter mit einem Standardwert enthält, können Sie auf diesen Wert zugreifen, indem Sie das DEFAULT-Schlüsselwort als Wert für den Parameter verwenden. Wenn der Abfrageparameter mit einem Berichtsparameter verknüpft ist, kann der Benutzer das Wort DEFAULT im Eingabefeld für den Berichtsparameter eingeben oder auswählen.  
  
Weitere Informationen zum Aufrufen von gespeicherten Prozeduren finden Sie unter [Gespeicherte Prozeduren (Datenbank-Engine)](../../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
## <a name="parameters"></a><a name="Parameters"></a> Parameter

Wenn Abfragetext Abfragevariablen oder gespeicherte Prozeduren mit Eingabeparametern enthält, werden die entsprechenden Abfrageparameter für das Dataset und Berichtsparameter für den Bericht automatisch generiert. Der Abfragetext darf keine DECLARE-Anweisung für jede Abfragevariable enthalten.  
  
 Durch die folgende SQL-Abfrage wird z. B. ein Berichtsparameter mit dem Namen **EmpID** erstellt:  

```sql
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```

Jeder Berichtsparameter ist standardmäßig vom Datentyp "Text" und verfügt über ein automatisch erstelltes Dataset, mit dem eine Dropdownliste verfügbarer Werte bereitgestellt wird. Die Standardwerte müssen möglicherweise nach dem Erstellen der Berichtsparameter geändert werden. Weitere Informationen finden Sie unter [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)" basiert.  

## <a name="remarks"></a><a name="Remarks"></a> Hinweise
  
###### <a name="alternate-data-extensions"></a>Alternative Datenerweiterungen

Sie können mit einem ODBC-Datenquellentyp auch aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank Daten abrufen. Das Herstellen einer Verbindung mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] über OLE DB wird nicht unterstützt.  
  
Weitere Informationen finden Sie unter [ODBC-Verbindungstyp (SSRS)](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
###### <a name="platform-and-version-information"></a>Plattform- und Versionsinformationen

Weitere Informationen zur Plattform- und Versionsunterstützung finden Sie unter [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  

::: moniker range=">=sql-server-2016"

## <a name="azure-sql-database-and-aad"></a>Azure SQL-Datenbank und AAD

Sie können die Azure SQL-Datenbank mit Azure Active Directory (AAD) verwenden.

Dieses Szenario wird unterstützt, wenn Sie die folgenden Elemente ordnungsgemäß einrichten:

- Die [Active Directory-Authentifizierungsbibliothek für SQL Server (Active Directory Authentication Library for SQL Server, ADALSQL)](https://www.microsoft.com/download/details.aspx?id=48742) ist auf dem Berichtsserver installiert.
- Die [Active Directory-Verbunddienste (Active Directory Federation Services, AD FS)](/windows-server/identity/active-directory-federation-services) sind zum Herstellen eines Verbunds über lokales Active Directory (AD) und AAD hinweg konfiguriert.
- Die [eingeschränkte Kerberos-Delegierung Delegation (Kerberos Constrained Delegation, KCD)](/windows-server/security/kerberos/kerberos-constrained-delegation-overview) ist vom Berichtsserver zum AD FS-Server konfiguriert.
- Konfigurieren Sie die Berichts-/Datenquelle für die Authentifizierung bei [Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview), wenn der Benutzer den Bericht anzeigt.

::: moniker-end

## <a name="how-to-topics"></a><a name="HowTo"></a> Themen zur Vorgehensweise

Dieser Abschnitt enthält schrittweise Anweisungen zum Arbeiten mit Datenverbindungen, Datenquellen und Datasets.  
  
[Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
[Erstellen eines freigegebenen Datasets oder eingebetteten Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
[Hinzufügen eines Filters zu einem Dataset &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
## <a name="related-sections"></a><a name="Related"></a> Verwandte Abschnitte

Diese Abschnitte der Dokumentation enthalten umfassende grundlegende Informationen zu Berichtsdaten und Informationen zum Definieren, Entwerfen, Anpassen und Verwenden der mit Daten zusammenhängenden Teile eines Berichts.  
  
[Berichtsdatasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
Bietet eine Übersicht über den Zugriff auf Daten für den Bericht.  
  
[Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
Enthält Informationen zu Datenverbindungen und Datenquellen.  
  
[Erstellen von Berichten zu eingebetteten und freigegebenen Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
Enthält Informationen zu eingebetteten und freigegebenen Datasets.  
  
[Datasetfelder-Sammlung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
Enthält Informationen zur von der Abfrage generierten Datasetfeldauflistung.  
  
[Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
Enthält ausführliche Informationen zur Plattform- und Versionsunterstützung für die einzelnen Datenerweiterungen.  
  
## <a name="see-also"></a>Weitere Informationen

[Microsoft Azure SQL-Datenbank auf docs.microsoft.com](/azure/sql-database/)  
[Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filtern, Gruppieren und Sortieren von Daten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)
