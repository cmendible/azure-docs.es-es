---
title: 'Obtención de valores para la autenticación de aplicaciones: Azure SQL Database | Microsoft Docs'
description: Cree una entidad de servicio para tener acceso a SQL Database desde el código.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: d7c8c6788a8699c5b57c39731c148454ad8dcfcf
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569321"
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Obtención de los valores necesarios para autenticar una aplicación para obtener acceso a SQL Database desde el código

Para crear y administrar SQL Database desde el código debe registrar la aplicación en el dominio de Azure Active Directory (AAD) en la suscripción donde se han creado los recursos de Azure.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Creación de una entidad de servicio para acceder a recursos desde una aplicación

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager de PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y en los módulos AzureRm son esencialmente idénticos.

El siguiente script de PowerShell crea la aplicación de Active Directory (AD) y la entidad de servicio que se necesitan para autenticar la aplicación de C#. En la salida del script, se encuentran los valores que se necesitan para el anterior ejemplo de C#. Para ver información detallada, consulte [Uso de Azure PowerShell para crear una entidad de servicio para acceder a recursos](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

    # Sign in to Azure.
    Connect-AzAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create an AAD app
    $azureAdApplication = New-AzADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Otras referencias
* [Creación de una Base de datos SQL con C#](sql-database-get-started-csharp.md)
* [Conexión a SQL Database mediante autenticación de Azure Active Directory](sql-database-aad-authentication.md)

