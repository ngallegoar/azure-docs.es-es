---
title: 'Tutorial: implementación y configuración de VMware HCX'
description: Aprenda a implementar y configurar una solución VMware HCX para la nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 424abeef567d88f7de37f7a7a4ab7a7a8b6ef3bc
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791416"
---
# <a name="deploy-and-configure-vmware-hcx"></a>Implementación y configuración de VMware HCX

En este artículo se describen los procedimientos para implementar y configurar el conector local de VMware HCX para la nube privada de Azure VMWare Solution. Con VMware HCX, es posible migrar las cargas de trabajo de VMware a Azure VMware Solution y a otros sitios conectados mediante varios tipos de migración. Dado que Azure VMware Solution implementa y configura el Administrador de la nube de HCX, debe descargar, activar y configurar el conector de HCX en el centro de datos de VMware local.

El conector avanzado de VMware HCX está implementado previamente en Azure VMware Solution. Admite hasta tres conexiones de sitio (de local a la nube o de nube a nube). Si necesita más de tres conexiones de sitio, envíe una [solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support) para habilitar el complemento [VMware HCX Enterprise](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/). El complemento está actualmente en versión preliminar. 

>[!NOTE]
>VMware HCX Enterprise Edition (EE) está disponible con Azure VMware Solution como un servicio en versión preliminar. Es gratuito y está sujeto a los términos y condiciones de un servicio en versión preliminar. Una vez que el servicio VMware HCX EE esté disponible de forma general, recibirá un aviso de 30 días de que la facturación cambiará. También tendrá la opción de desactivar o no participar en el servicio. Tenga en cuenta que, actualmente, no hay ninguna forma sencilla de pasar de HCX Enterprise a HCX Advanced; los clientes que decidan cambiar a una versión anterior tendrán que volver a implementarlo, con el tiempo de inactividad que conlleva.

