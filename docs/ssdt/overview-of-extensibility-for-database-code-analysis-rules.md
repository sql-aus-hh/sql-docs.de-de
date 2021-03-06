---
title: Übersicht über die Erweiterbarkeit um Regeln für die Datenbankcodeanalyse | Microsoft-Dokumentation
ms.custom:
- SSDT
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 62f5c980-18d5-43fe-b443-c9e149d01fc7
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4c754ce006834a44413d64821ea79349da2e62d9
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47699918"
---
# <a name="overview-of-extensibility-for-database-code-analysis-rules"></a>Übersicht der Erweiterbarkeit um Regeln für die Datenbank-Codeanalyse
Visual Studio-Editionen, die SQL Server Data Tools enthalten, beinhalten Regeln für die Codeanalyse, um Transact\-SQL-Warnungen zu Entwurf, Benennung und Leistung in Ihrem Datenbankcode zu melden. Weitere Informationen zur Codeanalyse finden Sie unter [Analysieren von Datenbankcode zum Verbessern der Codequalität](http://msdn.microsoft.com/library/dd172133(v=vs.100).aspx).  
  
Wenn die integrierten Regeln zur Codeanalyse ein bestimmtes Transact\-SQL-Problem, das Sie berücksichtigen möchten, nicht abdecken, können Sie benutzerdefinierte Regeln zur Analyse von Datenbankcode erstellen. Beispielsweise möchten Sie möglicherweise eine benutzerdefinierte Regel erstellen, die die Verwendung der WAITFOR DELAY-Anweisung verhindert, wie unter [Exemplarische Vorgehensweise: Erstellen einer benutzerdefinierten Regelassembly zur statischen Codeanalyse für SQL Server](../ssdt/walkthrough-author-custom-static-code-analysis-rule-assembly.md) veranschaulicht wird. Verwenden Sie zum Erstellen benutzerdefinierter Regeln zur Datenbankcodeanalyse die Klassen im [CodeAnalysis](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.codeanalysis.aspx)-Namespace.  
  
Bevor Sie benutzerdefinierte Regeln zur Codeanalyse erstellen, sollten Sie die Architektur der verschiedenen Komponenten von Regeln zur Datenbankcodeanalyse im Grundsatz verstehen.  
  
## <a name="database-code-analysis-rules-components"></a>Komponenten von Regeln zur Datenbankcodeanalyse  
Das folgende Diagramm veranschaulicht die Interaktion der Regeln zur Datenbankcodeanalyse:  
  
![Komponenten von Regeln zur Datenbankcodeanalyse](../ssdt/media/ssdt-database-code-analysis-rules-components.jpg "Komponenten von Regeln zur Datenbankcodeanalyse")  
  
Wenn Sie die Funktion für Regeln zur Datenbankcodeanalyse verwenden, entweder durch direktes Ausführen der statischen Codeanalyse (weitere Informationen finden Sie unter [Gewusst wie: Analysieren von Transact-SQL-Code auf Codefehler](http://msdn.microsoft.com/library/dd172119(v=vs.100).aspx)) oder durch Erstellen eines Builds, werden alle Regeln geladen und entsprechend ihrer Konfiguration in Ihrem Projekt verwendet. Weitere Informationen finden Sie unter [Gewusst wie: Aktivieren und Deaktivieren bestimmter Regeln für die statische Analyse von Datenbankcode](http://msdn.microsoft.com/library/dd172131(v=vs.100).aspx). Der Erweiterungs-Manager lädt außerdem alle benutzerdefinierten Regelassemblys, die von Ihnen erstellt und registriert wurden. Weitere Informationen finden Sie unter [Gewusst wie: Installieren und Verwalten von Funktionserweiterungen](../ssdt/how-to-install-and-manage-feature-extensions.md).  
  
Eine benutzerdefinierte Klasse von Codeanalyseregeln erbt von [SqlCodeAnalysisRule](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.codeanalysis.sqlcodeanalysisrule.aspx). Die benutzerdefinierte Regelklasse kann über ihren Regelausführungskontext auf eine Reihe von nützlichen Objekten zugreifen. Dazu gehören:  
  
-   Metadaten über die Regel selbst.  
  
-   Das Dac.Model.TSqlModel, das das Schema der Datenbank darstellt, einschließlich aller Modellelemente, der Beziehungen zwischen ihnen und eventueller Eigenschaften der Elemente.  
  
-   Für Regeln, die spezifische Elemente untersuchen, wird das Dac.Model.TSqlObject, das das Schemaelement im Modell darstellt, in den Kontext einbezogen.  
  
-   Viele Schemaobjekte weisen darüber hinaus eine [ScriptDom](https://msdn.microsoft.com/library/microsoft.sqlserver.transactsql.scriptdom.aspx)-Darstellung auf, auf die über diesen Kontext zugegriffen werden kann. Hierbei handelt es sich um eine AST-basierte Darstellung eines Elements, die beim Erforschen potenzieller Syntaxprobleme hilfreich sein kann, wie etwa dem Vorhandensein von [SelectStarExpression](https://msdn.microsoft.com/library/microsoft.sqlserver.transactsql.scriptdom.selectstarexpression.aspx).  
  
Ein Dac.CodeAnalysis.SqlRuleProblem wird von der Regel erstellt, um alle von ihr möglicherweise gefundenen Probleme darzustellen. Bei der Erstellung werden das relevante Dac.Model.TSqlObject und ein mögliches Element in der [ScriptDom](https://msdn.microsoft.com/library/microsoft.sqlserver.transactsql.scriptdom.aspx)-Darstellung an den Konstruktor übergeben und dazu verwendet, die Position des Problems in Ihren Quellcodedateien zu bestimmen. Am Ende der Analyse werden alle genannten Probleme an den Fehler-Manager übergeben und in der Fehlerliste angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
[Erweitern der Datenbankfunktionen](../ssdt/extending-the-database-features.md)  
  
