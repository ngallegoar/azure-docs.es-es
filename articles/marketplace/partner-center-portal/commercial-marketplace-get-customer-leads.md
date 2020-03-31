---
title: Configurar clientes potenciales | Azure Marketplace
description: Configure clientes potenciales en Marketplace comercial.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: dsindona
ms.openlocfilehash: ce576d9825819770486197a6c39425adf6ac3208
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275907"
---
# <a name="customer-leads-from-your-marketplace-offer"></a>Clientes potenciales a partir de la oferta de Marketplace

Los clientes potenciales son clientes que están interesados en sus ofertas o que las implementan desde [Azure Marketplace](https://azuremarketplace.microsoft.com) o [AppSource](https://appsource.microsoft.com). Recibirá clientes potenciales cuando la oferta se publique en Marketplace. En este artículo se explica:

* El modo en que su oferta de Marketplace genera clientes potenciales, asegurándose de que no pierde oportunidades de negocio. 
* La conexión de su CRM a su oferta, de modo que pueda administrar los clientes potenciales en una ubicación central.
* La descripción de los datos de clientes potenciales que le enviamos, de modo que pueda hacer un seguimiento de los clientes que se comunican con usted.

## <a name="generate-customer-leads"></a>Generar clientes potenciales

Estos son los lugares en los que se genera un cliente potencial:

1. Cuando un cliente acepta compartir su información tras seleccionar "Contacto" en Marketplace. Este cliente potencial muestra un **interés inicial**; en este momento, compartimos información sobre el cliente que ha expresado interés en obtener su producto. El cliente potencial se encuentra en la parte superior del embudo que desemboca en la adquisición.

      ![Contacto de Dynamics 365](./media/commercial-marketplace-get-customer-leads/dynamics-365-contact-me.png)

2. Cuando un cliente selecciona "Obtenerla ahora" o "Crear" (en [Azure Portal](https://portal.azure.com/)) para obtener la oferta, este cliente potencial es **un cliente potencial activo**, donde compartimos información sobre un cliente que ha empezado a implementar el producto.

    ![Obtenerla ahora de SQL](./media/commercial-marketplace-get-customer-leads/sql-get-it-now.png)

    ![Crear de Windows Server](./media/commercial-marketplace-get-customer-leads/windows-server-create.png)

3. Un cliente toma una "versión de prueba" o inicia una "evaluación gratuita" de la oferta. Las versiones de prueba o evaluaciones gratuitas representan vías rápidas para compartir su negocio al instante con clientes potenciales sin barreras de entrada.

    ![Versión de prueba de Dynamics 365](./media/commercial-marketplace-get-customer-leads/dynamics-365-test-drive.png)

    ![Versión de prueba de Dynamics 365](./media/commercial-marketplace-get-customer-leads/dynamics-365-free-trial.png)

## <a name="connect-to-your-crm-system"></a>Conectarse a su sistema CRM

[!INCLUDE [Test drive content](./includes/connect-lead-management.md)]

## <a name="understand-lead-data"></a>Descripción de los datos de clientes potenciales

Todos los clientes potenciales que recibe durante el proceso de adquisición de clientes tienen datos en campos específicos. El primer campo donde buscar es `LeadSource`, que sigue este formato: **Origen-Acción** | **Oferta**.

**Orígenes**: El valor de este campo se rellena en función del Marketplace que generó el cliente potencial. Los valores posibles son `"AzureMarketplace"`, `"AzurePortal"` y `"AppSource (SPZA)"`.

**Acciones**: El valor de este campo se rellena en función de la acción realizada por el cliente en el Marketplace que generó el cliente potencial. 

Los valores posibles son:

- "INS": instalación. Esta acción se encuentra en Azure Marketplace o AppSource cuando un cliente adquiere su producto.
- "PLT": representa "prueba dirigida por asociado". Esta acción se encuentra en AppSource cuando un cliente usa la opción Ponerse en contacto conmigo.
- "DNC": significa "no establecer contacto". Esta acción se encuentra en AppSource cuando se solicita a un asociado al que se hace referencia cruzada en la página de la aplicación el establecimiento de contacto. En este momento, compartimos la advertencia de que este cliente tenía una referencia cruzada en la aplicación, pero no es necesario establecer contacto con él.
- "Crear": esta acción aparece solo en Azure Portal y se genera cada vez que un cliente adquiere su oferta para su cuenta.
- "StartTestDrive": esta acción está destinada solo a las versiones de prueba y se genera cuando un cliente inicia su versión de prueba.

**Ofertas**: Puede tener varias ofertas en Marketplace. El valor de este campo se rellena en función de la oferta que generó el cliente potencial. El identificador del publicador y el identificador de la oferta se envían en este campo y son los valores que usted proporcionó al publicar la oferta en Marketplace.

A continuación se muestran los valores de ejemplo en el formato esperado `publisherid.offerid`: 

1. `checkpoint.check-point-r77-10sg-byol`
1. `bitnami.openedxcypress`
1. `docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145`

## <a name="customer-info"></a>Información del cliente

La información del cliente se envía a través de varios campos. En el ejemplo siguiente se muestra la información del cliente que se encuentra en un cliente potencial.

- Nombre: John
- Apellidos: Smith
- Correo electrónico: jsmith\@microsoft.com
- Teléfono: 1234567890
- País: US
- Compañía: Microsoft
- Título: CTO

>[!Note]
>No todos los datos del ejemplo anterior están siempre disponibles para cada cliente potencial. Puesto que obtendrá clientes potenciales a partir de distintos pasos, tal como se ha mencionado en la sección Clientes potenciales, la mejor manera de administrarlos es desduplicar los registros y personalizar los seguimientos. De este modo, cada cliente obtiene un mensaje adecuado y se entabla una relación única.

## <a name="best-practices-for-lead-management"></a>Procedimientos recomendados para la administración de clientes potenciales

1. *Proceso*: Defina un proceso de ventas claro, con hitos y KPI, y una clara propiedad del equipo.
2. *Cualificación*: Defina los requisitos previos, que indican si un cliente potencial se ha cualificado por completo. Asegúrese de que los representantes de ventas o marketing califiquen a los clientes potenciales con cuidado antes de hacerlos pasar por el proceso de ventas completo.
3. *Seguimiento*: No olvide hacer un seguimiento y esperar que la transacción típica requiera de 5 a 12 llamadas de seguimiento.
4. *Consolidación*: Consolide a los clientes potenciales con el fin de encaminarse hacia un aumento del margen de ganancias.

## <a name="leads-frequently-asked-questions"></a>Preguntas más frecuentes sobre clientes potenciales

### <a name="where-can-i-get-help-in-setting-up-my-lead-destination"></a>¿Dónde puedo obtener ayuda para la configuración de mi destino de clientes potenciales?

Puede encontrar documentación [aquí](#connect-to-your-crm-system) o enviar una incidencia de soporte técnico mediante aka.ms/marketplacepublishersupport, y luego seleccionar **"creación de la oferta"** → **"tipo de oferta"** → **"configuración de administración de clientes potenciales"** .

### <a name="am-i-required-to-configure-a-lead-destination-in-order-to-publish-an-offer-on-the-marketplace"></a>¿Tengo que configurar un destino de clientes potenciales para poder publicar una oferta en Marketplace?

La respuesta depende del tipo de oferta que se publique. Las ofertas de SaaS y Dynamics 365 for Customer Engagement que se enumeran como "Contacto", todas las ofertas de Dynamics 365 for Operations, todas las ofertas de Dynamics 365 Business Central y todas las ofertas de Servicio de asesoría requieren una conexión a un destino de clientes potenciales. Si el tipo de oferta no aparece en la lista, no es necesario. Sin embargo, se recomienda configurar un destino de clientes potenciales para no perder oportunidades de negocio.

### <a name="how-can-i-find-the-test-lead"></a>¿Cómo puedo encontrar al cliente potencial de prueba?

Busque `"MSFT_TEST"` en el destino de clientes potenciales; a continuación, le mostramos un ejemplo de cliente potencial de prueba de Microsoft:

```
company = MSFT_TEST_636573304831318844
country = US
description = MSFT_TEST_636573304831318844
email = MSFT_TEST_636573304831318844@test.com
encoding = UTF-8
encoding = UTF-8
first_name = MSFT_TEST_636573304831318844
last_name = MSFT_TEST_636573304831318844
lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|<Offer Name>
oid = 00Do0000000ZHog
phone = 1234567890
title = MSFT_TEST_636573304831318844
```

### <a name="i-have-a-live-offer-but-im-not-seeing-any-leads"></a>Tengo una oferta activa, pero no veo a los clientes potenciales.

Asegúrese de que la conexión con el destino de clientes potenciales sea válida. Le enviaremos un cliente potencial de prueba después de hacer clic en Publicar en la oferta en el Centro de partners. Si ve el cliente potencial de prueba, la conexión es válida. También puede probar la conexión de su cliente potencial intentando adquirir la versión preliminar de la oferta durante el paso de versión preliminar; para ello, haga clic en "Obtenerla ahora", "Contacto" o "Evaluación gratuita" en la lista de Marketplace.

Además, asegúrese de estar buscando los datos correctos. El contenido de la sección [Descripción de los datos de clientes potenciales](#understand-lead-data) de este documento describe los datos de clientes potenciales que se envían al destino de clientes potenciales.

### <a name="i-have-configured-azure-blob-as-my-lead-destination-why-dont-i-see-the-lead"></a>He configurado el blob de Azure como destino de clientes potenciales, ¿por qué no veo al cliente potencial?

Ya no se admite el destino de clientes potenciales de blobs de Azure, por lo que le faltan clientes potenciales generados por la oferta. Cambie a cualquiera de las otras [opciones de destino de clientes potenciales](./commercial-marketplace-get-customer-leads.md). 

### <a name="i-received-an-email-from-marketplace-why-cant-i-find-the-lead-in-my-crm"></a>He recibido un correo electrónico desde Marketplace, ¿por qué no puedo encontrar al cliente potencial en mi instancia de CRM?

Es posible que el dominio de correo electrónico del usuario final sea .edu. Por motivos de privacidad, no se pasan los datos de información de identificación personal provenientes del dominio .edu. Envíe una incidencia de soporte técnico a través de aka.ms/marketplacepublishersupport.

### <a name="i-have-configured-azure-table-as-my-lead-destination-how-can-i-view-the-leads"></a>He configurado la tabla de Azure como destino de clientes potenciales, ¿cómo puedo ver a los clientes potenciales?

Puede acceder a los datos de clientes potenciales almacenados en la tabla de Azure desde Azure Portal, o puede descargar e instalar el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) gratuitamente para ver los datos de las tablas de su cuenta de Azure Storage.

### <a name="i-have-configured-azure-table-as-my-lead-destination-can-i-get-notified-whenever-a-new-lead-is-sent-by-marketplace"></a>He configurado la tabla de Azure como destino de clientes potenciales, ¿puedo recibir una notificación cada vez que Marketplace envíe un nuevo cliente potencial?

Sí, siga las [instrucciones](./commercial-marketplace-lead-management-instructions-azure-table.md) para configurar un flujo de Microsoft que envíe un correo electrónico si se agrega un cliente potencial a la tabla de Azure.

### <a name="i-have-configured-salesforce-as-my-lead-destination-why-cant-i-find-the-leads"></a>He configurado Salesforce como destino de clientes potenciales, ¿por qué no puedo encontrar a los clientes potenciales?

Compruebe si el formulario "de web a cliente potencial" es un campo obligatorio en función de la lista desplegable. En caso afirmativo, cámbielo por un campo de texto no obligatorio.

### <a name="there-was-an-issue-with-my-lead-destination-and-i-missed-some-leads-can-i-have-them-sent-to-me-in-an-email"></a>Hubo un problema con el destino de clientes potenciales y perdí algunos clientes potenciales. ¿Pueden enviármelos en un correo electrónico?

Debido a las directivas de PII (información de identificación personal), no podemos compartir la información de clientes potenciales a través de correo electrónico no protegido.

### <a name="i-have-configured-azure-table-as-my-lead-destination-how-much-will-it-cost"></a>He configurado una tabla de Azure como el destino de clientes potenciales, ¿cuánto va a costar?

Los datos de generación de clientes potenciales son pocos (menos de 1 GB para casi todos los anunciantes). El costo dependerá del número de clientes potenciales recibido; si se reciben 1000 clientes en un mes, el costo aproximado sería de 50 céntimos de USD. Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Si aún no encuentra la respuesta a su pregunta, póngase en contacto con Soporte técnico mediante aka.ms/marketplacepublishersupport, y luego seleccione **"creación de la oferta"** → **"tipo de oferta"** → **"configuración de administración de clientes potenciales"** . 

## <a name="next-steps"></a>Pasos siguientes

Una vez que esté implementada la configuración técnica, debería incorporar estos clientes potenciales a la estrategia actual de ventas y marketing y los procesos operativos. Estamos interesados en recibir una mejor perspectiva sobre el proceso de ventas global, y deseamos trabajar en estrecha colaboración con usted para conseguir clientes potenciales de alta calidad y una cantidad suficiente de datos que le permitan tener éxito. Agradecemos sus comentarios acerca de cómo podemos optimizar y mejorar los clientes potenciales que le enviamos con datos adicionales para obtener los mejores resultados de estos clientes. Díganos si está interesado en [ofrecer comentarios](mailto:AzureMarketOnboard@microsoft.com) y sugerencias para ayudar a que su equipo de ventas tenga mayor éxito con los clientes potenciales de Marketplace.
