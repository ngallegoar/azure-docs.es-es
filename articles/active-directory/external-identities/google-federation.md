---
title: 'Incorporación de Google como proveedor de identidades para B2B: Azure AD'
description: Federación con Google para permitir a los usuarios invitados iniciar sesión en sus aplicaciones de Azure AD con sus cuentas de Gmail
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff8912794169cf61f394a097248a8476b2e0c0f3
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92926242"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Incorporación de Google como proveedor de identidades para los usuarios invitados de B2B

Si configura la federación con Google, puede permitir que los usuarios invitados puedan iniciar sesión en sus aplicaciones y recursos compartidos con sus propias cuentas de Google, sin tener que crear cuentas Microsoft. 

> [!NOTE]
> La federación de Google está diseñada específicamente para los usuarios de Gmail. Para realizar la federación con los dominios de G Suite, use la [federación directa](direct-federation.md).

## <a name="what-is-the-experience-for-the-google-user"></a>¿Cuál es la experiencia del usuario de Google?
Cuando envíe una invitación a usuarios de Gmail de Google, el usuario invitado debe acceder a sus aplicaciones o recursos compartidos mediante un vínculo que incluya el contexto del inquilino. Su experiencia varía en función de si ya han iniciado sesión en Google:
  - Se pedirá a los usuarios invitados que no hayan iniciado sesión en Google que lo hagan.
  - Se pedirá a los usuarios invitados que ya hayan iniciado sesión en Google que elijan la cuenta que desean usar. Deben elegir la cuenta donde hayan recibido la invitación.

Los usuarios invitados que ven un error "encabezado demasiado largo" pueden eliminar sus cookies o abrir una ventana privada o de incógnito e intentar iniciar sesión de nuevo.

![Captura de pantalla en la que se muestra la página de inicio de sesión de Google.](media/google-federation/google-sign-in.png)

## <a name="limitations"></a>Limitaciones

Teams admite totalmente a los usuarios invitados de Google en todos los dispositivos. Los usuarios de Google pueden iniciar sesión en Teams desde un punto de conexión común, como `https://teams.microsoft.com`.

Es posible que los puntos de conexión comunes de otras aplicaciones no admitan a los usuarios de Google. Los usuarios invitados de Google deberán iniciar sesión con un vínculo que incluya la información del inquilino. A continuación, se muestran algunos ejemplos:
  * `https://myapps.microsoft.com/?tenantid=<your tenant ID>`
  * `https://portal.azure.com/<your tenant ID>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`

   Si los usuarios invitados de Google intentan usar un vínculo como `https://myapps.microsoft.com` o `https://portal.azure.com`, recibirán un error.

También puede proporcionar a los usuarios invitados de Google un vínculo directo a una aplicación o recurso, siempre que el vínculo incluya la información del inquilino. Por ejemplo, `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`. 

## <a name="step-1-configure-a-google-developer-project"></a>Paso 1: configuración de un proyecto de desarrollador de Google
En primer lugar, cree un proyecto en la consola de desarrolladores de Google para obtener un identificador y un secreto de cliente que pueda agregar después a Azure Active Directory (Azure AD). 
1. Vaya a las API de Google de https://console.developers.google.com e inicie sesión con su cuenta de Google. Se recomienda utilizar una cuenta de Google compartida con el equipo.
2. Si se le solicita, acepte los términos del servicio.
3. Cree un nuevo proyecto: En el panel, seleccione **Crear proyecto** , asigne un nombre al proyecto (por ejemplo, **Azure AD B2B** ), y, después, seleccione **Crear** : 
   
   ![Captura en la que se muestra una página Nuevo proyecto.](media/google-federation/google-new-project.png)

4. En la página **API y servicios** , seleccione **Ver** en el nuevo proyecto.

5. Seleccione **Go to APIs overview** (Ir a la información general de las API) en la tarjeta de API. Seleccione **OAuth consent screen** (Pantalla de consentimiento de OAuth).

6. Seleccione **Externo** y después **Crear**. 

7. En la **Pantalla de consentimiento de OAuth** , especifique un **nombre de aplicación** :

   ![Captura de pantalla en la que se muestra la pantalla de consentimiento de OAuth de Google.](media/google-federation/google-oauth-consent-screen.png)

8. Desplácese hasta la sección **Dominios autorizados** y escriba **microsoftonline.com** :

   ![Captura de pantalla en la que se muestra la sección de dominios autorizados.](media/google-federation/google-oauth-authorized-domains.PNG)

