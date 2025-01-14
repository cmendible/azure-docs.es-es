---
title: Seguimiento distribuido en Azure Application Insights | Microsoft Docs
description: Proporciona información acerca de la compatibilidad de Microsoft con el seguimiento distribuido mediante el reenviador local y la asociación en el proyecto OpenCensus
services: application-insights
keywords: ''
author: nikmd23
ms.author: nimolnar
ms.reviewer: mbullwin
ms.date: 09/17/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7bc04748f2a5b8caa8f589140dd46f0650b7b390
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60898855"
---
# <a name="what-is-distributed-tracing"></a>¿Qué es el seguimiento distribuido?

La llegada de las modernas arquitecturas de [microservicios](https://azure.com/microservices) y en la nube ha dado lugar a servicios simples que se pueden implementar de forma independiente y que pueden ayudar a reducir los costos al tiempo que aumentan la disponibilidad y el rendimiento. Pero aunque estos movimientos han hecho más fáciles de entender los servicios individuales como un todo, se han creado sistemas que en general son más difíciles de analizar y depurar.

En las arquitecturas monolíticas, nos hemos acostumbrado a depurar con pilas de llamadas. Las pilas de llamadas son excelentes herramientas para mostrar el flujo de ejecución (el método A llama al método B y este al método C), junto con detalles y parámetros sobre cada una de esas llamadas. Esto es fantástico para estructuras monolíticas o servicios que se ejecutan en un único proceso, pero ¿cómo se depura cuando la llamada se produce fuera de los límites de un proceso y no es simplemente una referencia en la pila local? 

Aquí entra en juego el seguimiento distribuido.  

El seguimiento distribuido es el equivalente de las pilas de llamadas para arquitecturas de microservicios y en la nube modernas, junto con un generador de perfiles de rendimiento simple. En Azure Monitor, se proporcionan dos experiencias para consumir datos de seguimiento distribuido. La primera es la vista de [diagnósticos de transacción](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics), que es como una pila de llamadas con una dimensión de tiempo adicional. La vista de diagnósticos de transacción proporciona visibilidad sobre una sola transacción o solicitud y resulta útil para encontrar la causa raíz de problemas de confiabilidad y cuellos de botella de rendimiento por cada solicitud.

Azure Monitor ofrece también una vista de [mapa de aplicación](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) que agrega muchas transacciones para mostrar una vista topológica de cómo interactúan los sistemas y cuáles son las tasas promedio de rendimiento y de errores. 

## <a name="how-to-enable-distributed-tracing"></a>Cómo habilitar el seguimiento distribuido

Habilitar el seguimiento distribuido de los servicios de una aplicación es tan simple como agregar el SDK o la biblioteca adecuada a cada servicio, en función del lenguaje en el que se implementó el servicio.

## <a name="enabling-via-application-insights-sdks"></a>Habilitación mediante los SDK de Application Insights

Los SDK de Application Insights para .NET, .NET Core, Java, Node.js y JavaScript admiten el seguimiento distribuido de forma nativa. Las instrucciones para instalar y configurar cada SDK de Application Insights están disponibles a continuación:

* [.NET](https://docs.microsoft.com/azure/application-insights/quick-monitor-portal)
* [.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-dotnetcore-quick-start)
* [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-get-started)
* [Node.js](https://docs.microsoft.com/azure/application-insights/app-insights-nodejs-quick-start)
* [JavaScript](https://docs.microsoft.com/azure/application-insights/app-insights-javascript)

Con el SDK de Application Insights adecuado instalado y configurado, la información de seguimiento se recopila automáticamente para los marcos de trabajo, bibliotecas y tecnologías más populares mediante los auto-recopiladores de dependencias del SDK. La lista completa de las tecnologías compatibles está disponible en [la documentación de la recopilación automática de dependencias](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies).

 Además, se puede realizar el seguimiento manual de cualquier tecnología con una llamada a [TrackDependency](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) en [TelemetryClient](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics).

## <a name="enable-via-opencensus"></a>Habilitación mediante OpenCensus

Además de los SDK de Application Insights, Application Insights también admite el seguimiento distribuido mediante [OpenCensus](https://opencensus.io/). OpenCensus es una distribución de bibliotecas de código abierto e independiente del proveedor que proporciona recopilación de métricas y seguimiento distribuido para los servicios. También permite a la comunidad de código abierto habilitar el seguimiento distribuido con tecnologías conocidas como Redis, Memcached o MongoDB. [Microsoft colabora en OpenCensus junto con varios otros asociados de supervisión y en la nube](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/).

Para agregar funcionalidades de seguimiento distribuido a una aplicación con OpenCensus, primero debe [instalar y configurar el reenviador local de Application Insights](./../../azure-monitor/app/opencensus-local-forwarder.md). Desde allí, puede configurar OpenCensus para que enrute los datos de seguimiento distribuido mediante el reenviador local. Se admiten [Python](./../../azure-monitor/app/opencensus-python.md) y [Go](./../../azure-monitor/app/opencensus-go.md).

El sitio web de OpenCensus mantiene la documentación de referencia de la API para [Python](https://opencensus.io/api/python/trace/usage.html) y [Go](https://godoc.org/go.opencensus.io), así como diversas guías para el uso de OpenCensus. 

## <a name="next-steps"></a>Pasos siguientes

* [Guía de uso de Python de OpenCensus](https://opencensus.io/api/python/trace/usage.html)
* [Mapa de aplicación](./../../azure-monitor/app/app-map.md)
* [Supervisión del rendimiento de un extremo a otro](./../../azure-monitor/learn/tutorial-performance.md)
