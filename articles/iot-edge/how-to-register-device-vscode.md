---
title: 'Registre un dispositivo nuevo desde Visual Studio Code: Azure IoT Edge | Microsoft Docs'
description: Uso de Visual Studio Code para crear un nuevo dispositivo IoT Edge en Azure IoT Edge y recuperar la cadena de conexión
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c8fce104d48acc3a562599c65eb15cb0a66657b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495271"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registrar un nuevo dispositivo de Azure IoT Edge desde Visual Studio Code

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con IoT Hub. Una vez que registre un dispositivo, recibirá una cadena de conexión que se puede usar para configurar el dispositivo para las cargas de trabajo de IoT Edge.

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante Visual Studio Code (VS Code). Hay varias maneras de realizar la mayoría de operaciones en VS Code. En este artículo se usa Explorer, pero también puede usar la paleta de comandos para ejecutar los pasos.

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) para Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sesión para acceder a IoT Hub

Puede usar las extensiones de Azure IoT para Visual Studio Code para realizar operaciones con IoT Hub. Para que estas operaciones funcionen, debe iniciar sesión en su cuenta de Azure y seleccionar su instancia de IoT Hub.

1. En Visual Studio Code, abra la vista **Explorer** (Explorador).

1. En la parte inferior de Explorer, expanda la sección **Azure IoT Hub**.

   ![Sección Expansión de los dispositivos de Azure IoT Hub](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

1. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub**. Si no ve el botón de los puntos suspensivos, mantenga el ratón sobre el encabezado o haga clic en él.

1. Elija **Select IoT Hub** (Seleccionar IoT Hub).

1. Si no ha iniciado sesión en su cuenta de Azure, siga las indicaciones para hacerlo.

1. Seleccione su suscripción a Azure.

1. Seleccione IoT Hub.

## <a name="create-a-device"></a>Crear un dispositivo

1. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub).

1. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el ratón sobre el encabezado o haga clic en él.

1. Seleccione **Create IoT Edge Device** (Crear un dispositivo de IoT Edge).

1. En el cuadro de texto que aparece, otorgue a su dispositivo un identificador.

En la pantalla de salida, consulte el resultado del comando. Se imprime la información del dispositivo, que incluye el valor de **deviceId** que proporcionó, y el valor de **connectionString** que puede usar para conectar su dispositivo físico a su instancia de IoT Hub.

## <a name="view-all-devices"></a>Ver todos los dispositivos

Todos los dispositivos que se conectan a IoT Hub se enumeran en la sección **Azure IoT Hub** del explorador de Visual Studio Code. Los dispositivos de IoT Edge se distinguen de los dispositivos que no son Edge con un icono diferente y por el hecho de que los módulos **$edgeAgent** y **$edgeHub** se implementan en cada dispositivo IoT Edge.

   ![Visualización de todos los dispositivos IoT Edge en su centro de IoT](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. Haga clic con el botón derecho en el identificador del dispositivo en la sección **Azure IoT Hub**.

1. Seleccione **Copy Device Connection String** (Copiar la cadena de conexión del dispositivo).

   La cadena de conexión se copiará al portapapeles.

También puede seleccionar **Get Device Info** (Obtener información del dispositivo), si hace clic con el botón derecho del mouse en el menú para ver toda la información del dispositivo, incluyendo la cadena de conexión, en la ventana de salida.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar módulos en un dispositivo con Visual Studio Code](how-to-deploy-modules-vscode.md).
