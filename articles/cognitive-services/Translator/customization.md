---
title: Personalización de la traducción con Translator Text API
titleSuffix: Azure Cognitive Services
description: Use Microsoft Translator Hub para crear su propio sistema de traducción automática con su terminología y estilo preferidos.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: e6771a0d72592f1952853642ceea196b02774ada
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595310"
---
# <a name="customize-your-text-translations"></a>Personalización de las traducciones de texto

Traductor personalizado de Microsoft es una característica del servicio Microsoft Translator que permite a los usuarios personalizar la traducción automática neuronal avanzada de Microsoft Translator cuando se traduce texto con Translator Text API (solo en la versión 3).

Esta característica solamente puede utilizarse para personalizar la traducción de voz cuando se usa con el [servicio Voz de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Custom Translator

Con Custom Translator puede crear sistemas de traducción neuronal que comprendan la terminología usada en su propia empresa y sector. El sistema de traducción personalizada se integrará en las aplicaciones, los flujos de trabajo y los sitios web existentes.

### <a name="how-does-it-work"></a>¿Cómo funciona?

Utilice los documentos traducidos anteriormente (prospectos, páginas web, documentación, etc.) para crear un sistema de traducción que refleje la terminología y el estilo específicos de su dominio, mejor que un sistema de traducción genérico. Los usuarios pueden cargar documentos TMX, XLIFF, TXT, DOCX y XLSX.  

El sistema también acepta datos que sean paralelos a nivel de documento, pero que aún no estén alineados a nivel de frase. Si los usuarios tienen acceso a versiones del mismo contenido en varios idiomas, pero en documentos independientes Custom Translator podrá hace concordar automáticamente las frases de los distintos documentos.  El sistema también puede utilizar datos monolingües en uno de los idiomas, o en ambos, para complementar los datos de aprendizaje paralelos, con el fin de mejorar las traducciones.

A partir de ese momento, el sistema personalizado está disponible con una llamada normal a Microsoft Translator Text API con el parámetro de categoría.

Si se proporcionan el tipo y la cantidad apropiados de datos de aprendizaje, no es extraño que con Custom Translator la calidad de la traducción mejore entre 5 y 10 puntos BLEU, o incluso más.

Puede encontrar más detalles acerca de los diferentes niveles de personalización en función de los datos disponibles en el [Manual del usuario de Custom Translator](https://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Translator Hub

> [!NOTE]
> Microsoft Translator Hub (heredado) se retirará el 17 de mayo de 2019. [Ver información importante y fechas de migración](https://www.microsoft.com/translator/business/hub/).  

## <a name="custom-translator-versus-hub"></a>Custom Translator frente a Hub

|   | **Hub** | **Custom Translator**|
|:-----|:----:|:----:|
|Estado de la característica de personalización   | Disponibilidad general  | Disponibilidad general |
| Versión de Text API  | Solo v2   | Solo v3 |
| Personalización de SMT | Sí   | Sin |
| Personalización de NMT | Sin    | Sí |
| Nueva personalización unificada de servicios de voz | Sin    | Sí |
| [Sin seguimiento](https://www.aka.ms/notrace) | Sí  | Sí |

## <a name="collaborative-translations-framework"></a>Marco de traducciones en colaboración

> [!NOTE]
> A partir del 1 de febrero de 2018, AddTranslation() y AddTranslationArray() no se pueden usar con la versión 2.0 de Translator Text API. Estos métodos generarán un error y no se escribirá nada. La versión 3.0 de Translator Text API no admite estos métodos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de un sistema de idiomas personalizado mediante Custom Translator](https://aka.ms/CustomTranslatorDocs)
