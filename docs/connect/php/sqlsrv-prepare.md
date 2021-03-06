---
title: sqlsrv_prepare | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- sqlsrv_prepare
apitype: NA
helpviewer_keywords:
- executing queries
- API Reference, sqlsrv_prepare
- sqlsrv_prepare
ms.assetid: 8c74c697-3296-4f5d-8fb9-e361f53f19a6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9a293ff4bd2eae7f1e54914c805e4e180ce17fb2
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47669523"
---
# <a name="sqlsrvprepare"></a>sqlsrv_prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crea un recurso de instrucción asociado a la conexión especificada. Esta función resulta útil para la ejecución de varias consultas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlsrv_prepare(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Parámetros  
*$conn*: el recurso de conexión asociado a la instrucción creada.  
  
*$tsql*: expresión Transact-SQL que corresponde a la instrucción creada.  
  
*$params* [OPCIONAL]: **matriz** de valores que corresponden a parámetros de una consulta con parámetros. Cada elemento de la matriz puede ser uno de los siguientes:
  
-   Un valor literal  
  
-   Una referencia a una variable PHP  
  
-   Una **matriz** con la siguiente estructura:  
  
    ```  
    array(&$value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    > [!NOTE]  
    > Las variables transmitidas como parámetros de consulta se deben pasar por referencia en lugar de por valor. Por ejemplo, se debe transmitir `&$myVariable` en lugar de `$myVariable`. Se genera una advertencia de PHP cuando se ejecuta una consulta con parámetros por valor.  
  
    En la siguiente tabla se describen esos elementos de la matriz:  
  
    |Elemento|Descripción|  
    |-----------|---------------|  
    |*&$value*|Un valor literal o una referencia a una variable PHP.|  
    |*$direction*[opcional]|Una de las siguientes constantes **SQLSRV_PARAM_\*** usadas para indicar la dirección del parámetro: **SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. El valor predeterminado es **SQLSRV_PARAM_IN**.<br /><br />Para obtener más información sobre las constantes de PHP, vea [Constantes &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[opcional]|Constante **SQLSRV_PHPTYPE_\*** que especifica el tipo de datos PHP del valor devuelto.|  
    |*$sqlType*[opcional]|Constante **SQLSRV_SQLTYPE_\*** que especifica el tipo de datos de SQL Server del valor de entrada.|  
  
*$options* [OPCIONAL]: matriz asociativa que establece propiedades de consulta. En la siguiente tabla se indican las claves admitidas y los valores correspondientes:  
  
|Key|Valores admitidos|Descripción|  
|-------|--------------------|---------------|  
|QueryTimeout|Valor entero positivo.|Establece el tiempo de espera de consulta en segundos. De manera predeterminada, el controlador espera indefinidamente los resultados.|  
|SendStreamParamsAtExec|**True** o **False**<br /><br />El valor predeterminado es **true**.|Configura el controlador para enviar todos los datos de flujo en el momento de la ejecución (**true**) o para enviar los datos de flujo en fragmentos (**false**). De manera predeterminada, este valor se establece como **True**. Para obtener más información, consulte [sqlsrv_send_stream_data](../../connect/php/sqlsrv-send-stream-data.md).|  
|De desplazamiento|SQLSRV_CURSOR_FORWARD<br /><br />SQLSRV_CURSOR_STATIC<br /><br />SQLSRV_CURSOR_DYNAMIC<br /><br />SQLSRV_CURSOR_KEYSET<br /><br />SQLSRV_CURSOR_CLIENT_BUFFERED|Para obtener más información sobre estos valores, vea [Especificación de un tipo de cursor y selección de filas](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|  
  
## <a name="return-value"></a>Valor devuelto  
Un recurso de instrucción. Si no se puede crear el recurso de instrucción, se devuelve el valor **False** .  
  
## <a name="remarks"></a>Notas  
Al preparar una instrucción que usa variables como parámetros, estas se enlazan a la instrucción. Esto significa que si actualiza los valores de las variables, la próxima vez que se ejecute la instrucción, lo hará con los valores de parámetro actualizados.  
  
La combinación de **sqlsrv_prepare** y **sqlsrv_execute** separa la preparación y la ejecución de la instrucción en dos llamadas de función y se puede usar para ejecutar consultas con parámetros. Esta función resulta ideal para ejecutar una instrucción varias veces con distintos valores de parámetros para cada ejecución.  
  
Para consultar estrategias alternativas de lectura y escritura de grandes cantidades de información, vea [Lotes de instrucciones SQL](../../odbc/reference/develop-app/batches-of-sql-statements.md) y [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
Para obtener más información, consulte [How to: Retrieve Output Parameters Using the SQLSRV Driver](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se prepara y se ejecuta una instrucción. Cuando se ejecuta la instrucción (vea [sqlsrv_execute](../../connect/php/sqlsrv-execute.md)), se actualiza un campo de la tabla *Sales.SalesOrderDetail* de la base de datos AdventureWorks. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up Transact-SQL query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = ?   
         WHERE SalesOrderDetailID = ?";  
  
/* Assign parameter values. */  
$param1 = 5;  
$param2 = 10;  
$params = array(&$param1, &$param2);  
  
/* Prepare the statement. */  
if ($stmt = sqlsrv_prepare($conn, $tsql, $params)) {
    echo "Statement prepared.\n";  
} else {  
    echo "Statement could not be prepared.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement. */  
if (sqlsrv_execute($stmt)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Statement could not be executed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example"></a>Ejemplo  
En el ejemplo siguiente se muestra cómo preparar una instrucción y luego volver a ejecutarla con diferentes valores de parámetro. En el ejemplo se actualiza la columna *OrderQty* de la tabla *Sales.SalesOrderDetail* de la base de datos de AdventureWorks. Una vez finalizadas las actualizaciones, se consulta la base de datos para comprobar que las actualizaciones se hayan realizado correctamente. En el ejemplo se da por hecho que SQL Server y la base de datos [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) están instalados en el equipo local. Los resultados se agregan a la consola cuando se ejecuta el ejemplo en la línea de comandos.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
     echo "Could not connect.\n";  
     die(print_r(sqlsrv_errors(), true));  
}  
  
/* Define the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail  
         SET OrderQty = ?  
         WHERE SalesOrderDetailID = ?";  
  
/* Initialize parameters and prepare the statement. Variables $qty  
and $id are bound to the statement, $stmt1. */  
$qty = 0; $id = 0;  
$stmt1 = sqlsrv_prepare($conn, $tsql, array(&$qty, &$id));  
if ($stmt1) {  
    echo "Statement 1 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the SalesOrderDetailID and OrderQty information. This array  
maps the order ID to order quantity in key=>value pairs. */  
$orders = array(1=>10, 2=>20, 3=>30);  
  
/* Execute the statement for each order. */  
foreach ($orders as $id => $qty) {  
    // Because $id and $qty are bound to $stmt1, their updated  
    // values are used with each execution of the statement.   
    if (sqlsrv_execute($stmt1) === false) {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
echo "Orders updated.\n";  
  
/* Free $stmt1 resources.  This allows $id and $qty to be bound to a different statement.*/  
sqlsrv_free_stmt($stmt1);  
  
/* Now verify that the results were successfully written by selecting   
the newly inserted rows. */  
$tsql = "SELECT OrderQty   
         FROM Sales.SalesOrderDetail   
         WHERE SalesOrderDetailID = ?";  
  
/* Prepare the statement. Variable $id is bound to $stmt2. */  
$stmt2 = sqlsrv_prepare($conn, $tsql, array(&$id));  
if ($stmt2) {  
    echo "Statement 2 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement for each order. */  
foreach (array_keys($orders) as $id)  
{  
    /* Because $id is bound to $stmt2, its updated value   
    is used with each execution of the statement. */  
    if (sqlsrv_execute($stmt2)) {  
        sqlsrv_fetch($stmt2);  
        $quantity = sqlsrv_get_field($stmt2, 0);  
        echo "Order $id is for $quantity units.\n";  
    } else {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
  
/* Free $stmt2 and connection resources. */  
sqlsrv_free_stmt($stmt2);  
sqlsrv_close($conn);  
?>  
```  
  
