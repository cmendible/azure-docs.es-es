---
title: Inicio rápido para controlar un dispositivo desde Azure IoT Hub (Android) | Microsoft Docs
description: En este inicio rápido, ejecuta dos aplicaciones de Java de muestra. Una aplicación es una aplicación de servicio que puede controlar dispositivos conectados al centro de manera remota. La otra aplicación se ejecuta en un dispositivo físico o simulado conectado al centro que se puede controlar de manera remota.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.author: wesmc
ms.openlocfilehash: d125328d903b419aa81c54ffecf1f549d4cb4fe2
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330785"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-android"></a>Guía de inicio rápido: Control de un dispositivo conectado a IoT Hub (Android)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub es un servicio de Azure que le permite ingerir grandes volúmenes de datos de telemetría desde los dispositivos IoT en la nube y administrar dispositivos desde la nube. En este inicio rápido, usa un *método directo* para controlar un dispositivo simulado conectado a IoT Hub. Puede usar métodos directos para cambiar el comportamiento de un dispositivo conectado a IoT Hub de manera remota.

El inicio rápido usa dos aplicaciones Java escritas anteriormente:

* Una aplicación de dispositivo simulado que responde a métodos directos que se llaman desde una aplicación de servicio. Para recibir las llamadas de método directo, esta aplicación se conecta a un punto de conexión específico del dispositivo en IoT Hub.

* Una aplicación de servicio que llama al método directo en el dispositivo Android. Para llamar a un método directo en un dispositivo, esta aplicación se conecta a un punto de conexión de servicio en IoT Hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Android Studio desde https://developer.android.com/studio/. Para más información relativa a la instalación de Android Studio, consulte [android-installation](https://developer.android.com/studio/install).

* En el ejemplo de este artículo se usa Android SDK 27.

