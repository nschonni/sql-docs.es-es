---
title: Ver un seguimiento guardado (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- database-engine
ms.topic: conceptual
helpviewer_keywords:
- traces [SQL Server], viewing
- displaying traces
- viewing traces
ms.assetid: 3a95a816-aa89-4d5f-858c-968a9cb3ee87
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 01558096bb33aec8ffd21841d59a39b2cc26c6c5
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48166691"
---
# <a name="view-a-saved-trace-transact-sql"></a>Ver un seguimiento guardado (Transact-SQL)
  En este tema se describe cómo utilizar las funciones integradas para ver un seguimiento guardado.  
  
### <a name="to-view-a-specific-trace"></a>Para ver un seguimiento específico  
  
1.  Ejecute **fn_trace_getinfo** especificando el identificador del seguimiento del que se precisa información. Esta función devuelve una tabla con el seguimiento, la propiedad del seguimiento e información acerca de la propiedad.  
  
     Llame a la función de esta manera:  
  
    ```  
    SELECT *  
    FROM ::fn_trace_getinfo(trace_id)  
    ```  
  
### <a name="to-view-all-existing-traces"></a>Para ver todas los seguimientos existentes  
  
1.  Ejecute **fn_trace_getinfo** especificando `0` o `default`. Esta función devuelve una tabla con todos los seguimiento, sus propiedades e información acerca de estas propiedades.  
  
     Invoque la función de la siguiente manera:  
  
    ```  
    SELECT *  
    FROM ::fn_trace_getinfo(default)  
    ```  
  
## <a name="net-framework-security"></a>Seguridad de .NET Framework  
 Para ejecutar la función integrada **fn_trace_getinfo**, el usuario necesita el siguiente permiso:  
  
 ALTER TRACE en el servidor.  
  
## <a name="see-also"></a>Vea también  
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](/sql/relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql)   
 [Ver y analizar seguimientos con SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)  
  
  
