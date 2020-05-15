---
title: Tutorial sobre la devolución de Azure Data Box | Microsoft Docs
description: Aprenda a enviar un dispositivo Data Box a Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 09/20/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 03fe40cea5891e87f42f0d6a50eedee80a68f25e
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200535"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutorial: Devolución de Azure Data Box y comprobación de la carga de datos en Azure

::: zone-end

::: zone target="chromeless"

## <a name="return-data-box-and-verify-data-upload-to-azure"></a>Devolución de Data Box y comprobación de la carga de datos en Azure

::: zone-end

::: zone target="docs"

En este tutorial se describe cómo devolver Azure Data Box y comprobar los datos cargados en Azure.

En este tutorial, aprenderá sobre temas como:

> [!div class="checklist"]
> * Prerrequisitos
> * Preparación para el envío
> * Envío de Data Box a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de datos de Data Box

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, asegúrese de que:

- Ha completado el [Tutorial: Copia de datos a Azure Data Box y comprobación de](data-box-deploy-copy-data.md). 
- Los trabajos de copia están completos. Preparación para el envío no se puede ejecutar mientras que los trabajos de copia están en curso.

## <a name="prepare-to-ship"></a>Preparación para el envío

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

Una vez completada la copia de datos, prepare y envíe el dispositivo. Cuando el dispositivo llega al centro de datos de Azure, los datos se cargan automáticamente en Azure.

## <a name="prepare-to-ship"></a>Preparación para el envío

Antes de prepararse para enviar, asegúrese de que los trabajos de copia se han completado.

1. Vaya a la página **Preparar para enviar** de la interfaz de usuario web local y comience la preparación del envío. 
2. Desactive el dispositivo desde la interfaz de usuario web local. Quite los cables del dispositivo. 

Los siguientes pasos vienen determinados por el lugar al que se vaya a devolver el dispositivo.

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>Devolución de Data Box

Asegúrese de que la copia de datos se ha completado en el dispositivo y que la ejecución **Preparación para el envío** se ha realizado correctamente. Según la región a dónde envíe el dispositivo, el procedimiento es distinto.

::: zone-end

## <a name="in-us-canada-europe"></a>[En EE. UU., Canadá, Europa](#tab/in-us-canada-europe)

Realice los pasos siguientes si va a devolver el dispositivo en Estados Unidos, Canadá o Europa.

1. Asegúrese de que el dispositivo está apagado y de que se han quitado los cables. 
2. Enrolle y coloque de forma segura el cable de alimentación que se proporcionó junto con el dispositivo en la parte posterior del mismo.
3. Asegúrese de que la etiqueta de envío aparece en la pantalla de tinta electrónica y programe una recogida con su transportista. Si la etiqueta está dañada, se ha perdido o no aparece en la pantalla de tinta electrónica, póngase en contacto con el servicio de soporte técnico de Microsoft. Si el soporte técnico lo sugiere, puede ir a **Información general > Descargar la etiqueta de envío** en Azure Portal. Descargue la etiqueta de envío y péguela en el dispositivo. 
4. Programe una recogida con UPS si está devolviendo el dispositivo. Para programar una recogida:

    - Llame a la oficina local de UPS (número gratuito específico del país o región).
    - En la llamada, indique el número de seguimiento del envío inverso, que se muestra en la pantalla E-ink (Tinta electrónica) o la etiqueta impresa.
    - Si no se indica el número de seguimiento, UPS solicitará que el abono de una cantidad adicional en la recogida.

    En lugar de programar la recogida, también devolver la instancia de Data Box en la ubicación de recogida más cercana.
4. Una vez que el transportista recoge y examina el dispositivo Data Box, el estado del pedido en el portal se actualiza a **Picked up** (Recogido). También se muestra un identificador de seguimiento.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]



::: zone-end


## <a name="in-australia"></a>[En Australia](#tab/in-australia)

Los centros de datos de Azure en Australia tienen una notificación de seguridad adicional. Todos los envíos entrantes deben tener una notificación avanzada. Realice los pasos siguientes si el envío se realiza en Australia.


1. Conserve la caja original utilizada para devolver el dispositivo.
2. Asegúrese de que la copia de datos se ha completado en el dispositivo y que la ejecución **Preparación para el envío** se ha realizado correctamente.
3. Apague el dispositivo y quite los cables.
4. Enrolle y coloque de forma segura el cable de alimentación que se suministró junto con el dispositivo en la parte posterior del mismo.
5. Utilice el vínculo [DHL Link](https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference) para reservar en línea una recogida.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

## <a name="in-japan"></a>[En Japón](#tab/in-japan) 

