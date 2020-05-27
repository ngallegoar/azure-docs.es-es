---
title: Devolver o reemplazar el dispositivo Azure Stack Edge | Microsoft Docs
description: En este artículo se explica cómo devolver o reemplazar el dispositivo Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/26/2020
ms.author: alkohli
ms.openlocfilehash: bb73494dd5fe22c3be645f732f9d0958e48edb64
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743634"
---
# <a name="return-or-replace-your-azure-stack-edge-device"></a>Devolver o reemplazar el dispositivo Azure Stack Edge

En este artículo se explica cómo borrar los datos y luego devolver el dispositivo Azure Stack Edge. Una vez que haya devuelto el dispositivo, también puede eliminar el recurso asociado al dispositivo o solicitar un dispositivo de sustitución.

En este artículo aprenderá a:

> [!div class="checklist"]
> * Borrar los datos de los discos de datos del dispositivo
> * Abrir una incidencia de soporte técnico para devolver el dispositivo
> * Empaquetar el dispositivo y programar una recogida
> * Eliminar el recurso en Azure Portal
> * Obtener un dispositivo de sustitución

## <a name="erase-data-from-the-device"></a>Borrar los datos del dispositivo

Para borrar los datos de los discos de datos del dispositivo, debe restablecer el dispositivo. Puede restablecer el dispositivo mediante la interfaz de usuario web local o la interfaz de PowerShell.

Antes de restablecerlo, cree una copia de los datos locales del dispositivo si es necesario. Puede copiar los datos del dispositivo en un contenedor de Azure Storage.

Para restablecer el dispositivo mediante la interfaz de usuario web local, siga estos pasos.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Restablecer dispositivo**.
2. Seleccione **Restablecer dispositivo**.

    ![Restablecer el dispositivo](media/azure-stack-edge-return-device/device-reset-1.png)

3. Cuando se le pida confirmación, revise la advertencia y seleccione **Sí** para continuar.

    ![Confirmar el restablecimiento](media/azure-stack-edge-return-device/device-reset-2.png)  

Al restablecer el dispositivo se borran los datos de los discos de datos del dispositivo. Según la cantidad de datos del dispositivo, este proceso tarda entre 30 y 40 minutos.

