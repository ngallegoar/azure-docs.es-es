---
title: 'Conector de Administración de servicios de TI: Exportación segura en Azure Monitor'
description: En este artículo se muestra cómo conectar los productos o servicios de ITSM con Exportación seguridad en Azure Monitor para supervisar y administrar de manera centralizada los elementos de trabajo de ITSM.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: bf68963515e1208868efb40c2d3fc56c9ab4e0df
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92107766"
---
# <a name="connect-azure-to-itsm-tools-by-using-secure-export"></a>Conexión de Azure a las herramientas de ITSM mediante Exportación segura

En este artículo se muestra cómo configurar la conexión entre un producto o servicio de la Administración de servicios de TI (ITSM) mediante Exportación segura.

Exportación segura es una versión actualizada del [Conector de Administración de servicios de TI (ITSMC)](./itsmc-overview.md). Ambas versiones le permiten crear elementos de trabajo en una herramienta de ITSM cuando Azure Monitor envía alertas. La funcionalidad incluye alertas de métricas, de registros y de registros de actividad.

ITSMC usa las credenciales de nombre de usuario y contraseña. Exportación segura tiene una autenticación más sólida porque usa Azure Active Directory (Azure AD). Azure AD es un servicio de administración de acceso y de identidades basado en la nube de Microsoft. Ayuda a los usuarios a iniciar sesión y acceder a los recursos internos o externos. Usar Azure AD con ITSM ayuda a identificar las alertas de Azure (a través del id. de aplicación de Azure AD) que se enviaron al sistema externo.

> [!NOTE]
> La capacidad de conectar Azure a las herramientas de ITSM mediante Exportación segura está en versión preliminar.

## <a name="secure-export-architecture"></a>Arquitectura de Exportación segura

La arquitectura de Exportación segura presenta estas funcionalidades nuevas:

* **Nuevo grupo de acciones**: las alertas se envían a la herramienta de ITSM a través del grupo de acciones Webhook seguro en lugar del grupo de acciones ITSM que ITSMC usa.
* **Autenticación de Azure AD**: la autenticación se realiza a través de Azure AD en lugar de las credenciales de nombre de usuario y contraseña.

## <a name="secure-export-data-flow"></a>Flujo de datos de Exportación segura

Los pasos del flujo de datos de Exportación segura son los siguientes:

1. Azure Monitor envía una alerta que está configurada para usar Exportación segura.
1. Una acción de webhook seguro envía la carga de la alerta a la herramienta de ITSM.
1. La aplicación de ITSM comprueba con Azure AD si la alerta está autorizada para entrar a la herramienta de ITSM.
1. Si la alerta está autorizada, la aplicación:
   
   1. Crea un elemento de trabajo (por ejemplo, un incidente) en la herramienta de ITSM.
   1. Enlaza el id. del elemento de configuración (CI) a la base de datos de administración de clientes (CMDB).

