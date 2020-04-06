---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: shandilvarun
ms.service: virtual-machines
ms.topic: include
ms.date: 08/09/2018
ms.author: vashan, cynthn, rajsqr
ms.custom: include file
ms.openlocfilehash: 57f557a812ec5e4eea75b76ca1394ca360a85d30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186254"
---
Las máquinas virtuales de Azure pueden tener diferentes estados que se pueden clasificar en estados de *aprovisionamiento* y estados de *energía*. El propósito de este artículo es describir estos estados y resaltar específicamente cuándo se les factura a los clientes por el uso de la instancia. 

## <a name="power-states"></a>Estados de energía

El estado de energía representa el último estado conocido de la máquina virtual.

![Diagrama de estado de energía de la máquina virtual](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
La tabla siguiente proporciona una descripción del estado de cada instancia e indica si se factura por uso de instancia o no.

<table>
<tr>
<th>
State
</th>
<th>
Descripción
</th>
<th>
Facturación del uso de instancia
</th>
</tr>
<tr>
<td>
<p><b>Starting</b> (iniciándose)</p>
</td>
<td>
<p>La máquina virtual se está iniciando.</p>
<code>"statuses": [<br>
   {<br>
      "code": "PowerState/starting",<br>
       "level": "Info",<br>
        "displayStatus": "VM starting"<br>
    }<br>
    ]</code><br>
</td>
<td>
<p><b>No facturado</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Ejecución</b></p>
</td>
<td>
<p>Estado de funcionamiento normal para una máquina virtual</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/running",<br>
 "level": "Info",<br>
 "displayStatus": "VM running"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Facturado</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Stopping</b> (Deteniéndose)</p>
</td>
<td>
<p>Se trata de un estado transitorio. Cuando haya terminado, aparecerá como **Detenido**.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopping",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopping"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Facturado</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Stopped</b></p>
</td>
<td>
<p>Se ha apagado la máquina virtual desde dentro del sistema operativo invitado o está utilizando las API de PowerOff.</p>
<p>El hardware aún está asignado a la máquina virtual y permanece en el host. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopped",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopped"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Facturado&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Desasignando</b></p>
</td>
<td>
<p>Estado transitorio. Cuando haya terminado, la máquina virtual aparecerá como **Desasignado**.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocating",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocating"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>No facturado&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Desasignado</b></p>
</td>
<td>
<p>La máquina virtual se ha detenido correctamente y se ha eliminado del host. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocated",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocated"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>No facturado</b></p>
</td>
</tr>
</tbody>
</table>


&#42;Algunos recursos de Azure como los discos y las redes, incurren en gastos. Las licencias de software en la instancia no cuestan nada.

## <a name="provisioning-states"></a>Estados de aprovisionamiento

Un estado de aprovisionamiento es el estado de una operación de plano de control iniciada por el usuario en la máquina virtual. Estos estados son independientes del estado de energía de una máquina virtual.

- **Crear**: creación de máquinas virtuales.

- **Actualizar**: actualiza el modelo de una máquina virtual existente. Algunos cambios no relacionados con el modelo que se realizan en las máquinas virtuales, como Iniciar o Reiniciar, también se encuadran bajo la opción Actualizar.

- **Eliminar**: eliminación de máquinas virtuales.

- **Desasignar**: es donde se detiene una máquina virtual y se elimina del host. Desasignar una máquina virtual se considera una actualización, por lo que mostrará los estados de aprovisionamiento relacionados con la actualización.



Estos son los estados de operación transitorios una vez que la plataforma ha aceptado una acción iniciada por el usuario:

<br>

<table>
<tbody>
<tr>
<td width="162">
<p><b>Estados</b></p>
</td>
<td width="366">
<p>Descripción</p>
</td>
</tr>
<tr>
<td width="162">
<p><b>Creando</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating",<br>
 "level": "Info",<br>
 "displayStatus": "Creating"<br>
 }</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Actualizando</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/updating",<br>
 "level": "Info",<br>
 "displayStatus": "Updating"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Eliminando</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/deleting",<br>
 "level": "Info",<br>
 "displayStatus": "Deleting"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Estados de aprovisionamiento del sistema operativo</b></p>
</td>
<td width="366">
<p>Si se crea una máquina virtual con una imagen de sistema operativo y no con una imagen especializada, se pueden observar los subestados siguientes:</p>
<p>1. <b>OSProvisioningInprogress</b> &ndash; se está ejecutando la máquina virtual y la instalación del sistema operativo invitado está en curso. <p /> 
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningInprogress",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning In progress"<br>
 }<br>
]</code><br>
<p>2. <b>OSProvisioningComplete</b> &ndash; estado de corta duración. La máquina virtual pasa rápidamente a **Correcto** a menos que haya que instalar alguna extensión. La instalación de extensiones puede tardar un tiempo. <br />
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningComplete",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning Complete"<br>
 }<br>
]</code><br>
<p><b>Nota</b>: El aprovisionamiento del sistema operativo puede pasar a **Erróneo** si se produce un error del sistema operativo o este no realiza la instalación a tiempo. A los clientes se les facturará por la máquina virtual implementada en la infraestructura.</p>
</td>
</tr>
</table>


Una vez completada la operación, la máquina virtual pasará a uno de los siguientes estados:

- **Correcto**: se han completado las acciones iniciadas por el usuario.

    ```
  "statuses": [ 
  {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
  }
  ]
    ```

 

- **Erróneo**: representa una operación con errores. Consulte los códigos de error para obtener más información y posibles soluciones.

    ```
  "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```



## <a name="vm-instance-view"></a>Vista de instancia de máquina virtual

La API de vista de instancia proporciona información sobre el estado de ejecución de la máquina virtual. Para más información, consulte la documentación de API de [Máquinas virtuales: Instance View](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview).

Azure Resource Explorer proporciona una interfaz de usuario sencilla para ver el estado de ejecución de una máquina virtual: [Explorador de recursos](https://resources.azure.com/).

Los estados de aprovisionamiento son visibles en las propiedades y la vista de instancia de la máquina virtual. Los estados de energía están disponibles en la vista de instancia de la máquina virtual. 

