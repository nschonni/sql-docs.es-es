---
title: 'Pdostatement:: ErrorCode | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4161abec-c12b-444e-9de5-f1dac7b3e0e4
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5d83174f1ddc9dfdeb41bf3df1feaf196ce11c53
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47743053"
---
# <a name="pdostatementerrorcode"></a>PDOStatement::errorCode
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Recupera el valor de SQLSTATE de la operación más reciente realizada en el objeto de instrucción de base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
string PDOStatement::errorCode();  
```  
  
## <a name="return-value"></a>Valor devuelto  
Devuelve un valor de SQLSTATE de cinco caracteres como una cadena o Null si no hay ninguna operación en el identificador de instrucción.  
  
## <a name="remarks"></a>Notas  
En la versión 2.0 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], se agregó compatibilidad con PDO.  
  
## <a name="example"></a>Ejemplo  
En este ejemplo se muestra una consulta SQL con un error.  Después, se muestra el código de error.  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks", "", "");  
$stmt = $conn->prepare('SELECT * FROM Person.Addressx');  
  
$stmt->execute();  
print $stmt->errorCode();  
?>  
```  
  
## <a name="see-also"></a>Ver también  
[Clase PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
