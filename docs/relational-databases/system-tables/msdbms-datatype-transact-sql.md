---
title: MSdbms_datatype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology:
- replication
ms.topic: language-reference
f1_keywords:
- MSdbms_datatype
- MSdbms_datatype_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_datatype system table
ms.assetid: 606168cc-79a8-442f-ab43-936f8f884d72
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 65110b04cec21d1b6d5b840fb9b3cc10d4f98e45
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47807053"
---
# <a name="msdbmsdatatype-transact-sql"></a>MSdbms_datatype (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  El **MSdbms_datatype** tabla contiene una lista completa de tipos de datos nativos en cada sistema de administración de bases de datos (DBMS) utilizado como publicador o suscriptor en la replicación de base de datos heterogéneos. Esta tabla se almacena en el **msdb** base de datos.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**datatype_id**|**int**|Identifica cada tipo de datos único.|  
|**dbms_id**|**int**|Identifica el DBMS al que pertenece el tipo.|  
|**Tipo**|**sysname**|Nombre del tipo de datos (nativos).|  
|**CreateParams**|**int**|Mapa de bits que describe la combinación de longitud, precisión y escala aplicable a cada tipo de datos, que incluye:<br /><br /> **0 x 1** = precisión.<br /><br /> **0 x 2** = escala.<br /><br /> **0 x 4** = longitud.|  
  
## <a name="remarks"></a>Comentarios  
 Esta tabla contiene entradas para los tipos de datos de SQL Server porque una instancia de SQL Server puede suscribirse a una base de datos que no son de SQL Server y publicar para un suscriptor no SQL Server.  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Especificar asignaciones de tipo de datos para un publicador de Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Las tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
