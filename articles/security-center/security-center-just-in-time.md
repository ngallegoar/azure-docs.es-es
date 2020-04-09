---
title: Acceso a máquina virtual Just-In-Time en Azure Security Center | Microsoft Docs
description: En este documento se explica la forma en que el acceso a máquina virtual del tipo Just-In-Time en Azure Security Center le ayuda a controlar el acceso a máquinas virtuales de Azure.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: cc4e267c6912b8938db1ba5497a27f9c0026bd79
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887340"
---
# <a name="secure-your-management-ports-with-just-in-time-access"></a>Protección de los puertos de administración con acceso Just-in-Time

Si está en el plan de tarifa estándar de Security Center (consulte los [precios](/azure/security-center/security-center-pricing)), puede bloquear el tráfico entrante a las máquinas virtuales de Azure con el acceso de máquina virtual Just-in-Time (JIT). Esto reduce la exposición a los ataques a la vez que proporciona un acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario.

> [!NOTE]
> Actualmente, el acceso a máquina virtual del tipo Just-In-Time de Security Center solo admite las máquinas virtuales implementadas a través de Azure Resource Manager. Para obtener más información sobre los modelos de implementación clásico y de Resource Manager, vea [La implementación de Azure Resource Manager frente a la implementación clásica](../azure-resource-manager/management/deployment-models.md).

[!INCLUDE [security-center-jit-description](../../includes/security-center-jit-description.md)]

## <a name="configure-jit-on-a-vm"></a>Configurar JIT en una VM

Existen tres maneras de configurar una directiva JIT en una máquina virtual:

