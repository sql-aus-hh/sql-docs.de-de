---
title: Transact-SQL-Referenz (Datenbank-Engine)
description: Transact-SQL-Referenz (Datenbank-Engine)
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfe9a07eb1b37f0bddf128b1cbdf39de3d74a643
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689079"
---
# <a name="transact-sql-reference-database-engine"></a>Transact-SQL-Referenz (Datenbank-Engine)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

In diesem Artikel erfahren Sie die Grundlagen zum Thema Suchen und Verwenden von Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)]-Referenzthemen (T-SQL). T-SQL ist wesentlich für die Verwendung von Microsoft SQL-Produkten und -Diensten. Alle Tools und Anwendungen, die mit einer SQL-Datenbank kommunizieren, erreichen diese Kommunikation durch das Senden von T-SQL-Befehlen.  

## <a name="t-sql-compliance-with-sql-standard"></a>Konformität von T-SQL mit dem SQL-Standard
Ausführliche technische Dokumente dazu, wie bestimmte Standards in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implementiert sind, finden Sie in der [Supportdokumentation für Microsoft SQL Server-Standards](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2).

## <a name="tools-that-use-t-sql"></a>Tools, die T-SQL verwenden
Zu den Microsoft-Tools, die T-SQL-Befehle verwenden, gehören u.a. folgende:

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)

## <a name="locate-the-transact-sql-reference-topics"></a>Suchen der Referenzthemen zu Transact-SQL  
Um nach T-SQL-Themen zu suchen, verwenden Sie die Suche ganz oben rechts auf dieser Seite oder das Inhaltsverzeichnis auf der linken Seite. Sie können auch ein T-SQL-Schlüsselwort in das Fenster des Abfrage-Editors von Management Studio eingeben und F1 drücken.

## <a name="find-system-views"></a>Suchen von Systemsichten
Unter den folgenden Links finden Sie die Systemtabellen, Sichten, Funktionen und Prozeduren, die im Abschnitt [Verwenden relationaler Datenbanken](../relational-databases/databases/databases.md) der SQL-Dokumentation behandelt werden.

- [Systemkatalogsichten](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Systemkompatibilitätssichten](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [Dynamische Systemverwaltungssichten](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [System functions (Systemfunktionen)](../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Informationsschemasichten des Systems](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Gespeicherte Systemprozeduren](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [Systemtabellen](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>„Gilt für“-Verweise  

Die T-SQL-Referenzartikel gelten für mehrere Versionen von SQL Server (ab Version 2008) sowie für die anderen Azure SQL-Dienste. Am oberen Rand jedes Themas befindet sich ein Abschnitt, der angibt, für welche Produkte und Dienste das Thema gilt. 

Dieses Thema gilt beispielsweise für alle Versionen und hat folgende Bezeichnung.

[!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]

Ein weiteres Beispiel ist die folgende Bezeichnung für ein Thema, das nur für Azure Synapse Analytics und Parallel Data Warehouse gilt.

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

In einigen Fällen wird das Thema von einem Produkt oder Dienst verwendet, jedoch werden nicht alle Argumente unterstützt. In diesem Fall werden andere Abschnitte mit dem Titel **Gilt für** in die entsprechenden Argumentbeschreibungen in die Artikel eingefügt.

## <a name="get-help-from-microsoft-q--a"></a>Hilfe durch Microsoft Fragen und Antworten erhalten

Onlinehilfe finden Sie im [Microsoft Q & A-Transact-SQL-Forum](/answers/topics/sql-server-transact-sql.html).

## <a name="see-other-language-references"></a>Weitere Sprachreferenzen

Die SQL-Dokumentation enthält die folgenden weiteren Sprachreferenzen:

- [XQuery-Sprachreferenz](../xquery/xquery-language-reference-sql-server.md)
- [Integration Services-Sprachreferenz](../integration-services/integration-services-language-reference.md)
- [Replikationssprachreferenz](../relational-databases/replication/replication-language-reference.md)
- [Analysis Services-Sprachreferenz](../mdx/multidimensional-expressions-mdx-reference.md)

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun wissen, wie Sie bestimmte T-SQL-Referenzartikel finden, sind Sie bereit für die nächsten Schritte:

- Arbeiten Sie sich durch ein kurzes Tutorial zum Schreiben von T-SQL unter [Tutorial: Schreiben von Transact-SQL-Anweisungen](../t-sql/tutorial-writing-transact-sql-statements.md).
- Sehen Sie sich die [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) an.