---
title: Abrufen von Binärdaten
description: Beschreibt, wie Binärdaten oder große Datenstrukturen mithilfe von `CommandBehavior` abgerufen werden. `SequentialAccess` zum Ändern des Standardverhaltens einer `DataReader`-Instanz.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 56c5a9e3-31f1-482f-bce0-ff1c41a658d0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1033a6b5394d92a45cd19d70f4dd6c250ec37b62
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744433"
---
# <a name="retrieve-binary-data"></a>Abrufen von Binärdaten

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

In der Standardeinstellung lädt der **DataReader** eingehende Daten als Zeile, sobald eine ganze Datenzeile verfügbar ist. BLOBs (Binary Large Objects) müssen jedoch anders behandelt werden, da sie mehrere Gigabyte an Daten umfassen können, die nicht in eine einzelne Zeile passen. Die **Command.ExecuteReader**-Methode weist eine Überladung auf, die eine Änderung des Standardverhaltens von **DataReader** mit einem <xref:System.Data.CommandBehavior>-Argument erforderlich macht. Durch Übergeben von <xref:System.Data.CommandBehavior.SequentialAccess> an die **ExecuteReader**-Methode kann das Standardverhalten von **DataReader** so geändert werden, dass nicht Datenzeilen, sondern die Daten beim Empfang nacheinander geladen werden. Diese Methode eignet sich hervorragend zum Laden von BLOBs oder anderen großen Datenstrukturen.

> [!NOTE]
> Wenn Sie für **DataReader** die Verwendung von **SequentialAccess** festlegen, müssen Sie die Reihenfolge beachten, in der Sie auf die zurückgegebenen Felder zugreifen. Das Standardverhalten von **DataReader** (Laden der ganzen Zeile sobald sie verfügbar ist) ermöglicht Ihnen den Zugriff auf die zurückgegebenen Felder in beliebiger Reihenfolge, bis die nächste Zeile gelesen wird. Beim Verwenden von **SequentialAccess** muss jedoch auf die vom **DataReader** zurückgegebenen Felder in der entsprechenden Reihenfolge zugegriffen werden. Wenn die Abfrage beispielsweise drei Spalten zurückgibt, von der die dritte ein BLOB ist, müssen zuerst die Werte des ersten und zweiten Felds zurückgegeben werden, bevor auf die BLOB-Daten im dritten Feld zugegriffen werden darf. Wenn Sie vor dem ersten und zweiten Feld auf das dritte Feld zugreifen, sind die Werte des ersten und zweiten Feldes nicht mehr verfügbar. Dies liegt daran, dass der **DataReader** mit **SequentialAccess** geändert wurde, sodass Daten nur noch der Reihe nach zurückgegeben werden und die Daten nicht mehr verfügbar sind, nachdem der **DataReader** über das Ende dieser Daten hinaus gelesen hat.

Verwenden Sie zum Zugreifen auf die Daten im BLOB-Feld die typisierten Zugriffsmethoden **GetBytes** oder **GetChars** des **DataReader**, die ein Array mit Daten füllen. Für Zeichendaten können Sie auch **GetString** verwenden. Um die Systemressourcen nicht unnötig zu belasten, wird jedoch davon abgeraten, einen ganzen BLOB-Wert in eine einzelne Zeichenfolgenvariable zu laden. Sie können stattdessen eine bestimmte Puffergröße für Rückgabedaten und die Anfangsposition für das erste zu lesende Byte oder Zeichen in den Rückgabedaten angeben. Mit **GetBytes** und **GetChars** wird ein `long`-Wert zurückgegeben, der der Anzahl der zurückgegebenen Bytes oder Zeichen entspricht. Wenn Sie an **GetBytes** oder **GetChars** ein NULL-Array übergeben, entspricht der zurückgegebene long-Wert der Gesamtzahl von Bytes oder Zeichen im BLOB. Optional können Sie einen Index im Array als Anfangsposition für die gelesenen Daten angeben.

## <a name="example"></a>Beispiel

Im folgenden Beispiel werden die Herausgeber-ID und das Logo von der [**pubs**-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs) zurückgegeben. Die Herausgeber-ID (`pub_id`) ist ein Zeichenfeld, und das Logo ist ein Bild, das ein BLOB darstellt. Da das **logo**-Feld ein Bitmap ist, werden im Beispiel mithilfe von **GetBytes** Binärdaten zurückgegeben. Beachten Sie, dass für die aktuelle Datenzeile zuerst auf die Herausgeber-ID und dann auf das Logo zugegriffen wird, da der Zugriff auf die Felder der Reihe nach erfolgen muss.

[!code-csharp[SqlCommand_ExecuteReader_SequentialAccess#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteReader_SequentialAccess.cs#1)]

## <a name="see-also"></a>Weitere Informationen

- [Binäre Daten und Daten mit umfangreichen Werten in SQL Server](./sql/sql-server-binary-large-value-data.md)
- [Microsoft ADO.NET für SQL Server](microsoft-ado-net-sql-server.md)
