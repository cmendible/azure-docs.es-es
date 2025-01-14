---
title: Cumplimentación del formulario de configuración de oferta | Azure Marketplace
description: Explica los distintos campos que requieren valores en el formulario de configuración de la oferta para una nueva aplicación de Dynamics 365 Business Central.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pabutler
ms.openlocfilehash: d29b17e1a109b37a51a0e6bd2af2a7bb02b977a9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64934905"
---
<a name="how-to-fill-out-the-offer-settings-form"></a>Cumplimentación del formulario de configuración de oferta
=======================================

El formulario de configuración de oferta es un formulario básico en el que se especifica la configuración de la oferta.
Los campos obligatorios se describen a continuación.

### <a name="offer-id"></a>Id. de oferta

`OfferId` es un identificador único de la oferta en un perfil del anunciante.
Este identificador será visible en las direcciones URL de producto. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. Este campo queda bloqueado en cuanto se lanza una oferta.

Si, por ejemplo, el asociado "Contoso" crea un identificador de oferta denominado "sample-Web App", aparecerá en AppSource como:

&emsp;`https://appsource.microsoft.com/marketplace/apps/contoso.sample-Web App?tab=Overview`


### <a name="publisher-id"></a>Id. de publicador

Esta lista desplegable permite elegir el perfil del publicador en el que desea publicar esta oferta. Este campo queda bloqueado en cuanto se lanza una oferta.


### <a name="name"></a>NOMBRE

Este es el nombre para mostrar de la aplicación u oferta que aparece en Microsoft [AppSource](https://appsource.microsoft.com/). Puede tener un máximo de 50 caracteres.

> [!NOTE]
> El nombre corto debe ser el mismo que el nombre del anunciante especificado en el manifiesto de aplicación.

Haga clic en **Guardar** para guardar su progreso. El paso siguiente sería agregar información técnica para la oferta.
