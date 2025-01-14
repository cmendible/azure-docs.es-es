---
title: Creación de una asignación de directiva con una plantilla de Resource Manager
description: Este artículo le guiará por los pasos para usar una plantilla de Resource Manager para crear una asignación de directiva para identificar recursos no compatibles.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/13/2019
ms.topic: quickstart
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: f31d6197c22be4d66e0610ad7914f541a45ed995
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979569"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-by-using-a-resource-manager-template"></a>Inicio rápido: Creación de una asignación de directiva para identificar recursos no compatibles mediante una plantilla de Resource Manager

El primer paso para entender el cumplimiento en Azure es identificar el estado de sus recursos.
Esta guía de inicio rápido lo guiará por el proceso de creación de una asignación de directiva para identificar las máquinas virtuales que no están usando discos administrados.

Al finalizar este proceso, habrá identificado correctamente máquinas virtuales que no utilizan discos administrados. *No son compatibles* con la asignación de directiva.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-a-policy-assignment"></a>Creación de una asignación de directiva

En este inicio rápido, creará una asignación de directiva y asignará una definición de directiva integrada denominada *Auditoría de máquinas virtuales que no usan discos administrados*. Para una lista parcial de las directivas integradas disponibles, consulte los [ejemplos de Azure Policy](./samples/index.md).

Existen varios métodos de creación de asignaciones de directivas. En este inicio rápido se usa una [plantilla de inicio rápido](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/).
Esta es una copia de la plantilla:

[!code-json[policy-assignment](~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json)]

> [!NOTE]
> El servicio Azure Policy es gratuito.  Para más información, consulte la [Introducción a Azure Policy](./overview.md).

1. Seleccione la siguiente imagen para iniciar sesión en Azure Portal y abrir la plantilla:

   [![Implementación de la plantilla en Azure](./media/assign-policy-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json)

1. Seleccione o escriba los siguientes valores:

   | NOMBRE | Valor |
   |------|-------|
   | Subscription | Seleccione su suscripción a Azure. |
   | Grupos de recursos | Seleccione **Crear**, especifique un nombre y seleccione **Aceptar**. En la captura de pantalla, el nombre del grupo de recursos es *mypolicyquickstart\<Date in MMDD>rg*. |
   | Ubicación | Seleccione una región. Por ejemplo, **Centro de EE. UU**. |
   | Nombre de la asignación de directiva | Especifique un nombre para la asignación de directiva. Si lo desea, puede usar la definición de directiva en pantalla. Por ejemplo, **Auditoría de máquinas virtuales que no usan discos administrados**. |
   | Nombre del grupo de recursos | Especifique un nombre para el grupo de recursos donde desea asignar la directiva. En este inicio rápido se usa el valor predeterminado **[resourceGroup().name]** . **[resourceGroup()](../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)** es una función de plantilla que recupera el grupo de recursos. |
   | ID de definición de directiva | Especifique **/providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4c19-b460-a2d36003525a**. |
   | Acepto los términos y condiciones indicados anteriormente | (Seleccionar) |

1. Seleccione **Comprar**.

Algunos recursos adicionales:

- Para buscar más plantillas de ejemplo, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- Para ver la referencia de plantilla, vaya a la [referencia de plantilla de Azure](/azure/templates/microsoft.authorization/allversions).
- Para aprender a desarrollar plantillas de Resource Manager, consulte la [documentación de Azure Resource Manager](/azure/azure-resource-manager/).
- Para información sobre la implementación de nivel de suscripción, consulte [Create resource groups and resources at the subscription level](../../azure-resource-manager/deploy-to-subscription.md) (Creación de grupos de recursos y recursos en el nivel de suscripción).

## <a name="identify-non-compliant-resources"></a>Identificación de recursos no compatibles

Seleccione **Cumplimiento** en el panel izquierdo de la página. A continuación, busque la asignación de directiva **Auditoría de máquinas virtuales que no usan discos administrados** que ha creado.

![Página de información general del cumplimiento de directivas](./media/assign-policy-template/policy-compliance.png)

Si hay algún recurso existente no compatible con esta nueva asignación, aparecerá en la pestaña **Recursos no compatibles**.

Para más información, consulte [How compliance works](./how-to/get-compliance-data.md#how-compliance-works) (Funcionamiento del cumplimiento).

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar la asignación creada, siga estos pasos:

1. Seleccione **Cumplimiento** (o **Asignaciones**) en el lado izquierdo de página Azure Policy y busque la asignación de directiva **Auditoría de máquinas virtuales que no usan discos administrados** asignación de directiva que ha creado.

1. Haga clic con el botón derecho en la asignación de directiva **Auditoría de máquinas virtuales que no usan discos administrados** y seleccione **Eliminar asignación**.

   ![Eliminación de una asignación de la página de información general de cumplimiento](./media/assign-policy-template/delete-assignment.png)

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido se asigna una definición de directiva integrada a un ámbito y se evalúa su informe de cumplimiento. La definición de la directiva confirma que todos los recursos del ámbito son compatibles y se identifican cuáles no lo son.

Para más información sobre la asignación de directivas para garantizar la compatibilidad de los nuevos recursos, continúe con el tutorial para:

> [!div class="nextstepaction"]
> [Crear y administrar directivas](./tutorials/create-and-manage.md)