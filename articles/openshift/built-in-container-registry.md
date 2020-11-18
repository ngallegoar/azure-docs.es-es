---
title: Configuración del registro de contenedor integrado para Red Hat OpenShift 4 en Azure
description: Configuración del registro de contenedor integrado para Red Hat OpenShift 4 en Azure
author: jiangma
ms.author: jiangma
ms.service: container-service
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 917da58c7f5ac2294fc30cd385a4834fde3f5f0b
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94413744"
---
# <a name="configure-built-in-container-registry-for-azure-red-hat-openshift-4"></a>Configuración del registro de contenedor integrado para Red Hat OpenShift 4 en Azure

En este artículo, configurará el registro de imágenes de contenedor integrado para un clúster de Red Hat OpenShift en Azure (ARO) 4. Aprenderá a:

> [!div class="checklist"]
> * Configuración de Azure AD
> * Configurar OpenID Connect
> * Acceder al registro de imágenes de contenedor integrado

## <a name="prerequisites"></a>Requisitos previos

* Cree un clúster según los pasos que aparecen en [Creación de un clúster de Red Hat OpenShift en Azure 4](/azure/openshift/tutorial-create-cluster). Asegúrese de crear el clúster con el argumento `--pull-secret` en `az aro create`.  Esto es necesario si quiere configurar Azure AD para el inicio de sesión, tal como lo requiere este artículo.
* Para conectarse al clúster, siga los pasos que aparecen en [Conexión a un clúster de Red Hat OpenShift en Azure 4](/azure/openshift/tutorial-connect-cluster).
   * Asegúrese de seguir los pasos que aparecen en "Instalación de la CLI de OpenShift", porque usaremos el comando `oc` más adelante en este artículo.
   * Anote la dirección URL de la consola del clúster, que tiene este aspecto: `https://console-openshift-console.apps.<random>.<region>.aroapp.io/`. Los valores de `<random>` y `<region>` se usarán más adelante en este artículo.
   * Anote las credenciales de `kubeadmin`. Se usarán más adelante en este artículo.

## <a name="set-up-azure-active-directory"></a>Configuración de Azure Active Directory

Azure Active Directory (Azure AD) implementa OpenID Connect (OIDC). OIDC le permite usar Azure AD para iniciar sesión en el clúster de ARO. Siga los pasos que se indican a continuación para configurar Azure AD.

1. Configure un inquilino de Azure AD según los pasos que aparecen en [Inicio rápido: Configuración de un inquilino](/azure/active-directory/develop/quickstart-create-new-tenant). Puede que su cuenta de Azure ya tenga un inquilino. Si es así, puede omitir la creación de uno si tiene privilegios suficientes en el inquilino para completar los pasos. Anote el **id. del inquilino**. Este valor se usará más adelante.
2. Cree al menos un usuario de Azure AD según los pasos descritos en [Incorporación o eliminación de usuarios mediante Azure Active Directory](/azure/active-directory/fundamentals/add-users-azure-active-directory). Puede usar estas cuentas o sus cuentas propias para probar la aplicación. Anote las direcciones de correo electrónico y las contraseñas de estas cuentas para iniciar sesión.
3. Cree un registro de aplicación nuevo en el inquilino de Azure AD según los pasos que se indican en [Inicio rápido: Registro de una aplicación en la plataforma de identidad de Microsoft](/azure/active-directory/develop/quickstart-register-app). 
   1. Recupere lo que anotó en los requisitos previos sobre los valores de `<random>` y `<region>`. Use estos valores para crear un identificador URI según la fórmula siguiente:

      `https://oauth-openshift.apps.<random>.<region>.aroapp.io/oauth2callback/openid`
   1. Cuando llegue al paso donde se usa el campo **URI de redireccionamiento**, escriba el URI del paso anterior.
   1. Seleccione **Registrar**.
   1. En la página de **información general** de la aplicación, anote el valor mostrado para el **id. de la aplicación (cliente)** , tal como se muestra aquí.
      :::image type="content" source="media/built-in-container-registry/azure-ad-app-overview-client-id.png" alt-text="Página de información general de la aplicación de Azure AD.":::

