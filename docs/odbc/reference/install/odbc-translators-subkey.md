---
title: Unterschlüssel für ODBC-Konvertierungsprogramme | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- translator subkey [ODBC]
- subkeys [ODBC], translator subkey
- registry entries for components [ODBC], translator subkey
ms.assetid: 6b170f1f-e263-4aac-9d49-8d0ca0470ca2
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9e7109a6f1b88cf7639b2fc823ce0c5f14d05002
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47673238"
---
# <a name="odbc-translators-subkey"></a>Unterschlüssel für ODBC-Konvertierungsprogramme
Die Werte unter dem Unterschlüssel für ODBC-Konvertierungsprogramme Auflisten der installierten Übersetzer. Das Format dieser Werte wird in der folgenden Tabelle dargestellt.  
  
|Name|Datentyp|data|  
|----------|---------------|----------|  
|*Translator-desc*|REG_SZ|**installiert**|  
  
 Die *Translator-Desc* Name wird von der Translator-Entwickler definiert.  
  
 Nehmen wir beispielsweise an, dass ein Benutzer die Microsoft® Code Seite Translator und eine benutzerdefinierte ASCII zu EBCDIC-Konvertierer installiert ist. Die Werte unter dem Unterschlüssel für ODBC-Konvertierungsprogramme könnte folgendermaßen aussehen:  
  
```  
MS Code Page Translator: REG_SZ : Installed  
ASCII to EBCDIC: REG_SZ : Installed.  
```
