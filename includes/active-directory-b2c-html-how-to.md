---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/19/2020
ms.author: mimart
ms.openlocfilehash: af11283f9e9dbd925ec994dcb1d96393332b90fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80117014"
---
## <a name="use-custom-page-content"></a>Uso del contenido de la página personalizada

La característica de personalización de la interfaz de usuario de la página sirve para personalizar la apariencia y la experiencia de cualquier directiva personalizada. También puede mantener la coherencia visual y de la marca entre la aplicación y Azure AD B2C.

### <a name="how-it-works"></a>Funcionamiento

Azure AD B2C ejecuta código en el explorador del cliente con un enfoque denominado [Uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/). En tiempo de ejecución, el contenido se carga desde una dirección URL especificada en un flujo de usuario o directiva personalizada. Cada página de la experiencia del usuario carga su contenido desde la dirección URL que especifique para esa página. Cuando el contenido se carga desde la dirección URL se combina con un fragmento HTML insertado en Azure AD B2C y, a continuación, se muestra la página al cliente.

![Margen de contenido de la página personalizada](./media/active-directory-b2c-html-templates/html-content-merging.png)

## <a name="custom-html-page-content"></a>Contenido de la página HTML personalizada

Cree una página HTML con su propia personalización de marca para servir el contenido de la página personalizada. Esta página puede ser una página `*.html` estática, o una página dinámica como .NET, Node.js o PHP.

El contenido de la página personalizada puede contener cualquier elemento HTML, incluidos CSS y JavaScript, pero no puede incluir elementos no seguros como iframes. El único elemento necesario es un elemento div con el `id` establecido en `api`, como este `<div id="api"></div>` dentro de la página HTML.

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Product Brand Name</title>
</head>
<body>
    <div id="api"></div>
