---
title: Configurar clientes potenciales | Azure Marketplace
description: Configure clientes potenciales en Cloud Partner Portal.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: 56012fb2a907a6db6f87554660ee36b99a3dcbf9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280327"
---
<a name="get-customer-leads"></a>Obtención de clientes potenciales
==================

En este artículo se explica cómo crear clientes potenciales mediante Cloud Partner Portal. Puede conectar estos clientes potenciales a su sistema CRM e integrarlos en su canalización de ventas.

## <a name="leads"></a>Clientes potenciales

Los clientes potenciales son clientes que están interesados en sus productos o que los están implementando desde [Azure Marketplace](https://azuremarketplace.microsoft.com/) o [AppSource](https://appsource.microsoft.com).

### <a name="azure-marketplace"></a>Azure Marketplace

1.  Un cliente usa una "versión de prueba" de su oferta. Las versiones de prueba representan una vía rápida para compartir su negocio al instante con clientes potenciales sin barreras de entrada. Todas las versiones de prueba convierten en cliente potencial al usuario que se interesa por probar su producto para obtener más información. Obtenga más información acerca de las versiones de prueba en [Azure Marketplace Test Drive](https://azuremarketplace.azureedge.net/documents/azure-marketplace-test-drive-program.pdf) (Versión de prueba de Azure Marketplace).

    ![Ejemplos de versión de prueba de Marketplace](./media/cloud-partner-portal-get-customer-leads/test-drive-offer.png)
 

<!-- -->

1. El cliente acepta compartir su información tras seleccionar "Obtenerla ahora". Este cliente potencial muestra un **interés inicial**; en este momento, compartimos información sobre el cliente que ha expresado interés en la obtención de su producto. El cliente potencial se encuentra en la parte superior del embudo que desemboca en la adquisición.

   ![Opción Obtenerla ahora](./media/cloud-partner-portal-get-customer-leads/get-it-now-button.png)

1. Cliente selecciona "Compra" en [Azure Portal](https://portal.azure.com/) para obtener el producto. Este cliente potencial ya se considera **activo**; en este momento, compartimos información sobre un cliente que ha empezado a implementar el producto.

   ![Opción Compra](./media/cloud-partner-portal-get-customer-leads/purchase-button.png)


### <a name="appsource"></a>AppSource

1.  Un cliente descargó una "versión de prueba" de su oferta. Las versiones de prueba representan una vía rápida para compartir su negocio al instante con clientes potenciales sin barreras de entrada. Todas las versiones de prueba convertirán en cliente potencial al usuario que se interese por probar su producto para obtener más información. Obtenga más información acerca de las versiones de prueba en [AppSource Test Drive](https://appsource.microsoft.com/blogs/want-to-try-an-app-take-a-test-drive) (Versión de prueba de AppSource).

    ![Ejemplo de prueba de unidad](./media/cloud-partner-portal-get-customer-leads/test-drive-offer-2.png)

2.  El cliente acepta compartir su información tras seleccionar "Obtenerla ahora". Este cliente potencial muestra un **interés inicial**; en este momento, compartimos información sobre el cliente que ha expresado interés en la obtención de su producto. El cliente potencial se encuentra en la parte superior del embudo que desemboca en la adquisición.

      ![Opción Obtenerla ahora](./media/cloud-partner-portal-get-customer-leads/get-it-now-button-2.png)


3.  El cliente selecciona "Ponerse en contacto conmigo" en su oferta. Este cliente potencial ya se considera **activo**; en este momento, compartimos información sobre un cliente que solicita seguir recibiendo información acerca de su producto.

    ![Opción Ponerse en contacto conmigo](./media/cloud-partner-portal-get-customer-leads/contact-me-image.png)

<a name="lead-data"></a>Datos de clientes potenciales
---------

Todos los clientes potenciales que recibe durante el proceso de adquisición de clientes tienen datos en campos específicos. Puesto que obtendrá clientes potenciales desde diferentes pasos, la mejor manera de administrarlos es desduplicar y personalizar los seguimientos. De este modo, cada cliente obtiene un mensaje adecuado y se entabla una relación única.

### <a name="lead-source"></a>Origen del cliente potencial

El formato del origen de un cliente potencial es **Origen**-**Acción** |  **Oferta**

**Orígenes**: "AzureMarketplace", "AzurePortal", "TestDrive" y "AppSource (SPZA)"

**Acciones**:
- "INS": instalación. Esta acción se encuentra en Azure Marketplace o AppSource cuando un cliente adquiere su producto.
- "PLT": representa "prueba dirigida por asociado". Esta acción se encuentra en AppSource cuando un cliente usa la opción Ponerse en contacto conmigo.
- "DNC": significa "no establecer contacto". Esta acción se encuentra en AppSource cuando se solicita a un asociado al que se hace referencia cruzada en la página de la aplicación el establecimiento de contacto. En este momento, compartimos la advertencia de que este cliente tenía una referencia cruzada en la aplicación, pero no es necesario establecer contacto con él.
- "Crear": esta acción aparece solo en Azure Portal y se genera cada vez que un cliente adquiere su oferta para su cuenta.
- "StartTestDrive": esta acción está destinada solo a las versiones de prueba y se genera cuando un cliente inicia su versión de prueba.

**Ofertas**

En los ejemplos siguientes se muestran los identificadores únicos que se asignan a un publicador y una oferta específica: checkpoint.check-point-r77-10sg-byol, bitnami.openedxcypress y docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145a.


### <a name="customer-info"></a>Información del cliente

Los campos del ejemplo siguiente muestran la información del cliente que se encuentra en un cliente potencial.
- Nombre: John
- Apellidos: Smith
- Correo electrónico: jsmith\@microsoft.com
- Teléfono: 1234567890
- País: US
- Compañía: Microsoft
- Título: CTO

>[!Note]
>No todos los datos del ejemplo anterior están siempre disponibles para cada cliente potencial.

Estamos trabajando activamente para mejorar los clientes potenciales, por lo que si hay un campo de datos que no ve aquí pero que le gustaría tener, [envíenos sus comentarios](mailto:AzureMarketOnboard@microsoft.com).

<a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Conexión del sistema CRM con Cloud Partner Portal
------------------------------------------------------------

Para empezar a recibir clientes potenciales, hemos creado nuestro conector Administración de clientes potenciales en Cloud Partner Portal para que pueda conectar fácilmente su información de CRM, proceso que se realizará automáticamente. Ahora puede aprovechar fácilmente los clientes potenciales generados por Marketplace sin un importante esfuerzo de ingeniería por integrarse con un sistema externo.

![Conector Administración de clientes potenciales](./media/cloud-partner-portal-get-customer-leads/lead-management-connector.png)

Podemos escribir clientes potenciales en una variedad de sistemas CRM o directamente en una tabla de Azure Storage desde donde puede administrar los clientes potenciales tal y como desee. Todos los vínculos siguientes proporcionan instrucciones para la conexión a destinos de clientes potenciales posibles:

-   [Dynamics CRM Online](./cloud-partner-portal-lead-management-instructions-dynamics.md) para obtener instrucciones sobre cómo configurar Dynamics CRM Online para obtener clientes potenciales.
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md) para obtener instrucciones que le permitan establecer la configuración de clientes potenciales de Marketo a fin de obtener clientes potenciales.
-    [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md) para obtener instrucciones sobre cómo configurar la instancia de Salesforce para obtener clientes potenciales.
-    [Tabla de Azure](./cloud-partner-portal-lead-management-instructions-azure-table.md) para obtener instrucciones para configurar la cuenta de Azure Storage para obtener clientes potenciales en una tabla de Azure.
-   [Punto de conexión HTTPS](./cloud-partner-portal-lead-management-instructions-https.md) para ver instrucciones que le permitan establecer la configuración de su punto de conexión HTTPS a fin de obtener clientes potenciales.

Después de configurar el destino de clientes potenciales y publicar su oferta, validaremos la conexión y le enviaremos un cliente potencial de prueba. Si está viendo la oferta antes de publicarla en Internet, también puede probar la conexión de los clientes potenciales tratando de adquirir la oferta en el entorno de versión preliminar. Es importante asegurarse de que la configuración de los clientes potenciales se mantenga actualizada para no perder ninguno de estos clientes; así pues, asegúrese de actualizar estas conexiones siempre que cambie algo por su parte.

<a name="what-next"></a>¿Qué más?
----------

Una vez que esté implementada la configuración técnica, debería incorporar estos clientes potenciales a la estrategia actual de ventas y marketing y los procesos operativos. Estamos muy interesados en recibir una mejor perspectiva sobre el proceso de ventas global, y deseamos trabajar en estrecha colaboración con usted para conseguir clientes potenciales de alta calidad y una cantidad suficiente de datos que le lleven al éxito. Agradecemos sus comentarios acerca de cómo podemos optimizar y mejorar los clientes potenciales que le enviamos con datos adicionales para obtener los mejores resultados de estos clientes. Díganos si está interesado en [ofrecer comentarios](mailto:AzureMarketOnboard@microsoft.com) y sugerencias para ayudar al equipo de ventas a ser más exitoso con los clientes potenciales de Marketplace.
