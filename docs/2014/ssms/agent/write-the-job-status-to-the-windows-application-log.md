---
title: Escribir el estado de un trabajo en el registro de aplicación de Windows | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- status information [SQL Server], jobs
- SQL Server Agent jobs, status
- writing job status to log
- jobs [SQL Server Agent], status
- logs [SQL Server], jobs
ms.assetid: 3b813702-8f61-40ec-bf3b-ce9deb7e68be
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ec23171d809252bc7ebb669db1da020dd1a9862f
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48223035"
---
# <a name="write-the-job-status-to-the-windows-application-log"></a>Escribir el estado de un trabajo en el registro de aplicación de Windows
  En este tema se describe cómo configurar el Agente [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] para escribir el estado de un trabajo en el registro de eventos de aplicaciones Windows utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]u Objetos de administración de SQL Server.  
  
 Estas respuestas permiten a los administradores de las bases de datos saber cuándo se completan los trabajos y con qué frecuencia se ejecutan. Algunas respuestas de trabajos típicas son:  
  
-   Notificar al operador mediante correo electrónico, localizador electrónico o un mensaje de **net send** . Use una de estas respuestas de trabajo si el operador debe realizar una acción de seguimiento. Por ejemplo, si un trabajo de copia de seguridad se realiza correctamente, se debe notificar de ello al operador para que quite la cinta de copia de seguridad y la guarde en un lugar seguro.  
  
-   Escribir un mensaje de evento en el registro de aplicación Windows. Puede utilizar esta respuesta solo para trabajos con errores.  
  
-   Eliminar automáticamente el trabajo. Utilice esta respuesta de trabajo si está seguro de que no necesita volver a ejecutar este trabajo.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para escribir el estado de un trabajo en el registro de aplicaciones Windows, utilizando:**  
  
     [SQL Server Management Studio](#SSMS)  
  
     [objetos de administración de SQL Server](#SMO)  
  
##  <a name="BeforeYouBegin"></a> Antes de empezar  
  
###  <a name="Security"></a> Seguridad  
 Para obtener información detallada, vea [Implement SQL Server Agent Security](implement-sql-server-agent-security.md).  
  
##  <a name="SSMS"></a> Usar SQL Server Management Studio  
  
#### <a name="to-write-job-status-to-the-windows-application-log"></a>Para escribir el estado de un trabajo en el registro de aplicación Windows  
  
1.  En el **Explorador de objetos** , conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y, después, expándala.  
  
2.  Expanda **Agente SQL Server**, expanda **Trabajos**, haga clic con el botón derecho en el trabajo que desee editar y haga clic en **Propiedades**.  
  
3.  Seleccione la página **Notificaciones** .  
  
4.  Seleccione **Escribir en el registro de eventos de aplicación Windows**y elija una de las siguientes opciones:  
  
    -   Haga clic en**Si el trabajo tiene éxito**para registrar el estado del trabajo cuando este se complete correctamente.  
  
    -   Haga clic en**Si el trabajo no tiene éxito**para registrar el estado del trabajo cuando este no se complete correctamente.  
  
    -   Haga clic en**Si el trabajo termina** para registrar el estado del trabajo independientemente del estado de finalización.  
  
##  <a name="SMO"></a> Usar objetos de administración de SQL Server  
 **Para escribir el estado de un trabajo en el registro de aplicación Windows**  
  
 Llame a la propiedad `EventLogLevel` de la clase `Job` mediante el lenguaje de programación que desee, como Visual Basic, Visual C# o PowerShell.  
  
 En el ejemplo de código siguiente se establece el trabajo para generar una entrada del registro de eventos del sistema operativo cuando finaliza la ejecución del trabajo.  
  
 **PowerShell**  
  
```  
$srv = new-object Microsoft.SqlServer.Management.Smo.Server("(local)")  
$jb = new-object Microsoft.SqlServer.Management.Smo.Agent.Job($srv.JobServer, "Test Job")  
$jb.EventLogLevel = [Microsoft.SqlServer.Management.Smo.Agent.CompletionAction]::Always  
```  
  
  
