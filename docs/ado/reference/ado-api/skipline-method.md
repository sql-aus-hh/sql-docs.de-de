---
title: SkipLine-Methode | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Stream::raw_SkipLine
- _Stream::SkipLine
helpviewer_keywords:
- Skipline method [ADO]
ms.assetid: 0abc00fe-ee09-4c8e-b1f2-48ee9c5f3329
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b0e96900fdac55e97e3481ba5198e0f51d659877
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47713758"
---
# <a name="skipline-method"></a>SkipLine-Methode
Überspringt eine gesamte Zeile, beim Lesen einer Textdatei [Stream](../../../ado/reference/ado-api/stream-object-ado.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Stream.SkipLine  
```  
  
## <a name="remarks"></a>Hinweise  
 Alle Zeichen bis zur und einschließlich der nächsten Linie wird als Trennzeichen werden übersprungen. In der Standardeinstellung die [LineSeparator](../../../ado/reference/ado-api/lineseparator-property-ado.md) ist **AdCRLF**. Wenn Sie versuchen, überspringen [EOS](../../../ado/reference/ado-api/eos-property.md), die aktuelle Position bleibt **EOS**.  
  
 Die **SkipLine** Methode wird verwendet, mit dem Text-Streams ([Typ](../../../ado/reference/ado-api/type-property-ado-stream.md) ist **AdTypeText**).  
  
## <a name="applies-to"></a>Gilt für  
 [Stream-Objekt (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)
