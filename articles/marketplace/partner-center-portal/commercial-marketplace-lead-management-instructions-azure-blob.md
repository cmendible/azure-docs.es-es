---
title: Tabla de Azure en el programa Marketplace comercial | Azure Marketplace
description: Configuración de la administración de clientes potenciales para blob de Azure
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.topic: conceptual
ms.date: 7/30/2019
ms.author: evansma
ms.openlocfilehash: c67855422808f5ec4c81242edcece4e447f2f44f
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69901456"
---
# <a name="lead-management-instructions-for-azure-blob"></a>Instrucciones de administración de clientes potenciales para blob de Azure

>[!Caution]
>La opción Blob de Azure para procesar clientes potenciales a partir de una oferta de Marketplace está en desuso. Si actualmente tiene una oferta publicada con la configuración de administración de clientes potenciales de Blob de Azure, ya no recibirá clientes potenciales. Actualice la configuración de administración de clientes potenciales a cualquiera de las otras opciones de administración de clientes potenciales. Obtenga información sobre las demás opciones en la [página de aterrizaje de administración de clientes potenciales](./commercial-marketplace-get-customer-leads.md).

Si el sistema de administración de las relaciones con el cliente (CRM) no se admite explícitamente en el Centro de partners para recibir clientes potenciales de Azure Marketplace y AppSource, puede usar un blob de Azure para controlar estos clientes potenciales. Después, puede exportar los datos e importarlos en el sistema CRM. Las instrucciones de este artículo le guiarán por el proceso de creación de una cuenta de Azure Storage y un blob de Azure en esa cuenta. Además, puede crear un nuevo flujo mediante Microsoft Flow para enviar una notificación por correo electrónico cuando su oferta reciba un cliente potencial.


## <a name="how-to-configure-azure-blob"></a>Cómo configurar un blob de Azure

1. Si no tiene una cuenta de Azure, puede [crear una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
1. Cuando la cuenta de Azure esté activa, inicie sesión en [Azure Portal](https://portal.azure.com).
1. En Azure Portal, cree una cuenta de almacenamiento mediante el siguiente procedimiento.  
    1. En la barra de menús de la izquierda, seleccione **+ Crear un recurso**.  El panel **Nuevo** (hoja) se mostrará a la derecha.
    2. Seleccione **Almacenamiento** en el panel **Nuevo**.  En la derecha se mostrará una lista de **Destacados**.
    3. Seleccione **Cuenta de almacenamiento** para iniciar la creación de la cuenta.  Siga las instrucciones que aparecen en el artículo [Crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

    ![Pasos para crear una cuenta de Azure Storage](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-create.png)

    Para más información sobre las cuentas de almacenamiento, seleccione [Tutorial de inicio rápido](https://docs.microsoft.com/azure/storage/).  Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

4. Espere hasta que se aprovisione la cuenta de almacenamiento, proceso que normalmente tarda unos minutos.  Luego, acceda a la cuenta de almacenamiento desde la página de **Inicio** de Azure Portal mediante la selección de **Ver todos los recursos** o **Todos los recursos** en la barra de menú de navegación de la izquierda.

    ![Acceso a la cuenta de almacenamiento de Azure](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-access.png)

5. En el panel de la cuenta de almacenamiento, seleccione **Claves de acceso** y copie el valor de la *Cadena de conexión* de la clave. Guarde este valor, ya que es el valor de la *cadena de conexión de cuenta de almacenamiento* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

     Este es un ejemplo de una cadena de conexión:

     ```sql
     DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
     ```

    ![Claves de Azure Storage](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-keys-2.png)

6. En la página de la cuenta de almacenamiento, seleccione **Blobs**.

   ![Claves de Azure Storage](./media/commercial-marketplace-lead-management-instructions-azure-blob/select-blobs.png)

7. Cuando esté en la página de blobs, seleccione el botón **+ Contenedor**.

8. Escriba un **nombre** para el nuevo contenedor. El nombre del contenedor debe escribirse en minúsculas, comenzar por una letra o un número, y solo puede incluir letras, números y el carácter de guión (-). Para más información sobre la nomenclatura de contenedores y blobs, consulte [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Asignación de nombres y referencia a contenedores, blobs y metadatos).

    Guarde este valor, ya que es el valor de *Nombre del contenedor* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

9. Establezca el nivel de acceso público al contenedor como **Privado (sin acceso anónimo)** .

10. Seleccione **Aceptar** para crear el contenedor.

    ![Nuevo contenedor](./media/commercial-marketplace-lead-management-instructions-azure-blob/new-container.png)

## <a name="configure-your-offer-to-send-leads-to-the-azure-blob"></a>Configuración de la oferta para enviar clientes potenciales al blob de Azure

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos:

1. Vaya a la página **Configuración de la oferta** de su oferta.
2. Seleccione **Conectar** en la sección Administración de clientes potenciales.

    ![Conectar oferta](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-offer.png)

3. En la ventana emergente Detalles de conexión, seleccione **Blob de Azure** como destino de clientes potenciales.

    ![Detalles de conexión](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-details.png) 

4. Proporcione el **Nombre del contenedor** y la **Cadena de conexión de cuenta de almacenamiento** que ha recibido según las instrucciones a continuación.

    * Ejemplo de nombre de contenedor: `marketplaceleadcontainer`
    * Ejemplo de Cadena de conexión de cuenta de almacenamiento: `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net` ![Detalles de conexión](./media/commercial-marketplace-lead-management-instructions-azure-blob/connection-details.png) 

5. Seleccione **Guardar**.

    > [!NOTE]
    > Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.


