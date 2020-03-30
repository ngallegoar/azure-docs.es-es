---
title: Configuración de la administración de clientes potenciales en Marketo | Azure Marketplace
description: Configure la administración de clientes potenciales en Marketo para clientes de Azure Marketplace.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: dsindona
ms.openlocfilehash: 35d57d117f6308863965ffd789c0e28bedd0f301
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281517"
---
# <a name="configure-lead-management-in-marketo"></a>Configuración de la administración de clientes potenciales en Marketo

En este artículo se describe cómo configurar el sistema CRM de Marketo para procesar clientes potenciales de la oferta de Marketplace.

## <a name="set-up-your-marketo-crm-system"></a>Configuración del sistema CRM de Marketo

1. Inicie sesión en Marketo.
2. Seleccione **Design Studio**.
    ![Marketo Design Studio](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-1.png)

3.  Seleccione **Nuevo formulario**.
    ![Nuevo formulario de Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-2.png)

4.  Rellene los campos obligatorios en el nuevo formulario y seleccione **Crear**.
    ![Crear nuevo formulario de Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-3.png)

5.  En Detalles de campo, seleccione **Finalizar**.
    ![Finalizar formulario de Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-4.png)

6.  Confirme y cierre.

7. En la pestaña *MarketplaceLeadBacked*, seleccione **Código para insertar**. 

    ![Código para insertar](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-6.png)

8. Código para insertar de Marketo muestra un código similar al ejemplo siguiente.

    ```
    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>
    ```

9. Copie los valores de los campos siguientes que se muestran en el formulario Código para insertar. Usará estos valores para configurar la oferta con el fin de recibir clientes potenciales en el siguiente paso. Use el ejemplo siguiente como guía para obtener los identificadores que necesita desde el ejemplo de código para insertar de Marketo.

    - Id. de servidor: **ys12**
    - Id. de Munchkin: **123-PQR-789**
    - Id. de formulario: **1179**

    **Otra forma de averiguar estos valores**

    - El id. de servidor se encuentra en la dirección URL de la instancia de Marketo, por ejemplo, "`serverID.marketo.com`".
    - Para obtener el id. de Munching de la suscripción, vaya al menú Administración > Munchkin del campo "Id. de cuenta de Munchkin" o desde la primera parte del subdominio del host de la API REST de Marketo: `https://{Munchkin ID}.mktorest.com`.
    - El id. de formulario es el identificador del formulario Código para insertar que ha creado en el paso 7 para enrutar los clientes potenciales desde Marketplace.

## <a name="configure-your-offer-to-send-leads-to-marketo"></a>Configuración de la oferta para enviar clientes potenciales al Marketo

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos: 

1. Vaya a la página **Configuración de la oferta** de su oferta.
1. Seleccione **Conectar** en la sección Administración de clientes potenciales. 

    ![Administración de clientes potenciales: Conectar](./media/commercial-marketplace-lead-management-instructions-marketo/lead-management-connect.png)

1. En la ventana emergente Detalles de conexión, seleccione **Marketo** como destino del cliente potencial.

    ![Elección de un destino de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-marketo/choose-lead-destination.png)

4. Proporcione el **id. de servidor**, el **id. de cuenta de Munching** y el **id. de formulario**.

    >[!Note]
    >Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta. 

