---
title: "Método setFetchDirection (SQLServerStatement) | Documentos de Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- drivers
ms.tgt_pltfrm: 
ms.topic: article
apiname:
- SQLServerStatement.setFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 18176517-2fb3-4266-924d-0f01253083d2
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: f7e6274d77a9cdd4de6cbcaef559ca99f77b3608
ms.openlocfilehash: 510736faf5189cf596c96809f6601fb38e50977d
ms.contentlocale: es-es
ms.lasthandoff: 09/09/2017

---
# <a name="setfetchdirection-method-sqlserverstatement"></a>Método setFetchDirection (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Proporciona [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] una sugerencia sobre la dirección en qué resultado se deben procesar las filas del conjunto.  
  
> [!NOTE]  
>  Actualmente, el controlador JDBC omite la sugerencia que proporcionada este método.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setFetchDirection(int nDir)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *nDir*  
  
 Un **int** que indica la dirección, que puede ser uno de los siguientes valores de procesamiento de fila:  
  
 FETCH_FORWARD  
  
 FETCH_REVERSE  
  
 FETCH_UNKNOWN  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Comentarios  
 Este método setFetchDirection especificado por el método setFetchDirection en la interfaz java.sql.Statement.  
  
## <a name="see-also"></a>Vea también  
 [Miembros de SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  