Como alternativa, conéctese a la interfaz de PowerShell del dispositivo y utilice el cmdlet `Reset-HcsAppliance` para borrar los datos de los discos de datos. Para obtener más información, consulte [Restablecer el dispositivo](azure-stack-edge-connect-powershell-interface.md#reset-your-device).

> [!NOTE]
> - Si va a intercambiar o actualizar a un dispositivo nuevo, se recomienda restablecer el dispositivo solo después de que haya recibido el dispositivo nuevo.
> - Al restablecer el dispositivo solo se eliminan todos los datos locales del dispositivo. Los datos que están en la nube no se eliminan, sino que generan [cargos](https://azure.microsoft.com/pricing/details/storage/). Estos datos deben eliminarse por separado mediante una herramienta de administración de almacenamiento en la nube como [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="open-a-support-ticket"></a>Abrir una incidencia de soporte técnico

Para comenzar el proceso de devolución, siga los pasos siguientes.

1. Abra una incidencia de soporte técnico con Microsoft Support para indicar que quiere devolver el dispositivo. Seleccionar el tipo de problema como **Hardware Azure Stack Edge**.

    ![Abrir una incidencia de soporte técnico](media/azure-stack-edge-return-device/open-support-ticket-1.png)  

2. Un ingeniero de soporte técnico de Microsoft se pondrá en contacto con usted. Proporcione los detalles del envío.
3. Si necesita una caja de devolución, puede solicitarla. Responda **Sí** a la pregunta **Necesita una caja vacía para la devolución**.


## <a name="schedule-a-pickup"></a>Programar una recogida

1. Apague el dispositivo. En la interfaz de usuario web local, vaya a **Mantenimiento > Configuración de encendido**.
2. Seleccione **Apagar**. Cuando se le pida confirmación, haga clic en **Sí** para continuar. Para obtener más información, consulte [Administración de los cables de alimentación](data-box-gateway-manage-access-power-connectivity-mode.md#manage-power).
3. Desconecte los cables de alimentación y quite todos los cables de red del dispositivo.
4. Prepare el paquete de envío con su propia caja o con la caja que ha recibido de Azure. Introduzca en la caja el dispositivo y los cables de alimentación que se han enviado con el dispositivo.
5. Coloque en el paquete la etiqueta de envío que ha recibido de Azure.
6. Programe una recogida con su transportista regional. Si devuelve el dispositivo en EE. UU., el transportista podría ser UPS o FedEx. Póngase en contacto con UPS para programar la recogida:

    1. Llame a la oficina local de UPS (número gratuito específico del país o región).
    2. En la llamada, indique el número de seguimiento del envío inverso que se muestra en la etiqueta impresa.
    3. Si no indica el número de seguimiento, UPS le exigirá que pague una cantidad adicional en la recogida.

    En lugar de programar la recogida, puede devolver Azure Stack Edge en el punto de recogida más cercano.

## <a name="delete-the-resource"></a>Eliminar el recurso

Una vez recibido en el centro de datos de Azure, el dispositivo se inspecciona para detectar daños o signos de manipulación.

- Si el dispositivo llega intacto y en buen estado, el medidor de facturación se detiene para ese recurso. Un agente de Soporte técnico de Microsoft se pondrá en contacto con usted para confirmar que se ha devuelto el dispositivo. A continuación, puede eliminar el recurso asociado al dispositivo en Azure Portal.
- Si el dispositivo llega significativamente dañado, es posible que se apliquen sanciones. Para obtener más información, consulte las [preguntas más frecuentes sobre dispositivos perdidos o dañados](https://azure.microsoft.com/pricing/details/databox/edge/) y los [términos del servicio del producto](https://www.microsoft.com/licensing/product-licensing/products).  


Puede eliminar el dispositivo en Azure Portal:
-    Después de realizar el pedido y antes de que Microsoft prepare el dispositivo.
-    Tras devolver el dispositivo a Microsoft, pasa por la inspección física en el centro de datos de Azure y el Soporte técnico de Microsoft llama para confirmar que se ha devuelto.

Si ha activado el dispositivo en otra suscripción o ubicación, Microsoft moverá el pedido a la nueva suscripción o ubicación en el plazo de un día laborable. Una vez movido el pedido, puede eliminar este recurso.


Siga estos pasos para eliminar el dispositivo y el recurso en Azure Portal.

1. En Azure Portal, vaya al recurso y luego a **Introducción**. En la barra de comandos, seleccione **Eliminar**.

    ![Seleccionar Eliminar](media/azure-stack-edge-return-device/delete-resource-1.png)

2. En la hoja **Eliminar dispositivo**, escriba el nombre del dispositivo que quiere eliminar y seleccione **Eliminar**.

    ![Confirmar eliminación](media/azure-stack-edge-return-device/delete-resource-2.png)

Se le notificará una vez el dispositivo y el recurso asociado se hayan eliminado correctamente.

## <a name="get-a-replacement-device"></a>Obtener un dispositivo de sustitución

Se necesita un dispositivo de sustitución cuando el dispositivo existente tiene un error de hardware o necesita una actualización. Siga estos pasos cuando el dispositivo tenga un problema de hardware:

1. [Abra una incidencia de soporte técnico para el problema de hardware](#open-a-support-ticket). El equipo de Soporte técnico de Microsoft determinará si no está disponible para esta instancia una unidad de reemplazo de campo (FRU) o si el dispositivo necesita una actualización de hardware. En cualquier caso, el equipo de soporte técnico pedirá un dispositivo de sustitución.
2. [Cree un nuevo recurso](azure-stack-edge-deploy-prep.md#create-a-new-resource) para el dispositivo de sustitución. Asegúrese de activar la casilla **Tengo un dispositivo Azure Stack Edge** 
3. Después de recibir un dispositivo de sustitución, [instale](azure-stack-edge-deploy-install.md) y [active](azure-stack-edge-deploy-connect-setup-activate.md) el dispositivo de sustitución en el nuevo recurso.
4. Siga todos los pasos para devolver el dispositivo original:
    1. Abra otra incidencia de soporte técnico para devolver el dispositivo original.
    2. [Borre los datos del dispositivo](#erase-data-from-the-device).
    3. [Programe una recogida](#schedule-a-pickup).
    5. [Elimine el recurso](#delete-the-resource) asociado al dispositivo devuelto.



## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar el ancho de banda](azure-stack-edge-manage-bandwidth-schedules.md).
