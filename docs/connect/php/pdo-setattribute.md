---
title: 'PDO:: SetAttribute | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 07/13/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 56f9ee96-e1d2-46cc-b137-38f06a251863
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 66d64917b83e23739ad35e8629b028904a300060
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47707688"
---
# <a name="pdosetattribute"></a>PDO::setAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Legt ein vordefiniertes PDO-Attribut oder ein benutzerdefiniertes Treiberattribut fest.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
bool PDO::setAttribute ( $attribute, $value );  
```  
  
#### <a name="parameters"></a>Parameter  
*$attribute*: festzulegendes Attribut Eine Liste der unterstützten Attribute finden Sie bei den Hinweisen.  
  
*$value*: Der Wert (gemischter Typ).  
  
## <a name="return-value"></a>Rückgabewert  
„true“ bei Erfolg, andernfalls „false“.  
  
## <a name="remarks"></a>Remarks  
  
|attribute|Verarbeitet von|Unterstützte Werte|und Beschreibung|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|Legt fest, ob die Spaltennamen in Groß- oder Kleinbuchstaben geschrieben werden.<br /><br />PDO::CASE_LOWER bewirkt, das die Spaltennamen in Kleinbuchstaben geschrieben werden.<br /><br />PDO::CASE_NATURAL (der Standard) zeigt die Spaltennamen so an, wie sie aus der Datenbank zurückgegeben werden.<br /><br />PDO::CASE_UPPER bewirkt das die Spaltennamen in Großbuchstaben geschrieben werden.<br /><br />Dieses Attribut kann mit PDO::setAttribute eingerichtet werden.|  
|PDO::ATTR_DEFAULT_FETCH_MODE|PDO|Siehe PDO-Dokumentation.|Siehe PDO-Dokumentation.|  
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|Gibt an, wie der Treiber Fehler meldet.<br /><br />PDO::ERRMODE_SILENT (der Standard) legt die Fehlercodes und -informationen fest.<br /><br />PDO::ERRMODE_WARNING löst E_WARNING aus.<br /><br />PDO::ERRMODE_EXCEPTION bewirkt, dass eine Ausnahme ausgelöst wird.<br /><br />Dieses Attribut kann mit PDO::setAttribute eingerichtet werden.|  
|PDO::ATTR_ORACLE_NULLS|PDO|Siehe PDO-Dokumentation.|Legt fest, wie NULL-Werte ausgegeben werden sollen.<br /><br />PDO::NULL_NATURAL nimmt keine Konvertierung vor.<br /><br />PDO::NULL_EMPTY_STRING wandelt eine leere Zeichenfolge in NULL um.<br /><br />PDO::NULL_TO_STRING wandelt NULL in eine leere Zeichenfolge um.|  
|PDO::ATTR_STATEMENT_CLASS|PDO|Siehe PDO-Dokumentation.|Legt die vom PDOStatement abgeleitete und vom Benutzer bereitgestellte Anweisungsklasse fest.<br /><br />Erfordert `array(string classname, array(mixed constructor_args))`.<br /><br />Weitere Informationen finden Sie in der PDO-Dokumentation.|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|true oder false|Wandelt beim Abruf der Daten Zahlenwerte in Zeichenfolgen um.|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|1 bis zur Grenze des PHP-Speichers.|Legt die Größe des Puffers fest, der bei der Verwendung eines clientseitigen Cursors das Resultset enthält.<br /><br />Die Standardeinstellung ist 10240 KB, wenn nicht in der Datei "PHP.ini" angegeben.<br /><br />0 (null) und negative Zahlen sind nicht zulässig.<br /><br />Weitere Informationen zu Abfragen, die clientseitige Cursors erstellen, finden Sie unter [Cursortypen &#40;PDO_SQLSRV-Treiber&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true<br /><br />false|Legt fest, ob eine direkte oder eine vorbereitete Anweisung ausgeführt wird. Weitere Informationen finden Sie unter [Direkte Anweisungsausführung und vorbereitete Anweisungsausführung im PDO_SQLSRV-Treiber](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM.|Legt die Zeichensatzcodierung fest, die vom Treiber verwendet wird, um mit dem Server zu kommunizieren.<br /><br />PDO::SQLSRV_ENCODING_BINARY wird nicht unterstützt.<br /><br />Der Standard ist PDO::SQLSRV_ENCODING_UTF8.|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true oder false|Verarbeitet die numerischen Abrufe aus Spalten mit numerischen SQL-Typen (Bit, Integer, Smallint, Tinyint, Float oder Real).<br /><br />Wenn Optionsflags für die Verbindung ATTR_STRINGIFY_FETCHES aktiviert ist, ist der Rückgabewert eine Zeichenfolge, selbst wenn SQLSRV_ATTR_FETCHES_NUMERIC_TYPE auf.<br /><br />Wenn der zurückgegebene PDO-Typ in Bindung Spalte PDO_PARAM_INT ist, ist der Rückgabewert aus einer Spalte mit ganzen Zahlen eine ganze Zahl an, auch wenn SQLSRV_ATTR_FETCHES_NUMERIC_TYPE deaktiviert ist.|  
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|integer|Legt das Abfragetimeout in Sekunden fest.<br /><br />Der Standard ist 0, d. h. dass der Treiber unendlich lange auf Ergebnisse wartet.<br /><br />Negative Zahlen sind nicht zulässig.|  
  
PDO verarbeitet einige der vordefinierten Attribute. Die übrigen müssen vom Treiber verarbeitet werden. Alle benutzerdefinierten Attribute und alle Verbindungsoptionen werden vom Treiber verarbeitet. Ein nicht unterstütztes Attribut, eine nicht unterstützte Verbindungsoption oder ein nicht unterstützter Wert wird laut den Einstellungen in PDO::ATTR_ERRMODE gemeldet.  
  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="example"></a>Beispiel  
Dieses Beispiel zeigt, wie das PDO::ATTR_ERRMODE-Attribut eingerichtet wird.  
  
```  
<?php  
   $database = "AdventureWorks";  
   $conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
  
   $attributes1 = array( "ERRMODE" );  
   foreach ( $attributes1 as $val ) {  
      echo "PDO::ATTR_$val: ";  
      var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
   }  
  
   $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
  
   $attributes1 = array( "ERRMODE" );  
   foreach ( $attributes1 as $val ) {  
      echo "PDO::ATTR_$val: ";  
      var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
   }  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen finden Sie unter  
[PDO-Klasse](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
