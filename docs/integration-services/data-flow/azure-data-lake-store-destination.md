---
title: Destino de Azure Data Lake Store | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSDEST.F1
- sql14.dts.designer.afpadlsdest.f1
ms.assetid: 4c4f504f-dd2b-42c5-8a20-1a8ad9a5d632
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e1e2119358dc374dec7f2f70cf455c39d17d0870
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47765692"
---
# <a name="azure-data-lake-store-destination"></a>Destino de Azure Data Lake Store
  El componente **Destino de Azure Data Lake Store** permite que un paquete SSIS escriba datos en un Azure Data Lake Store. Los formatos de archivo admitidos son: Text, Avro y ORC. 
  
 El **Destino de Azure Data Lake Store** es un componente de [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
 
 >   [!NOTE]
 > Para garantizar que el Administrador de conexiones de Azure Data Lake Store y los componentes que lo utilizan (es decir, Origen de Data Lake Store y Destino de Azure Data Lake Store) se puedan conectar a los servicios, descargue la versión más reciente de Azure Feature Pack [aquí](https://www.microsoft.com/download/details.aspx?id=49492). 

## <a name="configure-the-azure-data-lake-store-destination"></a>Configurar Destino de Azure Data Lake Store  
1. Arrastre y coloque **Destino de Azure Data Lake Store** en el diseñador de flujos de datos y haga doble clic en él para ver el editor.  

2.  En el campo **Administrador de conexiones de Azure Data Lake Store** , especifique un administrador de conexiones de Azure Data Lake Store o cree uno nuevo que haga referencia a un servicio de Azure Data Lake Store.  
  
    1.  En el campo **Ruta de acceso del archivo** , especifique el nombre de archivo en el que quiere escribir datos. Si el archivo ya existe, se sobrescribirá su contenido.  
  
    2.  En el campo **Formato de archivo** , especifique el formato que quiere utilizar.  
  
        Si el formato de archivo es Text, debe especificar el valor **Carácter de delimitador de columna** . Seleccione también **Nombres de columna de la primera fila de datos** si la primera fila del archivo contiene nombres de columna.  

        Si el formato de archivo es ORC, debe instalar JRE de la plataforma correspondiente. 
  
3.  Después de especificar la información de conexión, cambie a la página **Columnas** para asignar columnas de origen a columnas de destino para el flujo de datos SSIS.  
  
  
