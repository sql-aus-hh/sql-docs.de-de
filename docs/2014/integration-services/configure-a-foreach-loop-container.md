---
title: Konfigurieren des Foreach-Schleifencontainer | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/22/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- integration-services
ms.topic: conceptual
helpviewer_keywords:
- Foreach Loop containers
- containers [Integration Services], Foreach Loop
ms.assetid: 519c6f96-5e1f-47d2-b96a-d49946948c25
author: douglaslms
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9d63a4f168c4a426c06bb00c634f89e328735332
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48159760"
---
# <a name="configure-a-foreach-loop-container"></a>Konfigurieren eines Foreach-Schleifencontainers
  In diesem Verfahren wird das Konfigurieren eines Foreach-Schleifencontainers beschrieben, einschließlich der Eigenschaftsausdrücke auf Enumerator- und Containerebene.  
  
### <a name="to-configure-the-foreach-loop-container"></a>So konfigurieren Sie den Foreach-Schleifencontainer  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]das [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Projekt mit dem gewünschten Paket.  
  
2.  Klicken Sie auf die Registerkarte **Ablaufsteuerung** , und doppelklicken Sie auf die Foreach-Schleife.  
  
3.  Klicken Sie im Dialogfeld **Foreach-Schleifen-Editor** auf **Allgemein** , und ändern Sie optional den Namen und die Beschreibung der Foreach-Schleife.  
  
4.  Klicken Sie auf **Auflistung** , und wählen Sie aus der Liste **Enumerator** einen Enumeratortyp aus.  
  
5.  Geben Sie einen Enumerator an, und legen Sie wie folgt Enumeratoroptionen fest:  
  
    -   Wenn Sie den Foreach-Dateienumerator verwenden möchten, stellen Sie den Ordner bereit, der die aufzuzählenden Dateien enthält. Geben Sie einen Filter für den Dateinamen und den Typ an, und geben Sie an, ob der vollqualifizierte Dateiname zurückgegeben werden soll. Zeigen Sie außerdem an, ob Unterordner nach weiteren Dateien durchsucht werden sollen.  
  
    -   Wenn Sie den Foreach Item-Enumerator verwenden möchten, klicken Sie auf **Spalten**, und klicken Sie im Dialogfeld **For Each Item-Spalten** auf **Hinzufügen** , um Spalten hinzuzufügen. Wählen Sie aus der Liste **Datentyp** einen Datentyp für jede Spalte aus, und klicken Sie auf **OK**.  
  
         Geben Sie Werte in die Spalten ein, oder wählen Sie in Listen Werte aus.  
  
        > [!NOTE]  
        >  Klicken Sie an einer beliebigen Stelle außerhalb der Zelle, in die Sie Werte eingegeben haben, um eine neue Zeile hinzuzufügen.  
  
        > [!NOTE]  
        >  Falls ein Wert nicht mit dem Spaltendatentyp kompatibel ist, wird der Text hervorgehoben dargestellt.  
  
    -   Wenn Sie den Foreach-ADO-Enumerator verwenden möchten, wählen Sie eine vorhandene Variable aus, oder klicken Sie in der Liste **ADO-Objektquellvariable** auf **Neue Variable** , um die Variable anzugeben, die den Namen des aufzuzählenden ADO-Objekts enthält, und wählen Sie eine Option für den Enumerationsmodus aus.  
  
         Wenn Sie eine neue Variable erstellen, legen Sie die Variableneigenschaften im Dialogfeld **Variable hinzufügen** fest.  
  
    -   Wenn Sie den Enumerator für Foreach-ADO.NET-Schemarowset verwenden möchten, wählen Sie eine vorhandene ADO.NET-Verbindung aus, oder klicken Sie in der Liste **Verbindung** auf **Neue Verbindung** , und wählen Sie ein Schema aus.  
  
         Klicken Sie optional auf **Einschränkungen festlegen** , und wählen Sie Schemaeinschränkungen aus, wählen Sie die Variable aus, die den Einschränkungswert enthält, oder geben Sie den Einschränkungswert ein, und klicken Sie auf **OK**.  
  
    -   Wenn Sie einen Foreach-Enumerator für Daten aus Variable verwenden möchten, wählen Sie aus der Liste **Variable** eine Variable aus.  
  
    -   Um den Foreach-NodeList-Enumerator zu verwenden, klicken Sie auf DocumentSourceType und wählen den Quelltyp aus der Liste aus. Anschließend klicken Sie auf DocumentSource. Je nach dem für DocumentSourceType ausgewählten Wert wählen Sie eine Variable bzw. eine Dateiverbindung aus der Liste aus, erstellen Sie eine neue Variable bzw. Dateiverbindung, oder geben Sie die XML-Quelle in den **Dokumentquellen-Editor**ein.  
  
         Klicken Sie anschließend auf „EnumerationType“, und wählen Sie einen Enumerationstyp aus der Liste aus. Wenn EnumerationType den Wert **Navigator, Node oder NodeText**hat, klicken Sie auf OuterXPathStringSourceType, wählen Sie den Quelltyp aus, und klicken Sie dann auf OuterXPathString. Je nach dem für OuterXPathStringSourceType festgelegten Wert wählen Sie eine Variable bzw. eine Dateiverbindung aus der Liste aus, erstellen Sie eine neue Variable bzw. Dateiverbindung, oder geben Sie die Zeichenfolge für den äußeren XPath-Ausdruck (XML Path Language) ein.  
  
         Wenn EnumerationType den Wert **ElementCollection**hat, legen Sie OuterXPathStringSourceType und OuterXPathString wie oben beschrieben fest. Klicken Sie anschließend auf InnerElementType, und wählen Sie einen Enumerationstyp für die inneren Elemente aus. Klicken Sie dann auf InnerXPathStringSourceType. Je nachdem, welchen Wert Sie für InnerXPathStringSourceType festgelegt haben, wählen Sie eine Variable bzw. eine Dateiverbindung aus, erstellen Sie eine neue Variable bzw. Dateiverbindung, oder geben Sie die Zeichenfolge für den inneren XPath-Ausdruck ein.  
  
    -   Wenn Sie den Foreach-SMO-Enumerator verwenden möchten, wählen Sie eine vorhandene ADO.NET-Verbindung aus, oder klicken Sie in der Liste **Verbindung** auf **Neue Verbindung** , und geben Sie die zu verwendende Zeichenfolge ein, oder klicken Sie auf **Durchsuchen**. **Dadurch** haben Sie im Dialogfeld **SMO-Enumeration auswählen** die Möglichkeit, den aufzuzählenden Objekttyp und den Enumerationstyp auszuwählen. Klicken Sie dann auf **OK**.  
  
