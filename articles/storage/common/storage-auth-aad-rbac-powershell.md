---
title: Usar Azure PowerShell para administrar los derechos de acceso de Azure AD a los datos de blob y cola con RBAC - Azure Storage
description: Use Azure PowerShell para asignar el acceso a colas y contenedores mediante el control de acceso basado en roles (RBAC). Azure Storage admite roles RBAC tanto integrados como personalizados para la autenticación a través de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/25/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f34e82a0011260ace4ffeed095903b033529a58d
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68514993"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-using-powershell"></a>Conceder acceso a datos de blob y cola de Azure con RBAC mediante PowerShell

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Azure Storage define un conjunto de roles de RBAC integrados que abarcan conjuntos comunes de permisos utilizados para acceder a los contenedores o las colas. 

Cuando un rol RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos a esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación o una [identidad de servicio administrada para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

En este artículo se describe cómo usar Azure PowerShell para mostrar los roles RBAC integrados y asignarlos a usuarios. Para obtener más información sobre cómo usar Azure PowerShell, vea [Introducción a Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Determinar el ámbito de recursos

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Mostrar los roles RBAC disponibles

Para obtener una lista de los roles RBAC integrados disponibles con Azure PowerShell, use el comando [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition):

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Aparecerá una lista con los roles de datos de Azure Storage integrados, así como otros roles integrados de Azure:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-security-principal"></a>Asignación de un rol RBAC a una entidad de seguridad

Para asignar un rol RBAC a una entidad de seguridad, use el comando [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment). El formato del comando puede variar en función del ámbito de la asignación. Los ejemplos siguientes muestran cómo asignar un rol a un usuario en varios ámbitos, pero puede usar el mismo comando para asignar un rol a cualquier entidad de seguridad.

### <a name="container-scope"></a>Ámbito de contenedor

Para asignar un rol cuyo ámbito es un contenedor, especifique una cadena que contenga el ámbito del contenedor en el parámetro `--scope`. El ámbito de un contenedor tiene este formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

En el siguiente ejemplo se asigna el rol **Colaborador de datos de Storage Blob** a un usuario y el ámbito se establece un contenedor denominado *sample-container*. Asegúrese de reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes angulares por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Ámbito de cola

Para asignar un rol cuyo ámbito es una cola, especifique una cadena que contenga el ámbito de la cola en el parámetro `--scope`. El ámbito de una cola tiene este formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

En el siguiente ejemplo se asigna el rol **Colaborador de datos de la cola de Storage Blob** a un usuario y el ámbito se establece una cola denominada *sample-queue*. Asegúrese de reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes angulares por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Ámbito de cuenta de almacenamiento

Para asignar un rol cuyo ámbito es la cuenta de almacenamiento, especifique el ámbito del recurso de la cuenta de almacenamiento en el parámetro `--scope`. El ámbito de una cuenta de almacenamiento tiene este formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

En el siguiente ejemplo se muestra cómo establecer el ámbito del rol **Lector de datos de Storage Blob** en un usuario en el nivel de la cuenta de almacenamiento. Asegúrese de reemplazar los valores de ejemplo por sus propios valores: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Ámbito de grupo de recursos

Para asignar un rol cuyo ámbito es el grupo de recursos, especifique el identificador o el nombre del grupo de recursos en el parámetro `--resource-group`. En el siguiente ejemplo se asigna el rol **Lector de datos de la cola de Storage Blob** a un usuario en el nivel del grupo de recursos. Asegúrese de reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes angulares por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Ámbito de suscripción

Para asignar un rol cuyo ámbito es la suscripción, especifique el ámbito de la suscripción en el parámetro `--scope`. El ámbito de una suscripción tiene este formato:

```
/subscriptions/<subscription>
```

En el siguiente ejemplo se muestra cómo asignar el rol **Lector de datos de Storage Blob** a un usuario en el nivel de la cuenta de almacenamiento. Asegúrese de reemplazar los valores de ejemplo por sus propios valores: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso a los recursos de Azure mediante RBAC y Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Conceder acceso a datos de blob y de cola de Azure con RBAC mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC en Azure Portal](storage-auth-aad-rbac-portal.md)
