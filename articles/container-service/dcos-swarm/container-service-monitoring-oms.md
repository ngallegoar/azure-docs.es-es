---
title: '(EN DESUSO) Supervisión de un clúster DC/OS en Azure: administración de operaciones'
description: Supervisión de un clúster de DC/OS de Azure Container Service con Log Analytics.
author: keikhara
ms.service: container-service
ms.topic: conceptual
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 1ab8d1cf3eb38a17f0b3d6c8137e37237498a527
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277323"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-log-analytics"></a>(EN DESUSO) Supervisión de un clúster de DC/OS en Azure Container Service con Log Analytics

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Log Analytics es la solución de administración de TI basada en la nube de Microsoft que lo ayuda a administrar y proteger su infraestructura local y en la nube. La solución de contenedor es una solución de Log Analytics, que le permite ver el inventario de contenedor, el rendimiento y los registros en una sola ubicación. Puede auditar contenedores y solucionar problemas de los mismos mediante la visualización de los registros en una ubicación centralizada, así como encontrar contenedores ruidosos con un consumo excesivo.

![](media/container-service-monitoring-oms/image1.png)

Para obtener más información sobre la solución de contenedor, consulte [Solución Containers (versión preliminar) en Log Analytics](../../azure-monitor/insights/containers.md).

## <a name="setting-up-log-analytics-from-the-dcos-universe"></a>Configuración de Log Analytics desde el universo de DC/OS


En este artículo se supone que ha configurado un DC/OS y ha implementado aplicaciones de contenedor web simples en el clúster.

### <a name="pre-requisite"></a>Requisito previo
- [Suscripción de Microsoft Azure](https://azure.microsoft.com/free/): se puede obtener gratis.  
- Configuración de área de trabajo de Log Analytics (consulte el "paso 3" a continuación)
- [CLI de DC/OS](https://docs.mesosphere.com/1.12/cli) instalada.

1. En el panel de DC/OS, haga clic en Universo y busque ‘OMS’ tal y como se muestra a continuación.

   >[!NOTE]
   >OMS ahora se conoce como Log Analytics.

   ![](media/container-service-monitoring-oms/image2.png)

2. Haga clic en **Instalar**. Verá un mensaje emergente con la información de versión y un botón **Instalar paquete** o **Advanced Installation** (Instalación avanzada). Al hacer clic en **Instalación avanzada**, se abre la página **OMS specific configuration properties** (Propiedades de configuración específicas de OMS).

   ![](media/container-service-monitoring-oms/image3.png)

   ![](media/container-service-monitoring-oms/image4.png)

3. Aquí se le pedirá que escriba `wsid` (el identificador del área de trabajo de Log Analytics) y `wskey` (la clave principal para el identificador del área de trabajo). Para obtener tanto `wsid` como `wskey`, tiene que crear una cuenta en <https://mms.microsoft.com>.
   Siga los pasos para crear una cuenta. Una vez que haya terminado de crear la cuenta, deberá obtener su `wsid` y `wskey` haciendo clic en **Configuración**, luego en **Orígenes conectados** y, por último, en **Servidores Linux**, tal y como se muestra a continuación.

   ![](media/container-service-monitoring-oms/image5.png)

4. Seleccione el número de instancias que desee y haga clic en el botón Revisar e instalar. Por lo general, querrá tener el mismo número de instancias que VM que hay en el clúster de agente. El agente de Log Analytics para Linux se instala como contenedores individuales en cada VM que quiere para recopilar información de supervisión e inicio de sesión.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

## <a name="setting-up-a-simple-log-analytics-dashboard"></a>Configuración de un panel de Log Analytics sencillo

Una vez que instalado al agente de Log Analytics para Linux en las VM, el siguiente paso es configurar el panel de Log Analytics. Puede configurar el panel a través de Azure Portal.

### <a name="azure-portal"></a>Portal de Azure 

Inicie sesión en Azure Portal en <https://portal.microsoft.com/>. Vaya a **Marketplace**, seleccione **Supervisión y administración** y haga clic en **See All** (Ver todo). A continuación, escriba `containers` en la búsqueda. Verá "contenedores" en los resultados de búsqueda. Seleccione **Contenedores** y haga clic en **Crear**.

![](media/container-service-monitoring-oms/image9.png)

Cuando haga clic en **Crear**, le preguntará por el área de trabajo. Seleccione el área de trabajo o si no tiene uno, cree uno nuevo.

![](media/container-service-monitoring-oms/image10.PNG)

Una vez que haya seleccionado el área de trabajo, haga clic en **Crear**.

![](media/container-service-monitoring-oms/image11.png)

Para más información sobre la solución de contenedor de Log Analytics, consulte [Solución de Supervisión de contenedores de Azure Log Analytics](../../azure-monitor/insights/containers.md).

### <a name="how-to-scale-log-analytics-agent-with-acs-dcos"></a>Escalado del agente de Log Analytics con ACS DC/OS 

En caso de que se necesite tener instalado el agente de Log Analytics o esté escalando verticalmente VMSS mediante la incorporación de más conjuntos de escalado de máquinas virtuales, puede hacerlo mediante el escalado del servicio `msoms`.

Puede ir a Marathon o la pestaña de servicios de IU de DC/OS y escalar verticalmente el número de nodos.

![](media/container-service-monitoring-oms/image12.PNG)

Esto se implementará en otros nodos que aún no han implementado el agente de Log Analytics.

## <a name="uninstall-ms-oms"></a>Desinstalación de MS OMS

Para desinstalar MS OMS escriba el siguiente comando:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>¡Queremos saber!
¿Qué funciona? ¿Qué falta? ¿Qué más necesita para esto para que sea útil para usted? Háganos saber en <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Pasos siguientes

 Ahora que ha configurado Log Analytics para supervisar los contenedores,[vea el panel de contenedores](../../azure-monitor/insights/containers.md).
