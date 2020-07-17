---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: ba695337a1da1e440895f6b9f6fe2eb34f5041e3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800618"
---
Security Center proporciona protección contra amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Security Center proporciona protección contra amenazas en diferentes niveles: 

* **Nivel de host**: el agente de Log Analytics supervisa si existen actividades sospechosas en Linux. El agente desencadena alertas de actividades sospechosas que se originan en el nodo o en un contenedor que se ejecuta en el mismo. Algunos ejemplos de estas actividades son la detección shell de web y la conexión con direcciones IP sospechosas conocidas.

    Para obtener una visión más detallada de la seguridad del entorno en el contenedor, el agente supervisa el análisis específico de ese contenedor. Esto desencadenará alertas de eventos como la creación de contenedores con privilegios, las actividades sospechosas de acceso a los servidores de API y los servidores de Secure Shell (SSH) que se ejecutan dentro de un contenedor de Docker.

    >[!IMPORTANT]
    > Si decide no instalar los agentes en los hosts, solo recibirá un subconjunto de las ventajas de la protección contra amenazas y las alertas de seguridad. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.

    Para obtener una lista de las alertas de nivel de host de AKS, consulte la [Tabla de referencia de alertas](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-containerhost).


* En el **nivel de clúster de AKS**, la protección contra amenazas se basa en el análisis de los registros de auditoría de Kubernetes. Para habilitar esta supervisión **sin agente**, agregue la opción de Kubernetes a la suscripción desde la página **Precios y configuración** (consulte los [precios](https://docs.microsoft.com/azure/security-center/security-center-pricing)). Para generar alertas en este nivel, Security Center supervisa los servicios que administra AKS con los registros que recupera el mismo AKS. Entre los ejemplos de eventos en este nivel se incluyen los paneles de Kubernetes expuestos y la creación de roles con privilegios elevados y de montajes confidenciales.

    >[!NOTE]
    > Igualmente, Security Center genera alertas de seguridad para las acciones y las implementaciones de Azure Kubernetes Service que se producen después de que la opción de Kubernetes esté habilitada en la configuración de la suscripción. 

    Para obtener una lista de las alertas de nivel de clúster de AKS, consulte la [Tabla de referencia de alertas](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-akscluster).

Además, nuestro equipo global de investigadores de seguridad supervisa constantemente el panorama de las amenazas. Agregan alertas específicas del contenedor y vulnerabilidades a medida que se detectan.

> [!TIP]
> Puede simular las alertas de contenedor al seguir las instrucciones de [esta entrada de blog](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).