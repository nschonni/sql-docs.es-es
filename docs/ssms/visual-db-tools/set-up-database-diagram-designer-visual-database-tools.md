---
title: Configurar el Diseñador de diagramas de base de datos (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.diagnostic.InstallSqlDiagramSupport
helpviewer_keywords:
- Database Diagram Designer
- database diagrams [SQL Server], Database Diagram Designer
- diagrams [SQL Server], Database Diagram Designer
ms.assetid: 927321ee-b459-4f5b-9719-4a7a95639143
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a593627965e006336b06a1afd1f391f3e175d46e
ms.sourcegitcommit: 9f2edcdf958e6afce9a09fb2e572ae36dfe9edb0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50098597"
---
# <a name="set-up-database-diagram-designer-visual-database-tools"></a>Configurar el Diseñador de diagramas de base de datos (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Para usar el diseñador de diagramas de base de datos, un miembro del rol **db_owner** debe configurarlo primero para controlar el acceso a los diagramas.  
  
### <a name="to-set-up-database-diagramming"></a>Para configurar diagramas de base de datos  
  
1.  En el Explorador de objetos, expanda un nodo de base de datos.  
  
2.  Expanda el nodo de Diagramas de base de datos bajo la conexión de bases de datos.  
  
3.  Seleccione **Sí** cuando se le pregunte si desea configurar la creación de diagramas de base de datos.  
  
    > [!NOTE]  
    > De esta forma creará la tabla de diagrama de base de datos, los procedimientos almacenados del sistema y una función del sistema en la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
4.  Visual Studio creará los siguientes objetos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
    1.  sysdiagrams - Tabla  
  
    2.  sp_alterdiagam - Procedimiento almacenado  
  
    3.  sp_creatediagram - Procedimiento almacenado  
  
    4.  sp_dropdiagram - Procedimiento almacenado  
  
    5.  sp_renamediagram - Procedimiento almacenado  
  
    6.  fn_diagramobjects - Función  
  
    7.  sp_helpdiagrams - Procedimiento almacenado  
  
    8.  sp_helpdiagramsdefinition - Procedimiento almacenado  
  
    9. sp_upgraddiagrams - Procedimiento almacenado  
  
## <a name="see-also"></a>Ver también  
[Descripción de la propiedad de un diagrama de base de datos &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/understand-database-diagram-ownership-visual-database-tools.md)  
[Actualizar diagramas de base de datos de ediciones anteriores &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/upgrade-database-diagrams-from-previous-editions-visual-database-tools.md)  
[ALTER AUTHORIZATION (Transact-SQL)](http://msdn.microsoft.com/8c805ae2-91ed-4133-96f6-9835c908f373)  
  
