---
title: El estado de sincronización de datos de alguna base de datos de disponibilidad no es correcto | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- dbe-high-availability
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- sql12.swb.agdashboard.drp3datasynchealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 89f95d15-33c6-4768-bccd-9dbf8c4f49a9
caps.latest.revision: 13
author: rothja
ms.author: jroth
manager: jhubbard
ms.openlocfilehash: 6aacc147cebd6a7387cb9025f4fb2b5483a92a37
ms.sourcegitcommit: 5dd5cad0c1bbd308471d6c885f516948ad67dfcf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36104785"
---
# <a name="data-synchronization-state-of-some-availability-database-is-not-healthy"></a>El estado de sincronización de datos de alguna base de datos de disponibilidad no está en buen estado
    
## <a name="introduction"></a>Introducción  
  
|||  
|-|-|  
|**Nombre de directiva**|Estado de sincronización de datos de las réplicas de disponibilidad|  
|**Problema**|El estado de sincronización de datos de alguna base de datos de disponibilidad no es correcto.|  
|**Categoría**|**Advertencia**|  
|**Faceta**|Réplica de disponibilidad|  
  
## <a name="description"></a>Descripción  
 Esta directiva comprueba el estado de sincronización de datos de la base de datos de disponibilidad (también denominada “réplica de base de datos”). La directiva está en mal estado cuando el estado de sincronización de datos es NOT SYNCHRONIZING o cuando no es SYNCHRONIZED para la réplica de base de datos de confirmación sincrónica.  
  
> [!NOTE]  
>  Para esta versión de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], la información sobre las posibles causas y soluciones se encuentra en [El estado de sincronización de datos de la base de datos de disponibilidad no es correcto](http://go.microsoft.com/fwlink/p/?LinkId=220863) en la wiki de TechNet.  
  
## <a name="possible-causes"></a>Posibles causas  
 Al menos una base de datos de disponibilidad en la réplica tiene un estado incorrecto de sincronización de datos. Si se trata de una réplica de disponibilidad de confirmación asincrónica, todas las bases de datos de disponibilidad deben estar en el estado SYNCHRONIZING. Si se trata de una réplica de disponibilidad de confirmación sincrónica, todas las bases de datos de disponibilidad deben estar en el estado SYNCHRONIZED. Este problema puede deberse a lo siguiente:  
  
-   La réplica de disponibilidad puede estar desconectada.  
  
-   El movimiento de datos puede haberse suspendido.  
  
-   La base de datos no está accesible.  
  
-   Puede haber un problema de retraso temporal debido a la latencia de red o a la carga en la réplica principal o secundaria.  
  
## <a name="possible-solution"></a>Solución posible  
 Resuelva cualquier problema de suspensión de conexión o de movimiento de datos. Compruebe los eventos correspondientes a este problema mediante SQL Server Management Studio y busque el error de la base de datos. Realice los siguientes pasos para solucionar problemas del error concreto.  
  
## <a name="see-also"></a>Vea también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](overview-of-always-on-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  