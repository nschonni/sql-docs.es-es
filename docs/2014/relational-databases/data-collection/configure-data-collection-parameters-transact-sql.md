---
title: Configurar parámetros para la recopilación de datos (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
ms.assetid: 850905b6-35d2-4ed1-ab51-de64daa832b2
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0261adbe86f1320984b4c56790835cf8cfd492cb
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48099925"
---
# <a name="configure-data-collection-parameters-transact-sql"></a>Configurar parámetros para la recopilación de datos (Transact-SQL)
  Antes de crear un conjunto de recopilación personalizado, primero debe configurar los parámetros de recopilación de datos. Puede hacerlo mediante los procedimientos almacenados que se proporcionan con el recopilador de datos. Para realizar esta tarea debe usar el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y llevar a cabo el siguiente procedimiento.  
  
> [!NOTE]  
>  Solo se configuran una vez los parámetros de recopilación de datos. Tras la configuración, esos parámetros se usan para cualquier conjunto de recopilación adicional que cree.  
  
### <a name="configure-data-collection-parameters"></a>Configurar parámetros para la recopilación de datos  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], conéctese a la base de datos donde desea crear un conjunto de recopilación personalizado.  
  
2.  En el Editor de consultas, emita las instrucciones siguientes.  
  
    ```tsql  
    USE msdb;  
    EXEC sp_syscollector_set_warehouse_instance_name N'INSTANCE_NAME';-- where instance name is the name of the SQL Server instance  
    EXEC sp_syscollector_set_warehouse_database_name N'MDW';  
    EXEC sp_syscollector_set_cache_directory N'D:\tempdata';  
    ```  
  
## <a name="see-also"></a>Vea también  
 [Recopilación de datos](data-collection.md)   
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql)  
  
  