* Ejecute el siguiente comando para agregar la extensión IoT de Microsoft Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* Esta guía de inicio rápido requiere dos aplicaciones de ejemplo: la [aplicación de Android de ejemplo del SDK de dispositivo](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample) y la [aplicación de Android de ejemplo del SDK de servicio](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/service/AndroidSample). Ambos ejemplos forman parte del repositorio azure-iot-samples-java de GitHub. Descargue o clone el repositorio [azure-iot-samples-java](https://github.com/Azure-Samples/azure-iot-samples-java).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Si ha completado la anterior [Guía de inicio rápido: Envío de datos de telemetría desde un dispositivo a IoT Hub](quickstart-send-telemetry-android.md), puede omitir este paso y usar el centro de IoT que ya ha creado.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Si ha completado la anterior [Guía de inicio rápido: Envío de datos de telemetría desde un dispositivo a IoT Hub](quickstart-send-telemetry-android.md), puede omitir este paso y usar el mismo dispositivo que se registró en la guía anterior.

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta guía de inicio rápido, usará Azure Cloud Shell para registrar un dispositivo simulado.

1. Ejecute los siguientes comandos en Azure Cloud Shell para crear la identidad del dispositivo.

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

   **MyAndroidDevice**: este valor es el nombre que se da al dispositivo registrado. Use MyAndroidDevice como se muestra a continuación. Si elige otro nombre para el dispositivo, puede que tenga que usarlo en todo el artículo y actualizar el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyAndroidDevice
    ```

2. Ejecute los siguientes comandos en Azure Cloud Shell para obtener la _cadena de conexión del dispositivo_ que acaba de registrar:

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyAndroidDevice \
      --output table
    ```

    Anote la cadena de conexión del dispositivo, que se parecerá a esta:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Usará este valor más adelante en este inicio rápido.

## <a name="retrieve-the-service-connection-string"></a>Recuperación de la cadena de conexión de servicio

También necesitará una _cadena de conexión del servicio_ para permitir que las aplicaciones del servicio back-end se conecten al centro de IoT y recuperen los mensajes. El comando siguiente recupera la cadena de conexión del servicio de su instancia de IoT Hub:

**YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

```azurecli-interactive
az iot hub show-connection-string --name YourIoTHubName --policy-name service --output table
```

Anote la cadena de conexión del servicio, que se parecerá a esta:

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Usará este valor más adelante en este inicio rápido. La cadena de conexión de servicio es diferente de la cadena de conexión de dispositivo.

## <a name="listen-for-direct-method-calls"></a>Escuchas para llamadas de método directo

La aplicación de ejemplo SDK de dispositivo se puede ejecutar en un dispositivo Android físico o en un emulador de Android. El ejemplo se conecta a un punto de conexión específico del dispositivo en IoT Hub, envía los datos de telemetría simulados y escucha llamadas de método directo desde el centro. En este inicio rápido, la llamada de método directo desde el centro indica al dispositivo que debe cambiar el intervalo en el que envía los datos de telemetría. El dispositivo simulado envía una confirmación al centro después de que ejecute el método directo.

1. En Android Studio, abra el proyecto de Android de ejemplo de GitHub. El proyecto se encuentra en el siguiente directorio de la copia que ha clonado o descargado del repositorio [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java).

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. En Android Studio, abra *gradle.properties* para el proyecto de ejemplo y reemplace el marcador de posición **Device_Connection_String** por la cadena de conexión de dispositivo que anotó anteriormente.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. En Android Studio, haga clic en **File**(Archivo) > **Sync Project with Gradle Files** (Sincronizar proyecto con archivos de Gradle). Compruebe que la compilación se completa.

   > [!NOTE]
   > Si se produce un error en la sincronización del proyecto, puede ser por uno de los siguientes motivos:
   >
   > * Las versiones del complemento Gradle para Android y Gradle al que se hace referencia en el proyecto están en desuso para su versión de Android Studio. Siga [estas instrucciones](https://developer.android.com/studio/releases/gradle-plugin) para hacer referencia a las versiones correctas del complemento y de Gradle e instalarlas.
   > * No se ha firmado el contrato de licencia de Android SDK. Siga las instrucciones de la salida de compilación para firmar el contrato de licencia y descargar el SDK.

4. Una vez completada la compilación, haga clic en **Run**(Ejecutar) > **Run 'app'** (Ejecutar "aplicación"). Configure la aplicación para que se ejecute en un dispositivo Android físico o un emulador de Android. Para más información sobre la ejecución de una aplicación de Android en un dispositivo físico o en un emulador, consulte [Ejecutar la aplicación](https://developer.android.com/training/basics/firstapp/running-app).

5. Cuando se cargue la aplicación, haga clic en el botón **Iniciar** para empezar a enviar datos de telemetría a IoT Hub:

    ![Captura de pantalla de ejemplo de la aplicación Android del dispositivo cliente](media/quickstart-control-device-android/sample-screenshot.png)

Esta aplicación debe dejarse en ejecución en un dispositivo físico o en un emulador mientras ejecuta la aplicación de ejemplo SDK de servicio para actualizar el intervalo de telemetría durante el tiempo de ejecución.

## <a name="read-the-telemetry-from-your-hub"></a>Lectura de los datos de telemetría procedentes de su instancia de IoT Hub

En esta sección, usará Azure Cloud Shell con la [extensión de IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) para supervisar los mensajes de dispositivo que envía el dispositivo Android.

1. Mediante Azure Cloud Shell, ejecute el siguiente comando para conectarse y leer mensajes desde el centro de IoT:

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

    La siguiente captura de pantalla muestra la salida en la que IoT Hub recibe los datos de telemetría que el dispositivo Android ha enviado:

      ![Leer los mensajes de dispositivo mediante la CLI de Azure](media/quickstart-control-device-android/read-data.png)

De forma predeterminada la aplicación de telemetría envía datos de telemetría desde el dispositivo Android cada 5 segundos. En la sección siguiente, usará una llamada al método directo para actualizar el intervalo de telemetría del dispositivo IoT de Android.

## <a name="call-the-direct-method"></a>Llamar al método directo

La aplicación de servicio se conecta a un punto de conexión de servicio en IoT Hub. La aplicación realiza llamadas de método directo a un dispositivo con IoT Hub y realiza escuchas para confirmaciones.

Ejecute esta aplicación en un dispositivo Android físico independiente o en un emulador de Android.

Una aplicación de servicio de back-end de IoT Hub normalmente se ejecuta en la nube donde es más fácil mitigar los riesgos asociados a la cadena de conexión confidencial que controla todos los dispositivos de IoT Hub. En este ejemplo, la vamos a ejecutar como una aplicación de Android solo con fines de demostración. Las versiones de esta guía de inicio rápido en otros idiomas proporcionan otros ejemplos que se adaptan mejor a una aplicación de servicio de back-end.

1. En Android Studio, abra el proyecto de Android de ejemplo de servicio de GitHub. El proyecto se encuentra en el siguiente directorio de la copia que ha clonado o descargado del repositorio [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java).

        \azure-iot-samples-java\iot-hub\Samples\service\AndroidSample

2. En Android Studio, abra *gradle.properties* para el proyecto de ejemplo y actualice el valor de las propiedades **ConnectionString** y **DeviceId** con la cadena de conexión de servicio que anotó anteriormente y el identificador de dispositivo Android que registró.

    ```
    ConnectionString=HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}
    DeviceId=MyAndroidDevice
    ```

3. En Android Studio, haga clic en **File**(Archivo) > **Sync Project with Gradle Files** (Sincronizar proyecto con archivos de Gradle). Compruebe que la compilación se completa.

   > [!NOTE]
   > Si se produce un error en la sincronización del proyecto, puede ser por uno de los siguientes motivos:
   >
   > * Las versiones del complemento Gradle para Android y Gradle al que se hace referencia en el proyecto están en desuso para su versión de Android Studio. Siga [estas instrucciones](https://developer.android.com/studio/releases/gradle-plugin) para hacer referencia a las versiones correctas del complemento y de Gradle e instalarlas.
   > * No se ha firmado el contrato de licencia de Android SDK. Siga las instrucciones de la salida de compilación para firmar el contrato de licencia y descargar el SDK.

4. Una vez completada la compilación, haga clic en **Run**(Ejecutar) > **Run 'app'** (Ejecutar "aplicación"). Configure la aplicación para que se ejecute en un dispositivo Android físico independiente o un emulador de Android. Para más información sobre la ejecución de una aplicación de Android en un dispositivo físico o en un emulador, consulte [Ejecutar la aplicación](https://developer.android.com/training/basics/firstapp/running-app).

5. Cuando se cargue la aplicación, actualice el valor **Set Messaging Interval** (Establecer intervalo de mensajes) en **1000** y haga clic en **Invoke** (Invocar).

    El intervalo de mensajes de telemetría se da en milisegundos. El intervalo de telemetría predeterminado del dispositivo de ejemplo se ha establecido en 5 segundos. Este cambio actualizará el dispositivo IoT de Android para que se envíen datos de telemetría cada segundo.

    ![Especificar el intervalo de telemetría](media/quickstart-control-device-android/enter-telemetry-interval.png)

6. La aplicación recibirá una confirmación que indica si el método se ha ejecutado correctamente o no.

    ![Confirmación del método directo](media/quickstart-control-device-android/direct-method-ack.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha llamado a un método directo en un dispositivo desde una aplicación de back-end y ha respondido a la llamada de método directo en una aplicación de dispositivo simulado.

Para obtener información sobre cómo redirigir mensajes del dispositivo a la nube a diferentes destinos en la nube, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Enrutar datos de telemetría a distintos puntos de conexión para procesamiento](tutorial-routing.md)