4. Cree un secreto de cliente. 
   1. En el registro de aplicación recién creado, seleccione **Certificados y secretos** en el panel de navegación de la izquierda.
   1. Seleccione **Nuevo secreto de cliente**.
   1. Proporcione **una descripción** y seleccione **Agregar**.
   1. Guarde en otro lugar el valor del **secreto de cliente** generado. Va a usar este valor más adelante en el artículo.

### <a name="add-openid-connect-identity-provider"></a>Incorporación del proveedor de identidades de OpenID Connect

ARO proporciona un registro de contenedor integrado.  Para acceder a él, siga estos pasos para configurar un proveedor de identidades (IDP) con OIDC de Azure AD.

1. Inicie sesión en la consola web de OpenShift desde el explorador como `kubeadmin`.  Se trata del mismo procedimiento que se usa al ejecutar los pasos que aparecen en [Tutorial: Conexión a un clúster de Red Hat OpenShift en Azure 4](/azure/openshift/tutorial-connect-cluster).
1. En el panel de navegación de la izquierda, seleccione **Administración** > **Configuración del clúster**.
1. En el medio de la página, seleccione **Configuración global**.
1. Busque **OAuth** en la columna **Recurso de configuración** de la tabla y selecciónelo.
1. En **Proveedores de identidades**, seleccione **Agregar** y elija **OpenID Connect**.
1. Establezca el **Nombre** como **openid**.  Es posible que este valor ya esté rellenado.
1. Establezca el **id. de cliente** y el **secreto de cliente** como los valores del paso anterior.
1. Siga este paso para buscar el valor de la **dirección URL del emisor**.
   1. Reemplace el valor de **\<tenant-id>** por el que guardó en la sección **Configuración de Azure Active Directory** en la dirección URL `https://login.microsoftonline.com/<tenant-id>/v2.0/.well-known/openid-configuration`.
   1. Abra la dirección URL en el mismo explorador que ha usado para interactuar con Azure Portal.
   1. Copie el valor de la propiedad **issuer** en el cuerpo de JSON devuelto y péguelo en el cuadro de texto con la etiqueta **URL del emisor**.  El valor **issuer** tendrá un aspecto similar a `https://login.microsoftonline.com/44p4o433-2q55-474q-on88-4on94469o74n/v2.0`.
1. Vaya a la parte inferior de la página y seleccione **Agregar**.
   :::image type="content" source="media/built-in-container-registry/openid-connect-identity-provider.png" alt-text="OpenID Connect en OpenShift.":::
1. Para cerrar la sesión de la consola web de OpenShift, seleccione el botón **kube:admin** en la esquina superior derecha de la ventana del explorador y elija **Cerrar sesión**.

### <a name="access-the-built-in-container-image-registry"></a>Acceder al registro de imágenes de contenedor integrado

Las instrucciones siguientes permiten acceder al registro integrado.

#### <a name="define-the-azure-ad-user-to-be-an-administrator"></a>Definición del usuario de Azure AD como administrador

1. Inicie sesión en la consola web de OpenShift desde el explorador con las credenciales de un usuario de Azure AD.

   1. Use InPrivate, Incógnito u otra característica de ventana del explorador equivalente para iniciar sesión en la consola.
   1. La ventana se verá distinta después de habilitar OIDC.
   :::image type="content" source="media/built-in-container-registry/oidc-enabled-login-window.png" alt-text="Ventana de inicio de sesión habilitada para OpenID Connect.":::
   1. Seleccione **openid**.

   > [!NOTE]
   > Anote el nombre de usuario y la contraseña que usa para iniciar sesión aquí. Este nombre de usuario y contraseña funcionarán como administrador para otras acciones en este y en otros artículos.
