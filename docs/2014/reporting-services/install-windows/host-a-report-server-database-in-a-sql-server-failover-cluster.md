---
title: Hospedar una base de datos del servidor de informes en un clúster de conmutación por error de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 7bd5f019-2857-452f-a023-cc3b9e93aec4
caps.latest.revision: 4
author: markingmyname
ms.author: maghan
manager: jhubbard
ms.openlocfilehash: d2a0a0fc008c2bdb08786d7cf32f76310f259b6d
ms.sourcegitcommit: 5dd5cad0c1bbd308471d6c885f516948ad67dfcf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36105768"
---
# <a name="host-a-report-server-database-in-a-sql-server-failover-cluster"></a>Hospedar una base de datos del servidor de informes en un clúster de conmutación por error de SQL Server
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el uso de clústeres de conmutación por error para que se puedan utilizar varios discos para una o más instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El uso de clústeres de conmutación por error solamente se admite para la base de datos del servidor de informes; no se puede ejecutar el servicio del servidor de informes como parte de un clúster de conmutación por error.  
  
 Para hospedar una base de datos del servidor de informes en un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el clúster debe estar previamente instalado y configurado. A continuación, puede seleccionar el clúster de conmutación por error como nombre del servidor al crear la base de datos del servidor de informes en la página Instalación de base de datos de la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Aunque el servicio del servidor de informes no puede participar en un clúster de conmutación por error, se puede instalar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en un equipo que tenga instalado un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El servidor de informes se ejecuta de manera independiente del clúster de conmutación por error. Si se instala un servidor de informes en un equipo que forma parte de una instancia de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no es obligatorio utilizar el clúster de conmutación por error para la base de datos del servidor de informes; para hospedarla, se puede utilizar otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Vea también  
 [Base de datos del servidor de informes &#40;modo nativo de SSRS&#41;](../report-server/report-server-database-ssrs-native-mode.md)   
 [Crear una base de datos del servidor de informes &#40;Administrador de configuración de SSRS&#41;](../../sql-server/install/create-a-report-server-database-ssrs-configuration-manager.md)  
  
  