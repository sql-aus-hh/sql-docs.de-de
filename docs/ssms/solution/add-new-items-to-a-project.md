---
title: Hinzufügen von neuen Elementen zu einem Projekt | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- projects [SQL Server Management Studio], item additions
- adding project items
ms.assetid: 76af8692-324f-4f5e-b1a0-d72ca8a107e3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 369e132b176237a7c1318d101026ccf2663505dc
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47804168"
---
# <a name="add-new-items-to-a-project"></a>Hinzufügen neuer Elemente zu einem Projekt
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Sie können neue Elemente zu einem Projekt hinzufügen, um die Funktionalität der Anwendung zu erweitern. Bei einem neuen Element kann es sich um eine Abfrage oder eine Verbindung handeln. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] hat zwei Projekttypen: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Skriptprojekt und Analysis Services-Skriptprojekt. Der Projekttyp bestimmt die Elemente, die Sie zu einem Projekt hinzufügen können. So können Sie beispielsweise eine [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage (eine Datei mit einer SQL-Erweiterung) zu einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Skriptprojekt hinzufügen, nicht jedoch zu einem Analysis Services-Skriptprojekt.  
  
[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] gestattet es Ihnen nicht, Ordner innerhalb von Projekten zu erstellen. Um Ihre Arbeit zu organisieren, erstellen Sie mehrere Projekte innerhalb der Projektmappe.  
  
### <a name="to-add-a-new-query-to-an-existing-project"></a>So fügen Sie einem vorhandenen Projekt eine neue Abfrage hinzu  
  
1.  Wählen Sie im Projektmappen-Explorer ein Zielprojekt aus.  
  
2.  Klicken Sie im Menü **Projekt** auf **Neues Element hinzufügen**.  
  
3.  Wählen Sie im linken Bereich des Dialogfelds **Neues Element hinzufügen** eine Kategorie aus.  
  
4.  Wählen Sie im rechten Bereich eine Abfragevorlage aus, und klicken Sie dann auf **Hinzufügen**. Die neue Abfrage wird im Ordner **Abfragen** des Projekts hinzugefügt.  
  
5.  Geben Sie im Dialogfeld **Verbindung mit Datenbank-Engine herstellen** eine Verbindung für die neue Abfrage an, und klicken Sie dann auf **Verbinden**. Wenn Sie der neuen Abfrage keine Verbindung zuordnen möchten, klicken Sie in dem Verbindungsdialog auf **Abbrechen** .  
  
6.  Bei Bedarf können Sie die Abfrage im Projektmappen-Explorer umbenennen.  
  
### <a name="to-add-a-new-connection-to-an-existing-project"></a>So fügen Sie einem vorhandenen Projekt eine neue Verbindung hinzu  
  
1.  Wählen Sie im Projektmappen-Explorer ein Zielprojekt aus.  
  
2.  Klicken Sie im Menü **Projekt** auf **Neues Element hinzufügen**.  
  
3.  Wählen Sie im linken Bereich die Option **Verbindung** aus.  
  
4.  Wählen Sie im rechten Bereich die Option **Neue Verbindung** aus, und klicken Sie dann auf **Hinzufügen**.  
  
5.  Geben Sie im Dialogfeld **Verbindung mit Datenbank-Engine herstellen** eine Verbindung für die neue Abfrage an, und klicken Sie dann auf **Verbinden**. Die neue Verbindung wird im Ordner **Verbindungen** des Projekts hinzugefügt.  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
[Projektmappen-Explorer](../../ssms/solution/solution-explorer.md)  
[Zuordnen von Dateierweiterungen zu einem Code-Editor](../../relational-databases/scripting/associate-file-extensions-to-a-code-editor.md)  
[Hinzufügen vorhandener Elemente zu einem Projekt](../../ssms/solution/add-existing-items-to-a-project.md)  
[Entfernen oder Löschen eines Elements oder Projekts](../../ssms/solution/remove-or-delete-an-item-or-project.md)  
  
