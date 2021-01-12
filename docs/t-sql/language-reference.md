---
description: Transact-SQL-Referenz (Datenbank-Engine)
title: Transact-SQL-Referenz (Datenbank-Engine) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
ms.assetid: dbba47d7-e08e-4435-b876-35dced1f325d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00d01b9b0646848ceb8b1deff2ab7cbe16518d0f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095850"
---
# <a name="transact-sql-reference-database-engine"></a>Transact-SQL-Referenz (Datenbank-Engine)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

In diesem Artikel erfahren Sie die Grundlagen zum Thema Suchen und Verwenden von Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)]-Referenzthemen (T-SQL). T-SQL ist wesentlich für die Verwendung von Microsoft SQL-Produkten und -Diensten. Alle Tools und Anwendungen, die mit einer SQL-Datenbank kommunizieren, erreichen diese Kommunikation durch das Senden von T-SQL-Befehlen.  

## <a name="t-sql-compliance-to-sql-standard"></a>Konformität von T-SQL mit dem SQL-Standard
Ausführliche technische Dokumente dazu, wie bestimmte Standards in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implementiert sind, finden Sie in der [Supportdokumentation für Microsoft SQL Server-Standards](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2).

## <a name="tools-that-use-t-sql"></a>Tools, die T-SQL verwenden
Zu den Microsoft-Tools, die T-SQL-Befehle verwenden, gehören u.a. folgende:

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
  
## <a name="locate-the-transact-sql-reference-topics"></a>Suchen der Referenzthemen zu Transact-SQL  
Um nach T-SQL-Themen zu suchen, verwenden Sie die Suche ganz oben rechts auf dieser Seite oder das Inhaltsverzeichnis auf der linken Seite. Sie können auch ein T-SQL-Schlüsselwort in das Fenster des Abfrage-Editors von Management Studio eingeben und F1 drücken. 
  
## <a name="find-system-views"></a>Suchen von Systemsichten
Um nach den Systemtabellen, -sichten, -funktionen und -prozeduren zu suchen, verwenden Sie die folgenden Links aus dem [Leitfaden für die Verwendung von relationalen Microsoft SQL-Datenbanken](../relational-databases/databases/databases.md) der SQL-Dokumentation.

- [Systemkatalogsichten](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Systemkompatibilitätssichten](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [Dynamische Systemverwaltungssichten](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [System functions (Systemfunktionen)](../relational-databases/system-functions/system-functions-category-transact-sql.md)
- [Informationsschemasichten des Systems](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Gespeicherte Systemprozeduren](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [Systemtabellen](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>„Gilt für“-Verweise  
 Die T-SQL-Referenzthemen gelten für mehrere Versionen von SQL Server (ab Version 2008) sowie für die anderen Azure SQL-Dienste. Am oberen Rand jedes Themas befindet sich ein Abschnitt, der angibt, für welche Produkte und Dienste das Thema gilt. 

Dieses Thema gilt beispielsweise für alle Versionen und hat folgende Bezeichnung. 
  
 [!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]   

Ein weiteres Beispiel ist die folgende Bezeichnung für ein Thema, das nur für Azure Synapse Analytics und Parallel Data Warehouse gilt.

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

In einigen Fällen wird das Thema von einem Produkt oder Dienst verwendet, jedoch werden nicht alle Argumente unterstützt. In diesem Fall werden zusätzliche **Applies to** -Abschnitte in den Beschreibungen der entsprechenden Arguments in den Text des Themas eingefügt.  
 
## <a name="get-help-from-microsoft-q--a"></a>Hilfe durch Microsoft Fragen und Antworten erhalten  
Onlinehilfe finden Sie im [Microsoft Q & A-Transact-SQL-Forum](/answers/topics/sql-server-transact-sql.html).  
 
## <a name="see-other-language-references"></a>Weitere Sprachreferenzen
Die SQL-Dokumentation enthält die folgenden weiteren Sprachreferenzen:
  
- [XQuery-Sprachreferenz](../xquery/xquery-language-reference-sql-server.md)
- [Integration Services-Sprachreferenz](../integration-services/integration-services-language-reference.md)
- [Replikationssprachreferenz](../relational-databases/replication/replication-language-reference.md)
- [Analysis Services-Sprachreferenz](../mdx/multidimensional-expressions-mdx-reference.md)  

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun die Grundlagen zum Suchen der T-SQL-Referenzthemen kennen, sind Sie bereit für folgende Schritte:

- Arbeiten Sie sich durch ein kurzes Tutorial zum Schreiben von T-SQL unter [Tutorial: Schreiben von Transact-SQL-Anweisungen](../t-sql/tutorial-writing-transact-sql-statements.md). 
- Sehen Sie sich die [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) an.  

