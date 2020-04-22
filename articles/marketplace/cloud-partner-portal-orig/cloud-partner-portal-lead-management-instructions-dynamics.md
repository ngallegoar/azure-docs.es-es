---
title: Dynamics CRM | Azure Marketplace
description: Configure la administración de clientes potenciales de Dynamics CRM.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: d64c8460f5653f28b96396025f29ea13af15c8c3
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416319"
---
# <a name="configure-lead-management-for-dynamics-crm-online"></a>Configure la administración de clientes potenciales de Dynamics CRM Online

En este artículo se describe cómo configurar Dynamics CRM Online para procesar las ventas de clientes potenciales.

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este artículo, se requieren los siguientes permisos de usuario:
- Debe ser un administrador en la instancia de Dynamics CRM Online para instalar una solución.
- Debe ser un administrador de inquilinos para crear una nueva cuenta de servicio para el servicio de clientes potenciales.

<a name="install-the-solution"></a>Instalar la solución
--------------------

1.  Descargue la [solución Microsoft Marketplace Lead Writer](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) y guárdela en una unidad local.

2.  Abra Dynamics CRM Online y vaya a Configuración.
    >[!NOTE]
    >Si no ve las opciones en la siguiente captura de pantalla significará que no tiene los permisos que necesita.
 
       ![Vista de configuración de Dynamics](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline1.png)

3.  Haga clic en **Importar** y seleccione la solución que ha descargado en el paso 1.
 
    ![Opción de importación de Dynamics](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline2.png)

4.  Termine de instalar la solución.

## <a name="configure-user-permissions"></a>Configurar los permisos de usuario

Para escribir clientes potenciales en su instancia de Dynamics CRM, debe compartir una cuenta de servicio con nosotros y configurar los permisos para la cuenta.

Utilice los siguientes pasos para crear la cuenta de servicio y asignar permisos. Puede usar **Azure Active Directory** u **Office 365**.

### <a name="azure-active-directory"></a>Azure Active Directory

Se recomienda esta opción, ya que obtiene la ventaja adicional de nunca tener que actualizar el nombre de usuario y la contraseña para seguir recibiendo clientes potenciales. Para usar la opción de Azure Active Directory, debe proporcionar el identificador de la aplicación, la clave de aplicación y el id. de directorio de la aplicación de Active Directory.

Use los siguientes pasos para configurar Azure Active Directory para Dynamics CRM.

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/) y después seleccione el servicio Azure Active Directory.

2.  Seleccione **Propiedades** y después copie el **Id. de directorio**. Esta es la identificación de la cuenta de inquilino que se debe usa en Cloud Partner Portal.

    ![Obtener identificador de directorio](./media/cloud-partner-portal-lead-management-instructions-dynamics/directoryid.png)

3.  Seleccione **Registros de aplicaciones** y luego **Nuevo registro de aplicaciones**.
4.  Escriba el nombre de la aplicación.
5.  Para el tipo seleccione **Aplicación web o API**.
6.  Proporcione una dirección URL. Este campo no es necesario para los clientes potenciales, pero es necesario para crear una aplicación.
7. Seleccione **Crear**.
8.  Ahora que la aplicación está registrada, seleccione **Propiedades** y después **Copiar el Id. de aplicación**. Usará esta información de conexión en Cloud Partner Portal.
9.  En Propiedades, establezca la aplicación como multiinquilino y después seleccione **Guardar**.

10. Seleccione **Claves** y cree una nueva clave con la duración establecida en *Nunca caduca*. Seleccione **Guardar** para crear la clave. 
11. En el menú Claves, seleccione **Copiar el valor de clave**. Guarde una copia de este valor ya que la necesitará para Cloud Partner Portal.
    
    ![Obtener clave registrada de Dynamics](./media/cloud-partner-portal-lead-management-instructions-dynamics/registerkeys.png)
    
12. Seleccione **Permisos necesarios** y luego **Agregar**. 
13. Seleccione **Dynamics CRM Online** como la nueva API y compruebe el permiso para *Acceder a CRM Online como usuarios de la organización*.

