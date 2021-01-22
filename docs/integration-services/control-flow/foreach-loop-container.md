---
description: Foreach-Schleifencontainer
title: Foreach-Schleifencontainer | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.foreachloopcontainer.f1
- sql13.dts.designer.foreachloopcontainer.general.f1
- sql13.dts.designer.foreachloopcontainer.collection.f1
- sql13.dts.designer.foreachloopcontainer.mapping.f1
- sql13.dts.designer.schemarestrictions.f1
- sql13.dts.designer.foreachitemcolumns.f1
- sql13.dts.designer.selectsmoenumeration.f1
- sql14.dts.designer.foreachloopcontainer.f1
- sql14.dts.designer.foreachloopcontainer.general.f1
- sql14.dts.designer.foreachloopcontainer.collection.f1
- sql14.dts.designer.foreachloopcontainer.mapping.f1
- sql14.dts.designer.schemarestrictions.f1
- sql14.dts.designer.foreachitemcolumns.f1
- sql14.dts.designer.selectsmoenumeration.f1
helpviewer_keywords:
- repeating control flow
- Foreach Loop containers
- foreach enumerators [Integration Services]
- containers [Integration Services], Foreach Loop
ms.assetid: dd6cc2ba-631f-4adf-89dc-29ef449c6933
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7ae64e777b08cdf4df6f7e8fdd05ff8c3822a06b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171522"
---
# <a name="foreach-loop-container"></a>Foreach-Schleifencontainer

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der Foreach-Schleifencontainer definiert eine sich wiederholende Ablaufsteuerung in einem Paket. Die Schleifenimplementierung ist mit der **Foreach**-Schleifenstruktur in Programmiersprachen zu vergleichen. In einem Paket wird die Schleife mithilfe eines Foreach-Enumerators aktiviert.  Der Foreach-Schleifencontainer wiederholt die Ablaufsteuerung für jedes Element eines angegebenen Enumerators.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stellt die folgenden Enumeratortypen bereit:  
  
-   Foreach-ADO-Enumerator zum Aufzählen von Zeilen in Tabellen. Beispielsweise können Sie die Zeilen in einem ADO-Recordset abrufen.  
  
     Das Recordset-Ziel speichert Daten im Speicher in einem Recordset, das in einer **Object**-Paketvariablen des Datentyps gespeichert ist. Sie verwenden einen Foreach-Schleifencontainer mit dem Foreach-ADO-Enumerator normalerweise, um die Zeilen eines Recordsets nacheinander zu verarbeiten. Die für den Foreach-ADO-Enumerator angegebene Variable muss vom Object-Datentyp sein. Weitere Informationen zum Recordset-Ziel finden Sie unter [Use a Recordset Destination (Verwenden eines Recordset-Ziels)](../../integration-services/data-flow/use-a-recordset-destination.md).  
  
-   Enumerator für Foreach-ADO.NET-Schemarowsets zum Aufzählen der Schemainformationen zu einer Datenquelle. Beispielsweise können Sie die Tabellen in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] aufzählen und eine Liste mit diesen Tabellen abrufen.  
  
-   Foreach-Datei-Enumerator zum Aufzählen von Dateien in einem Ordner. Der Enumerator kann Unterordner durchlaufen. Beispielsweise können Sie alle Dateien mit der Dateinamenerweiterung LOG im Windows-Ordner und in dessen Unterordnern lesen. Beachten Sie, dass die Reihenfolge, in der die Dateien abgerufen werden, nicht festgelegt werden kann.  
  
-   Foreach-Enumerator für Daten aus Variablen zum Aufzählen des aufzählbaren Objekts, das in einer angegebenen Variable enthalten ist. Das aufzählbare Objekt kann z. B. ein Array, ein ADO.NET-**DataTable**-Objekt oder ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Enumerator sein. Beispielsweise können Sie die Werte eines Arrays aufzählen, das Servernamen enthält.  
  
-   Foreach-Element-Enumerator zum Aufzählen von Elementen, bei denen es sich um Sammlungen handelt. Beispielsweise können Sie die Namen der ausführbaren Dateien und Arbeitsverzeichnisse aufzählen, die ein Task „Prozess ausführen“ verwendet.  
  
-   Foreach-NodeList-Enumerator zum Aufzählen des Resultsets eines XPATH-Ausdrucks (XML Path Language). Beispielsweise zählt der folgende Ausdruck alle Autoren der Klassik auf und ruft eine Liste dafür ab: `/authors/author[@period='classical']`.  
  
-   Foreach-SMO-Enumerator zum Aufzählen von SMO-Objekten ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects). Beispielsweise können Sie die Sichten in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank aufzählen und eine Liste dafür abrufen.  
  
-   Foreach-HDFS-Datei-Enumerator zum Aufzählen von HDFS-Dateien am angegebenen HDFS-Speicherort.  
  
-   Foreach-Azure-Blob-Enumerator zum Aufzählen von Blobs in einem Blobcontainer in Azure Storage.  

-   Foreach-ADLS-Datei-Enumerator zum Aufzählen von Dateien in einem Verzeichnis in Azure Data Lake Store.

