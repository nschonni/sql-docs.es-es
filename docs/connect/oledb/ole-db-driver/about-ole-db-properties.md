---
title: Acerca de las propiedades OLE DB | Microsoft Docs
description: Acerca de las propiedades de OLE DB
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, properties
- OLE DB Driver for SQL Server, properties
- properties [OLE DB]
- property values [OLE DB Driver for SQL Server]
author: pmasl
ms.author: pelopes
manager: craigg
ms.openlocfilehash: f514cdf3e115ed381d758aa2f94605e7a1b1cb2b
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47664873"
---
# <a name="about-ole-db-properties"></a>Acerca de las propiedades de OLE DB
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Los consumidores establecen valores de propiedades para solicitar el comportamiento de un objeto específico. Por ejemplo, los consumidores usan propiedades para especificar las interfaces que va a exponer un conjunto de filas. Los consumidores obtienen los valores de las propiedades para determinar las capacidades de un objeto, como un conjunto de filas, una sesión o un objeto de origen de datos.  
  
 Cada propiedad incluye un valor, un tipo, una descripción y un atributo de lectura/escritura y, en el caso de las propiedades de conjunto de filas, un indicador de si puede aplicarse columna por columna.  
  
 Una propiedad se identifica mediante un GUID y un número entero que representa el identificador de propiedad. Un conjunto de propiedades es un conjunto de todas las propiedades que comparten el mismo GUID. Además de los conjuntos de propiedades predefinidos de OLE DB, el controlador OLE DB para SQL Server implementa conjuntos de propiedades específicos del proveedor, así como sus propiedades. Cada propiedad pertenece a uno o varios grupos de propiedades. Un grupo de propiedades es el grupo de todas las propiedades que se aplican a un objeto determinado. Algunos grupos de propiedades incluyen el grupo de propiedades de inicialización, el grupo de propiedades de origen de datos, el grupo de propiedades de sesión, el grupo de propiedades de conjunto de filas, el grupo de propiedades de tabla y el grupo de propiedades de columna. Hay propiedades en todos estos grupos de propiedades.  
  
 El establecimiento de valores de propiedades implica:  
  
1.  Determinar las propiedades para las que van a establecerse valores.  
  
2.  Determinar los conjuntos de propiedades que contienen las propiedades identificadas.  
  
3.  Asignar una matriz de estructuras DBPROPSET, una para cada conjunto de propiedades identificado.  
  
4.  Asignar una matriz de estructuras DBPROP para cada conjunto de propiedades. El número de elementos de cada matriz es igual al número de propiedades (identificado en el paso 1) que pertenecen a ese conjunto de propiedades.  
  
5.  Rellenar la estructura DBPROP para cada propiedad.  
  
6.  Rellenar la información (GUID del conjunto de propiedades, recuento del número de elementos y un puntero a la matriz DBPROP correspondiente) de la estructura DBPROPSET para cada conjunto de propiedades.  
  
7.  Llamar a un método para establecer las propiedades y pasar el recuento y la matriz de estructuras DBPROPSET.  
  
## <a name="see-also"></a>Ver también  
 [Creación de un controlador OLE DB para la aplicación de SQL Server](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md)   
 [Propiedades (OLE DB)](http://go.microsoft.com/fwlink/?LinkId=112207)  
  
  