</body>
</html>
```

### <a name="customize-the-default-azure-ad-b2c-pages"></a>Personalización de las páginas predeterminadas de Azure AD B2C

En lugar de crear el contenido de la página personalizada desde el principio, puede personalizar el contenido de la página predeterminada de Azure AD B2C.

En la tabla siguiente se muestra el contenido de la página predeterminada proporcionado por Azure AD B2C. Descargue los archivos y úselos como punto de partida para crear sus propias páginas personalizadas.

| Página predeterminada | Descripción | Id. de definición de contenido<br/>(solo directiva personalizada) |
|:-----------------------|:--------|-------------|
| [exception.html](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Página de error**. Esta página se muestra cuando se produce una excepción o un error. | *api.error* |
| [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) |  **Página autoafirmada**. Use este archivo como contenido de la página predeterminada para una página de registro en una cuenta social, una página de registro en una cuenta local, una página de inicio de sesión en una cuenta local, el restablecimiento de contraseña y mucho más. El formulario puede contener varios controles de entrada, como un cuadro de entrada de texto, un cuadro de entrada de contraseña, un botón de radio, cuadros desplegables de selección única y casillas de selección múltiple. | *api.localaccountsignin*, *api.localaccountsignup*, *api.localaccountpasswordreset*, *api.selfasserted* |
| [multifactor-1.0.0.html](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Página de autenticación multifactor**. Esta página permite a los usuarios verificar sus números de teléfono (mediante texto o voz) durante el registro o el inicio de sesión. | *api.phonefactor* |
| [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Página de actualización de perfil**. Esta página contiene un formulario que los usuarios pueden usar para actualizar su perfil. Esta página es similar a la página de registro en una cuenta social, excepto por los campos de entrada de contraseña. | *api.selfasserted.profileupdate* |
| [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Página de inicio de sesión o registro unificada**. Esta página controla los procesos de registro y de inicio de sesión del usuario. Los usuarios pueden utilizar proveedores de identidades de empresa, proveedores de identidades sociales como Facebook y Google+ o cuentas locales. | *api.signuporsignin* |

## <a name="hosting-the-page-content"></a>Hospedaje del contenido de la página

Al utilizar sus propios archivos HTML y CSS para personalizar la interfaz de usuario, hospede el contenido de la interfaz de usuario en cualquier punto de conexión HTTPS disponible públicamente que admita CORS. Por ejemplo, [Azure Blob Storage](../articles/storage/blobs/storage-blobs-introduction.md), [Azure App Service](/azure/app-service/), servidores web, redes CDN, AWS S3 o sistemas de uso compartido de archivos.

## <a name="guidelines-for-using-custom-page-content"></a>Directrices para usar el contenido de la página personalizada

- Use una dirección URL absoluta al incluir recursos externos como archivos multimedia, CSS y JavaScript en el archivo HTML.
- Con la [versión de diseño de página](../articles/active-directory-b2c/page-layout.md) 1.2.0 y posterior, puede agregar el atributo `data-preload="true"` en las etiquetas HTML para controlar el orden de carga de CSS y JavaScript. Con `data-preload=true`, la página se construye antes de que se muestre al usuario. Este atributo ayuda a evitar que la página "parpadee" al cargar previamente el archivo CSS, sin que el HTML sin estilo se muestre al usuario. El siguiente fragmento de código HTML muestra el uso de la etiqueta `data-preload`.
  ```HTML
  <link href="https://path-to-your-file/sample.css" rel="stylesheet" type="text/css" data-preload="true"/>
  ```
- Se recomienda comenzar con el contenido de la página predeterminada y compilar sobre él.
- Puede incluir JavaScript en el contenido personalizado tanto para los [flujos de usuario](../articles/active-directory-b2c/user-flow-javascript-overview.md) como para las [directivas personalizadas](../articles/active-directory-b2c/javascript-samples.md).
- Estas son las versiones de explorador admitidas:
  - Internet Explorer 11 y 10 y Microsoft Edge
  - Compatibilidad limitada con Internet Explorer 9 y 8.
  - Google Chrome 42.0 y versiones posteriores
  - Mozilla Firefox 38.0 y versiones posteriores
  - Safari para iOS y macOS, versión 12 y posteriores
- Debido a las restricciones de seguridad, Azure AD B2C no es compatible con los elementos HTML `frame`, `iframe` o `form`.

## <a name="custom-page-content-walkthrough"></a>Tutorial sobre el contenido de la página personalizada

A continuación se muestra una información general acerca del proceso:

1. Prepare una ubicación para hospedar el contenido de la página personalizada (un punto de conexión HTTPS con acceso público y habilitado para CORS).
1. Descargue y personalice un archivo de contenido de la página predeterminada, por ejemplo `unified.html`.
1. Publique el contenido de la página personalizada en el punto de conexión HTTPS disponible públicamente.
1. Establecimiento de uso compartido de recursos entre orígenes (CORS) para la aplicación web.
1. Apunte la directiva al URI de contenido de la directiva personalizada.

### <a name="1-create-your-html-content"></a>1. Creación del contenido HTML

Puede crear contenido de la página personalizada con el nombre de la marca del producto en el título.

1. Copie el siguiente fragmento de código HTML. Es HTML5 bien formado con un elemento vacío denominado *\<div id="api"\>\</div\>* situado en las etiquetas *\<body\>* . Este elemento indica dónde se va a insertar el contenido de Azure AD B2C.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

1. Pegue el fragmento de código copiado en un editor de texto y guarde el archivo como *customize-ui.html*.

> [!NOTE]
> Si se usa login.microsoftonline.com, los elementos de formulario HTML se quitarán debido a las restricciones de seguridad. Si quiere usar elementos de formulario HTML en el contenido HTML personalizado, [utilice b2clogin.com](../articles/active-directory-b2c/b2clogin.md).

### <a name="2-create-an-azure-blob-storage-account"></a>2. Creación de una cuenta de Almacenamiento de blobs de Azure

En este artículo se usa Azure Blob Storage para hospedar el contenido. Puede elegir hospedar el contenido en un servidor web, pero deberá [habilitar CORS en el servidor web](https://enable-cors.org/server.html).

Para hospedar el contenido HTML en Blob Storage, haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En el menú **central**, seleccione **Nuevo** > **Almacenamiento** > **Cuenta de almacenamiento**.
1. Seleccione una **Suscripción** para la cuenta de almacenamiento.
1. Cree un **Grupo de recursos** o seleccione uno existente.
1. Escriba un **Nombre** único para la cuenta de almacenamiento.
1. Seleccione la **Ubicación geográfica** de la cuenta de almacenamiento.
1. El **Modelo de implementación** puede permanecer como **Resource Manager**.
1. El **Rendimiento** puede permanecer como **Estándar**.
1. Cambie **Tipo de cuenta** a **Almacenamiento de blobs**.
1. La **Replicación** puede permanecer como **RA-GRS**.
1. El **Nivel de acceso** puede permanecer **Activo**.
1. Haga clic en **Revisar y crear** para crear la cuenta de almacenamiento.
    Espere a que termine la implementación y la página de la **cuenta de almacenamiento** se abrirá automáticamente.

#### <a name="21-create-a-container"></a>2.1 Creación de un contenedor

Para crear un contenedor público en Blob Storage, realice los siguientes pasos:

1. En **Blob service** en el menú izquierdo, seleccione **Blobs**.
1. Seleccione **+ Contenedor**.
1. En **Nombre**, escriba *root*. El nombre puede ser el que quiera, por ejemplo *contoso*; sin embargo, usamos *root* en este ejemplo por motivos de simplicidad.
1. Para **Nivel de acceso público**, seleccione **Blob** y, luego, **Aceptar**.
1. Seleccione **root** para abrir el contenedor nuevo.

#### <a name="22-upload-your-custom-page-content-files"></a>2.2 Carga de los archivos de contenido de la página personalizada

1. Seleccione **Cargar**.
1. Haga clic en el icono de carpeta junto a **Seleccionar un archivo**.
1. Vaya a **customize-ui.html**, que creó anteriormente en la sección Personalización de la interfaz de usuario de la página, y selecciónelo.
1. Si quiere cargar en una subcarpeta, expanda **Avanzadas** y escriba un nombre de la carpeta en **Cargar en carpeta**.
1. Seleccione **Cargar**.
1. Seleccione el blob **customize-ui.html** que cargó.
1. A la derecha del cuadro de texto **URL**, seleccione el icono **Copiar al Portapapeles** para copiar la dirección URL en el Portapapeles.
1. En el explorador web, vaya a la dirección URL que copió para verificar que puede acceder al blob que cargó. Si no puede acceder, por ejemplo, si hay un error `ResourceNotFound`, asegúrese de que el tipo de acceso del contenedor esté establecido en **blob**.

### <a name="3-configure-cors"></a>3. Configuración de CORS

Para configurar Blob Storage para el uso compartido de recursos entre orígenes (CORS), realice los siguientes pasos:

1. En el menú, seleccione **CORS**.
1. En **Orígenes permitidos**, escriba `https://your-tenant-name.b2clogin.com`. Reemplace `your-tenant-name` por el nombre del inquilino de Azure AD B2C. Por ejemplo, `https://fabrikam.b2clogin.com`. Al escribir su nombre de inquilino, use solo minúsculas.
1. En **Métodos permitidos**, seleccione `GET` y `OPTIONS`.
1. En **Encabezados permitidos**, escriba un asterisco (*).
1. En **Encabezados expuestos**, escriba un asterisco (*).
1. Para **Antigüedad máxima**, introduzca 200.
1. Seleccione **Guardar**.

#### <a name="31-test-cors"></a>3.1 Prueba de CORS

Para comprobar que está listo, realice los siguientes pasos:

1. Repita el paso de configuración de CORS. En **Orígenes permitidos**, escriba `https://www.test-cors.org`.
1. Vaya a [www.test-cors.org](https://www.test-cors.org/). 
1. En el cuadro de **URL remotas**, pegue la dirección URL del archivo HTML. Por ejemplo: `https://your-account.blob.core.windows.net/azure-ad-b2c/unified.html`
1. Seleccione **Enviar solicitud**.
    El resultado debe ser `XHR status: 200`. 
    Si recibe un error, asegúrese de que la configuración de CORS sea correcta. Puede que también deba borrar la caché del explorador o abrir una sesión de navegación privada, para ello, presione Ctrl + Mayús + P.
