---
title: Administración de alertas de seguridad en Azure Security Center | Microsoft Docs
description: Este documento le ayuda a usar las capacidades del Centro de seguridad de Azure para administrar y responder a las alertas de seguridad.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/27/2019
ms.author: v-mohabe
ms.openlocfilehash: a31b6006ff3345947fc0c64b9427d79e3870a2b6
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70052076"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Administración y respuesta a alertas de seguridad en Azure Security Center

En este tema se muestra cómo ver y procesar las alertas que ha recibido con el fin de proteger los recursos. 

* Para obtener información sobre los distintos tipos de alertas, vea [Tipos de alertas de seguridad](security-center-alerts-overview.md#security-alert-types).
* Para obtener información general sobre la forma en que Security Center genera alertas, vea [¿Cómo detecta Security Center las amenazas?](security-center-alerts-overview.md#detect-threats)

> [!NOTE]
> Para habilitar las detecciones avanzadas, actualice a la versión estándar de Azure Security Center. Hay una evaluación gratuita disponible. Para realizar la actualización, seleccione el plan de tarifa en la [directiva de seguridad](tutorial-security-policy.md). Consulte [Precios de Azure Security Center](security-center-pricing.md) para más información.

## <a name="what-are-security-alerts"></a>¿Qué son las alertas de seguridad?
Security Center recopila, analiza e integra automáticamente los datos de registro de los recursos de Azure, la red y las soluciones de asociados conectados, como firewalls y soluciones de protección de puntos de conexión, para detectar amenazas reales y reducir los falsos positivos. En Security Center, se muestra una lista de alertas de seguridad prioritarias, junto con la información que necesita para investigar rápidamente y recomendaciones para corregir un ataque.

> [!NOTE]
> Para obtener más información sobre la forma en que actúan las funcionalidades de detección de Security Center, vea [¿Cómo detecta Security Center las amenazas?](security-center-alerts-overview.md#detect-threats)

## <a name="manage-your-security-alerts"></a>Administración de las alertas de seguridad

1. En el panel de Security Center, consulte el icono **Protección contra amenazas** para ver y obtener información general de las alertas.

    ![Icono Alertas de seguridad en Security Center](./media/security-center-managing-and-responding-alerts/security-center-dashboard-alert.png)

1. Para ver más detalles sobre las alertas, haga clic en el icono.

   ![Las alertas de seguridad en Security Center](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Para filtrar las alertas que se muestran, haga clic en **Filtro** y, en la hoja **Filtro** que se abre, seleccione las opciones de filtro que quiere aplicar. La lista se actualiza según el filtro seleccionado. El filtrado puede resultar muy útil. Por ejemplo, podría comprobar las alertas de seguridad que se produjeron en las 24 horas anteriores, ya que se está investigando una posible brecha en el sistema.

    ![Filtrado de alertas en Security Center](./media/security-center-managing-and-responding-alerts/security-center-filter-alerts.png)

## <a name="respond-to-security-alerts"></a>Responder a alertas de seguridad

1. En la lista **Alertas de seguridad**, haga clic en una alerta de seguridad. Se muestran los recursos implicados y los pasos que debe seguir para solucionar las consecuencias de un ataque.

    ![Responder a alertas de seguridad](./media/security-center-managing-and-responding-alerts/security-center-alert.png)

1. Después de revisar esta información, haga clic en un recurso atacado.

    ![Sugerencias sobre qué hacer con las alertas de seguridad](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    La sección **Información general** puede ofrecer una visión general de lo que desencadenó la alerta de seguridad. Muestra información como el recurso de destino o la dirección IP de origen (si es aplicable) si la alerta sigue activa, así como recomendaciones sobre cómo solucionarla.  

    > [!NOTE]
    >En algunos casos, la dirección IP de origen no está disponible, ya que algunos registros de eventos de seguridad de Windows no incluyen la dirección IP.

1. Los pasos de corrección sugeridos por Security Center varían según la alerta de seguridad. Sígalos para cada una de las alertas. En algunos casos, para mitigar una alerta de detección de amenazas, puede que tenga que usar otros servicios o controles de Azure para implementar la corrección recomendada. 

    Los temas siguientes le guiarán a través de las diferentes alertas según los tipos de recursos:
    
    * [Alertas de servidores y máquinas virtuales de IaaS](security-center-alerts-iaas.md)
    * [Alertas de procesos nativos](security-center-alerts-compute.md)
    * [Alertas de servicios de datos](security-center-alerts-data-services.md)
    
    Los temas siguientes explican cómo Security Center usa los diferentes datos de telemetría que recopila de la integración con la infraestructura de Azure a fin de aplicar capas de protección adicionales para los recursos implementados en Azure:
    
    * [Alertas de la capa de servicios](security-center-alerts-service-layer.md)
    * [Integración con productos de seguridad de Azure](security-center-alerts-integration.md)
    
## <a name="see-also"></a>Otras referencias

En este documento ha aprendido a configurar directivas de seguridad en el Centro de seguridad. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Alertas de seguridad en Azure Security Center](security-center-alerts-overview.md)
* [Control de incidentes de seguridad en Azure Security Center](security-center-incident.md)
* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md)
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/) : encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.
