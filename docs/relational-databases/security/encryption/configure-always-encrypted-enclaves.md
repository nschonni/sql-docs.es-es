---
title: Configuración de Always Encrypted con enclaves seguros | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
manager: craigg
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 48580f2ca2e83a968f9599b98956c079f763bf71
ms.sourcegitcommit: 0acd84d0b22a264b3901fa968726f53ad7be815c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49307129"
---
# <a name="configure-always-encrypted-with-secure-enclaves"></a>Configuración de Always Encrypted con enclaves seguros
[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../../../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

[Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) amplía la característica [Always Encrypted](always-encrypted-database-engine.md) existente para ofrecer una funcionalidad más completa en los datos confidenciales mientras mantiene la confidencialidad de estos.

Para configurar Always Encrypted con enclaves seguros, use el siguiente flujo de trabajo:

1. Configure la atestación del Servicio de protección de host (HGS).
2. Instale [!INCLUDE[sql-server-2019](..\..\..\includes\sssqlv15-md.md)] en el equipo con SQL Server.
3. Instale herramientas en el equipo de desarrollo/cliente.
4. Configure el tipo de enclave en su instancia de SQL Server.
5. Aprovisione claves habilitadas para el enclave.
6. Cifre las columnas con datos confidenciales.

>[!NOTE]
>Para ver un tutorial paso a paso sobre cómo configurar el entorno de prueba y probar la funcionalidad de Always Encrypted con enclaves seguros en SSMS, consulte [Tutorial: Getting started with Always Encrypted with secure enclaves using SSMS](../tutorial-getting-started-with-always-encrypted-enclaves.md) (Tutorial: Introducción a Always Encrypted con enclaves seguros mediante SSMS).

## <a name="configure-your-environment"></a>Configurar su entorno

Para usar enclaves seguros con Always Encrypted, su entorno requiere Windows Server 2019 Preview, SQL Server Management Studio (SSMS) 18.0 (versión preliminar), .NET Framework y algunos componentes más. En las siguientes secciones se proporcionan detalles y vínculos para obtener los componentes necesarios.

### <a name="sql-server-computer-requirements"></a>Requisitos del equipo con SQL Server

El equipo que ejecuta SQL Server necesita el siguiente sistema operativo y la siguiente versión de SQL Server:

*SQL Server*:

- [!INCLUDE[sql-server-2019](..\..\..\includes\sssqlv15-md.md)] o posterior

*Windows*:

- Windows 10 Enterprise, versión 1809
- Windows Server Datacenter (canal semianual), versión 1809
- Windows Server 2019 Datacenter

> [!IMPORTANT]
> El equipo con SQL Server debe configurarse como host protegido, acreditado por HGS. La atestación de TPM es el método de atestación de enclave recomendado para entornos de producción y requiere que SQL Server se ejecute en una máquina física, no en una máquina virtual. Las máquinas virtuales son adecuadas solo para entornos de preproducción.

### <a name="hgs-computer-requirements"></a>Requisitos del equipo de HGS

Basta con un solo equipo de HGS durante las pruebas y la creación de prototipos. Para la producción, se recomienda totalmente un clúster de conmutación por error de Windows con tres equipos.

El Servicio de protección de host (HGS) de Windows debe instalarse en equipos de HGS independientes y no en el mismo equipo como SQL Server. Para obtener detalles sobre los requisitos del equipo de HGS y su configuración, consulte [Setting up the Host Guardian Service for Always Encrypted in SQL Server](https://docs.microsoft.com/windows-server/security/set-up-hgs-for-always-encrypted-in-sql-server) (Configuración del Servicio de protección de host para Always Encrypted en SQL Server).


### <a name="determine-your-attestation-service-url"></a>Determinar su dirección URL de servicio de atestación

Para determinar la dirección URL de servicio de atestación, debe configurar sus herramientas y aplicaciones:

1. Inicie sesión en su equipo con SQL Server como administrador.
2. Ejecute PowerShell como administrador.
3. Ejecute [Get-HGSClientConfiguration](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsclientconfiguration).
4. Escriba y guarde la propiedad AttestationServerURL. Debe tener un aspecto parecido a este: `http://x.x.x.x/Attestation`.


### <a name="install-tools"></a>Instalar herramientas

Instale las siguientes herramientas en el equipo de desarrollo/cliente:

1. [.NET Framework 4.7.2](https://www.microsoft.com/net/download/dotnet-framework-runtime).
2. [Versión 18.0 de SSMS o posterior](../../../ssms/download-sql-server-management-studio-ssms.md).
3. Versión 21.1 o posterior del [módulo de SQL Server PowerShell](../../../powershell/download-sql-server-ps-module.md).
4. [Visual Studio (se recomienda la versión de 2017 o posterior)](https://visualstudio.microsoft.com/downloads/).
5. [Paquete de desarrollador de .NET Framework 4.7.2](https://www.microsoft.com/net/download/visual-studio-sdks).
6. [Paquete NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider), versión 2.2.0 o posterior.
7. [Paquete NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages?q=Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders).

Los paquetes NuGet están pensados para su uso en proyectos de Visual Studio con el fin de desarrollar aplicaciones mediante Always Encrypted con enclaves seguros. Solo es necesario el primer paquete si almacena sus claves maestras de columna en Azure Key Vault. Para obtener detalles, consulte [Desarrollar aplicaciones](#develop-applications-issuing-rich-queries-in-visual-studio).

### <a name="configure-a-secure-enclave"></a>Configurar un enclave seguro

En el equipo de desarrollo/cliente:

1. Abra SSMS y conéctese a su instancia de SQL Server como administrador de Active Directory (AD).
2. Para comprobar que Always Encrypted con enclaves seguros se admite en su instancia, ejecute la siguiente consulta:

   ```sql
   SELECT [name], [value], [value_in_use] FROM sys.configurations
   WHERE [name] = 'column encryption enclave type'
   ```

    La consulta debería devolver una fila con un aspecto similar al siguiente:  

    | NAME                           | value | value_in_use |
    | ------------------------------ | ----- | -------------|
    | tipo de enclave de cifrado de columnas | 0     | 0            |

3. Configure el tipo de enclave seguro a enclaves de VBS.

   ```sql
   EXEC sys.sp_configure 'column encryption enclave type', 1
   RECONFIGURE
   ```

4. Reinicie su instancia de SQL Server para que el cambio anterior surta efecto. Puede reiniciar la instancia en SSMS haciendo clic con el botón derecho en ella en el Explorador de objetos y seleccionando Reiniciar. Una vez que se reinicie la instancia, vuelva a conectarse a ella.

5. Confirme que el enclave seguro se ha cargado ahora ejecutando la siguiente consulta:

   ```sql
   SELECT [name], [value], [value_in_use] FROM sys.configurations
   WHERE [name] = 'column encryption enclave type'
   ```   

    La consulta debería devolver una fila con un aspecto similar al siguiente:  

    | NAME                           | value | value_in_use |
    | ------------------------------ | ----- | -------------- |
    | tipo de enclave de cifrado de columnas | 1     | 1              |

6. Para habilitar cálculos completos en las columnas cifradas, ejecute la siguiente consulta:

   ```sql
   DBCC traceon(127,-1)
   ```

    > [!NOTE]
    > Los cálculos completos están deshabilitados de forma predeterminada en [!INCLUDE[sql-server-2019](..\..\..\includes\sssqlv15-md.md)]. Deben habilitarse mediante la instrucción anterior tras cada reinicio de su instancia de SQL Server.

## <a name="provision-enclave-enabled-keys"></a>Aprovisionar claves habilitadas para el enclave

Básicamente, la presentación de claves habilitadas para el enclave no cambia los [flujos de trabajo de administración y aprovisionamiento de claves para Always Encrypted](overview-of-key-management-for-always-encrypted.md). El único cambio es el flujo de trabajo de aprovisionamiento de claves maestras de columna, donde ahora puede marcar la clave como habilitada para el enclave (de forma predeterminada, las claves maestras de columna no están habilitadas para el enclave). Cuando especifique que la nueva clave maestra de columna va a estar habilitada para el enclave (con SSMS o PowerShell), ocurrirá lo siguiente:

- Se ha establecido la propiedad **ENCLAVE_COMPUTATIONS** en los metadatos de clave maestra de columna en la base de datos.
- Los valores de propiedad de la clave maestra de columna (incluida la configuración de **ENCLAVE_COMPUTATIONS**) se han firmado digitalmente. La herramienta agrega la firma, producida mediante la clave maestra de columna actual, a los metadatos. La finalidad de la firma es impedir que DBA y administradores de equipos malintencionados alteren la configuración de **ENCLAVE_COMPUTATIONS**. Los controladores cliente SQL comprueban las firmas antes de permitir el uso de enclaves. Esto proporciona a los administradores de seguridad control sobre qué datos de la columna se pueden calcular en el enclave.

La propiedad **ENCLAVE_COMPUTATIONS** de una clave maestra de columna es inmutable (no puede cambiarla una vez que se ha aprovisionado la clave). Puede, sin embargo, reemplazar la clave maestra de columna por una nueva clave con un valor de la propiedad **ENCLAVE_COMPUTATIONS** diferente al de la clave original, a través de un proceso llamado [rotación de clave maestra de columna](#initiate-the-rotation-from-the-current-column-master-key-to-the-new-column-master-key). Para obtener más información sobre la propiedad **ENCLAVE_COMPUTATIONS**, consulte [CREAR CLAVE MAESTRA DE COLUMNA](../../../t-sql/statements/create-column-master-key-transact-sql.md).

Para aprovisionar una clave de cifrado de columnas habilitada para el enclave, debe asegurarse de que la clave maestra de columna que cifra la clave de cifrado de columnas está habilitada para el enclave.

Las siguientes limitaciones se aplican actualmente a las claves habilitadas para el enclave de aprovisionamiento:

- Las claves maestras de columna habilitadas para el enclave **deben almacenarse en el almacén de certificados de Windows o en Azure Key Vault**. El almacenamiento de claves maestras de columna habilitadas para el enclave en otros tipos de almacenes de claves (módulos de seguridad de hardware o almacenes de claves personalizados) no se admite actualmente.

### <a name="provision-enclave-enabled-keys-using-sql-server-management-studio-ssms"></a>**Aprovisionamiento de claves habilitadas para el enclave mediante SQL Server Management Studio (SSMS)**

Los siguientes pasos crean claves habilitadas para el enclave (se requiere la versión 18.0 de SSMS o posterior):

1. Conéctese a su base de datos mediante SSMS.
2. En el **Explorador de objetos**, expanda su base de datos y vaya a **Seguridad** > **Claves de Always Encrypted**.
3. Aprovisione una nueva clave maestra de columna habilitada para el enclave:

    1. Haga clic con el botón derecho en **Claves de Always Encrypted** y seleccione **Nueva clave maestra de columna…**.
    2. Seleccione su nombre de clave maestra de columna.
    3. Asegúrese de seleccionar **Almacén de certificados de Windows (usuario actual o máquina local)** o **Azure Key Vault**.
    4. Seleccione **Permitir cálculos de enclave**.
    5. Si seleccionó Azure Key Vault, inicie sesión en Azure y seleccione su almacén de claves. Para obtener más información sobre cómo crear un almacén de claves para Always Encrypted, consulte [Manage your key vaults from Azure portal](https://blogs.technet.microsoft.com/kv/2016/09/12/manage-your-key-vaults-from-new-azure-portal/) (Administrar sus almacenes de claves desde Azure Portal).
    6. Seleccione su clave si ya existe o siga las indicaciones incluidas en el formulario para crear una nueva clave.
    7. Haga clic en **Aceptar**.

        ![Permitir cálculos de enclave](./media/always-encrypted-enclaves/allow-enclave-computations.png)

4. Cree una nueva clave de cifrado de columnas habilitada para el enclave:

    1. Haga clic con el botón derecho en **Claves de Always Encrypted** y seleccione **Nueva clave maestra de columna**.
    2. Escriba un nombre para la nueva clave de cifrado de columnas.
    3. En la lista desplegable **Clave maestra de columna**, seleccione la clave maestra de columna que creó en los pasos anteriores.
    4. Haga clic en **Aceptar**.

### <a name="provision-enclave-enabled-keys-using-powershell"></a>**Aprovisionamiento de claves habilitadas para el enclave mediante PowerShell**

En las siguientes secciones se proporcionan scripts de PowerShell de ejemplo para aprovisionar claves habilitadas para el enclave. Se resaltan los pasos que son específicos (nuevos) de Always Encrypted con enclaves seguros. Para obtener más información (no específica de Always Encrypted con enclaves seguros) sobre cómo aprovisionar claves mediante PowerShell, consulte [Configure Always Encrypted Keys using PowerShell](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell) (Configurar claves de Always Encrypted con PowerShell).

**Aprovisionamiento de claves habilitadas para el enclave: almacén de certificados de Windows**

En el equipo de desarrollo/cliente, abra Windows PowerShell ISE y ejecute el siguiente script.

Es importante tener en cuenta el uso del parámetro `-AllowEnclaveComputations` en el cmdlet [**New-SqlCertificateStoreColumnMasterKeySettings**](https://docs.microsoft.com/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings).

```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "\<Subject Name\>" -CertStoreLocation Cert:CurrentUser\\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database. Provide the server/db name. Modify the connection string, if needed.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Data Source = " + $serverName + "; Initial Catalog = " + $databaseName + "; Integrated Security = true"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key
# using the -AllowEnclaveComputations parameter.
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "<column master key name in the database>"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database.
$cekName = "<column encryption key name in the database>"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```


### <a name="provisioning-enclave-enabled-keys--azure-key-vault"></a>Aprovisionamiento de claves habilitadas para el enclave: Azure Key Vault

En el equipo de desarrollo/cliente, abra Windows PowerShell ISE y ejecute el siguiente script.

**Paso 1: Aprovisionamiento de una clave maestra de columna en Azure Key Vault**

Esto también puede realizarse con Azure Portal. Para obtener detalles, consulte [Manage your key vaults from Azure portal](https://blogs.technet.microsoft.com/kv/2016/09/12/manage-your-key-vaults-from-new-azure-portal/) (Administrar sus almacenes de claves desde Azure Portal).


```powershell
Import-Module AzureRM
Connect-AzureRmAccount

# User values
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"

# Set the context to the specified subscription.
$azureCtx = Set-AzureRMConteXt -SubscriptionId $SubscriptionId

# Create a new resource group - skip, if your desired group already exists.
New-AzureRmResourceGroup –Name $resourceGroup –Location $azureLocation

# Create a new key vault - skip if your vault already exists.
New-AzureRmKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation

# Grant yourself permissions needed to create and use the column master key.
Set-AzureRmKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, list, update, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account

# Create a column master key in Azure Key Vault.
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"
```

**Paso 2: Creación de metadatos de clave maestra de columna en la base de datos, creación de una clave de cifrado de columnas y creación de metadatos de clave de cifrado de columnas en la base de datos**


```powershell
# Import the SqlServer module.
Import-Module "SqlServer" -Version

# Connect to your database. Provide the server and db name. If needed, modify the connection string.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Data Source = " + $serverName + "; Initial Catalog = " + $databaseName + "; Integrated Security = true"
$database = Get-SqlDatabase -ConnectionString $connStr

# Authenticate to Azure before calling New-SqlAzureKeyVaultColumnMasterKeySettings,
# because -AllowEnclaveComputations causes a call to Azure Key Vault
# to generate the signature of the column master key.
Add-SqlAzureAuthenticationContext -Interactive

# Create a SqlColumnMasterKeySettings object for your column master key.
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "<column master key name in the database>"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database.
$cekName = "<column encryption key name in the database>"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```


## <a name="identify-enclave-enabled-keys-and-columns"></a>Identificar columnas y claves habilitadas para el enclave

Para incluir claves maestras de columna en una lista, configuradas en su base de datos, puede consultar la vista de catálogo [sys.column_master_keys](../../system-catalog-views/sys-column-master-keys-transact-sql.md) (por ejemplo, en SSMS). La nueva columna **allow_enclave_computations** se ha agregado a la vista. Indica si una clave maestra de columna está habilitada para el enclave.

```sql
SELECT name, allow_enclave_computations
FROM sys.column_master_keys
```

Para determinar qué claves de cifrado de columnas están cifradas con claves de cifrado de columna habilitadas para el enclave (y, por tanto, están habilitadas para el enclave), debe unirse a [sys.column_master_keys](../../system-catalog-views/sys-column-master-keys-transact-sql.md), [sys.column_encryption_key_values](../../system-catalog-views/sys-column-encryption-key-values-transact-sql.md) y [sys.column_encryption_keys](../../system-catalog-views/sys-column-encryption-keys-transact-sql.md).


```sql
SELECT cek.name AS [cek_name]
, cmk.name AS [cmk_name]
, cmk.allow_enclave_computations
FROM sys.column_master_keys cmk
JOIN sys.column_encryption_key_values cekv
   ON cmk.column_master_key_id = cekv.column_master_key_id
JOIN sys.column_encryption_keys cek
   ON cekv.column_encryption_key_id = cek.column_encryption_key_id
```

Para determinar qué columnas están habilitadas para el enclave (las columnas que están cifradas con claves de cifrado de columnas que están habilitadas para el enclave), use la siguiente consulta:

```sql
SELECT c.name AS column_name
, cek.name AS cek_name
, cmk.name AS [cmk_name]
, cmk.allow_enclave_computations
from sys.columns c
JOIN sys.column_encryption_keys cek 
ON c.column_encryption_key_id = cek.column_encryption_key_id 
JOIN sys.column_encryption_key_values cekv 
ON cekv.column_encryption_key_id = cek.column_encryption_key_id 
JOIN sys.column_master_keys cmk 
ON cmk.column_master_key_id = cekv.column_master_key_id
```


## <a name="manage-collations"></a>Administrar intercalaciones

Desde su versión inicial, Always Encrypted ha tenido una restricción en cuanto al uso de intercalaciones: las intercalaciones distintas de BIN2 no se permiten para las columnas de cadena de caracteres cifradas mediante cifrado determinista. Esta restricción también se aplica a las columnas de cadena habilitadas para el enclave.

El uso de intercalaciones distintas de BIN2 se permite para las columnas de cadena de caracteres cifradas con claves de cifrado de columnas habilitadas para el enclave y cifrado aleatorio. Sin embargo, la única funcionalidad nueva habilitada para dichas columnas es el cifrado en contexto. Para habilitar cálculos completos (operaciones de comparación y coincidencia de patrones), debe garantizar que la columna use una intercalación BIN2.

En la siguiente tabla se resume la funcionalidad para las columnas de cadena habilitadas para el enclave, dependiendo del tipo de cifrado y el criterio de ordenación de intercalación.

| **Criterio de ordenación de intercalación** | **Cifrado determinista** | **Cifrado aleatorio**                 |
| ------------------------ | ---------------------------- | ----------------------------------------- |
| **Distinta de BIN2**             | No compatible                | Cifrado en contexto                       |
| **BIN2**                 | Comparación de igualdad          | Cifrado en contexto y cálculos completos |

### <a name="determining-and-changing-collations"></a>Determinación y cambio de intercalaciones

En SQL Server, las intercalaciones se pueden establecer en el nivel de columna, base de datos o servidor. Para obtener instrucciones generales sobre cómo determinar la intercalación actual y cambiar una intercalación en el nivel de columna, base de datos o servidor, consulte [Compatibilidad con la intercalación y Unicode](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support).

**Consideraciones especiales para las columnas de cadena distintas de UNICODE**:

La siguiente restricción adicional, impuesta por una limitación en los controladores cliente SQL (no relacionados con Always Encrypted), se aplica a las columnas de cadena distintas de UNICODE (ASCII). Si sobrescribe la intercalación de base de datos para una columna de cadena distinta de UNICODE (char, varchar), debe asegurarse de que la intercalación de columna usa la misma página de códigos que la intercalación de base de datos.
Para incluir todas las intercalaciones en una lista junto con sus identificadores de página de códigos, use la siguiente consulta:

```sql
SELECT [Name]
   , [Description]
   , [CodePage] = COLLATIONPROPERTY([Name], 'CodePage')
FROM ::fn_helpcollations()
```

Por ejemplo, Chinese_Traditional_Stroke_Order_100_CI_AI_WS y Chinese_Traditional_Stroke_Order_100_BIN2 tienen la misma página de código (950), pero Chinese_Traditional_Stroke_Order_100_CI_AI_WS y Latin1_General_100_BIN2 tienen páginas de código distintas (950 y 1252, respectivamente). La restricción anterior no se aplica a las columnas de cadena UNICODE (nchar, nvarchar). Por tanto, como solución alternativa, puede considerar la posibilidad de establecer un tipo de datos UNICODE para sus nuevas columnas cifradas. Usted crea o cambia el tipo a un tipo UNICODE antes de cifrar una columna existente.


## <a name="create-a-new-table-with-enclave-enabled-columns"></a>Crear una nueva tabla con columnas habilitadas para el enclave

Puede crear una nueva tabla con columnas cifradas mediante la instrucción [CREATE TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql). Always Encrypted con enclaves seguros no cambia la sintaxis de esta instrucción.

1. Mediante SSMS, conéctese a su base de datos y abra una ventana de consulta.
   
     > [!NOTE]
     > Always Encrypted no tiene que habilitarse en la cadena de conexión para esta tarea.

2. En la ventana de consulta, emita una instrucción CREATE TABLE para crear su nueva tabla, especificando la cláusula ENCRYPTED WITH en la [definición de columna](https://docs.microsoft.com/sql/t-sql/statements/alter-table-column-definition-transact-sql) para cada una de las columnas que se van a cifrar. Para hacer que una columna esté habilitada para el enclave, asegúrese de especificar una clave de cifrado de columnas habilitada para el enclave. También es posible que tenga que especificar una intercalación BIN2 para las columnas de cadena si la intercalación predeterminada para su base de datos no es una intercalación BIN2. Consulte la sección Collation Setup (Configuración de intercalación) para obtener detalles.

### <a name="example"></a>Ejemplo

La siguiente instrucción crea una nueva tabla con dos columnas cifradas, SSN y Salario. Suponiendo que CEK1 es una clave de cifrado de columnas habilitada para el enclave, el Motor de SQL Server admite el cifrado en contexto y los cálculos completos para ambas columnas, y las columnas usan cifrado aleatorio. La instrucción establece la intercalación Latin1\_General\_BIN2 para la columna SSN UNICODE, que se requiere suponiendo que la intercalación de base de datos predeterminada es una intercalación distinta de BIN2 Latin1.

```sql
CREATE TABLE [dbo].[Employees]
(
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [SSN] [char](11) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (
        COLUMN_ENCRYPTION_KEY = [CEK1],
        ENCRYPTION_TYPE = Randomized,
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [Salary] [int] ENCRYPTED WITH (
        COLUMN_ENCRYPTION_KEY = [CEK1],
        ENCRYPTION_TYPE = Randomized,
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
    CONSTRAINT [PK_dbo.Employees] PRIMARY KEY CLUSTERED (
[EmployeeID] ASC
)
) ON [PRIMARY]
GO
```


## <a name="add-a-new-enclave-enabled-column-to-an-existing-table"></a>Agregar una nueva columna habilitada para el enclave a una tabla existente

Puede agregar una nueva columna cifrada a una tabla existente mediante la instrucción [ALTER TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) / ADD. Always Encrypted con enclaves seguros no cambia la sintaxis de esta instrucción.

1. Mediante SSMS, conéctese a su base de datos y abra una ventana de consulta.
    
   Always Encrypted no tiene que habilitarse en la cadena de conexión para esta tarea.

2. En la ventana de consulta, emita la instrucción ALTER TABLE con la cláusula ADD, especificando la cláusula ENCRYPTED WITH en la [definición de columna](https://docs.microsoft.com/sql/t-sql/statements/alter-table-column-definition-transact-sql) y usando una clave de cifrado de columna habilitada para el enclave. También es posible que tenga que especificar una intercalación BIN2 si su nueva columna es una columna de cadena y si la intercalación predeterminada para su base de datos no es una intercalación BIN2. Consulte la sección Collation Setup (Configuración de intercalación) para obtener detalles.

### <a name="example"></a>Ejemplo

Suponiendo que CEK-1 es una clave de cifrado de columnas habilitada para el enclave, la siguiente instrucción agrega una nueva columna cifrada, llamada BirthDate, que admite tanto consultas completas como el cifrado en contexto (ya que la columna usa el cifrado aleatorio).

```sql
ALTER TABLE [dbo].[Employees]
ADD [BirthDate] [Date] ENCRYPTED WITH (
COLUMN_ENCRYPTION_KEY = [CEK1],
ENCRYPTION_TYPE = Randomized,
ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL
```


## <a name="prepare-an-ssms-query-window-with-always-encrypted-enabled"></a>Preparar una ventana de consulta de SSMS con Always Encrypted habilitado

Para agregar los parámetros de conexión necesarios a fin de habilitar cálculos de enclave:

1. Abra SSMS.
2. Después de especificar el nombre del servidor y un nombre de base de datos en el cuadro de diálogo Connect To Server (Conectarse al servidor), haga clic en **Opciones**. Vaya a la pestaña **Always Encrypted**, seleccione **Habilitar Always Encrypted** y especifique su dirección URL de atestación de enclave.
    
    ![Configuración de cifrado de columnas](./media/always-encrypted-enclaves/column-encryption-setting.png)

3. Haga clic en Conectar.
4. Abra una nueva ventana de consulta.

Como alternativa, si ya tiene abierta una ventana de consulta, puede actualizar su conexión de base de datos de la siguiente manera para habilitar Always Encrypted:

1. Haga clic con el botón derecho en cualquier parte en una ventana de consulta existente.
2. Seleccione Conexión \> Cambiar conexión.
3. Haga clic en **Opciones**. Vaya a la pestaña **Always Encrypted**, seleccione **Habilitar Always Encrypted** y especifique su dirección URL de atestación de enclave.
4. Haga clic en Conectar.


## <a name="work-with-encrypted-columns"></a>Trabajar con columnas cifradas

### <a name="encrypt-an-existing-plaintext-column-in-place"></a>Cifrar una columna de texto sin formato existente en contexto

Puede cifrar una columna de texto sin formato existente en contexto mediante la instrucción [ALTER TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) / ALTER COLUMN, siempre que use una clave de cifrado de columnas habilitada para el enclave.

Para cifrar una columna que usa una clave que no está habilitada para el enclave, debe usar herramientas del lado cliente, como el asistente de Always Encrypted en SSMS o el cmdlet Set-SqlColumnEncryption en el módulo de PowerShell SqlServer. Para obtener detalles, consulte:

- [Asistente para Always Encrypted](always-encrypted-wizard.md)
- [Configurar el cifrado de columna con PowerShell](configure-column-encryption-using-powershell.md)


### <a name="prerequisites"></a>Prerequisites

- La columna existente no está cifrada.
- Ha aprovisionado claves habilitadas para el enclave.
- Tiene acceso a la clave maestra de columna.

#### <a name="steps"></a>Pasos

1. Prepare una ventana de consulta de SSMS con Always Encrypted y cálculos de enclave habilitados en la conexión de base de datos. Para obtener detalles, consulte [Preparar una ventana de consulta de SSMS con Always Encrypted habilitado](#prepare-an-ssms-query-window-with-always-encrypted-enabled).
2. En la ventana de consulta, emita la instrucción ALTER TABLE con la cláusula ALTER COLUMN, especificando una clave de cifrado de columnas habilitada para el enclave en la cláusula ENCRYPTED WITH. Si su columna es una columna de cadena (por ejemplo, char, varchar, nchar, nvarchar), es posible que también tenga que cambiar la intercalación a una intercalación BIN2. Consulte la sección Collation Setup (Configuración de intercalación) para obtener detalles.
    
    > [!NOTE]
    > Si su clave maestra de columna se almacena en Azure Key Vault, es posible que se le pida que inicie sesión en Azure.

3. Borre (de forma opcional) la caché de planes mediante [DBCC FREEPROCCACHE](../../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md) para garantizar que los planes de cualquier consulta sobre las columnas que ha cifrado vuelvan a crearse en la primera ejecución de consulta.
  
    > [!NOTE]
    > Si no quita el plan para la consulta afectada de la memoria caché, la primera ejecución de la consulta tras el cifrado puede producir un error.

    > [!NOTE]
    > Use DBCC FREEPROCCACHE para borrar la caché de planes con cuidado, ya que puede dar lugar a una degradación del rendimiento de consultas temporal. Para minimizar el impacto negativo de haber borrado la memoria caché, únicamente puede quitar de forma selectiva los planes para las consultas afectadas.

4.  Llame (de forma opcional) a [sp_refresh_parameter_encryption](../../system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) para actualizar los metadatos para los parámetros de cada módulo (procedimiento almacenado, función, vista, disparador) que puedan haberse invalidado cifrando las columnas.

#### <a name="example"></a>Ejemplo

En el ejemplo siguiente se da por sentado que:

  - CEK1 es una clave de cifrado de columnas habilitada para el enclave.

  - La columna SSN es texto sin formato y actualmente usa una intercalación Latin1 y distinta de BIN2 (por ejemplo, Latin1\_General\_CI\_AI\_KS\_WS).

La instrucción cifra la columna SSN mediante el cifrado aleatorio y la clave de cifrado de columnas habilitada para el enclave. También sobrescribe la intercalación de base de datos predeterminada con la intercalación BIN2 correspondiente (en la misma página de código).

La operación se realiza en línea (ONLINE = ON). Tenga también en cuenta la llamada a **DBCC FREEPROCCACHE** que vuelve a crear los planes de las consultas afectadas por el cambio en el esquema de la tabla.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char] COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
WITH
(ONLINE = ON)
GO
DBCC FREEPROCCACHE
GO
```


### <a name="make-an-existing-encrypted-column-enclave-enabled"></a>Hacer que una columna cifrada existente esté habilitada para el enclave

Existen varias formas de habilitar la funcionalidad de enclave para una columna existente no habilitada para el enclave. El método que elija dependerá de varios factores:

- **Ámbito/granularidad:** ¿desea habilitar la funcionalidad de enclave para un subconjunto de columnas, o bien para todas las columnas protegidas con una clave maestra de columna especificada?
- **Tamaño de los datos:** ¿cuál es el tamaño de las tablas que incluyen las columnas que desea hacer que estén habilitadas para el enclave?
- ¿Desea también cambiar el tipo de cifrado para sus columnas? Recuerde que solo el cifrado aleatorio admite los cálculos completos (búsqueda de patrones, operadores de comparación). Si su columna se cifra mediante el cifrado determinista, también tendrá que volver a cifrarla con el cifrado aleatorio para desbloquear la funcionalidad completa del enclave.

Estos son los tres enfoques para habilitar los enclaves para las columnas existentes:

#### <a name="option-1-rotate-the-column-master-key-to-replace-it-with-an-enclave-enabled-column-master-key"></a>Opción 1: rote la clave maestra de columna para reemplazarla por una clave maestra de columna habilitada para el enclave.
  
- Ventajas:
  - No implica que haya que volver a cifrar los datos, por lo que suele ser el enfoque más rápido. Es un enfoque recomendado para las columnas que incluyen grandes cantidades de datos, siempre que todas las columnas para las que tenga que habilitar los cálculos completos usen ya el cifrado determinista y, por tanto, no se tengan que volver a cifrar.
  - Puede habilitar la funcionalidad de enclave para varias columnas a escala, ya que hace que todas las claves de cifrado de columnas y todas las columnas cifradas, asociadas a la clave maestra de columna original, estén habilitadas para el enclave.
  
- Inconvenientes:
  - No admite el cambio del tipo de cifrado de determinista a aleatorio, de modo que, pese a que desbloquea el cifrado en contexto para las columnas cifradas de manera determinista, no habilita los cálculos completos.
  - No le permite convertir de forma selectiva ninguna de las columnas, asociadas a una clave maestra de columna especificada.
  - Presenta una sobrecarga de administración de claves (debe crear una nueva clave maestra de columna y ponerla a disposición de las aplicaciones que consultan las columnas afectadas).  


#### <a name="option-2-this-approach-involves-two-steps-1-rotating-the-column-master-key-as-in-option-1-and-2-re-encrypting-a-subset-of-deterministically-encrypted-columns-using-randomized-encryption-to-enable-rich-computations-for-those-columns"></a>Opción 2: este enfoque implica dos pasos: 1) rotar la clave maestra de columna (como en la opción 1) y 2) volver a cifrar un subconjunto de columnas cifradas de manera determinista mediante el cifrado aleatorio, para habilitar los cálculos completos para esas columnas.
  
- Ventajas:
  - Vuelve a cifrar los datos en contexto y, por tanto, es un método recomendado para habilitar las consultas completas para las columnas cifradas de manera determinista que incluyen grandes cantidades de datos. Tenga en cuenta que en el paso 1 se desbloquea el cifrado en contexto para las columnas que usan el cifrado determinista, por lo que el paso 2 puede realizarse en contexto.
  - Puede habilitar la funcionalidad de enclave para varias columnas a escala.
  
- Inconvenientes:
  - No le permite convertir de forma selectiva ninguna de las columnas, asociadas a una clave maestra de columna especificada.
  - Presenta una sobrecarga de administración de claves (debe crear una nueva clave maestra de columna y ponerla a disposición de las aplicaciones que consultan las columnas afectadas).

#### <a name="option-3-re-encrypting-selected-columns-with-a-new-enclave-enabled-column-encryption-key-and-randomized-encryption-if-needed-on-the-client-side"></a>Opción 3: volver a cifrar las columnas seleccionadas con una nueva clave de cifrado de columnas habilitada para el enclave y el cifrado aleatorio (en caso necesario) en el lado cliente.
  
- Ventajas: este método:
  - Le permite habilitar de forma selectiva la funcionalidad de enclave para una columna o un pequeño subconjunto de columnas.
  - Puede habilitar los cálculos completos para una columna cifrada de manera determinista en un paso.
  - No requiere la creación de una nueva clave maestra de columna, por lo que tiene un impacto más pequeño en las aplicaciones.
  
- Inconvenientes:
  - Todo el contenido de la tabla que incluye la columna debe moverse fuera de la base de datos para el nuevo cifrado, por lo que solo se recomienda para las tablas pequeñas. 

Para obtener más información, vea las secciones siguientes:
  - [Hacer que las columnas estén habilitadas para el enclave girando su clave maestra de columna](#make-columns-enclave-enabled-by-rotating-their-column-master-key)
  - [Volver a cifrar las columnas en contexto](#re-encrypt-columns-in-place)
  - [Volver a cifrar las columnas en el lado cliente](#re-encrypt-columns-on-the-client-side)

### <a name="make-columns-enclave-enabled-by-rotating-their-column-master-key"></a>Hacer que las columnas estén habilitadas para el enclave girando su clave maestra de columna

La rotación de una clave maestra de columna es un proceso por el cual se reemplaza una clave maestra de columna existente por otra nueva. Implica volver a cifrar las claves de cifrado de columna, asociadas a la antigua clave maestra de columna con la nueva clave maestra de columna. Este flujo de trabajo ha existido desde la versión inicial de Always Encrypted para admitir la sustitución de una clave maestra de columna por motivos de cumplimiento o seguridad (en caso de que la clave maestra de columna existente se ponga en peligro).

Always Encrypted con enclaves agrega un nuevo objetivo para el flujo de trabajo de rotación de una clave maestra de columna. Suponiendo que la antigua clave maestra de columna no está habilitada para el enclave y que la nueva clave maestra de columna está habilitada para el enclave, el proceso de rotación hace, de forma eficaz, que todas las claves de cifrado de columna asociadas a la clave maestra de columna estén habilitadas para el enclave. La rotación de una clave maestra de columna no implica el nuevo cifrado de los datos y, por tanto, es un proceso recomendado para habilitar funcionalidades de enclave para las columnas existentes.

La rotación de la clave maestra de columna no cambia el tipo de cifrado de las columnas afectadas. Por tanto, solo puede desbloquear el cifrado en contexto para las columnas cifradas de manera determinista. Para desbloquear los cálculos completos para las columnas que usan el cifrado determinista, debe volver a cifrarlas (en contexto) tras rotar la clave maestra de columna.

También es posible que tenga que cambiar la intercalación para las columnas de cadena, mediante el cifrado aleatorio, a una intercalación BIN2 a fin de desbloquear los cálculos completos. Consulte la sección Collation Setup (Configuración de intercalación) para obtener detalles.

El proceso de rotación de una clave maestra de columna es el mismo, independientemente de si la clave afectada está habilitada para el enclave. Los detalles sobre cómo rotar la clave maestra de columna se incluyen en los siguientes artículos:

- [Rotar una clave maestra de columna con SSMS](configure-always-encrypted-using-sql-server-management-studio.md)
- [Rotación de una clave maestra de columna con PowerShell](rotate-always-encrypted-keys-using-powershell.md)

Para su comodidad, a continuación se proporciona un script de PowerShell para rotar una clave maestra de columna.

#### <a name="pre-requisites"></a>Requisitos previos

- Ha aprovisionado una nueva clave maestra de columna habilitada para el enclave.
- Tiene acceso tanto a la antigua clave maestra de columna como a la nueva.
- Todas las columnas de cadena, protegidas con la antigua clave maestra de columna, usan intercalaciones BIN2 (nota: de forma alternativa, puede cambiar la intercalación de columnas de cadena tras rotar la clave maestra de columna).

#### <a name="steps"></a>Pasos

Pegue el siguiente script en Windows PowerShell ISE y reemplace los \<marcadores de posición\> por sus valores específicos:


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database. Modify server/db name or/and the connection string, if needed.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Data Source = " + $serverName + "; Initial Catalog = " +
$databaseName + "; Integrated Security = true"
$database = Get-SqlDatabase -ConnectionString $connStr

# Set the names of the old/current column master key and the new/target enclave-enabled column master key. (Change the names, if needed).
$oldCmkName = "<old column master key name>"
$newCmkName = "<new column master key name>"

# Authenticate to Azure. Needed only of either column master key is stored in Azure Key Vault.
Add-SqlAzureAuthenticationContext -Interactive

# Initiate the rotation from the current column master key to the new column master key.
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName
-TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName
$oldCmkName -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```


### <a name="re-encrypt-columns-in-place"></a>Volver a cifrar las columnas en contexto 

Una vez que haya hecho que su columna esté habilitada para el enclave, podrá realizar las siguientes operaciones en contexto (dentro del enclave, sin tener que sacar los datos de la base de datos):

- Rotar la clave de cifrado de columnas (para reemplazarla por una nueva clave), por ejemplo, para cumplir las normas de conformidad, algunas de las cuales exigen la rotación periódica de la clave, o por motivos de seguridad (en caso de que su clave de cifrado de columnas se ponga en peligro).
- Cambiar el tipo de cifrado, por ejemplo, del cifrado determinista al cifrado aleatorio, para desbloquear los cálculos completos para la columna.

#### <a name="prerequisites"></a>Prerequisites

- Su columna se cifra mediante una clave de cifrado de columnas habilitada para el enclave.
- Ha aprovisionado una clave de cifrado de columnas habilitada para el enclave (si su objetivo es reemplazar la clave de cifrado de columnas habilitada para el enclave, protegiendo la columna).
- Tiene acceso a la clave maestra de columna.

#### <a name="steps"></a>Pasos

1. Prepare una ventana de consulta de SSMS con Always Encrypted y cálculos de enclave habilitados para la conexión de base de datos. Para obtener detalles, consulte [Preparar una ventana de consulta de SSMS con Always Encrypted habilitado](#prepare-an-ssms-query-window-with-always-encrypted-enabled).

2. En la ventana de consulta, emita el uso de la instrucción [ALTER TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) con la cláusula ALTER COLUMN, especificando lo siguiente en la cláusula ENCRYPTED WITH:
    
    1. El nombre de la nueva clave de cifrado de columnas habilitada para el enclave si rota la clave actual. Si no cambia la clave de cifrado de columnas, debe especificar el nombre de la clave actual.
    
    2. El nuevo tipo de cifrado si lo cambia. Si no cambia el tipo de cifrado, debe especificar el tipo de cifrado actual.
        
       Si la columna que vuelve a cifrar usa una intercalación (BIN2) que es diferente a la intercalación de base de datos predeterminada, debe incluir la frase COLLATE y especificar la intercalación de columna actual en la definición de columna (para que la intercalación no cambie).
        
       > [!NOTE]
       > Si su clave maestra de columna se almacena en Azure Key Vault, es posible que se le pida que inicie sesión en Azure.

3. Borre (de forma opcional) la caché de planes mediante [DBCC FREEPROCCACHE](../../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md) para garantizar que los planes de cualquier consulta sobre las columnas que ha vuelto a cifrar vuelvan a crearse en la primera ejecución de consulta.
    
    Si no quita el plan para la consulta afectada de la memoria caché, la primera ejecución de la consulta tras el nuevo cifrado puede producir un error.
    
    > [!NOTE]
    > Use DBCC FREEPROCCACHE para borrar la caché de planes con cuidado, ya que puede dar lugar a una degradación del rendimiento de consultas temporal. Para minimizar el impacto negativo de haber borrado la memoria caché, únicamente puede quitar de forma selectiva los planes para las consultas afectadas. Consulte [DBCC FREEPROCCACHE](../../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md).aspx) para obtener detalles.

4. Llame (de forma opcional) a [sp_refresh_parameter_encryption](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql) para actualizar los metadatos para los parámetros de cada módulo (procedimiento almacenado, función, vista, disparador) que puedan haberse invalidado volviendo a cifrar las columnas.

#### <a name="examples"></a>Ejemplos

Suponiendo que actualmente se cifra la columna SSN mediante una clave de cifrado de columnas habilitada para el enclave, llamada CEK1, y el cifrado determinista, y que la intercalación actual, establecida en el nivel de columna, es Latin1\_General\_BIN2, la siguiente instrucción vuelve a cifrar la columna con el cifrado aleatorio y la misma clave.


```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
GO
DBCC FREEPROCCACHE
GO
```


Suponiendo que actualmente se cifra la columna SSN mediante una clave de cifrado de columnas habilitada para el enclave, llamada CEK1, y el cifrado determinista, y que la intercalación de base de datos predeterminada es una intercalación BIN2 (y no se ha establecido en el nivel de columna), la siguiente instrucción vuelve a cifrar la columna con una nueva clave habilitada para el enclave, llamada CEK2 (sin cambiar el tipo de cifrado).

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) 
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK2]
, ENCRYPTION\_TYPE = Deterministic
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
GO
DBCC FREEPROCCACHE
GO
```

Suponiendo que actualmente se cifra la columna SSN mediante una clave de cifrado de columnas habilitada para el enclave llamada CEK1, y el cifrado determinista, y que la intercalación de base de datos predeterminada es una intercalación BIN2 (y no se ha establecido en el nivel de columna), la siguiente instrucción vuelve a cifrar la columna con una nueva clave habilitada para el enclave y el cifrado aleatorio. Además, la operación se realiza en el modo en línea.


```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) 
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL 
WITH (ONLINE = ON)
GO
DBCC FREEPROCCACHE
GO
```


### <a name="re-encrypt-columns-on-the-client-side"></a>Volver a cifrar las columnas en el lado cliente 

El método heredado para volver a cifrar (y cifrar o descifrar) las columnas emplea herramientas del lado cliente, como el asistente de Always Encrypted o PowerShell. En general, no se recomienda usar este método, salvo si la tabla que contiene las columnas (que se vuelven a cifrar) es pequeña y si su objetivo es combinar el nuevo cifrado de una columna con una nueva clave habilitada para el enclave y el cambio del tipo de cifrado (de determinista a aleatorio).

Tenga en cuenta que si al volver a cifrar una columna se usa el cifrado aleatorio, es posible que deba cambiar su intercalación a una intercalación BIN2 (antes o después del nuevo cifrado), para desbloquear los cálculos completos. Consulte la sección Collation Setup (Configuración de intercalación) para obtener detalles.

Para obtener detalles, consulte:

  - Rotación de claves de cifrado de columnas mediante SSMS: <https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard>
  - Rotación de claves de cifrado de columnas mediante PowerShell: <https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-column-encryption-using-powershell>

### <a name="decrypt-a-column-in-place"></a>Descifrar una columna en contexto

Si su columna se cifra con una clave de cifrado de columnas habilitada para el enclave, puede descifrarla (convertirla a una columna de texto sin formato) en contexto, mediante la instrucción ALTER TABLE. Además, la operación se realiza en el modo en línea.

#### <a name="prerequisites"></a>Prerequisites

- Su columna se cifra mediante una clave de cifrado de columnas habilitada para el enclave.
- Tiene acceso a la clave maestra de columna.



#### <a name="steps"></a>Pasos

1.  Prepare una ventana de consulta de SSMS con Always Encrypted y cálculos de enclave habilitados para la conexión de base de datos. Para obtener detalles, consulte [Preparar una ventana de consulta de SSMS con Always Encrypted habilitado](#prepare-an-ssms-query-window-with-always-encrypted-enabled).

2.  En la ventana de consulta, emita la instrucción [ALTER TABLE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) con la cláusula ALTER COLUMN, especificando la configuración de columna deseada **sin** la cláusula ENCRYPTED WITH.
    
    > [!NOTE]
    > Si su clave maestra de columna se almacena en Azure Key Vault, es posible que se le pida que inicie sesión en Azure.

3.  Borre (de forma opcional) la caché de planes mediante [DBCC FREEPROCCACHE](../../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md) para garantizar que los planes de cualquier consulta sobre las columnas que ha descifrado vuelvan a crearse en la primera ejecución de consulta.
    
    > [!NOTE]
    > Si no quita el plan para la consulta afectada de la memoria caché, la primera ejecución de la consulta tras el descifrado puede producir un error.
    
    > [!NOTE]
    > Use DBCC FREEPROCCACHE para borrar la caché de planes con cuidado, ya que puede dar lugar a una degradación del rendimiento de consultas temporal. Para minimizar el impacto negativo de haber borrado la memoria caché, únicamente puede quitar de forma selectiva los planes para las consultas afectadas. Consulte [DBCC FREEPROCCACHE](../../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md) para obtener detalles.

4.  Llame (de forma opcional) a [sp\_refresh\_parameter\_encryption](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql) para actualizar los metadatos para los parámetros de cada módulo (procedimiento almacenado, función, vista, disparador) que puedan haberse invalidado descifrando las columnas.

#### <a name="example"></a>Ejemplo

Suponiendo que la columna SSN se cifra y que la intercalación actual, establecida en el nivel de columna, es Latin1\_General\_BIN2, la siguiente instrucción descifra la columna (y hace que la intercalación no cambie - de forma alternativa, puede elegir cambiar la intercalación, por ejemplo, a una intercalación distinta de BIN2 en la misma instrucción).


```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
WITH (ONLINE = ON)
GO
DBCC FREEPROCCACHE
GO
```


## <a name="issue-rich-queries-against-encrypted-columns-using-ssms"></a>Emitir consultas completas sobre columnas cifradas mediante SSMS

La forma más rápida de probar consultas completas sobre sus columnas habilitadas para el enclave es desde una ventana de consulta de SSMS con Parametrización para Always Encrypted habilitada. Para obtener detalles sobre esta útil funcionalidad de SSMS, consulte:

- [Parametrización para Always Encrypted: uso de SSMS para insertar columnas cifradas, actualizarlas y filtrar por ellas](https://blogs.msdn.microsoft.com/sqlsecurity/2016/12/13/parameterization-for-always-encrypted-using-ssms-to-insert-into-update-and-filter-by-encrypted-columns/)
- [Consulta de columnas cifradas](configure-always-encrypted-using-sql-server-management-studio.md#querying-encrypted-columns)



### <a name="prerequisites"></a>Prerequisites

- Las columnas que se van a consultar están habilitadas para el enclave.
- Tiene acceso a la clave maestra de columna (o claves).

### <a name="steps"></a>Pasos

1.  Prepare una ventana de consulta de SSMS con Always Encrypted y cálculos de enclave habilitados para la conexión de base de datos. Para obtener detalles, consulte [Preparar una ventana de consulta de SSMS con Always Encrypted habilitado](#prepare-an-ssms-query-window-with-always-encrypted-enabled).

2.  Habilite la parametrización para Always Encrypted.
    
    1.  Seleccione **Consulta** en el menú principal de SSMS.
    2.  Seleccione **Opciones de consulta…**.
    3.  Vaya a **Ejecución** > **Avanzadas**.
    4.  Seleccione o anule la selección de Habilitar parametrización para Always Encrypted.
    5.  Haga clic en Aceptar.

3.  Cree y ejecute sus consultas mediante los cálculos completos en las columnas cifradas. Debe declarar una variable Transact-SQL para cada valor cuyo destino sea una columna cifrada en su consulta. Las variables deben usar inicializaciones alineadas (no se pueden establecer a través de la instrucción SET).
    
    > [!NOTE]
    > Si su clave maestra de columna se almacena en Azure Key Vault, es posible que se le pida que inicie sesión en Azure.

### <a name="example"></a>Ejemplo

En este ejemplo se da por supuesto que su base de datos contiene una tabla creada mediante la siguiente instrucción.

```sql
CREATE TABLE [dbo].[Employees]
(
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [SSN] [char](11) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (
        COLUMN_ENCRYPTION_KEY = [CEK1],
        ENCRYPTION_TYPE = Randomized,
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [Salary] [int] ENCRYPTED WITH (
        COLUMN_ENCRYPTION_KEY = [CEK1],
        ENCRYPTION_TYPE = Randomized,
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
    CONSTRAINT [PK_dbo.Employees] PRIMARY KEY CLUSTERED (
[EmployeeID] ASC
)
) ON [PRIMARY]
GO
```


CEK1 es una clave de cifrado de columnas habilitada para el enclave.

Este es un ejemplo de una consulta que cumple las directrices para la parametrización, sobre esta tabla:


```sql
DECLARE @SSNPattern CHAR(11) = '%1111%'
DECLARE @MinSalary INT = 1000
SELECT *
FROM [dbo].[Employees]
WHERE SSN LIKE @SSNPattern
    AND [Salary] >= @MinSalary;
GO;
```


## <a name="develop-applications-issuing-rich-queries-in-visual-studio"></a>Desarrollar aplicaciones que emiten consultas completas en Visual Studio

### <a name="set-up-your-you-visual-studio-project"></a>Configurar su proyecto de Visual Studio

Para usar Always Encrypted con enclaves seguros en una aplicación de .NET Framework, debe asegurarse de que su aplicación se compila en .NET Framework 4.7.2 y se integra con el NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders. Además, si almacena su clave maestra de columna en Azure Key Vault, también debe integrar su aplicación con el NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider, versión 2.2.0 o posterior. 

1. Abra Visual Studio.
2. Cree un nuevo proyecto de Visual C\# o abra un proyecto existente.
3. Asegúrese de que su proyecto tenga como destino al menos .NET Framework 4.7.2. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones, seleccione Propiedades y establezca el marco de destino en .NET Framework 4.7.2.

4. Instale el siguiente paquete NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

  ```powershell
  Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider --IncludePrerelease 
  ```

5. Si usa Azure Key Vault para almacenar sus claves maestras de columna, instale los siguientes paquetes NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

  ```powershell
  Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider --IncludePrerelease -Version 2.2.0
  Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
  ```

6. Seleccione su proyecto y haga clic en Instalar.
7. Abra el archivo de configuración desde su proyecto (por ejemplo, App.config o Web.config).
8. Busque la sección de \<configuración\>. En la sección de \<configuración\>, busque la sección \<configSections\>. Agregue la siguiente sección dentro de \<configSections\>:

  ```
  <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /\>
  ```

9. Dentro de la sección de configuración, después de \<configSections\>, agregue la siguiente sección, que especifica un proveedor de enclave que se va a usar para dar fe de los enclaves Intel SGX e interactuar con ellos:

  ```
  \<SqlColumnEncryptionEnclaveProviders\>
      \<providers\>
      \<add name="VBS" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.VirtualizationBasedSecurityEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,   Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/\>
      \</SqlColumnEncryptionEnclaveProviders\>
  ```
 

### <a name="develop-and-test-your-app"></a>Desarrollar y probar su aplicación 

Para usar Always Encrypted y los cálculos de enclave, su aplicación debe conectarse a la base de datos con las siguientes dos palabras clave en la cadena de conexión: `Column Encryption Setting = Enabled; Enclave Attestation Url=http://x.x.x.x/Attestation` (donde xxxx puede ser un protocolo de Internet, un dominio, etc).

Además, su aplicación debe cumplir las directrices habituales que se aplican a las aplicaciones mediante Always Encrypted, por ejemplo, su aplicación debe tener acceso a las claves maestras de columna asociadas a las columnas de base de datos, a las que se hace referencia en las consultas de la aplicación.

Para obtener detalles sobre el desarrollo de aplicaciones .NET Framework mediante Always Encrypted, consulte los siguientes artículos:

- [Desarrollar con Always Encrypted con el proveedor de datos .NET Framework](develop-using-always-encrypted-with-net-framework-data-provider.md)
- [Always Encrypted: protección de información confidencial en SQL Database y almacenamiento de las claves de cifrado en Azure Key Vault](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted)

#### <a name="example"></a>Ejemplo

El siguiente código es solo un ejemplo de una aplicación de la consola de C\# que emite una consulta LIKE sobre la tabla con el siguiente esquema:

```sql
CREATE TABLE [dbo].[Employees]
(
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [SSN] [char](11) ENCRYPTED WITH (
        COLUMN_ENCRYPTION_KEY = [CEK1],
        ENCRYPTION_TYPE = Randomized,
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [Salary] [int] ENCRYPTED WITH (
        COLUMN_ENCRYPTION_KEY = [CEK1],
        ENCRYPTION_TYPE = Randomized,
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
    CONSTRAINT [PK_dbo.Employees] PRIMARY KEY CLUSTERED (
[EmployeeID] ASC
)
) ON [PRIMARY]
GO
```

Se da por sentado que CEK1 es una clave de cifrado de columnas habilitada para el enclave.


```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.SqlClient;
using System.Data;
namespace ConsoleApp1
{
   class Program
   {
      static void Main(string\[\] args)
   {

   string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = http://10.193.16.185/Attestation/attestationservice.svc/signingCertificates; Integrated Security = true";

using (SqlConnection connection = new SqlConnection(connectionString))
{
   connection.Open();
   
   SqlCommand cmd = connection.CreateCommand();
   cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";
   
   SqlParameter paramSSNPattern = cmd.CreateParameter();
   
   paramSSNPattern.ParameterName = @"@SSNPattern";
   paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
   paramSSNPattern.Direction = ParameterDirection.Input;
   paramSSNPattern.Value = "%1111";
   paramSSNPattern.Size = 11;
   
   cmd.Parameters.Add(paramSSNPattern);
   
   SqlParameter MinSalary = cmd.CreateParameter();
   
   MinSalary.ParameterName = @"@MinSalary";
   MinSalary.DbType = DbType.Int32;
   MinSalary.Direction = ParameterDirection.Input;
   MinSalary.Value = 900;
   
   cmd.Parameters.Add(MinSalary);
   cmd.ExecuteNonQuery();
   
   SqlDataReader reader = cmd.ExecuteReader();
   while (reader.Read())
   
   {
     Console.WriteLine(reader);
     Console.WriteLine(reader\[0\] + ", " + reader\[1\] + ", " + reader\[2\] + ", " + reader\[3\]);
   }

   Console.ReadKey();

   }
  }
 }
}
```
