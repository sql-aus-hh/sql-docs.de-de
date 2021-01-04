---
title: SQL Database Projects-Erweiterung
description: Installieren und Verwenden der SQL Database Project-Erweiterung für Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 12/15/2020
ms.openlocfilehash: 7b52827de249153adc54d148ead5d93a015d152d
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559092"
---
# <a name="sql-database-projects-extension-preview"></a>SQL Database Projects-Erweiterung (Vorschau)

Die Erweiterung „SQL Database Projects“ (Vorschau) ist eine Erweiterung für die Entwicklung von SQL-Datenbanken in einer projektbasierten Entwicklungsumgebung. 


## <a name="features"></a>Features

1. Erstellen eines Projekts aus einer verbundenen Datenbank.
2. Erstellen eines neuen leeren Projekts.
3. Öffnen eines Projekts, das in [Azure Data Studio](sql-database-project-extension-getting-started.md) oder [SQL Server Data Tools](../../ssdt/sql-server-data-tools.md) erstellt wurde.
4. Bearbeiten eines Projekts durch Hinzufügen oder Entfernen von Tabellen, Sichten, gespeicherten Prozeduren oder benutzerdefinierten Skripts.
5. Organisieren von Dateien oder Skripts in Ordnern.
6. Hinzufügen von Verweisen auf Systemdatenbanken oder DACPAC-Benutzerdateien.
7. Erstellen eines einzelnen Projekts.
8. Bereitstellen eines einzelnen Projekts.
9. Laden von Verbindungsdetails (SQL-Windows-Authentifizierung) und SQLCMD-Variablen aus einem Bereitstellungsprofil.

In diesem kurzen 10-minütigen Video erhalten Sie eine Einführung in die Erweiterung für SQL-Datenbank-Projekte in Azure Data Studio:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Build-SQL-Database-Projects-Easily-in-Azure-Data-Studio/player?WT.mc_id=dataexposed-c9-niner]

## <a name="install-the-sql-database-projects-extension"></a>Installieren der Erweiterung „SQL Database Projects“

1. Öffnen Sie den Erweiterungs-Manager, um auf die verfügbaren Erweiterungen zuzugreifen.  Klicken Sie dazu entweder auf das Erweiterungssymbol, oder wählen Sie im Menü **Ansicht** die Option **Erweiterungen** aus.
2. Suchen Sie die Erweiterung *SQL Database Projects*, indem Sie deren Namen ganz oder teilweise in das Suchfeld für Erweiterungen eingeben. Wählen Sie eine verfügbare Erweiterung aus, um deren Details anzuzeigen.

   ![Installieren der Erweiterung](media/sql-database-projects-extension/install-database-projects.png)

3. Wählen Sie die gewünschte Erweiterung aus, und **installieren** Sie diese.
4. Klicken Sie auf **Erneut laden**, um die Erweiterung zu aktivieren (nur bei der Erstinstallation einer Erweiterung erforderlich).
5. Klicken Sie in der Aktivitätsleiste auf das Dateisymbol, oder klicken Sie im Menü **View** (Ansicht) auf **Explorer**. Ein neues Viewlet für **Projekte** ist nun verfügbar.

   > [!NOTE]
   > Das .NET Core SDK ist für die Buildfunktion des Projekts erforderlich, und Sie werden zur Installation des SDK aufgefordert, wenn es von der Erweiterung nicht gefunden wird.  Das .NET Core SDK (v3.1 oder höher) kann von [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) heruntergeladen und installiert werden.

   > [!NOTE]
   > Für die Nutzung des vollen Funktionsumfangs wird empfohlen, die [Erweiterung „Schemavergleich“](schema-compare-extension.md) zusammen mit der Erweiterung „SQL Database Projects“ zu installieren.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Das Laden von Dateien als Link wird im Azure Data Studio-Viewlet derzeit nicht unterstützt. Die Dateien werden aber in die oberste Ebene der Struktur geladen und beim Kompilieren erwartungsgemäß einbezogen.
- SQLCLR-Objekte im Projekt werden in der .NET Core-Version von DacFx nicht unterstützt.
- Tasks (Kompilieren/Veröffentlichen) sind nicht benutzerdefiniert.
- Veröffentlichen von Zielen, die von DacFx definiert wurden.
- Die Unterstützung für die WSL-Umgebung ist eingeschränkt.

## <a name="workspace"></a>Arbeitsbereich
SQL-Datenbankprojekte in Azure Data Studio befinden sich in einem logischen Arbeitsbereich.  Ein Arbeitsbereich verwaltet die im Bereich „Explorer“ angezeigten Ordner sowie die Projekte, die im Bereich „Projekte“ angezeigt werden. Sie können Projekte über die Azure Data Studio-Schnittstelle im Bereich „Projekte“ zu einem Arbeitsbereich hinzufügen oder aus diesem entfernen. Bei Bedarf können die Einstellungen eines Arbeitsbereichs jedoch auch manuell in der `.code-workspace`-Datei bearbeitet werden.

In der unter veranschaulichten `.code-workspace`-Beispieldatei führt das `folders`-Array alle im Bereich „Explorer“ enthaltenen Ordner auf, und das `dataworkspace.projects`-Array in `settings` führt alle im Bereich „Projekte“ enthaltenen SQL-Projekte auf.

```json
{
    "folders": [
        {
            "path": "."
        },
        {
            "name": "WideWorldImportersDW",
            "path": "..\\WideWorldImportersDW"
        }
    ],
    "settings": {
        "dataworkspace.projects": [
            "AdventureWorksLT.sqlproj",
            "..\\WideWorldImportersDW\\WideWorldImportersDW.sqlproj"
        ]
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit der Erweiterung „SQL Database Projects“](sql-database-project-extension-getting-started.md)
- [Erstellen und Veröffentlichen Sie ein Projekt mit der Erweiterung „SQL Database Projects“ für Azure Data Studio.](sql-database-project-extension-build.md)
