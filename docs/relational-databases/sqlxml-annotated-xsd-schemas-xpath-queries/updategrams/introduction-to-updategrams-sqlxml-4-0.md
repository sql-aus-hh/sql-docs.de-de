---
title: Einführung in Update grams (SQLXML)
description: Erfahren Sie mehr über SQLXML 4,0 Update grams, die zum Einfügen, aktualisieren oder Löschen von Daten in einer Datenbank verwendet werden können.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- updategrams [SQLXML], mapping schema specifying
- namespaces [SQLXML]
- updategrams [SQLXML], about updategrams
- attribute-centric mapping
- invalid characters [SQLXML]
- element-centric mapping [SQLXML]
- mapping schema [SQLXML], updategrams
- namespaces [SQLXML], updategrams
- executing updategrams [SQLXML]
- implicit schema mapping
ms.assetid: cfe24e82-a645-4f93-ab16-39c21f90cce6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a416950247208c682d0fdc923db71357405fd0e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170622"
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Einführung in Updategrams (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Mithilfe eines Update grams oder der OPENXML-Funktion können Sie eine Datenbank in aus einem vorhandenen XML-Dokument ändern (einfügen, aktualisieren oder löschen) [!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
 Die OPENXML-Funktion ändert eine Datenbank durch Aufteilen des vorhandenen XML-Dokuments und Bereitstellen eines Rowsets, das an eine INSERT-, UPDATE- oder DELETE-Anweisung übergeben werden kann. Mit OPENXML werden Operationen direkt für die Datenbanktabellen ausgeführt. Daher eignet sich OPENXML besonders gut, wenn Rowsetanbieter, wie Tabellen, als Quelle auftreten können.  
  
 Wie mit OPENXML können Sie mit einem Updategram Daten in der Datenbank einfügen, aktualisieren oder löschen. Ein Updategram wird jedoch für die XML-Sichten verwendet, die vom XSD-Schema (oder einem XDR-Schema) bereitgestellt werden. So werden beispielsweise die Updates auf die vom Zuordnungsschema bereitgestellte XML-Sicht angewendet. Das Zuordnungsschema enthält die erforderlichen Informationen zum Zuordnen von XML-Elementen und -Attributen zu den entsprechenden Datenbanktabellen und -spalten. Das Updategram verwendet diese Zuordnungsinformationen zum Aktualisieren der Datenbanktabellen und -spalten.  
  
> [!NOTE]  
>  Diese Dokumentation setzt voraus, dass Sie mit Vorlagen und der Unterstützung von Zuordnungsschemas in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vertraut sind. Weitere Informationen finden Sie unter [Einführung in XSD-Schemas mit Anmerkungen &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Informationen zu Legacy Anwendungen, die XDR verwenden, finden Sie unter mit Anmerkungen versehene [XDR-Schemas &#40;in SQLXML 4,0&#41;veraltet ](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="required-namespaces-in-the-updategram"></a>Erforderliche Namespaces im Updategram  
 Die Schlüsselwörter in einem Update Gram, z **\<sync>** . b **\<before>** ., und **\<after>** , sind im **urn: Schemas-Microsoft-com: XML-Updategram-** Namespace vorhanden. Sie können ein beliebiges Namespacepräfix verwenden. In dieser Dokumentation gibt das **updg** -Präfix den **Update Gram** -Namespace an.  
  
## <a name="reviewing-syntax"></a>Überprüfen der Syntax  
 Ein Update Gram ist eine Vorlage mit **\<sync>** -, **\<before>** -und-Blöcken, die **\<after>** die Syntax des Update grams bilden. Der folgende Code zeigt diese Syntax in ihrer einfachsten Form:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema= "AnnotatedSchemaFile.xml"] >  
    <updg:before>  
        ...  
    </updg:before>  
    <updg:after>  
        ...  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 In den folgenden Definitionen werden die Rollen der einzelnen Blöcke beschrieben:  
  
 **\<before>**  
 Identifiziert den vorhandenen Status (auch als "Vorher-Status" bezeichnet) der Datensatzinstanz.  
  
 **\<after>**  
 Identifiziert den neuen Status, in den Daten geändert werden sollen.  
  
 **\<sync>**  
 Enthält die **\<before>** -und- **\<after>** Blöcke. Ein **\<sync>** -Block kann mehr als einen Satz von **\<before>** -und- **\<after>** Blöcken enthalten. Wenn mehr als ein Satz von **\<before>** -und- **\<after>** Blöcken vorhanden ist, müssen diese Blöcke (auch wenn Sie leer sind) als Paare angegeben werden. Außerdem kann ein Update Gram mehr als einen **\<sync>** Block enthalten. Jeder **\<sync>** Block ist eine Einheit der Transaktion (was bedeutet, dass entweder alles im **\<sync>** Block ausgeführt wird oder nichts ausgeführt wird). Wenn Sie mehrere- **\<sync>** Blöcke in einem Update Gram angeben, wirkt sich der Ausfall eines **\<sync>** Blocks nicht auf die anderen- **\<sync>** Blöcke aus.  
  
 Ob ein Update Gram eine Daten Satz Instanz löscht, einfügt oder aktualisiert, hängt vom Inhalt der **\<before>** -und- **\<after>** Blöcke ab:  
  
-   Wenn eine Daten Satz Instanz nur im- **\<before>** Block ohne entsprechende Instanz im- **\<after>** Block angezeigt wird, führt das Update Gram einen Löschvorgang aus.  
  
-   Wenn eine Daten Satz Instanz nur im- **\<after>** Block ohne entsprechende Instanz im-Block angezeigt wird **\<before>** , handelt es sich um einen INSERT-Vorgang.  
  
-   Wenn eine Daten Satz Instanz im **\<before>** -Block angezeigt wird und über eine entsprechende-Instanz im- **\<after>** Block verfügt, handelt es sich um einen Update Vorgang. In diesem Fall aktualisiert das Update Gram die Daten Satz Instanz auf die Werte, die im-Block angegeben werden **\<after>** .  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>Angeben eines Zuordnungsschemas im Updategram  
 In einem Updategram kann die von einem Zuordnungsschema (sowohl XSD- als auch XDR-Schemas werden unterstützt) bereitgestellte XML-Abstraktion implizit oder explizit sein (d. h., ein Updategram kann mit angegebenem Zuordnungsschema oder ohne angegebenes Zuordnungsschema verwendet werden). Wenn Sie kein Zuordnungs Schema angeben, geht das Update Gram davon aus, dass eine implizite Zuordnung (die Standard Zuordnung) erfolgt, wobei jedes Element im **\<before>** Block oder **\<after>** Block einer Tabelle zugeordnet wird und dass das untergeordnete Element oder Attribut jedes Elements einer Spalte in der Datenbank zugeordnet ist. Wenn Sie ausdrücklich ein Zuordnungsschema angeben, müssen die Elemente und Attribute im Updategram mit den Elementen und Attributen im Zuordnungsschema übereinstimmen.  
  
### <a name="implicit-default-mapping"></a>Implizite Zuordnung (Standardzuordnung)  
 Ein Updategram, das einfache Updates durchführt, benötigt in der Regel kein Zuordnungsschema. In diesem Fall verwendet das Updategram das Standardzuordnungsschema.  
  
 Das folgende Updategram zeigt eine implizite Zuordnung. In diesem Beispiel fügt das Updategram einen neuen Kunden in die Sales.Customer-Tabelle ein. Da in diesem Update Gram die implizite Zuordnung verwendet wird, wird das \<Sales.Customer> -Element der Sales. Customer-Tabelle zugeordnet, und die Attribute CustomerID und SalesPersonID werden den entsprechenden Spalten in der Sales. Customer-Tabelle zugeordnet.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
</updg:before>  
<updg:after>  
    <Sales.Customer CustomerID="1" SalesPersonID="277" />  
    </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="explicit-mapping"></a>Explizite Zuordnung  
 Wenn Sie ein Zuordnungsschema angeben (XSD oder XDR) verwendet das Updategram das Schema um zu bestimmen, welche Datenbanktabellen und -spalten aktualisiert werden sollen.  
  
 Wenn das Update Gram eine komplexe Aktualisierung ausführt (z. b. das Einfügen von Datensätzen in mehrere Tabellen auf der Grundlage der über-/Unterordnungsbeziehung, die im Zuordnungsschema angegeben ist), müssen Sie das Zuordnungschema explizit mithilfe des Attributs **Mapping-Schema** angeben, für das das Update Gram ausgeführt wird.  
  
 Da ein Updategram eine Vorlage ist, ist der für das Zuordnungsschema im Updategram angegebene Pfad bezieht sich auf den Speicherort der Vorlagendatei (relativ zum Speicherort des Updategrams). Weitere Informationen finden Sie unter [Angeben eines Mapping-Schemas mit Anmerkungen in einem Update Gram &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Elementzentrierte und attributzentrierte Zuordnung in Updategrams  
 Bei der Standardzuordnung (wenn im Updategram kein Zuordnungsschema angegeben ist) werden die Updategramelemente Tabellen und die untergeordneten Elemente (bei der elementzentrierten Zuordnung) und die Attribute (bei der attributzentrierten Zuordnung) Spalten zugeordnet.  
  
### <a name="element-centric-mapping"></a>Elementzentrierte Zuordnung  
 Ein Element in einem elementzentrierten Updategram enthält untergeordnete Elemente, die die Eigenschaften des Elements angeben. Ein Beispiel finden Sie im folgenden Updategram. Das **\<Person.Contact>** -Element enthält die untergeordneten **\<FirstName>** **\<LastName>** Elemente und. Diese untergeordneten Elemente sind Eigenschaften des- **\<Person.Contact>** Elements.  
  
 Da in diesem Update Gram kein Zuordnungs Schema angegeben ist, verwendet das Update Gram die implizite Zuordnung, bei der das **\<Person.Contact>** -Element der Person. Contact-Tabelle zugeordnet wird und die untergeordneten Elemente den Spalten FirstName und LastName zugeordnet werden.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:after>  
    <Person.Contact>  
       <FirstName>Catherine</FirstName>  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="attribute-centric-mapping"></a>Attributzentrierte Zuordnung  
 Die Elemente in einer attributzentrierten Zuordnung verfügen über Attribute. Im folgenden Updategram wird die attributzentrierte Zuordnung verwendet. In diesem Beispiel besteht das **\<Person.Contact>** -Element aus den Attributen **FirstName** und **LastName** . Diese Attribute sind die Eigenschaften des- **\<Person.Contact>** Elements. Wie im vorherigen Beispiel ist in diesem Update Gram kein Zuordnungs Schema angegeben. Daher basiert es auf impliziter Zuordnung, um das **\<Person.Contact>** Element der Person. Contact-Tabelle und die Attribute des Elements den entsprechenden Spalten in der Tabelle zuzuordnen.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" LastName="Abel" />  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="using-both-element-centric-and-attribute-centric-mapping"></a>Verwenden der elementzentrierten und der attributzentrierten Zuordnung  
 Sie können eine Mischung elementzentrierter und attributzentrierter Zuordnung angeben, wie im folgenden Updategram dargestellt. Beachten Sie, dass das **\<Person.Contact>** -Element sowohl ein Attribut als auch ein untergeordnetes Element enthält. Für dieses Updategram wird die implizite Zuordnung verwendet. Folglich werden das **FirstName** -Attribut und das untergeordnete- **\<LastName>** Element den entsprechenden Spalten in der Person. Contact-Tabelle zugeordnet.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" >  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="working-with-characters-valid-in-sql-server-but-not-valid-in-xml"></a>Verwenden von Zeichen, die in SQL Server gültig sind, in XML jedoch nicht  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dürfen Tabellennamen Leerzeichen enthalten. Dieser Tabellennamentyp ist in XML jedoch nicht gültig.  
  
 Verwenden Sie zum Codieren von Zeichen, die gültige Bezeichner sind [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , aber keine gültigen XML-Bezeichner sind, "__xHHHH \_ \_ " als Codierungs Wert, wobei HHHH für den vierstelligen hexadezimalen UCS-2-Code für das Zeichen in der signifikantesten bidirektionalen Reihenfolge steht. Mit diesem Codierungsschema wird ein Leerzeichen durch x0020 (der vierstellige Hexadezimal Code für ein Leerzeichen) ersetzt; Daher wird der Tabellenname [Order Details] in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] \_ in XML _x005B_Order_x0020_Details_x005D.  
  
 Ebenso müssen Sie möglicherweise dreiteilige Elementnamen angeben, z \<[database].[owner].[table]> . b.. Da die Klammer Zeichen ([und]) in XML nicht gültig sind, müssen Sie diese als angeben \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_> , wobei _x005B \_ die Codierung für die linke eckige Klammer ([) und _x005D \_ die Codierung für die rechte eckige Klammer (]) ist.  
  
## <a name="executing-updategrams"></a>Ausführen von Updategrams  
 Da ein Updategram eine Vorlage ist, gelten alle Verarbeitungsmechanismen einer Vorlage auch für ein Updategram. Zum Ausführen eines Updategrams in SQLXML 4.0 können Sie eine der beiden folgenden Möglichkeiten verwenden:  
  
-   Senden des Updategrams in einem ADO-Befehl  
  
-   Senden des Updategrams in einem OLE DB-Befehl  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitsüberlegungen zu Update grams &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
