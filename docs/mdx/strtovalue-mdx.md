---
title: StrToValue (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5a46b68ac8e93a00c7730b32593331a28655c1c5
ms.sourcegitcommit: 97bef3f248abce57422f15530c1685f91392b494
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34743069"
---
# <a name="strtovalue-mdx"></a>StrToValue (MDX)


  Gibt den durch eine Zeichenfolge im MDX-Format (Multidimensional Expressions) angegebenen numerischen Wert zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
StrToValue(MDX_Expression [,CONSTRAINED] )   
```  
  
## <a name="arguments"></a>Argumente  
 *MDX_Expression*  
 Ein gültiger Zeichenfolgenausdruck, der direkt oder indirekt zu einer einzelnen Zelle aufgelöst wird.  
  
## <a name="remarks"></a>Hinweise  
 Die **StrToValue** Funktion gibt den numerischen Wert, der durch den MDX-Ausdruck angegeben. Die **StrToValue** Funktion wird in der Regel mit benutzerdefinierten Funktionen verwendet, um einen MDX-Ausdruck aus einer externen Funktion an eine MDX-Anweisung zurückzugeben, die in einer einzelnen Zelle aufgelöst werden kann.  
  
-   Wenn das CONSTRAINED-Flag verwendet wird, darf der MDX-Ausdruck nur einen Skalarwert enthalten. Das CONSTRAINED-Flag wird verwendet, um das Risiko von Injection-Angriffen über die angegebene Zeichenfolge zu minimieren. Wenn ein MDX-Ausdruck bereitgestellt wird, der nicht direkt zu einem Skalarwert aufgelöst werden kann, wird eine Fehlermeldung angezeigt, die besagt, dass die durch das CONSTRAINED-Flag in der STRTOVALUE-Funktion vorgegebenen Einschränkungen verletzt wurden.  
  
-   Wenn das CONSTRAINED-Flag nicht verwendet wird, kann der angegebene MDX-Ausdruck beliebig komplex sein, vorausgesetzt er wird zu einem gültigen MDX-Ausdruck (Multidimensional Expressions) aufgelöst, der eine einzelne Zelle zurückgibt.  
  
> [!NOTE]  
>  Das Zurückgeben des Ergebnisses eines MDX-Ausdrucks als numerischen Wert kann sinnvoll sein, wenn der Wert als Text gespeichert ist und Sie arithmetische Operationen für die zurückgegebenen Werte ausführen möchten.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird die **StrToValue** Funktion, um die Gewichtung jedes Fahrrades als Wert zurückgeben.  
  
```  
WITH MEMBER Measures.x AS   
StrToValue   
   ([Product].[Product].CurrentMember.Properties ('Weight')  
   ,CONSTRAINED  
   )  
SELECT Measures.x ON 0  
,[Product].[Product].[Product].Members ON 1  
FROM [Adventure Works]  
WHERE [Product].[Product Categories].[Bikes]  
```  
  
## <a name="see-also"></a>Siehe auch  
 [MDX-Funktionsreferenz &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
