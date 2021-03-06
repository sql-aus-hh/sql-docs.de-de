---
title: 'Gewusst wie: Löschen von Objekten und Auflösen von Abhängigkeiten | Microsoft-Dokumentation'
ms.custom:
- SSDT
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- Microsoft.VisualStudio.Data.Tools.Project.HelpKeywords.SqlProjectDropDatabaseConfirmationDialog
- sql.data.tools.dropdatabaseconfirmation.dialog
- sql.data.tools.dropmultipledatabasesconfirmation.dialog
ms.assetid: fb31c2b1-ca4f-4e11-a0b6-5c26430f1c8c
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1c52b2bcd700d4b7399fe27c79063f4b27d4e68a
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47676168"
---
# <a name="how-to-delete-objects-and-resolve-dependencies"></a>Gewusst wie: Löschen von Objekten und Auflösen von Abhängigkeiten
Wenn Sie ein Objekt im **SQL Server-Objekt-Explorer** umbenennen oder löschen, erkennt SQL Server Data Tools automatisch alle zugehörigen Abhängigkeitsobjekte und bereitet ein ALTER-Skript vor, um die Abhängigkeit entsprechend umzubenennen oder zu löschen.  
  
> [!WARNING]  
> Bei den folgenden Vorgehensweisen werden die Entitäten verwendet, die in vorherigen Vorgehensweisen im Abschnitt [Entwicklung verbundener Datenbanken](../ssdt/connected-database-development.md) erstellt wurden.  
  
### <a name="to-delete-a-database"></a>So löschen Sie eine Datenbank  
  
1.  Klicken Sie mit der rechten Maustaste auf eine Datenbank im **SQL Server-Objekt-Explorer**, und klicken Sie auf **Löschen**.  
  
2.  Übernehmen Sie alle Standardeinstellungen im Dialogfeld **Datenbank löschen**, und klicken Sie auf **OK**.  
  
### <a name="to-rename-a-table"></a>So benennen Sie eine Tabelle um  
  
1.  Vergewissern Sie sich, dass die Tabelle `Customer` nicht im Tabellen-Designer oder im Transact\-SQL-Editor geöffnet ist.  
  
2.  Erweitern Sie den Knoten **Tabellen** im **SQL Server-Objekt-Explorer**. Klicken Sie mit der rechten Maustaste auf die Tabelle **Customer**, und wählen Sie **Umbenennen** aus.  
  
3.  Ändern Sie den Tabellennamen in **Customers**, und drücken Sie die EINGABETASTE.  
  
4.  Beachten Sie, dass der Vorgang **Datenbankupdate** sofort in Ihrem Namen gestartet wird. Die gespeicherte Prozedur "sp_rename" wird von SSDT in Ihrem Namen aufgerufen, um die Tabelle umzubenennen. Wenn abhängige Objekte (z. B. Fremdschlüsseleinschränkungen) vorhanden sind, werden diese ebenfalls aktualisiert.  
  
    > [!WARNING]  
    > Skriptbasierte Abhängigkeiten (z. B. Verweise aus einer Sicht auf eine Tabelle) und gespeicherte Prozeduren werden von SSDT nicht automatisch aktualisiert. Nach dem Umbenennen können Sie anhand des Bereichs **Fehlerliste** alle anderen Abhängigkeiten suchen und diese manuell korrigieren.  
  
5.  Wenden Sie die Änderung gemäß den Schritten im vorherigen Verfahren [Gewusst wie: Aktualisieren einer verbundenen Datenbank mit Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md) an.  
  
6.  Klicken Sie im **SQL Server-Objekt-Explorer** erneut mit der rechten Maustaste auf die Tabelle **Customers**, und wählen Sie die Option **Daten anzeigen** aus. Beachten Sie, dass Tabellendaten nach dem Umbenennungsvorgang unverändert beibehalten sind.  
  
7.  Klicken Sie mit der rechten Maustaste auf die Tabelle **Products**, und wählen Sie **Code anzeigen** aus. Der Fremdschlüsselverweis wurde automatisch in `REFERENCES [dbo].[Customers] ([Id])` aktualisiert, um die Umbenennung widerzuspiegeln.  
  
### <a name="to-delete-a-table"></a>So löschen Sie eine Tabelle  
  
1.  Klicken Sie im **SQL Server-Objekt-Explorer** mit der rechten Maustaste auf die Tabelle **Customers**, und wählen Sie die Option **Löschen** aus.  
  
2.  Überzeugen Sie sich im Dialogfeld **Vorschau der Datenbankupdates** unter **Benutzeraktion**, dass alle abhängigen Objekte von SSDT identifiziert wurden. In diesem Fall handelt es sich um einen Fremdschlüsselverweis, der gelöscht wird.  
  
3.  Klicken Sie auf **Datenbank aktualisieren**.  
  
4.  Klicken Sie im **SQL Server-Objekt-Explorer** mit der rechten Maustaste auf die Tabelle **Products**, und wählen Sie die Option **Code anzeigen** aus. Der Fremdschlüsselverweis auf die Tabelle `Customers` ist nicht mehr vorhanden.  
  
    > [!WARNING]  
    > Wenn Sie bei Ausführung des Löschvorgangs die Tabelle **Products** bereits im Tabellen-Designer oder im Transact\-SQL-Editor geöffnet haben, erfolgt keine automatische Aktualisierung, und das Löschen des Fremdschlüsselverweises wird nicht widergespiegelt. Darüber hinaus können Fehler in Bezug auf nicht aufgelöste Verweise in der **Fehlerliste** aufgeführt werden. Um dieses Problem zu beheben, schließen Sie den Tabellen-Designer bzw. den Transact\-SQL-Editor, und öffnen Sie die Tabelle „Products“ erneut.  
  
