---
title: STGeometryN (tipo de datos geometry) | Documentos de Microsoft
ms.custom: 
ms.date: 08/03/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- STGeometryN_TSQL
- STGeometryN (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STGeometryN (geometry Data Type)
ms.assetid: 348c7047-3442-4590-8879-fe841e79058c
caps.latest.revision: 19
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: fe227cc574662025aa43016ca3f053ad36bf0678
ms.contentlocale: es-es
ms.lasthandoff: 09/01/2017

---
# <a name="stgeometryn-geometry-data-type"></a>STGeometryN (tipo de datos geometry)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Devuelve una geometría especificada en un **colección geometry**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STGeometryN ( expression )  
```  
  
## <a name="arguments"></a>Argumentos  
 *expression*  
 Es un **int** expresión entre 1 y el número de **geometry** instancias en el **geometrycollection**.  
  
## <a name="return-types"></a>Tipos devueltos  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]tipo de valor devuelto: **geometry**  
  
 Tipo de valor devuelto de CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Comentarios  
 Este método devuelve **null** si el parámetro es mayor que el resultado de `STNumGeometries()` y producirá una **ArgumentOutOfRangeException** si la *expresión* parámetro es menor que 1.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea un `MultiPoint``geometry collection` y utiliza `STGeometryN()` para hallar la segunda `geometry` instancia de la colección.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT(0 0, 13.5 2, 7 19)', 0);  
SELECT @g.STGeometryN(2).ToString();  
```  
  
## <a name="see-also"></a>Vea también  
 [Métodos de OGC en instancias de Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

