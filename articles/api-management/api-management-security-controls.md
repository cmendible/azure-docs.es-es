---
title: Controles de seguridad de Azure API Management
description: Lista de comprobación de los controles de seguridad que se utilizan para evaluar API Management
services: api-management
author: msmbaldwin
manager: rkarlin
ms.service: api-management
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: e808f373ed3c977fb3263bc9e2e25bc602c7a7e1
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886293"
---
# <a name="security-controls-for-api-management"></a>Controles de seguridad de API Management

En este artículo, se explican los controles de seguridad integrados en API Management.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Red

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Compatibilidad con puntos de conexión de servicio| Sin | |
| Compatibilidad con la inserción de redes virtuales| Sí | |
| Compatibilidad con el aislamiento de red y los firewalls| Sí | Se usan grupos de seguridad de red (NSG) y Azure Application Gateway (u otro dispositivo de software), respectivamente. |
| Compatibilidad con la tunelización forzada| Sí | Las redes de Azure proporcionan la tunelización forzada. |

## <a name="monitoring--logging"></a>Supervisión y registro

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la supervisión de Azure (Log Analytics, Application Insights, etc.)| Sí | |
| Registro y auditoría del plano de administración y de control| Sí | [Registros de actividad en Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Registro y auditoría del plano de datos| Sí | [Registros de diagnóstico de Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) y (opcionalmente) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="identity"></a>Identidad

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Authentication| Sí | |
| Authorization| Sí | |

## <a name="data-protection"></a>Protección de datos

| Control de seguridad | Sí/No | Notas |
|---|---|--|
| Cifrado del lado servidor en reposo: Claves administradas por Microsoft | Sí | Los datos confidenciales, como certificados, claves y valores con nombre secreto, se cifran con claves que administra el servicio por instancia de servicio. |
| Cifrado del lado servidor en reposo: claves administradas por el cliente (BYOK) | Sin | Todas las claves de cifrado son por instancia de servicio y las administra el servicio. |
| Cifrado de nivel de columna (Azure Data Services)| N/D | |
| Cifrado en tránsito (por ejemplo, cifrado de ExpressRoute, cifrado en la red virtual y cifrado de red virtual a red virtual)| Sí | El cifrado de [ExpressRoute](../expressroute/index.yml) y de red virtual lo proporcionan las [redes de Azure](../virtual-network/index.yml). |
| Llamadas a API cifradas| Sí | Las llamadas del plano de administración se hacen mediante [Azure Resource Manager](../azure-resource-manager/index.yml) con TLS. Se requiere un token web de JSON (JWT) válido.  Las llamadas del plano de datos pueden estar protegidas con TLS y uno de los mecanismos de autenticación admitidos (por ejemplo, el certificado de cliente o JWT).
 |

## <a name="configuration-management"></a>Administración de configuración

| Control de seguridad | Sí/No | Notas|
|---|---|--|
| Compatibilidad con la administración de configuración (control de versiones de configuración, etc.)| Sí | Se usa el [kit de recursos de DevOps de Azure API Management](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>Exámenes de vulnerabilidades de falsos positivos

Esta sección documenta las vulnerabilidades más comunes, que no afectan a Azure API Management.

| Punto vulnerable               | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed es una vulnerabilidad en la implementación de la extensión TLS SessionTicket que se encuentra en algunos productos de F5. Permite la pérdida ("hemorragia") de hasta 31 bytes de datos de la memoria sin inicializar. Esto se debe a que la pila de TLS rellena con datos un identificador de sesión, que pasa el cliente, para que tenga una longitud de 32 bits. |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [controles de seguridad integrados en los servicios de Azure](../security/fundamentals/security-controls.md).