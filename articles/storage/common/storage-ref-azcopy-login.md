---
title: azcopy login | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy login.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 2938d85becbea738acc21fc7b15991301eef759f
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196742"
---
# <a name="azcopy-login"></a>azcopy login

Inicia sesión en Azure Active Directory para acceder a recursos de Azure Storage.

## <a name="synopsis"></a>Sinopsis

Inicie sesión en Azure Active Directory para acceder a recursos de Azure Storage.

Para que se le autorice en su cuenta de Azure Storage, debe asignar el rol **Colaborador de datos de blobs de almacenamiento** a su cuenta de usuario en el contexto de la cuenta de almacenamiento, el grupo de recursos primario o la suscripción primaria.

Este comando almacenará en caché la información de inicio de sesión cifrada para el usuario actual que está utilizando los mecanismos integrados del sistema operativo.

Consulte los ejemplos para más información.

> [!IMPORTANT]
> Si establece una variable de entorno mediante la línea de comandos, esa variable se podrá leer en el historial de la línea de comandos. Considere la posibilidad de borrar las variables que contengan credenciales del historial de la línea de comandos. Para evitar que aparezcan variables en el historial, puede usar un script para pedir al usuario sus credenciales y establecer la variable de entorno.

```azcopy
azcopy login [flags]
```

## <a name="examples"></a>Ejemplos

Inicie sesión de forma interactiva con el identificador de inquilino de AAD predeterminado establecido en "common" (común):

```azcopy
azcopy login
```

Inicie sesión de forma interactiva con un identificador de inquilino especificado:

```azcopy
azcopy login --tenant-id "[TenantID]"
```

Inicie sesión con una identidad de máquina virtual asignada por el sistema:

```azcopy
azcopy login --identity
```

Inicie sesión mediante una identidad de máquina virtual asignada por el usuario con un identificador de cliente de la identidad de servicio:

```azcopy
azcopy login --identity --identity-client-id "[ServiceIdentityClientID]"
```

Inicie sesión mediante una identidad de máquina virtual asignada por el usuario con un identificador de objeto de la identidad de servicio:

```azcopy
azcopy login --identity --identity-object-id "[ServiceIdentityObjectID]"
```

Inicie sesión mediante una identidad de máquina virtual asignada por el usuario con un identificador de recurso de la identidad de servicio:

```azcopy
azcopy login --identity --identity-resource-id "/subscriptions/<subscriptionId>/resourcegroups/myRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID"
```

Inicie sesión como una entidad de servicio mediante un secreto de cliente. Establezca la variable de entorno AZCOPY_SPA_CLIENT_SECRET en el secreto de cliente para la autenticación de entidades de servicio basada en secretos.

```azcopy
azcopy login --service-principal
```

Inicie sesión como una entidad de servicio mediante un certificado y una contraseña. Establezca la variable de entorno AZCOPY_SPA_CERT_PASSWORD en la contraseña del certificado para la autorización de entidades de servicio basada en certificados.

```azcopy
azcopy login --service-principal --certificate-path /path/to/my/cert
```

Asegúrese de tratar/path/to/my/cert como una ruta de acceso a un archivo PEM o PKCS12. AzCopy no llega al almacén de certificados del sistema para obtener el certificado.

--certificate-path es obligatorio en la autenticación de entidades de servicio basada en certificados.

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|--application-id string|Identificador de aplicación de la identidad asignada por el usuario. Se requiere para la autenticación de entidades de servicio.|
|--certificate-path string|Ruta de acceso al certificado para la autenticación de SPN. Se requiere para la autenticación de entidades de servicio basada en certificados.|
|-h, --help|Muestra el contenido de la ayuda para el comando login.|
|--identity|Inicia sesión con la identidad de la máquina virtual, también conocida como Managed Service Identity (MSI).|
|--identity-client-id string|Identificador de cliente de la identidad asignada por el usuario.|
|--identity-object-id string|Identificador de objeto de la identidad asignada por el usuario.|
|--identity-resource-id string|Identificador de recurso de la identidad asignada por el usuario.|
|--service-principal|Inicia sesión a través de SPN (nombre de entidad de seguridad de servicio) con un certificado o un secreto. El secreto de cliente o la contraseña del certificado deben colocarse en la variable de entorno adecuada. Escriba `AzCopy env` para ver los nombres y descripciones de las variables de entorno.|
|--tenant-id string| El identificador de inquilino de Azure Active Directory que se usará para el inicio de sesión interactivo de dispositivos con OAuth.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