- [Configurar el acceso JIT en Azure Security Center](#jit-asc)
- [Configurar el acceso JIT en una página de la máquina virtual de Azure](#jit-vm)
- [Configurar una directiva JIT en una VM mediante programación](#jit-program)

## <a name="configure-jit-in-azure-security-center"></a>Configurar JIT en Azure Security Center

Desde Security Center, puede configurar un directiva de JIT y solicitar acceso a una VM mediante una directiva JIT.

### <a name="configure-jit-access-on-a-vm-in-security-center"></a>Configuración del acceso JIT en una máquina virtual en Security Center<a name="jit-asc"></a>

1. Abra el panel **Security Center**.

1. En el panel izquierdo, seleccione **Just-in-time VM access** (Acceso a máquina virtual del tipo Just-In-Time).

    ![Icono de acceso a máquina virtual del tipo Just-In-Time](./media/security-center-just-in-time/just-in-time.png)

    Se abre la ventana **Acceso a VM Just-In-Time** y se muestra información sobre el estado de las máquinas virtuales:

    - **Configurado**: máquinas virtuales que se han configurado para admitir el acceso a máquina virtual Just-In-Time. Los datos que se muestran son de la última semana e incluyen, para cada máquina virtual, el número de solicitudes aprobadas, la fecha y hora del último acceso y el último usuario.
    - **Recomendado**: máquinas virtuales que pueden admitir el acceso a máquina virtual Just-In-Time, pero que no se han configurado para ello. Se recomienda habilitar el control de acceso a máquina virtual Just-In-Time para estas máquinas virtuales.
    - **Ninguna recomendación**: una máquina virtual podría no recomendarse por diversas razones:
      - Falta grupo de seguridad de red: la solución Just-In-Time requiere que exista un grupo de seguridad de red.
      - Máquina virtual clásica: en la actualidad, el acceso a máquina virtual Just-In-Time de Security Center solo admite las máquinas virtuales implementadas mediante Azure Resource Manager. La solución Just-In-Time no admite las implementaciones clásicas. 
      - Otros: una VM se encuentra en esta categoría si la solución Just-In-Time está desactivada en la directiva de seguridad de la suscripción o grupo de recursos, o si la VM no tiene una IP pública ni existe ningún grupo de seguridad de red.

1. Seleccione la pestaña **Recomendado**.

1. En **MÁQUINA VIRTUAL**, haga clic en las VM que quiere habilitar. De este modo, se coloca una marca de verificación junto a una máquina virtual.

      ![Habilitar acceso Just-In-Time](./media/security-center-just-in-time/enable-just-in-time.png)

1. Haga clic en **Habilitar JIT en VM**. Se abre un panel con los puertos predeterminados que recomienda Azure Security Center:
    - 22 - SSH
    - 3389 - RDP
    - 5985 - WinRM 
    - 5986 - WinRM
1. También tiene la opción de agregar puertos personalizados a la lista:

      1. Haga clic en **Agregar**. Se abre la ventana **Agregar configuración de puerto**.
      1. Para cada puerto que elija configurar, tanto el predeterminado como el configurado, puede personalizar los valores siguientes:
            - **Tipo de protocolo**: el protocolo que se permite en este puerto cuando se aprueba una solicitud.
            - **Direcciones IP de origen permitidas**: los intervalos de IP que se permiten en este puerto cuando se aprueba una solicitud.
            - **Tiempo de solicitud máximo**: el período de tiempo máximo durante el que puede estar abierto un puerto específico.

     1. Haga clic en **OK**.

1. Haga clic en **Save**(Guardar).

> [!NOTE]
>Cuando el acceso a la VM JIT se habilita en una VM, Azure Security Center crea reglas para denegar todo el tráfico entrante para los puertos seleccionados en los grupos de seguridad de red asociados y en Azure Firewall. En caso de que se hubieran creado otras reglas relativas a los puertos seleccionados, las reglas existentes tendrán prioridad sobre las nuevas reglas para "denegar todo el tráfico entrante". Si no hay ninguna regla existente en los puertos seleccionados, las nuevas reglas de "denegar todo el tráfico entrante" tendrán prioridad principal en grupos de seguridad de red y Azure Firewall.


## <a name="request-jit-access-via-security-center"></a>Solicitud de acceso JIT a través de Security Center

Para solicitar acceso a una VM a través de Security Center:

1. En **Just in time VM access** (Acceso a máquina virtual del tipo Just-In-Time), seleccione la pestaña **Configurado**.

1. En **Máquina virtual**, haga clic en las VM a las que quiera solicitar acceso. De este modo, se coloca una marca de verificación junto a la VM.

    - El icono de la columna **Detalles de la conexión** indica si JIT está habilitado en el NSG o firewall. Si está habilitado en ambos, solo aparece el icono de firewall.

    - En la columna **Detalles de la conexión** se proporciona la información necesaria para conectarse a la máquina virtual y sus puertos abiertos.

      ![Solicitud de acceso Just-In-Time](./media/security-center-just-in-time/request-just-in-time-access.png)

1. Haga clic en **Solicitar acceso**. Se abre la ventana **Solicitar acceso**.

      ![Detalles de JIT](./media/security-center-just-in-time/just-in-time-details.png)

1. En **Solicitar acceso**, configure para cada máquina virtual los puertos que desee que se abran y las direcciones IP de origen en que se abre el puerto y el tiempo durante el que permanecerá abierto. Solo se podrá solicitar acceso a los puertos que estén configurados en la directiva Just-In-Time. Cada puerto tiene un tiempo máximo permitido que se basa en la directiva Just-In-Time.

1. Haga clic en **Abrir puertos**.

> [!NOTE]
> Si un usuario que solicita acceso está detrás de un proxy, es posible que la opción de **Mi IP** no funcione. Es posible que necesite definir todo el intervalo de direcciones IP de la organización.



## <a name="edit-a-jit-access-policy-via-security-center"></a>Edición de una directiva de acceso JIT a través de Security Center

Para cambiar la directiva Just-In-Time de una máquina virtual, agregue y configure un puerto nuevo que proteja dicha máquina virtual, o bien cambie otro valor relacionado con un puerto ya protegido.

Para editar una directiva Just-In-Time existente de una máquina virtual:

1. En la ficha **Configurado**, en **Máquinas virtuales**, seleccione la máquina virtual a la que desea agregar un puerto. Para ello, haga clic en los tres puntos situados en la fila de dicha máquina. 

1. Seleccione **Editar**.

1. En **Configuración de acceso a máquina virtual del tipo JIT** , puede modificar la configuración existente de un puerto protegido, o bien agregar un puerto personalizado. 
  ![acceso a vm de tipo jit](./media/security-center-just-in-time/edit-policy.png)



## <a name="audit-jit-access-activity-in-security-center"></a>Auditoría de la actividad de acceso JIT en Security Center

Puede usar la búsqueda de registros para obtener información sobre las actividades de las máquinas virtuales. Para ver los registros:

1. En **Just in time VM access** (Acceso a máquina virtual del tipo Just-In-Time), seleccione la pestaña **Configurado**.
2. En **Máquinas virtuales**, seleccione una máquina virtual sobre la que ver información; para ello, haga clic en los tres puntos situados dentro de la fila correspondiente a esa máquina virtual y seleccione **Registro de actividad** en el menú. Se abre el **registro de actividad**.

   ![Seleccionar el registro de actividades](./media/security-center-just-in-time/select-activity-log.png)

   En **Registro de actividades** se proporciona una vista filtrada de las operaciones anteriores para esa máquina virtual, junto con el tiempo, la fecha y la suscripción.

Para descargar la información del registro, seleccione **Haga clic aquí para descargar todos los elementos como archivo .csv**.

Modifique los filtros y haga clic en **Aplicar** para crear una búsqueda y un registro.



## <a name="configure-jit-access-from-an-azure-vms-page"></a>Configuración del acceso JIT desde la página de una máquina virtual de Azure<a name="jit-vm"></a>

Para su comodidad, puede conectarse a una máquina virtual con JIT directamente desde la página de la máquina virtual en Security Center.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-page"></a>Configuración del acceso JIT en una máquina virtual a través de la página de la máquina virtual de Azure

Para facilitar el despliegue del acceso Just-In-Time en las VM, puede establecer una VM para permitir solo el acceso Just-In-Time directamente desde dentro de la VM.

1. En [Azure Portal](https://ms.portal.azure.com), busque y seleccione **Máquinas virtuales**. 
2. Seleccione la máquina virtual a la que quiere limitar el acceso Just-In-Time.
3. En el menú, seleccione **Configuración**.
4. En **Acceso Just-In-Time**, seleccione **Habilitar Just-In-Time**. 

Esto habilita el acceso Just-In-Time para la VM con las siguientes opciones:

- Servidores Windows:
    - Puerto RDP 3389
    - Se permite un tiempo de acceso máximo de tres horas
    - Las direcciones IP de origen permitidas se establecen en Cualquiera
- Servidores Linux:
    - Puerto SSH 22
    - Se permite un tiempo de acceso máximo de tres horas
    - Las direcciones IP de origen permitidas se establecen en Cualquiera
     
Si una VM ya tiene Just-In-Time habilitado, cuando va a su página de configuración podrá ver que Just-In-Time está habilitado y que puede usar el vínculo para abrir la directiva en Azure Security Center para ver y cambiar la configuración.

![jit config in vm](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-an-azure-vms-page"></a>Solicitud de acceso JIT a una máquina virtual mediante la página de una máquina virtual de Azure

En Azure Portal, al intentar conectarse a una máquina virtual, Azure comprueba si hay alguna directiva de acceso Just-In-Time configurada en ella.

- Si tiene una directiva JIT configurada en la máquina virtual, puede hacer clic en **Solicitar acceso** para conceder acceso de acuerdo con la directiva JIT establecida para la máquina virtual. 

  >![solicitud de JIT](./media/security-center-just-in-time/jit-request.png)

  El acceso se solicita con los parámetros predeterminados siguientes:

  - **source IP**: "Cualquiera" (*) (no se puede modificar)
  - **time range**: Tres horas (o se puede modificar) <!--Isn't this set in the policy-->
  - **port number**: puerto RDP 3389 para Windows; puerto 22 para Linux (se puede cambiar)

    > [!NOTE]
    > Después de aprobarse una solicitud para una VM protegida con Azure Firewall, Security Center proporciona al usuario los datos de conexión (asignación de puerto de la tabla DNAT) que debe usar para conectarse a la máquina virtual.

- Si hay alguna máquina virtual en la que no esté configurado JIT, se le solicitará que lo configure.

  ![solicitud de jit](./media/security-center-just-in-time/jit-prompt.png)

## <a name="configure-a-jit-policy-on-a-vm-programmatically"></a>Configurar una directiva JIT en una VM mediante programación <a name="jit-program"></a>

Just-In-Time se puede configurar y usar tanto a través de las API REST como de PowerShell.

### <a name="jit-vm-access-via-rest-apis"></a>Acceso a la VM JIT a través de las API de REST

La característica de acceso a máquina virtual Just-in-Time se puede usar mediante la API de Azure Security Center. Use esta API para obtener más información acerca de las máquinas virtuales configuradas, cómo agregar otras nuevas o solicitar acceso a una máquina virtual, entre otras cosas. Consulte [Jit Network Access Policies](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies) (Directivas de acceso a red JIT), para más información acerca de la API REST Just-in-Time.

### <a name="jit-vm-access-via-powershell"></a>Acceso a la VM JIT mediante PowerShell

Para utilizar la solución de acceso a máquina virtual Just-In-Time mediante PowerShell, use los cmdlets de PowerShell de Azure Security Center oficiales; específicamente `Set-AzJitNetworkAccessPolicy`.

En el siguiente ejemplo se establece una directiva de acceso a la máquina virtual del tipo Just-in-Time en una máquina virtual específica, y se establece lo siguiente:

1.    Cierre los puertos 22 y 3389.

2.    Establezca una ventana de tiempo máximo de 3 horas para cada una, para que así puedan abrirse según la solicitud aprobada.
3.    Permite que el usuario que solicita el acceso controle las direcciones IP de origen; asimismo, le permite establecer una sesión correcta tras una solicitud de acceso del tipo Just-In-Time aprobada.

Ejecute lo siguiente en PowerShell, para poder lograr lo que se ha detallado:

1.    Asigne una variable que contenga la directiva de acceso a máquina virtual del tipo Just-in-Time para una máquina virtual:

        $JitPolicy = (@{    id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"   ports=(@{        number=22;        protocol="\*";        allowedSourceAddressPrefix=@("\*");        maxRequestAccessDuration="PT3H"},        @{        number=3389;        protocol="\*";        allowedSourceAddressPrefix=@("\*");        maxRequestAccessDuration="PT3H"})})

2.    Inserte la directiva de acceso a máquina virtual del tipo Just-in-Time de la máquina virtual en una matriz:
    
        $JitPolicyArr=@($JitPolicy)

3.    Configure la directiva de acceso a máquina virtual del tipo Just-in-Time en la máquina virtual seleccionada:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="request-access-to-a-vm-via-powershell"></a>Solicitar acceso a una VM a través de PowerShell

En el siguiente ejemplo, puede ver una solicitud de acceso a máquina virtual del tipo Just-In-Time para una máquina virtual específica, en la que se solicita que se abra el puerto 22 para una dirección IP específica y durante un período concreto:

Ejecute lo siguiente en PowerShell:
1.    Configure las propiedades de acceso de solicitud de la máquina virtual.

        $JitPolicyVm1 = (@{     id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"   ports=(@{      number=22;      endTimeUtc="2018-09-17T17:00:00.3658798Z";      allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.    Inserte los parámetros de solicitud de acceso a la máquina virtual en una matriz:

        $JitPolicyArr=@($JitPolicyVm1)
3.    Envíe el acceso de solicitud (use el id. de recurso que obtuvo en el paso 1).

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Para más información, consulte la [documentación del cmdlet de PowerShell](https://docs.microsoft.com/powershell/scripting/developer/cmdlet/cmdlet-overview).


## <a name="automatic-cleanup-of-redundant-jit-rules"></a>Limpieza automática de reglas JIT redundantes 

Siempre que se actualiza una directiva JIT, se ejecuta automáticamente una herramienta de limpieza para comprobar la validez de todo el conjunto de reglas. La herramienta busca discrepancias entre las reglas de la directiva y las reglas del grupo de seguridad de red. Si la herramienta de limpieza encuentra una discrepancia, determina la causa y, cuando es seguro hacerlo, quita las reglas integradas que ya no son necesarias. El limpiador no elimina nunca las reglas que ha creado.

Ejemplos de escenarios en los que el limpiador podría quitar una regla integrada:

- Cuando existen dos reglas con definiciones idénticas y una tiene una prioridad más alta que la otra (lo que significa que nunca se usará la regla de prioridad inferior)
- Cuando una descripción de la regla incluye el nombre de una máquina virtual que no coincide con la dirección IP de destino de la regla 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido la forma en que el acceso a máquina virtual del tipo Just-In-Time en Security Center ayuda a controlar el acceso a máquinas virtuales de Azure.

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

- El módulo de Microsoft Learn [Protección de los servidores y máquinas virtuales de ataques por fuerza bruta y de malware con Azure Security Center](https://docs.microsoft.com/learn/modules/secure-vms-with-azure-security-center/)
- [Establecimiento de directivas de seguridad](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
- [Administración de recomendaciones de seguridad](security-center-recommendations.md): conozca una serie de recomendaciones que le ayudarán a proteger los recursos de Azure.
- [Supervisión del estado de seguridad](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
