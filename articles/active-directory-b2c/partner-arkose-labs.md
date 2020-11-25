---
title: Arkose Labs con Azure Active Directory B2C
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo integrar la autenticación de Azure AD B2C con Arkose Labs para ayudar a protegerse contra ataques de bots, ataques de adquisición de cuentas y aperturas de cuentas fraudulentas.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/08/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 333bb42643539cedec04d37680749c749a003536
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95994067"
---
# <a name="tutorial-for-configuring-arkose-labs-with-azure-active-directory-b2c"></a>Tutorial para configurar Arkose Labs con Azure Active Directory B2C

En este tutorial, aprenderá a integrar la autenticación de Azure AD B2C con Arkose Labs. Arkose Labs ayuda a las organizaciones contra ataques de bots, ataques de adquisición de cuentas y aperturas de cuentas fraudulentas.  

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesitará lo siguiente:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* [Un inquilino de Azure AD B2C](tutorial-create-tenant.md) vinculado a la suscripción de Azure.

## <a name="scenario-description"></a>Descripción del escenario

En el diagrama siguiente se describe cómo Arkose Labs se integra con Azure AD B2C.

![Diagrama de la arquitectura de Arkose Labs](media/partner-arkose-labs/arkose-architecture-diagram.png)

| Paso  | Descripción |
|---|---|
|1     | Un usuario inicia sesión con una cuenta creada anteriormente. Cuando el usuario selecciona Enviar, aparece un desafío de cumplimiento de Arkose Labs. Una vez que el usuario completa el desafío, el estado se envía a Arkose Labs para generar un token.        |
|2     |  Arkose Labs envía el token de vuelta a Azure AD B2C.       |
|3     |  Antes de enviar el formulario de inicio de sesión, el token se envía a Arkose Labs para su comprobación.       |
|4     |  Arkose devuelve un resultado correcto o incorrecto del desafío.       |
|5     |  Si el desafío se completa correctamente, se envía un formulario de inicio de sesión a Azure AD B2C, y Azure AD B2C completa la autenticación.       |
|   |   |

## <a name="onboard-with-arkose-labs"></a>Incorporación con Arkose Labs