-   Foreach-Data Lake Storage Gen2-Datei-Enumerator zum Aufzählen von Dateien in einem Verzeichnis in Azure Data Lake Storage Gen2.
  
 Das folgende Diagramm zeigt einen Foreach-Schleifencontainer mit einem Dateisystemtask. Die Foreach-Schleife verwendet den Foreach-Datei-Enumerator, und der Dateisystemtask ist so konfiguriert, dass eine Datei kopiert wird. Falls der vom Enumerator angegebene Ordner vier Dateien enthält, wird die Schleife viermal wiederholt, und die vier Dateien werden kopiert.  
  
 ![Foreach-Schleifencontainer, der einen Ordner aufzählt](../../integration-services/control-flow/media/ssis-foreachloop.gif "Foreach-Schleifencontainer, der einen Ordner aufzählt")  
  
 Sie können eine Kombination aus Variablen und Eigenschaftsausdrücken verwenden, um die Paketobjekteigenschaft mit dem Wert aus der Enumeratorsammlung zu aktualisieren. Zunächst ordnen Sie den Sammlungswert einer benutzerdefinierten Variablen zu. Anschließend implementieren Sie einen Eigenschaftsausdruck für die Eigenschaft, die die Variable verwendet. Beispielsweise wird der Sammlungswert des Foreach-Datei-Enumerators der Variablen **MyFile** zugeordnet. Diese Variable wird dann im Eigenschaftsausdruck für die Subject-Eigenschaft des Tasks „Mail senden“ verwendet. Beim Ausführen des Pakets wird die Subject-Eigenschaft bei jeder Wiederholung der Schleife mit dem Namen einer Datei aktualisiert. Weitere Informationen finden Sie unter [Verwenden von Eigenschaftsausdrücken in Paketen](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
 Variablen, die dem Enumeratorsammlungswert zugeordnet sind, können auch in Ausdrücken und Skripts verwendet werden.  
  
 Ein Foreach-Schleifencontainer kann mehrere Tasks und Container umfassen, aber nur einen Enumeratortyp verwenden. Falls der Foreach-Schleifencontainer mehrere Tasks umfasst, können Sie den Enumeratorsammlungswert mehreren Eigenschaften jedes Tasks zuordnen.  
  
 Sie können ein Transaktionsattribut für den Foreach-Schleifencontainer festlegen, um eine Transaktion für eine Teilmenge der Paketablaufsteuerung zu definieren. Auf diese Weise können Sie Transaktionen statt auf der Paketebene auf der Ebene der Foreach-Schleife verwalten. Wenn z. B. ein Foreach-Schleifencontainer eine Ablaufsteuerung wiederholt, die Dimensions- und Faktentabellen in einem Sternschema aktualisiert, können Sie eine Transaktion konfigurieren, um sicherzustellen, dass entweder alle oder überhaupt keine Faktentabellen aktualisiert werden. Weitere Informationen finden Sie unter [Integration Services-Transaktionen](../../integration-services/integration-services-transactions.md).  
  
## <a name="enumerator-types"></a>Enumeratortypen  
 Enumeratoren sind konfigurierbar, und je nach Enumerator müssen Sie unterschiedliche Informationen angeben.  
  
 In der folgenden Tabelle sind die Informationen zusammengefasst, die für die einzelnen Enumeratortypen erforderlich sind.  
  
|Enumerator|Konfigurationsanforderungen|  
|----------------|--------------------------------|  
|Foreach-ADO-Enumerator|Geben Sie die ADO-Objektquellvariable und den Enumeratormodus an. Die Variable muss vom Datentyp „Object“ sein.|  
|Enumerator für Foreach-ADO.NET-Schemarowset|Geben Sie die Verbindung mit einer Datenbank und das aufzuzählende Schema an.|  
|Foreach-Datei-Enumerator|Geben Sie einen Ordner sowie die aufzuzählenden Dateien und das Format des Dateinamens der abgerufenen Dateien an. Geben Sie ferner an, ob Unterordner durchlaufen werden sollen.|  
|Foreach-Enumerator für Daten aus Variable|Geben Sie die Variable an, die die aufzuzählenden Objekte enthält.|  
|Foreach-Element-Enumerator|Definieren Sie die Elemente in der Foreach-Element-Sammlung, einschließlich der Spalten und Spaltendatentypen.|  
|Foreach-NodeList-Enumerator|Geben Sie die Quelle des XML-Dokuments an, und konfigurieren Sie den XPath-Vorgang.|  
|Foreach-SMO-Enumerator|Geben Sie die Verbindung mit einer Datenbank und die aufzuzählenden SMO-Objekte an.|  
|Foreach-HDFS-Datei-Enumerator|Geben Sie einen Ordner sowie die aufzuzählenden Dateien und das Format des Dateinamens der abgerufenen Dateien an. Geben Sie ferner an, ob Unterordner durchlaufen werden sollen.|  
|Foreach-Azure-Blob-Enumerator|Geben Sie den Azure-Blobcontainer mit den aufzuzählenden Blobs an.|  
|Foreach-ADLS-Datei|Geben Sie das Azure Data Lake Store-Verzeichnis an, das die aufzuzählenden Dateien enthält.|
|Foreach-Data Lake Storage Gen2-Datei|Geben Sie das Azure Data Lake Storage Gen2-Verzeichnis zusammen mit anderen Optionen an, das die aufzuzählenden Dateien enthält.|

## <a name="add-enumeration-to-a-control-flow-with-a-foreach-loop-container"></a>Hinzufügen einer Enumeration zu einer Ablaufsteuerung mit einem Foreach-Schleifencontainer
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] umfasst den Foreach-Schleifencontainer. Dabei handelt es sich um ein Ablaufsteuerungselement, mit dem Sie auf einfache Weise eine Schleifenkonstruktion zum Aufzählen der Dateien und Objekte in der Ablaufsteuerung eines Pakets einbinden können. Weitere Informationen finden Sie unter [Foreach-Schleifencontainer](../../integration-services/control-flow/foreach-loop-container.md).  
  
 Der Foreach-Schleifencontainer bietet keine Funktionalität, sondern stellt lediglich eine Struktur bereit, in der Sie eine wiederholbare Ablaufsteuerung erstellen, einen Enumeratortyp angeben und den Enumerator konfigurieren. Sie müssen mindestens einen Task in den Foreach-Schleifencontainer einschließen, um eine Containerfunktionalität bereitzustellen. Weitere Informationen finden Sie unter [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md).  
  
 Der Foreach-Schleifencontainer kann eine Ablaufsteuerung mit mehreren Tasks und weiteren Containern einschließen. Das Hinzufügen von Tasks und Containern zu einem Foreach-Schleifencontainer ähnelt dem Hinzufügen von Tasks und Containern zu einem Paket, nur ziehen Sie die Tasks und Container nicht auf das Paket, sondern auf den Foreach-Schleifencontainer. Falls der Foreach-Schleifencontainer mehrere Tasks oder Container umfasst, können Sie diese wie bei einem Paket mithilfe von Rangfolgeneinschränkungen verbinden. Weitere Informationen finden Sie unter [Rangfolgeneinschränkungen](../../integration-services/control-flow/precedence-constraints.md).  
  
### <a name="add-and-configure-a-foreach-loop-container"></a>Hinzufügen und Konfigurieren eines Foreach-Schleifencontainers
  