> [!NOTE]
> Se recomienda usar cadenas como entradas al enlazar los valores para un [columna decimal o numeric](https://docs.microsoft.com/sql/t-sql/data-types/decimal-and-numeric-transact-sql) para garantizar la precisión y la precisión PHP tiene limitada la precisión para [números de punto flotante](http://php.net/manual/en/language.types.float.php). Lo mismo se aplica a las columnas de tipo bigint, especialmente cuando los valores que están fuera del intervalo de un [entero](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="example"></a>Ejemplo  
Este ejemplo de código muestra cómo enlazar un valor decimal como un parámetro de entrada.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"YourTestDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume TestTable exists with a decimal field 
$input = "9223372036854.80000";
$params = array($input);
$stmt = sqlsrv_prepare($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);
sqlsrv_execute($stmt);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Ver también  
[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Realización de consultas con parámetros](../../connect/php/how-to-perform-parameterized-queries.md)

[Sobre los ejemplos de código de la documentación](../../connect/php/about-code-examples-in-the-documentation.md)

[Envío de datos como una secuencia](../../connect/php/how-to-send-data-as-a-stream.md)

[Uso de parámetros direccionales](../../connect/php/using-directional-parameters.md)

[Recuperación de datos](../../connect/php/retrieving-data.md)

[Actualización de datos &#40;controladores de Microsoft para PHP para SQL Server&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)

