---
title: Configurar notificaciones por correo electrónico (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- e-mail [Master Data Services], configuring
- notifications [Master Data Services], configuring notifications
ms.assetid: 4241a6ab-7465-471b-9890-57c6b572037e
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: jhubbard
ms.openlocfilehash: 6484a5b978a50ab30cf63d00c48573012ad56b6f
ms.sourcegitcommit: 5dd5cad0c1bbd308471d6c885f516948ad67dfcf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36114057"
---
# <a name="configure-email-notifications-master-data-services"></a>Configurar notificaciones por correo electrónico (Master Data Services)
  Configure mensajes de correo electrónico de notificación si desea que [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] envíe mensajes de correo electrónico automáticamente.  
  
### <a name="to-configure-notifications"></a>Para configurar las notificaciones  
  
1.  En [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], en la página **Base de datos** , seleccione su base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
2.  En la sección **Configuración del sistema** , haga clic en **Crear perfil**.  
  
3.  Complete todos los campos obligatorios. Para obtener más información, consulte [Cuadro de diálogo Crear perfil y cuenta de Correo electrónico de base de datos &#40;Administrador de configuración de Master Data Services&#41;](../../2014/master-data-services/create-database-mail-profile-and-account-dialog-box.md).  
  
4.  Haga clic en **Aceptar**.  
  
    > [!NOTE]  
    >  Después de configurar las notificaciones, no puede utilizar [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] para realizar cambios. Debe realizar las modificaciones directamente  en la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Para más información, consulte [Database Mail Configuration Objects](../relational-databases/database-mail/database-mail-configuration-objects.md).  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Hay opciones de [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] que afectan a las notificaciones. Puede ajustarlas en [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] o directamente en la tabla Configuración del sistema de la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Para obtener más información, vea [Configuración del sistema &#40;Master Data Services&#41;](system-settings-master-data-services.md).  
  
## <a name="see-also"></a>Vea también  
 [Las notificaciones &#40;Master Data Services&#41;](../../2014/master-data-services/notifications-master-data-services.md)   
 [Solucionar problemas de notificaciones de correo electrónico (Master Data Services)](http://social.technet.microsoft.com/wiki/contents/articles/troubleshooting-email-notifications-master-data-services.aspx)   
 [Configurar reglas de negocios para enviar notificaciones &#40;Master Data Services&#41;](../../2014/master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)  
  
  