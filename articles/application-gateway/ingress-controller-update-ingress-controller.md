---
title: Actualización del controlador de entrada con Helm
description: En este artículo se proporciona información sobre cómo actualizar una entrada de Application Gateway mediante Helm.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 3903ccd1c15765d06cd1794a40567e2c70062538
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795900"
---
# <a name="how-to-upgrade-application-gateway-ingress-controller-using-helm"></a>Cómo actualizar el controlador de entrada de Application Gateway mediante Helm 

El controlador de entrada de Azure Application Gateway para Kubernetes (AGIC) se puede actualizar mediante un repositorio de Helm hospedado en Azure Storage.

Antes de comenzar el procedimiento de actualización, asegúrese de que ha agregado el repositorio necesario:

- Vea los repositorios de Helm agregados actualmente con:

    ```bash
    helm repo list
    ```

- Agregue el repositorio de AGIC con:

    ```bash
    helm repo add \
        application-gateway-kubernetes-ingress \
        https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    ```

## <a name="upgrade"></a>Actualizar

1. Actualice el repositorio de Helm de AGIC para obtener la versión más reciente:

    ```bash
    helm repo update
    ```

1. Vea las versiones disponibles del gráfico de `application-gateway-kubernetes-ingress`:

    ``` bash
    helm search -l application-gateway-kubernetes-ingress
    ```

    Respuesta de ejemplo:

    ```bash
    NAME                                                    CHART VERSION   APP VERSION     DESCRIPTION
    application-gateway-kubernetes-ingress/ingress-azure    0.7.0-rc1       0.7.0-rc1       Use Azure Application Gateway as the ingress for an Azure...
    application-gateway-kubernetes-ingress/ingress-azure    0.6.0           0.6.0           Use Azure Application Gateway as the ingress for an Azure...
    ```

    La última versión disponible de la lista anterior es la siguiente: `0.7.0-rc1`

1. Vea los gráficos de Helm instalados actualmente:

    ```bash
    helm list
    ```

    Respuesta de ejemplo:

    ```bash
    NAME            REVISION        UPDATED                         STATUS  CHART                   APP VERSION     NAMESPACE
    odd-billygoat   22              Fri Jun 21 15:56:06 2019        FAILED  ingress-azure-0.7.0-rc1 0.7.0-rc1       default
    ```

    La instalación del gráfico de Helm de la respuesta de ejemplo anterior se denomina `odd-billygoat`. Usaremos este nombre para el resto de los comandos. Lo más probable es que el nombre de la implementación real sea diferente.

1. Actualice la implementación de Helm a una nueva versión:

    ```bash
    helm upgrade \
        odd-billygoat \
        application-gateway-kubernetes-ingress/ingress-azure \
        --version 0.9.0-rc2
    ```

## <a name="rollback"></a>Reversión

Si se produce un error en la implementación de Helm, puede revertir a una versión anterior.

1. Obtenga el último número de versión correcta conocida:

    ```bash
    helm history odd-billygoat
    ```

    Salida del ejemplo:

    ```bash
    REVISION        UPDATED                         STATUS          CHART                   DESCRIPTION
    1               Mon Jun 17 13:49:42 2019        DEPLOYED        ingress-azure-0.6.0     Install complete
    2               Fri Jun 21 15:56:06 2019        FAILED          ingress-azure-xx        xxxx
    ```

    A partir de la salida de ejemplo del comando `helm history` parece que la última implementación correcta de `odd-billygoat` fue la revisión `1`

1. Revierta a la última revisión correcta:

    ```bash
    helm rollback odd-billygoat 1
    ```