1. Siga estos pasos para iniciar sesión con la CLI de OpenShift.  Para el análisis, este proceso se conoce como `oc login`.
   1. En la parte superior derecha de la consola web, expanda el menú contextual del usuario que inició sesión y, luego, seleccione **Copy Login Command** (Copiar comando de inicio de sesión).
   1. Si es necesario, inicie sesión en una pestaña nueva con el mismo usuario.
   1. Seleccione **Mostrar token**.
   1. Copie el valor que aparece debajo de **Iniciar sesión con este token** en el portapapeles y ejecútelo en un shell, como se muestra aquí.

       ```bash
       oc login --token=XOdASlzeT7BHT0JZW6Fd4dl5EwHpeBlN27TAdWHseob --server=https://api.aqlm62xm.rnfghf.aroapp.io:6443
       Logged into "https://api.aqlm62xm.rnfghf.aroapp.io:6443" as "kube:admin" using the token provided.

       You have access to 57 projects, the list has been suppressed. You can list all projects with 'oc projects'

       Using project "default".
       ```

1. Ejecute `oc whoami` en la consola y anote la salida como **\<aad-user>** .  Usaremos este valor más adelante en este artículo.
1. Cierre la sesión de la consola web de OpenShift. Seleccione el botón en la esquina superior derecha de la ventana del explorador con la etiqueta **\<aad-user>** y elija **Cerrar sesión**.


#### <a name="grant-the-azure-ad-user-the-necessary-roles-for-registry-interaction"></a>Concesión de los roles necesarios para la interacción con el registro al usuario de Azure AD

1. Inicie sesión en la consola web de OpenShift desde el explorador con las credenciales de `kubeadmin`.
1. Inicie sesión en la CLI de OpenShift con el token para `kubeadmin` según los pasos para `oc login` mencionados anteriormente, pero hágalo después de iniciar sesión en la consola web con `kubeadmin`.
1. Ejecute los comandos siguientes para habilitar el acceso al registro integrado correspondiente a **aad-user**.

   ```bash
   # Switch to project "openshift-image-registry"
   oc project openshift-image-registry
   ```

   La salida debe tener una apariencia similar a la siguiente.

   ```bash
   Now using project "openshift-image-registry" on server "https://api.x8xl3f4y.eastus.aroapp.io:6443".
   ```

   ```bash
   # Expose the registry using "DefaultRoute"
   oc patch configs.imageregistry.operator.openshift.io/cluster --patch '{"spec":{"defaultRoute":true}}' --type=merge
   ```

   La salida debe tener una apariencia similar a la siguiente.

   ```bash
   config.imageregistry.operator.openshift.io/cluster patched
   ```

   ```bash
   # Add roles to "aad-user" for pulling and pushing images
   # Note: replace "<aad-user>" with the one you wrote down before
   oc policy add-role-to-user registry-viewer <aad-user>
   ```

   La salida debe tener una apariencia similar a la siguiente.

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-viewer added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

   ```bash
   oc policy add-role-to-user registry-editor <aad-user>
   ```

   La salida debe tener una apariencia similar a la siguiente.

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-editor added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

#### <a name="obtain-the-container-registry-url"></a>Obtención de la dirección URL del registro de contenedor

Use el comando `oc get route` como se muestra a continuación para obtener la dirección URL del registro de contenedor.

```bash
# Note: the value of "Container Registry URL" in the output is the fully qualified registry name.
HOST=$(oc get route default-route --template='{{ .spec.host }}')
echo "Container Registry URL: $HOST"
```

   > [!NOTE]
   > Anote la salida de la consola correspondiente a la **dirección URL del registro de contenedor**. Se usará como el nombre completo del registro en esta guía y las siguientes.

## <a name="next-steps"></a>Pasos siguientes

Implemente una aplicación en OpenShift para usar el registro de imágenes de contenedor integrado.  En el caso de las aplicaciones Java, siga los procedimientos que aparecen en [Implementación de una aplicación Java con Open Liberty/WebSphere Liberty en un clúster de Red Hat OpenShift en Azure 4](howto-deploy-java-liberty-app.md).
