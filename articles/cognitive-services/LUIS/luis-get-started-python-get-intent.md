---
title: 'Inicio rápido: Obtención de intenciones con Python: LUIS'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, pasará expresiones a un punto de conexión de LUIS y obtendrá entidades e intenciones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: 0d36c7aee77cd9ba8136a988ac98d226d7f4dc5f
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70375476"
---
# <a name="quickstart-get-intent-using-python"></a>Guía de inicio rápido: Obtención de intención con Python
En esta guía de inicio rápido, pasará expresiones a un punto de conexión de LUIS y obtendrá entidades e intenciones.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obtención de la intención con el explorador

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>Obtención de la intención mediante programación

Puede usar Python para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior.

1. Copie uno de los siguientes fragmentos de código en un archivo denominado `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Sustituya el valor del campo `Ocp-Apim-Subscription-Key` por su clave de punto de conexión de LUIS.

3. Instale las dependencias con `pip install requests`.

4. Ejecute el script con `python ./quickstart-call-endpoint.py`. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos
Elimine el archivo de python. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-python-add-utterance.md)
