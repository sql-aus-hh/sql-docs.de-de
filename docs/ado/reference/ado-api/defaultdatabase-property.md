---
title: DefaultDatabase-Eigenschaft | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Connection15::DefaultDatabase
helpviewer_keywords:
- DefaultDatabase property
ms.assetid: 41e8a8dd-e69c-4a09-8736-93502e01961c
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2cd93826e03f7767455ec4b656ed14d1c35c21d2
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47757449"
---
# <a name="defaultdatabase-property"></a>DefaultDatabase-Eigenschaft
Gibt die Standarddatenbank für eine [Verbindung](../../../ado/reference/ado-api/connection-object-ado.md) Objekt.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt fest oder gibt einen **Zeichenfolge** -Wert, der den Namen einer Datenbank vom Anbieter ergibt.  
  
## <a name="remarks"></a>Hinweise  
 Verwenden der **DefaultDatabase** Eigenschaft so festlegen oder Zurückgeben der Name der Standarddatenbank für einen bestimmten **Verbindung** Objekt.  
  
 Ist eine Standarddatenbank, können SQL-Zeichenfolgen eine unvollständige Syntax verwenden, den Zugriff auf Objekte in dieser Datenbank. Zum Zugreifen auf Objekte in einer anderen Datenbank als die der **DefaultDatabase** -Eigenschaft, müssen Sie mit dem gewünschten Namen der zu verwendenden Objektnamen qualifizieren. Beim Herstellen der Verbindung, der Anbieter Datenbank Standardinformationen zum Schreiben der **DefaultDatabase** Eigenschaft. Einige Anbieter können nur eine Datenbank pro Verbindung, die in diesem Fall können Sie nicht ändern der **DefaultDatabase** Eigenschaft.  
  
 Einige Datenquellen und Anbietern können dieses Feature wird nicht unterstützt und möglicherweise ein Fehler oder eine leere Zeichenfolge zurück.  
  
> [!NOTE]
>  **Remote Datendienstnutzung** diese Eigenschaft ist nicht verfügbar für eine clientseitige **Verbindung** Objekt.  
  
## <a name="applies-to"></a>Gilt für  
 [Connection-Objekt (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>Siehe auch  
 [Provider- und DefaultDatabase-Eigenschaften-Beispiel (VB)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vb.md)   
 [Provider- und DefaultDatabase-Eigenschaft – Beispiel (VC++)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vc.md)   
