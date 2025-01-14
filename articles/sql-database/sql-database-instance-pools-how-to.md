---
title: Guía paso a paso sobre los grupos de instancias de Azure SQL Database (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo crear y administrar grupos de instancias de Azure SQL Database (versión preliminar).
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: 5862a54c92de7395ce42865ae32d453e926048d8
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293786"
---
# <a name="azure-sql-database-instance-pools-preview-how-to-guide"></a>Guía paso a paso sobre los grupos de instancias de Azure SQL Database (versión preliminar)

En este artículo se proporcionan detalles sobre cómo crear y administrar [grupos de instancias](sql-database-instance-pools.md).

## <a name="instance-pool-operations"></a>Operaciones de grupos de instancias

En la tabla siguiente se muestran las operaciones disponibles relacionadas con los grupos de instancias y su disponibilidad en Azure Portal y PowerShell.

|Get-Help|Portal de Azure|PowerShell|
|:---|:---|:---|
|Crear un grupo de instancias|Sin|Sí|
|Actualizar un grupo de instancias (número limitado de propiedades)|Sin |Sí |
|Comprobar el uso de un grupo de instancias y sus propiedades|Sin|Sí |
|Eliminar un grupo de instancias|Sin|Sí|
|Crear una instancia administrada dentro del grupo de instancias|Sin|Sí|
|Actualizar el uso de un recurso de instancia administrada|Sí |Sí|
|Comprobar el uso de una instancia administrada y sus propiedades|Sí|Sí|
|Eliminar una instancia administrada del grupo|Sí|Sí|
|Crear una base de datos en una instancia administrada colocada en el grupo|Sí|Sí|
|Eliminar una base de datos de una instancia administrada|Sí|Sí|

[Comandos de PowerShell](https://docs.microsoft.com/powershell/module/az.sql/) disponibles

|Cmdlet |DESCRIPCIÓN |
|:---|:---|
|[New-AzSqlInstancePool](/powershell/module/az.sql/new-azsqlinstancepool/) | Crea un grupo de instancias de Azure SQL Database. |
|[Get-AzSqlInstancePool](/powershell/module/az.sql/get-azsqlinstancepool/) | Devuelve información sobre el grupo de instancias de Azure SQL. |
|[Set-AzSqlInstancePool](/powershell/module/az.sql/set-azsqlinstancepool/) | Establece las propiedades para un grupo de instancias de Azure SQL Database. |
|[Remove-AzSqlInstancePool](/powershell/module/az.sql/remove-azsqlinstancepool/) | Quita un grupo de instancias de Azure SQL Database. |
|[Get-AzSqlInstancePoolUsage](/powershell/module/az.sql/get-azsqlinstancepoolusage/) | Devuelve información sobre el uso de los grupos de instancias de Azure SQL. |


Para usar PowerShell, [instale la versión más reciente de PowerShell Core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell#powershell-core) y siga las instrucciones para [instalar el módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

En el caso de las operaciones relacionadas con instancias dentro de grupos e instancias únicas, use los [comandos de instancia administrada](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances) estándar, pero la propiedad de *nombre del grupo de instancias* se debe rellenar al usar estos comandos para una instancia de un grupo.

## <a name="how-to-deploy-managed-instances-into-pools"></a>Implementación de las instancias administradas en los grupos

El proceso de implementación de una instancia en un grupo consta de estos dos pasos:

1. Implementación de un grupo de instancias de un solo uso. Se trata de una operación de larga duración, que dura lo mismo que la implementación de una [instancia única creada en una subred vacía](sql-database-managed-instance.md#managed-instance-management-operations).

2. Implementación de instancias repetitivas en un grupo de instancias. El parámetro del grupo de instancias se debe especificar de manera explícita como parte de esta operación. Se trata de una operación relativamente rápida que por lo general tarda hasta 5 minutos.

En la versión preliminar pública, ambos pasos solo son compatibles con las plantillas de Resource Manager y PowerShell. La experiencia de Azure Portal no está disponible actualmente.

Una vez implementada una instancia administrada en un grupo, *puede* usar Azure Portal para cambiar sus propiedades en la página de planes de tarifa.


## <a name="create-an-instance-pool"></a>Creación de un grupo de instancias

Para crear un grupo de instancias:

1. [Cree una red virtual con una subred](#create-a-virtual-network-with-a-subnet).
2. [Cree un grupo de instancias](#create-an-instance-pool).


### <a name="create-a-virtual-network-with-a-subnet"></a>Creación de una red virtual con una subred 

Para colocar varios grupos de instancias dentro de la misma red virtual, consulte los artículos siguientes:

- [Determinación del tamaño de la red virtual y la subred para Instancia administrada de Azure SQL Database](sql-database-managed-instance-determine-size-vnet-subnet.md).
- Cree una subred y red virtual nuevas con la [plantilla de Azure Portal](sql-database-managed-instance-create-vnet-subnet.md) o siga las instrucciones para [preparar una red virtual existente](sql-database-managed-instance-configure-vnet-subnet.md).
 


### <a name="create-an-instance-pool"></a>Creación de un grupo de instancias 

Después de completar los pasos anteriores, ya está preparado para crear un grupo de instancias.

Las restricciones siguientes se aplican a los grupos de instancias:

- Solo los grupos de instancias de uso general y Gen5 están disponibles en la versión preliminar pública.
- El nombre del grupo solo puede contener minúsculas, números y guiones y no puede empezar con un guion.
- Para obtener el identificador de subred, use `Get-AzVirtualNetworkSubnetConfig -Name "miPoolSubnet" -VirtualNetwork $virtualNetwork`.
- Si quiere usar AHB (Ventaja híbrida de Azure), se aplica en el nivel de grupo de instancias. Puede establecer el tipo de licencia durante la creación del grupo o actualizarlo en cualquier momento después de la creación.

> [!IMPORTANT]
> La implementación de un grupo de instancias es una operación de larga duración que tarda aproximadamente 4,5 horas.

Para crear un grupo de instancias:

```powershell
$instancePool = New-AzSqlInstancePool `
  -ResourceGroupName "myResourceGroup" `
  -Name "mi-pool-name" `
  -SubnetId "/subscriptions/subscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/miPoolVirtualNetwork/subnets/miPoolSubnet" `
  -LicenseType "LicenseIncluded" `
  -VCore 80 `
  -Edition "GeneralPurpose" `
  -ComputeGeneration "Gen5" `
  -Location "westeurope"
```

> [!IMPORTANT]
> Dado que la implementación de un grupo de instancias es una operación de larga duración, debe esperar hasta que se complete antes de ejecutar cualquiera de los pasos siguientes descritos en este artículo.

## <a name="create-a-managed-instance-inside-the-pool"></a>Creación de una instancia administrada dentro del grupo 

Después de la implementación correcta del grupo de instancias, es momento de crear una instancia dentro de él.

Para crear una instancia administrada, ejecute el comando siguiente:

```powershell
$instanceOne = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 2 -StorageSizeInGB 256
```

La implementación de una instancia dentro de un grupo tarda un par de minutos. Una vez creada la primera instancia, se pueden crear instancias adicionales:

```powershell
$instanceTwo = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 4 -StorageSizeInGB 512
```

## <a name="create-a-database-inside-an-instance"></a>Creación de una base de datos dentro de una instancia 

Para crear y administrar bases de datos en una instancia administrada que se encuentre dentro de un grupo, use los comandos de instancia única.

Para crear una base de datos dentro de una instancia administrada:

```powershell
$poolinstancedb = New-AzSqlInstanceDatabase -Name "mipooldb1" -InstanceName "poolmi-001" -ResourceGroupName "myResourceGroup"
```


## <a name="get-instance-pool-usage"></a>Obtención del uso de un grupo de instancias 
 
Para obtener una lista de instancias dentro de un grupo:

```powershell
$instancePool | Get-AzSqlInstance
```


Para obtener el uso de los recursos de un grupo:

```powershell
$instancePool | Get-AzSqlInstancePoolUsage
```


Para obtener información general detallada sobre el uso del grupo y las instancias que contiene:

```powershell
$instancePool | Get-AzSqlInstancePoolUsage –ExpandChildren
```

Para enumerar las bases de datos de una instancia:

```powershell
$databases = Get-AzSqlInstanceDatabase -InstanceName "pool-mi-001" -ResourceGroupName "resource-group-name"
```


> [!NOTE]
> Hay un límite de 100 bases de datos por grupo (no por instancia).


## <a name="scale-a-managed-instance-inside-a-pool"></a>Escala de una instancia administrada dentro de un grupo 


Después de rellenar una instancia administrada con bases de datos, puede alcanzar los límites de instancias con respecto al almacenamiento o al rendimiento. En ese caso, si no se ha superado el uso del grupo, puede escalar la instancia.
Escalar una instancia administrada dentro de un grupo es una operación que tarda un par de minutos. El requisito previo para el escalado son núcleos virtuales y almacenamiento disponibles en el nivel de grupo de instancias.

Para actualizar el número de núcleos virtuales y el tamaño de almacenamiento:

```powershell
$instanceOne | Set-AzSqlInstance -VCore 8 -StorageSizeInGB 512 -InstancePoolName "mi-pool-name"
```


Para actualizar solo el tamaño de almacenamiento:

```powershell
$instance | Set-AzSqlInstance -StorageSizeInGB 1024 -InstancePoolName "mi-pool-name"
```



## <a name="connect-to-a-managed-instance-inside-a-pool"></a>Conexión a una instancia administrada dentro de un grupo

Para conectarse a una instancia administrada en un grupo, se necesitan los dos pasos siguientes:

1. [Habilite el punto de conexión público para la instancia](#enable-the-public-endpoint-for-the-instance).
2. [Agregue una regla de entrada al grupo de seguridad de red (NSG)](#add-an-inbound-rule-to-the-network-security-group).

Una vez completados ambos pasos, puede conectarse a la instancia de mediante una dirección de punto de conexión pública, un puerto y las credenciales proporcionadas durante la creación de la instancia. 

### <a name="enable-the-public-endpoint-for-the-instance"></a>Habilitación del punto de conexión público para la instancia

La habilitación del punto de conexión público para una instancia se puede realizar a través de Azure Portal o mediante el comando de PowerShell siguiente:


```powershell
$instanceOne | Set-AzSqlInstance -InstancePoolName "pool-mi-001" -PublicDataEndpointEnabled $true
```

Este parámetro también se puede establecer durante la creación de la instancia.

### <a name="add-an-inbound-rule-to-the-network-security-group"></a>Incorporación de una regla de entrada al grupo de seguridad de red 

Este paso se puede realizar a través de Azure Portal o mediante comandos de PowerShell y se puede hacer en cualquier momento después de preparar la subred para la instancia administrada.

Para información detallada, consulte [Permitir el tráfico del punto de conexión público en el grupo de seguridad de red](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group).


## <a name="move-an-existing-single-instance-inside-an-instance-pool"></a>Traslado de una instancia única existente dentro de un grupo de instancias 
 
Mover instancias dentro y fuera de un grupo es una de las limitaciones de la versión preliminar pública. Una solución alternativa que se puede usar depende de la restauración a un momento dado de bases de datos desde una instancia fuera de un grupo a una instancia que ya está en un grupo. 

Ambas instancias deben estar en la misma suscripción y región. Actualmente no se admite la restauración entre regiones y entre suscripciones.

Este proceso tiene un período de tiempo de inactividad.

Para mover bases de datos existentes:

1. Pause las cargas de trabajo de la instancia administrada desde la cual se realizará la migración.
2. Genere scripts para crear bases de datos del sistema y ejecútelos en la instancia que se encuentra dentro del grupo de instancias.
3. Realice una restauración a un momento dado de cada base de datos desde la instancia única a la instancia del grupo.

    ```powershell
    $resourceGroupName = "my resource group name"
    $managedInstanceName = "my managed instance name"
    $databaseName = "my source database name"
    $pointInTime = "2019-08-21T08:51:39.3882806Z"
    $targetDatabase = "name of the new database that will be created"
    $targetResourceGroupName = "resource group of instance pool"
    $targetInstanceName = "pool instance name"
       
    Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
      -ResourceGroupName $resourceGroupName `
      -InstanceName $managedInstanceName `
      -Name $databaseName `
      -PointInTime $pointInTime `
      -TargetInstanceDatabaseName $targetDatabase `
      -TargetResourceGroupName $targetResourceGroupName `
      -TargetInstanceName $targetInstanceName
    ```

4. Dirija la aplicación a la instancia nueva y reanude sus cargas de trabajo.

Si hay varias bases de datos, repita el proceso para cada base de datos.


## <a name="next-steps"></a>Pasos siguientes

- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](sql-database-features.md).
- Para más información sobre la configuración de redes virtuales, vea [Configuración de una red virtual de instancia administrada](sql-database-managed-instance-connectivity-architecture.md).
- Para ver una guía de inicio rápido en la que se crea una instancia administrada y se restaura una base de datos desde un archivo de copia de seguridad, vea [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
- Para consultar un tutorial con Azure Database Migration Service (DMS) para la migración, vea [Migración a Instancia administrada con DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para una supervisión avanzada del rendimiento de una base de datos de instancia administrada con inteligencia de solución de problemas integrada, consulte [Supervisión de instancias de Azure SQL Database con Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).
- Para obtener información de precios, vea [Precios de Instancia administrada de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