9. Seleccione **Guardar**.

10. Seleccione **Credenciales**. En el menú **Crear credenciales** , seleccione **Id. del cliente de OAuth**.

    ![Captura de pantalla en la que se muestra el menú Crear credenciales de las API de Google.](media/google-federation/google-api-credentials.png)

11. En **Application type** (Tipo de aplicación), seleccione **Web application** (Aplicación web). Asigne un nombre adecuado a la aplicación, como **Azure AD B2B**. En **URI de redirección autorizados** , escriba los siguientes URI:
    - `https://login.microsoftonline.com`
    - `https://login.microsoftonline.com/te/<tenant ID>/oauth2/authresp` <br>(donde `<tenant ID>` es el id. de inquilino)
   
    > [!NOTE]
    > Para buscar el identificador de inquilino, vaya a [Azure Portal](https://portal.azure.com). En **Azure Active Directory** , seleccione **Propiedades** y copie el **Id. del inquilino**.

    ![Captura de pantalla en la que se muestra la sección de identificadores URI de redirección autorizados.](media/google-federation/google-create-oauth-client-id.png)

12. Seleccione **Crear**. Copie el identificador de cliente y del secreto de cliente. Los usará cuando agregue el proveedor de identidades en Azure Portal.

    ![Captura de pantalla en la que se muestra el Id. de cliente de OAuth y el secreto de cliente.](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Paso 2: configuración de la federación de Google en Azure AD 
Defina ahora el identificador y el secreto de cliente de Google. Para ello, puede usar Azure Portal o PowerShell. No olvide probar la configuración de la federación de Google; para ello, invítese a usted mismo. Use una dirección de Gmail e intente canjear la invitación con su cuenta de Google invitada. 

**Para configurar la federación de Google en Azure Portal** 
1. Vaya a [Azure Portal](https://portal.azure.com). En el panel izquierdo, seleccione **Azure Active Directory**. 
2. Seleccione **External Identities**.
3. Seleccione **Todos los proveedores de identidades** y haga clic en el botón de **Google**.
4. Escriba el identificador y el secreto de cliente que obtuvo anteriormente. Seleccione **Guardar** : 

   ![Captura de pantalla en la que se muestra la página para agregar un proveedor de identidades de Google.](media/google-federation/google-identity-provider.png)

**Configuración de la federación de Google con PowerShell**
1. Instale la versión más reciente de Azure AD PowerShell para el módulo Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Ejecute este comando: `Connect-AzureAD`
3. En el símbolo del sistema, inicie sesión con la cuenta de administrador global administrada.  
4. Ejecute el siguiente comando: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId <client ID> -ClientSecret <client secret>`
 
   > [!NOTE]
   > Use el identificador y secreto de cliente de la aplicación que creó en el "Paso 1: configuración de un proyecto de desarrollador de Google". Para más información, vea [New-AzureADMSIdentityProvider](/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview). 
 
## <a name="how-do-i-remove-google-federation"></a>¿Cómo se quita la federación de Google?
La configuración de la federación de Google se puede eliminar. Si lo hace, los usuarios invitados de Google que ya hayan canjeado su invitación no podrán iniciar sesión. Pero puede darles acceso a los recursos de nuevo si los elimina del directorio y los vuelve a invitar. 
 
**Para eliminar la federación de Google desde el portal de Azure AD**
1. Vaya a [Azure Portal](https://portal.azure.com). En el panel izquierdo, seleccione **Azure Active Directory**. 
2. Seleccione **External Identities**.
3. Seleccione **Todos los proveedores de identidades**.
4. En la línea de **Google** , seleccione el botón de puntos suspensivos ( **...** ) y, a continuación, seleccione **Eliminar**. 
   
   ![Captura de pantalla en la que se muestra el botón de eliminación del proveedor de identidades sociales.](media/google-federation/google-social-identity-providers.png)

1. Seleccione **Sí** para confirmar la eliminación. 

**Para eliminar la federación de Google mediante PowerShell** 
1. Instale la versión más reciente de Azure AD PowerShell para el módulo Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Ejecute `Connect-AzureAD`.  
4. En el símbolo del sistema, inicie sesión con la cuenta de administrador global administrada.  
5. Escriba el comando siguiente:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Para más información, consulte [Remove-AzureADMSIdentityProvider](/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview).
