---
title: Habilitar copias de seguridad coordinadas para la replicación transaccional | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- transactional replication, backup and restore
- sp_replicationdboption
- sync with backup [SQL Server replication]
- coordinated backups [SQL Server replication]
- backups [SQL Server replication], transactional replication
ms.assetid: 73a914ba-8b2d-4f4d-ac1b-db9bac676a30
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8a42406af2eb1a3133ffb3e6e65699f75f08bf46
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47660423"
---
# <a name="enable-coordinated-backups-for-transactional-replication"></a>Habilitar copias de seguridad coordinadas para la replicación transaccional
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Al habilitar una base de datos para la replicación transaccional, puede especificar que se tenga que realizar una copia de seguridad de todas las transacciones antes de ser entregadas a la base de datos de distribución. También puede habilitar la copia de seguridad coordinada en la base de datos de distribución para que el registro de transacciones de la base de datos de publicación no se trunque hasta que se haya realizado una copia de seguridad de las transacciones que se han propagado al distribuidor. Para más información, consulte [Estrategias para hacer copias de seguridad y restaurar replicación de instantáneas o replicación transaccional](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
### <a name="to-enable-coordinated-backups-for-a-database-published-with-transactional-replication"></a>Para habilitar las copias de seguridad coordinadas de una base de datos publicada con replicación transaccional  
  
1.  En el publicador, use la función [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md) para devolver la propiedad **IsSyncWithBackup** de la base de datos de publicación. Si la función devuelve **1**, las copias de seguridad coordinadas ya están habilitadas para la base de datos publicada.  
  
2.  Si la función del paso 1 devuelve **0**, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) en la base de datos de publicación del publicador. Especifique un valor de **sync with backup** para **@optname**y **true** para **@value**.  
  
    > [!NOTE]  
    >  Si cambia la opción **sync with backup** a **false**, el punto de la truncación de la base de datos de publicación estará actualizado después de que el Agente de registro del LOG se ejecute o después de un intervalo si el Agente de registro del LOG está ejecutando permanentemente. El parámetro del agente **–MessageInterval** , que tiene un valor predeterminado de 30 segundos, controla el intervalo máximo.  
  
### <a name="to-enable-coordinated-backups-for-a-distribution-database"></a>Para habilitar las copias de seguridad coordinadas para una base de datos de distribución  
  
1.  En el distribuidor, use la función [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md) para devolver la propiedad **IsSyncWithBackup** de la base de datos de distribución. Si la función devuelve **1**, las copias de seguridad coordinadas ya están habilitadas para la base de datos de distribución.  
  
2.  Si la función del paso 1 devuelve **0**, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) en la base de datos de distribución del distribuidor. Especifique un valor de **sync with backup** para **@optname** y **true** para **@value**.  
  
### <a name="to-disable-coordinated-backups"></a>Para deshabilitar las copias de seguridad coordinadas  
  
1.  En el publicador de la base de datos de publicación o en el distribuidor de la base de datos de distribución, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Especifique un valor de **sync with backup** para **@optname** y **false** para **@value**.  
  
  
