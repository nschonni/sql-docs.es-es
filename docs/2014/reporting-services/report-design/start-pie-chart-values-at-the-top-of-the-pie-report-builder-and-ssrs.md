---
title: Iniciar los valores del gráfico circular desde la parte superior del gráfico (Generador de informes y SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- reporting-services-native
ms.topic: conceptual
ms.assetid: d0e6fb59-ca4e-4d70-97cb-0ad183da21d3
author: maggiesMSFT
ms.author: maggies
manager: craigg
ms.openlocfilehash: fba0cbf880348fa05f32e26fe8a47644aab10924
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48119205"
---
# <a name="start-pie-chart-values-at-the-top-of-the-pie-report-builder-and-ssrs"></a>Iniciar los valores del gráfico circular desde la parte superior del gráfico (Generador de informes y SSRS)
  De forma predeterminada en los gráficos circulares, el primer valor del conjunto de datos se inicia en 90 grados desde la parte superior del círculo. Puede preferir que el primer valor se inicie desde arriba.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-start-the-pie-chart-at-the-top-of-the-pie"></a>Iniciar el gráfico circular desde la parte superior del círculo  
  
1.  Haga clic en el círculo.  
  
2.  Si el panel **Propiedades** no está abierto, en la pestaña **Ver** haga clic en **Propiedades**.  
  
3.  En el panel **Propiedades** , en **Atributos personalizados**, cambie **PieStartAngle** de **0** a **270**.  
  
4.  Haga clic en **Ejecutar** para obtener una vista previa del informe.  
  
 El primer valor se inicia ahora en la parte superior del gráfico circular.  
  
## <a name="see-also"></a>Vea también  
 [Aplicar formato a un gráfico &#40;Generador de informes y SSRS&#41;](formatting-a-chart-report-builder-and-ssrs.md)   
 [Los gráficos circulares &#40;generador de informes y SSRS&#41;](charts-report-builder-and-ssrs.md)  
  
  
