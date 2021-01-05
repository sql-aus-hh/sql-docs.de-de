---
title: Ausführen von Katalogoperationen
description: Beschreibt das Ausführen von Befehlen, mit denen ein Datenbankschema geändert wird.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: e60f542f-6271-495b-a9e4-48553481c2a3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 79eef694b3c6294eb12630f27c4b3688823581c7
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771544"
---
# <a name="performing-catalog-operations"></a>Ausführen von Katalogoperationen

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Um einen Befehl zum Ändern einer Datenbank oder eines Katalogs (z. B. die CREATE TABLE- oder die CREATE PROCEDURE-Anweisung) auszuführen, erstellen Sie ein **Command**-Objekt mithilfe der entsprechenden SQL-Anweisungen und eines **Connection**-Objekts. Führen Sie den Befehl mit der <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A>-Methode des <xref:Microsoft.Data.SqlClient.SqlCommand>-Objekts aus.

## <a name="example"></a>Beispiel

Im folgenden Codebeispiel wird eine gespeicherte Prozedur in einer Microsoft SQL Server-Datenbank erstellt.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#3](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#3)]

## <a name="see-also"></a>Weitere Informationen:

- [Verwenden von Befehlen zum Ändern von Daten](use-commands-to-modify-data.md)
- [Befehle und Parameter](commands-parameters.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
