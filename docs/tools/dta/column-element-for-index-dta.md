---
title: Elemento de la columna de índice (DTA) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology:
- database-engine
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Column element
ms.assetid: ba9fac20-26bd-4333-940e-842c15241b46
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 6fa488250a0dd7d49a47c22c09401395ac5e1949
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MTE75
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47684183"
---
# <a name="column-element-for-index-dta"></a>Column (DTA, elemento de Index)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Establece las columnas de una configuración especificada por el usuario en las que se va a crear el índice.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<Create>  
  <Index>  
    <Name>...</Name>  
    <Column [Type | SortOrder]>  
     ...code removed here...  
     </Column>  
```  
  
## <a name="element-attributes"></a>Atributos del elemento  
  
 **Type**: opcional. Especifica el tipo de columna de índice. Utilice un tipo de datos **string** para especificar este atributo mediante uno de los siguientes valores permitidos:  
  
-   **KeyColumn**  
  
     Especifica que una clave de índice hace referencia a la columna. Utilice la siguiente sintaxis para establecer este atributo:  
  
    ```  
    <Column Type="KeyColumn">  
    ```  
  
     Para obtener más información sobre las columnas de claves, vea [Índices agrupados y no agrupados descritos](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md).  
  
-   **IncludedColumn**  
  
     Especifica que la columna es una columna incluida (en lugar de una columna de clave). Utilice la siguiente sintaxis para establecer este atributo:  
  
    ```  
    <Column Type="IncludedColumn">  
    ```  
  
     Para obtener más información sobre las columnas incluidas, vea [Crear índices con columnas incluidas](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
 **SortOrder**: opcional. Especifica el orden de la columna. Utilice un tipo de datos **string** para especificar un orden **"Ascending"** (ascendente) o **"Descending"** (descendente), como se muestra a continuación:  
  
```  
<Column SortOrder="Ascending">  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|Ninguno.|  
|**Valor predeterminado**|Ninguno.|  
|**Repetición**|Puede especificar hasta 1.024 columnas para el elemento **Index** .|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[Index &#40;DTA, elemento&#41;](../../tools/dta/index-element-dta.md)|  
|**Elementos secundarios**|[Elemento Name de Column &#40;DTA&#41;](../../tools/dta/name-element-for-column-dta.md)|  
  
## <a name="example"></a>Ejemplo  
 Para obtener un ejemplo de uso de este elemento, vea [Ejemplo de archivo de entrada XML con configuración especificada por el usuario &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Ver también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
