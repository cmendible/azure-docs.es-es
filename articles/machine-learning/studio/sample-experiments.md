---
title: Puesta en marcha de los experimentos a partir de ejemplos
titleSuffix: Azure Machine Learning Studio
description: Aprenda a usar experimentos de aprendizaje automático de ejemplo para crear nuevos experimentos con Azure AI Gallery y Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/05/2018
ms.openlocfilehash: f88323069ed23f4a038ffa4a030b1c4d4541ec42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61460384"
---
# <a name="create-azure-machine-learning-studio-experiments-from-working-examples-in-azure-ai-gallery"></a>Crear experimentos de Azure Machine Learning Studio a partir de ejemplos de trabajo en Azure AI Gallery

Aprenda cómo comenzar con experimentos de ejemplo desde la [galería de inteligencia artificial de Azure](https://gallery.azure.ai/) en lugar de crear experimentos de aprendizaje automático desde cero. Puede usar los ejemplos para crear su propia solución de aprendizaje automático.

En la galería hay experimentos de ejemplo creados por el equipo Microsoft Azure Machine Learning Studio, así como ejemplos compartidos por la comunidad de Machine Learning. También puede plantear preguntas o publicar comentarios acerca de experimentos.

Para ver cómo usar la galería, vea el vídeo de 3 minutos [Copia del trabajo de otras personas para realizar ciencia de datos](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) de la serie [Ciencia de datos para principiantes](data-science-for-beginners-the-5-questions-data-science-answers.md).



## <a name="find-an-experiment-to-copy-in-azure-ai-gallery"></a>Busque un experimento para copiarlo en la galería de inteligencia artificial de Azure
Para ver qué experimentos hay disponibles, vaya a la [galería](https://gallery.azure.ai/) y haga clic en **Experiments** (Experimentos) en la parte superior de la página.

### <a name="find-the-newest-or-most-popular-experiments"></a>Busque los experimentos más recientes o más populares
En esta página puede ver experimentos **agregados recientemente** (Recently added), desplazarse hacia abajo para averiguar **qué es popular** (What's popular) o consultar los **experimentos populares de Microsoft** (Popular Microsoft experiments) más recientes.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Busque un experimento que cumpla los requisitos específicos
Para examinar todos los experimentos:

1. Haga clic en **Browse all** (Examinar todo) en la parte superior de la página.
2. En el lado izquierdo, en **Refine by** (Refinar por) de la sección **Categories** (Categorías), seleccione **Experiment** (Experimento) para ver los experimentos de la galería.
3. Puede encontrar los experimentos que cumplan con sus requisitos de las siguientes maneras:
   * **Seleccione los filtros de la izquierda.** Por ejemplo, para examinar experimentos que usan un algoritmo de detección de anomalías basado en PCA: en **Categories** (Categorías), haga clic en **Experiment** (Experimento). A continuación, en **Algorithms Used** (Algoritmos utilizados), haga clic en **Show all** (Mostrar todos) y en el cuadro de diálogo, elija **PCA-Based Anomaly Detection** (Detección de anomalías basada en PCA). Es posible que tenga que desplazarse para verlo.<br></br>
     ![Selección de filtros](./media/sample-experiments/choose-an-algorithm.png)
   * **Utilice el cuadro de búsqueda.** Por ejemplo, para buscar experimentos aportados por Microsoft relacionados con el reconocimiento de dígitos que usen un algoritmo de máquina de vectores de soporte de dos clases, escriba "digit recognition" (reconocimiento de dígitos) en el cuadro de búsqueda. A continuación, seleccione los filtros **Experiment** (Experimento), **Microsoft content only** (Solo contenido de Microsoft) y **Two-Class Support Vector Machine** (Máquina de vectores de soporte de dos clases):<br></br>
     ![Uso del cuadro de búsqueda](./media/sample-experiments/search-for-experiments.png)
4. Haga clic en un experimento para obtener más información.
5. Para ejecutar o modificar el experimento, haga clic en **Open in Studio** (Abrir en Estudio) en la página del experimento. <br></br>

    ![Experimento de ejemplo](./media/sample-experiments/example-experiment.png)

    > [!NOTE]
    > Cuando abra un experimento en Machine Learning Studio por primera vez, puede probarlo de forma gratuita o comprar una suscripción a Azure. [Vea información sobre la diferencia entre la prueba gratuita de Machine Learning Studio y el servicio de pago](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Creación de un nuevo experimento usando un ejemplo como plantilla
También puede crear un nuevo experimento en Machine Learning Studio tomando un ejemplo de la galería como plantilla.

1. Inicie sesión con las credenciales de su cuenta de Microsoft en [Studio](https://studio.azureml.net) y haga clic en **Nuevo** para crear un experimento.
2. Examine el contenido de ejemplo y haga clic en uno.

Se crea un nuevo experimento en su área de trabajo de Machine Learning Studio tomando como plantilla el experimento de ejemplo.

## <a name="next-steps"></a>Pasos siguientes
* [Importación de datos desde varios orígenes](import-data.md)
* [Tutorial rápido para el lenguaje R en Machine Learning](r-quickstart.md)
* [Implementación de un servicio web Machine Learning](publish-a-machine-learning-web-service.md)