1.  Fügen Sie den Foreach-Schleifencontainer zum Paket hinzu. Weitere Informationen hierzu finden Sie unter [Hinzufügen oder Löschen eines Tasks oder Containers in einer Ablaufsteuerung](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
2.  Fügen Sie dem Foreach-Schleifencontainer Tasks und Container hinzu. Weitere Informationen hierzu finden Sie unter [Hinzufügen oder Löschen eines Tasks oder Containers in einer Ablaufsteuerung](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
3.  Verbinden Sie Tasks und Container im Foreach-Schleifencontainer mithilfe von Rangfolgeneinschränkungen. Weitere Informationen finden Sie unter [Verbinden von Tasks und Containern mithilfe einer Standardrangfolgeneinschränkung](./precedence-constraints.md).  
  
4.  Konfigurieren Sie den Foreach-Schleifencontainer. Weitere Informationen finden Sie unter [Konfigurieren eines Foreach-Schleifencontainers]().  

## <a name="configure-a-foreach-loop-container"></a>Konfigurieren eines Foreach-Schleifencontainers
In diesem Verfahren wird das Konfigurieren eines Foreach-Schleifencontainers beschrieben, einschließlich der Eigenschaftsausdrücke auf Enumerator- und Containerebene.  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt mit dem gewünschten Paket.  
  
2.  Klicken Sie auf die Registerkarte **Ablaufsteuerung**, und doppelklicken Sie auf die Foreach-Schleife.  
  
3.  Klicken Sie im Dialogfeld **Foreach-Schleifen-Editor** auf **Allgemein**, und ändern Sie optional den Namen und die Beschreibung der Foreach-Schleife.  
  
4.  Klicken Sie auf **Sammlung**, und wählen Sie aus der Liste **Enumerator** einen Enumeratortyp aus.  
  
5.  Geben Sie einen Enumerator an, und legen Sie die Enumeratoroptionen wie folgt fest:  
  
    -   Zum Verwenden des Foreach-Datei-Enumerators geben Sie den Ordner an, der die aufzuzählenden Dateien enthält. Geben Sie einen Filter für den Dateinamen und -typ an, und legen Sie fest, ob der vollqualifizierte Dateiname zurückgegeben werden soll. Bestimmen Sie außerdem, ob Unterordner nach weiteren Dateien durchsucht werden sollen.  
  
    -   Wenn Sie den Foreach-Element-Enumerator verwenden möchten, klicken Sie auf **Spalten**, und klicken Sie im Dialogfeld **For Each Item-Spalten** auf **Hinzufügen**, um Spalten hinzuzufügen. Wählen Sie in der Liste **Datentyp** einen Datentyp für jede Spalte aus, und klicken Sie auf **OK**.  
  
         Geben Sie Werte in die Spalten ein, oder wählen Sie Werte in Listen aus.  
  
        > [!NOTE]  
        >  Klicken Sie an einer beliebigen Stelle außerhalb der Zelle, in die Sie Werte eingegeben haben, um eine neue Zeile hinzuzufügen.  
  
        > [!NOTE]  
        >  Falls ein Wert nicht mit dem Spaltendatentyp kompatibel ist, wird der Text hervorgehoben dargestellt.  
  
    -   Wenn Sie den Foreach-ADO-Enumerator verwenden möchten, wählen Sie eine vorhandene Variable aus, oder Sie klicken in der Liste **ADO-Objektquellvariable** auf **Neue Variable**, um die Variable anzugeben, die den Namen des aufzuzählenden ADO-Objekts enthält, und wählen dann eine Option für den Enumerationsmodus aus.  
  
         Wenn Sie eine neue Variable erstellen, legen Sie die Variableneigenschaften im Dialogfeld **Variable hinzufügen** fest.  
  
    -   Wenn Sie den Enumerator für Foreach-ADO.NET-Schemarowsets verwenden möchten, wählen Sie eine vorhandene ADO.NET-Verbindung aus, oder klicken Sie in der Liste **Verbindung** auf **Neue Verbindung**, und wählen Sie ein Schema aus.  
  
         Klicken Sie optional auf **Einschränkungen festlegen**, und wählen Sie Schemaeinschränkungen aus, wählen Sie die Variable aus, die den Einschränkungswert enthält, oder geben Sie den Einschränkungswert ein, und klicken Sie auf **OK**.  
  
    -   Wenn Sie einen Foreach-Enumerator für Daten aus Variablen verwenden möchten, wählen Sie aus der Liste **Variable** eine Variable aus.  
  
    -   Um den Foreach-NodeList-Enumerator zu verwenden, klicken Sie auf „DocumentSourceType“ und wählen den Quelltyp aus der Liste aus. Anschließend klicken Sie auf „DocumentSource“. Je nach dem für „DocumentSourceType“ ausgewählten Wert wählen Sie eine Variable bzw. eine Dateiverbindung aus der Liste aus, erstellen eine neue Variable bzw. Dateiverbindung oder geben die XML-Quelle in den **Dokumentquellen-Editor** ein.  
  
         Klicken Sie anschließend auf „EnumerationType“, und wählen Sie einen Enumerationstyp aus der Liste aus. Wenn „EnumerationType“ den Wert **Navigator, Node oder NodeText** hat, klicken Sie auf „OuterXPathStringSourceType“, wählen den Quelltyp aus und klicken dann auf „OuterXPathString“. Je nach dem für „OuterXPathStringSourceType“ festgelegten Wert wählen Sie eine Variable bzw. eine Dateiverbindung aus der Liste aus, erstellen eine neue Variable bzw. Dateiverbindung oder geben die Zeichenfolge für den äußeren XPath-Ausdruck (XML Path Language) ein.  
  
         Wenn „EnumerationType“ den Wert **ElementCollection** hat, legen Sie „OuterXPathStringSourceType“ und „OuterXPathString“ wie oben beschrieben fest. Klicken Sie anschließend auf „InnerElementType“, und wählen Sie einen Enumerationstyp für die inneren Elemente aus. Klicken Sie dann auf „InnerXPathStringSourceType“. Je nachdem, welchen Wert Sie für „InnerXPathStringSourceType“ festgelegt haben, wählen Sie eine Variable bzw. eine Dateiverbindung aus, erstellen eine neue Variable bzw. Dateiverbindung oder geben die Zeichenfolge für den inneren XPath-Ausdruck ein.  
  
    -   Wenn Sie den Foreach-SMO-Enumerator verwenden möchten, wählen Sie eine vorhandene ADO.NET-Verbindung aus oder klicken in der Liste **Verbindung** auf **Neue Verbindung**. Dann geben Sie die zu verwendende Zeichenfolge ein oder klicken auf **Durchsuchen**. **Dadurch** haben Sie im Dialogfeld **SMO-Enumeration auswählen** die Möglichkeit, den aufzuzählenden Objekttyp und den Enumerationstyp auszuwählen. Klicken Sie dann auf **OK**.  
  
6.  Klicken Sie optional im Textfeld **Ausdrücke** auf der Seite **Sammlung** auf die Schaltfläche mit der Ellipse **(…)** , um Ausdrücke zu erstellen, mit denen Eigenschaftswerte aktualisiert werden. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern eines Eigenschaftsausdrucks](../../integration-services/expressions/add-or-change-a-property-expression.md).  
  
    > [!NOTE]  
    >  Die in der Liste **Eigenschaft** aufgeführten Eigenschaften hängen vom Enumerator ab.  
  
7.  Klicken Sie optional auf **Variablenzuordnungen**, um Objekteigenschaften dem Sammlungswert zuzuordnen, und führen Sie dann folgende Aktionen aus:  
  
    1.  Wählen Sie in der Liste **Variablen** eine Variable aus, oder klicken Sie auf **\<New Variable>** , um eine neue Variable zu erstellen.  
  
    2.  Wenn Sie eine neue Variable hinzufügen, legen Sie die Variableneigenschaften im Dialogfeld **Variable hinzufügen** fest, und klicken Sie auf **OK**.  
  
    3.  Wenn Sie den Foreach-Element-Enumerator verwenden, können Sie den Indexwert in der Liste **Index** aktualisieren.  
  
        > [!NOTE]  
        >  Der Indexwert zeigt an, welche Spalte im Element der Variablen zugeordnet werden soll. Nur der Foreach-Element-Enumerator kann einen anderen Indexwert als 0 verwenden.  
  
8.  Klicken Sie optional auf **Ausdrücke**, und erstellen Sie auf der Seite **Ausdrücke** Eigenschaftsausdrücke für die Eigenschaften des Foreach-Schleifencontainers. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern eines Eigenschaftsausdrucks](../../integration-services/expressions/add-or-change-a-property-expression.md).  
  
