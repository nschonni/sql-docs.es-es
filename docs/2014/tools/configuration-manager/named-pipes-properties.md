---
title: Propiedades de canalizaciones con nombre | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology:
- configmgr-client
ms.topic: conceptual
helpviewer_keywords:
- pipes [SQL Server]
- listening [SQL Server], pipes
- pipes [SQL Server], listening on pipes
- Named Pipes [SQL Server], listening on pipes
ms.assetid: a5fd5b8e-f889-485b-89e3-d4010ec4c6ec
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: de562f78dbf2898267a909524226387096145bee
ms.sourcegitcommit: 3da2edf82763852cff6772a1a282ace3034b4936
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48177177"
---
# <a name="named-pipes-properties"></a>Propiedades de Canalizaciones con nombre
  Utilice la página **Protocolo**del cuadro de diálogo **Named Pipes Properties** (Propiedades de canalizaciones con nombre) para ver o cambiar la canalización con nombre que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha, cuando se utiliza el protocolo Canalizaciones con nombre.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para habilitar o deshabilitar el protocolo, o cambiar la canalización con nombre.  
  
## <a name="options"></a>Opciones  
 **Enabled**  
 Los valores posibles **Yes** y **No**.  
  
 **Nombre de canalización**  
 Especifica la canalización con nombre en la que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha. De manera predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escucha en: `\\.\pipe\sql\query` para la instancia predeterminada y `\\.\pipe\MSSQL$<instancename>\sql\query` para una instancia con nombre. Este campo tiene un límite de 2.047 caracteres.  
  
## <a name="creating-an-alternate-named-pipe"></a>Crear una canalización con nombre alternativa  
 Para cambiar la canalización con nombre, escriba el nuevo nombre de canalización en el cuadro **Nombre de canalización** y, a continuación, detenga y reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Como **sql\query** se conoce ampliamente como la canalización con nombre usada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el cambio de la canalización puede ayudar a reducir el riesgo de ataques por programas malintencionados.  
  
### <a name="example"></a>Ejemplo  
 Escriba **\\\\.\pipe\unit\app** para escuchar en la canalización **unit\app** .  
  
 Escriba **\\\\.\pipe\acct** para escuchar en la canalización **acct** .  
  
## <a name="see-also"></a>Vea también  
 [Habilitar o deshabilitar un protocolo de red de servidor](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [Elegir un protocolo de red](../../../2014/tools/configuration-manager/choosing-a-network-protocol.md)   
 [Crear una cadena de conexión válida con canalizaciones con nombre](../../../2014/tools/configuration-manager/creating-a-valid-connection-string-using-named-pipes.md)  
  
  
