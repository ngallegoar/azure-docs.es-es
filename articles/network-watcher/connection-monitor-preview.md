---
title: Monitor de conexión (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo usar el Monitor de conexión (versión preliminar) para supervisar la comunicación de red en un entorno distribuido.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: d926a9f686f0f4c39203b8a217a7c608cfad926e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548121"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>Supervisión de conectividad de red con el Monitor de conexión (versión preliminar)

El Monitor de conexión (versión preliminar) proporciona una supervisión de conexión unificada de un extremo a otro en Azure Network Watcher. La característica Monitor de conexión (versión preliminar) admite implementaciones híbridas y en la nube de Azure. Network Watcher proporciona herramientas para supervisar, diagnosticar y ver las métricas relacionadas con la conectividad de las implementaciones de Azure.

Estos son algunos casos de uso del Monitor de conexión (versión preliminar):

- La VM de servidor front-end web se comunica con una VM de servidor de bases de datos en una aplicación de varios niveles. Quiere comprobar la conectividad de red entre las dos VM.
- Quiere que las VM en la región Este de EE. UU. hagan ping a las VM en la región Centro de EE. UU. y quiere comparar las latencias de red entre regiones.
- Tiene varios sitios de oficina locales en Seattle, Washington y en Ashburn, Virginia. Los sitios de oficina se conectan a las direcciones URL de Office 365. Para los usuarios de las direcciones URL de Office 365, compare las latencias entre Seattle y Ashburn.
- La aplicación híbrida necesita conectividad a un punto de conexión de Azure Storage. El sitio local y la aplicación de Azure se conectan al mismo punto de conexión de Azure Storage. Quiere comparar las latencias del sitio local con las latencias de la aplicación de Azure.
- Quiere comprobar la conectividad entre las instalaciones locales y las máquinas virtuales de Azure que hospedan la aplicación en la nube.

En su fase de versión preliminar, el Monitor de conexión combina lo mejor de dos características: la [Monitor de conexión](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) de Network Watcher y la característica [Monitor de conectividad de servicio](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) de Network Performance Monitor (NPM).

Estas son algunas ventajas del Monitor de conexión (versión preliminar):

* Experiencia unificada e intuitiva para las necesidades de supervisión híbrida y de Azure
* Supervisión de conectividad entre regiones y entre áreas de trabajo
* Frecuencias de sondeo más altas y una mejor visibilidad del rendimiento de la red
* Alertas más rápidas para las implementaciones híbridas
* Compatibilidad con las comprobaciones de conectividad basadas en HTTP, TCP e ICMP 
* Las métricas y Log Analytics admiten configuraciones de pruebas de Azure y que no son de Azure

![Diagrama que muestra cómo el Monitor de conexión interactúa con las máquinas virtuales de Azure, los hosts que no son de Azure, los puntos de conexión y las ubicaciones de almacenamiento de datos](./media/connection-monitor-2-preview/hero-graphic.png)

Siga estos pasos para empezar a usar el Monitor de conexión (versión preliminar) para la supervisión: 

1. Instale los agentes de supervisión.
1. Habilite Network Watcher en la suscripción.
1. Cree un monitor de conexión.
1. Configure las alertas y el análisis de datos.
1. Diagnostique los problemas en la red.

En las secciones siguientes se proporcionan detalles sobre estos pasos.

## <a name="install-monitoring-agents"></a>Instalar los agentes de supervisión

El Monitor de conexión se basa en archivos ejecutables ligeros para ejecutar las comprobaciones de conectividad.  Admite las comprobaciones de conectividad desde entornos locales y de Azure. El archivo ejecutable que usa depende de si la VM se hospeda en Azure o en un entorno local.

### <a name="agents-for-azure-virtual-machines"></a>Agentes para máquinas virtuales de Azure

Para que el Monitor de conexión reconozca las máquinas virtuales de Azure como orígenes de supervisión, instale en ellas la extensión de máquina virtual del agente de Network Watcher. Esta extensión también se conoce como la *extensión Network Watcher*. Las máquinas virtuales de Azure requieren la extensión para desencadenar la supervisión de un extremo a otro y otras funcionalidades avanzadas. 

Puede instalar la extensión Network Watcher cuando [cree una máquina virtual](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm). También puede instalar, configurar y solucionar problemas de la extensión de Network Watcher para [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) y [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows) por separado.

Las reglas de un grupo de seguridad de red (NSG) o firewall pueden bloquear la comunicación entre el origen y el destino. El Monitor de conexión detecta este problema y lo muestra como un mensaje de diagnóstico en la topología. Para habilitar la supervisión de la conexión, asegúrese de que las reglas de firewall y NSG permitan los paquetes a través de TCP o ICMP entre el origen y el destino.

### <a name="agents-for-on-premises-machines"></a>Agentes para máquinas locales

Para que el Monitor de conexión reconozca las máquinas locales como orígenes para la supervisión, instale el agente de Log Analytics en las máquinas. Luego, habilite la solución Network Performance Monitor. Estos agentes están vinculados a áreas de trabajo de Log Analytics, por lo que es necesario configurar el identificador de área de trabajo y la clave principal antes de que los agentes puedan iniciar la supervisión.

Para instalar el agente de Log Analytics para las máquinas Windows, consulte [Extensión de máquina virtual de Azure Monitor para Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Si la ruta de acceso incluye firewalls o aplicaciones virtuales de red (NVA), asegúrese de que el destino sea accesible.

## <a name="enable-network-watcher-on-your-subscription"></a>Habilitar Network Watcher en la suscripción

Todas las suscripciones que tienen una red virtual se habilitan con Network Watcher. Al crear una red virtual en la suscripción, Network Watcher se habilita automáticamente en la región y suscripción de la red virtual. Esta habilitación automática no afecta los recursos ni incurre en cargos. Asegúrese de que Network Watcher no esté explícitamente deshabilitado en la suscripción. 

Para más información, consulte [Habilitación de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="create-a-connection-monitor"></a>Creación de un monitor de conexión 

El Monitor de conexión supervisa la comunicación a intervalos regulares. Informa sobre los cambios en la disponibilidad y la latencia. También puede comprobar la topología de red actual e histórica entre los agentes de origen y los puntos de conexión de destino.

Los orígenes pueden ser máquinas virtuales de Azure o máquinas locales que tengan instalado un agente de supervisión. Los puntos de conexión de destino pueden ser direcciones URL de Office 365, direcciones URL de Dynamics 365, direcciones URL personalizadas, identificadores de recursos de máquinas virtuales de Azure, IPv4, IPv6, FQDN o cualquier nombre de dominio.

### <a name="access-connection-monitor-preview"></a>Acceso al Monitor de conexión (versión preliminar)

1. En la página principal de Azure Portal, vaya a **Network Watcher**.
1. A la izquierda, en la sección **Supervisión**, seleccione **Monitor de conexión (versión preliminar)** .
1. Verá todos los monitores de conexión que se crearon en el Monitor de conexión (versión preliminar). Para ver los monitores de conexión que se crearon en la experiencia clásica del Monitor de conexión, vaya a la pestaña **Monitor de conexión**.

    ![Captura de pantalla que muestra los monitores de conexión que se crearon en el Monitor de conexión (versión preliminar)](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="create-a-connection-monitor"></a>Creación de un monitor de conexión

En los monitores de conexión que crea en el Monitor de conexión (versión preliminar), puede agregar máquinas locales y máquinas virtuales de Azure como orígenes. Estos monitores de conexión también pueden supervisar la conectividad con los puntos de conexión. Los puntos de conexión pueden estar en Azure o en cualquier otra dirección URL o IP.

El Monitor de conexión (versión preliminar) incluye estas entidades:

* **Recurso de monitor de conexión**: un recurso de Azure específico de la región. Todas las entidades siguientes son propiedades de un recurso de monitor de conexión.
* **Punto de conexión**: un origen o destino que participa en las comprobaciones de conectividad. Algunos ejemplos de puntos de conexión son las máquinas virtuales de Azure, los agentes locales, las direcciones URL y las direcciones IP.
* **Configuración de prueba**: la configuración de una prueba específica para el protocolo. En función del protocolo elegido, puede definir el puerto, los umbrales, la frecuencia de las pruebas y otros parámetros.
* **Grupo de prueba**: el grupo que contiene puntos de conexión de origen, puntos de conexión de destino y configuraciones de prueba. Un monitor de conexión puede contener más de un grupo de prueba.
* **Prueba**: la combinación de un punto de conexión de origen, un punto de conexión de destino y una configuración de prueba. Una prueba es el nivel más granular en el que están disponibles los datos de supervisión. Los datos de supervisión incluyen el porcentaje de comprobaciones con errores y el tiempo de ida y vuelta (RTT).

 ![Diagrama que muestra un monitor de conexión y donde se define la relación entre los grupos de prueba y las pruebas](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="create-a-connection-monitor-from-the-azure-portal"></a>Creación de un monitor de conexión desde Azure Portal

Siga estos pasos para crear un monitor de conexión desde Azure Portal:

1. En el panel **Monitor de conexión (versión preliminar)** , en la esquina superior izquierda, seleccione **Crear**.
1. En la pestaña **Aspectos básicos**, escriba la información correspondiente al monitor de conexión:
   * **Nombre del monitor de conexión**: agregue el nombre del monitor de conexión. Use las reglas de nomenclatura estándar para los recursos de Azure.
   * **Suscripción**: elija una suscripción para el monitor de conexión.
   * **Región**: elija una región para el monitor de conexión. Solo puede seleccionar las VM de origen que se crean en esta región.
   * **Configuración del área de trabajo**: el área de trabajo contiene los datos de supervisión. Puede usar un área de trabajo personalizada o el área de trabajo predeterminada. 
       * Para usar el área de trabajo predeterminada, active la casilla. 
       * Para elegir un área de trabajo personalizada, desactive la casilla. A continuación, elija la suscripción y la región del área de trabajo personalizada. 
1. En la parte inferior de la pestaña, seleccione **Siguiente: grupos de prueba**.

   ![Captura de pantalla que muestra la pestaña Aspectos básicos en el Monitor de conexión](./media/connection-monitor-2-preview/create-cm-basics.png)

1. En la pestaña **Grupos de prueba**, seleccione **+ Grupo de prueba**. Para configurar los grupos de prueba, consulte [Creación de grupos de prueba en el Monitor de conexión](#create-test-groups-in-a-connection-monitor). 
1. En la parte inferior de la pestaña, seleccione **Siguiente: Revisar y crear** para revisar el monitor de conexión.

   ![Captura de pantalla que muestra la pestaña Grupos de prueba y el panel donde se agregan los detalles del grupo de prueba](./media/connection-monitor-2-preview/create-tg.png)

1. En la pestaña **Revisar y crear**, revise la información básica y los grupos de prueba antes de crear el monitor de conexión. Si necesita editar el monitor de conexión:
   * Para editar los detalles básicos, seleccione el icono de lápiz.
   * Para editar un grupo de prueba, selecciónelo.

   > [!NOTE] 
   > La pestaña **Revisar y crear** muestra el costo mensual durante la fase de versión preliminar del Monitor de conexión. Actualmente, la columna **CURRENT COST** no muestra ningún cargo. Cuando el Monitor de conexión esté disponible con carácter general, esta columna mostrará un cargo mensual. 
   > 
   > Incluso en la fase de versión preliminar del Monitor de conexión, se aplican los cargos de ingesta de Log Analytics.

1. Cuando esté listo para crear el monitor de conexión, en la parte inferior de la pestaña **Revisar y crear**, seleccione **Crear**.

   ![Captura de pantalla del Monitor de conexión que muestra la pestaña Revisar y crear](./media/connection-monitor-2-preview/review-create-cm.png)

El Monitor de conexión (versión preliminar) creará el recurso de monitor de conexión en segundo plano.

#### <a name="create-a-connection-monitor-by-using-armclient"></a>Creación de un monitor de conexión mediante ARMClient

Use el código siguiente para crear un monitor de conexión mediante ARMClient.

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "https://management.azure.com"

$SUB = "subscriptions/<subscription id 1>;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_<region>"

$body =

"{

location: '<region>',

properties: {

endpoints: [{

name: 'workspace',

resourceId: '/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

filter: {

 items: [{

type: 'AgentAddress',

address: '<FQDN of your on-premises agent>'

}]

}

          },

 {

name: 'vm1',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

},

 {

name: 'vm2',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

   },

{

name: 'azure portal'

address: '<URL>'

   },

 {

    name: 'ip',

     address: '<IP>'

 }

  ],

  testGroups: [{

    name: 'Connectivity to Azure Portal and Public IP',

    testConfigurations: ['http', 'https', 'tcpEnabled', 'icmpEnabled'],

    sources: ['vm1', 'workspace'],

    destinations: ['azure portal', 'ip']

   },

{

    name: 'Connectivty from Azure VM 1 to Azure VM 2',

   // Choose your protocol
   
    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: <threshold for checks failed %>,

     roundTripTimeMs: <threshold for RTT>

    }

   }, {

    name: 'https',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    httpConfiguration: {

     preferHTTPS: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }

  ]

 }

} "
```

Este es el comando de implementación:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="create-test-groups-in-a-connection-monitor"></a>Creación de grupos de prueba en un monitor de conexión

Cada grupo de prueba de un monitor de conexión incluye orígenes y destinos que se prueban en parámetros de red. Se prueban para ver el porcentaje de comprobaciones con errores y el valor de RTT en las configuraciones de prueba.

En Azure Portal, para crear un grupo de prueba en un monitor de conexión, especifique los valores para los campos siguientes:

* **Disable Test Group** (Deshabilitar grupo de prueba): puede seleccionar este campo para deshabilitar la supervisión de todos los orígenes y destinos que el grupo de prueba especifica. Esta selección está desactivada de forma predeterminada.
* **Nombre**: nombre del grupo de prueba.
* **Orígenes**: puede especificar máquinas virtuales de Azure y máquinas locales como orígenes si los agentes están instalados en ellas. Para instalar un agente para el origen, consulte [Instalación de los agentes de supervisión](#install-monitoring-agents).
   * Para elegir los agentes de Azure, seleccione la pestaña **Agentes de Azure**. Aquí solo se verán las máquinas virtuales que están enlazadas a la región que especificó al crear el monitor de conexión. De manera predeterminada, las máquinas virtuales se agrupan en la suscripción a la que pertenecen. Estos grupos están contraídos. 
   
       Puede explorar en profundidad desde el nivel de suscripción a otros niveles de la jerarquía:

      **Suscripción** > **Grupos de recursos** > **Redes virtuales** > **Subredes** > **Máquinas virtuales con agentes**

      También puede cambiar el valor del campo **Agrupar por** para iniciar el árbol desde cualquier otro nivel. Por ejemplo, si se agrupa por red virtual, verá las máquinas virtuales que tienen agentes en la jerarquía **Redes virtuales** > **Subredes** > **Máquinas virtuales con agentes**.

      ![Captura de pantalla del Monitor de conexión, que muestra el panel Agregar orígenes y la pestaña Agentes de Azure](./media/connection-monitor-2-preview/add-azure-sources.png)

   * Para elegir los agentes locales, seleccione la pestaña **Agentes que no son de Azure**. De forma predeterminada, los agentes se agrupan en áreas de trabajo por región. Todas estas áreas de trabajo tienen configurada la solución Network Performance Monitor. 
   
       Si necesita agregar Network Performance Monitor al área de trabajo, puede obtenerlo en [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). Para información sobre cómo agregar Network Performance Monitor, consulte [Soluciones de supervisión en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solutions). 
   
       En la vista **Crear un monitor de conexión**, en la pestaña **Aspectos básicos**, aparece seleccionada la región predeterminada. Si cambia la región, puede elegir agentes de áreas de trabajo de la región nueva. También puede cambiar el valor del campo **Agrupar por** para agrupar por subredes.

      ![Captura de pantalla del Monitor de conexión, que muestra el panel Agregar orígenes y la pestaña Agentes que no son de Azure](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   * Para revisar los agentes de Azure y los que no son de Azure que ha seleccionado, vaya a la pestaña **Revisar**.

      ![Captura de pantalla del Monitor de conexión, que muestra el panel Agregar orígenes y la pestaña Revisar](./media/connection-monitor-2-preview/review-sources.png)

   * Cuando termine de configurar los orígenes, en la parte inferior del panel **Agregar orígenes**, seleccione **Listo**.

* **Destinos**: puede supervisar la conectividad a las máquinas virtuales de Azure o a cualquier punto de conexión (una IP pública, una dirección URL o un FQDN) si se especifican como destinos. En un solo grupo de prueba, puede agregar máquinas virtuales de Azure, direcciones URL de Office 365, direcciones URL de Dynamics 365 y puntos de conexión personalizados.

    * Para elegir las máquinas virtuales de Azure como destinos, seleccione la pestaña **Máquinas virtuales de Azure**. De forma predeterminada, las máquinas virtuales de Azure se agrupan en una jerarquía de suscripción que se encuentra en la misma región que seleccionó en la vista **Crear un monitor de conexión**, en la pestaña **Aspectos básicos**. Puede cambiar la región y elegir VM de Azure de la región recién seleccionada. Luego puede explorar en profundidad desde el nivel Suscripción a otros niveles de la jerarquía, como el nivel Agentes de Azure.

       ![Captura de pantalla del panel Agregar destinos, donde se muestra la pestaña Máquinas virtuales de Azure](./media/connection-monitor-2-preview/add-azure-dests1.png)

       ![Captura de pantalla del panel Agregar destinos en el que se muestra el nivel Suscripción](./media/connection-monitor-2-preview/add-azure-dests2.png)

    * Para elegir puntos de conexión como destinos, seleccione la pestaña **Puntos de conexión**. La lista de puntos de conexión incluye las direcciones URL de prueba de Office 365 y las direcciones URL de prueba de Dynamics 365, agrupadas por nombre. Además de estos puntos de conexión, puede elegir un punto de conexión creado en otros grupos de prueba en el mismo monitor de conexión. 
    
        Para agregar un punto de conexión nuevo, en la esquina superior derecha, seleccione **+ Puntos de conexión**. A continuación, proporcione un nombre de punto de conexión y una dirección URL, IP o FQDN.

       ![Captura de pantalla que muestra dónde agregar puntos de conexión como destinos en el Monitor de conexión](./media/connection-monitor-2-preview/add-endpoints.png)

    * Para revisar las máquinas virtuales de Azure y los puntos de conexión que eligió, seleccione la pestaña **Revisar**.
    * Cuando termine de elegir destinos, seleccione **Listo**.

* **Configuraciones de prueba**: puede asociar configuraciones de prueba a un grupo de prueba. Azure Portal solo permite una configuración de prueba por cada grupo de prueba, pero puede usar ARMClient para agregar más.

    * **Nombre**: asigne un nombre a la configuración de prueba.
    * **Protocolo**: elija TCP, ICMP o HTTP. Para cambiar HTTP a HTTPS, seleccione **HTTP** como el protocolo y **443** como el puerto.
        * **Crear configuración de prueba de red**: esta casilla solo aparece si selecciona **HTTP** en el campo **Protocolo**. Active esta casilla para crear otra configuración de prueba que use los mismos orígenes y destinos que especificó en cualquier otra parte de la configuración. La configuración de prueba recién creada se denomina `<the name of your test configuration>_networkTestConfig`.
        * **Deshabilitar traceroute**: este campo se aplica a los grupos de prueba cuyo protocolo es TCP o ICMP. Active esta casilla evitar que los orígenes detecten la topología y RTT de salto a salto.
    * **Puerto de destino**: puede personalizar este campo con un puerto de destino de su elección.
    * **Frecuencia de prueba**: use este campo para elegir la frecuencia con la que los orígenes harán ping a los destinos en el protocolo y el puerto que especificó. Puede elegir 30 segundos, 1 minuto, 5 minutos, 15 minutos o 30 minutos. Los orígenes probarán la conectividad a los destinos según el valor que elija.  Por ejemplo, si selecciona 30 segundos, los orígenes comprobarán la conectividad con el destino al menos una vez en un período de 30 segundos.
    * **Umbral correcto**: puede establecer umbrales en estos parámetros de red:
       * **Comprobaciones con error**: establezca el porcentaje de comprobaciones que pueden presentar errores cuando los orígenes comprueban la conectividad con los destinos mediante los criterios especificados. En el caso del protocolo TCP o ICMP, el porcentaje de comprobaciones con error puede ser igual al porcentaje de pérdida de paquetes. En el caso del protocolo HTTP, este campo representa el porcentaje de solicitudes HTTP que no recibieron respuesta.
       * **Tiempo de ida y vuelta**: establezca el RTT en milisegundos para indicar cuánto tiempo pueden tardar los orígenes en conectarse al destino a través de la configuración de prueba.
    
       ![Captura de pantalla que muestra dónde establecer una configuración de prueba en el Monitor de conexión](./media/connection-monitor-2-preview/add-test-config.png)

Todos los orígenes, destinos y configuraciones de prueba que se agregan a un grupo de prueba se dividen en pruebas individuales. Este un ejemplo de cómo se dividen los orígenes y destinos:

* Grupo de prueba: TG1
* Orígenes: 3 (A, B, C)
* Destinos: 2 (D, E)
* Configuraciones de prueba: 2 (Config 1, Config 2)
* Total de pruebas creadas: 12

| Número de prueba | Source | Destination | Configuración de prueba |
| --- | --- | --- | --- |
| 1 | Un | D | Config 1 |
| 2 | Un | D | Config 2 |
| 3 | Un | E | Config 1 |
| 4 | Un | E | Config 2 |
| 5 | B | D | Config 1 |
| 6 | B | D | Config 2 |
| 7 | B | E | Config 1 |
| 8 | B | E | Config 2 |
| 9 | C | D | Config 1 |
| 10 | C | D | Config 2 |
| 11 | C | E | Config 1 |
| 12 | C | E | Config 2 |

### <a name="scale-limits"></a>Límites de escalado

Los monitores de conexión tienen los límites de escala siguientes:

* Máximo de monitores de conexión por suscripción por región: 100
* Máximo de grupos de prueba por monitor de conexión: 20
* Máximo de orígenes y destinos por monitor de conexión: 100
* Máximo de configuraciones de prueba por monitor de conexión: 
    * 20 a través de ARMClient
    * 2 a través de Azure Portal

## <a name="analyze-monitoring-data-and-set-alerts"></a>Análisis de los datos de supervisión y establecimiento de alertas

Una vez que se crea un monitor de conexión, los orígenes comprueban la conectividad con los destinos en función de la configuración de prueba.

### <a name="checks-in-a-test"></a>Comprobaciones en una prueba

En función del protocolo que eligió en la configuración de prueba, el Monitor de conexión (versión preliminar) ejecuta una serie de comprobaciones para el par origen-destino. Las comprobaciones se ejecutan según la frecuencia de prueba elegida.

Si usa HTTP, el servicio calcula el número de respuestas HTTP que devolvieron un código de respuesta. El resultado determina el porcentaje de comprobaciones con error. Para calcular el RTT, el servicio mide el tiempo entre una llamada HTTP y la respuesta.

Si usa TCP o ICMP, el servicio calcula el porcentaje de pérdida de paquetes para determinar el porcentaje de comprobaciones con error. Para calcular el RTT, el servicio mide el tiempo que se tarda en recibir la confirmación (ACK) de los paquetes que se enviaron. Si habilitó los datos de traceroute para las pruebas de red, puede ver la pérdida y la latencia salto a salto correspondiente a la red local.

### <a name="states-of-a-test"></a>Estados de una prueba

En función de los datos devueltos por las comprobaciones, las pruebas pueden tener estos estados:

* **Pass** (Sin errores): los valores reales para el porcentaje de comprobaciones con error y RTT están dentro de los umbrales especificados.
* **Fail** (Con errores): los valores reales para el porcentaje de comprobaciones con error o RTT superaron los umbrales especificados. Si no se especifica ningún umbral, una prueba alcanza el estado Fail (Con errores) cuando el porcentaje de comprobaciones con error es 100.
* **Warning** (Advertencia): no se especificó ningún criterio para el porcentaje de comprobaciones con error. En ausencia de los criterios especificados, el Monitor de conexión (versión preliminar) asigna automáticamente un umbral. Cuando se supera ese umbral, el estado de la prueba cambia a Warning (Advertencia).

### <a name="data-collection-analysis-and-alerts"></a>Colección de datos, análisis y alertas

Los datos que el Monitor de conexión (vista previa) recopila se almacenan en el área de trabajo de Log Analytics. Esta área de trabajo se configura al crear el monitor de conexión. 

Los datos de supervisión también están disponibles en métricas de Azure Monitor. Puede usar Log Analytics para conservar los datos de supervisión durante el tiempo que desee. De manera predeterminada, Azure Monitor solo almacena las métricas de 30 días. 

A continuación, puede [establecer alertas basadas en métricas para los datos](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards"></a>Paneles de supervisión

En los paneles de supervisión, verá una lista de los monitores de conexión a los que puede acceder para sus suscripciones, regiones, marcas de tiempo, orígenes y tipos de destino.

Al ir al Monitor de conexión (versión preliminar) desde Network Watcher, puede ver los datos por:

* **Monitor de conexión**: lista de todos los monitores de conexión creados para las suscripciones, regiones, marcas de tiempo, orígenes y tipos de destino. Esta es la vista predeterminada.
* **Grupos de prueba**: lista de todos los grupos de prueba creados para las suscripciones, regiones, marcas de tiempo, orígenes y tipos de destino. Estos grupos de prueba no se filtran por monitores de conexión.
* **Prueba**: lista de todas las pruebas que se ejecutan para las suscripciones, regiones, marcas de tiempo, orígenes y tipos de destino. Estas pruebas no se filtran por monitores de conexión ni grupos de prueba.

En la imagen siguiente, las tres vistas de datos se indican con la flecha 1.

En el panel, puede expandir cada monitor de conexión para ver sus grupos de prueba. A continuación, puede expandir cada grupo de prueba para ver las pruebas que se ejecutan en él. 

Puede filtrar esta lista en función de lo siguiente:

* **Filtros de nivel superior**: elija suscripciones, regiones, marcas de tiempo, orígenes y tipos de destino. Vea el cuadro 2 en la imagen siguiente.
* **Filtros basados en estado**: filtre por el estado del monitor de conexión, el grupo de prueba o la prueba. Vea la flecha 3 en la imagen siguiente.
* **Filtros personalizados**: elija **Seleccionar todo** para llevar a cabo una búsqueda genérica. Para buscar por una entidad específica, seleccione en la lista desplegable. Vea la flecha 4 en la imagen siguiente.

![Captura de pantalla que muestra cómo filtrar vistas de monitores de conexión, grupos de prueba y pruebas en el Monitor de conexión (versión preliminar)](./media/connection-monitor-2-preview/cm-view.png)

Por ejemplo, para ver todas las pruebas del Monitor de conexión (versión preliminar) donde la dirección IP de origen es 10.192.64.56:
1. Cambie la vista a **Prueba**.
1. En el campo de búsqueda, escriba *10.192.64.56*.
1. En la lista desplegable, seleccione **Orígenes**.

Para mostrar solo las pruebas con error en el Monitor de conexión (versión preliminar) donde la dirección IP de origen es 10.192.64.56:
1. Cambie la vista a **Prueba**.
1. En el caso del filtro basado en estado, seleccione **Fail** (Con errores).
1. En el campo de búsqueda, escriba *10.192.64.56*.
1. En la lista desplegable, seleccione **Orígenes**.

Para mostrar solo las pruebas con error en el Monitor de conexión (versión preliminar) donde el destino es outlook.office365.com:
1. Cambie la vista a **Prueba**.
1. En el caso del filtro basado en estado, seleccione **Fail** (Con errores).
1. En el campo de búsqueda, escriba *outlook.office365.com*.
1. En la lista desplegable, seleccione **Destinos**.

   ![Captura de pantalla que muestra una vista filtrada para mostrar solo las pruebas con error para el destino Outlook.Office365.com](./media/connection-monitor-2-preview/tests-view.png)

Para ver las tendencias en RTT y el porcentaje de comprobaciones con error de un monitor de conexión:
1. Seleccione el monitor de conexión que desea investigar. De forma predeterminada, los datos de supervisión están organizados por grupo de prueba.

   ![Captura de pantalla que muestra las métricas de un monitor de conexión por grupo de prueba](./media/connection-monitor-2-preview/cm-drill-landing.png)

1. Elija el grupo de prueba que quiere investigar.

   ![Captura de pantalla que muestra dónde seleccionar un grupo de prueba](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

    Verá las cinco principales pruebas con error del grupo de prueba, según el RTT o el porcentaje de comprobaciones con error. Para cada prueba, verá el RTT y las líneas de tendencia del porcentaje de comprobaciones con error.
1. Seleccione una prueba de la lista o elija otra prueba para investigar. Para el intervalo de tiempo y el porcentaje de comprobaciones con error, verá el umbral y los valores reales. En el caso de RTT, verá los valores de umbral, promedio, mínimo y máximo.

   ![Captura de pantalla que muestra los resultados de una prueba para el RTT y el porcentaje de comprobaciones con error](./media/connection-monitor-2-preview/cm-drill-charts.png)

1. Cambie el intervalo de tiempo para ver más datos.
1. Cambie la vista para ver los orígenes, destinos o configuraciones de prueba. 
1. Elija un origen basado en pruebas con error e investigue las cinco principales pruebas con error. Por ejemplo, elija **Ver por** > **Orígenes** y **Ver por** > **Destinos** para investigar las pruebas pertinentes en el monitor de conexión.

   ![Captura de pantalla que muestra las métricas de rendimiento de las cinco principales pruebas con error](./media/connection-monitor-2-preview/cm-drill-select-source.png)

Para ver las tendencias en RTT y el porcentaje de comprobaciones con error de un grupo de prueba:

1. Seleccione el grupo de prueba que quiere investigar. 

    De forma predeterminada, los datos de supervisión se organizan por orígenes, destinos y configuraciones de prueba (pruebas). Posteriormente, puede cambiar la vista de grupos de prueba a orígenes, destinos o configuraciones de prueba. A continuación, elija una entidad para investigar las cinco principales pruebas con error. Por ejemplo, cambie la vista a orígenes y destinos para investigar las pruebas pertinentes en el monitor de conexión seleccionado.
1. Elija la prueba que quiere investigar.

   ![Captura de pantalla que muestra dónde seleccionar una prueba](./media/connection-monitor-2-preview/tg-drill.png)

    Para el intervalo de tiempo y el porcentaje de comprobaciones con error, verá los valores de umbral y los valores reales. En el caso de RTT, verá los valores de umbral, promedio, mínimo y máximo. También verá alertas activadas para la prueba que seleccionó.
1. Cambie el intervalo de tiempo para ver más datos.

Para ver las tendencias en RTT y el porcentaje de comprobaciones con error de una prueba:
1. Seleccione el origen, el destino y la configuración de prueba que quiere investigar.

    Para el intervalo de tiempo y el porcentaje de comprobaciones con error, verá los valores de umbral y los valores reales. En el caso de RTT, verá los valores de umbral, promedio, mínimo y máximo. También verá alertas activadas para la prueba que seleccionó.

   ![Captura de pantalla que muestra las métricas de una prueba](./media/connection-monitor-2-preview/test-drill.png)

1. Para ver la topología de red, seleccione **Topología**.

   ![Captura de pantalla que muestra la pestaña Topología de red](./media/connection-monitor-2-preview/test-topo.png)

1. Para ver los problemas identificados, en la topología, seleccione cualquier salto en la ruta de acceso. (Estos saltos son recursos de Azure). Actualmente, esta funcionalidad no está disponible para las redes locales.

   ![Captura de pantalla que muestra un vínculo de salto seleccionado en la pestaña Topología](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-log-analytics"></a>Consultas de registro en Log Analytics

Utilice Log Analytics para crear vistas personalizadas de los datos de supervisión. Todos los datos que muestra la interfaz de usuario son de Log Analytics. Puede analizar de manera interactiva los datos en el repositorio. Correlacione los datos de Agent Health u otras soluciones basadas en Log Analytics. Exporte los datos a Excel o Power BI, o bien cree un vínculo que se pueda compartir.

#### <a name="metrics-in-azure-monitor"></a>Métricas en Azure Monitor

En los monitores de conexión que se crearon antes de la experiencia del Monitor de conexión (versión preliminar), las cuatro métricas están disponibles: % Probes Failed (% de sondeos con error), AverageRoundtripMs, ChecksFailedPercent (versión preliminar) y RoundTripTimeMs (versión preliminar). En los monitores de conexión que se crearon en la experiencia del Monitor de conexión (versión preliminar), los datos solo están disponibles para las métricas etiquetadas con *(versión preliminar)* .

![Captura de pantalla que muestra métricas en el Monitor de conexión (versión preliminar)](./media/connection-monitor-2-preview/monitor-metrics.png)

Cuando use métricas, establezca el tipo de recurso como Microsoft.Network/networkWatchers/connectionMonitors.

| Métrica | Nombre para mostrar | Unidad | Tipo de agregación | Descripción | Dimensions |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | % de sondeos con error | Porcentaje | Average | Porcentaje de sondeos de supervisión de conectividad con error. | Sin dimensiones |
| AverageRoundtripMs | Prom. Tiempo de ida y vuelta (ms) | Milisegundos | Average | RTT de red promedio para los sondeos de supervisión de conectividad que se envían entre el origen y el destino. |             Sin dimensiones |
| ChecksFailedPercent (versión preliminar) | Comprobaciones erróneas (%) (versión preliminar) | Porcentaje | Average | Porcentaje de comprobaciones con error de una prueba. | ConnectionMonitorResourceId <br>SourceAddress <br>SourceName <br>SourceResourceId <br>SourceType <br>Protocolo <br>DestinationAddress <br>DestinationName <br>DestinationResourceId <br>DestinationType <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Region |
| RoundTripTimeMs (versión preliminar) | Tiempo de ida y vuelta (ms) (versión preliminar) | Milisegundos | Average | RTT para las comprobaciones enviadas entre el origen y el destino. Este valor no se calcula como promedio. | ConnectionMonitorResourceId <br>SourceAddress <br>SourceName <br>SourceResourceId <br>SourceType <br>Protocolo <br>DestinationAddress <br>DestinationName <br>DestinationResourceId <br>DestinationType <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Region |

#### <a name="metric-alerts-in-azure-monitor"></a>Alertas de métricas en Azure Monitor

Para crear una alerta en Azure Monitor:

1. Elija el recurso de monitor de conexión que creó en el Monitor de conexión (versión preliminar).
1. Asegúrese de que la **Métrica** se muestra como un tipo de señal para el monitor de conexión.
1. En **Agregar conexión**, para el **Nombre de señal**, seleccione **ChecksFailedPercent(Preview)** o **RoundTripTimeMs(Preview)** .
1. En **Tipo de señal**, elija **Métricas**. Por ejemplo, seleccione **ChecksFailedPercent(Preview)** .
1. Se muestran todas las dimensiones de la métrica. Elija el nombre de la dimensión y el valor de la dimensión. Por ejemplo, seleccione **Dirección de origen** y escriba la dirección IP de cualquier origen en el monitor de conexión.
1. En **Lógica de alerta**, rellene estos detalles:
   * **Tipo de condición**: **Estática**.
   * **Condición** y **Umbral**.
   * **Granularidad de agregación y frecuencia de evaluación**: el Monitor de conexión (versión preliminar) actualiza los datos cada minuto.
1. En **Acciones**, elija el grupo de acciones.
1. Proporcione los detalles de la alerta.
1. Cree la regla de alertas.

   ![Captura de pantalla que muestra el área Crear regla en Azure Monitor. Se resalta "Dirección de origen" y "Nombre del punto de conexión de origen"](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="diagnose-issues-in-your-network"></a>Diagnosticar problemas en la red

El Monitor de conexión (versión preliminar) lo ayuda a diagnosticar problemas en el monitor de conexión y en la red. Los agentes de Log Analytics que instaló anteriormente detectan problemas en la red híbrida. La extensión Network Watcher detecta los problemas en Azure. 

Puede ver los problemas en la red de Azure en la topología de red.

En el caso de las redes cuyos orígenes son máquinas virtuales locales, se pueden detectar estos problemas:

* Se ha agotado el tiempo de espera para la solicitud.
* Punto de conexión no resuelto por DNS: temporal o persistente. Dirección URL no válida.
* No se encontraron hosts.
* El origen no puede conectarse al destino. Destino no accesible a través de ICMP.
* Problemas relacionados con los certificados: 
    * Se requiere un certificado de cliente para autenticar el agente. 
    * No se puede tener acceso a la lista de reubicación de certificados. 
    * El nombre de host del punto de conexión no coincide con el nombre alternativo del firmante o el firmante del certificado. 
    * Falta el certificado raíz en el almacén de entidades de certificación de confianza del equipo local del origen. 
    * El certificado SSL expiró, no es válido, se revocó o es incompatible.

En el caso de las redes cuyos orígenes son máquinas virtuales de Azure, se pueden detectar estos problemas:

* Problemas del agente:
    * Se detuvo el agente.
    * Error de resolución de DNS.
    * Ningún agente de escucha ni ninguna aplicación escucha en el puerto de destino.
    * No se pudo abrir el socket.
* Problemas de estado de la máquina virtual: 
    * Iniciando
    * Deteniéndose
    * Detenido
    * Desasignando
    * Desasignado
    * Reiniciándose
    * No asignada
* Falta la entrada de la tabla ARP.
* El tráfico se bloqueó debido a problemas de firewall local o a reglas de NSG.
* Problemas de la puerta de enlace de red virtual: 
    * Faltan rutas.
    * El túnel entre dos puertas de enlace está desconectado o falta.
    * El túnel no encontró la segunda puerta de enlace.
    * No se encontró información de emparejamiento.
* Falta la ruta en Microsoft Edge.
* Tráfico detenido debido a rutas del sistema o UDR.
* BGP no está habilitado en la conexión de puerta de enlace.
* El sondeo de DIP está inactivo en el equilibrador de carga.