9. Klicken Sie auf **OK**.  

## <a name="general-page---foreach-loop-editor"></a>Seite „Allgemein“ des Foreach-Schleifen-Editors
Auf der Seite **Allgemein** des Dialogfelds **Foreach-Schleifen-Editor** können Sie Namen und Beschreibung für einen Foreach-Schleifencontainer angeben, der mithilfe eines festgelegten Enumerators den gleichen Workflow für alle Elemente einer Sammlung wiederholt.  
  
 Weitere Informationen zu Foreach-Schleifencontainern und ihrer Konfiguration finden Sie unter [Foreach-Schleifencontainer](../../integration-services/control-flow/foreach-loop-container.md) und [Konfigurieren eines Foreach-Schleifencontainers]().  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den Foreach-Schleifencontainer an. Dieser Name wird als Bezeichnung des Tasksymbols und in Protokollen verwendet.  
  
> [!NOTE]  
>  Objektnamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung für den Foreach-Schleifencontainer an.  

## <a name="collection-page---foreach-loop-editor"></a>Seite „Sammlung“ des Foreach-Schleifen-Editors
 Auf der Seite **Sammlung** des Dialogfelds **Foreach-Schleifen-Editor** können Sie den Enumeratortyp angeben und den Enumerator konfigurieren.  
  
 Weitere Informationen zu Foreach-Schleifencontainern und ihrer Konfiguration finden Sie unter [Foreach-Schleifencontainer](../../integration-services/control-flow/foreach-loop-container.md) und [Konfigurieren eines Foreach-Schleifencontainers]().  
  
### <a name="static-options"></a>Statische Optionen  
 **Enumerator**  
 Wählen Sie den Enumeratortyp aus der Liste aus. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Foreach-Datei-Enumerator**|Zählt Dateien auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-Datei-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-Element-Enumerator**|Zählt Werte in einem Element auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-Element-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-ADO-Enumerator**|Zählt Tabellen oder Zeilen in Tabellen auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-ADO-Enumerator** die dynamischen Optionen angezeigt.|  
|**Enumerator für Foreach-ADO.NET-Schemarowset**|Zählt ein Schema auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-ADO-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-Enumerator für Daten aus Variable**|Zählt den Wert in einer Variablen auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-Enumerator für Daten aus Variable** die dynamischen Optionen angezeigt.|  
|**Foreach-NodeList-Enumerator**|Zählt Knoten in einem XML-Dokument auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-NodeList-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-SMO-Enumerator**|Zählt ein SMO-Objekt auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-SMO-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-HDFS-Datei-Enumerator**|Zählt HDFS-Dateien am angegebenen HDFS-Speicherort auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-HDFS-Datei-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-Azure-Blob-Enumerator**|Zählt Blobdateien am angegebenen Blobspeicherort auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-Azure-Blob-Enumerator** die dynamischen Optionen angezeigt.|  
|**Foreach-ADLS-Datei-Enumerator**|Zählt Dateien im angegebenen Data Lake Store-Verzeichnis auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-ADLS-Datei-Enumerator** die dynamischen Optionen angezeigt.|
|**Foreach-Data Lake Storage Gen2-Datei-Enumerator**|Zählt Dateien im angegebenen Data Lake Storage Gen2-Verzeichnis auf. Wenn Sie diesen Wert auswählen, werden im Abschnitt **Foreach-Data Lake Storage Gen2-Datei-Enumerator** die dynamischen Optionen angezeigt.|
  
 **Ausdrücke**  
 Klicken Sie auf die Option **Ausdrücke**, oder erweitern Sie diese, um die Liste der vorhandenen Eigenschaftsausdrücke anzuzeigen. Klicken Sie auf die Schaltfläche mit der Ellipse **(…)** , um einen Eigenschaftsausdruck für eine Enumeratoreigenschaft hinzuzufügen oder einen vorhandenen Eigenschaftsausdruck zu bearbeiten und auszuwerten.  
  
 **Verwandte Themen:**  [Integration Services-Ausdrücke &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md), [Eigenschaftsausdrucks-Editor](../../integration-services/expressions/property-expressions-editor.md), [Ausdrucks-Generator](../../integration-services/expressions/expression-builder.md)  
  
### <a name="enumerator-dynamic-options"></a>Dynamische Enumeratoroptionen  
  
#### <a name="enumerator--foreach-file-enumerator"></a>Enumerator = Foreach-Datei-Enumerator  
 Mithilfe des Foreach-Datei-Enumerators können Dateien in einem Ordner aufgezählt werden. Wenn die Foreach-Schleife z. B. einen Task „SQL ausführen“ enthält, können Sie mithilfe des Foreach-Datei-Enumerators Dateien aufzählen, die vom Task „SQL ausführen“ ausgeführte SQL-Anweisungen enthalten. Der Enumerator kann so konfiguriert werden, dass Unterordner berücksichtigt werden.  
  
 Der Inhalt der Ordner und Unterordner, die der Foreach-Datei-Enumerator aufzählt, ändert sich möglicherweise beim Durchlaufen der Schleife, da externe Prozesse oder Tasks in der Schleife beim Durchlaufen der Schleife Dateien hinzufügen, umbenennen oder löschen. Diese Änderungen können zu unerwarteten Ergebnissen führen:  
  
-   Wenn Dateien gelöscht werden, beeinflussen die Aktionen einer Aufgabe in der Foreach-Schleife möglicherweise andere Dateien als diejenigen, die von nachfolgenden Aufgaben genutzt werden.  
  
-   Wenn Dateien umbenannt werden und ein externer Prozess automatisch Dateien hinzufügt, um die umbenannten Dateien zu ersetzen, verarbeitet die Foreach-Schleife dieselben Dateien möglicherweise zweimal.  
  
-   Werden Dateien hinzugefügt, dann können die von der Foreach-Schleife beeinflussten Dateien schwer zu erkennen sein.  
  
 **Ordner**  
 Gibt den Pfad für den Stammordner für die Enumeration an.  
  
 **Durchsuchen**  
 Mit dieser Option können Sie zum Stammordner navigieren.  
  
 **Dateien**  
 Hier geben Sie die aufzuzählenden Dateien an.  
  
