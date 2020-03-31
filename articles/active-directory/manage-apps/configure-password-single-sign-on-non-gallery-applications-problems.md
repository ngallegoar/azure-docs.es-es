---
title: Problemas en la configuración de SSO con contraseña para una aplicación ajena a la galería
description: Problemas comunes que se producen al configurar el inicio de sesión único (SSO) con contraseña para aplicaciones personalizadas que no están en la galería de aplicaciones de Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed8bafe7f5bc28cf37205107f8ab6dd5cdb4907c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74274145"
---
# <a name="problems-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Problemas en la configuración del inicio de sesión único con contraseña para una aplicación ajena a la galería

En este artículo se describen problemas comunes que pueden producirse al configurar el *inicio de sesión único (SSO) con contraseña* para una aplicación ajena a la galería.

## <a name="capture-sign-in-fields-for-an-app"></a>Capturar los campos de inicio de sesión para una aplicación

La captura de campos de inicio de sesión solo se admite para páginas de inicio de sesión habilitadas para HTML. No se admite no en las páginas de inicio de sesión no estándar, como las que usan Adobe Flash u otras tecnologías que no compatibles con HTML.

Existen dos maneras de capturar campos de inicio de sesión para aplicaciones personalizadas:

- La **captura automática de campos de inicio de sesión** funciona bien con la mayoría de las páginas de inicio de sesión compatibles con HTML si usan *identificadores DIV conocidos* para los campos de nombre de usuario y contraseña. El código HTML en la página se extrae para encontrar identificadores DIV que cumplen determinados criterios. Esos metadatos se guardan para que puedan reproducirse en la aplicación más adelante.

- La **captura manual de campos de inicio de sesión** se usa si el proveedor de la aplicación *no etiqueta los campos de entrada de inicio de sesión*. La captura manual también se usa si el proveedor *presenta varios campos que no se pueden detectar automáticamente*. Azure Active Directory (Azure AD) puede almacenar datos de tantos campos como haya en la página de inicio de sesión, si le indica dónde se encuentran esos campos en la página.

Por lo general, si la captura automática de campos de inicio de sesión no funciona, pruebe la opción manual.

### <a name="automatically-capture-sign-in-fields-for-an-app"></a>Capturar automáticamente campos de inicio de sesión para una aplicación

Para configurar el SSO basado en contraseña con la captura automática de campos de inicio de sesión, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/). Inicie sesión como administrador global o coadministrador.

2. En el panel de navegación del lado izquierdo, seleccione **Todos los servicios** para abrir la extensión de Azure AD.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda de filtrado y, luego, seleccione **Azure Active Directory**.

4. En el panel de navegación de Azure AD, seleccione **Aplicaciones empresariales**.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   > [!NOTE]
   > Si no ve la aplicación que busca, use el control **Filtro** en la parte superior de la lista **Todas las aplicaciones**. Establezca la opción **Mostrar** en "Todas las aplicaciones".

6. Seleccione la aplicación para la que quiere configurar el SSO.

7. Después de que la aplicación se carga, seleccione **Inicio de sesión único** en el panel de navegación de la izquierda.

8. Seleccione el modo **Inicio de sesión con contraseña**.

9. Escriba la **URL de inicio de sesión**, que es la dirección URL de la página donde los usuarios escriben su nombre de usuario y contraseña para iniciar sesión. *Asegúrese de que los campos de inicio de sesión estén visibles en la página de la dirección URL que proporcione*.

10. Seleccione **Guardar**.

    La página se extrae automáticamente para los cuadros de entrada de nombre de usuario y contraseña. Ahora puede usar Azure AD para transmitir de forma segura las contraseñas a esa aplicación mediante la extensión de explorador del Panel de acceso.

### <a name="manually-capture-sign-in-fields-for-an-app"></a>Capturar manualmente campos de inicio de sesión para una aplicación

Para capturar manualmente los campos de inicio de sesión, debe tener instalada la extensión del explorador del Panel de acceso. Además, el explorador no puede estar en ejecución en modo *inPrivate*, *incógnito* ni *privado*.

Para instalar la extensión, consulte la sección [Instalación de la extensión del explorador del Panel de acceso](#install-the-access-panel-browser-extension) de este artículo.

Para configurar el SSO basado en contraseña para una aplicación con la captura manual de campos de inicio de sesión, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/). Inicie sesión como administrador global o coadministrador.

2. En el panel de navegación del lado izquierdo, seleccione **Todos los servicios** para abrir la extensión de Azure AD.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda de filtrado y, luego, seleccione **Azure Active Directory**.

