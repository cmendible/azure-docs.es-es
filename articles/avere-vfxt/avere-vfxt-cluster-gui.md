---
title: Acceso al panel de control de Avere vFXT - Azure
description: Cómo conectarse al clúster de vFXT y al panel de control basado en explorador de Avere para configurar Avere vFXT
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: v-erkell
ms.openlocfilehash: 830be92d37f304598cca05c3ac80973158c38a59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439985"
---
# <a name="access-the-vfxt-cluster"></a>Acceso al clúster de vFXT

Para cambiar la configuración y supervisar el clúster de Avere vFXT, use el panel de control de Avere. El panel de control de Avere es una interfaz gráfica basada en explorador para el clúster.

Dado que el clúster de vFXT se encuentra dentro de una red virtual privada, debe crear un túnel SSH o usar otro método para conectarse con la dirección IP de administración del clúster. Hay dos pasos básicos: 

1. Crear una conexión entre su estación de trabajo y la red virtual privada 
1. Cargar el panel de control del clúster en un explorador web 

> [!NOTE] 
> En este artículo se da por supuesto que ha establecido una dirección IP pública en el controlador del clúster o en otra VM dentro de la red virtual del clúster. En este artículo se describe cómo usar esa máquina virtual como host para acceder al clúster. Si usa una VPN o ExpressRoute para el acceso a la red virtual, vaya a [Conexión al panel de control de Avere](#connect-to-the-avere-control-panel-in-a-browser).

Antes de conectarse, asegúrese de que el par de claves pública y privada SSH que usó al crear el controlador del clúster está instalado en el equipo local. Lea la documentación de las claves SSH para [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) o para [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) si necesita ayuda. (Si utilizó una contraseña en lugar de una clave pública, se le pedirá que la escriba cuando se conecte). 

## <a name="create-an-ssh-tunnel"></a>Creación de un túnel SSH 

Puede crear un túnel SSH desde la línea de comandos de un sistema de cliente basado en Linux o Windows 10. 

Use un comando de tunelización SSH con este formato: 

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*\@*controller_public_IP*

Este comando se conecta a la dirección IP de administración del clúster a través de la dirección IP del controlador del clúster.

Ejemplo:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

La autenticación es automática si usa la clave pública SSH para crear el clúster y la clave correspondiente está instalada en el sistema cliente. Si usó una contraseña, el sistema le pedirá que la escriba.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Conexión al panel de control de Avere en un explorador

Este paso usa un explorador web para conectarse a la utilidad de configuración que se ejecutan en el clúster de vFXT.

* Para una conexión de túnel SSH, abra el explorador web y vaya a `https://127.0.0.1:8443`. 

  Se conectó a la dirección IP del clúster cuando creó el túnel, por lo que deberá usar la dirección IP del host local en el explorador. Si utilizó un puerto local distinto a 8443, use su número de puerto en su lugar.

* Si utiliza una VPN o ExpressRoute para comunicarse con el clúster, vaya a la dirección IP de la administración de clústeres en el explorador. Ejemplo: ``https://203.0.113.51``

Según el explorador, es posible que tenga que hacer clic en **Advanced** y comprobar que es seguro continuar a la página.

Especifique el nombre de usuario `admin` y la contraseña administrativa que proporcionó al crear el clúster.

![Captura de pantalla de la página de inicio de sesión de Avere con el nombre de usuario "admin" y una contraseña](media/avere-vfxt-gui-login.png)

Haga clic en **Login** o presione ENTRAR en el teclado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que puede acceder al clúster, habilite el [soporte](avere-vfxt-enable-support.md).