1. Para empezar, póngase en contacto con [Arkose Labs](https://www.arkoselabs.com/book-a-demo/) y cree una cuenta.

2. Una vez creada la cuenta, vaya a https://dashboard.arkoselabs.com/login.

3. En el panel, vaya a la configuración del sitio para buscar la clave pública y la clave privada. Esta información se necesitará más adelante para configurar Azure AD B2C.

## <a name="integrate-with-azure-ad-b2c"></a>Integración con Azure AD B2C

### <a name="part-1--create-blob-storage-to-store-the-custom-html"></a>Parte 1: Creación de almacenamiento de blobs para almacenar el código HTML personalizado

Para crear una cuenta de almacenamiento, siga estos pasos:  

1. Inicie sesión en Azure Portal.

2. Asegúrese de que usa el directorio que contiene su suscripción de Azure. Seleccione el filtro  **Directorio y suscripciones** en el menú superior y elija el directorio que contiene su suscripción. Este directorio es diferente que el de la carpeta que contiene el inquilino de Azure B2C.

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal y, después, busque y seleccione  **Cuentas de almacenamiento**.

4. Seleccione  **Agregar**.

5. En  **Grupo de recursos**, seleccione  **Crear nuevo** y, después, proporcione un nombre para el nuevo grupo de recursos y seleccione **Aceptar**.

6. Escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe ser único en Azure, tener entre tres y 24 caracteres y solo pueden contener números y letras minúsculas.

7. Seleccione la ubicación de la cuenta de almacenamiento o acepte la ubicación predeterminada.

8. Acepte los demás valores predeterminados y seleccione   **Revisar y crear** > **Crear**.

9. Una vez creada la cuenta de almacenamiento, seleccione  **Ir al recurso**.

#### <a name="create-a-container"></a>Crear un contenedor

1. En la página de información general de la cuenta de almacenamiento, seleccione   **Blobs**.

2. Seleccione   **Contenedor**, escriba un nombre para el contenedor, elija   **Blob (acceso de lectura anónimo solo para blobs)** y, después, seleccione **Aceptar**.

#### <a name="enable-cross-origin-resource-sharing-cors"></a>Habilitar el uso compartido de recursos entre orígenes (CORS)

El código de Azure AD B2C en un explorador utiliza un enfoque moderno y estándar para cargar contenido personalizado desde una dirección URL que especifique en un flujo de usuario. CORS permite que los recursos restringidos en una página web se soliciten desde otros dominios.

1. En el menú, seleccione   **CORS**.

2. En   **Orígenes permitidos**, escriba  `https://your-tenant-name.b2clogin.com`. Reemplace el nombre del inquilino por el nombre del inquilino de Azure AD B2C. Por ejemplo:  `https://fabrikam.b2clogin.com`. Al escribir su nombre de inquilino, use solo minúsculas.

3. En  **Métodos permitidos**, seleccione  **GET**, **PUT** y  **OPTIONS**.

4. En **Encabezados permitidos**, escriba un asterisco (*).

5. En  **Encabezados expuestos**, escriba un asterisco (*).

6. Para **Antigüedad máxima**, introduzca 200.

   ![Registro e inicio de sesión de Arkose Labs](media/partner-arkose-labs/signup-signin-arkose.png)

7. Seleccione **Guardar**.

### <a name="part-2--set-up-a-back-end-server"></a>Parte 2: Configuración de un servidor back-end

Descargue Git Bash y siga estos pasos:

1. Siga las instrucciones para [crear una aplicación web](../app-service/quickstart-php.md), hasta que aparezca el mensaje "Enhorabuena. Ha implementado su primera aplicación PHP en App Service".

2. Abra la carpeta local y cambie el nombre del archivo **index.php** por **verify-token.php**.

3. Abra el archivo verify-token.php al que acaba de cambiar el nombre y:

   a. Reemplace el contenido por el contenido del archivo verify-token.php que se encuentra en el [repositorio de GitHub](https://github.com/ArkoseLabs/Azure-AD-B2C).

   b. Reemplace <private_key> en la línea 3 por la clave privada obtenida en el panel de Arkose Labs.

4. En la ventana del terminal local, confirme los cambios en Git e inserte los cambios de código en Azure; para ello, escriba lo siguiente en Git Bash:

   ``git commit -am "updated output"``

   ``git push azure master``  

### <a name="part-3---final-setup"></a>Parte 3: Configuración final

#### <a name="store-the-custom-html"></a>Almacenamiento del código HTML personalizado

1. Abra el archivo index.html almacenado en el [repositorio de GitHub](https://github.com/ArkoseLabs/Azure-AD-B2C).

2. Reemplace todas las instancias de `<tenantname>` por el nombre del inquilino de B2C (es decir, `<tenantname>.b2clogin.com`). Debe haber cuatro instancias.

3. Reemplace `<appname>` por el nombre de la aplicación que creó en la parte 2, paso 1.

   ![Captura de pantalla que muestra la CLI de Azure para Arkose Labs](media/partner-arkose-labs/arkose-azure-cli.png)

4. Reemplace `<public_key>` en la línea 64 por la clave pública obtenida en el panel de Arkose Labs.

5. Cargue el archivo index.html en el almacenamiento de blobs creado anteriormente.

6. Vaya a **Almacenamiento** > **Contenedor** > **Cargar**.

#### <a name="set-up-azure-ad-b2c"></a>Configuración de Azure AD B2C

> [!NOTE]
> Si todavía no tiene uno, [cree un inquilino de Azure AD B2C](tutorial-create-tenant.md) vinculado a la suscripción de Azure.

1. Cree un flujo de usuario en función de la información disponible [aquí](tutorial-create-user-flows.md). Deténgase cuando llegue a la sección **Prueba del flujo de usuario**.

2. Habilite JavaScript en el [flujo de usuario](user-flow-javascript-overview.md).

3. En la misma página del flujo de usuario, habilite la dirección URL de la página personalizada: Vaya a **Flujo de usuario** > **Diseño de página** > **Usar contenido de la página personalizada** = **Sí** > **insert custom page URL** (Insertar la dirección URL de la página personalizada).
Esta dirección URL de página personalizada se obtiene de la ubicación del archivo index.html dentro de Blob Storage.  

   ![Captura de pantalla que muestra la dirección URL de almacenamiento de Arkose Labs](media/partner-arkose-labs/arkose-storage-url.png)

## <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Abra el inquilino de Azure AD B2C y, en **Directivas**, seleccione **Flujos de usuario**.

2. Seleccione el flujo de usuario que creó anteriormente.

3. Seleccione **Ejecutar flujo de usuario** y elija la configuración:

   a. **Aplicación**: seleccione la aplicación registrada (en el ejemplo es JWT).

   b. **URL de respuesta**: seleccione la dirección URL de redireccionamiento.

   c. Seleccione **Ejecutar flujo de usuario**.

4. Recorra el flujo de registro y cree una cuenta.

5. Cierre la sesión.

6. Recorra el flujo de inicio de sesión.

7. Aparecerá un rompecabezas de Arkose Labs después de seleccionar **Continuar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los artículos siguientes:

- [Directivas personalizadas de Azure AD B2C](custom-policy-overview.md)

- [Introducción a las directivas personalizadas en Azure AD B2C](custom-policy-get-started.md?tabs=applications)