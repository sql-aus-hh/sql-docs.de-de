---
title: Zwischenspeichern von Schemas (SQLXML 4.0) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- database-engine
- docset-sql-devref
ms.topic: reference
helpviewer_keywords:
- registry keys [SQLXML]
- cache [SQLXML]
- schemas [SQLXML]
ms.assetid: 7e5fda21-b435-41fd-b637-8b616560a93f
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cfa52002e68fe8fa29a46a39cf224845274c2141
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48111982"
---
# <a name="schema-caching-sqlxml-40"></a>Zwischenspeichern von Schemas (SQLXML 4.0)
  Bei einer Seite-an-Seite-Installation von XML-Code für Microsoft SQL Server 2000 Web Release 1, Microsoft SQLXML 2.0 und SQLXML 3.0 können Sie das Zwischenspeichern von Schemas in alle Versionen mit den folgenden Registrierungsschlüsseln explizit steuern:  
  
 Web Release 1:  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXMLX\SchemaCacheSize  
```  
  
 SQLXML 2.0:  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML2\SchemaCacheSize  
```  
  
 SQLXML 3.0:  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML3\SchemaCacheSize  
```  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteRegistry](../../../includes/ssnoteregistry-md.md)]  
  
 Weitere Informationen zur Seite-an-Seite-Installation finden Sie unter [Neues in SQLXML 4.0 SP1](../../sqlxml/what-s-new-in-sqlxml-4-0-sp1.md).  
  
 Das Zwischenspeichern von Schemas verbessert die Leistung einer XPath-Abfrage deutlich. Wenn eine XPath-Abfrage für ein Zuordnungsschema ausgeführt wird, wird das Schema im Arbeitsspeicher gespeichert und werden die notwendigen Datenstrukturen im Arbeitsspeicher erstellt. Wenn das Zwischenspeichern von Schemas festgelegt ist, bleibt das Schema im Arbeitsspeicher und verbessert dadurch die Leistung für nachfolgende XPath-Abfragen.  
  
 Sie können die Größe des Schemacache durch Hinzufügen des oben angegebenen Schlüssels in der Registrierung hinzufügen.  
  
 Die Schemagröße wird basierend auf dem zur Verfügung stehenden Arbeitsspeicher und der Anzahl von Schemas festgelegt, die Sie verwenden. Der Standardwert **SchemaCacheSize** lautet 31. Setzen Sie **SchemaCacheSize** höher, mehr Arbeitsspeicher verwendet. Sie können somit die Cachegröße erhöhen, wenn der Schemazugriff langsam erscheint, oder die Cachegröße verringern, wenn der Arbeitsspeicher zu gering ist.  
  
 Aus Leistungsgründen wird empfohlen, dass Sie festlegen, **SchemaCacheSize** höher als die Anzahl der von Ihnen üblicherweise verwendeten Zuordnungsschemas. Die Anzahl von Schemas, wenn erhöhen **SchemaCacheSize** ist kleiner als die Anzahl von Schemas, die Sie verfügen, die die Leistung beeinträchtigt wird.  
  
> [!NOTE]  
>  Während der Entwicklung empfiehlt es sich, die Schemas nicht zwischenzuspeichern, da Änderungen an den Schemas im Zwischenspeicher erst nach zwei Minuten enthalten sind.  
  
## <a name="see-also"></a>Siehe auch  
 [Zwischenspeichern von Vorlagen &#40;SQLXML 4.0&#41;](template-caching-sqlxml-4-0.md)   
 [XSL-zwischenspeichern &#40;SQLXML 4.0&#41;](xsl-caching-sqlxml-4-0.md)  
  
  
