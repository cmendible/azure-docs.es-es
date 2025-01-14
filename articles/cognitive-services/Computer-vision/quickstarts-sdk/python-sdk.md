---
title: 'Inicio rápido: Biblioteca cliente de Computer Vision para Python | Microsoft Docs'
description: Introducción a la biblioteca cliente de Computer Vision para Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/10/2019
ms.author: pafarley
ms.openlocfilehash: 8d47ae84fd489b4841d8bcf7755da6c30cf6035d
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70967001"
---
# <a name="quickstart-computer-vision-client-library-for-python"></a>Inicio rápido: Biblioteca cliente de Computer Vision para Python

El servicio Computer Vision proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información. Los algoritmos de Computer Vision analizan el contenido de una imagen de diferentes formas, en función de las características visuales que le interesen.

Puede utilizar la biblioteca cliente de Computer Vision para Python para:

* Analizar una imagen para ver las etiquetas, la descripción de texto, las caras, el contenido para adultos, etc.
* Reconocer texto manuscrito e impreso con Batch Read API.

> [!NOTE]
> Los escenarios de este inicio rápido usan direcciones URL de imágenes remotas. Para ver un código de ejemplo que realiza las mismas operaciones en imágenes locales, consulte el código en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py).

[Documentación de referencia](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-computervision) | [Paquete (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-computervision/) | [Ejemplos](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Instalación

### <a name="create-a-computer-vision-azure-resource"></a>Creación de un recurso de Computer Vision en Azure

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Computer Vision con [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services/#decision) válida durante siete días de forma gratuita Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Ver este recurso en [Azure Portal](https://portal.azure.com/).

Después de obtener una clave de la suscripción de evaluación o el recurso, [cree una variable de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la clave y la dirección URL del punto de conexión, denominadas `COMPUTER_VISION_SUBSCRIPTION_KEY` y `COMPUTER_VISION_ENDPOINT`, respectivamente.
 
### <a name="create-a-new-python-application"></a>Creación de una nueva aplicación de Python

Cree un nuevo script de Python, por ejemplo, &mdash;*quickstart-file.py*. Ábralo en el editor o el IDE que prefiera e importe las siguientes bibliotecas.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_imports)]

A continuación, cree variables para el punto de conexión y la clave de Azure del recurso.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Si ha creado la variable de entorno después de haber iniciado la aplicación, deberá cerrar y volver a abrir el editor, el IDE o el shell que lo ejecuta para acceder a la variable.

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Puede instalar la biblioteca cliente con lo siguiente:

```console
pip install --upgrade azure-cognitiveservices-Computer Vision
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales del SDK de Python para Computer Vision.

|NOMBRE|DESCRIPCIÓN|
|---|---|
|[ComputerVisionClientOperationsMixin](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.operations.computervisionclientoperationsmixin?view=azure-python)| Esta clase controla directamente todas las operaciones de imagen, como el análisis de imágenes, la detección de texto y la generación de miniaturas.|
| [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) | Esta clase es necesaria para todas las funcionalidades de Computer Vision. Cree una instancia de ella con la información de suscripción y úsela para generar instancias de otras clases. Implementa **ComputerVisionClientOperationsMixin**.|
|[VisualFeatureTypes](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python)| Esta enumeración define los diferentes tipos de análisis de imágenes que se pueden realizar en una operación de análisis estándar. Debe especificar un conjunto de valores de **VisualFeatureTypes** en función de sus necesidades. |

## <a name="code-examples"></a>Ejemplos de código

En estos fragmentos de código se muestra cómo realizar las siguientes tareas con la biblioteca cliente de Computer Vision para Python:

* [Autenticar el cliente](#authenticate-the-client)
* [Analizar una imagen](#analyze-an-image)
* [Leer texto manuscrito e impreso](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Autenticar el cliente

> [!NOTE]
> En este inicio rápido se da por supuesto que ha [creado una variable de entorno](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) para la clave de Computer Vision, denominada `COMPUTER_VISION_SUBSCRIPTION_KEY`.

Cree una instancia de un cliente con la clave y el punto de conexión. Cree un objeto [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) con la clave y úselo con el punto de conexión para crear un objeto [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_client)]

## <a name="analyze-an-image"></a>Análisis de una imagen

Guarde una referencia a la dirección URL de una imagen que desee analizar.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_remoteimage)]

### <a name="get-image-description"></a>Obtención de la descripción de la imagen

El código siguiente obtiene la lista de títulos generados para la imagen. Consulte [Descripción de imágenes](../concept-describing-images.md) para más detalles.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_describe)]

### <a name="get-image-category"></a>Obtención de la categoría de imagen

El código siguiente obtiene la categoría detectada de la imagen. Consulte [Categorización de imágenes](../concept-categorizing-images.md) para más detalles.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_categorize)]

### <a name="get-image-tags"></a>Obtención de etiquetas de imagen

El código siguiente obtiene el conjunto de las etiquetas detectadas en la imagen. Consulte [Etiquetas de contenido](../concept-tagging-images.md) para más detalles.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_tags)]

### <a name="detect-objects"></a>Detección de objetos

El código siguiente detecta objetos comunes en la imagen y los imprime en la consola. Consulte [Detección de objetos](../concept-object-detection.md) para más información.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-brands"></a>Detección de marcas

El código siguiente detecta marcas corporativas y logotipos en la imagen y los imprime en la consola. Consulte [Detección de marcas](../concept-brand-detection.md) para más información.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-faces"></a>Detección de caras

El código siguiente devuelve las caras detectadas en la imagen con sus coordenadas de rectángulo y selecciona los atributos de cara. Consulte [Detección de caras](../concept-detecting-faces.md) para más información.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_faces)]

### <a name="detect-adult-or-racy-content"></a>Detección de contenido para adultos o subido de tono

El código siguiente imprime la presencia detectada de contenido para adultos o subido de tono en la imagen. Consulte [Contenido para adultos o subido de tono](../concept-detecting-adult-content.md) para más detalles.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Obtención de la combinación de colores de imagen

El código siguiente imprime los atributos de color detectados en la imagen, como los colores dominantes y el color de énfasis. Consulte [Combinaciones de colores](../concept-detecting-color-schemes.md) para más detalles.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Obtención de contenido específico del dominio

Computer Vision puede usar un modelo especializado para realizar análisis adicionales en las imágenes. Consulte [Contenido específico del dominio](../concept-detecting-domain-content.md) para más detalles. 

En el código siguiente se analizan los datos sobre las celebridades detectadas en la imagen.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_celebs)]

En el código siguiente se analizan los datos sobre los paisajes detectados en la imagen.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Obtención del tipo de imagen

El código siguiente imprime información sobre el tipo de imagen (si es una imagen prediseñada o dibujo lineal).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Lectura de texto manuscrito e impreso

Computer Vision puede leer texto visible de una imagen y convertirlo en un flujo de caracteres. Esto se hace en dos partes.

### <a name="call-the-read-api"></a>Llamada a la API Read

En primer lugar, use el siguiente código para llamar al método **batch_read_file** para la imagen especificada. Esto devuelve un identificador de operación e inicia un proceso asincrónico para leer el contenido de la imagen.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_call)]

### <a name="get-read-results"></a>Obtención de resultados de lectura

A continuación, obtenga el identificador de operación devuelto de la llamada a **batch_read_file** y úselo para consultar los resultados de la operación en el servicio. El código siguiente comprueba la operación a intervalos de un segundo hasta que se devuelven los resultados. A continuación, imprime los datos de texto extraídos en la consola.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_response)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `python` en el archivo de inicio rápido.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)


## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a usar la biblioteca de Computer Vision para Python para realizar tareas básicas. A continuación, consulte la documentación de referencia para más información sobre la biblioteca.


> [!div class="nextstepaction"]
>[Referencia de la API Computer Vision (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)

* [¿Qué es la API Computer Vision?](../Home.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py).