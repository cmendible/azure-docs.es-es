---
title: 'Extracción de frases clave: pasos de configuración e implementación de Kubernetes'
titleSuffix: Azure Cognitive Services
description: 'Extracción de frases clave: pasos de configuración e implementación de Kubernetes'
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/21/2019
ms.author: dapine
ms.openlocfilehash: e79717b15cabe815debfa9fd960c0b2623e8088e
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70847734"
---
## <a name="deploy-the-key-phrase-extraction-container-to-an-aks-cluster"></a>Implementación del contenedor de extracción de frases clave en un clúster de AKS

1. Abra la CLI de Azure e inicie sesión en Azure.

    ```azurecli
    az login
    ```

1. Inicie sesión en el clúster de AKS. Reemplace `your-cluster-name` y `your-resource-group` por los valores adecuados.

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Después de la ejecución de este comando, se muestra un mensaje similar al siguiente:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Si tiene varias suscripciones disponibles en su cuenta de Azure y el comando `az aks get-credentials` devuelve un error, puede deberse a un problema común de uso de la suscripción incorrecta. Establezca el contexto de la sesión de la CLI de Azure para usar la misma suscripción con la que creó los recursos y vuelva a intentarlo.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Abra el editor de texto que prefiera. En este ejemplo se utiliza Visual Studio Code.

    ```azurecli
    code .
    ```

1. En el editor de texto, cree un nuevo archivo llamado *keyphrase.yaml* y pegue el siguiente código YAML en él. Asegúrese de reemplazar `billing/value` y `apikey/value` por su propia información.

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: keyphrase
    spec:
      template:
        metadata:
          labels:
            app: keyphrase-app
        spec:
          containers:
          - name: keyphrase
            image: mcr.microsoft.com/azure-cognitive-services/keyphrase
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: # {ENDPOINT_URI}
            - name: apikey
              value: # {API_KEY}
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: keyphrase
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: keyphrase-app
    ```

1. Guarde el archivo y cierre el editor de texto.
1. Ejecute el comando `apply` de Kubernetes con el archivo *keyphrase.yaml* como destino:

    ```console
    kuberctl apply -f keyphrase.yaml
    ```

    Una vez que el comando ha aplicado correctamente la configuración de implementación, se muestra un mensaje similar al siguiente:

    ```console
    deployment.apps "keyphrase" created
    service "keyphrase" created
    ```
1. Compruebe que el pod está implementado:

    ```console
    kubectl get pods
    ```

    La salida del estado de ejecución del pod:

    ```console
    NAME                         READY     STATUS    RESTARTS   AGE
    keyphrase-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Compruebe que el servicio está disponible y obtenga la dirección IP.

    ```console
    kubectl get services
    ```

    La salida del estado de ejecución del servicio *keyphrase* en el pod:

    ```console
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    keyphrase    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```
