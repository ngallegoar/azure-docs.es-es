---
title: Configuración de la administración de clientes potenciales en Salesforce | Azure Marketplace
description: Configure la administración de clientes potenciales en Salesforce para clientes de Azure Marketplace.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: dsindona
ms.openlocfilehash: 1720026b4beff941b02a60cd1c755a043d66bdb5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281500"
---
# <a name="configure-lead-management-for-salesforce"></a>Configuración de la administración de clientes potenciales en Salesforce

En este artículo se describe cómo configurar el sistema de Salesforce para procesar clientes potenciales de la oferta de Marketplace.

## <a name="set-up-your-salesforce-system"></a>Configurar el sistema de Salesforce

1. Inicie sesión en Salesforce.
2. Si usa la experiencia Salesforce Lighting:
    1. Seleccione **Configuración** en la página principal de Salesforce.
    ![Configuración de Salesforce](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-1.png)

    1. En la página Configuración, navegue mediante el panel izquierdo a **Platform Tools (Herramientas de la plataforma) -> Configuración de la característica -> Marketing-> Web-to-Lead (Web para clientes potenciales)** .
    ![Web para clientes potenciales de Salesforce](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-2.png)

3. Si usa la experiencia Salesforce Classic:
    1. Seleccione **Configuración** en la página principal de Salesforce.
    ![Configuración de Salesforce Classic](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-setup.png)

    1. En la página Configuración, navegue mediante el panel izquierdo a **Compilación -> Personalizar -> Clientes potenciales -> Web-to-Lead (Web para clientes potenciales)** .
    ![Web para clientes potenciales de Salesforce Classic](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-web-to-lead.png)

El resto de las instrucciones son las mismas independientemente de la experiencia de Salesforce que esté usando.

4. En la **página Configuración de Web-to-Lead** (Web para clientes potenciales), seleccione el botón **Create Web-to-Lead Form** (Crear un formulario web para clientes potenciales).
5. En **Web-to-Lead Setup** (Configuración de web para clientes potenciales), seleccione **Create Web-to-Lead Form** (Crear formulario web para clientes potenciales).
    ![Salesforce: configuración de Web para clientes potenciales](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-3.png)

6. En **Create Web-to-Lead Form** (Crear un formulario web para clientes potenciales), asegúrese de que la opción `the Include reCAPTCHA in HTML` esté desactivada y seleccione **Generar**. 
    ![Salesforce: crear un formulario web para clientes potenciales](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-4.png)

7. Se le mostrará texto HTML. Busque el texto "oid" y copie el **valor de oid** del texto HTML (solo el texto entre comillas) y guárdelo. Pegue este valor en el campo **Identificador de la organización** en el portal de publicación.
    ![Salesforce: crear un formulario web para clientes potenciales](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-5.png)

8. Seleccione **Finalizado**.

## <a name="configure-your-offer-to-send-leads-to-salesforce"></a>Configurar la oferta para enviar clientes potenciales a Salesforce

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos:

1. Vaya a la página **Configuración de la oferta** de su oferta.
1. Seleccione **Conectar** en la sección Administración de clientes potenciales.
    ![Administración de clientes potenciales: Conectar](./media/commercial-marketplace-lead-management-instructions-salesforce/lead-management-connect.png)

1. En la ventana emergente Detalles de conexión, seleccione **Salesforce** para el valor de **Destino de clientes potenciales** y péguelo en `oid` desde el formulario web para clientes potenciales que creó siguiendo los pasos anteriores en el campo **Identificador de la organización**.

1. Seleccione **Guardar**. 

    >[!Note]
    >Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.

    ![Detalles de la conexión: elegir un destino de cliente potencial](./media/commercial-marketplace-lead-management-instructions-salesforce/choose-lead-destination.png)

    ![Detalles de la conexión: elegir un destino de cliente potencial](./media/commercial-marketplace-lead-management-instructions-salesforce/connection-details.png)
