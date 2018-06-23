---
title: Importación de datos (Master Data Services) | Documentos de Microsoft
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- staging process [Master Data Services], about staging process
- importing data [Master Data Services]
- staging process [Master Data Services]
ms.assetid: 181d1e22-379c-45d1-b03c-e1e22ff14164
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: jhubbard
ms.openlocfilehash: d386070df790164e11763d0dfc459cb7de2c1a95
ms.sourcegitcommit: 5dd5cad0c1bbd308471d6c885f516948ad67dfcf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36104721"
---
# <a name="data-import-master-data-services"></a>Importación de datos (Master Data Services)
  Una vez que haya creado un modelo para los datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], puede empezar a agregar datos y realizar cambios a los datos en el [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] base de datos.   Puede usar tablas de almacenamiento provisional de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , procedimientos almacenados y Master Data Manager.  
  
 También puede usar el [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)], para agregar datos en el repositorio MDS ([!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] base de datos). Para obtener más información, consulte [publicar datos &#40;complemento MDS para Excel&#41;](microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md).  
  
 Al agregar y actualizar los datos, puede hacer lo siguiente.  
  
-   Cargar y actualizar miembros, así como actualizar valores de atributo  
  
-   Desactivar o eliminar miembros  
  
-   Mover miembros de jerarquías explícitas  
  
 La adición y la actualización de datos incluyen las siguientes tareas principales.  
  
1.  Cargar datos en las tablas de almacenamiento provisional en la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
2.  Cargar datos de las tablas de almacenamiento provisional en las tablas adecuadas de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
     Puede usar los procedimientos almacenados de almacenamiento provisional o [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] para cargar los datos.  
  
> [!NOTE]  
>  En [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], la compatibilidad con los procesos de almacenamiento provisional de [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] está en desuso.  
  
## <a name="deactivating-and-deleting-members"></a>Desactivación y eliminación de miembros  
 La desactivación significa que el miembro se puede reactivar. Si reactiva un miembro, se restauran sus atributos y su pertenencia a jerarquías y colecciones. Todas las transacciones anteriores quedan intactas. Las transacciones desactivación son visibles para los administradores en el área funcional de **Administración de versiones** de Master Data Manager.  
  
 Eliminar significa purgar el miembro del sistema permanentemente. Todas las transacciones del miembro, todas las relaciones y todos los atributos se eliminan de forma permanente.  
  
> [!NOTE]  
>  No se puede utilizar el almacenamiento provisional para reactivar los miembros. Se debe hacer manualmente en Master Data Manager. Para obtener más información, consulte [Reactivar un miembro o una colección &#40;Master Data Services&#41;](reactivate-a-member-or-collection-master-data-services.md).  
>   
>  No se puede utilizar el almacenamiento provisional para eliminar o desactivar colecciones. Para obtener más información sobre cómo desactivar colecciones de forma manual, consulte [Eliminar un miembro o una colección &#40;Master Data Services&#41;](../../2014/master-data-services/delete-a-member-or-collection-master-data-services.md).  
  
## <a name="moving-explicit-hierarchy-members"></a>Movimiento de miembros de jerarquías explícitas  
 Al mover la ubicación de los miembros de jerarquías explícitas de forma masiva, puede designar lo siguiente.  
  
-   Un miembro consolidado como elemento primario de un miembro consolidado.  
  
-   Un miembro consolidado como elemento primario de un miembro hoja.  
  
-   Un miembro hoja es un elemento relacionado de una hoja o de un miembro consolidado.  
  
-   Un miembro consolidado es un elemento relacionado de una hoja o de un miembro consolidado.  
  
## <a name="staging-tables-and-stored-procedures"></a>Tablas y procedimientos almacenados de de almacenamiento provisional  
 La base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] incluye los siguientes tipos de tablas de almacenamiento provisional que puede rellenar con sus datos.  
  
-   [Tabla de ensayo de miembros de hoja &#40;Master Data Services&#41;](../../2014/master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Consolidado de la tabla de ensayo de miembros &#40;Master Data Services&#41;](../../2014/master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Tabla de ensayo de relaciones &#40;Master Data Services&#41;](../../2014/master-data-services/relationship-staging-table-master-data-services.md)  
  
 Para cada entidad del modelo, hay una tabla de almacenamiento provisional. El nombre de la tabla indica la entidad correspondiente y el tipo de entidad, como miembro hoja. La siguiente imagen muestra las tablas de almacenamiento provisional de las entidades de moneda, cliente y producto.  
  
 ![Tablas de almacenamiento provisional en la base de datos de MDS](../../2014/master-data-services/media/mds-stagingtables.png "Tablas de almacenamiento provisional en la base de datos de MDS")  
  
 El nombre de cada tabla se especifica cuando se crea una entidad y no se puede cambiar. Si el nombre de la tabla de ensayo contiene _1 u otro número, otra tabla con ese nombre ya existía cuando se creó la entidad.  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] incluye los siguientes tipos de procedimientos almacenados de almacenamiento provisional.  
  
-   stg.udp_\<name>_Leaf  
  
-   stg.udp_\<name>_Consolidated  
  
-   stg.udp_\<name>_Relationship  
  
 Para cada entidad del modelo, hay tres procedimientos almacenados que corresponden a las tablas de almacenamiento provisional de miembros hoja, miembros consolidados y relaciones.  La siguiente imagen muestra los procedimientos almacenados de almacenamiento provisional de las entidades de moneda, cliente y producto.  
  
 ![Procedimientos almacenados de almacenamiento provisional en la base de datos MDS](../../2014/master-data-services/media/mds-stagingstoredprocedures.png "procedimientos almacenados de almacenamiento provisional en la base de datos MDS")  
  
 Para obtener más información sobre los procedimientos almacenados, consulte [Procedimiento almacenado de almacenamiento provisional &#40;Master Data Services&#41;](../../2014/master-data-services/staging-stored-procedure-master-data-services.md).  
  
## <a name="logging-transactions"></a>Registrar transacciones  
 Se pueden registrar todas las transacciones que se producen cuando se importan o actualizan datos o relaciones. Una opción del procedimiento almacenado permite este registro. Si inicia el proceso de almacenamiento provisional mediante [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], no se realiza ningún registro.  
  
 En [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], el valor de **Registrar todas las transacciones de almacenamiento provisional** no se aplica a este método de almacenamiento provisional de datos.  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   [Validación &#40;Master Data Services&#41;](../../2014/master-data-services/validation-master-data-services.md)  
  
-   [Las reglas de negocios &#40;Master Data Services&#41;](../../2014/master-data-services/business-rules-master-data-services.md)  
  
  