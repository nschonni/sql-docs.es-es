---
title: DROP SIGNATURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SIGNATURE
- DROP_SIGNATURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting signatures
- dropping signatures
- DROP SIGNATURE statement
- removing signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 8a1fd8c5-0e75-4b2f-9d3c-c296bed56cc7
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: b6a83bfc8691e883b69ff88e93514a0819d9eb67
ms.sourcegitcommit: 61381ef939415fe019285def9450d7583df1fed0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47777043"
---
# <a name="drop-signature-transact-sql"></a>DROP SIGNATURE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Quita una firma digital de un procedimiento almacenado, una función, un desencadenador o un ensamblado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DROP [ COUNTER ] SIGNATURE FROM module_name   
    BY <crypto_list> [ ,...n ]  
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | ASYMMETRIC KEY Asym_key_name  
```  
  
## <a name="arguments"></a>Argumentos  
 *module_name*  
 Es el nombre de un procedimiento almacenado, una función, un ensamblado o un desencadenador.  
  
 CERTIFICATE *cert_name*  
 Es el nombre de un certificado con el que está firmado el procedimiento almacenado, la función, el ensamblado o el desencadenador.  
  
 ASYMMETRIC KEY *Asym_key_name*  
 Es el nombre de una clave asimétrica con la que está firmado el procedimiento almacenado, la función, el ensamblado o el desencadenador.  
  
## <a name="remarks"></a>Notas  
 Para obtener más información acerca de las firmas, vea la vista de catálogo sys.crypt_properties.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER para el objeto y el permiso CONTROL para el certificado o la clave asimétrica. Si una clave privada asociada está protegida por una contraseña, el usuario también debe tener la contraseña.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita la firma del certificado `HumanResourcesDP` desde el procedimiento almacenado `HumanResources.uspUpdateEmployeeLogin`.  
  
```  
USE AdventureWorks2012;  
DROP SIGNATURE FROM HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
## <a name="see-also"></a>Ver también  
 [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)   
 [ADD SIGNATURE &#40;Transact-SQL&#41;](../../t-sql/statements/add-signature-transact-sql.md)  
  
  
