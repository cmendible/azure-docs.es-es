---
title: Creación y cifrado de una máquina virtual Linux con Azure PowerShell
description: En esta guía de inicio rápido, aprenderá a usar Azure PowerShell para crear y cifrar una máquina virtual Linux
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 123d3e6ad0312a76540b68a28abf13008d419ca7
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331415"
---
# <a name="quickstart-create-and-encrypt-a-linux-virtual-machine-in-azure-with-powershell"></a>Inicio rápido: Creación y cifrado de una máquina virtual Linux en Azure con PowerShell

El módulo de Azure PowerShell se usa para crear y administrar recursos de Azure desde la línea de comandos de PowerShell o en scripts. En esta guía de inicio rápido se le muestra cómo usar el módulo de Azure PowerShell para crear una máquina virtual (VM) Linux, crear una instancia de Key Vault para el almacenamiento de claves de cifrado y cifrar la máquina virtual. En esta guía de inicio rápido se usa la imagen de Marketplace de Ubuntu 16.04 LTS de Canonical y un tamaño Standard_D2S_V3 de VM. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos de Azure con [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y administran los recursos de Azure.

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Cree una máquina virtual de Azure con [New-AzVM](/powershell/module/az.compute/new-azvm), a la cual pasa el objeto de configuración de máquina virtual.

```powershell
$securePassword = ConvertTo-SecureString 'AZUREuserPA$$W0RD' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:16.04-LTS:latest -Size Standard_D2S_V3
```

La implementación de la máquina virtual tardará unos minutos. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Creación de una instancia de Key Vault configurada para claves de cifrado

Azure Disk Encryption almacena su clave de cifrado en una instancia de Azure Key Vault. Cree una instancia de Key Vault con [New-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault). Para habilitar la instancia de Key Vault para almacenar claves de cifrado, use el parámetro -EnabledForDiskEncryption.

> [!Important]
> Cada instancia de Key Vault debe tener un nombre único. En el siguiente ejemplo se crea una instancia de Key Vault llamada *myKV*, pero debe llamar a la suya de forma diferente.

```powershell
New-AzKeyvault -name MyKV -ResourceGroupName myResourceGroup -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Cifrado de la máquina virtual

Cifre su máquina virtual con [Set-AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

Set-AzVmDiskEncryptionExtension requiere algunos valores de su objeto de Key Vault. Puede obtener estos valores pasando el nombre único de su almacén de claves a [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault).

```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName MyVM -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

Después de unos minutos, el proceso devolverá el siguiente resultado:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

Puede comprobar el proceso de cifrado ejecutando [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus).

```powershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Al habilitarse el cifrado, verá lo siguiente en la salida que se devuelve:

```
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : NotMounted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos, la VM y todos los recursos relacionados:

```powershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, creó una máquina virtual y una instancia de Key Vault que se habilitó para claves de cifrado, y cifró la máquina virtual.  En el artículo siguiente obtendrá más información acerca de los requisitos previos de Azure Disk Encryption para máquinas virtuales IaaS.

> [!div class="nextstepaction"]
> [Requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)