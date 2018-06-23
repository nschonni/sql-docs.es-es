---
title: Herramientas de cálculo (pestaña cálculos, Diseñador de cubos) (Analysis Services - datos multidimensionales) | Documentos de Microsoft
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- analysis-services
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- sql12.asvs.cubeeditor.calculationsview.calculationtoolspane.f1
ms.assetid: b1aa8a1a-6532-45d2-8f53-d3e211d7197a
caps.latest.revision: 26
author: Minewiskan
ms.author: owend
manager: mblythe
ms.openlocfilehash: 106821212578d341292be4ef5f33bef055d5c1c8
ms.sourcegitcommit: 5dd5cad0c1bbd308471d6c885f516948ad67dfcf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36202630"
---
# <a name="calculation-tools-calculations-tab-cube-designer-analysis-services---multidimensional-data"></a>Herramientas de cálculo (pestaña Cálculos, Diseñador de cubos) (Analysis Services - Datos multidimensionales)
  Use el panel **Herramientas de cálculo** en la pestaña **Cálculos** del Diseñador de cubos para explorar metadatos, funciones y plantillas disponibles para utilizarlos en cálculos.  
  
## <a name="options"></a>Opciones  
 **Metadatos**  
 Muestra los metadatos del cubo seleccionado.  
  
 Arrastre el elemento seleccionado al panel Editor de script, Editor de Formulario de miembro calculado o Editor de Formulario de conjunto con nombre para incluir la sintaxis de las Expresiones multidimensionales (MDX) de dicho elemento en la ubicación seleccionada del panel.  
  
 **Funciones**  
 Muestra las funciones disponibles para las expresiones y condiciones.  
  
 Arrastre el elemento seleccionado al panel **Editor de script**, **Editor de Formulario de miembro calculado**o **Editor de Formulario de conjunto con nombre** para incluir la sintaxis MDX de dicho elemento en la ubicación seleccionada del panel.  
  
> [!NOTE]  
>  En el modo de proyecto, el cuadro de diálogo **Herramientas de cálculo** lee la información de esta opción desde un archivo XML con el nombre MDXFunctions.xml, incluido en [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. En el modo en línea, la información de esta opción se recupera del conjunto de filas del esquema MDSCHEMA_FUNCTIONS para la instancia.  
  
 **Templates** (Plantillas [C++])  
 Muestra las plantillas predefinidas disponibles para los miembros calculados, los conjuntos con nombre y los comandos de script.  
  
 Arrastre el elemento seleccionado al panel **Editor de script**, **Editor de Formulario de miembro calculado**o **Editor de Formulario de conjunto con nombre** para incluir la sintaxis MDX de dicho elemento en la ubicación seleccionada del panel.  
  
## <a name="context-menu"></a>Menú contextual  
 Las siguientes opciones están disponibles en el menú contextual que se muestra al hacer clic con el botón derecho en un elemento del panel **Herramientas de cálculo** :  
  
 **Copiar**  
 Seleccione esta opción para copiar el elemento seleccionado en **Metadatos** o **Funciones** en el Portapapeles.  
  
> [!NOTE]  
>  No se mostrará esta opción si se selecciona **Plantillas** .  
  
> [!NOTE]  
>  Se deshabilitará esta opción si no es posible copiar el miembro seleccionado, al igual que la carpeta **Conjuntos** de una dimensión mostrada en **Metadatos** o la carpeta del grupo de función para una función mostrada en **Funciones**.  
  
 **Filtrar miembros**  
 Seleccione esta opción para mostrar el cuadro de diálogo **Filtrar miembros** y filtrar los miembros del elemento seleccionado en **Metadatos**. Para más información sobre el cuadro de diálogo **Filtrar miembros**, vea [Cuadro de diálogo Filtrar miembros &#40;Analysis Services - Datos multidimensionales&#41;](filter-members-dialog-box-analysis-services-multidimensional-data.md).  
  
> [!NOTE]  
>  Solo se mostrará esta opción si se selecciona **Metadatos** .  
  
> [!NOTE]  
>  Solo se habilitará esta opción si se selecciona un nivel para un atributo en **Metadatos**.  
  
 **Agregar plantilla**  
 Seleccione esta opción para agregar un nuevo miembro calculado, un conjunto con nombre o un comando de script basado en la plantilla seleccionada al script del cubo y para mostrar el **Editor de script**, el **Editor de Formulario de miembro calculado**o el **Editor de Formulario de conjunto con nombre** según sea necesario para dicho comando (en la vista Formulario) o para desplazarse por el contenido del panel **Editor de script** hasta la ubicación del comando en el script del cubo (en la Vista de script).  
  
> [!NOTE]  
>  Solo se mostrará esta opción si se selecciona **Metadatos** .  
  
## <a name="see-also"></a>Vea también  
 [Diseñador de cubos &#40;Analysis Services - datos multidimensionales&#41;](cube-designer-analysis-services-multidimensional-data.md)   
 [Barra de herramientas &#40;pestaña cálculos, Diseñador de cubos&#41; &#40;Analysis Services - datos multidimensionales&#41;](toolbar-calculations-tab-cube-designer-analysis-services-multidimensional-data.md)   
 [Organizador de script &#40;pestaña cálculos, Diseñador de cubos&#41; &#40;Analysis Services - datos multidimensionales&#41;](script-organizer-cube-designer-analysis-services-multidimensional-data.md)   
 [Editor de formulario de miembro calculado &#40;pestaña cálculos, Diseñador de cubos&#41; &#40;Analysis Services - datos multidimensionales&#41;](calculated-member-form-editor-cube-designer-analysis-services-multidimensional-data.md)   
 [Editor de formulario de conjuntos con nombres &#40;pestaña cálculos, Diseñador de cubos&#41; &#40;Analysis Services - datos multidimensionales&#41;](named-set-form-editor-cube-designer-analysis-services-multidimensional-data.md)   
 [Editor de secuencia de comandos &#40;pestaña cálculos, Diseñador de cubos&#41; &#40;Analysis Services - datos multidimensionales&#41;](script-editor-calculations-cube-designer-analysis-services-multidimensional-data.md)   
 [Cálculos &#40;Diseñador de cubos&#41; &#40;Analysis Services - datos multidimensionales&#41;](calculations-cube-designer-analysis-services-multidimensional-data.md)  
  
  