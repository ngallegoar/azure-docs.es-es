---
title: Acceso a orígenes de datos en entornos locales
description: Conexión a orígenes de datos locales desde Azure Logic Apps mediante la creación de una puerta de enlace de datos local de Azure
services: logic-apps
ms.suite: integration
ms.reviewer: arthii, logicappspm
ms.topic: article
ms.date: 11/06/2019
ms.openlocfilehash: 29c1aaf18ea45d869d32a8817aeb03faa3b67c32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456582"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Conexión a orígenes de datos locales desde Azure Logic Apps

Antes de poder acceder a los orígenes de datos locales desde las aplicaciones lógicas, debe crear un recurso de Azure después de [instalar la *puerta de enlace de datos local* en un equipo local](../logic-apps/logic-apps-gateway-install.md). Después, las aplicaciones lógicas usan este recurso de puerta de enlace de Azure en los desencadenadores y las acciones proporcionadas por los [conectores locales](../connectors/apis-list.md#on-premises-connectors) que están disponibles para Azure Logic Apps.

En este artículo se muestra cómo crear el recurso de puerta de enlace de Azure para una [puerta de enlace instalada previamente en el equipo local](../logic-apps/logic-apps-gateway-install.md). Para más información sobre la puerta de enlace, consulte el [funcionamiento de la puerta de enlace](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service).

> [!TIP]
> Para conectarse a redes virtuales de Azure, considere la posibilidad de crear en su lugar un [*entorno del servicio de integración*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). 

Para información sobre cómo usar la puerta de enlace con otros servicios, consulte estos artículos:

* [Puerta de enlace de datos local de Microsoft Power Automate](/power-automate/gateway-reference)
* [Puerta de enlace de datos local de Microsoft Power BI](/power-bi/service-gateway-onprem)
* [Puerta de enlace de datos local de Microsoft Power Apps](/powerapps/maker/canvas-apps/gateway-reference)
* [Puerta de enlace de datos local de Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

<a name="supported-connections"></a>

## <a name="supported-data-sources"></a>Orígenes de datos admitidos

En Azure Logic Apps, la puerta de enlace de datos local admite los [conectores locales](../connectors/apis-list.md#on-premises-connectors) para estos orígenes de datos:

* BizTalk Server 2016
* Sistema de archivos
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Base de datos de Oracle
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

Azure Logic Apps admite operaciones de lectura y escritura a través de la puerta de enlace de datos. Sin embargo, estas operaciones tienen [límites en su tamaño de carga](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations). Aunque la puerta de enlace por sí sola no incurre en costos adicionales, el [modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md) se aplica a estos conectores y a otras operaciones en Azure Logic Apps.

## <a name="prerequisites"></a>Prerrequisitos

* Ya ha [instalado la puerta de enlace de datos local en un equipo local](../logic-apps/logic-apps-gateway-install.md).

* Usa la [misma cuenta y suscripción de Azure](../logic-apps/logic-apps-gateway-install.md#requirements) que se usó al instalar esta puerta de enlace de datos. Esta cuenta de Azure debe pertenecer a un único [inquilino o directorio de Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md#terminology).

* La instalación de la puerta de enlace aún no está registrada ni ha sido reclamada por otro recurso de puerta de enlace en Azure.

  Al crear un recurso de puerta de enlace en Azure Portal, selecciona una instalación de la puerta de enlace que está vinculada al recurso de puerta de enlace y solo a ese recurso. En Azure Logic Apps, los desencadenadores y las acciones locales usan el recurso de puerta de enlace para conectarse a orígenes de datos locales. En estos desencadenadores y acciones, selecciona su suscripción de Azure y el recurso de puerta de enlace asociado que quiere utilizar. Cada recurso de puerta de enlace está vinculado a una única instalación de la puerta de enlace, que está vinculada a una sola cuenta de Azure.

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>Creación de un recurso de puerta de enlace de Azure

Después de instalar la puerta de enlace en un equipo local, cree un recurso de Azure para la puerta de enlace.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con la misma cuenta de Azure que se usó para instalar la puerta de enlace.

1. En el cuadro de búsqueda de Azure Portal, escriba "puerta de enlace de datos local" y luego elija **Puertas de enlace de datos locales**.

   ![Busque "Puerta de enlace de datos local".](./media/logic-apps-gateway-connection/search-for-on-premises-data-gateway.png)

1. Busque **Puertas de enlace de datos locales** y seleccione **Agregar**.

   ![Agregue un nuevo recurso de Azure para la puerta de enlace de datos](./media/logic-apps-gateway-connection/add-azure-data-gateway-resource.png)

1. En **Crear puerta de enlace de conexión**, especifique la siguiente información el recurso de puerta de enlace. Seleccione **Crear** cuando haya terminado.

   | Propiedad | Descripción |
   |----------|-------------|
   | **Nombre de recurso** | Proporcione un nombre para el recurso de puerta de enlace, que contenga solo letras, números, guiones (`-`), caracteres de subrayado (`_`), paréntesis (`(`,`)`) o puntos (`.`). |
   | **Suscripción** | Seleccione la suscripción de Azure para la cuenta de Azure que se usó para la instalación de la puerta de enlace. La suscripción predeterminada se basa en la cuenta de Azure que usó para iniciar sesión. |
   | **Grupos de recursos** | El [grupo de recursos de Azure](../azure-resource-manager/management/overview.md) que quiere usar. |
   | **Ubicación** | La misma región o ubicación seleccionada para el servicio en la nube de la puerta de enlace durante la [instalación de la puerta de enlace](../logic-apps/logic-apps-gateway-install.md). De lo contrario, la instalación de la puerta de enlace no aparecerá en la lista **Nombre de la instalación**. La ubicación de la aplicación lógica puede diferir de la ubicación del recurso de la puerta de enlace. |
   | **Nombre de instalación** | Seleccione una instalación de la puerta de enlace, que aparece en la lista solo cuando se cumplen estas condiciones: <p><p>- La instalación de la puerta de enlace usa la misma región que el recurso de puerta de enlace que quiere crear. <br>- La instalación de la puerta de enlace no está vinculada a otro recurso de puerta de enlace de Azure. <br>- La instalación de la puerta de enlace está vinculada a la misma cuenta de Azure que está usando para crear el recurso de puerta de enlace. <br>- Su cuenta de Azure pertenece a un único [inquilino o directorio de Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md#terminology) y es la misma cuenta que se usó para la instalación de la puerta de enlace. <p><p>Para más información, consulte la sección [Preguntas frecuentes](#faq). |
   |||

   En este ejemplo se muestra una instalación de la puerta de enlace que se encuentra en la misma región que el recurso de puerta de enlace y que está vinculada a la misma cuenta de Azure:

   ![Proporcionar detalles para crear el recurso de la puerta de enlace de datos](./media/logic-apps-gateway-connection/on-premises-data-gateway-create-connection.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Conectarse a datos locales

Después de crear el recurso de puerta de enlace y asociar su suscripción de Azure con este recurso, ahora puede crear una conexión entre la aplicación lógica y el origen de datos local mediante el uso de la puerta de enlace.

1. En Azure Portal, cree o abra la aplicación lógica en el Diseñador de aplicación lógica.

1. Agregue un conector que admita conexiones locales como, por ejemplo, **SQL Server**.

1. Seleccione **Connect via on-premises data gateway** (Conectarse a través de la puerta de enlace de datos local).

1. En **Puertas de enlace**, en la lista **Suscripciones**, seleccione la suscripción de Azure con el recurso de puerta de enlace que desee.

1. En la lista **Puerta de enlace de conexión**, que muestra los recursos de puerta de enlace disponibles en la suscripción seleccionada, seleccione el recurso de puerta de enlace que desee. Cada recurso de puerta de enlace está vinculado a una única instalación de la puerta de enlace.

   > [!NOTE]
   > La lista de puertas de enlace incluye recursos de puerta de enlace de otras regiones porque la ubicación de la aplicación lógica puede diferir de la ubicación del recurso de la puerta de enlace. 

1. Proporcione un nombre de conexión único y otra información necesaria, que depende de la conexión que desee crear.

   Un nombre de conexión único le ayuda a identificar fácilmente la conexión más adelante, especialmente al crear varias conexiones. Si procede, incluya también el dominio completo para el nombre de usuario.

   Este es un ejemplo:

   ![Creación de conexiones entre la aplicación lógica y la puerta de enlace de datos](./media/logic-apps-gateway-connection/logic-app-gateway-connection.png)

1. Seleccione **Crear** cuando haya terminado.

La conexión de puerta de enlace ya está lista para que la use la aplicación lógica.

## <a name="edit-connection"></a>Edición de la conexión

Para actualizar la configuración de una conexión de puerta de enlace, puede editar la conexión.

1. Para encontrar todas las conexiones de API únicamente para su aplicación lógica, en el menú de la aplicación lógica, seleccione **Conexiones de API** en **Herramientas de desarrollo**.

   ![En el menú de la aplicación lógica, seleccione "Conexiones de API".](./media/logic-apps-gateway-connection/logic-app-api-connections.png)

1. Seleccione la conexión de puerta de enlace que desee y elija **Editar la conexión de API**.

   > [!TIP]
   > Si las actualizaciones no surten efecto, intente [detener y reiniciar la cuenta de servicio de Windows de la puerta de enlace](../logic-apps/logic-apps-gateway-install.md#restart-gateway) para su instalación de puerta de enlace.

Para encontrar todas las conexiones de API asociadas a su suscripción de Azure:

* En el menú de Azure Portal, seleccione **Todos los servicios** > **Web** > **Conexiones de API**.
* O bien, en el menú de Azure Portal, seleccione **Todos los recursos**. Establezca el filtro **Tipo** en **Conexión de API**.

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Eliminación del recurso de puerta de enlace

Para crear un recurso de puerta de enlace diferente, vincular la instalación de la puerta de enlace a un recurso de puerta de enlace diferente o quitar el recurso de puerta de enlace, puede eliminar el recurso de puerta de enlace sin que afecte a la instalación de la puerta de enlace.

1. En el menú de Azure Portal, seleccione **Todos los recursos** o busque y seleccione **Todos los recursos** en cualquier página. Busque y seleccione el recurso de puerta de enlace.

1. Si aún no está seleccionada, en el menú de recursos de puerta de enlace, seleccione **Puerta de enlace de datos local**. En la barra de herramientas de recursos de puerta de enlace, seleccione **Eliminar**.

   Por ejemplo:

   ![Elimine el recurso de puerta de enlace](./media/logic-apps-gateway-connection/delete-on-premises-data-gateway.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P.** : ¿Por qué no aparece mi instalación de la puerta de enlace al crear el recurso de puerta de enlace en Azure? <br/>
**R.** : Este problema puede deberse a estos motivos:

* Su cuenta de Azure debe ser la misma que la que está vinculada a la instalación de la puerta de enlace en el equipo local. Compruebe si ha iniciado sesión en Azure Portal con la misma identidad que está vinculada a la instalación de la puerta de enlace. Asegúrese de que su cuenta de Azure pertenece a un único [inquilino o directorio de Azure AD](../active-directory/fundamentals/active-directory-whatis.md#terminology) y de que es el mismo que se usó durante la instalación de la puerta de enlace.

* El recurso de puerta de enlace y la instalación de la puerta de enlace deben usar la misma región. No obstante, la ubicación de la aplicación lógica puede diferir de la ubicación del recurso de puerta de enlace.

* La instalación de la puerta de enlace ya está registrada y ha sido reclamada por otro recurso de puerta de enlace. Estas instalaciones no aparecerán en la lista **Nombre de instalación**. Para revisar los registros de puerta de enlace en Azure Portal, encuentre todos los recursos de Azure que tengan el tipo **Puertas de enlace de datos locales** en *todas* las suscripciones de Azure. Para desvincular la instalación de la puerta de enlace del otro recurso de puerta de enlace, consulte [Eliminación del recurso de puerta de enlace](#change-delete-gateway-resource).

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Protección de las aplicaciones lógicas](./logic-apps-securing-a-logic-app.md)
* [Ejemplos y escenarios habituales de las aplicaciones lógicas](./logic-apps-examples-and-scenarios.md)