4. En el panel de navegación de Azure AD, seleccione **Aplicaciones empresariales**.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   > [!NOTE] 
   > Si no ve la aplicación que busca, use el control **Filtro** en la parte superior de la lista **Todas las aplicaciones**. Establezca la opción **Mostrar** en "Todas las aplicaciones".

6. Seleccione la aplicación para la que quiere configurar el SSO.

7. Después de que la aplicación se carga, seleccione **Inicio de sesión único** en el panel de navegación de la izquierda.

8. Seleccione el modo **Inicio de sesión con contraseña**.

9. Escriba la **URL de inicio de sesión**, que es la página donde los usuarios escriben su nombre de usuario y contraseña para iniciar sesión. *Asegúrese de que los campos de inicio de sesión estén visibles en la página de la dirección URL que proporcione*.

10. Seleccione **Establecer configuración de inicio de sesión único con contraseña de *&lt;nombre de la aplicación&gt;*** .

11. Seleccione **Detectar campos de inicio de sesión manualmente**.

14. Seleccione **Aceptar**.

15. Seleccione **Guardar**.

16. Siga las instrucciones para usar el Panel de acceso.

## <a name="troubleshoot-problems"></a>Solucionar problemas

### <a name="i-get-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Recibo el error "No pudimos encontrar ningún campo de inicio de sesión en esa URL"

