---
title: 'Administración de clientes potenciales para Dynamics 365 Customer Engagement: marketplace comercial de Microsoft'
description: Aprenda a configurar Dynamics 365 Customer Engagement para administrar clientes potenciales de Microsoft AppSource y Azure Marketplace.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 985b3258eb0b957242d529945f32ed9704a91e7d
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791007"
---
# <a name="configure-lead-management-for-dynamics-365-customer-engagement"></a>Configuración de la administración de clientes potenciales para Dynamics 365 Customer Engagement

En este artículo se describe cómo configurar Dynamics 365 Customer Engagement (anteriormente Dynamics CRM Online). Encuentre más información sobre el cambio en [Configurar la autenticación basada en servidor con Customer Engagement y SharePoint Online](https://docs.microsoft.com/dynamics365/customerengagement/on-premises/admin/on-prem-server-based-sharepoint-online) para procesar clientes potenciales de la oferta de Marketplace comercial.

>[!NOTE]
>Estas instrucciones son específicas del entorno de nube hospedado en Microsoft para Dynamics 365 Customer Engagement. Actualmente no se admite la conexión directa a un entorno local de Dynamics. Hay otras opciones para recibir clientes potenciales, como configurar un [punto de conexión HTTPS](./commercial-marketplace-lead-management-instructions-https.md) o una [tabla de Azure](./commercial-marketplace-lead-management-instructions-azure-table.md).

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este artículo, se necesitan los siguientes permisos de usuario:

* Derechos de administrador de la instancia de Dynamics 365 Customer Engagement para poder instalar una solución.
* Derechos de administrador de inquilinos para crear una cuenta de servicio para el servicio de clientes potenciales que se usa para enviar clientes potenciales de las ofertas de marketplace comercial.
* Acceso al portal de administración de Office 365.
* Acceso a Azure Portal.

## <a name="install-the-solution"></a>Instalar la solución

1. Descargue la [solución Microsoft Marketplace Lead Writer](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) y guárdela en una unidad local de su equipo.

1. Abra Dynamics 365 Customer Engagement, para ello, vaya a la dirección URL de la instancia de Dynamics (por ejemplo, `https://tenant.crm.dynamics.com`).

1. Seleccione el icono de engranaje de la barra superior y, luego, elija **Configuración avanzada**.
 
    ![Elemento de menú de la configuración avanzada de Dynamics 365](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-advanced-settings.png)

1. En la página **Configuración**, abra el menú **Configuración** en la barra superior y seleccione **Soluciones**.

    >[!NOTE]
    >Si no ve las opciones en la siguiente pantalla, significa que no tiene los permisos necesarios para continuar. Póngase en contacto con un administrador de la instancia de Dynamics 365 Customer Engagement.

    ![Opción Soluciones de Dynamics 365](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-solutions.png)

1. En la página **Soluciones**, seleccione **Importar** y vaya a donde guardó la solución **Microsoft Marketplace Lead Writer** que descargó en el paso 1.

    ![Botón Importar](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-crm-import.png)

1. Complete la importación de la solución siguiendo el Asistente para importar soluciones.

## <a name="configure-user-permissions"></a>Configurar los permisos de usuario

Para escribir clientes potenciales en la instancia de Dynamics 365 Customer Engagement, tiene que compartir una cuenta de servicio con Microsoft y configurar permisos para la cuenta.

Utilice los siguientes pasos para crear la cuenta de servicio y asignar permisos. Puede usar Azure Active Directory u Office 365.

>[!NOTE]
>En función de la opción de autenticación que seleccione, puede ir directamente a las instrucciones correspondientes. Consulte [Azure Active Directory](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#azure-active-directory) u [Office 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#office-365).

### <a name="azure-active-directory"></a>Azure Active Directory

Se recomienda esta opción porque no es necesario actualizar nunca el nombre de usuario o la contraseña para seguir recibiendo clientes potenciales. Para usar la opción de Azure Active Directory, debe proporcionar el identificador de la aplicación, la clave de aplicación y el identificador de directorio de la aplicación de Active Directory.

Para configurar Azure Active Directory para Dynamics 365 Customer Engagement, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/). En el panel izquierdo, seleccione **Azure Active Directory**.

1. Seleccione **Propiedades** y copie el valor de **Id. de directorio** en la página **Propiedades del directorio**. Guarde este valor, ya que lo tendrá que proporcionar en el portal de publicación para recibir clientes potenciales de su oferta de Marketplace.

    ![Elemento de menú Propiedades de Azure Active Directory](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-properties.png)

1. Seleccione **Registros de aplicaciones** en el panel izquierdo de Azure Active Directory y, luego, elija **Nuevo registro** en esa página.
1. Escriba un nombre descriptivo para el nombre de la aplicación.
1. En **Tipos de cuenta compatibles**, seleccione **Cuentas en cualquier directorio organizativo**.
1. En **URI de redirección (opcional)** , seleccione **Web** y escriba un URI, por ejemplo, `https://contosoapp1/auth`. 
1. Seleccione **Registrar**.

    ![Página de registro de una aplicación](./media/commercial-marketplace-lead-management-instructions-dynamics/register-an-application.png)

1. Ahora que la aplicación está registrada, acceda a la página de información general de la aplicación. Copie el valor de **Id. de aplicación (cliente)** de esa página. Guarde este valor, ya que lo tendrá que proporcionar en el portal de publicación y en Dynamics 365 para recibir clientes potenciales de su oferta de Marketplace.

    ![Cuadro Id. de aplicación (cliente)](./media/commercial-marketplace-lead-management-instructions-dynamics/application-id.png)

1. Seleccione **Certificados y secretos** en el panel izquierdo de la aplicación y elija el botón **Nuevo secreto de cliente**. Escriba una descripción clara para el secreto de cliente y, en **Expira**, seleccione la opción **Nunca**. Seleccione **Agregar** para crear el secreto de cliente.

    ![Elemento de menú Certificados y secretos](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-certificates-secrets.png)

1. En cuanto el secreto de cliente se cree correctamente, copie el valor de **Secreto de cliente**. Después de abandonar la página, no podrá recuperar el valor. Guarde este valor, ya que lo tendrá que proporcionar en el portal de publicación para recibir clientes potenciales de su oferta de Marketplace. 
1. Seleccione **Permisos de API** en el panel izquierdo de la aplicación y, luego, elija **Agregar un permiso**.
1. Seleccione **API de Microsoft** y, luego, elija **Dynamics CRM** como API.
1. En **¿Qué tipo de permiso necesita la aplicación web?** , asegúrese de que la opción **Permisos delegados** esté seleccionada. 
1. En **Permiso**, marque la casilla **user_impersonation** de **Access Common Data Service as organization users** (Acceder a Common Data Service como usuarios de la organización). Seleccione **Agregar permisos**.

    ![Botón Agregar permisos](./media/commercial-marketplace-lead-management-instructions-dynamics/api-permissions.png)

1. Después de completar los pasos del 1 al 14 en Azure Portal, vaya a la instancia de Dynamics 365 Customer Engagement mediante la dirección URL, por ejemplo, `https://tenant.crm.dynamics.com`.
1. Seleccione el icono de engranaje de la barra superior y, luego, elija **Configuración avanzada**.
1. En la página **Configuración**, abra el menú **Configuración** en la barra superior y seleccione **Seguridad**.
1. En la página **Seguridad**, seleccione **Usuarios**. En la página **Usuarios**, seleccione la lista desplegable **Usuarios habilitados** y, luego, elija **Usuarios de la aplicación**.
1. Seleccione **Nuevo** para crear un nuevo usuario. 

    ![Creación de un nuevo usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/application-users.png)

1. En el panel **Nuevo usuario**, asegúrese de que esté seleccionado **USUARIO: USUARIO DE LA APLICACIÓN**. Escriba un nombre de usuario, un nombre completo y una dirección de correo electrónico para el usuario que quiera usar con esta conexión. Además, pegue el **Id. de aplicación** de la aplicación que creó en Azure Portal en el paso 8. Seleccione **Guardar y cerrar** para terminar de agregar el usuario.

    ![Panel Nuevo usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/new-user-info.png)

1. Vaya a la sección “Configuración de seguridad” de este artículo para terminar de configurar la conexión de este usuario.

### <a name="office-365"></a>Office 365

Si no quiere usar Azure Active Directory, puede registrar un nuevo usuario en el Centro de administración de Microsoft 365. Tendrá que actualizar cada 90 días su nombre de usuario y contraseña para continuar recibiendo clientes potenciales.

Para configurar Office 365 para Dynamics 365 Customer Engagement, siga estos pasos:

1. Inicie sesión en el [Centro de administración de Microsoft 365](https://admin.microsoft.com).

1. Seleccione **Agregar un usuario**.

    ![Centro de administración de Microsoft 365: opción Agregar un usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-add-user.png)

1. Cree un usuario nuevo para el servicio de escritura de clientes potenciales. Configure las siguientes opciones:

    * Especifique un nombre de usuario.
    * Proporcione una contraseña y desactive la opción **Hacer que este usuario cambie la contraseña cuando inicie sesión por primera vez**.
    * Seleccione **User (no administrator access)** (Usuario, sin acceso como administrador) como rol del usuario.
    * Seleccione **Plan Dynamics 365 Customer Engagement** como licencia del producto, como se muestra en la pantalla siguiente. Se le cobrará por la licencia que elija. 

Guarde estos valores, ya que deberá proporcionar los valores de **Nombre de usuario** y **Contraseña** en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

![Centro de administración de Microsoft 365: panel Nuevo usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-new-user.png)

## <a name="security-settings"></a>Configuración de seguridad

El paso final es permitir que el usuario que ha creado escriba los clientes potenciales.

1. Abra Dynamics 365 Customer Engagement, para ello, vaya a la dirección URL de la instancia de Dynamics, por ejemplo, `https://tenant.crm.dynamics.com`.
1. Seleccione el icono de engranaje de la barra superior y, luego, elija **Configuración avanzada**.
1. En la página **Configuración**, abra el menú **Configuración** en la barra superior y seleccione **Seguridad**.
1. En la página **Seguridad**, seleccione **Usuarios** y elija el usuario que creó en la sección "Configurar los permisos de usuario" de este documento. A continuación, seleccione **Administrar roles**. 

    ![Pestaña Administrar roles](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-roles.png)

1. Busque el nombre del rol **Microsoft Marketplace Lead Writer** y selecciónelo para asignarlo al usuario.

    ![Panel Administrar roles de usuario](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-user-roles.png)

    >[!NOTE]
    >Este rol lo crea la solución que importó y solo tiene permisos para escribir clientes potenciales y para realizar un seguimiento de la versión de la solución a fin de garantizar la compatibilidad.

1. Vuelva a la página **Seguridad** y seleccione **Roles de seguridad**. Busque el rol **Microsoft Marketplace Lead Writer** y selecciónelo.

    ![Panel Roles de seguridad](./media/commercial-marketplace-lead-management-instructions-dynamics/security-roles.png)

1. En el rol de seguridad, seleccione la pestaña **Registros principales**. Busque el elemento **Configuración de UI de la entidad de usuario**. Habilite los permisos de creación, lectura y escritura para el usuario (1/4 del círculo amarillo) para esa entidad haciendo clic una vez en cada uno de los círculos correspondientes.

    ![Pestaña Registros principales de Microsoft Marketplace Lead Writer](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer.png)

1. En la pestaña **Personalización**, busque el elemento **Trabajo del sistema**. Habilite los permisos de lectura, escritura y anexión para la organización (círculos verdes completos) para esa entidad haciendo clic cuatro veces en cada uno de los círculos correspondientes.

    ![Pestaña Personalización de Microsoft Marketplace Lead Writer](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer-customization.png)

1. Seleccione **Guardar y cerrar**.

## <a name="configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement"></a>Configuración de la oferta para enviar clientes potenciales a Dynamics 365 Customer Engagement 

Para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos:

1. Vaya a la página **Configuración de la oferta** de su oferta.
1. Seleccione **Conectar** en la sección **Administración de clientes potenciales**.

    ![Botón Conectar de la sección Administración de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-dynamics/connect-lead-management.png)

1. En la ventana emergente Detalles de la conexión, seleccione **Dynamics 365 Customer Engagement** como destino de clientes potenciales.

    ![Cuadro Destino de clientes potenciales](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-lead-destination.png)

1. Escriba la **dirección URL** de la instancia de Dynamics 365, por ejemplo `https://contoso.crm4.dynamics.com`.

1. Seleccione el método de **Autenticación**: Azure Active Directory u Office 365. 
1. Si seleccionó **Azure Active Directory**, escriba los valores de **Id. de aplicación (cliente)** (por ejemplo, `23456052-aaaa-bbbb-8662-1234df56788f`), **Id. de directorio** (por ejemplo, `12345678-8af1-4asf-1234-12234d01db47`) y **Secreto de cliente** (por ejemplo, `1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=`).

    ![Autenticación con Azure Active Directory seleccionado](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-application-id.png)

1. Si seleccionó **Office 365**, escriba los valores de **Nombre de usuario** (por ejemplo, `contoso@contoso.onmicrosoft.com`) y **Contraseña** (por ejemplo, `P@ssw0rd`).

    ![Cuadro de nombre de usuario de Office 365](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-authentication.png)

1. En **Correo electrónico de contacto**, escriba las direcciones de correo electrónico de las personas de su empresa que deban recibir notificaciones de correo electrónico cuando se reciba un nuevo cliente potencial. Para escribir varias direcciones de correo electrónico, sepárelas con punto y coma.
1. Seleccione **Aceptar**.

Para asegurarse de que se ha conectado correctamente a un destino de clientes potenciales, seleccione el botón **Validar**. Si la validación es correcta, tendrá un cliente potencial de prueba en el destino de clientes potenciales.

![Cuadro Dirección de correo electrónico de contacto](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-connection-details.png)

>[!NOTE]
>Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.