![Diagrama que muestra cómo la herramienta de ITSM se comunica con Azure AD, las alertas de Azure y un grupo de acciones.](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="connection-with-bmc-helix"></a>Conexión con BMC Helix

Exportación segura es compatible con BMC Helix. Algunas de las ventajas de la integración son las siguientes:

* **Mejor autenticación**: Azure AD proporciona una autenticación más segura sin los tiempos de espera que suelen producirse en ITSMC.
* **Alertas resueltas en la herramienta de ITSM**: las alertas de métricas implementan los estados "activado" y "resuelto". Cuando se cumple la condición, el estado de la alerta es "activado". Cuando ya no se cumple la condición, el estado de la alerta es "resuelto". En ITSMC, las alertas no se pueden resolver de manera automática. Con Exportación segura, el estado resuelto fluye a la herramienta de ITSM, por lo que se actualiza automáticamente.
* **[Esquema de alerta común](./alerts-common-schema.md)** : en ITSMC, el esquema de la carga de la alerta es distinto en función del tipo de alerta. En Exportación segura, hay un esquema común para todos los tipos de alerta. Este esquema común contiene el elemento de configuración para todos los tipos de alerta. Todos los tipos de alerta podrán enlazar su elemento de configuración con la CMDB.

Empiece a usar la herramienta del Conector de Administración de servicios de TI con estos pasos:

1. Registrar la aplicación con Azure AD.
2. Crear un grupo de acciones Webhook seguro.
3. Configurar el entorno del asociado.

## <a name="register-with-azure-active-directory"></a>Registro con Azure Active Directory

Siga estos pasos para registrar la aplicación con Azure AD:

1. Siga los pasos que aparecen en [Registro de una aplicación en la plataforma de identidad de Microsoft](../../active-directory/develop/quickstart-register-app.md).
1. En Azure AD, seleccione **Exponer aplicación**.
1. Seleccione **Establecer** para **URI de id. de aplicación**.

   [![Captura de pantalla de la opción para establecer el identificador URI del id. de la aplicación.](media/it-service-management-connector-secure-webhook-connections/azure-ad.png)](media/it-service-management-connector-secure-webhook-connections/azure-ad-expand.png#lightbox)
1. Seleccione **Guardar**.

## <a name="create-a-secure-webhook-action-group"></a>Creación de un grupo de acciones Webhook seguro

Una vez registrada la aplicación con Azure AD, puede crear elementos de trabajo en la herramienta de ITSM en función de las alertas de Azure mediante el uso de la acción de webhook seguro en grupos de acciones.

Los grupos de acciones proporcionan una manera modular y reutilizable de desencadenar acciones para las alertas de Azure. Puede usar los grupos de acciones con alertas de métricas, de registros de actividad y de Azure Log Analytics en Azure Portal.
Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones en Azure Portal](./action-groups.md).

Use el procedimiento siguiente en el entorno de BMC Helix:

1. Inicie sesión en Integration Studio.
1. Busque el flujo de **Create Incident from Azure Alerts** (Creación de incidente a partir de alertas de Azure).
1. Copie la dirección URL del webhook.
   
   ![Captura de pantalla de la dirección URL del webhook en Integration Studio.](media/it-service-management-connector-secure-webhook-connections/bmc-url.png)

Para agregar un webhook a una acción, siga estas instrucciones para Webhook seguro:

1. En [Azure Portal](https://portal.azure.com/), busque y seleccione **Monitor**. El panel **Monitor** consolida todas las opciones de configuración y todos los datos de supervisión en una vista.
1. Seleccione **Alertas** > **Administrar acciones**.
1. Seleccione [Agregar grupo de acciones](./action-groups.md#create-an-action-group-by-using-the-azure-portal) y rellene los campos.
1. Escriba un nombre en el cuadro de texto **Nombre del grupo de acciones** y especifique un nombre en el cuadro de texto **Nombre corto**. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.
1. Seleccione **Webhook seguro**.
1. Seleccione estos detalles:
   1. Seleccione el id. de objeto de la instancia de Azure Active Directory que registró.
   1. En el URI, pegue la dirección URL del webhook que copió del entorno de BMC Helix.
   1. Establezca **Habilitar el esquema de alerta común** en **Sí**. 

   En la imagen siguiente se muestra la configuración de una acción de webhook seguro de ejemplo:

   ![Captura de pantalla que muestra una acción de webhook segura.](media/it-service-management-connector-secure-webhook-connections/secure-webhook.png)

## <a name="configure-the-partner-environment"></a>Configuración del entorno del asociado

### <a name="connect-bmc-helix-to-azure-monitor"></a>Conexión de BMC Helix a Azure Monitor

En las secciones siguientes se proporcionan detalles sobre cómo conectar el producto de BMC Helix y Exportación segura en Azure.

### <a name="prerequisites"></a>Requisitos previos

Asegúrese de que se cumplen los requisitos previos siguientes:

* Azure AD está registrado.
* Tiene la versión admitida de BMC Helix Multi-Cloud Service Management (19.08 o posterior).

### <a name="configure-the-bmc-helix-connection"></a>Configuración de la conexión de BMC Helix

1. Siga las instrucciones correspondientes a la versión:
   * [Habilitación de la integración precompilada con Azure Monitor para la versión 20.02](https://docs.bmc.com/docs/multicloud/enabling-prebuilt-integration-with-azure-monitor-879728195.html).
   * [Habilitación de la integración precompilada con Azure Monitor para la versión 19.11](https://docs.bmc.com/docs/multicloudprevious/enabling-prebuilt-integration-with-azure-monitor-904157623.html).

1. Como parte de la configuración de la conexión en BMC Helix, vaya a la instancia de BMC de integración y siga estas instrucciones:

   1. Seleccione **Catálogo**.
   1. Seleccione **Alertas de Azure**.
   1. Seleccione **Conectores**.
   1. Seleccione **Configuración**.
   1. Seleccione la configuración **Agregar nueva conexión**.
   1. Rellene la información de la sección de configuración:
      - **Name**: cree uno propio.
      - **Tipo de autorización**: **NONE**
      - **Descripción**: cree uno propio.
      - **Sitio**: **Cloud**
      - **Número de instancias**: **2**, el valor predeterminado.
      - **Comprobación**: seleccionado de manera predeterminada para habilitar el uso.
      - El id. de aplicación de Azure y el id. de inquilino de Azure se toman de la aplicación que definió anteriormente.

![Captura de pantalla que muestra la configuración de BMC.](media/it-service-management-connector-secure-webhook-connections/bmc-configuration.png)

## <a name="next-steps"></a>Pasos siguientes

* [Creación de elementos de trabajo de ITSM a partir de alertas de Azure](./itsmc-overview.md)