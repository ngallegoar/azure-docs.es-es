---
title: Tutorial sobre la devolución de Azure Data Box Disk | Microsoft Docs
description: Use este tutorial para saber cómo enviar Azure Data Box Disk a Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 05/08/2020
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 6b165df42da35077c7ef252291d8e1514bb3b1b0
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82995997"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-disk"></a>Tutorial: Devolución de Azure Data Box Disk 

En este tutorial se describe cómo programar una recogida para devolver la instancia de Azure Data Box Disk. Las instrucciones de recogida dependen de dónde se devuelva el dispositivo. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Envío de Data Box Disk a Microsoft
> * Recogida de Data Box Disk en distintas regiones

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, asegúrese de que ha completado [Tutorial: Copia de datos a Azure Data Box Disk y comprobación de los mismos](data-box-disk-deploy-copy-data.md).


## <a name="ship-data-box-disk-back"></a>Devolución de Data Box Disk

::: zone-end

::: zone target="chromeless"

## <a name="return-azure-data-box-disk"></a>Devolución de Azure Data Box Disk 

::: zone-end

1. Una vez completada la validación de datos, desconecte los discos. Quite los cables de conexión.
2. Envuelva todos los discos y los cables de conexión en un envoltorio de burbujas y colóquelos en la caja de envío. La falta de accesorios puede acarrear un costo.
    - Utilice el empaquetado que se usó en el envío inicial.  
    - Es aconsejable usar un envoltorio de burbujas bien protegido para empaquetar los discos.
    - Asegúrese de que todo encaja perfectamente para reducir los movimientos dentro de la caja.

Los siguientes pasos vienen determinados por el lugar al que se vaya a devolver el dispositivo. Las instrucciones son diferentes para EE. UU. o Canadá, Unión Europea (UE), Australia o países o regiones de Asia.

### <a name="in-us-or-canada"></a>[En Estados Unidos o Canadá](#tab/in-us-or-canada)

Realice los pasos siguientes si va a devolver el dispositivo en Estados Unidos o Canadá.

1. Utilice la etiqueta de envío de devolución en la funda plástica transparente fijada en la caja. Si la etiqueta se daña o se pierde:
    - Vaya a **Información general > Descargar la etiqueta de envío** y descargar una etiqueta de envío de devolución.
    - Pegue la etiqueta en el dispositivo.

2. Selle la caja de envío y asegúrese de que la etiqueta de envío de devolución está visible.
3. Póngase en contacto con UPS para programar la recogida del dispositivo. Para programar una recogida:

    - Llame a la oficina local de UPS (número gratuito específico del país o región).
    - En la llamada, indique el número de seguimiento del envío inverso, que se muestra en la etiqueta impresa.
    - Si no se indica el número de seguimiento, UPS solicitará que el abono de una cantidad adicional en la recogida.
    - En lugar de programar la recogida, también puede devolver la instancia de Data Box Disk en el punto de recogida más cercano.

### <a name="in-europe"></a>[En Europa](#tab/in-europe)

Realice los pasos siguientes si va a devolver el dispositivo en Europa.

1. Utilice la etiqueta de envío de devolución en la funda plástica transparente fijada en la caja. Si la etiqueta se daña o se pierde:
    - Vaya a **Información general > Descargar la etiqueta de envío** y descargar una etiqueta de envío de devolución.
    - Pegue la etiqueta en el dispositivo.

2. Selle la caja de envío y asegúrese de que la etiqueta de envío de devolución está visible.
3. Si va a devolver el dispositivo en Europa con DHL, para solicitar la recogida de DHL, visite su sitio Web y especifique el número de factura de porte aéreo.
4. Visite el sitio Web de DHL Express del país o región y elija **Book a Courier Collection > eReturn Shipment** (Reservar una colección Courier > Envío eReturn).    
3. Especifique el número de factura de porte aéreo y haga clic en **Schedule Pickup** (Programar la recogida) para organizar la recogida.

### <a name="in-australia"></a>[En Australia](#tab/in-australia)

Los centros de datos de Azure en Australia tienen una notificación de seguridad adicional. Todos los envíos entrantes deben tener una notificación avanzada. Realice los pasos siguientes para la recogida en Australia.

1. Use la etiqueta de devolución que se proporciona y asegúrese de en ella aparece el código TAU (número de referencia). Si falta la etiqueta de envío o tiene cualquier otro problema, envíe un correo electrónico al departamento de [operaciones en Asia de Data Box](mailto:adbo@microsoft.com). Especifique el nombre del pedido en el encabezado del asunto y los detalles del problema que haya aparecido.
3. Pegue la etiqueta en la caja. 
4. Utilice el vínculo https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference para reservar en línea una recogida. 

### <a name="in-japan"></a>[En Japón](#tab/in-japan)

