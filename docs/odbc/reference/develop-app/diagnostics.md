---
title: Diagnóstico | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- diagnostic information [ODBC]
- functions [ODBC], diagnostic information
- diagnostic information [ODBC], about diagnostic information
ms.assetid: 450abd88-90a1-4fbc-b417-8efbdd8e1dea
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c0c7ddfeda7538027c56af17664e5962d09903b1
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47686353"
---
# <a name="diagnostics"></a>Diagnósticos
Funciones de ODBC devuelven información de diagnóstico de dos maneras. El código de retorno indica el éxito o error de la función, general, mientras que los registros de diagnóstico proporcionan información detallada acerca de la función. Al menos un registro de diagnóstico, el registro de encabezado, se devuelve incluso si la función es correcta.  
  
 Información de diagnóstico se usa en tiempo de desarrollo para detectar errores de programación como identificadores no válidos y errores de sintaxis en las instrucciones SQL codificadas de forma rígida. En tiempo de ejecución se usa para detectar errores de tiempo de ejecución y las advertencias como errores de sintaxis, las infracciones de acceso y el truncamiento de datos en instrucciones SQL escritas por el usuario.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Códigos de retorno](../../../odbc/reference/develop-app/return-codes-odbc.md)  
  
-   [Registros de diagnóstico](../../../odbc/reference/develop-app/diagnostic-records.md)  
  
-   [Uso de SQLGetDiagRec y SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md)  
  
-   [Implementar SQLGetDiagRec y SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md)  
  
-   [Ejemplos de control de diagnóstico](../../../odbc/reference/develop-app/diagnostic-handling-examples.md)
