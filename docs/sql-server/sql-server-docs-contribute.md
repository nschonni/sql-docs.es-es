---
title: Cómo colaborar en la documentación de SQL Server | Microsoft Docs
ms.date: 08/13/2018
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: rothja
ms.author: jroth
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 881189fdec593d48b443d85ee548ca1bb80b24a8
ms.sourcegitcommit: 7fe14c61083684dc576d88377e32e2fc315b7107
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50146060"
---
# <a name="how-to-contribute-to-sql-server-documentation"></a>Cómo colaborar en la documentación de SQL Server

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Cualquier usuario puede colaborar en la documentación de SQL Server. Implica la corrección de errores tipográficos, la sugerencia de explicaciones mejoradas y la mejora de la precisión técnica. En este artículo se explica cómo empezar con las colaboraciones de contenido y cómo funciona el proceso.

Hay dos flujos de trabajo principales que se pueden aplicar para colaborar:

|||
|---|---|
| [Edición en el navegador](#githubui) | Es útil para ediciones rápidas y breves de cualquier artículo. |
| [Edición local con herramientas](#tools) | Es útil para ediciones más complejas, ediciones que afecten a varios artículos y colaboraciones frecuentes en docs.microsoft.com. |

## <a id="githubui"></a> Edición en el navegador

Puede realizar modificaciones sencillas en el contenido de SQL Server en el explorador y, después, enviarlas a Microsoft. El proceso completo está documentado en el artículo [Guía para colaboradores de Microsoft Docs: información general](https://docs.microsoft.com/contribute/#quick-edits-to-existing-documents). En el vídeo siguiente se muestra el proceso completo para enviar los cambios en el explorador:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE23pxh]

> [!TIP]
> Tenga en cuenta que la ubicación del botón **Editar** es ligeramente diferente a la que se muestra en el vídeo, pero el proceso es el mismo.
>
> ![Botón Editar](./media/sql-server-docs-contribute/edit-sql-server-docs.png)

## <a id="tools"></a> Edición local con herramientas

Otra opción de edición consiste en bifurcar el repositorio **sql-docs** o **azure-docs** y clonarlo localmente en el equipo. Después puede usar un editor de Markdown y un cliente de Git para enviar los cambios. Este flujo de trabajo es útil para las ediciones que son más complejas o que afectan a varios archivos. También es práctico para los colaboradores frecuentes de docs.microsoft.com.

Para colaborar con este método, consulte los siguientes artículos:

- [Creación de una cuenta de GitHub](https://docs.microsoft.com/contribute/get-started-setup-github)
- [Instalación de herramientas de creación de contenido](https://docs.microsoft.com/contribute/get-started-setup-tools)
- [Configuración local de un repositorio de Git](https://docs.microsoft.com/contribute/get-started-setup-local)
- [Uso de herramientas para la colaboración](https://docs.microsoft.com/contribute/how-to-write-workflows-major)

Si envía una solicitud de incorporación de cambios con cambios importantes en la documentación, recibirá un comentario en GitHub en el que se le pedirá que envíe un **contrato de licencia de colaboración (CLA)** en línea. Debe cumplimentar el formulario en línea para que se pueda aceptar la solicitud de incorporación de cambios.

## <a name="recognition"></a>Reconocimiento

Si se aceptan los cambios, será reconocido como colaborador en la parte superior del artículo.

![Reconocimiento de la colaboración en contenido](./media/sql-server-docs-contribute/contribution-recognition.png)

## <a name="sql-docs-overview"></a>información general de sql-docs

En esta sección se muestran instrucciones adicionales sobre cómo trabajar en el repositorio **sql-docs**.

> [!IMPORTANT]
> La información de esta sección es específica de **sql-docs**. Si edita un artículo de SQL en la documentación de Azure, vea [el archivo Léame del repositorio azure-docs en GitHub](https://github.com/MicrosoftDocs/azure-docs/blob/master/README.md).

El repositorio [sql-docs](https://github.com/MicrosoftDocs/sql-docs) usa muchas carpetas estándares para organizar el contenido.

| Carpeta | Descripción |
|---|---|
| [docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs) | Contiene todo el contenido publicado de SQL Server. Las subcarpetas organizan de forma lógica distintas áreas del contenido. |
| [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) | Contiene archivos de inclusión. Estos archivos son bloques de contenido que se pueden incluir en uno o más temas. |
| **./media** | Cada carpeta puede tener una subcarpeta **media** para imágenes de artículos. La carpeta **media** tiene a su vez subcarpetas con el mismo nombre que los temas en los que aparece la imagen. Las imágenes deben ser archivos .png con todas las letras en minúsculas y sin espacios en blanco. |
| **TOC.MD** | Archivo de tabla de contenidos. Cada subcarpeta tiene la opción de usar un archivo TOC.MD. |

#### <a name="applies-to-includes"></a>Archivos de inclusión applies-to

Todos los artículos de SQL Server contienen un archivo de inclusión **applies-to** al final del título. Indica las áreas o las versiones de SQL Server a las que se aplica el artículo.

Observe el siguiente ejemplo de Markdown, que incorpora el archivo de inclusión **appliesto-ss-asdb-asdw-pdw-md.md**.

```Markdown
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
```

De esta manera se agrega el siguiente texto en la parte superior del artículo:

![Texto "Se aplica a"](./media/sql-server-docs-contribute/applies-to.png)

Para buscar el archivo de inclusión applies-to correcto para su artículo, aplique las siguientes sugerencias:

- Para obtener una lista de los archivos de inclusión más utilizados, vea ["Applies to" e "Includes" de SQL](applies-to-includes.md).
- Consulte otros artículos que aborden la misma función o una tarea relacionada. Si edita ese mismo artículo, puede copiar el Markdown del vínculo del archivo de inclusión applies-to (puede cancelar la edición sin enviarla).
- Busque el directorio [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) de los archivos que contienen el texto "applies-to". Puede usar el botón **Find** (Buscar) en GitHub para filtrar rápidamente. Haga clic en el archivo para ver cómo se representa.
- Preste atención a la convención de nomenclatura. Si hay x en el nombre, suelen ser marcadores de posición que indican la falta de compatibilidad de un servicio. Por ejemplo, **appliesto-xx-xxxx-asdw-xxx-md.md** indica compatibilidad con solo Azure SQL Data Warehouse, porque solo se especifica **asdw** y los demás campos contienen x.
- Algunos archivos de inclusión especifican un número de versión, como **tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md**. Use estos archivos de inclusión únicamente si sabe que la característica se ha introducido con una versión específica de SQL Server.

## <a name="contributor-resources"></a>Recursos para los colaboradores

- [Guía del colaborador de docs.microsoft.com](https://docs.microsoft.com/contribute/)
- [Guía de estilo de Microsoft](https://docs.microsoft.com/teamblog/style-guide)
- [Markdown basics](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) (Conceptos básicos de Markdown)

> [!TIP]
> Si tiene comentarios sobre el producto en vez de comentarios sobre la documentación, [aquí puede enviar comentarios sobre el producto de SQL Server](https://feedback.azure.com/forums/908035-sql-server).

## <a name="next-steps"></a>Pasos siguientes

Explore el [repositorio sql-docs](https://github.com/MicrosoftDocs/sql-docs) en GitHub.

Busque un artículo, envíe un cambio y ayude a la comunidad de SQL Server. 

¡Gracias!
