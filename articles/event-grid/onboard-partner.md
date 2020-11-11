---
title: Incorporación como asociado de Azure Event Grid mediante Azure Portal
description: Use Azure Portal para incorporar a un asociado de Azure Event Grid.
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 36fab35923b8a536a9054e5dc4bfa4c5b82172a7
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93102714"
---
# <a name="onboard-as-an-azure-event-grid-partner-using-the-azure-portal"></a>Incorporación como asociado de Azure Event Grid mediante Azure Portal
En este artículo se describe cómo los proveedores de SaaS de terceros, también conocidos como asociados o publicadores de eventos, se incorporan a Event Grid para poder publicar eventos desde sus servicios y cómo los usuarios finales consumen esos eventos.

> [!IMPORTANT]
> Si no está familiarizado con la característica Eventos de asociado, consulte el artículo de [información general de Eventos de asociado](partner-events-overview.md) para una introducción detallada de los conceptos clave que son críticos para comprender y seguir los pasos de este artículo.

## <a name="onboarding-process-for-event-publishers-partners"></a>Proceso de incorporación para publicadores de eventos (asociados)
En resumen, la habilitación de los eventos del servicio para que los consuman los usuarios suele implicar el proceso siguiente:

1. **Comunique su interés** en convertirse en asociado al equipo de servicio de Event Grid antes de continuar con los próximos pasos.
1. Cree un **registro** para crear un tipo de tema de asociado. 
1. Cree un **espacio de nombres**.
1. Cree un **canal de eventos** y un **tema de asociado** (paso único).
1. Pruebe de manera integral la funcionalidad Eventos de asociado.

En el paso 4, debe decidir que clase de experiencia de usuario quiere proporcionar. Tiene las siguientes opciones:
- Proporcione su propia solución, por lo general una experiencia de interfaz gráfica de usuario (GUI) web, hospeda en su dominio con nuestra SDK o API REST para crear un canal de eventos y su tema de asociado correspondiente. Con esta opción, puede pedirle al usuario la suscripción y el grupo de recursos en el que va a crear un tema de asociado.
- Use Azure Portal o la CLI de Azure para crear el canal de eventos y el tema de asociado correspondiente. Con esta opción, debe tener acceso de algún modo a la suscripción de Azure del usuario y al grupo de recursos en el que creará un tema de asociado. 

En este artículo se muestra cómo realizar la incorporación como asociado de Azure Event Grid mediante Azure Portal. 

