---
title: Abrufen eines Einzelwerts aus einer Datenbank
description: Erfahren Sie, wie Sie einen Einzelwert in ADO.NET zurückgeben. Mit diesem Beispielcode wird der Wert der Identitätsspalte für einen eingefügten Datensatz zurückgegeben.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: b38526cd-a62a-48cb-822a-e91dfa68e02d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3937c1d4617f3cecb9012d82d590f75634ab1043
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771492"
---
# <a name="obtaining-a-single-value-from-a-database"></a>Abrufen eines Einzelwerts aus einer Datenbank

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Es kann vorkommen, dass Sie lediglich einzelne Werte anstelle von Tabellen oder Datenstreams aus einer Datenbank zurückgeben möchten. Angenommen, Sie möchten das Ergebnis einer Aggregatfunktion wie COUNT(\*), SUM(Price) oder AVG(Quantity) zurückgeben. Das **Command**-Objekt bietet mit der **ExecuteScalar**-Methode die Möglichkeit, Einzelwerte zurückzugeben. Die **ExecuteScalar**-Methode gibt den Wert in der ersten Spalte der ersten Zeile des Resultsets als Skalarwert zurück.

## <a name="example"></a>Beispiel

Im folgenden Codebeispiel wird mit einem <xref:Microsoft.Data.SqlClient.SqlCommand> ein neuer Wert in der Datenbank eingefügt. Mit der <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalar%2A>-Methode wird der Wert der Identitätsspalte für den eingefügten Datensatz zurückgegeben.

[!code-csharp[DataWorks SqlCommand.ExecuteScalar#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteScalar_Return_Id.cs#1)]

## <a name="see-also"></a>Weitere Informationen:

- [Befehle und Parameter](commands-parameters.md)
- [Ausführen eines Befehls](execute-command.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
