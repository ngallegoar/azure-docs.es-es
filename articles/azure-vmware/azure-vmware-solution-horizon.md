---
title: Implementación de Horizon en Azure VMware Solution
description: Aprenda a implementar VMware Horizon en Azure VMware Solution.
ms.topic: how-to
ms.date: 09/29/2020
ms.openlocfilehash: babce512b896009c08165d2e3d9aec7c33724bf4
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321364"
---
# <a name="deploy-horizon-on-azure-vmware-solution"></a>Implementación de Horizon en Azure VMware Solution 

>[!NOTE]
>Este documento se centra en el producto VMware Horizon, anteriormente conocido como Horizon 7. Horizon es una solución distinta a Horizon Cloud en Azure, aunque tienen algunos componentes compartidos. Las principales ventajas de Azure VMware Solution incluyen un método de ajuste de tamaño más directo y la integración de la administración de VMware Cloud Foundation en Azure Portal.

[VMware Horizon](https://www.vmware.com/products/horizon.html)®, una plataforma de escritorio virtual y aplicaciones, se ejecuta en el centro de datos y proporciona una administración sencilla y centralizada. Proporciona escritorios virtuales y aplicaciones en cualquier dispositivo y en cualquier lugar. Horizon permite crear y administrar conexiones a escritorios virtuales Windows y Linux, aplicaciones hospedadas de Servidor de Escritorio remoto (RDS), escritorios y máquinas físicas.

Este artículo se centra específicamente en la implementación de Horizon en Azure VMware Solution. Para obtener información general sobre VMware Horizon, vea la documentación de producción de Horizon:

* [Descripción de VMware Horizon](https://www.vmware.com/products/horizon.html)

* [Más información sobre VMware Horizon](https://docs.vmware.com/en/VMware-Horizon/index.html)

* [Arquitectura de referencia de Horizon](https://techzone.vmware.com/resource/workspace-one-and-horizon-reference-architecture)

Con la incorporación de Horizon a Azure VMware Solution, ya hay dos soluciones de infraestructura de escritorio virtual (VDI) en la plataforma Azure. En el diagrama siguiente se resumen las principales diferencias generales.

:::image type="content" source="media/horizon/difference-horizon-azure-vmware-solution-horizon-cloud-azure.png" alt-text="Horizon en Azure VMware Solution y Horizon Cloud en Azure" border="false":::

Horizon 2006 y las versiones posteriores de la línea de versiones de Horizon 8 admiten implementación local e implementación de Azure VMware Solution. Hay algunas características de Horizon que se admiten en local pero no en Azure VMware Solution. También se admiten productos adicionales del ecosistema de Horizon. Para obtener información, vea [Interoperabilidad y paridad de características](https://kb.vmware.com/s/article/80850).

## <a name="deploy-horizon-in-a-hybrid-cloud"></a>Implementación de Horizon en una nube híbrida

Es posible implementar Horizon en un entorno de nube híbrida si se usa la arquitectura de pods en la nube (CPA) de Horizon para interconectar los centros de datos locales y de Azure. CPA escala verticalmente la implementación, crea una nube híbrida y proporciona redundancia para la continuidad empresarial y recuperación ante desastres.  Para más información, consulte [Expansión de los entornos de Horizon 7 existentes](https://techzone.vmware.com/resource/business-continuity-vmware-horizon#_Toc41650874).

>[!IMPORTANT]
>CPA no es una implementación extendida; cada pod de Horizon es distinto, y todos los servidores de conexión que pertenecen a cada uno de los pods individuales deben encontrarse en una única ubicación y ejecutarse en el mismo dominio de transmisión desde una perspectiva de red.

Al igual que un centro de datos local o privado, Horizon se puede implementar en una nube privada de Azure VMware Solution. En las secciones siguientes se describen las diferencias principales entre la implementación de Horizon en el entorno local y en Azure VMware Solution.

La nube privada de Azure es conceptualmente igual que el SDDC de VMware, un término que se usa habitualmente en la documentación de Horizon. En el resto de este documento se usan los términos nube privada de Azure y SDDC de VMware indistintamente.

El conector de nube de Horizon es necesario para que Horizon en Azure VMware Solution administre licencias de suscripción. El conector de nube se puede implementar en Azure Virtual Network junto con servidores de conexión de Horizon.

>[!IMPORTANT]
>La compatibilidad del plano de control de Horizon con Horizon en Azure VMware Solution todavía no está disponible. Asegúrese de descargar la versión de VHD del conector de nube de Horizon.

## <a name="vcenter-cloud-admin-role"></a>Rol de Administrador de nube de vCenter

Dado que Azure VMware Solution es un servicio de SDDC y Azure administra el ciclo de vida del SDDC en Azure VMware Solution, el modelo de permisos de vCenter en Azure VMware Solution está limitado por diseño.

Los clientes deben usar el rol de Administrador de nube, que tiene un conjunto limitado de permisos de vCenter. El producto Horizon se ha modificado para funcionar con el rol de Administrador de nube en Azure VMware Solution, en concreto:

* El aprovisionamiento de clones instantáneo se ha modificado para ejecutarse en Azure VMware Solution.

* Se ha creado una directiva específica de vSAN (VMware_Horizon) en Azure VMware Solution para funcionar con Horizon, que debe estar disponible y usarse en los SDDC implementados para Horizon.

* Content-Based Read Cache (CBRC) de vSphere, también conocido como View Storage Accelerator, se deshabilita al ejecutar en Azure VMware Solution.

>[!IMPORTANT]
>CBRC no se debe volver a activar.

>[!NOTE]
>Azure VMware Solution configura automáticamente valores específicos de Horizon siempre que se implemente Horizon 2006 (es decir, Horizon 8) y superior en la rama de Horizon 8 y se seleccione la opción **Azure** en el instalador del servidor de conexión de Horizon.

## <a name="horizon-on-azure-vmware-solution-deployment-architecture"></a>Arquitectura de implementación de Horizon en Azure VMware Solution

Un diseño de arquitectura típico de Horizon usa una estrategia de pods y bloques. Un bloque es un solo vCenter, mientras que varios bloques combinados forman un pod. Un pod de Horizon es una unidad de organización determinada por los límites de escalabilidad de Horizon. Cada pod de Horizon tiene un portal de administración independiente y, por tanto, un procedimiento de diseño estándar es minimizar el número de pods.

Cada nube tiene su propio esquema de conectividad de red. En combinación con NSX Edge o redes de SDDC de VMware, la conectividad de red de Azure VMware Solution presenta requisitos únicos para implementar Horizon que difieren del entorno local.

Cada nube privada de Azure y SDDC pueden controlar 4000 sesiones de escritorio o de aplicación, suponiendo que:

* El tráfico de las cargas de trabajo se alinea con el perfil de trabajo de la tarea LoginVSI.

* Solo se tiene en cuenta el tráfico de protocolos, sin datos de usuario.

* NSX Edge se configura para un tamaño grande.

>[!NOTE]
>El perfil de las cargas de trabajo y las necesidades pueden diferir, por lo que los resultados pueden variar en función del caso de uso. Los volúmenes de datos de usuario pueden reducir los límites de escala en el contexto de la carga de trabajo. Ajuste el tamaño de la implementación y planéela en consecuencia. Para obtener más información, vea las instrucciones de ajuste de tamaño en la sección [Ajuste del tamaño de los hosts de Azure VMware Solution para implementaciones de Horizon](#size-azure-vmware-solution-hosts-for-horizon-deployments).

Dado el límite máximo de la nube privada de Azure y SDDC, se recomienda una arquitectura de implementación en la que los servidores de conexión de Horizon y las puertas de enlace de acceso unificado (UAG) de VMware se ejecuten dentro de Azure Virtual Network. En la práctica, esto convierte a cada nube privada de Azure y SDDC en un bloque. Como resultado, se maximiza la escalabilidad de Horizon en Azure VMware Solution.

La conexión desde Azure Virtual Network a las nubes privadas de Azure o SDDC debe configurarse con FastPath de ExpressRoute. En el diagrama siguiente se muestra una implementación de pods de Horizon básica.

:::image type="content" source="media/horizon/horizon-pod-deployment-expresspath-fast-path.png" alt-text="Implementación típica de pods de Horizon mediante FastPath de ExpressRoute" border="false":::

## <a name="network-connectivity-to-scale-horizon-on-azure-vmware-solution"></a>Conectividad de red para escalar Horizon en Azure VMware Solution

En esta sección se presenta la arquitectura de red general con algunos ejemplos de implementación comunes para ayudarle a escalar Horizon en Azure VMware Solution. El contenido se centra específicamente en los elementos críticos de las redes. 

### <a name="single-horizon-pod-on-azure-vmware-solution"></a>Pod único de Horizon en Azure VMware Solution

:::image type="content" source="media/horizon/single-horizon-pod-azure-vmware-solution.png" alt-text="Pod único de Horizon en Azure VMware Solution" border="false":::

El escenario de implementación más directo es un único pod de Horizon, porque implementa solo un pod de Horizon en la región Este de EE. UU.  Puesto que se estima que cada nube privada y SDDC pueden controlar 4000 sesiones de escritorio, se implementa el tamaño máximo del pod de Horizon.  Puede planear la implementación de hasta tres nubes privadas o SDDC.

Con las máquinas virtuales de infraestructura de Horizon implementadas en Azure Virtual Network, puede llegar hasta 12 000 sesiones por pod de Horizon. La conexión entre cada nube privada o SDDC y Azure Virtual Network es una ruta rápida de ExpressRoute.  No se necesita tráfico Este-Oeste entre las nubes privadas. 

Las principales suposiciones de este ejemplo de implementación básica son que:

* No se tiene un pod de Horizon local que se quiera conectar a este nuevo pod mediante la arquitectura de pods en la nube (CPA).

* Los usuarios finales se conectan a sus escritorios virtuales mediante Internet (en lugar de hacerlo a través de un centro de datos local).

Puede conectar el controlador de dominio de AD de Azure Virtual Network con la instancia local de AD mediante VPN o un circuito de ExpressRoute.

Una variación del ejemplo básico podría ser admitir la conectividad de los recursos locales. Por ejemplo, los usuarios acceden a los escritorios y generan tráfico de aplicaciones de escritorio virtual o se conectan a un pod de Horizon local mediante CPA.

En el diagrama se muestra cómo admitir la conectividad de los recursos locales. Para conectar la red corporativa a Azure Virtual Network, necesitará un circuito de ExpressRoute.  También necesitará conectar la red corporativa con cada una de las nubes privadas y los SDDC mediante ExpressRoute Global Reach.  Permite la conectividad desde el SDDC al circuito de ExpressRoute y los recursos locales. 

:::image type="content" source="media/horizon/connect-corporate-network-azure-virtual-network.png" alt-text="Conexión de la red corporativa a una instancia de Azure Virtual Network" border="false":::

### <a name="multiple-horizon-pods-on-azure-vmware-solution-across-multiple-regions"></a>Varios pods de Horizon en Azure VMware Solution en varias regiones

Otro escenario es el escalado de Horizon en varios pods.  En este escenario, se implementan dos pods de Horizon en dos regiones diferentes y se federan mediante CPA. Es similar a la configuración de red del ejemplo anterior, pero con algunos vínculos cruzados entre regiones adicionales. 

Conectará la instancia de Azure Virtual Network de cada región a las nubes privadas o los SDDC de la otra región. Esto permite que los servidores de conexión de Horizon que forman parte de la federación de CPA se conecten a todos los escritorios que administran. La incorporación de nubes privadas o SDDC adicionales a esta configuración le permitiría escalar hasta 24 000 sesiones en total. 

Se aplican los mismos principios si se implementan dos pods de Horizon en la misma región.  Asegúrese de implementar el segundo pod de Horizon en una instancia independiente de *Azure Virtual Network*. Al igual que en el ejemplo de un único pod, puede conectar la red corporativa y el pod local a este ejemplo de varios pods o regiones mediante ExpressRoute y Global Reach. 

:::image type="content" source="media/horizon/multiple-horizon-pod-azure-vmware-solution.png" alt-text="Varios pods de Horizon en Azure VMware Solution en varias regiones" border="false":::

## <a name="size-azure-vmware-solution-hosts-for-horizon-deployments"></a>Ajuste del tamaño de los hosts de Azure VMware Solution para implementaciones de Horizon 

La metodología de dimensionamiento de Horizon en un host que se ejecuta en Azure VMware Solution es más sencilla que en Horizon local.  Esto se debe a que el host de Azure VMware Solution está estandarizado.  El dimensionamiento exacto del host ayuda a determinar el número de hosts necesarios para admitir los requisitos de VDI.  Es fundamental para determinar el costo por escritorio.

### <a name="azure-vmware-solution-host-instance"></a>Instancia de host de Azure VMware Solution

* Servidor PowerEdge R640 - RESTRINGIDO DSS

* 36 núcleos \@2,3 GHz

* 576 GB de RAM

* Controlador SAS HBA HBA330 de 12 GB/s (NO RAID)

* SSD SATA de 1,92 TB, uso combinado, 6 GB/s, 512e, 2,5", de conexión en marcha, unidad AG, 3 DWPD, 10 512 TBW

* Intel 1,6 TB, NVMe, uso combinado, Express Flash, 2,5", unidad SFF, U.2, P4600 con operador

* 2 grupos de discos vSAN: 1,6 x 4 (1,92 TB)

### <a name="horizon-sizing-inputs"></a>Entradas de ajuste de tamaño de Horizon

Esto es lo que necesitará recopilar para la carga de trabajo planeada:

* Número de escritorios simultáneos

* vCPU requerida por escritorio

* vRAM requerida por escritorio

* Almacenamiento requerido por escritorio

En general, las implementaciones de VDI están restringidas por CPU o RAM, lo que determina el tamaño del host. Veamos el siguiente ejemplo de un tipo de trabajador del conocimiento LoginVSI de carga de trabajo validado con pruebas de rendimiento:

* Implementación de 2000 escritorios simultáneos

* 2 vCPU por escritorio

* 4 GB de vRAM por escritorio.

* 50 GB de almacenamiento por escritorio

En este ejemplo, el número total de hosts asciende a 18, lo que resulta en una densidad de VM por host de 111.

>[!IMPORTANT]
>Las cargas de trabajo de cliente varían de este ejemplo de un trabajador del conocimiento LoginVSI. Como parte del plan de la implementación, trabaje con los SE de VMware EUC sobre sus necesidades específicas de tamaño y rendimiento. Asegúrese de ejecutar sus propias pruebas de rendimiento con la carga de trabajo real planeada antes de finalizar el ajuste de tamaño del host y ajuste en consecuencia.

## <a name="horizon-on-azure-vmware-solution-licensing"></a>Licencias de Horizon en Azure VMware Solution 

Hay cuatro componentes en los costos generales de ejecución de Horizon en Azure VMware Solution. 

### <a name="azure-vmware-solution-capacity-cost"></a>Costo de capacidad de Azure VMware Solution

Para obtener información sobre los precios, vea la página [Precios de Azure VMware Solution](https://azure.microsoft.com/pricing/details/azure-vmware/)

### <a name="horizon-licensing-cost"></a>Costo de licencias de Horizon

Hay dos licencias disponibles para su uso con Azure VMware Solution, Usuario simultáneo (CCU) o Usuario con nombre (NU):

* Licencia de suscripción de Horizon

* Licencia de suscripción Universal de Horizon

Si solo implementa Horizon en Azure VMware Solution para el futuro inmediato, use la licencia de suscripción de Horizon, ya que su costo es inferior.

Si se ha implementado en Azure VMware Solution y en el entorno local, como en el caso de uso de recuperación ante desastres, elija la licencia de suscripción Universal de Horizon. Incluye una licencia de vSphere para la implementación local, por lo que tiene un costo mayor.

Trabaje con el equipo de ventas de VMware EUC para determinar el costo de la licencia de Horizon en función de sus necesidades.

### <a name="cost-of-the-horizon-infrastructure-vms-on-azure-virtual-network"></a>Costo de las máquinas virtuales de infraestructura de Horizon en Azure Virtual Network

En función de la arquitectura de implementación estándar, las máquinas virtuales de infraestructura de Horizon se componen de servidores de conexión, UAG y administradores de volúmenes de aplicaciones. Se implementan en la instancia de Azure Virtual Network del cliente. Se necesitan instancias nativas de Azure adicionales para admitir servicios de alta disponibilidad (HA), Microsoft SQL o Microsoft Active Directory (AD) en Azure. En la tabla se enumeran las instancias de Azure en función de un ejemplo de implementación de 2000 escritorios. 

>[!NOTE]
>Para poder controlar los errores, implemente un servidor más de los necesarios para el número de conexiones (n+1). El número mínimo recomendado de instancias del servidor de conexión, UAG y el administrador de volúmenes de aplicaciones es 2, y el número requerido aumentará en función de la cantidad de usuarios admitidos por el entorno.  Un solo servidor de conexión admite un máximo de 4000 sesiones, aunque se recomiendan 2000 como procedimiento recomendado. Se admiten hasta siete servidores de conexión por pod, con una recomendación de 12 000 sesiones activas en total por pod. Para obtener los números más recientes, consulte el [artículo de base de conocimientos de VMware sobre los límites y recomendaciones de tamaño de VMware Horizon 7](https://kb.vmware.com/s/article/2150348).

| Componente de infraestructura de Horizon | Instancia de Azure | Número de instancias necesarias (para 2000 escritorios)    | Comentario  |
|----------------------------------|----------------|----------------------------------------------------|----------|
| Servidor de conexión                | D4sv3          | 2       | *Consulte la nota anterior*                         |    
| UAG                              | F2sv2          | 2       | *Consulte la nota anterior*                         |
| Administrador de volúmenes de aplicaciones              | D4sv3          | 2       | *Consulte la nota anterior*                         |
| Conector de nube                  | D4sv3          | 1       |                                          |
| Controlador de AD                    | D4sv3          | 2       | *Opción de usar el servicio MSFT AD en Azure* |
| Base de datos MS-SQL                  | D4sv3          | 2       | *Opción de usar el servicio SQL en Azure*     |
| Recurso compartido de archivos de Windows               | D4sv3          |         | *Opcional*                               |

El costo de las máquinas virtuales de infraestructura es de \$0,36 por usuario y mes para la implementación de 2000 escritorios del ejemplo anterior. En este ejemplo se usan los precios de junio de 2020 para instancias de Azure en la región Este de EE. UU. Los precios pueden variar en función de la región, las opciones seleccionadas y el tiempo.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre VMware Horizon en Azure VMware Solution, lea el artículo de [preguntas frecuentes sobre VMware Horizon](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/products/horizon/vmw-horizon-on-microsoft-azure-vmware-solution-faq.pdf).