> [!NOTE]  
>  Sie können Platzhalterzeichen (*) verwenden, um die in die Sammlung einzuschließenden Dateien anzugeben. Verwenden Sie zum Einbeziehen von Dateien, deren Name „abc“ enthält, beispielsweise den folgenden Filter: \*abc\*.  
>   
>  Wenn Sie eine Dateierweiterung angeben, gibt der Enumerator auch Dateien mit derselben Erweiterung mit angehängten zusätzlichen Zeichen zurück. (Dieses Verhalten entspricht dem Verhalten des **dir**-Befehls im Betriebssystem, mit dem 8.3-Dateinamen auf Abwärtskompatibilität überprüft werden.) Dieses Verhalten des Enumerators könnte unerwartete Ergebnisse verursachen. Angenommen, Sie möchten nur Excel 2003-Dateien auflisten und geben „.xls“ an. Der Enumerator gibt in diesem Fall auch Excel 2007-Dateien zurück, da sie die Erweiterung „.xlsx“ haben.  
>   
>  Sie können einen Ausdruck verwenden, um die in eine Sammlung einzubeziehenden Dateien anzugeben. Erweitern Sie dazu auf der Seite **Sammlung** die Option **Ausdrücke**, wählen Sie die **FileSpec**-Eigenschaft aus, und klicken Sie anschließend auf die Schaltfläche mit der Ellipse („…“), um den Eigenschaftsausdruck hinzuzufügen.  
  
 **Vollqualifiziert**  
 Wählen Sie diese Option aus, um den vollqualifizierten Pfad von Dateinamen abzurufen. Wenn in der Dateioption Platzhalterzeichen angegeben wurden, stimmen die zurückgegebenen vollqualifizierten Pfade mit dem Filter überein.  
  
 **Nur Name**  
 Wählen Sie diese Option aus, um nur die Dateinamen abzurufen. Wenn in der Dateioption Platzhalterzeichen angegeben wurden, stimmen die zurückgegebenen Dateinamen mit dem Filter überein.  
  
 **Name und Erweiterung**  
 Wählen Sie diese Option aus, um die Dateinamen und die Dateinamenerweiterungen abzurufen. Wenn in der Dateioption Platzhalterzeichen angegeben wurden, stimmen die zurückgegebenen Dateinamen und Dateierweiterungen mit dem Filter überein.  
  
 **Unterordner durchlaufen**  
 Wählen Sie diese Option aus, wenn die Unterordner in der Enumeration berücksichtigt werden sollen.  
  
#### <a name="enumerator--foreach-item-enumerator"></a>Enumerator = Foreach-Element-Enumerator  
 Mithilfe des Foreach-Element-Enumerators können Elemente in einer Sammlung aufgezählt werden. Sie definieren die Elemente in der Sammlung, indem Sie Spalten und Spaltenwerte angeben. Ein Element wird durch die Spalten in einer Zeile definiert. Ein Element, das die von einem Task „Prozess ausführen“ ausgeführten ausführbaren Dateien sowie das vom Task verwendete Arbeitsverzeichnis angibt, verfügt über zwei Spalten. Eine Spalte listet die Namen der ausführbaren Dateien auf, die andere das Arbeitsverzeichnis. Die Zeilenanzahl bestimmt, wie oft die Schleife wiederholt wird. Wenn die Tabelle also zehn Zeilen aufweist, wird die Schleife zehn Mal wiederholt.  
  
 Um die Eigenschaften des Tasks „Prozess ausführen“ zu aktualisieren, ordnen Sie Elementspalten mithilfe des Spaltenindex Variablen zu. Die erste im Enumeratorelement definierte Spalte hat den Indexwert 0, die zweite den Wert 1 usw. Die Variablenwerte werden bei jeder Wiederholung der Schleife aktualisiert. Die Eigenschaften **Executable** und **WorkingDirectory** des Tasks „Prozess ausführen“ können dann mithilfe von Eigenschaftsausdrücken, die diese Variablen verwenden, aktualisiert werden.  
  
 **Elemente für die ForEach-Elementsammlung definieren**  
 Geben Sie einen Wert für jede Spalte in der Tabelle an.  
  
> [!NOTE]  
>  Sobald Sie die Werte für die Spalten in die Zeile eingegeben haben, wird der Tabelle automatisch eine neue Zeile hinzugefügt.  
  
> [!NOTE]  
>  Wenn die angegebenen Werte nicht mit dem Spaltendatentyp kompatibel sind, wird der Text rot angezeigt.  
  
 **Datentyp der Spalte**  
 Führt den Datentyp der aktiven Spalte auf.  
  
 **Remove**  
 Wählen Sie ein Element aus, und klicken Sie anschließend auf **Entfernen**, um es aus der Liste zu entfernen.  
  
 **Spalten**  
 Klicken Sie auf diese Option, um den Datentyp der Spalten im Element zu konfigurieren.  
  
 **Verwandte Themen:** [Foreach-Elementspalten (Dialogfeld, Referenz zur Benutzeroberfläche)]()  
  
#### <a name="enumerator--foreach-ado-enumerator"></a>Enumerator = Foreach-ADO-Enumerator  
 Mithilfe des Foreach-ADO-Enumerators werden Zeilen oder Tabellen in einem in einer Variablen gespeicherten ADO- oder ADO.NET-Objekt aufgezählt. Wenn die Foreach-Schleife z. B. einen Skripttask enthält, mit dem ein Dataset in eine Variable geschrieben wird, können Sie mithilfe des Foreach-ADO-Enumerators die Zeilen im Dataset aufzählen. Enthält die Variable ein ADO.NET-Dataset, dann kann der Enumerator zum Aufzählen von Zeilen in mehreren Tabellen oder zum Aufzählen von Tabellen konfiguriert werden.  
  
 **ADO-Objektquellvariable**  
 Wählen Sie eine benutzerdefinierte Variable aus der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
> [!NOTE]  
>  Der Datentyp der Variablen muss „Object“ sein, andernfalls tritt ein Fehler auf.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
 **Zeilen in der ersten Tabelle**  
 Wählen Sie diese Option aus, um nur Zeilen in der ersten Tabelle aufzuzählen.  
  
 **Zeilen in allen Tabellen (nur ADO.NET-Dataset)**  
 Wählen Sie diese Option aus, um Zeilen in allen Tabellen aufzuzählen. Diese Option ist nur verfügbar, wenn die aufzuzählenden Objekte alle Elemente desselben ADO.NET-Datasets sind.  
  
 **Alle Tabellen (nur ADO.NET-Dataset)**  
 Wählen Sie diese Option aus, um nur Tabellen aufzuzählen.  
  
#### <a name="enumerator--foreach-adonet-schema-rowset-enumerator"></a>Enumerator = Enumerator für Foreach-ADO.NET-Schemarowset  
 Mithilfe des Enumerators für Foreach ADO.NET-Schemarowset kann ein Schema für eine angegebene Datenquelle aufgezählt werden. Wenn die Foreach-Schleife z. B. einen Task „SQL ausführen“ enthält, können Sie mit dem Enumerator für Foreach ADO.NET-Schemarowset Schemas aufzählen (beispielsweise die Spalten in der **AdventureWorks**-Datenbank) und mit dem Task „SQL ausführen“ Schemaberechtigungen abrufen.  
  
 **Connection**  
 Wählen Sie einen ADO.NET-Verbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
> [!IMPORTANT]  
>  Der ADO.NET-Verbindungs-Manager muss einen .NET-Anbieter für OLE DB verwenden. Wenn Sie eine Verbindung mit SQL Server herstellen, ist der empfohlene Anbieter der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, der im Dialogfeld **Verbindungs-Manager** im Abschnitt **.NET-Anbieter für OleDb** aufgeführt ist.  
  
 **Verwandte Themen:** [ADO-Verbindungs-Manager](../../integration-services/connection-manager/ado-connection-manager.md), [Konfigurieren von ADO.NET-Verbindungs-Manager](../connection-manager/ado-net-connection-manager.md)  
  
 **Schema**  
 Hiermit wählen Sie das aufzuzählende Schema aus.  
  
 **Einschränkungen festlegen**  
 Hiermit legen Sie die Einschränkungen fest, die auf das angegebene Schema angewendet werden sollen.  
  
 **Verwandte Themen:** [Schemaeinschränkungen (Dialogfeld)]()  
  