1. Conserve la caja original utilizada para devolver el dispositivo.
2. Apague el dispositivo y quite los cables.
3. Enrolle y coloque de forma segura el cable de alimentación que se suministró junto con el dispositivo en la parte posterior del mismo.
4. Escriba el nombre y la dirección de la empresa en la nota de entrega como información del remitente.
5. Envíe un correo electrónico a Quantium Solutions mediante la plantilla de correo electrónico que tiene a continuación.

    - Tanto si no se incluyó la nota de entrega de Japan Post Chakubarai como si falta, especifíquelo en este correo electrónico. Quantium Solutions Japan se encargará de solicitar a Japan Post que le proporcionen una nota de entrega en la recogida.
    - Si tiene varios pedidos, envíe un correo electrónico para comprobar cada recogida individual.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

3. Recibirá un correo electrónico de confirmación de Quantium Solutions tras concertar una recogida. Este correo electrónico también incluye información sobre la nota de entrega de Chakubarai.

Si es necesario, puede ponerse en contacto con el soporte técnico de Quantium Solutions (en japonés) en: 

- Correo electrónico: Customerservice.JP@quantiumsolutions.com 
- Teléfono：03-5755-0150 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

## <a name="in-singapore"></a>[En Singapur](#tab/in-singapore) 

1. Conserve la caja original utilizada para devolver el dispositivo.
2. Anote el número de seguimiento (que se muestra como número de referencia en la página Preparación para el envío de la interfaz de usuario web local de Data Box). Estará disponible cuando el paso de preparación para el envío se complete correctamente. Descargue la etiqueta de envío de esta página y péguela en la caja de embalaje. 
3. Apague el dispositivo y quite los cables.
4. Enrolle y coloque de forma segura el cable de alimentación que se suministró junto con el dispositivo en la parte posterior del mismo. 
5. Envíe un correo electrónico al servicio de atención al cliente de SingPost utilizando la siguiente plantilla de correo electrónico con el número de seguimiento.

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pick-up - OrderName 
    Body: 
        1. Requestor name  
        2. Requestor contact number
        3. Requestor collection address
        4. Preferred collection date
    ```

   > [!NOTE]
   > Para las solicitudes de reserva recibidas en un día laborable:
   > - Antes de las 15:00 p.m., la recogida se realizará el siguiente día laborable entre las 9:00 a.m. y las 13:00 p. m.
   > - Después de las 15:00 p.m., la recogida se realizará el siguiente día laborable entre las 14:00 p.m. y las 18:00 p. m.  

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end


<!--## [In Korea](#tab/in-korea) 

1. Retain the original box used to ship the device for return shipment.
2. Note down the tracking number (shown as reference number on the Prepare to Ship page of the Data Box local web UI). This is available after the prepare to ship step successfully completes. Download the shipping label from this page and paste on the packing box. 
3. Power off the device and remove the cables.
4. Spool and securely place the power cord that was provided with the device in the back of the device. 

Request pickup  
If consignment note is present:  

1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number to arrange for a collection.
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.  
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.  
4. You will receive an email confirmation for the pickup schedule.  

Exception process
If the consignment note is not present:
1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number. Specify that you need a new consignment note to arrange for a collection. Provide sender (customer), receiver information (Azure datacenter), and reference number (service request number).
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.
4. You get a verbal confirmation if request is made via telephone.  
::: zone target="chromeless"

## Verify data upload to Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## Erasure of data from Data Box
 
Once the upload to Azure is complete, the Data Box erases the data on its disks as per the [NIST SP 800-88 Revision 1 guidelines](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end
-->

## <a name="self-managed"></a>[Autoadministrado](#tab/in-selfmanaged) 

Si usa Data Box en Japón, Singapur, Corea y Oeste de Europa, y ha seleccionado la opción de envío autoadministrado durante la creación del pedido, siga estas instrucciones. 

1. Una vez que este paso se complete correctamente, anote el código de autorización que se muestra en la página Preparación para el envío de la interfaz de usuario web local de Data Box.
2. Apague el dispositivo y quite los cables. Enrolle y coloque de forma segura el cable de alimentación que se suministró junto con el dispositivo en la parte posterior del mismo.
3. Envíe un correo electrónico al equipo de operaciones de Azure Data Box mediante la siguiente plantilla cuando esté listo para devolver el dispositivo.
    
    ```
    To: adbops@microsoft.com 
    Subject: Request for Azure Data Box drop-off for order: ‘orderName’ 
    Body: 
        1. Order name  
        2. Authorization code available after Prepare to Ship has completed [Yes/No]  
        3. Contact name of the person dropping off. You will need to display a Government approved ID during the drop off.
    ```

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

