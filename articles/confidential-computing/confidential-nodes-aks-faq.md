---
title: Preguntas más frecuentes sobre la compatibilidad con nodos confidenciales en Azure Kubernetes Service (AKS)
description: Encuentre respuestas a algunas de las preguntas habituales sobre Azure Kubernetes Service (AKS) y la compatibilidad con los nodos de computación confidencial de Azure.
author: agowdamsft
ms.service: container-service
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: amgowda
ms.openlocfilehash: b4b6c04a1e35031387a006b6350ebf6cbe502fee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993861"
---
# <a name="frequently-asked-questions-about-confidential-computing-nodes-on-azure-kubernetes-service-aks"></a>Preguntas más frecuentes sobre los nodos de computación confidencial en Azure Kubernetes Service (AKS)

En este artículo se abordan las preguntas más frecuentes sobre los nodos de computación confidencial basados en Intel SGX en Azure Kubernetes Service (AKS). Si tiene más preguntas, puede enviar un correo electrónico a acconaks@microsoft.com.

## <a name="what-service-level-agreement-sla-and-azure-support-is-provided-during-the-preview"></a>¿Qué Acuerdo de Nivel de Servicio (SLA) y Soporte técnico de Azure se proporcionan durante la versión preliminar? 

No se proporciona ningún SLA durante la versión preliminar del producto, tal como se define [aquí](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sin embargo, se ofrece soporte técnico a través del servicio Soporte técnico de Azure.

## <a name="what-is-attestation-and-how-can-we-do-attestation-of-apps-running-in-enclaves"></a>¿Qué es la atestación y cómo se puede realizar la atestación de aplicaciones que se ejecutan en enclaves? 

La atestación es el proceso por el que se demuestra y se confirma que se ha creado correctamente una instancia de un fragmento de software en la plataforma de hardware específica. Este proceso también asegura que su evidencia es comprobable, para proporcionar garantías de que se está ejecutando en una plataforma segura y no se ha alterado. [Lea más](attestation.md) sobre cómo se realiza la atestación en las aplicaciones de enclave.

## <a name="can-i-bring-my-existing-containerized-applications-and-run-it-on-aks-with-azure-confidential-computing"></a>¿Puedo traer las aplicaciones en contenedores que tengo y ejecutarlas en AKS con la computación confidencial de Azure (ACC)? 

Sí, revise la [página de contenedores confidenciales](confidential-containers.md) para obtener más detalles sobre los habilitadores de la plataforma.

## <a name="what-intel-sgx-driver-version-is-installed-in-the-aks-image"></a>¿Qué versión del controlador Intel SGX está instalada en la imagen de AKS? 

Actualmente, las máquinas virtuales DCSv2 de computación confidencial de Azure tienen instalado Intel SGX DCAP 1.33. 

## <a name="can-i-open-an-azure-support-ticket-if-i-run-into-issues"></a>¿Puedo abrir una incidencia de soporte técnico de Azure si tengo problemas? 

Sí. Durante la versión preliminar se proporciona soporte técnico de Azure. No hay ningún acuerdo de nivel de servicio asociado porque el producto está en fase de versión preliminar.

## <a name="can-i-inject-post-install-scriptscustomize-drivers-to-the-nodes-provisioned-by-aks"></a>¿Puedo insertar scripts posteriores a la instalación o controladores personalizados en los nodos aprovisionados por AKS? 

No. Los [nodos de computación confidencial basados en el motor de AKS](https://github.com/Azure/aks-engine/blob/master/docs/topics/sgx.md) son compatibles con nodos de computación confidencial que permiten instalaciones personalizadas.

## <a name="should-i-be-using-a-docker-base-image-to-get-started-on-enclave-applications"></a>¿Debo usar una imagen base de Docker para empezar a trabajar con las aplicaciones de enclave? 

Diversos habilitadores (ISV y proyectos de OSS) proporcionan métodos para habilitar contenedores confidenciales. Revise la [página de contenedores confidenciales](confidential-containers.md) para obtener más detalles y referencias individuales a las implementaciones.

## <a name="can-i-run-acc-nodes-with-other-standard-aks-skus-build-a-heterogenous-node-pool-cluster"></a>¿Puedo ejecutar nodos de ACC con otras SKU de AKS estándar (es decir, crear un clúster de grupos de nodos heterogéneos)? 

Sí, puede ejecutar grupos de nodos diferentes en el mismo clúster de AKS, incluidos los nodos de ACC. Para establecer como destino las aplicaciones de enclave de un grupo de nodos específico, puede agregar selectores de nodo o aplicar límites de EPC. Consulte más detalles sobre el inicio rápido en los nodos confidenciales [aquí](confidential-nodes-aks-get-started.md).

## <a name="can-i-run-windows-nodes-and-windows-containers-with-acc"></a>¿Puedo ejecutar nodos de Windows y contenedores de Windows con ACC? 

De momento, no. Póngase en contacto con nosotros si tiene necesidades relacionadas con nodos o contenedores de Windows. 

## <a name="what-if-my-container-size-is-more-than-available-epc-memory"></a>¿Qué ocurre si el tamaño del contenedor es mayor que la memoria EPC disponible? 

La memoria EPC se aplica a la parte de la aplicación que está programada para ejecutarse en el enclave. Por tanto, no tiene sentido comparar el tamaño total del contenedor con la cantidad máxima de memoria EPC disponible. De hecho, las máquinas DCSv2 con SGX admiten una cantidad máxima de memoria de la máquina virtual de 32 GB, donde se usaría la parte que no es de confianza de la aplicación. Sin embargo, si el contenedor consume más memoria EPC de la que tiene disponible, el rendimiento de la parte del programa que se ejecuta en el enclave podría verse afectado.

Para administrar mejor la memoria EPC en los nodos de trabajo, puede administrar los límites basados en memoria EPC mediante Kubernetes. Siga el ejemplo siguiente como referencia.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
  labels:
    app: sgx-test
spec:
  template:
    metadata:
      labels:
        app: sgx-test
    spec:
      containers:
      - name: sgxtest
        image: oeciteam/sgx-test:1.0
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 10 # This limit will automatically place the job into confidential computing node. Alternatively you can target deployment to nodepools
      restartPolicy: Never
  backoffLimit: 0
```

## <a name="what-happens-if-my-enclave-consumes-more-than-maximum-available-epc-memory"></a>¿Qué ocurre si mi enclave consume más memoria EPC que la cantidad máxima disponible? 

La memoria EPC total disponible se comparte entre las aplicaciones del enclave de las mismas máquinas virtuales o nodos de trabajo. Si la aplicación usa más cantidad de memoria EPC de la que hay disponible, el rendimiento de la aplicación podría verse afectado. Por esta razón, se recomienda establecer valores toleration por aplicación en el archivo YAML de implementación para administrar mejor la memoria EPC disponible por nodos de trabajo, tal como se ha mostrado en los ejemplos anteriores. Como alternativa, siempre puede optar por aumentar los tamaños de máquina virtual del grupo de nodos de trabajo o agregar más nodos. 

## <a name="why-cant-i-do-forks--and-exec-to-run-multiple-processes-in-my-enclave-application"></a>¿Por qué no puedo usar bifurcaciones y exec para ejecutar varios procesos en mi aplicación de enclave? 

Actualmente, las máquinas virtuales de la SKU de DCsv2 de computación confidencial de Azure solo admiten un espacio de direcciones para el programa que se ejecuta en un enclave. Para garantizar un alto nivel de seguridad, se ha impuesto la limitación de un único proceso. Sin embargo, los habilitadores de contenedores confidenciales pueden tener implementaciones alternativas para superar esta limitación.

## <a name="do-you-automatically-install-any-additional-daemonsets-to-expose-the-sgx-drivers"></a>¿Se instala automáticamente cualquier daemonset adicional para exponer los controladores de SGX? 

Sí. Los nombres de los daemonsets son sgx-device-plugin y sgx-quote-helper. Lea más sobre sus propósitos respectivos [aquí](confidential-nodes-aks-overview.md).  

## <a name="what-is-the-vm-sku-i-should-be-choosing-for-confidential-computing-nodes"></a>¿Qué SKU de máquina virtual debería elegir para los nodos de computación confidencial? 

Las SKU de DCSv2. Las [SKU de DCSv2](../virtual-machines/dcv2-series.md) están disponibles en las [regiones admitidas](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all).

## <a name="can-i-still-schedule-and-run-non-enclave-containers-on-confidential-computing-nodes"></a>¿Puedo seguir programando y ejecutando contenedores que no sean de enclave en nodos de computación confidencial? 

Sí. Las máquinas virtuales también tienen una memoria normal que puede ejecutar cargas de trabajo de contenedor estándar. Antes de decidir los modelos de implementación, tenga en cuenta el modelo de seguridad y amenazas de las aplicaciones.

## <a name="can-i-provision-aks-with-dcsv2-node-pools-through-azure-portal"></a>¿Puedo aprovisionar AKS con grupos de nodos DCSv2 mediante Azure Portal? 

Sí. Como alternativa, también se puede usar la CLI de Azure, tal como se documenta [aquí](confidential-nodes-aks-get-started.md).

## <a name="what-ubuntu-version-and-vm-generation-is-supported"></a>¿Qué versión de Ubuntu y generación de máquinas virtuales se admite? 

18.04 en Gen 2. 

## <a name="can-we-change-the-current-intel-sgx-dcap-diver-version-on-aks"></a>¿Se puede cambiar la versión actual de controlador de Intel SGX DCAP en AKS? 

No. Para realizar cualquier instalación personalizada, se recomienda elegir implementaciones de [nodos de trabajo de computación confidencial de motor de AKS](https://github.com/Azure/aks-engine/blob/master/docs/topics/sgx.md). 

## <a name="what-version-of-kubernetes-do-you-support-and-recommend"></a>¿Qué versión de Kubernetes se admite y se recomienda? 

Se admite y se recomienda la versión 1.16 de Kubernetes y versiones posteriores. 

## <a name="what-are-the-known-current-limitation-or-technical-limitations-of-the-product-in-preview"></a>¿Cuál es la limitación actual conocida o las limitaciones técnicas del producto en versión preliminar? 

- Solo admite nodos de máquina virtual de la generación 2 de Ubuntu 18.04. 
- No se admiten nodos de Windows ni contenedores de Windows.
- No se admite la escalabilidad automática horizontal de pods basado en memoria EPC. Se admite el escalado basado en CPU y en memoria normal.
- Actualmente no se admite Dev Spaces en AKS para aplicaciones confidenciales.

## <a name="next-steps"></a>Pasos siguientes
Revise la [página de contenedores confidenciales](confidential-containers.md) para obtener más detalles sobre los contenedores confidenciales.