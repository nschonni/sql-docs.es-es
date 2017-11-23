---
title: sp_columns (Transact-SQL) | Documentos de Microsoft
ms.custom: 
ms.date: 10/17/2016
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: 
ms.component: system-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs: TSQL
helpviewer_keywords: sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
caps.latest.revision: "45"
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: On Demand
ms.openlocfilehash: 7ea208a7c7c5c1cb969bfa556a5be27b32e5a856
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2017
---
# <a name="spcolumns-transact-sql"></a>sp_columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Devuelve la información de columna para los objetos especificados, que se pueden consultar en el entorno actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure SQL Data Warehouse, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [  **@table_name=**] *objeto*  
 Es el nombre del objeto para devolver la información de catálogo. *objeto* puede ser una tabla, vista u otro objeto que tenga columnas como funciones con valores de tabla. *objeto* es **nvarchar (384)**, no tiene ningún valor predeterminado. Se admite la coincidencia de patrón de caracteres comodín.  
  
 [  **@table_owner*** =**] *propietario*  
 Es el propietario del objeto utilizado para devolver la información de catálogo. *propietario* es **nvarchar (384)**, su valor predeterminado es null. Se admite la coincidencia de patrón de caracteres comodín. Si *propietario* no se especifica, se aplican las reglas predeterminadas de visibilidad de objeto del DBMS subyacente.  
  
 Si el usuario actual posee un objeto con el nombre especificado, se devuelven las columnas de ese objeto. Si *propietario* no se especifica y el usuario actual no posee un objeto con los valores especificados *objeto*, **sp_columns** busca un objeto con los valores especificados  *objeto* pertenecen al propietario de la base de datos. Si existe uno, se devuelven las columnas de ese objeto.  
  
 [  **@table_qualifier*** =**] *calificador*  
 Es el nombre del calificador de objeto. *calificador de* es **sysname**, su valor predeterminado es null. Varios productos DBMS admiten nombres de tres partes para objetos (*calificador***.** *propietario***.** *nombre*). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre de servidor del entorno de base de datos del objeto.  
  
 [  **@column_name=**] *columna*  
 Es una sola columna y se utiliza cuando solo se desea una columna de información del catálogo. *columna* es **nvarchar (384)**, su valor predeterminado es null. Si *columna* no es se especifica, se devuelven todas las columnas. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], *columna* representa el nombre de columna como se muestra en el **syscolumns** tabla. Se admite la coincidencia de patrón de caracteres comodín. Para obtener una interoperabilidad máxima, el cliente de puerta de enlace solo debe dar por supuesta la coincidencia de patrón estándar de SQL-92 (caracteres comodín % y _).  
  
 [  **@ODBCVer=**] *ODBCVer*  
 Es la versión de ODBC que se está usando. *ODBCVer* es **int**, su valor predeterminado es 2. Esto indica ODBC Versión 2. Los valores válidos son 2 ó 3. Para las diferencias de comportamiento entre las versiones 2 y 3, vea el SDK de ODBC **SQLColumns** especificación.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 Ninguno  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 El **sp_columns** procedimiento almacenado de catálogo es equivalente a **SQLColumns** en ODBC. Los resultados devueltos se ordenan por **TABLE_QUALIFIER**, **TABLE_OWNER**, y **TABLE_NAME**.  
  
|Nombre de columna|Tipo de datos|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nombre del calificador del objeto. Este campo puede ser NULL.|  
|**TABLE_OWNER**|**sysname**|Nombre del propietario del objeto. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre del objeto. Este campo siempre devuelve un valor.|  
|**COLUMN_NAME**|**sysname**|Nombre de columna para cada columna de la **TABLE_NAME** devuelto. Este campo siempre devuelve un valor.|  
|**DATA_TYPE**|**smallint**|Código entero del tipo de datos ODBC. Si se trata de un tipo de datos que no se puede asignar a un tipo ODBC, se considera NULL. El nombre de tipo de datos nativo se devuelve en el **TYPE_NAME** columna.|  
|**TYPE_NAME**|**sysname**|Cadena que representa un tipo de datos. El DBMS subyacente presenta este nombre del tipo de datos.|  
|**PRECISIÓN**|**int**|Número de dígitos significativos. El valor devuelto para la **precisión** columna está expresado en base 10.|  
|**LENGTH**|**int**|Tamaño de transferencia de los datos. <sup>1</sup>|  
|**ESCALA**|**smallint**|Número de dígitos a la derecha del separador decimal.|  
|**BASE**|**smallint**|Base para tipos de datos numéricos.|  
|**QUE ACEPTAN VALORES NULL**|**smallint**|Especifica la nulabilidad.<br /><br /> 1 = Se admiten valores NULL.<br /><br /> 0 = No se admiten valores NULL.|  
|**COMENTARIOS**|**varchar(254)**|Este campo siempre devuelve NULL.|  
|**COLUMN_DEF**|**nvarchar(4000)**|Valor predeterminado de la columna.|  
|**SQL_DATA_TYPE**|**smallint**|Valor del tipo de datos SQL tal como aparece en el campo TYPE del descriptor. Esta columna es el mismo que el **DATA_TYPE** columna, excepto para la **datetime** y SQL-92 **intervalo** tipos de datos. Esta columna siempre devuelve un valor.|  
|**SQL_DATETIME_SUB**|**smallint**|Código de subtipo para **datetime** y SQL-92 **intervalo** tipos de datos. Para otros tipos de datos, esta columna devuelve NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longitud máxima en bytes de una columna de tipos de datos de caracteres o enteros. Para los demás tipos de datos, esta columna devuelve NULL.|  
|**ORDINAL_POSITION**|**int**|Posición ordinal de la columna en el objeto. La primera columna del objeto es 1. Esta columna siempre devuelve un valor.|  
|**IS_NULLABLE**|**varchar(254)**|Indica si la columna admite valores NULL en el objeto. Se siguen las normas ISO para determinar la nulabilidad. Un DBMS que cumpla la norma ISO SQL no puede devolver una cadena vacía.<br /><br /> YES = La columna puede incluir valores NULL.<br /><br /> NO = La columna no puede incluir valores NULL.<br /><br /> Esta columna devuelve una cadena de longitud cero si no se conoce la nulabilidad.<br /><br /> Devuelve el valor de esta columna es diferente del valor devuelto para la **NULLABLE** columna.|  
|**SS_DATA_TYPE**|**tinyint**|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizado por procedimientos almacenados extendidos. Para obtener más información, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
 <sup>1</sup> para obtener más información, consulte la documentación de Microsoft ODBC.  
  
## <a name="permissions"></a>Permissions  
 Requiere permisos SELECT y VIEW DEFINITION en el esquema.  
  
## <a name="remarks"></a>Comentarios  
 **sp_columns** cumple los requisitos para los identificadores delimitados. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información de columna para una tabla especificada.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se devuelve información de columna para una tabla especificada.  
  
```  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>Vea también  
 [sp_tables &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [Catálogo de procedimientos almacenados &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