Recibe este error cuando no funciona la detección automática de campos de inicio de sesión. Para resolver el problema, pruebe la detección manual de campos de inicio de sesión. Consulte la sección [Captura manual de campos de inicio de sesión para una aplicación](#manually-capture-sign-in-fields-for-an-app) de este artículo.

### <a name="i-get-an-unable-to-save-single-sign-on-configuration-error"></a>Recibo un error "No se puede guardar la configuración del inicio de sesión único"

En contadas ocasiones, no es posible actualizar la configuración de SSO. Para resolver este problema, intente guardar la configuración de nuevo.

Si sigue apareciendo el error, abra una incidencia de soporte técnico. Incluya la información que se describe en las secciones [Ver los detalles de notificaciones del portal](#view-portal-notification-details) y [Enviar los detalles de notificaciones a un ingeniero de soporte técnico para obtener ayuda](#send-notification-details-to-a-support-engineer-to-get-help) de este artículo.

### <a name="i-cant-manually-detect-sign-in-fields-for-my-app"></a>No puedo detectar manualmente los campos de inicio de sesión para mi aplicación

Puede que observe los comportamientos siguientes cuando no funcione la detección manual:

- El proceso de captura manual parece haber funcionado, pero los campos capturados no son correctos.

- No se resaltan los campos correctos al ejecutar el proceso de captura.

- El proceso de captura lo dirige a la página de inicio de sesión de la aplicación según lo previsto, pero no ocurre nada.

- La captura manual parece funcionar, pero el SSO no se realiza cuando los usuarios acceden a la aplicación desde el Panel de acceso.

Si experimenta alguno de estos problemas, haga lo siguiente:

- Asegúrese de tener *instalada y habilitada* la versión más reciente de la extensión de explorador del Panel de acceso. Consulte la sección [Instalación de la extensión del explorador del Panel de acceso](#install-the-access-panel-browser-extension) de este artículo.

- Asegúrese de que el explorador no se encuentre en los modos *InPrivate*, *incógnito* ni *privado* durante el proceso de captura. La extensión del Panel de acceso no es compatible con estos modos.

- Asegúrese de que los usuarios no intentan iniciar sesión en la aplicación desde el Panel de acceso en los *modos incógnito*, *inPrivate* ni *privado*.

- Vuelva a intentar el proceso de captura manual. Asegúrese de que los marcadores rojos se encuentren sobre los campos correctos.

- Si el proceso de captura manual parece no responder o la página de inicio de sesión no responde, intente realizar el proceso de captura manual de nuevo. Sin embargo, esta vez, tras completar el proceso, presione la tecla F12 para abrir la consola del desarrollador del explorador. Seleccione la pestaña **Consola**. Escriba **window.location=" *&lt;escriba la dirección URL de inicio de sesión especificada al configurar la aplicación&gt;* "** y, luego, presione Entrar. De este modo, se forzará una redirección de página que finalizará el proceso de captura y almacenará los campos capturados.

### <a name="contact-support"></a>Ponerse en contacto con soporte técnico

Si sigue teniendo problemas, abra un caso en Soporte técnico de Microsoft. Describa lo que ha intentado. Incluya los detalles que se describen en las secciones [Ver los detalles de notificaciones del portal](#view-portal-notification-details) y [Enviar los detalles de notificaciones a un ingeniero de soporte técnico para obtener ayuda](#send-notification-details-to-a-support-engineer-to-get-help) de este artículo (si corresponde).

## <a name="install-the-access-panel-browser-extension"></a>Instalación de la extensión del explorador del Panel de acceso

Siga estos pasos:

1. Abra el [Panel de acceso](https://myapps.microsoft.com) en un explorador compatible. Inicie sesión en Azure AD como *usuario*.

2. Seleccione una **aplicación de SSO con contraseña** en el Panel de acceso.

3. Cuando se le pida instalar el software, seleccione **Instalar ahora**.

4. Se le dirigirá a una página de descarga del explorador. Elija **Agregar** la extensión.

5. Si se le pide, seleccione **Habilitar** o **Permitir**.

6. Después de la instalación, reinicie el explorador.

7. Inicie sesión en el Panel de acceso. Vea si puede abrir las aplicaciones compatibles con SSO con contraseña.

También puede descargar directamente la extensión del explorador para Chrome y Firefox a través de estos vínculos:

-   [Extensión del Panel de acceso para Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Extensión del panel de acceso para Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="view-portal-notification-details"></a>Ver los detalles de notificaciones del portal

Para ver los detalles de cualquier notificación del portal, siga estos pasos:

1. Seleccione el icono **Notificaciones** (con forma de campana) de la esquina superior derecha de Azure Portal.

2. Seleccione una notificación que tenga un estado de *Error*. (Tienen un "!" rojo).

   > [!NOTE]
   > No se puede seleccionar notificaciones con estado *Correcto* o *En curso*.

3. Se abre el panel **Detalles de la notificación**. Lea la información para conocer más sobre el problema.

5. Si aún necesita ayuda, comparta la información con un ingeniero de soporte técnico o el grupo de producto. Seleccione el icono de **copia** a la derecha del cuadro **Copiar error** para copiar los detalles de notificación para compartirlos.

## <a name="send-notification-details-to-a-support-engineer-to-get-help"></a>Enviar los detalles de notificaciones a un ingeniero de soporte técnico para obtener ayuda

Es importante que comparta con el soporte técnico *todos* los detalles que aparecen en esta sección para que pueden ayudarle rápidamente. Para registrarlos, puede tomar una captura de pantalla o seleccionar **Copiar error**.

En la siguiente información se explica qué significa cada elemento de la notificación y se proporcionan ejemplos.

### <a name="essential-notification-items"></a>Elementos esenciales de una notificación

- **Título**: El título descriptivo de la notificación.

   Ejemplo: *Configuración del proxy de aplicación*

- **Descripción**: Qué ha ocurrido como consecuencia de la operación.

   Ejemplo: *La dirección URL interna especificada ya se está usando en otra aplicación.*

- **Identificador de notificación**: El identificador único de la notificación.

    Ejemplo: *clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068*

- **Identificador de solicitud de cliente**: El identificador de solicitud específico generado por el explorador.

    Ejemplo: *302fd775-3329-4670-a9f3-bea37004f0bc*

- **Marca de tiempo UTC**: La marca de tiempo de cuándo tuvo lugar la notificación, en formato UTC.

    Ejemplo: *2017-03-23T19:50:43.7583681Z*

- **Identificador de transacción interno**: El identificador interno que se ha usado para buscar el error en nuestros sistemas.

    Ejemplo: **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN**: El usuario que ejecutó la operación.

    Ejemplo: *tperkins\@f128.info*

- **Identificador de inquilino**: El identificador único del inquilino del que forma parte el usuario que ejecutó la operación.

    Ejemplo: *7918d4b5-0442-4a97-be2d-36f9f9962ece*

- **Id. de objeto de usuario**: El identificador único del usuario que ejecutó la operación.

    Ejemplo: *17f84be4-51f8-483a-b533-383791227a99*

### <a name="detailed-notification-items"></a>Elementos detallados de una notificación

- **Nombre para mostrar**: (Puede estar vacío) un nombre para mostrar más detallado del error.

    Ejemplo: *Configuración del proxy de aplicación*

- **Estado**: El estado específico de la notificación.

    Ejemplo: *Erróneo*

- **Identificador de objeto**: (Puede estar vacío) el identificador del objeto en el que se ejecutó la operación.

   Ejemplo: *8e08161d-f2fd-40ad-a34a-a9632d6bb599*

- **Detalles**: La descripción detallada de lo que se produjo como consecuencia de la operación.

    Ejemplo: *La dirección url interna "<https://bing.com/>" no es válida puesto que ya está en uso.*

- **Copiar error**: Le permite seleccionar el **icono de copia** a la derecha del cuadro **Copiar error** para copiar los detalles de la notificación para compartirlos con soporte técnico.

    Ejemplo:   ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Pasos siguientes
[Proporcionar un inicio de sesión único a las aplicaciones con el proxy de aplicación](application-proxy-configure-single-sign-on-with-kcd.md)