6.  Klicken Sie optional im Textfeld **Ausdrücke** auf der Seite **Sammlung** auf die Schaltfläche mit den drei Punkten **(…)** , um Ausdrücke zu erstellen, mit denen Eigenschaftswerte aktualisiert werden. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern eines Eigenschaftsausdrucks](expressions/add-or-change-a-property-expression.md).  
  
    > [!NOTE]  
    >  Die in der Liste **Eigenschaft** aufgeführten Eigenschaften hängen vom Enumerator ab.  
  
7.  Klicken Sie optional auf **Variablenzuordnungen** , um Objekteigenschaften dem Auflistungswert zuzuordnen, und führen Sie dann folgende Aktionen aus:  
  
    1.  Wählen Sie in der Liste **Variablen** eine Variable aus, oder klicken Sie auf **\<Neue Variable>**, um eine neue Variable zu erstellen.  
  
    2.  Wenn Sie eine neue Variable hinzufügen, legen Sie die Variableneigenschaften im Dialogfeld **Variable hinzufügen** fest, und klicken Sie auf **OK**.  
  
    3.  Wenn Sie den Foreach Item-Enumerator verwenden, können Sie den Indexwert in der Liste **Index** aktualisieren.  
  
        > [!NOTE]  
        >  Der Indexwert zeigt an, welche Spalte im Element der Variablen zugeordnet werden soll. Nur der Foreach Item-Enumerator kann einen anderen Indexwert als 0 verwenden.  
  
8.  Klicken Sie optional auf **Ausdrücke** , und erstellen Sie auf der Seite **Ausdrücke** Eigenschaftsausdrücke für die Eigenschaften des Foreach-Schleifencontainers. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern eines Eigenschaftsausdrucks](expressions/add-or-change-a-property-expression.md).  
  
9. Klicken Sie auf **OK**.  
  
## <a name="see-also"></a>Siehe auch  
 [Foreach-Schleifencontainer](control-flow/foreach-loop-container.md)  
  
  