#### <a name="enumerator--foreach-from-variable-enumerator"></a>Enumerator = Foreach-Enumerator für Daten aus Variable  
 Mithilfe des Foreach-Enumerators für Daten aus Variablen werden aufzählbare Objekte in einer angegebenen Variable aufgezählt. Wenn die Foreach-Schleife z. B. einen Task „SQL ausführen“ enthält, der eine Abfrage ausführt und das Ergebnis in einer Variablen speichert, können Sie den Foreach-Enumerator für Daten aus Variablen zum Aufzählen der Abfrageergebnisse verwenden.  
  
 **Variable**  
 Wählen Sie eine Variable aus der Liste aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
#### <a name="enumerator--foreach-nodelist-enumerator"></a>Enumerator = Foreach-NodeList-Enumerator  
 Mithilfe des Foreach-NodeList-Enumerators werden alle XML-Knoten aufgezählt, die zum Ergebnis der Anwendung eines XPath-Ausdrucks auf eine XML-Datei gehören. Wenn die Foreach-Schleife beispielsweise einen Skripttask enthält, können Sie mit dem Foreach-NodeList-Enumerator einen Wert, der den Kriterien des XPath-Ausdrucks entspricht, aus der XML-Datei an den Skripttask übergeben.  
  
 Der XPath-Ausdruck, der auf die XML-Datei angewendet wird, ist der in der OuterXPathString-Eigenschaft gespeicherte äußere XPath-Vorgang. Wenn der XPath-Enumerationstyp auf **ElementCollection** festgelegt ist, kann der Foreach-NodeList-Enumerator einen in der InnerXPathString-Eigenschaft gespeicherten inneren XPath-Ausdruck auf eine Sammlung von Elementen anwenden.  
  
 Weitere Informationen zum Arbeiten mit XML-Dokumenten und -Daten finden Sie unter "[XML im .NET Framework](/previous-versions/aa720019(v=vs.71))" in der MSDN Library.  
  
 **DocumentSourceType**  
 Hiermit wählen Sie den Quelltyp des XML-Dokuments aus. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **DocumentSource**  
 Wenn für **DocumentSourceType** die Option **Direkteingabe** festgelegt ist, geben Sie den XML-Code an, oder klicken Sie auf die Schaltfläche mit der Ellipse („…“), um im Dialogfeld **Dokumentquellen-Editor** den XML-Code anzugeben.  
  
 Wenn **DocumentSourceType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **DocumentSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **EnumerationType**  
 Hiermit wählen Sie einen Enumerationstyp aus der Liste aus. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Navigator**|Die Enumeration erfolgt mithilfe eines XPathNavigator.|  
|**Node**|Zählt Knoten auf, die von einem XPath-Vorgang zurückgegeben wurden.|  
|**NodeText**|Zählt Textknoten auf, die von einem XPath-Vorgang zurückgegeben wurden.|  
|**ElementCollection**|Zählt Elementknoten auf, die von einem XPath-Vorgang zurückgegeben wurden.|  
  
 **OuterXPathStringSourceType**  
 Hiermit wählen Sie den Quelltyp der XPath-Zeichenfolge aus. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar: 
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **OuterXPathString**  
 Wenn **OuterXPathStringSourceType** auf **Direkteingabe** festgelegt ist, geben Sie die XPath-Zeichenfolge an.  
  
 Wenn **OuterXPathStringSourceType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **OuterXPathStringSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
 **InnerElementType**  
 Wenn **EnumerationType** auf **ElementCollection** festgelegt ist, wählen Sie den Typ des inneren Elements in der Liste aus.  
  
 **InnerXPathStringSourceType**  
 Hiermit wählen Sie den Quelltyp der inneren XPath-Zeichenfolge aus. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direkteingabe**|Legt ein XML-Dokument als Quelle fest.|  
|**Dateiverbindung**|Wählt eine Datei aus, die das XML-Dokument enthält.|  
|**Variable**|Legt als Quelle eine Variable fest, die das XML-Dokument enthält.|  
  
 **InnerXPathString**  
 Wenn **InnerXPathStringSourceType** auf **Direkteingabe** festgelegt ist, geben Sie die XPath-Zeichenfolge an.  
  
 Wenn **InnerXPathStringSourceType** auf **Dateiverbindung** festgelegt ist, wählen Sie einen Dateiverbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md), [Dateiverbindungs-Manager-Editor](../connection-manager/file-connection-manager.md)  
  
 Wenn **InnerXPathStringSourceType** auf **Variable** festgelegt ist, wählen Sie eine vorhandene Variable aus, oder klicken Sie auf \<**New variable...**>, um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md).  
  
#### <a name="enumerator--foreach-smo-enumerator"></a>Enumerator = Foreach-SMO-Enumerator  
 Mithilfe des Foreach-SMO-Enumerators werden SMO-Objekte (SQL Server Management Object) aufgezählt. Wenn die Foreach-Schleife z. B. einen Task „SQL ausführen“ enthält, können Sie den Foreach-SMO-Enumerator zum Aufzählen der Tabellen in der **AdventureWorks**-Datenbank und zum Ausführen von Abfragen verwenden, mit denen die Anzahl von Zeilen pro Tabelle ermittelt wird.  
  
 **Connection**  
 Wählen Sie einen vorhandenen ADO.NET-Verbindungs-Manager aus, oder klicken Sie auf \<**New connection...**>, um einen neuen Verbindungs-Manager zu erstellen.  
  
 Verwandte Themen: [ADONET-Verbindungs-Manager](../../integration-services/connection-manager/ado-net-connection-manager.md), [Konfigurieren von ADO.NET-Verbindungs-Manager](../connection-manager/ado-net-connection-manager.md)  
  
 **Aufzählen**  
 Hier geben Sie das aufzuzählende SMO-Objekt an.  
  
 **Durchsuchen**  
 Hiermit wählen Sie die SMO-Enumeration aus.  
  
 **Verwandte Themen:** [Das Dialogfeld „SMO-Enumeration auswählen“]()  
  
