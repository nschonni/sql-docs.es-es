---
title: 'PDO:: ErrorInfo | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8216e6c9adbb2154a1e416510b989db252572cb2
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47690583"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera información ampliada de errores de la operación más reciente en el identificador de la base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Valor devuelto  
Una matriz de información de errores de la operación más reciente en el identificador de la base de datos. La matriz consta de los siguientes campos:  
  
-   El código de error de SQLSTATE  
  
-   El código de error específico del controlador  
  
-   El mensaje de error específico del controlador.  
  
Si no hay ningún error, o si no se establece el valor de SQLSTATE, los campos específicos del controlador serán NULL.  
  
## <a name="remarks"></a>Notas  
PDO::errorInfo solo recupera información de error para operaciones realizadas directamente en la base de datos. Utilice PDOStatement::errorInfo cuando se cree una instancia de PDOStatement con PDO::prepare o PDO::query.  
  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo, no se ha escrito correctamente el nombre de la columna (`Cityx` en lugar de `City`), lo que ha provocado un error, del que se informa después.  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  
  
## <a name="see-also"></a>Ver también  
[Clase PDO](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
