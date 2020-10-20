---
title: 'Tutorial: implementación y configuración de VMware HCX'
description: Aprenda a implementar y configurar la solución VMware HCX para la nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.date: 10/02/2020
ms.openlocfilehash: 58fd8b4518f60f1f736d8c19ddcf62729353f251
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058002"
---
# <a name="deploy-and-configure-vmware-hcx"></a>Implementación y configuración de VMware HCX

En este artículo se describen los procedimientos para implementar y configurar el "conector" local de VMware HCX para la nube privada de Azure VMWare Solution. VMware HCX permite realizar la migración de las cargas de trabajo de VMware a Azure VMware Solution y a otros sitios conectados a través de varios tipos de migración. Azure VMware Solution ya ha implementado y configurado el "Administrador de la nube", por lo que el cliente necesita descargar, activar y configurar el "conector" en el centro de datos de VMware local.

El conector avanzado de VMware HCX (que está implementado de antemano en Azure VMware Solution) admite hasta tres conexiones de sitio (de local a la nube o de nube a nube). Si son necesarias más de tres conexiones de sitio, los clientes tienen la opción de habilitar el complemento [VMware HCX Enterprise](https://cloud.vmware.com/community/2019/08/08/introducing-hcx-enterprise/) (que se encuentra actualmente en su *versión preliminar*), mediante el envío de una [solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support). VMware HCX Enterprise Edition (EE) está disponible con Azure VMware Solution, como una función o servicio en *versión preliminar*. Aunque VMware HCX EE para Azure VMware Solution esté en su *versión preliminar*, es una función o servicio gratuito y está sujeto a los términos y condiciones de los servicios de versión preliminar. Una vez que el servicio VMware HCX EE esté disponible de forma general, recibirá un aviso de 30 días de que la facturación cambiará. También tendrá la opción de desactivar o no participar en el servicio.

Antes de nada, revise primero detenidamente las secciones [Antes de comenzar](#before-you-begin), [Requisitos de versión de software](#software-version-requirements) y [Requisitos previos](#prerequisites). 

A continuación, examinaremos todos los procedimientos necesarios para:

> [!div class="checklist"]
> * Implementar el archivo OVA (conector de HCX) de VMware HCX local
> * Activar el conector de VMware HCX
> * Emparejar el conector de HCX local con el Administrador de la nube de HCX de Azure VMware Solution
> * Configurar la interconexión (perfil de red, perfil de proceso y malla de servicios)
> * Es posible completar la instalación mediante la comprobación del estado del dispositivo y la validación de la migración.

Una vez finalizado, puede seguir los pasos siguientes recomendados al final de este artículo.  

## <a name="before-you-begin"></a>Antes de empezar
   
* Revise la [serie de tutoriales](tutorial-network-checklist.md) básicos de Software Defined Datacenter (SDDC) de Azure VMware Solution.
* Revise y consulte la [documentación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/index.html), incluida la guía de usuario de HCX.
* Revise los documentos de VMware [Migración de máquinas virtuales con VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g).
* También, revise [Consideraciones de implementación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/install-checklist/GUID-C0A0E820-D5D0-4A3D-AD8E-EEAA3229F325.html).
* Opcionalmente, revise los materiales relacionados de VMware en HCX, como la [serie de blogs](https://blogs.vmware.com/vsphere/2019/10/cloud-migration-series-part-2.html) de VMware vSphere en HCX. 
* De forma opcional, puede solicitar la activación de HCX Enterprise para Azure VMware Solution a través de los canales de soporte técnico de Azure VMware Solution.
* Opcionalmente, [revise los puertos de red necesarios para HCX](https://ports.vmware.com/home/VMware-HCX).
* Aunque el Administrador de la nube de HCX de Azure VMware Solution viene preconfigurado a partir del intervalo /22 que se proporciona para la nube privada de Azure VMware Solution, el conector local de HCX requiere que el cliente asigne intervalos de red desde su red local. Estas redes e intervalos se describen más adelante en el documento.

El ajuste de tamaño de las cargas de trabajo en los recursos de proceso y de almacenamiento es un paso de planeación esencial. Aborde el paso de ajuste de tamaño como parte del planeamiento inicial del entorno de la nube privada. 

También puede ajustar el tamaño de las cargas de trabajo si completa una [evaluación de Azure VMware Solution](../migrate/how-to-create-azure-vmware-solution-assessment.md) en el portal de Azure Migrate.

## <a name="software-version-requirements"></a>Requisitos de versión de software

Los componentes de la infraestructura deben ejecutar la versión mínima necesaria. 
                                                         
| Tipo de componente    | Requisitos del entorno de origen    | Requisitos del entorno de destino   |
| --- | --- | --- |
| vCenter Server   | 5,1<br/><br/>Si usa 5.5 U1 o versiones anteriores, emplee la interfaz de usuario de HCX independiente para las operaciones de HCX.  | 6.0 U2 y versiones posteriores   |
| ESXi   | 5.0    | ESXi 6.0 y versiones posteriores   |
| NSX    | Para la extensión de red de HCX de conmutadores lógicos en el origen: NSXv 6.2+ o NSX-T 2.4+   | NSXv 6.2+ o NSX-T 2.4+<br/><br/>Para el enrutamiento de proximidad de HCX: NSXv 6.4 + (el enrutamiento de proximidad no se admite con NSX-T) |
| vCloud Director   | No necesario: sin interoperabilidad con vCloud Director en el sitio de origen | Al integrar el entorno de destino con vCloud Director, la versión mínima es 9.1.0.2.  |

## <a name="prerequisites"></a>Requisitos previos

### <a name="network-and-ports"></a>Red y puertos

* Se debe configurar [Global Reach de ExpressRoute](tutorial-expressroute-global-reach-private-cloud.md) entre los circuitos de ExpressRoute del SDDC de Azure VMware Solution y del entorno local.

* Todos los puertos necesarios deben estar abiertos para realizar la comunicación entre los componentes locales y la versión local del SDDC de Azure VMware Solution (consulte la [documentación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-E456F078-22BE-494B-8E4B-076EF33A9CF4.html)).


### <a name="ip-addresses"></a>Direcciones IP

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]
   
## <a name="deploy-the-vmware-hcx-connector-ova-on-premises"></a>Implementación del archivo OVA del conector de VMware HCX local

>[!NOTE]
>Antes de implementar el dispositivo virtual en la instancia local de vCenter, deberá descargar el archivo OVA del conector de VMware HCX. 

1. Abra una ventana del explorador, inicie sesión en el administrador de HCX de Azure VMware Solution en el puerto 443 en `https://x.x.x.9` con las credenciales del usuario **cloudadmin** y, a continuación, vaya a **Support** (Soporte técnico).

   >[!TIP]
   >Anote la dirección IP del Administrador de la nube de HCX en Azure VMware Solution. Para identificar la dirección IP del Administrador de la nube de HCX, vaya a la hoja Azure VMware Solution, vaya a **Manage** > **Connectivity** (Administrar > Conectividad) y, a continuación, seleccione la pestaña **HCX**. 
   >
   >La contraseña de vCenter se definió al configurar la nube privada.

1. Seleccione el vínculo **Download** (Descargar) para descargar el archivo OVA del conector de VMware HCX.

1. Vaya a la instancia local de vCenter y seleccione una plantilla OVF, que es el archivo OVA que descargó, para implementar el conector de HCX en la instancia local de vCenter.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-ovf-template.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-ovf-template.png":::


1. Seleccione un nombre y una ubicación, un recurso o un clúster en el que va a implementar el conector de HCX y, a continuación, revise los detalles y los recursos necesarios.  

   :::image type="content" source="media/tutorial-vmware-hcx/configure-template.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/configure-template.png":::

1. Revise los términos de licencia y, si está de acuerdo con ellos, seleccione el almacenamiento y la red necesarios y, a continuación, seleccione **Siguiente**.

1. En **Customize template** (Personalizar plantilla), escriba toda la información necesaria. 

   :::image type="content" source="media/tutorial-vmware-hcx/customize-template.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/customize-template.png":::

1. Seleccione **Next** (Siguiente), verifique la configuración y, a continuación, seleccione **Finish** (Finalizar) para implementar el archivo OVA del conector de HCX.
     
   >[!NOTE]
   >Por lo general, el conector de VMware HCX que va a implementar ahora se implementa en la red de administración del clúster.  
   
   > [!IMPORTANT]
   > Una vez finalizada la implementación, es posible que necesite encender el dispositivo virtual manualmente.
   > Después de encender el dispositivo HCX, espere entre 10 y 15 minutos antes de pasar al siguiente paso.

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: implementación de dispositivos de VMware HCX](https://www.youtube.com/embed/BwSnQeefnso). 


## <a name="activate-vmware-hcx"></a>Activación de VMware HCX

Después de implementar el archivo OVA del conector de VMware HCX en el entorno local e iniciar el dispositivo, está listo para activarlo, pero primero debe recuperar una clave de licencia desde el portal de Azure VMware Solution en Azure.

1. En el portal de Azure VMware Solution, vaya a **Manage** > **Connectivity** (Administrar > Conectividad), seleccione la pestaña **HCX** y, a continuación, seleccione **Add** (Agregar).

1. Inicie sesión en la instancia local de VMware HCX Manager en `https://HCXManagerIP:9443` e inicie sesión en el **administrador** con las credenciales de usuario. 

   > [!IMPORTANT]
   > Asegúrese de incluir el número de puerto `9443` con la dirección IP de VMware HCX Manager.

1. En **Licensing** (Licencia), escriba **HCX Advanced Key** (Licencia de HCX Advanced).  
   
    > [!NOTE]
    > VMware HCX Manager debe tener acceso abierto a Internet o un proxy configurado.

1. En la **Ubicación del centro de recursos**, proporcione la ubicación más cercana donde va a instalar la instancia local de VMware HCX Manager.

1. Modifique el **nombre del sistema** o acepte el valor predeterminado.
   
1. Puede finalizar el proceso más tarde o continuar con el mismo si selecciona la opción **Yes, Continue** (Sí, continuar).
    
1. En **Connect your vCenter** (Conectar vCenter), proporcione el FQDN o la dirección IP del servidor de vCenter y las credenciales adecuadas y, a continuación, seleccione **Continuar**.
   
1. En **Configure SSO/PSC** (configurar SSO/PSC), proporcione el FQDN o la dirección IP del PSC y, a continuación, seleccione **Continuar**.
   
   >[!NOTE]
   >Normalmente, este valor es el mismo que el FQDN o la IP de vCenter.

1. Compruebe que todas las entradas sean correctas y seleccione **Reiniciar**.
    
   > [!NOTE]
   > Verá que el proceso se ralentiza después de reiniciar y antes de que se le solicite realizar el siguiente paso.
   >
   >Una vez que se reinicien los servicios, es fundamental que el modo de vCenter se muestre en verde en la pantalla que aparecerá. Tanto vCenter como SSO tienen los parámetros de configuración adecuados, que deben ser los mismos que los de la pantalla anterior.

   :::image type="content" source="media/tutorial-vmware-hcx/activation-done.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/activation-done.png":::  

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: activación de VMware HCX](https://www.youtube.com/embed/BkAV_TNYxdE).


## <a name="configure-vmware-hcx-connector"></a>Configuración del conector de VMware HCX

Ya está listo para agregar un emparejamiento de sitios, crear un perfil de red y de proceso y habilitar servicios como la migración, la extensión de red o la recuperación ante desastres. 

### <a name="add-a-site-pairing"></a>Agregar un emparejamiento de sitios

Puede conectar (emparejar) el Administrador de la nube de VMware HCX de Azure VMware Solution con el conector de VMware HCX del centro de datos. 

1. Inicie sesión en la instancia local de vCenter y en **Inicio**, seleccione **HCX**.

   :::image type="content" source="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/vcenter-vmware-hcx.png":::

1. En **Infraestructura**, seleccione **Site Pairing** (Emparejamiento de sitios) y, a continuación, seleccione la opción **Connect To Remote Site** (Conectar con el sitio remoto) que se encuentra en medio de la pantalla. 

   :::image type="content" source="media/tutorial-vmware-hcx/connect-remote-site.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/connect-remote-site.png":::

1. Escriba la **dirección IP o dirección URL remota de HCX**, el nombre de usuario cloudadmin@vsphere.local y la **contraseña** de Azure VMware Solution y, a continuación, seleccione **Connect** (Conectar).

   > [!NOTE]
   > La **dirección URL remota de HCX** es la dirección IP del Administrador de la nube de HCX de la nube privada de Azure VMware Solution y es normalmente la dirección ".9" de la red de administración.  Por ejemplo, si la instancia de vCenter es 192.168.4.2, la dirección URL de HCX será 192.168.4.9.
   >
   > La **contraseña** será la misma que usó para iniciar sesión en vCenter. Recuerde que definió esta contraseña en la pantalla de implementación inicial.

   Verá una pantalla que muestra el Administrador de la nube de HCX en Azure VMware Solution y el conector de HCX local conectados (emparejados).

   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él.":::

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: emparejamiento de sitios de VMware HCX](https://www.youtube.com/embed/sKizDCRHOko).



### <a name="create-network-profiles"></a>Creación de perfiles de red

VMware HCX implementa un subconjunto de aplicaciones virtuales (automatizadas) que requieren varios segmentos IP.  Al crear los perfiles de red, se definen los segmentos IP que se identificaron durante la [fase de preparación y planeación previas a la implementación de los segmentos de red de VMware HCX](production-ready-deployment-steps.md#vmware-hcx-network-segments).

Debe crear cuatro perfiles de red:

   - Administración
   - vMotion
   - Replicación
   - Vínculo superior

1. En **Infrastructure** (Infraestructura), seleccione **Interconnect** > **Multi-Site Service Mesh** > **Network Profiles** > **Create Network Profile** (Interconectar > Malla de servicios de varios sitios > Perfiles de red > Crear perfil de red).

   :::image type="content" source="media/tutorial-vmware-hcx/network-profile-start.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/network-profile-start.png":::

1. En cada perfil de red, seleccione la red, el grupo de puertos, proporcione un nombre, cree el grupo de direcciones IP de ese segmento en particular y, a continuación, seleccione **Crear**. 

   :::image type="content" source="media/tutorial-vmware-hcx/example-configurations-network-profile.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él.":::

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: creación de un perfil de red de VMware HCX](https://www.youtube.com/embed/NhyEcLco4JY).


### <a name="create-compute-profile"></a>Creación de un perfil de proceso

1. Seleccione **Compute Profiles** > **Create Compute Profile** (Perfiles de proceso > Crear perfil de proceso).

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-create.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/compute-profile-create.png"::: (Perfil de proceso > Crear perfil de proceso).

1. Escriba un nombre para el perfil y seleccione **Continuar**.  

   :::image type="content" source="media/tutorial-vmware-hcx/name-compute-profile.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/name-compute-profile.png":::

1. Seleccione los servicios que se van a habilitar, como la migración, la extensión de red o la recuperación ante desastres y, a continuación, seleccione **Continue** (Continuar).
  
   > [!NOTE]
   > En general, no se cambiará nada aquí.

1. En **Select Service Resources** (Seleccionar recursos de servicio), seleccione uno o más recursos de servicio (clústeres) para los que se deban habilitar los servicios de VMware HCX seleccionados.  

1. Cuando vea los clústeres en el centro de datos local, seleccione **Continuar**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-service-resource.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-service-resource.png":::

1. En **Select Datastore** (Seleccionar almacén de datos), seleccione el recurso de almacenamiento del almacén de datos para implementar los dispositivos de interconexión de VMware HCX y, a continuación, seleccione **Continuar**.

   Cuando se seleccionan varios recursos, VMware HCX usa el primero seleccionado hasta que se agota su capacidad.   

   :::image type="content" source="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/deployment-resources-and-reservations.png":::  

1. Seleccione **Management Network Profile** (Perfil de red de administración), que es el perfil de red de administración que creó en los pasos anteriores y, a continuación, seleccione **Continuar**.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-management-network-profile.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-management-network-profile.png":::

   > [!NOTE]
   > El perfil de red de administración permite a los dispositivos de VMware HCX comunicarse con vCenter y se puede alcanzar con los hosts de ESXi.

1. Seleccione **Uplink Network Profile** (Perfil de red del vínculo superior), esto es, el perfil de red del vínculo superior que creó en los pasos anteriores y, a continuación, seleccione **Continuar**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-uplink-network-profile.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-uplink-network-profile.png":::

1. Seleccione **vMotion Network Profile** (Perfil de red de vMotion), esto es, el perfil de red de vMotion que creó en los pasos anteriores y, a continuación, seleccione **Continuar**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-vmotion-network-profile.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-vmotion-network-profile.png":::

1. Seleccione **vSphere Replication Network Profile** (Perfil de red de replicación de vSphere), esto es, el perfil de red de replicación que creó en los pasos anteriores y, a continuación, seleccione **Continuar**.

   En la mayoría de los casos, este perfil de red de replicación es el mismo que el perfil de red de administración.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-replication-network-profile.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-replication-network-profile.png":::

1. Seleccione **Distributed Switches for Network Extensions** (Conmutadores distribuidos para extensiones de red), que son los conmutadores virtuales distribuidos que contienen las máquinas virtuales que se van a migrar a Azure VMware Solution en una red extendida de dos capas y, a continuación, seleccione **Continuar**.

   :::image type=" content" source="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-layer-2-distributed-virtual-switch.png":::

1. Revise las reglas de conexión y seleccione **Continue** (Continuar).  

   :::image type="content" source="media/tutorial-vmware-hcx/review-connection-rules.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/review-connection-rules.png":::

1. Seleccione **Finish** (Finalizar) para crear el perfil de proceso.

   Verá una pantalla similar a la que se muestra a continuación.

   :::image type="content" source="media/tutorial-vmware-hcx/compute-profile-done.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/compute-profile-done.png":::

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: creación de un perfil de proceso de VMware HCX](https://www.youtube.com/embed/qASXi5xrFzM).




### <a name="create-service-mesh"></a>Creación de una malla de servicios

En este paso debe configurar la malla de servicios entre el entorno local y el SDDC de Azure VMware Solution.

1. En la opción **Infrastructure** (Infraestructura), seleccione **Interconnect** > **Service Mesh** > **Create Service Mesh** (Interconexión > Malla de servicios > Crear malla de servicios).    

   :::image type="content" source="media/tutorial-vmware-hcx/create-service-mesh.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/create-service-mesh.png":::

1. Revise los sitios que se han rellenado previamente y, a continuación, seleccione **Continuar**. 

   >[!NOTE]
   >Si esta es la primera configuración de la malla de servicios, no tendrá que modificar esta pantalla.  

1. Seleccione la lista desplegable de los perfiles de proceso de origen y remotos y, a continuación, haga clic en **Continuar**.  

   Las selecciones definen los recursos donde las VM pueden consumir los servicios de VMware HCX.  

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-source.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-compute-profile-source.png":::

   :::image type="content" source="media/tutorial-vmware-hcx/select-compute-profile-remote.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/select-compute-profile-remote.png":::

1. Revise los servicios que se van a habilitar y, a continuación, seleccione **Continuar**.  

1. En **Advanced Configuration - Override Uplink Network profiles** (Configuración avanzada: Invalidar los perfiles de vínculo superior de red), seleccione **Continue** (Continuar).  Los perfiles de red de vínculo superior se conectan a la red a través de la cual se puede acceder a los dispositivos de interconexión del sitio remoto.  
  
1. En **Advanced Configuration - Network Extension Appliance Scale Out** (Configuración avanzada: escalabilidad horizontal del dispositivo de extensión de red), revise la información y seleccione **Continuar**. 

1. En **Advanced Configuration - Traffic Engineering** (Configuración avanzada: ingeniería de tráfico), revise y realice las modificaciones que considere necesarias y, a continuación, seleccione **Continuar**.

1. Revise la vista previa de la topología y seleccione **Continue** (Continuar).

1. A continuación, escriba un nombre descriptivo para esta malla de servicios y seleccione **Finalizar** para terminar el proceso.  

1. Seleccione **Ver tareas** para supervisar la implementación. 

   :::image type="content" source="media/tutorial-vmware-hcx/monitor-service-mesh.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él.":::

   Una vez que la implementación de la malla de servicios se complete correctamente, verá los servicios en color verde.

   :::image type="content" source="media/tutorial-vmware-hcx/service-mesh-green.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/service-mesh-green.png":::

1. Para comprobar el estado de la malla de servicios cuando también compruebe el estado de la aplicación, seleccione **Interconexión** > **Dispositivos**.

   :::image type="content" source="media/tutorial-vmware-hcx/interconnect-appliance-state.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/interconnect-appliance-state.png":::

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: creación de una malla de servicios de VMware HCX](https://www.youtube.com/embed/FyZ0d3P_T24).



### <a name="optional-create-a-network-extension"></a>(Opcional) Creación de una extensión de red

Si quiere extender las redes de su entorno local a Azure VMware Solution, siga estos pasos para ampliar esas redes.

1. En **Servicios**, seleccione **Extensión de red** y, a continuación, seleccione **Create a Network Extension** (Crear una extensión de red).

   :::image type="content" source="media/tutorial-vmware-hcx/create-network-extension.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/create-network-extension.png":::

1. Seleccione cada una de las redes que quiera extender a Azure VMware Solution y, a continuación, seleccione **Siguiente**.

   :::image type="content" source="media/tutorial-vmware-hcx/select-extend-networks.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él.":::.

1. Escriba la dirección IP de puerta de enlace local para cada una de las redes que va a extender y, a continuación, seleccione **Enviar**. 

   :::image type="content" source="media/tutorial-vmware-hcx/extend-networks-gateway.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él.":::.

   Tenga en cuenta que la extensión de red tardará unos minutos en completarse. Cuando lo haga, verá el cambio de estado a **extensión completada**.

   :::image type="content" source="media/tutorial-vmware-hcx/extension-complete.png" alt-text="Vaya a la instancia local de vCenter y seleccione una plantilla OVF para implementarla en él." lightbox="media/tutorial-vmware-hcx/extension-complete.png":::.

Para obtener información general de un extremo a otro de este paso, consulte el vídeo [Azure VMware Solution: extensión de red de VMware HCX](https://www.youtube.com/embed/cNlp0f_tTr0).


## <a name="next-steps"></a>Pasos siguientes

Si ha llegado a este punto y el valor de Tunnel Status (Estado del túnel) es **UP** (ACTIVO) y de color verde, está listo para migrar y proteger las VM de Azure VMware Solution con VMware HCX.  Azure VMware Solution admite migraciones de cargas de trabajo (con una extensión de red).  Todavía puede realizar migraciones de cargas de trabajo en el entorno local de vSphere, para la creación de redes y la implementación de VM en esas mismas redes.  Para obtener más información, consulte la [documentación de VMware HCX](https://docs.vmware.com/en/VMware-HCX/index.html) y [Migración de máquinas virtuales con VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g) en la documentación técnica de VMware para más información sobre el uso de HCX.