Revise primero detenidamente las secciones [Antes de comenzar](#before-you-begin), [Requisitos de versión de software](#software-version-requirements) y [Requisitos previos](#prerequisites) de este artículo. 

A continuación, examinaremos todos los procedimientos necesarios para:

> [!div class="checklist"]
> * Implementar el archivo OVA (conector de HCX) de VMware HCX local.
> * Activar el conector de VMware HCX.
> * Emparejar el conector de HCX local con el Administrador de la nube de HCX de Azure VMware Solution.
> * Configurar la interconexión (perfil de red, perfil de proceso y malla de servicios).
> * Para completar la instalación, compruebe el estado del dispositivo y valide que la migración es posible.

Una vez finalizado, puede seguir los pasos siguientes recomendados al final de este artículo.  

## <a name="before-you-begin"></a>Antes de empezar
   
* Revise la [serie de tutoriales](tutorial-network-checklist.md) básicos de Software Defined Datacenter (SDDC) de Azure VMware Solution.
* Revise y consulte la [documentación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/index.html), incluida la guía de usuario de HCX.
* Revise [Migración de máquinas virtuales con VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) en la documentación de VMware.
* También, revise [Consideraciones de implementación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html).
* Opcionalmente, revise los materiales relacionados de VMware sobre HCX, como la [serie de blogs](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) de VMware vSphere. 
* De forma opcional, puede solicitar la activación de HCX Enterprise para Azure VMware Solution a través de los canales de soporte técnico de Azure VMware Solution.
* Opcionalmente, [revise los puertos de red necesarios para HCX](https://ports.vmware.com/home/VMware-HCX).
* Aunque el Administrador de la nube de HCX de Azure VMware Solution viene preconfigurado a partir de la red /22 que se proporciona para la nube privada de Azure VMware Solution, el conector local de HCX requiere que asigne intervalos de red desde su red local. Estos redes e intervalos se describen más adelante en este artículo.

El ajuste de tamaño de las cargas de trabajo en los recursos de proceso y de almacenamiento es un paso de planeación esencial. Aborde el paso de ajuste de tamaño como parte del planeamiento inicial del entorno de la nube privada. 

Puede ajustar el tamaño de las cargas de trabajo; para ello, complete una [evaluación de Azure VMware Solution](../migrate/how-to-create-azure-vmware-solution-assessment.md) en el portal de Azure Migrate.

## <a name="software-version-requirements"></a>Requisitos de versión de software

Los componentes de la infraestructura deben ejecutar la versión mínima necesaria. 
                                                         
| Tipo de componente    | Requisitos del entorno de origen    | Requisitos del entorno de destino   |
| --- | --- | --- |
| vCenter Server   | 5,1<br/><br/>Si usa la versión 5.5 U1 o versiones anteriores, emplee la interfaz de usuario de HCX independiente para las operaciones de HCX.  | 6.0 U2 y versiones posteriores   |
| ESXi   | 5.0    | ESXi 6.0 y versiones posteriores   |
| NSX    | Para la extensión de red de HCX de los conmutadores lógicos en el origen: NSXv 6.2+ o NSX-T 2.4+.   | NSXv 6.2+ o NSX-T 2.4+<br/><br/>Para el enrutamiento de proximidad de HCX: NSXv 6.4+. (No se admite el enrutamiento de proximidad con NSX-T). |
| vCloud Director   | No es necesario. No hay interoperabilidad con vCloud Director en el sitio de origen. | Al integrar el entorno de destino con vCloud Director, la versión mínima es la 9.1.0.2.  |

## <a name="prerequisites"></a>Requisitos previos

### <a name="network-and-ports"></a>Red y puertos

* Configure [Global Reach de Azure ExpressRoute](tutorial-expressroute-global-reach-private-cloud.md) entre los circuitos de ExpressRoute del SDDC de Azure VMware Solution y del entorno local.

* [Todos los puertos necesarios](https://ports.vmware.com/home/VMware-HCX) deben estar abiertos para realizar la comunicación entre los componentes locales y el SDDC de Azure VMware Solution.

Para más información, consulte la [documentación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html).


### <a name="ip-addresses"></a>Direcciones IP

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-connector-ova-on-premises"></a>Implementación del archivo OVA del conector de VMware HCX local

> [!NOTE]
> Antes de implementar el dispositivo virtual en la instancia local de vCenter, debe descargar el archivo OVA del conector de VMware HCX. 

1. Abra una ventana del explorador, inicie sesión en el administrador de HCX de Azure VMware Solution en el puerto 443 en `https://x.x.x.9` con las credenciales del usuario **cloudadmin** y, a continuación, vaya a **Support** (Soporte técnico).

   > [!TIP]
   > Anote la dirección IP del Administrador de la nube de HCX en Azure VMware Solution. Para identificar la dirección IP, en el panel de Azure VMware Solution, vaya a **Manage** > **Connectivity** (Administrar > Conectividad) y, a continuación, seleccione la pestaña **HCX**. 
   >
   >La contraseña de vCenter se definió al configurar la nube privada.

1. Seleccione el vínculo **Download** (Descargar) para descargar el archivo OVA del conector de VMware HCX.

1. Vaya a la instancia local de vCenter. Seleccione una plantilla OVF, que es el archivo OVA que descargó, para implementar el conector de HCX en la instancia local de vCenter.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="Captura de pantalla de la búsqueda de una plantilla OVF." lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. Seleccione un nombre y una ubicación y seleccione el recurso o el clúster en el que va a implementar el conector de HCX. A continuación, revise los detalles y los recursos necesarios.  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="Captura de pantalla de la revisión de los detalles de la plantilla." lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. Revise los términos de la licencia. Si está de acuerdo con ellos, seleccione el almacenamiento y la red necesarios y, a continuación, seleccione **Next** (Siguiente).

1. En **Customize template** (Personalizar plantilla), escriba toda la información necesaria. 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="Captura de pantalla de los cuadros para personalizar una plantilla." lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. Seleccione **Next** (Siguiente), verifique la configuración y, a continuación, seleccione **Finish** (Finalizar) para implementar el archivo OVA del conector de HCX.
     
   > [!NOTE]
   > Por lo general, el conector de VMware HCX que va a implementar ahora se implementa en la red de administración del clúster.  
   
   > [!IMPORTANT]
   > Es posible que tenga que activar el dispositivo virtual manualmente.  Si es el caso, espere de 10 a 15 minutos antes de continuar con el paso siguiente.

Puede encontrar información de un extremo a otro de este procedimiento en el vídeo [Azure VMware Solution: Implementación del dispositivo HCX](https://www.youtube.com/embed/BwSnQeefnso). 


## <a name="activate-vmware-hcx"></a>Activación de VMware HCX

Después de implementar el archivo OVA del conector de VMware HCX en el entorno local e iniciar el dispositivo, está listo para realizar la activación. En primer lugar, debe obtener una clave de licencia en el portal de Azure VMware Solution.

1. En el portal de Azure VMware Solution, vaya a **Manage** > **Connectivity** (Administrar > Conectividad), seleccione la pestaña **HCX** y, a continuación, seleccione **Add** (Agregar).

1. Use las credenciales del **administrador** para iniciar sesión en el administrador de VMware HCX local en `https://HCXManagerIP:9443`. 

   > [!IMPORTANT]
   > Asegúrese de incluir el número de puerto `9443` con la dirección IP de VMware HCX Manager.

1. En **Licensing** (Concesión de licencias), escriba la clave en **HCX Advanced Key** (Clave de HCX Advanced).  
   
    > [!NOTE]
    > El administrador de VMware HCX debe tener acceso abierto a Internet o un servidor proxy configurado.

1. En la **Datacenter Location** (Ubicación del centro de datos), proporcione la ubicación más cercana donde va a instalar el administrador de VMware HCX.

1. En **System Name** (Nombre del sistema), modifique el nombre o acepte el valor predeterminado.
   
1. Seleccione **Yes, Continue** (Sí, continuar).
    
1. En **Connect your vCenter** (Conectar vCenter), proporcione el FQDN o la dirección IP del servidor de vCenter y las credenciales adecuadas y, a continuación, seleccione **Continuar**.
   
1. En **Configure SSO/PSC** (Configurar SSO/PSC), proporcione el FQDN o la dirección IP de Platform Services Controller y, a continuación, seleccione **Continue** (Continuar).
   
   >[!NOTE]
   >Normalmente, esta entrada es la misma que la dirección IP o el FQDN de la instancia de vCenter.

1. Compruebe que todas las entradas sean correctas y seleccione **Restart** (Reiniciar).
    
   > [!NOTE]
   > Verá que el proceso se ralentiza después de reiniciar y antes de que se le solicite realizar el siguiente paso.

Una vez que se reinicien los servicios, es fundamental que el modo de vCenter se muestre en verde en la pantalla que aparecerá. Tanto vCenter como SSO deben tener los parámetros de configuración adecuados, que deben ser los mismos que los de la pantalla anterior.

:::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="Captura de pantalla del panel con el estado de vCenter en color verde." lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

Puede encontrar información de un extremo a otro de este procedimiento en el vídeo [Azure VMware Solution: Activación de HCX](https://www.youtube.com/embed/BkAV_TNYxdE).


## <a name="configure-the-vmware-hcx-connector"></a>Configuración del conector de VMware HCX

Ya está listo para agregar un emparejamiento de sitios, crear un perfil de red y de proceso y habilitar servicios como la migración, la extensión de red o la recuperación ante desastres. 

### <a name="add-a-site-pairing"></a>Agregar un emparejamiento de sitios

Puede conectar (emparejar) el Administrador de la nube de VMware HCX de Azure VMware Solution con el conector de VMware HCX del centro de datos. 

1. Inicie sesión en la instancia local de vCenter y en **Inicio** , seleccione **HCX**.

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="Captura de pantalla del cliente de vCenter con HCX seleccionado entre los accesos directos." lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. En **Infrastructure** (Infraestructura), seleccione **Site Pairing** (Emparejamiento de sitios) y, a continuación, seleccione la opción **Connect To Remote Site** (Conectar con el sitio remoto) que se encuentra en medio de la pantalla. 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="Captura de pantalla de las selecciones para crear un sitio remoto." lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. Escriba la dirección IP o la dirección URL de la instancia remota de HCX que anotó anteriormente, el nombre de usuario cloudadmin@vsphere.local y la contraseña de Azure VMware Solution. A continuación, seleccione **Conectar**.

   > [!NOTE]
   > Para establecer un emparejamiento de sitios correctamente, el conector de HCX se debe poder enrutar a la dirección IP del Administrador de la nube de HCX en el puerto 443.
   >
   > La contraseña es la misma que usó para iniciar sesión en vCenter. Recuerde que definió esta contraseña en la pantalla de implementación inicial.

   Verá una pantalla que muestra el Administrador de la nube de HCX en Azure VMware Solution y el conector de HCX local conectados (emparejados).

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Captura de pantalla que muestra el emparejamiento del Administrador de HCX en Azure VMware Solution y el conector de HCX.":::

Puede encontrar información de un extremo a otro de este procedimiento en el vídeo [Azure VMware Solution: Emparejamiento de sitios de HCX](https://www.youtube.com/embed/sKizDCRHOko).



### <a name="create-network-profiles"></a>Creación de perfiles de red

VMware HCX implementa un subconjunto de aplicaciones virtuales (automatizadas) que requieren varios segmentos IP. Al crear los perfiles de red, se definen los segmentos IP que se identificaron durante la [fase de preparación y planeación previas a la implementación de los segmentos de red de VMware HCX](production-ready-deployment-steps.md#vmware-hcx-network-segments).

Va a crear cuatro perfiles de red:

   - Administración
   - vMotion
   - Replicación
   - Vínculo superior

1. En **Infrastructure** (Infraestructura), seleccione **Interconnect** > **Multi-Site Service Mesh** > **Network Profiles** > **Create Network Profile** (Interconectar > Malla de servicios de varios sitios > Perfiles de red > Crear perfil de red).

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="Captura de pantalla de las selecciones para empezar a crear un perfil de red." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. En cada perfil de red, seleccione la red y el grupo de puertos, proporcione un nombre y cree el grupo de direcciones IP de ese segmento. Seleccione **Crear**. 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="Captura de pantalla de los detalles de un nuevo perfil de red.":::

Puede encontrar información de un extremo a otro de este procedimiento en el vídeo [Azure VMware Solution: Perfil de red de HCX](https://www.youtube.com/embed/NhyEcLco4JY).


### <a name="create-a-compute-profile"></a>Creación de un perfil de proceso

1. Seleccione **Compute Profiles** > **Create Compute Profile** (Perfiles de proceso > Crear perfil de proceso).

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="Captura de pantalla que muestra las selecciones para empezar a crear un perfil de proceso." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png":::

1. Escriba un nombre para el perfil y seleccione **Continuar**.  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="Captura de pantalla que muestra la entrada de un nombre de perfil de proceso y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Seleccione los servicios que se van a habilitar, como la migración, la extensión de red o la recuperación ante desastres y, a continuación, seleccione **Continue** (Continuar).
  
   > [!NOTE]
   > Por lo general, no es necesario cambiar nada.

1. En **Select Service Resources** (Seleccionar recursos de servicio), seleccione uno o más recursos de servicio (clústeres) para habilitar los servicios de VMware HCX seleccionados.  

1. Cuando vea los clústeres en el centro de datos local, seleccione **Continuar**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="Captura de pantalla que muestra los recursos de servicio seleccionados y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. En **Select Datastore** (Seleccionar almacén de datos), seleccione el recurso de almacenamiento del almacén de datos para implementar los dispositivos de interconexión de VMware HCX. Después, seleccione **Continuar**.

   Cuando se seleccionan varios recursos, VMware HCX usa el primero seleccionado hasta que se agota su capacidad.   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="Captura de pantalla que muestra un recurso de almacenamiento de datos seleccionado y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. En **Select Management Network Profile** (Seleccionar perfil de red de administración), seleccione el perfil de red de administración que creó en los pasos anteriores. Después, seleccione **Continuar**.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="Captura de pantalla que muestra la selección de un perfil de red de administración y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > El perfil de red de administración permite a los dispositivos de VMware HCX comunicarse con vCenter. Se puede acceder a los hosts ESXi mediante este perfil.

1. En **Select Uplink Network Profile** (Seleccionar perfil de red de vínculo superior), seleccione el perfil de red de vínculo superior que creó en los pasos anteriores. Después, seleccione **Continuar**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="Captura de pantalla que muestra la selección de un perfil de red de vínculo superior y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. En **Select vMotion Network Profile** (Seleccionar perfil de red de vMotion), seleccione el perfil de red que creó en los pasos anteriores. Después, seleccione **Continuar**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="Captura de pantalla que muestra la selección de un perfil de red de vMotion y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. En **Select vSphere Replication Network Profile** (Seleccionar perfil de red de replicación de vSphere), seleccione el perfil de red que creó en los pasos anteriores. Después, seleccione **Continuar**.

   En la mayoría de los casos, el perfil de red de replicación es el mismo que el perfil de red de administración.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="Captura de pantalla que muestra la selección de un perfil de red de replicación y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. En **Select Distributed Switches for Network Extensions** (Seleccionar los conmutadores distribuidos para las extensiones de red), seleccione los conmutadores virtuales distribuidos que contienen las máquinas virtuales que se van a migrar a Azure VMware Solution en una red extendida de capa 2. Después, seleccione **Continuar**.

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="Captura de pantalla que muestra la selección de los conmutadores virtuales distribuidos y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Revise las reglas de conexión y seleccione **Continue** (Continuar).  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="Captura de pantalla que muestra las reglas de conexión y el botón Continue (Continuar)." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. Seleccione **Finish** (Finalizar) para crear el perfil de proceso.


   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="Captura de pantalla que muestra información del perfil de proceso." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

Puede encontrar información de un extremo a otro de este procedimiento en el vídeo [Azure VMware Solution: Perfil de proceso](https://www.youtube.com/embed/qASXi5xrFzM).

### <a name="create-a-service-mesh"></a>Creación de una malla de servicios

En este paso debe configurar una malla de servicios entre el entorno local y el SDDC de Azure VMware Solution.

   > [!NOTE]
   > Para establecer correctamente una malla de servicios con Azure VMware Solution:
   >
   > Los puertos UDP 500/4500 están abiertos entre las direcciones del perfil de red de "vínculo superior" definidas por el conector de HCX local y las direcciones del perfil de red de "vínculo superior" de la nube de HCX de Azure VMware Solution.
   >
   > Asegúrese de revisar [Puertos necesarios de HCX](https://ports.vmware.com/home/VMware-HCX).

1. En la opción **Infrastructure** (Infraestructura), seleccione **Interconnect** > **Service Mesh** > **Create Service Mesh** (Interconexión > Malla de servicios > Crear malla de servicios).    

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="Captura de pantalla de las selecciones para empezar a crear una malla de servicios." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Revise los sitios que se han rellenado previamente y, a continuación, seleccione **Continuar**. 

   >[!NOTE]
   >Si esta es la primera configuración de la malla de servicios, no tendrá que modificar esta pantalla.  

1. Seleccione los perfiles de proceso de origen y remoto en las listas desplegables y, a continuación, seleccione **Continue** (Continuar).  

   Las selecciones definen los recursos donde las VM pueden consumir los servicios de VMware HCX.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="Captura de pantalla que muestra la selección del perfil de proceso de origen." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="Captura de pantalla que muestra la selección del perfil de proceso remoto." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Revise los servicios que se van a habilitar y, a continuación, seleccione **Continue** (Continuar).  

1. En **Advanced Configuration - Override Uplink Network profiles** (Configuración avanzada: Invalidar los perfiles de red de vínculo superior), seleccione **Continue** (Continuar).  

   Los perfiles de red de vínculo superior se conectan a la red a través de la cual se puede acceder a los dispositivos de interconexión del sitio remoto.  
  
1. En **Advanced Configuration - Network Extension Appliance Scale Out** (Configuración avanzada: escalabilidad horizontal del dispositivo de extensión de red), revise la información y seleccione **Continuar**. 

1. En **Advanced Configuration - Traffic Engineering** (Configuración avanzada: ingeniería de tráfico), revise y realice las modificaciones que considere necesarias y, a continuación, seleccione **Continue** (Continuar).

1. Revise la vista previa de la topología y seleccione **Continue** (Continuar).

1. A continuación, escriba un nombre descriptivo para esta malla de servicios y seleccione **Finalizar** para terminar el proceso.  

1. Seleccione **Ver tareas** para supervisar la implementación. 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="Captura de pantalla que muestra el botón para ver las tareas.":::

   Una vez que la implementación de la malla de servicios finalice correctamente, verá los servicios en color verde.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="Captura de pantalla que muestra indicadores verdes en los servicios." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Para comprobar el estado de la malla de servicios, compruebe el estado del dispositivo. 
1. Seleccione **Interconnect** > **Appliances** (Interconectar > Dispositivos).

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="Captura de pantalla que muestra las selecciones para comprobar el estado del dispositivo." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

Puede encontrar información de un extremo a otro de este procedimiento en el vídeo [Azure VMware Solution: Malla de servicios](https://www.youtube.com/embed/FyZ0d3P_T24).



### <a name="optional-create-a-network-extension"></a>(Opcional) Creación de una extensión de red

Si quiere extender las redes de su entorno local a Azure VMware Solution, siga estos pasos:

1. En **Servicios** , seleccione **Extensión de red** y, a continuación, seleccione **Create a Network Extension** (Crear una extensión de red).

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="Captura de pantalla que muestra las selecciones para empezar a crear una extensión de red." lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. Seleccione cada una de las redes que quiere extender a Azure VMware Solution y, a continuación, seleccione **Next** (Siguiente).

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="Captura de pantalla que muestra la selección de una red.":::

1. Escriba la dirección IP de puerta de enlace local para cada una de las redes que va a extender y, a continuación, seleccione **Enviar**. 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="Captura de pantalla que muestra la entrada de una dirección IP de puerta de enlace.":::

   La extensión de red tardará unos minutos en finalizar. Cuando lo haga, verá que el estado cambia a **Extension complete** (Extensión completada).

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="Captura de pantalla que muestra el estado de extensión completada." lightbox="media/tutorial-vmware-hcx/extension-complete.png":::

Puede encontrar información completa de este paso en el vídeo [Azure VMware Solution: Extensión de red](https://www.youtube.com/embed/cNlp0f_tTr0).


## <a name="next-steps"></a>Pasos siguientes

Si ha llegado a este punto y el estado del túnel de interconexión del dispositivo es **UP** (Activo) y de color verde, está listo para migrar y proteger las máquinas virtuales de Azure VMware Solution con VMware HCX. Azure VMware Solution admite migraciones de cargas de trabajo (con o sin una extensión de red). Todavía puede migrar cargas de trabajo en el entorno de vSphere, junto con la creación local de redes y la implementación de máquinas virtuales en esas redes.  

Para más información sobre el uso de HCX, consulte la documentación técnica de VMware:

* [Documentación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/index.html)
* [Migración de máquinas virtuales con VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* [Puertos necesarios de HCX](https://ports.vmware.com/home/VMware-HCX)