####  <a name="enumerator--foreach-hdfs-file-enumerator"></a><a name="ForeachHDFSFile"></a> Enumerator = Foreach-HDFS-Datei-Enumerator  
 Der **Foreach-HDFS-Datei-Enumerator** ermöglicht einem SSIS-Paket das Aufzählen von HDFS-Dateien am angegebenen HDFS-Speicherort. Der Name jeder HDFS-Datei kann in einer Variablen gespeichert und in Tasks innerhalb des Foreach-Schleifencontainers verwendet werden.  
  
 **Hadoop-Verbindungs-Manager**  
 Geben Sie einen vorhandenen Hadoop-Verbindungs-Manager an, oder erstellen Sie einen neuen Hadoop-Verbindungs-Manager, der angibt, wo die HDFS-Dateien gehostet werden. Weitere Informationen finden Sie unter [Hadoop-Verbindungs-Manager](../../integration-services/connection-manager/hadoop-connection-manager.md).  
  
 **Verzeichnispfad**  
 Hier geben Sie den Namen des HDFS-Verzeichnisses an, das die aufzuzählenden HDFS-Dateien enthält.  
  
 **Dateinamensfilter**  
 Hier geben Sie einen Namensfilter zum Auswählen von Dateien mit einem bestimmten Namensmuster an. Zum Beispiel schließt „MySheet*.xls\*“ Dateien wie „MySheet001.xls“ und „MySheetABC.xlsx“ ein.  
  
 **Dateinamen abrufen**  
 Hier geben Sie den Typ der von SSIS abgerufenen Dateinamen an.  
  
-   **Vollqualifizierter Name** bezeichnet den vollständigen Namen, der den Verzeichnispfad und den Dateinamen enthält.  
  
-   **Nur Name** bedeutet, dass nur der Dateiname ohne Verzeichnispfad abgerufen wird.  
  
 **Unterordner durchsuchen**  
 Hiermit geben Sie an, ob Unterordner rekursiv in einer Schleife durchlaufen werden sollen.  
  
 Erstellen oder wählen Sie auf der Seite **Variablenzuordnungen** eine Variable zum Speichern des Namens der aufgezählten HDFS-Datei.  
  
####  <a name="enumerator--foreach-azure-blob-enumerator"></a><a name="ForeachAzureBlob"></a> Enumerator = Foreach-Azure-Blob-Enumerator  
 Der **Azure-Blob-Enumerator** ermöglicht einem SSIS-Paket das Aufzählen von Blobdateien am angegebenen Blobspeicherort. Sie können den Namen der aufgezählten Blobdatei in einer Variablen speichern und in Tasks innerhalb des Foreach-Schleifencontainers verwenden.  
  
 Der **Azure-Blob-Enumerator** ist eine Komponente des SQL Server Integration Services (SSIS) Feature Pack für Azure für [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]. Laden Sie das Feature Pack [hier](https://go.microsoft.com/fwlink/?LinkID=626967)herunter.  
  
 **Azure Storage-Verbindungs-Manager**  
 Wählen Sie einen vorhandenen Azure Storage-Verbindungs-Manager aus, oder erstellen Sie einen neuen, der auf ein Azure Storage-Konto verweist.  
  
 Verwandte Themen: [Azure Storage-Verbindungs-Manager](../../integration-services/connection-manager/azure-storage-connection-manager.md).  
  
 **Blobcontainername**  
 Geben Sie den Namen des Blobcontainers an, der die aufzuzählenden Blobdateien enthält.
  
 **Blobverzeichnis**  
 Geben Sie das Blobverzeichnis an, das die aufzuzählenden Blobdateien enthält. Das Blobverzeichnis ist eine virtuelle hierarchische Struktur.  
  
 **Rekursiv suchen**  
 Gibt an, ob Unterverzeichnisse rekursiv durchsucht werden sollen.

 **Blobnamensfilter**  
 Geben Sie einen Namensfilter zum Auflisten von Dateien mit einem bestimmten Namensmuster an. Zum Beispiel schließt `MySheet*.xls\*` Dateien wie „MySheet001.xls“ und „MySheetABC.xlsx“ ein.  
  
 **Blobzeitraumfilter „von/bis“**  
 Geben Sie einen Filter für den Zeitraum an. Es werden Dateien aufgezählt, die nach **TimeRangeFrom**, aber vor **TimeRangeTo** geändert wurden. 

####  <a name="enumerator--foreach-adls-file-enumerator"></a><a name="ForeachAdlsFile"></a> Enumerator = Foreach-ADLS-Datei-Enumerator 
Der **ADLS-Datei-Enumerator** ermöglicht einem SSIS-Paket das Aufzählen von Dateien in Azure Data Lake Store. Sie können den vollständigen Pfad der aufgezählten Datei (mit einem Schrägstrich `/` als Präfix) in einer Variablen speichern und den Dateipfad in Tasks im Foreach-Schleifencontainer verwenden.
  
**AzureDataLakeConnection**  
Gibt einen Azure Data Lake-Verbindungs-Manager an oder erstellt einen neuen Azure Data Lake-Verbindungs-Manager, der auf ein ADLS-Konto verweist.   
  
**AzureDataLakeDirectory**  
Gibt das ADLS-Verzeichnis an, das die aufzuzählenden Dateien enthält.
  
**FileNamePattern**  
Gibt einen Dateinamensfilter an. Nur Dateien, deren Namen dem angegebenen Muster entsprechen, werden aufgelistet. Die Platzhalterzeichen `*` und `?` werden unterstützt. 
  
**SearchRecursively**  
Gibt an, ob im angegebenen Verzeichnis rekursiv gesucht werden soll.  

####  <a name="enumerator--foreach-data-lake-storage-gen2-file-enumerator"></a><a name="ForeachBlobFsFile"></a> Enumerator = Foreach-Data Lake Storage Gen2-Datei-Enumerator 
Der **Foreach-Data Lake Storage Gen2-Datei-Enumerator** ermöglicht einem SSIS-Paket das Aufzählen von Dateien in Azure Data Lake Storage Gen2.

**AzureStorageConnection**  
Gibt einen vorhandenen Azure Storage-Verbindungs-Manager an oder erstellt einen neuen Manager, der auf einen Data Lake Storage Gen2-Dienst verweist.

**FolderPath**  
Gibt den Pfad des Ordners an, in dem Dateien aufgezählt werden sollen.

**SearchRecursively**  
Gibt an, ob im angegebenen Ordner rekursiv gesucht werden soll.

**_Hinweise zur Konfiguration der Dienstprinzipalberechtigung_* _

Die Berechtigung Data Lake Storage Gen2 wird durch die [RBAC](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal) und durch [ACLs](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer) bestimmt.
Beachten Sie, dass ACLs wie [hier](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal) beschrieben mithilfe der Objekt-ID (OID) des Dienstprinzipals für die App-Registrierung konfiguriert werden.
Dies unterscheidet sich von der Anwendungs-ID (Client-ID), die mit der RBAC-Konfiguration verwendet wird.
Wenn ein Sicherheitsprinzipal durch eine integrierte Rolle oder eine benutzerdefinierte Rolle RBAC-Datenberechtigungen erhält, werden diese Berechtigungen vor der Autorisierung einer Anforderung zunächst ausgewertet.
Wenn der Anforderungsvorgang von den RBAC-Zuweisungen des Sicherheitsprinzipals autorisiert wurde, wird die Autorisierung sofort aufgelöst, und es werden keine weiteren ACL-Prüfungen durchgeführt.
Wenn der Sicherheitsprinzipal über keine RBAC-Zuweisung verfügt oder der Vorgang der Anforderung nicht mit der zugewiesenen Berechtigung übereinstimmt, werden alternativ ACL-Prüfungen durchgeführt, um zu bestimmen, ob der Sicherheitsprinzipal für die Durchführung des angeforderten Vorgangs autorisiert ist.
Damit der Enumerator funktioniert, müssen Sie mindestens die Berechtigung _ *Ausführen** ab dem Stammdateisystem sowie die Berechtigung **Lesen** für den Zielordner gewähren.
Gewähren Sie alternativ mindestens die Rolle **Storage-Blobdatenleser** mit der RBAC.
Weitere Informationen finden Sie in [diesem Artikel](/azure/storage/blobs/data-lake-storage-access-control).

## <a name="variable-mappings-page---foreach-loop-editor"></a>Seite „Variablenzuordnungen“ im Foreach-Schleifen-Editor
 Auf der Seite **Variablenzuordnungen** des Dialogfelds **Foreach-Schleifen-Editor** können Sie dem Sammlungswert Variablen zuordnen. Der Wert der Variablen wird bei jeder Iteration der Schleife mit den Sammlungswerten aktualisiert.  
  
 Informationen zum Verwenden des Foreach-Schleifencontainers in einem Integration Services-Paket finden Sie unter [Foreach-Schleifen-Editor](../../integration-services/control-flow/foreach-loop-container.md). Informationen zum Konfigurieren dieses Containers finden Sie unter [Konfigurieren eines Foreach-Schleifencontainers]().  
  
 Das Tutorial für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] zum Erstellen eines einfachen ETL-Pakets enthält eine Lektion, die Ihnen zeigt, wie Sie eine Foreach-Schleife hinzufügen und konfigurieren können.  
  
