---
title: Conexión remota a un nodo de clúster de Azure Service Fabric
description: Aprenda a conectarse de forma remota a una instancia del conjunto de escalado (un nodo de clúster de Service Fabric).
ms.topic: conceptual
ms.date: 03/23/2018
ms.openlocfilehash: 98d573af4fc2026134e75d4caf24a09e57e52c87
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96012551"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Conexión remota a una instancia de conjunto de escalado de máquinas virtuales o a un nodo de clúster
En un clúster de Service Fabric que se ejecuta Azure, cada tipo de nodo de clúster que defina [configura una escala independiente de la máquina virtual](service-fabric-cluster-nodetypes.md).  También puede conectarse de forma remota a instancias específicas del conjunto de escalado (nodos de clúster).  A diferencia de las máquinas virtuales de instancia única, las instancias de conjuntos de escalado no tienen direcciones IP virtuales. Esto puede resultar un poco complicado si desea obtener una dirección IP y un puerto que pueda usar para conectarse de manera remota a una instancia específica.

Para buscar una dirección IP y un puerto que pueda usar para conectarse remotamente a una instancia concreta, realice los pasos siguientes.

1. Obtenga las reglas de NAT de entrada para Protocolo de escritorio remoto (RDP).

    Normalmente, cada tipo de nodo definido en el clúster tiene su propia dirección IP virtual y un equilibrador de carga dedicado. De forma predeterminada, el equilibrador de carga para un tipo de nodo se denomina con el formato siguiente: *LB-{nombre del clúster}-{tipo de nodo}* ; por ejemplo, *LB-mycluster-FrontEnd*. 
    
    En Azure Portal, en la página del equilibrador de carga, seleccione **Configuración** > **Reglas NAT de entrada**. 

    ![Captura de pantalla de la página de un equilibrador de carga en Azure Portal. En el menú de la izquierda, en CONFIGURACIÓN, la opción Reglas NAT de entrada está seleccionada.](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    La siguiente captura de pantalla muestra las reglas NAT de entrada para un tipo de nodo con el nombre FrontEnd: 

    ![Captura de pantalla en la que se muestran las reglas NAT de entrada de un equilibrador de carga. El nombre, la versión de IP, el destino, el objetivo y el servicio se muestran para cada regla.](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    Para cada nodo, la dirección IP aparece en la columna **DESTINO**, la columna **OBJETIVO** indica la instancia del conjunto de escalado y la columna **SERVICIO** indica el número de puerto. Para la conexión remota, los puertos se asignan a cada nodo en orden ascendente a partir del puerto 3389.

    También puede encontrar las reglas NAT de entrada en la sección `Microsoft.Network/loadBalancers` de la plantilla de Resource Manager del clúster.
    
2. Para confirmar la asignación del puerto de entrada al puerto de destino para un nodo, puede hacer clic en la regla y consultar el valor **Puerto de destino**. La siguiente captura de pantalla muestra la regla NAT de entrada para el nodo **FrontEnd (instancia 1)** del paso anterior. Tenga en cuenta que, aunque el número de puerto (entrada) es 3390, el puerto de destino está asignado al puerto 3389, el puerto para el servicio RDP en el destino.  

    ![Asignación de puertos de destino](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    De forma predeterminada, para los clústeres Windows, el puerto de destino es 3389, que se asigna al servicio RDP en el nodo de destino. Para los clústeres Linux, el puerto de destino es 22, que se asigna al servicio de Secure Shell (SSH).

3. Conéctese de forma remota al nodo específico (instancia del conjunto de escalado). Puede usar el nombre de usuario y la contraseña que estableció cuando creó el clúster u otras credenciales que haya configurado. 

    La siguiente captura de pantalla muestra cómo usar Conexión a Escritorio remoto para conectarse al nodo **FrontEnd (instancia 1)** en un clúster Windows:
    
    ![Conexión del Escritorio remoto](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    En los nodos Linux, puede conectarse mediante SSH (en el ejemplo siguiente se vuelve a utilizar la misma dirección IP y puerto para abreviar):

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Para ver los próximos pasos, lea los siguientes artículos:
* Consulte [Descripción general de la característica "Deploy anywhere" y comparación con los clústeres administrados de Azure](service-fabric-deploy-anywhere.md).
* Obtenga más información sobre la [seguridad del clúster](service-fabric-cluster-security.md).
* [Actualización de los valores del intervalo de puertos RDP](./scripts/service-fabric-powershell-change-rdp-port-range.md) en máquinas virtuales del clúster después de la implementación
* [Cambio de la contraseña y el nombre de usuario administrador](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) para máquinas virtuales del clúster

