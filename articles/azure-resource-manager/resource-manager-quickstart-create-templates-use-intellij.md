---
title: Creación e implementación de una plantilla de Azure Resource Manager mediante IntelliJ IDEA| Microsoft Docs
description: Aprenda cómo crear su primera plantilla de Azure Resource Manager mediante IntelliJ IDEA y cómo implementarla.
services: azure-resource-manager
documentationcenter: ''
author: yucwan
manager: ''
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: java
ms.date: 08/01/2019
ms.topic: quickstart
ms.author: yucwan
ms.openlocfilehash: cbeaccf4cdea87d6f34d5ee77e6a08b32abd76b5
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708198"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-intellij-idea"></a>Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante IntelliJ IDEA

Aprenda a implementar una plantilla de Resource Manager en Azure con IntelliJ IDEA y el proceso de edición y actualización de la plantilla directamente desde el entorno de desarrollo integrado. Las plantillas de Resource Manager son archivos JSON que definen los recursos que necesita para implementar la solución. Para entender los conceptos asociados a la implementación y administración de sus soluciones de Azure, consulte [Introducción a Azure Resource Manager](resource-group-overview.md).

![Diagrama de inicio rápido de la plantilla de Resource Manager desde el portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-export-deploy-template-portal.png)

Tras completar el tutorial implemente una cuenta de Azure Storage. El mismo proceso se puede usar para implementar otros recursos de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition o Community Edition instalado
* [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) instalado; consulte la [guía de administración de complementos de IntelliJ](https://www.jetbrains.com/help/idea/managing-plugins.html) para más información.
* Haber [iniciado sesion](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) en su cuenta de Azure para Azure Toolkit for IntelliJ.

## <a name="deploy-a-quickstart-template"></a>Implementación de una plantilla de inicio rápido

En lugar de crear una plantilla desde cero, abra una plantilla en las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/). Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. La plantilla usada en esta guía de inicio rápido se denomina [Crear una cuenta de almacenamiento estándar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-storage-account-create/). Define un recurso de la cuenta de almacenamiento de Azure. 

1. Haga clic con el botón derecho y guarde [`azuredeploy.json`](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) y [`azuredeploy.parameters.json`](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json) en el equipo local.

1. Si el kit de herramientas de Azure está instalado y se ha iniciado sesión correctamente, debería ver el Explorador de Azure en la barra lateral de IntelliJ IDEA. Haga clic con el botón derecho en **Administración de recursos** y seleccione **Crear implementación**.

    ![Plantilla de Resource Manager: clic con el botón derecho para crear la implementación](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-create-deployment-right-click.png)

1. Configure el **Nombre de implementación**, la **Suscripción**, el **Grupo de recursos** y la **Región**. Aquí implementamos la plantilla en un nuevo grupo de recursos, `testRG`. A continuación, seleccione la ruta de acceso de **Resource Template** (Plantilla del recurso) como `azuredeploy.json` y los **Parámetros del recurso** como el archivo `azuredeploy.parameters.json` que descargó.

    ![Plantilla de Resource Manager: selección de los archivos para crear la implementación](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-create-deployment-select-files.png)

1. Al hacer clic en Aceptar, se inicia la implementación. Hasta que la implementación se complete, encontrará el progreso en la **barra de estado** de IntelliJ IDEA en la parte inferior.

    ![Plantilla de Resource Manager: estado de la implementación](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-create-deployment-status.png)

## <a name="browse-an-existing-deployment"></a>Examinar una implementación existente

1. Una vez finalizada la implementación, puede ver el nuevo grupo de recursos, `testRG`, y una nueva implementación creada. Haga clic con el botón derecho en la implementación para ver una lista de acciones posibles. Seleccione **Mostrar propiedades**.

    ![Plantilla de Resource Manager: examinar implementación](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-deployment-browse.png)

1. Se abrirá una vista de pestaña para mostrar algunas propiedades útiles, como el estado de la implementación y la estructura de la plantilla.

    ![Plantilla de Resource Manager: mostrar propiedades de implementación](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-deployment-show-properties.png)

## <a name="edit-and-update-an-existing-deployment"></a>Edición y actualización de una implementación existente

1. Seleccione **Edit Deployment** (Editar implementación) en el menú que aparece al hacer clic con el botón derecho o en la vista Mostrar propiedades anterior. Se abrirá otra vista de pestaña que muestra los archivos de la plantilla y de parámetros para la implementación en Azure. Para guardar esos archivos en el entorno local, puede hacer clic en **Export Template File** (Exportar archivo de plantilla) o **Export Parameter Files** (Exportar archivos de parámetros).

    ![Implementación de la edición de la plantilla de Resource Manager](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-edit-deployment.png)

1. Puede editar los dos archivos de esta página e implementar los cambios en Azure. Aquí se modifica el valor de **storageAccountType** en los archivos de parámetros, de `Standard_LRS` a `Standard_GRS`. A continuación, haga clic en **Implementación de actualizaciones** en la parte inferior y confirme la actualización.

    ![Implementación de la edición de la plantilla de Resource Manager](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-edit-deployment-update.png)

1. Una vez finalizada la implementación de la actualización, puede comprobar en el portal que la cuenta de almacenamiento creada ha cambiado a `Standard_GRS`.

## <a name="clean-up-resources"></a>Limpieza de recursos

1. Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos. Puede hacerlo desde Azure Portal o mediante la CLI de Azure. En el explorador de Azure de IntelliJ IDEA, haga clic con el botón derecho en el **grupo de recursos** creado y seleccione Eliminar.

    ![Eliminación del grupo de recursos del explorador de Azure desde IntelliJ IDEA](./media/resource-manager-quickstart-create-templates-use-intellij/delete-resource-group.png)

> [!NOTE]
> Tenga en cuenta que la eliminación de una implementación no elimina los recursos creados por la implementación. Elimine el grupo de recursos o los recursos correspondientes si ya no los necesita.

## <a name="next-steps"></a>Pasos siguientes

El objetivo principal de este inicio rápido consiste en utilizar IntelliJ IDEA para implementar una plantilla existente de las plantillas de inicio rápido de Azure. También ha aprendido a ver y actualizar una implementación existente en Azure. Es posible que las plantillas de inicio rápido de Azure no le proporcionen todo lo que necesita. En el siguiente tutorial se indica cómo encontrar la información en la referencia sobre plantillas para que pueda crear una cuenta de Azure Storage cifrada.

> [!div class="nextstepaction"]
> [Creación de una cuenta de almacenamiento cifrada](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

> [!div class="nextstepaction"]
> [Visite Java en el centro de desarrollo de Azure](https://docs.microsoft.com/azure/java)