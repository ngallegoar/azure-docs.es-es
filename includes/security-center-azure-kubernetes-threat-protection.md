---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: 051cd8646718c17c288b06a535d35e1eed5e9259
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95561215"
---
Security Center proporciona protección contra amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Security Center proporciona protección contra amenazas en diferentes niveles: 

* **Nivel de host (proporcionado por Azure Defender para servidores)** : con el mismo agente de Log Analytics que Security Center usa en otras máquinas virtuales, Azure Defender supervisa los nodos de AKS de Linux en busca de actividades sospechosas, como la detección de shell web y la conexión con direcciones IP sospechosas conocidas. El agente también supervisa análisis específicos de contenedores, como la creación de contenedores con privilegios, las actividades sospechosas de acceso a los servidores de API y los servidores de Secure Shell (SSH) que se ejecutan dentro de un contenedor de Docker.

    >[!IMPORTANT]
    > Si decide no instalar los agentes en los hosts, solo recibirá un subconjunto de las ventajas de la protección contra amenazas y las alertas de seguridad. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.

    Para obtener una lista de las alertas de nivel de host de AKS, consulte la [Tabla de referencia de alertas](../articles/security-center/alerts-reference.md#alerts-containerhost).


* **Nivel de clúster de AKS (proporcionado por Azure Defender para Kubernetes)** : en el nivel de clúster, la protección contra amenazas se basa en analizar los registros de auditoría de Kubernetes. Para habilitar esta supervisión **sin agente**, habilite Azure Defender. Para generar alertas en este nivel, Security Center supervisa los servicios que administra AKS con los registros que recupera el mismo AKS. Entre los ejemplos de eventos en este nivel se incluyen los paneles de Kubernetes expuestos y la creación de roles con privilegios elevados y de montajes confidenciales.

    >[!NOTE]
    > Igualmente, Security Center genera alertas de seguridad para las acciones y las implementaciones de Azure Kubernetes Service que se producen después de que la opción de Kubernetes esté habilitada en la configuración de la suscripción. 

    Para obtener una lista de las alertas de nivel de clúster de AKS, consulte la [Tabla de referencia de alertas](../articles/security-center/alerts-reference.md#alerts-akscluster).

Además, nuestro equipo global de investigadores de seguridad supervisa constantemente el panorama de las amenazas. Agregan alertas específicas del contenedor y vulnerabilidades a medida que se detectan.

> [!TIP]
> Puede simular las alertas de contenedor al seguir las instrucciones de [esta entrada de blog](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).