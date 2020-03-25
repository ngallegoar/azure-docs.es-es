---
title: Tutorial para pedir Azure Data Box | Microsoft Docs
description: Obtenga información acerca de los requisitos previos de implementación y de cómo se debe pedir Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: b0204673c0706403c8c5a7367be19e590d9cb134
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "65604091"
---
# <a name="tutorial-order-azure-data-box"></a>Tutorial: Realización de pedidos de Azure Data Box

Azure Data Box es una solución híbrida que permite importar datos locales en Azure de una forma rápida, fácil y confiable. Transfiera los datos a un dispositivo de almacenamiento de 80 TB (capacidad utilizable) que suministra Microsoft y, después, devuelva el dispositivo. Luego, dichos datos se cargan en Azure.

En En este tutorial se describe cómo se puede solicitar Azure Data Box. En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
> * Requisitos previos para implementar Data Box
> * Realizar un pedido de Data Box
> * Seguimiento del pedido
> * Cancelar el pedido

## <a name="prerequisites"></a>Prerequisites

Antes de implementar el dispositivo, complete los siguientes requisitos previos de configuración tanto del servicio Data Box como del dispositivo.

### <a name="for-service"></a>Para el servicio

Antes de comenzar, asegúrese de que:
- Tiene una cuenta de almacenamiento de Microsoft Azure con credenciales de acceso.
- Asegúrese de que la suscripción que utilice para el servicio Data Box sea de uno de los siguientes tipos:
    - Contrato Enterprise (EA) de Microsoft. Más información acerca de [las suscripciones de EA](https://azure.microsoft.com/pricing/enterprise-agreement/).
    - Proveedor de soluciones en la nube (CSP). Más información acerca del [programa Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview).
    - Patrocinio de Microsoft Azure Obtenga más información sobre el [programa de patrocinio de Azure](https://azure.microsoft.com/offers/ms-azr-0036p/).

- Asegúrese de que tiene acceso de propietario o colaborador a la suscripción para crear un pedido de Data Box.

### <a name="for-device"></a>Para el dispositivo

Antes de comenzar, asegúrese de que:
- Tiene un equipo host conectado a la red del centro de datos. Data Box copiará los datos de dicho equipo. El equipo host debe ejecutar un sistema operativo compatible, como se describe en [Azure Data Box system requirements](data-box-system-requirements.md) (Requisitos del sistema de Azure Data Box).
- El centro de datos debe tener una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, se puede usar un vínculo de datos de 1 GbE, pero la velocidad de copia resultará afectada.


## <a name="order-data-box"></a>Realización de un pedido de Data Box

Para solicitar un dispositivo, siga estos pasos en Azure Portal.

1. Use sus credenciales de Microsoft Azure para iniciar sesión en esta dirección URL: [https://portal.azure.com](https://portal.azure.com).
2. Haga clic en **+ Crear un recurso** y busque *Azure Data Box*. Haga clic en **Azure Data Box**.
    
   [![Buscar Azure Data Box 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. Haga clic en **Crear**.

4. Compruebe si el servicio Data Box está disponible en su región. Escriba o seleccione la siguiente información y haga clic en **Aplicar**. 

    |Configuración  |Value  |
    |---------|---------|
    |Subscription     | Seleccione una suscripción patrocinada por EA, CSP o Azure para el servicio Data Box. <br> La suscripción está vinculada a la cuenta de facturación.       |
    |Tipo de transferencia     | Seleccione **Importar en Azure**.        |
    |País de origen     |   Seleccione el país o región en que residen los datos actualmente.         |
    |Región de Azure de destino     |     Seleccione la región de Azure a la que desea transferir los datos.        |

5. Seleccione Data **Box**. La capacidad máxima utilizable para un solo pedido es de 80 TB. Para tamaños de datos mayores puede crear varios pedidos.

      [![Seleccionar opción 1 de Data Box](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

6. En **Order** (Pedido), especifique el valor de **Order details** (Detalles del pedido). Escriba o seleccione la siguiente información y haga clic en **Siguiente**.
    
    |Configuración  |Value  |
    |---------|---------|
    |Nombre     |  Especifique un nombre descriptivo para hacer un seguimiento del pedido. <br> El nombre puede tener entre 3 y 24 caracteres que pueden ser letras, números y guiones. <br> El nombre debe empezar y terminar con una letra o un número.      |
    |Resource group     |   Uso uno existente o cree uno nuevo. <br> Un grupo de recursos es un contenedor lógico para los recursos que se pueden administrar o implementar conjuntamente.         |
    |Región de Azure de destino     | Seleccione la región de la cuenta de almacenamiento. <br> Para más información, vaya a [Disponibilidad por región](data-box-overview.md#region-availability).        |
    |Destino de almacenamiento     | Elija una cuenta de almacenamiento o discos administrados, o ambos. <br> En función de la región de Azure especificada, seleccione una o varias cuentas de almacenamiento en la lista filtrada una cuenta de almacenamiento existente. Data Box se puede vincular con un máximo de diez cuentas de almacenamiento. <br> También puede crear una nueva **cuenta de uso general v1**, **cuenta de uso general v2** o **cuenta de almacenamiento de blobs**. <br>Se admiten cuentas de almacenamiento con redes virtuales. Para permitir que el servicio de Data Box trabaje con cuentas de almacenamiento seguro, habilite los servicios de confianza dentro de la configuración de firewall de la red de la cuenta de almacenamiento. Para obtener más información, vea cómo [agregar Azure Data Box como un servicio de confianza](../storage/common/storage-network-security.md#exceptions).|

    Si usa la cuenta de almacenamiento como el destino de almacenamiento, consulte la siguiente captura de pantalla:

    ![Solicitud de Data Box para la cuenta de almacenamiento](media/data-box-deploy-ordered/order-storage-account.png)

    Si usa Data Box para crear discos administrados desde los discos duros virtuales locales, también deberá proporcionar la siguiente información:

    |Configuración  |Value  |
    |---------|---------|
    |Grupos de recursos     | Cree grupos de recursos si pretende crear discos administrados desde los discos duros virtuales locales. Puede usar un grupo de recursos existente solo si el grupo de recursos se creó anteriormente al crear un pedido de Data Box para el disco administrado por el servicio Data Box. <br> Especifique varios grupos de recursos separados por punto y coma. Se admite un máximo de 10 grupos de recursos.|

    ![Solicitud de Data Box para disco administrado](media/data-box-deploy-ordered/order-managed-disks.png)

    La cuenta de almacenamiento especificada para los discos administrados se usa como una cuenta de almacenamiento provisional. El servicio Data Box carga los discos duros virtuales como blob en páginas en la cuenta de almacenamiento provisional y, a continuación, los convierte en discos administrados y los mueve a los grupos de recursos. Para más información, vea [Comprobación de la carga de datos en Azure](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. En **Dirección de envío**, escriba su nombre y apellidos, el nombre y la dirección postal de la empresa y un número de teléfono válido. Haga clic en **Validar la dirección**. El servicio valida la dirección de envío para conocer la disponibilidad del servicio. Si el servicio está disponible para la dirección de envío especificada, recibirá una notificación al respecto. Haga clic en **Next**.

8. En **Detalles de la notificación**, especifique las direcciones de correo electrónico. El servicio envía notificaciones por correo electrónico si se produce cualquier actualización en el estado del pedido a las direcciones de correo electrónico especificadas.

    Es aconsejable usar un correo electrónico de grupo, con el fin de que siga recibiendo notificaciones aunque algún administrador deje el grupo.

9. Examine el **resumen** de la información relacionada con el pedido, el contacto, la notificación y los términos de privacidad. Active la casilla correspondiente a contrato acuerdo con los términos de privacidad.

10. Haga clic en **Pedido**. El pedido tarda unos minutos en crearse.


## <a name="track-the-order"></a>Seguimiento del pedido

Tras realizar el pedido, puede hacer un seguimiento del estado del mismo desde Azure Portal. Vaya al pedido de Data Box y, después, vaya a **Información general** para ver el estado. El portal muestra el pedido en estado **Ordered** (Pedido).

Si el dispositivo no está disponible, recibe una notificación. Si el dispositivo está disponible, Microsoft identifica que está listo para el envío y prepara dicho envío. Durante la preparación del dispositivo, se producen las siguientes acciones:

- Se crean recursos compartidos de SMB para cada cuenta de almacenamiento asociada con el dispositivo.
- Para cada cuenta, se generan las credenciales de acceso, como el nombre de usuario y la contraseña.
- También se genera la contraseña del dispositivo, que le ayuda a desbloquear el dispositivo.
- Data Box está bloqueado para evitar el acceso no autorizado al dispositivo en todo momento.

Una vez que se completa la preparación del dispositivo, el portal muestra el pedido en estado **Processed** (Procesado).

![Pedido de Data Box procesado](media/data-box-overview/data-box-order-status-processed.png)

Luego, Microsoft prepara y envía el disco a través de un operador regional. Una vez que se realiza el envío, recibe un número de seguimiento. El portal muestra el orden en estado **Dispatched** (Enviado).

![Pedido de Data Box enviado](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>Cancelar el pedido

Para cancelar el pedido, en Azure Portal, vaya a **Información general** y haga clic en **Cancelar** en la barra de comandos.

Después de realizar un pedido, puede cancelarlo en cualquier momento, siempre que no se encuentre en estado procesado.
 
Para eliminar un pedido cancelado, vaya a **Información general** y haga clic en **Eliminar** desde la barra de comandos.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Requisitos previos para implementar Data Box
> * Realización de un pedido de Data Box
> * Seguimiento del pedido
> * Cancelar el pedido

En el siguiente tutorial aprenderá a configurar Data Box.

> [!div class="nextstepaction"]
> [Configuración de Azure Data Box](./data-box-deploy-set-up.md)


