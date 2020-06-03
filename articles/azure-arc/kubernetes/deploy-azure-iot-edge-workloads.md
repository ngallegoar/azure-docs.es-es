---
title: Implementación de cargas de trabajo de Azure IoT Edge (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Implementación de cargas de trabajo de Azure IoT Edge
keywords: Kubernetes, Arc, Azure, K8s, contenedores
ms.openlocfilehash: d82c93783d80060bc3443131191b7cec32dc4878
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680785"
---
# <a name="deploy-azure-iot-edge-workloads-preview"></a>Implementación de cargas de trabajo de Azure IoT Edge (versión preliminar)

## <a name="overview"></a>Información general

Azure Arc y Azure IoT Edge complementan sus correspondientes funcionalidades. Azure Arc proporciona mecanismos para que los operadores de clúster configuren los componentes básicos de un clúster, así como para aplicar directivas de clúster. Por su parte, IoT Edge permite a los operadores de aplicaciones implementar y administrar de forma remota las cargas de trabajo a escala con una cómoda ingesta en la nube y primitivas de comunicación bidireccional. Esto se muestra en el diagrama siguiente:

![Configuración de Arc de IoT](./media/edge-arc.png)

## <a name="pre-requisites"></a>Requisitos previos

* [Registre un dispositivo IoT Edge](https://docs.microsoft.com/azure/iot-edge/quickstart-linux#register-an-iot-edge-device) e [implemente el módulo de sensor de temperatura simulado](https://docs.microsoft.com/azure/iot-edge/quickstart-linux#deploy-a-module). Asegúrese de anotar la cadena de conexión del dispositivo.

* Use [la compatibilidad con Kubernetes de IoT Edge](https://aka.ms/edgek8sdoc) para implementarlo a través del operador Flux de Azure Arc.

* Descargue el archivo [**values.yaml**](https://github.com/Azure/iotedge/blob/master/kubernetes/charts/edge-kubernetes/values.yaml) para el gráfico Helm de IoT Edge y reemplace el marcador de posición **deviceConnectionString** al final del archivo por el que ha anotado en el paso 1. Puede establecer cualquier otra opción de instalación de gráficos admitida según sea necesario. Cree un espacio de nombres para la carga de trabajo de IoT Edge y agréguele un secreto:

    ```
    $ kubectl create ns iotedge

    $ kubectl create secret generic dcs --from-file=fully-qualified-path-to-values.yaml --namespace iotedge
    ```

    También puede establecerlo de forma remota mediante el [ejemplo de configuración de clúster](./use-gitops-connected-cluster.md).

## <a name="connect-a-cluster"></a>Conexión de un clúster

Use la extensión `connectedk8s` de la CLI de `az` para conectar un clúster de Kubernetes a Azure Arc:

  ```
  az connectedk8s connect --name AzureArcIotEdge --resource-group AzureArcTest
  ```

## <a name="create-a-configuration-for-iot-edge"></a>Creación de una configuración para IoT Edge

Repositorio de ejemplo: https://github.com/veyalla/edgearc

Este repositorio apunta al gráfico de Helm de IoT Edge y hace referencia al secreto creado en la sección de requisitos previos.

1. Use la extensión `k8sconfiguration` de la CLI de `az` a fin de crear una configuración para vincular el clúster conectado al repositorio de Git:

    ```
    az k8sconfiguration create --name iotedge --cluster-name AzureArcIotEdge --resource-group AzureArcTest --operator-instance-name iotedge --operator-namespace azure-arc-iot-edge --enable-helm-operator --helm-operator-chart-version 0.6.0 --helm-operator-chart-values "--set helm.versions=v3" --repository-url "git://github.com/veyalla/edgearc.git" --cluster-scoped
    ```

    En uno o dos minutos, debería ver los módulos de la carga de trabajo de IoT Edge implementados en el espacio de nombres `iotedge` del clúster. Puede examinar los registros del pod `SimulatedTemperatureSensor` en ese espacio de nombres para ver los valores de ejemplo que se generan. También puede ver los mensajes que llegan al centro de IoT mediante la [extensión Azure IoT Hub Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

## <a name="cleanup"></a>Limpieza

Puede quitar la configuración mediante:

```
az k8sconfiguration delete -g AzureArcTest --cluster-name AzureArcIotEdge --name iotedge
```

## <a name="next-steps"></a>Pasos siguientes

[Uso de Azure Policy para controlar la configuración del clúster](./use-azure-policy.md)
