---
title: Comunicación con una aplicación de dispositivo en C# mediante flujos de dispositivo de Azure IoT Hub (versión preliminar) | Microsoft Docs
description: En este inicio rápido, va a ejecutar dos aplicaciones C# de ejemplo que se comunican mediante un flujo de dispositivo establecido con IoT Hub.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: de581362371e28523c99f961dfdb5c2009901343
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446119"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Inicio rápido: Comunicación con una aplicación de dispositivo en C# mediante flujos de dispositivo de IoT Hub (versión preliminar)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub admite actualmente flujos de dispositivos como una [característica en vista previa (GB)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Los flujos de dispositivo de IoT Hub](./iot-hub-device-streams-overview.md) permiten que las aplicaciones de servicio y de dispositivo se comuniquen de forma segura y apta para el firewall. En este inicio rápido se trabaja con dos aplicaciones de C# que aprovechan los flujos de dispositivo para enviar datos de un lado a otro (eco).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* La versión preliminar de los flujos de dispositivo solo se admite en este momento en instancias de IoT Hub creadas en las siguientes regiones:
  * Centro de EE. UU.
  * EUAP del centro de EE. UU.

* Las dos aplicaciones de ejemplo que se ejecutan en este inicio rápido se escriben con C#. Necesita el SDK de .NET Core 2.1.0 o una versión posterior en la máquina de desarrollo.
  * Descargue el [SDK de .NET Core para varias plataformas desde .NET](https://www.microsoft.com/net/download/all).
  * Verifique la versión actual de C# en la máquina de desarrollo con el comando siguiente:

   ```
   dotnet --version
   ```

* Ejecute el siguiente comando para agregar la extensión IoT de Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* [Descargue el proyecto de C# de ejemplo](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) y extraiga el archivo ZIP. Lo necesitará en el lado del dispositivo y el lado del servicio.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta sección, usará Azure Cloud Shell para registrar un dispositivo simulado.

1. Ejecute el siguiente comando en Cloud Shell para crear la identidad del dispositivo:

   > [!NOTE]
   > * Reemplace el marcador de posición *YourIoTHubName* por el nombre que eligió para su centro de IoT.
   > * Use *MyDevice* tal como se muestra. Es el nombre que se da al dispositivo registrado. Si elige otro nombre para el dispositivo, úselo en todo el artículo y actualice el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Ejecute el siguiente comando en Cloud Shell para obtener la *cadena de conexión del dispositivo* que acaba de registrar:

   > [!NOTE]
   > Reemplace el marcador de posición *YourIoTHubName* por el nombre que eligió para su centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Anote la cadena de conexión del dispositivo para usarla más adelante en este inicio rápido. Debe ser similar al ejemplo siguiente:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

3. También necesita la *cadena de conexión de servicio* de su centro de IoT para que la aplicación del lado del servicio pueda conectarse al centro de IoT y establecer el flujo de dispositivo. El siguiente comando recupera este valor para el centro de IoT:

   > [!NOTE]
   > Reemplace el marcador de posición *YourIoTHubName* por el nombre que eligió para su centro de IoT.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Anote el valor devuelto para su uso posterior en este inicio rápido. Debe ser similar al ejemplo siguiente:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Comunicación entre un dispositivo y un servicio mediante flujos de dispositivo

En esta sección, ejecuta la aplicación del lado del dispositivo y la aplicación del lado del servicio y se comunica entre las dos.

### <a name="run-the-service-side-application"></a>Ejecución de la aplicación del lado del servicio

Vaya al directorio *iot-hub/Quickstarts/device-streams-echo/service* de la carpeta de proyecto descomprimida. Tenga la siguiente información a mano:

| Nombre de parámetro | Valor del parámetro |
|----------------|-----------------|
| `ServiceConnectionString` | Proporcione la cadena de conexión del servicio de la instancia de IoT Hub. |
| `DeviceId` | Proporcione el identificador del dispositivo que creó anteriormente, por ejemplo, *MyDevice*. |

Compile y ejecute el código de la siguiente forma:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Si la aplicación no responde a tiempo del lado del dispositivo, el tiempo expira.

### <a name="run-the-device-side-application"></a>Ejecución de la aplicación del lado del dispositivo

Vaya al directorio *iot-hub/Quickstarts/device-streams-echo/device* de la carpeta de proyecto descomprimida. Tenga la siguiente información a mano:

| Nombre de parámetro | Valor del parámetro |
|----------------|-----------------|
| `DeviceConnectionString` | Proporcione la cadena de conexión del dispositivo de la instancia de IoT Hub. |

Compile y ejecute el código de la siguiente forma:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Al final del último paso, la aplicación del lado del servicio inicia un flujo hacia el dispositivo. Una vez establecido el flujo, la aplicación envía un búfer de cadena al servicio a través de este flujo. En este ejemplo, la aplicación del lado de servicio simplemente devuelve los mismos datos al dispositivo, lo que demuestra que la comunicación bidireccional entre las dos aplicaciones es correcta.

Salida de la consola en el lado del dispositivo:

![Salida de la consola en el lado del dispositivo](./media/quickstart-device-streams-echo-csharp/device-console-output.png)

Salida de la consola en el lado del servicio:

![Salida de la consola en el lado del servicio](./media/quickstart-device-streams-echo-csharp/service-console-output.png)

El tráfico que se envía por el flujo se tuneliza mediante IoT Hub en lugar de enviarse directamente. Las ventajas ofrecidas se detallan en la sección [Ventajas](./iot-hub-device-streams-overview.md#benefits) de los flujos de dispositivo.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha configurado un centro de IoT, ha registrado un dispositivo, ha establecido un flujo de dispositivo entre las aplicaciones C# de dispositivo y de servicio, y ha usado el flujo para enviar datos entre las aplicaciones.

Para más información sobre los flujos de dispositivo, consulte:

> [!div class="nextstepaction"]
> [Introducción a los flujos de dispositivo](./iot-hub-device-streams-overview.md)
