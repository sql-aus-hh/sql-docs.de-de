---
title: Gleitkommazahlen
description: Lernen Sie einige der Probleme beim Arbeiten mit Gleitkommazahlen im Microsoft SqlClient-Datenanbieter für SQL Server kennen.
ms.date: 11/13/2020
ms.assetid: 73c218c6-1c44-4402-a167-4f6262629a91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1ca251c6de5ca6157ace8ee481bae02afa9fb2fb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771565"
---
# <a name="floating-point-numbers"></a>Gleitkommazahlen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In diesem Thema werden einige der Probleme beschrieben, auf die Entwickler häufig stoßen, wenn sie im Microsoft SqlClient-Datenanbieter für SQL Server mit Gleitkommazahlen arbeiten. Ursache für diese Probleme, die nicht spezifisch für einen bestimmten Anbieter wie <xref:Microsoft.Data.SqlClient> sind, ist die Art und Weise, wie Computer Gleitkommazahlen speichern.

## <a name="floating-point-pitfall"></a>Gleitkommafehler

Für Gleitkommazahlen gibt es in der Regel keine genaue binäre Darstellung. Stattdessen speichert der Computer einen Näherungswert der Zahl. Es ist möglich, dass zu unterschiedlichen Zeiten auch unterschiedliche Binärziffern zur Darstellung der Zahl verwendet werden. Wenn die Darstellung einer Gleitkommazahl in eine andere Darstellung umgewandelt wird, können die letzten gültigen Ziffern dieser Zahl geringfügig anders ausfallen. Zu einer solchen Umwandlung kommt es in der Regel dann, wenn die Zahl von einem Typ in einen anderen Typ konvertiert wird. Die Abweichung tritt unabhängig davon auf, ob die Umwandlung innerhalb einer Datenbank, zwischen Typen, die Datenbankwerte darstellen, oder zwischen Typen erfolgt. Aufgrund dieser Änderungen können sich Zahlen, die logisch gleich sind, in ihren letzten gültigen Ziffern unterscheiden, sodass sie verschiedene Werte besitzen. Die Anzahl der präzisen Ziffern in der Zahl kann höher oder niedriger als erwartet ausfallen. Beim Formatieren als Zeichenfolge zeigt die Zahl u. U. nicht den erwarteten Wert.

## <a name="suggested-workarounds"></a>Empfohlene Problemumgehungen

Um diese Effekte so gering wie möglich zu halten, sollten Sie die bestmögliche Übereinstimmung zwischen den Ihnen zur Verfügung stehenden numerischen Typen verwenden. Wenn Sie z. B. mit SQL Server arbeiten, kann sich der exakte numerische Wert ändern, wenn Sie einen Transact-SQL-Wert des Typs „real“ in einen Wert des Typs „float“ ändern. In .NET kann auch die Umwandlung von <xref:System.Single> zu <xref:System.Double> zu unerwarteten Ergebnissen führen. In beiden Fällen empfiehlt es sich, dafür zu sorgen, dass alle Werte in der Anwendung denselben numerischen Typ verwenden. Sie können auch einen Dezimaltyp mit fester Genauigkeit verwenden oder Gleitkommazahlen in Dezimalzahlen mit fester Genauigkeit umwandeln, bevor Sie mit ihnen arbeiten.

Wenn Sie Probleme mit Gleichheitsvergleichen umgehen möchten, überlegen Sie, ob Sie Ihre Anwendung nicht so programmieren können, dass Abweichungen bei den letzten gültigen Ziffern ignoriert werden. So können Sie z. B. statt zu vergleichen, ob zwei Zahlen gleich sind, eine Zahl von der anderen Zahl subtrahieren. Wenn die Differenz in einem akzeptablen Rundungsbereich liegt, kann Ihre Anwendung die Zahlen so behandeln, als wären sie gleich.

## <a name="see-also"></a>Weitere Informationen:

- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