14. En Dynamics CRM, vaya a Usuarios y seleccione la lista desplegable de “Usuarios habilitados” para cambiar a **Usuarios de la aplicación**.
    
    ![Usuarios de la aplicación](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuserfirst.PNG)

15. Seleccione **Nuevo** para crear un nuevo usuario. Seleccione la lista desplegable **USUARIO: USUARIO DE LA APLICACIÓN**.
    
    ![Agregar un nuevo usuario de la aplicación](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuser.PNG)

16. En **Nuevo usuario** proporcione el nombre y el correo electrónico que desea usar con esta conexión. Pegue el **Id. de aplicación** de la aplicación que creó en Azure Portal.

     ![Configurar el nuevo usuario](./media/cloud-partner-portal-lead-management-instructions-dynamics/leadgencreateuser.PNG)

17. Vaya a “Configuración de seguridad” en este artículo para finalizar la configuración de la conexión para este usuario.

### <a name="office-365"></a>Office 365

Si no quiere usar Azure Active Directory, puede registrar un nuevo usuario en el *Centro de administración de Microsoft 365*. Tendrá que actualizar cada 90 días su nombre de usuario y contraseña para continuar obteniendo clientes potenciales.

Use los siguientes pasos para configurar Office 365 para Dynamics CRM.

1. Inicie sesión en el [Centro de administración de Microsoft 365](https://admin.microsoft.com).

2. Seleccione el mosaico **Admin**.

    ![Centro de administración de Office](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline3.png)

3. Seleccione **Agregar un usuario**.

    ![Adición de un usuario](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline4.png)

4. Cree un usuario nuevo para el servicio de escritura de clientes potenciales. Configure las siguientes opciones:

    -   Proporcione una contraseña y desactive la opción “Este usuario debe cambiar la contraseña la primera vez que inicie sesión”.
    -   Seleccione el rol para que sea “Usuario, sin acceso como administrador” como el rol para el usuario.
    -   Seleccione la licencia del producto que se muestra en la siguiente captura de pantalla. Se le cobrará por la licencia que elija. La solución también funcionará con la licencia Dynamics CRM Online Basic.
    
    ![Configuración de los permisos de usuario y la licencia](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline5.png)

## <a name="security-settings"></a>Configuración de seguridad

El paso final es permitir que el usuario que ha creado escriba los clientes potenciales.

1.  Inicie sesión en Dynamics CRM Online.
2.  En **Configuración**, seleccione **Seguridad**.
    
    ![Configuración de seguridad](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline6.png)

3.  Seleccione el usuario que creó en **Permisos de usuario** y después **Administrar roles de usuario**. Marque **Microsoft Marketplace Lead Writer** para asignar el rol.

    ![Asignación del rol de usuario](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline7.png)\

    >[!NOTE]
    >Este rol lo crea la solución que importó y solo tiene permisos para escribir clientes potenciales y para realizar un seguimiento de la versión de la solución a fin de garantizar la compatibilidad.

4.  En Seguridad, seleccione **Roles de seguridad** y busque el rol Microsoft Marketplace Lead Writer.
    
    ![Configuración del sistema de escritura de clientes potenciales de seguridad](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline10.jpg)\

5. Seleccione la pestaña **Registros principales**. Habilite Crear, Leer o Escribir para la interfaz de usuario de la entidad de usuario.

    ![Habilitar crear, leer o escribir para el usuario](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline11.jpg)\

## <a name="wrap-up"></a>Encapsulado

Finalice la configuración de Dynamics CRM para la administración de clientes potenciales agregando la información de cuentas generada a Cloud Partner Portal. Por ejemplo:

-   **Azure Active Directory** - **Id. de aplicación** (ejemplo: *23456052-aaaa-bbbb-8662-1234df56788f*), **Identificador de directorio** (ejemplo: *12345678-8af1-4asf-1234-12234d01db47*) y **Clave de la aplicación** (ejemplo: *1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=* ).
-   **Office 365** - **Dirección URL** (ejemplo: **`https://contoso.crm4.dynamics.com`** ), **Nombre de usuario** (ejemplo: **`contoso\@contoso.onmicrosoft.com`** ) y **Contraseña** (ejemplo: *P\@ssw0rd*).