### <a name="options"></a>Tastatur  
 **Variable**  
 Wählen Sie eine vorhandene Variable aus, oder klicken Sie auf **Neue Variable...** , um eine neue Variable zu erstellen.  
  
> [!NOTE]  
>  Nachdem Sie eine Variable zugeordnet haben, wird der Liste **Variable** automatisch eine neue Zeile hinzugefügt.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
 **Index**  
 Geben Sie bei Verwendung des Foreach-Element-Enumerators den Index der Spalte im Sammlungswert an, der der Variable zugeordnet werden soll. Bei anderen Enumeratortypen ist der Index schreibgeschützt.  
  
> [!NOTE]  
>  Der Index basiert auf 0.  
  
**Löschen**  
 Wählen Sie eine Variable aus, und klicken Sie auf **Löschen**.  

## <a name="schema-restrictions-dialog-box-adonet"></a>Das Dialogfeld „Schemaeinschränkungen“ (ADO.NET)
Im Dialogfeld **Schemaeinschränkungen** legen Sie die Schemaeinschränkungen fest, die für den Enumerator für das Foreach-ADO.NET-Schemarowset gelten.  
  
### <a name="options"></a>Tastatur  
 **Einschränkungen**  
 Wählen Sie die Einschränkungen aus, die für das Schema gelten.  
  
 **Variable**  
 Verwendet eine Variable, um die Einschränkungen zu definieren. Wählen Sie eine Variable aus der Liste aus, oder klicken Sie auf **Neue Variable…** , um eine neue Variable zu erstellen.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Hinzufügen von Variablen](../integration-services-ssis-variables.md)  
  
 **Text**  
 Geben Sie den Text an, um Einschränkungen zu definieren.  
 
## <a name="for-each-item-columns-dialog-box"></a>Das Dialogfeld „ForEach-Elementspalten“
Mithilfe des Dialogfelds **ForEach-Elementspalten** definieren Sie Spalten, die in den Elementen des ForEach-Element-Enumerators enthalten sind.  
  
### <a name="options"></a>Tastatur  
 **Spalte**  
 Listet die Spalten auf.  
  
 **Datentyp**  
 Hier wählen Sie den Datentyp aus.  
  
 **Add (Hinzufügen)**  
 Fügt eine neue Spalte hinzu.  
  
 **Remove**  
 Wählen Sie eine Spalte aus, und klicken Sie dann auf **Entfernen**.  
 
 ## <a name="select-smo-enumeration-dialog-box"></a>Das Dialogfeld „SMO-Enumeration auswählen“
Im Dialogfeld **SMO-Enumeration auswählen** geben Sie das aufzuzählende SMO-Objekt ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects) für die angegebene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz an und wählen den Enumerationstyp aus.  
  
### <a name="options"></a>Tastatur  
 **Aufzählen**  
 Hiermit erweitern Sie den Server und wählen ein SMO-Objekt aus.  
  
 **Objekte**  
 Verwenden Sie den Enumerationstyp „Objekte“.  
  
 **Vorauffüllen**  
 Verwenden Sie die Option **Vorauffüllen** mit dem Enumerationstyp „Objekte“.  
  
 **Namen**  
 Hiermit verwenden Sie den Enumerationstyp „Namen“.  
  
 **URNs**  
 Hiermit verwenden Sie den Enumerationstyp „URNs“.  
  
 **Speicherorte**  
 Hiermit verwenden Sie den Enumerationstyp „Speicherorte“. Diese Option ist nur für Dateien verfügbar.  

## <a name="use-property-expressions-with-foreach-loop-containers"></a>Verwenden von Eigenschaftsausdrücken mit Foreach-Schleifencontainern  
 Pakete können so konfiguriert werden, dass mehrere ausführbare Dateien gleichzeitig ausgeführt werden. Diese Konfiguration sollte mit Vorsicht verwendet werden, wenn das Paket einen Foreach-Schleifencontainer enthält, der Eigenschaftsausdrücke implementiert.  
  
 Es ist häufig nützlich, einen Eigenschaftsausdruck zu implementieren, um den Wert der ConnectionString-Eigenschaft des Verbindungs-Managers festzulegen, den die Foreach-Schleifenenumeratoren verwenden. Der ConnectionString-Eigenschaftsausdruck wird durch eine Variable festgelegt, die dem Sammlungswert des Enumerators zugeordnet ist und bei jeder Iteration der Schleife aktualisiert wird.  
  
 Das Paket sollte so konfiguriert sein, dass jeweils nur eine ausführbare Datei ausgeführt wird, um in der Schleife negative Auswirkungen einer unbestimmten Zeitvorgabe der parallelen Taskausführung zu vermeiden. Wenn beispielsweise ein Paket mehrere Tasks gleichzeitig ausführen kann, können bei einem Foreach-Schleifencontainer, der im Ordner vorhandene Dateien aufzählt, die Dateinamen abruft und dann mithilfe eines Tasks „SQL ausführen“ die Dateinamen in eine Tabelle einfügt, Schreibkonflikte auftreten, falls zwei Instanzen des Tasks „SQL ausführen“ gleichzeitig zu schreiben versuchen. Weitere Informationen finden Sie unter [Verwenden von Eigenschaftsausdrücken in Paketen](../../integration-services/expressions/use-property-expressions-in-packages.md).  

## <a name="see-also"></a>Weitere Informationen  
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)   
 [SQL Server Integration Services-Container](../../integration-services/control-flow/integration-services-containers.md)  
  