> [!NOTE]
> El registro de un tipo de tema de asociado es un paso opcional. Para ayudarlo a decidir si debe crear un tipo de tema de asociado, consulte la sección sobre [Recursos administrados por el publicador de eventos](partner-events-overview.md#resources-managed-by-event-publishers).

## <a name="communicate-your-interest-in-becoming-a-partner"></a>Comunique su interés en convertirse en asociado
Rellene [este formulario](https://aka.ms/gridpartnerform) y póngase en contacto con el equipo de Event Grid en [GridPartner@microsoft.com](mailto:GridPartner@microsoft.com). Conversaremos con usted para compartir información detallada sobre los casos de uso, los roles, el proceso de incorporación, la funcionalidad y el precio de Eventos de asociado, entre otros.

## <a name="prerequisites"></a>Requisitos previos
Para completar los pasos restantes, asegúrese de tener lo siguiente:

- Suscripción a Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Un [inquilino](../active-directory/develop/quickstart-create-new-tenant.md) de Azure.

## <a name="register-a-partner-topic-type-optional"></a>Registre un tipo de tema de asociado (opcional)
1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el panel de navegación de la izquierda, seleccione **Todos los servicios** , escriba **Registros de asociado de Event Grid** en la barra de búsqueda y selecciónelo. 
1. En la página **Registros de asociado de Event Grid** , seleccione **+ Agregar** en la barra de herramientas. 

    :::image type="content" source="./media/onboard-partner/add-partner-registration-link.png" alt-text="Vínculo para agregar registro de asociado":::
1. En la página **Creación de registros de tipo de tema de asociado: aspectos básicos** , escriba esta información: 
    1. En la sección **Detalles del proyecto** , siga estos pasos:
        1. Selección la **suscripción** de Azure. 
        1. Seleccione un **grupo de recursos** de Azure existente o cree uno nuevo. 
    1. En la sección **Detalles del registro** , siga estos pasos:
        1. En **Nombre del registro** , escriba un nombre para el registro. 
        1. En **Nombre de la organización** , escriba el nombre de la organización. 
    1. En la sección **Tipo de recurso de asociado** , escriba detalles sobre el tipo de recurso que se mostrarán en la página de **creación del tema de asociado** : 
        1. En **Nombre del tipo de recurso de asociado** , escriba el nombre del tipo de recurso. Este será el tipo de tema de asociado que se creará en la suscripción de Azure. 
        2. En **Nombre para mostrar** , escriba un nombre descriptivo para mostrar para el tipo de tema de asociado (recurso). 
        3. Escriba una **descripción para el tipo de recurso**. 
        4. Escriba una **descripción para el escenario**. Debe explicar las maneras o los escenarios en los que se pueden usar los temas de asociados para los recursos.  

            :::image type="content" source="./media/onboard-partner/create-partner-registration-page.png" alt-text="Creación de registro de asociado":::            
1. Seleccione **Siguiente: Servicio personalizado** en la parte inferior de la página. En la pestaña **Servicio personalizado** de la página **Creación de registro de asociado** , escriba información que los usuarios suscriptores usarán para ponerse en contacto con usted en caso de problemas con el origen del evento:
    1. Escriba el **número de teléfono**.
    1. Escriba la **extensión** del número de teléfono.
    1. Escriba la **dirección URL** de un sitio web de soporte. 
    
        :::image type="content" source="./media/onboard-partner/create-partner-registration-customer-service.png" alt-text="Creación de registro de asociado: servicio personalizado":::        
1. Seleccione **Siguiente: Etiquetas** en la parte inferior de la página. 
1. En la página **Etiquetas** , configure estos valores. 
    1. Escriba un **nombre** y un **valor** para la etiqueta que desea agregar. Este paso es **opcional**. 
    1. Seleccione **Revisar y crear** en la parte inferior de la página para crear el registro (tipo de tema de asociado).

## <a name="create-a-partner-namespace"></a>Creación de un espacio de nombres de asociado

1. En Azure Portal, seleccione **Todos los servicios** en el menú de navegación de la izquierda, luego escriba **Espacios de nombres de asociado de Event Grid** en la barra de búsqueda y, luego, selecciónelo en la lista. 
1. En la página **Espacios de nombres de asociado de Event Grid** , seleccione **+ Agregar** en la barra de herramientas. 
    
    :::image type="content" source="./media/onboard-partner/add-partner-namespace-link.png" alt-text="Espacios de nombres de asociado: vínculo Agregar":::
1. En la página **Creación de un espacio de nombres de asociado: aspectos básicos** , especifique esta información.
    1. En la sección **Detalles del proyecto** , siga estos pasos: 
        1. Seleccione una **suscripción** de Azure.
        1. Seleccione un **grupo de recursos** existente o cree uno nuevo. 
    1. En la sección **Detalles del espacio de nombres** , siga estos pasos:
        1. Escriba el **nombre** del espacio de nombres. 
        1. Seleccione una **ubicación** para el espacio de nombres. 
    1. En la sección **Detalles del registro** , siga estos pasos para asociar el espacio de nombres con un registro de asociado. 
        1. Seleccione la **suscripción** en la que existe el registro de asociado. 
        1. Seleccione el **grupo de recursos** que contiene el registro de asociado. 
        1. Seleccione el **registro de asociado** en la lista desplegable.
    1. Seleccione **Siguiente: Etiquetas** en la parte inferior de la página.

        :::image type="content" source="./media/onboard-partner/create-partner-namespace-basics-page.png" alt-text="Página Creación de un espacio de nombres de asociado: aspectos básicos":::
1. En la página **Etiquetas** , agregue etiquetas (opcional).
    1. Escriba un **nombre** y un **valor** para la etiqueta que desea agregar. Este paso es **opcional**.
    1. En la parte inferior de la página, seleccione **Revisar y crear**.         
1. En la página **Revisar y crear** , revise los detalles y seleccione **Crear**. 

## <a name="create-an-event-channel"></a>Creación de un canal de eventos
> [!IMPORTANT]
> Tendrá que solicitar al usuario una suscripción de Azure, un grupo de recursos, una ubicación y un nombre de tema de asociado para crear un tema de asociado que el usuario poseerá y administrará.

1. Vaya a la página **Información general** del espacio de nombres que creó. 

    :::image type="content" source="./media/onboard-partner/partner-namespace-overview.png" alt-text="Página Espacio de nombres de asociado: información general":::
    partner-namespace-overview.png
1. Seleccione **+ Canal de eventos** en la barra de herramientas. 
1. En la página **Creación de un canal de eventos: aspectos básicos** , especifique esta información. 
    1. En la sección **Detalles del canal** , siga estos pasos:
        1. En **Nombre del canal de eventos** , escriba un nombre para el canal de eventos. 
        1. Escriba el **origen**. Consulte las [especificaciones de CloudEvents 1.0](https://github.com/cloudevents/spec/blob/v1.0/spec.md#source-1) para obtener una idea de un valor adecuado para el origen. Consulte también [este ejemplo de esquema de CloudEvents](cloud-event-schema.md#sample-event-using-cloudevents-schema).
        1. Escriba el origen (¿QUÉ ES ESTO?).
    1. En la sección **Detalles del destino** , escriba los detalles del tema de asociado de destino que se creará para este canal de eventos. 
        1. Escriba el **id. de la suscripción** en la que se creará el tema de asociado. 
        1. Escriba el **nombre del grupo de recursos** en el que se creará el recurso de tema de asociado. 
        1. Escriba un **nombre para el tema de asociado**. 
    1. Seleccione **Siguiente: Filtros** en la parte inferior de la página. 
    
        :::image type="content" source="./media/onboard-partner/create-event-channel-basics-page.png" alt-text="Página Creación de un canal de eventos: aspectos básicos":::
1. En la página **Filtros** , agregue filtros. siga estos pasos:
    1. Filtre según los atributos de cada evento. Solo se entregan los eventos que coinciden con todos los filtros. Se pueden especificar hasta 25 filtros. Las comparaciones no distinguen entre mayúsculas y minúsculas. Las claves válidas usadas para los filtros varían en función del esquema de eventos. En el ejemplo siguiente, `eventid`, `source`, `eventtype` y `eventtypeversioin` se pueden usar para las claves. También puede usar las propiedades personalizadas dentro de la carga de datos al usar `.` como el operador anidado. Por ejemplo: `data`, `data.key`, `data.key1.key2`.
    1. Seleccione **Siguiente: Características adicionales** en la parte inferior de la página. 
    
        :::image type="content" source="./media/onboard-partner/create-event-channel-filters-page.png" alt-text="Página Creación de un canal de eventos: filtros":::
        create-event-channel-filters-page.png
1. En la página **Características adicionales** , puede establecer un **plazo de expiración** y una **descripción para el tema de asociado**. 
    1. El **plazo de expiración** es el plazo después del cual se eliminarán automáticamente el tema y su canal de eventos asociado si no los activa el cliente. Si no se proporciona un plazo, se usa un valor predeterminado de siete días. Active la casilla para especificar su propio plazo de expiración. 
    1. Como este tema es un recurso no creado por el usuario, una **descripción** puede ayudar al usuario a entender la naturaleza del tema. Si no se establece ninguna, se proporcionará una descripción genérica. Active la casilla para establecer su propia descripción de tema de asociado. 
    1. Seleccione **Siguiente: Review + create** (Revisar y crear). 
    
        :::image type="content" source="./media/onboard-partner/create-event-channel-additional-features-page.png" alt-text="Página Creación de un canal de eventos: características adicionales":::
1. En **Revisar y crear** , revise la configuración y seleccione **Crear** para crear el canal de eventos. 

## <a name="next-steps"></a>Pasos siguientes
- [Introducción a los temas de asociados](partner-topics-overview.md)
- [Formulario de incorporación de temas de asociados](https://aka.ms/gridpartnerform)
- [Tema de asociado de Auth0](auth0-overview.md)
- [Procedimientos para usar el tema de asociado de Auth0](auth0-how-to.md)