1. Escriba el nombre y la dirección de la empresa en la nota de entrega como información del remitente.
2. Envíe un correo electrónico a Quantium Solutions mediante la plantilla de correo electrónico que tiene a continuación.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Microsoft Azure Data Box Disk｜Job Name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
        a. 08：00-13：00 
        b. 13：00-15：00 
        c. 15：00-17：00 
        d. 17：00-19：00 
    ```
    - **Si va a recoger en Osaka**, modifique el asunto de la plantilla de correo electrónico por `Pickup request for Microsoft Azure OSA`.
    - Tanto si no se incluyó la nota de entrega de Japan Post Chakubarai como si falta, especifíquelo en este correo electrónico. Quantium Solutions Japan se encargará de solicitar a Japan Post que le proporcionen una nota de entrega en la recogida.
    - Si tiene varios pedidos, envíe un correo electrónico para comprobar cada recogida individual.

3. Recibirá un correo electrónico de confirmación de Quantium Solutions tras concertar una recogida. Este correo electrónico también incluye información sobre la nota de entrega de Chakubarai.

Si es necesario, puede ponerse en contacto con el soporte técnico de Quantium Solutions (en japonés) en: 

- Correo electrónico: Customerservice.JP@quantiumsolutions.com 
- Teléfono：03-5755-0150 

### <a name="in-korea"></a>[En Corea](#tab/in-korea)

1. Asegúrese de incluir la nota de entrega de la devolución.
2. Para solicitar la recogida con la nota de entrega:
    1. Llame a la línea directa de *Quantium Solutions International*, 070-8231-1418, en horario de oficina (de 10 AM a 5 PM, de lunes a viernes). Indique *Microsoft Azure pickup* y el número de solicitud de servicio para organizar la recogida.  
    2. Si la línea directa está ocupada, envíe un mensaje de correo electrónico a `microsoft@rocketparcel.com` con el asunto *Microsoft Azure Pickup* y el número de solicitud de servicio como referencia.
    3. Si el mensajero no realiza la recogida, llame a la línea directa de *Quantium Solutions International* para buscar otra fecha. 
    4. Recibirá una confirmación por correo electrónico para concertar la recogida.
3. Siga este paso solo si la nota de entrega no está presente. Para solicitar la recogida:
    1. Llame a la línea directa de *Quantium Solutions International*, 070-8231-1418, en horario de oficina (de 10 AM a 5 PM, de lunes a viernes). Indique *Microsoft Azure pickup* y el número de solicitud de servicio para organizar la recogida. Especifique que necesita una nueva nota de entrega para concertar una recogida. Proporcione la información sobre el remitente (cliente) y el receptor (centro de datos de Azure), así como el número de referencia (número de solicitud de servicio). 
    2. Si la línea directa está ocupada, envíe un mensaje de correo electrónico a `microsoft@rocketparcel.com` con el asunto *Microsoft Azure Pickup* y el número de solicitud de servicio como referencia.
    3. Si el mensajero no realiza la recogida, llame a la línea directa de *Quantium Solutions International* para buscar otra fecha. 
    4. Recibirá una confirmación verbal si la solicitud se realiza por teléfono.


### <a name="in-singapore"></a>[En Singapur](#tab/in-singapore)

1. Imprima la etiqueta de envío y colóquela en la caja. Si la etiqueta se daña o se pierde:
    - Vaya a **Información general > Descargar la etiqueta de envío** y obtenga una etiqueta de envío de devolución.
    - Pegue la etiqueta en el dispositivo. Asegúrese de que la etiqueta está visible.

2. Para solicitar la recogida, envíe un correo electrónico al servicio de atención al cliente de SingPost mediante la siguiente plantilla con el número de seguimiento (el número de seguimiento se puede encontrar en la etiqueta de retorno que se encuentra en el paquete entregado).

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pick-up - XZ00001234567 
    Body: 
     a. Requestor name
     b. Requestor contact number
     c. Requestor collection address
     d. Preferred collection date
    ```

   > [!NOTE]
   > Para las solicitudes de reserva recibidas en un día laborable:
   > - Antes de las 15:00 p.m., la recogida se realizará el siguiente día laborable entre las 9:00 a.m. y las 13:00 p. m.
   > - Después de las 15:00 p.m., la recogida se realizará el siguiente día laborable entre las 14:00 p.m. y las 18:00 p. m.

   Si surge algún problema, póngase en contacto con Data Box Operations Asia a la siguiente dirección de correo electrónico, adbo@microsoft.com. Especifique el nombre del trabajo en el encabezado del asunto y el problema que ha aparecido.

3. Realice la entrega al mensajero.

### <a name="in-self-managed"></a>[En entorno autoadministrado](#tab/in-selfmanaged)

Si usa Data Box Disk en Japón, Singapur, Corea y Europa Occidental, y ha seleccionado la opción de envío autoadministrada durante la creación del pedido, siga estas instrucciones. 

1. Vaya a la hoja **Información general** del pedido en Azure Portal. Siga las instrucciones que se muestran al seleccionar **Programar recogida**. Debería ver un código de autorización que se utiliza en el momento de dejar el pedido.

2. Envíe un correo electrónico al equipo de operaciones de Azure Data Box mediante la siguiente plantilla cuando esté listo para devolver el dispositivo.

    ```
    To: adbops@microsoft.com
    Subject: Request for Azure Data Box Disk drop-off for order: ‘orderName’
    Body: 
     a. Order name
     b. Contact name of the person dropping off. You will need to display a Government approved ID during the drop off.
    ```
3. El equipo de operaciones de Azure Data Box trabajará con usted para organizar la devolución en el centro de datos de Azure.

::: zone target="docs"

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido sobre temas relacionados con Azure Data Box Disk; por ejemplo:

> [!div class="checklist"]
> * Envío de Data Box Disk a Microsoft
> * Recogida de Data Box Disk en distintas regiones

Continúe con el siguiente procedimiento para obtener información sobre cómo comprobar la carga de datos de Data Box Disk en la cuenta de Azure Storage.

> [!div class="nextstepaction"]
> [Comprobación de la carga de datos desde Azure Data Box Disk](./data-box-disk-deploy-picked-up.md)

::: zone-end




