---
title: Integración de Azure Active Directory con Azure Kubernetes Service
description: Creación de clústeres de Azure Kubernetes Service (AKS) habilitados para Azure Active Directory
services: container-service
ms.topic: article
ms.date: 02/02/2019
ms.openlocfilehash: de57a46f92fab2486aa7722daf8745a01be1f4f6
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617594"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Integración de Azure Active Directory con Azure Kubernetes Service

Es posible configurar Azure Kubernetes Service (AKS) para que utilice Azure Active Directory (Azure AD) para la autenticación de usuarios. En esta configuración, puede iniciar sesión en un clúster de AKS mediante su token de autenticación de Azure AD.

Los administradores del clúster pueden configurar el control de acceso basado en rol (RBAC) de Kubernetes en función de la identidad de los usuarios o su pertenencia a un grupo del directorio.

En este artículo se explica lo siguiente:

- Implemente los requisitos previos para AKS y Azure AD.
- Implemente un clúster habilitado para Azure AD.
- Cree un rol RBAC básico en el clúster de AKS mediante Azure Portal.

También puede completar estos pasos con la [CLI de Azure][azure-ad-cli].

> [!NOTE]
> Azure AD solo puede habilitarse cuando se crea un nuevo clúster habilitado para RBAC. No se puede habilitar Azure AD en un clúster AKS existente.

## <a name="authentication-details"></a>Detalles de la autenticación

La autenticación de Azure AD se proporciona a los clústeres de AKS con OpenID Connect. OpenID Connect es una capa de identidad creada basándose en el protocolo OAuth 2.0.

Para obtener más información sobre OpenID Connect, consulte [Autorización del acceso a aplicaciones web con OpenID Connect y Azure AD][open-id-connect].

Dentro de un clúster de Kubernetes, se usa la autenticación de token de webhook para los tokens de autenticación. La autenticación de token de webhook se configura y administra como parte del clúster de AKS.

Para obtener más información sobre la autenticación de token de webhook, consulte la sección [Webhook Token Authentication][kubernetes-webhook] (Autenticación de token de webhook) de la documentación de Kubernetes.

Para proporcionar autenticación de Azure AD para un clúster de AKS, se crean dos aplicaciones de Azure AD. La primera aplicación es un componente de servidor que proporciona autenticación de usuario. La segunda es un componente de cliente que se usa cuando la CLI le solicita que se autentique. Esta aplicación cliente usa la aplicación de servidor para la autenticación real de las credenciales que proporciona el cliente.

> [!NOTE]
> Al configurar Azure AD para la autenticación de AKS, se configuran dos aplicaciones de Azure AD. Los pasos para delegar permisos para cada aplicación debe realizarlos un administrador de inquilinos de Azure.

## <a name="create-the-server-application"></a>Creación de la aplicación de servidor

La primera aplicación de Azure AD se aplica para obtener la pertenencia a un grupo de Azure AD del usuario. Para crear esta aplicación en Azure Portal:

1. Haga clic en **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.

    a. Asigne un nombre a la aplicación como *AKSAzureADServer*.

    b. Para la opción **Tipos de cuenta admitidos**, seleccione **Solo las cuentas de este directorio organizativo**.
    
    c. Elija **Web** para el tipo de URI de redirección y, a continuación, escriba cualquier valor con formato URI, como *https://aksazureadserver* .

    d. Cuando haya finalizado, seleccione **Registro**.

2. Seleccione **Manifiesto** y, a continuación, edite el valor **groupMembershipClaims:** como **Todos**. Cuando haya terminado con las actualizaciones, seleccione **Guardar**.

    ![Actualizar la pertenencia al grupo a All (Todos)](media/aad-integration/edit-manifest.png)

3. En el panel izquierdo de la aplicación de Azure AD, seleccione **Certificados y secretos**.

    a. Seleccione **+ Nuevo secreto de cliente**.

    b. Agregue una descripción de la clave, como *servidor de Azure AD de AKS*. Elija una hora de expiración y, a continuación, seleccione **Agregar**.

    c. Tenga en cuenta el valor de clave, ya que solo se muestra en este momento. Al implementar un clúster de AKS habilitado para Azure AD, este valor se denomina el secreto de aplicación de servidor.

4. En el panel izquierdo de la aplicación de Azure AD, seleccione **Permisos de API** y, a continuación, seleccione **+ Agregar un permiso**.

    a. En **API de Microsoft**, seleccione **Microsoft Graph**.

    b. Seleccione **Permisos delegados** y, a continuación, seleccione la casilla junto a **Directorio > Directory.Read.All (Leer datos de directorio)** .

    c. Si no existe ningún permiso delegado predeterminado para **Usuario > User.Read (Iniciar sesión y leer el perfil de usuario)** , seleccione la casilla que se encuentra al lado.

    d. Seleccione **Permisos de la aplicación** y, a continuación, seleccione la casilla junto a **Directorio > Directory.Read.All (Leer datos de directorio)** .

    ![Establecer permisos de gráficos](media/aad-integration/graph-permissions.png)

    e. Seleccione **Agregar permisos** para guardar las actualizaciones.

    f. En **Otorgar consentimiento**, seleccione **Conceder consentimiento de administrador**. Este botón no estará disponible si la cuenta que se usa no aparece como administrador de inquilinos.

    Una vez se han concedido los permisos correctamente, la siguiente notificación se muestra en el portal:

   ![Notificación de concesión de permisos correcta](media/aad-integration/permissions-granted.png)

5. En el panel izquierdo de la aplicación de Azure AD, seleccione **Exponer una API** y, a continuación, seleccione **+ Agregar un ámbito**.
    
    a. Escriba un **nombre de ámbito**, un **nombre para mostrar del consentimiento del administrador** y, luego, una **descripción del consentimiento del administrador**, como *AKSAzureADServer*.

    b. Asegúrese de que el **estado** esté establecido en **Habilitado**.

    ![Exponer la aplicación de servidor como una API para su uso con otros servicios](media/aad-integration/expose-api.png)

    c. Seleccione la opción **Agregar un ámbito**.

6. Vuelva a la página **Descripción general**  de la aplicación y tome nota del **id. de aplicación (cliente)** . Al implementar un clúster de AKS habilitado para Azure AD, este valor se denomina id. de aplicación de servidor.

    ![Obtención del identificador de la aplicación](media/aad-integration/application-id.png)

## <a name="create-the-client-application"></a>Creación de la aplicación cliente

La segunda aplicación de Azure AD se usa cuando inicia sesión con la CLI de Kubernetes (kubectl).

1. Haga clic en **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.

    a. Asigne un nombre a la aplicación como *AKSAzureADClient*.

    b. Para la opción **Tipos de cuenta admitidos**, seleccione **Solo las cuentas de este directorio organizativo**.

    c. Seleccione **Web** para el tipo de URI de redirección y, a continuación, escriba cualquier valor con formato URI, como *https://aksazureadclient* .

    >[!NOTE]
    >Si está creando un nuevo clúster habilitado para RBAC para admitir Azure Monitor para contenedores, agregue las dos direcciones URL de redireccionamiento adicionales que se muestran a continuación a esta lista como tipos de aplicación **web**. El primer valor de URL base debe ser `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` y el segundo, `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.
    >
    >Si usa esta característica en Azure China, el primer valor de dirección URL base debe ser `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` y el segundo, `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.
    >
    >Para obtener más información, consulte [Cómo configurar la característica de datos en directo (versión preliminar)](../azure-monitor/insights/container-insights-livedata-setup.md) de Azure Monitor para contenedores y los pasos para configurar la autenticación en la sección [Configuración de la autenticación integrada de AD](../azure-monitor/insights/container-insights-livedata-setup.md#configure-ad-integrated-authentication).

    d. Cuando haya finalizado, seleccione **Registro**.

2. En el panel izquierdo de la aplicación de Azure AD, seleccione **Permisos de API** y, a continuación, seleccione **+ Agregar un permiso**.

    a. Seleccione **Mis API** y, a continuación, elija la aplicación de servidor de Azure AD que creó en el paso anterior, como *AKSAzureADServer*.

    b. Seleccione **Permisos delegados** y, a continuación, seleccione la casilla situada junto a la aplicación de servidor de Azure AD.

    ![Configuración de los permisos de la aplicación](media/aad-integration/select-api.png)

    c. Seleccione **Agregar permisos**.

    d. En **Otorgar consentimiento**, seleccione **Conceder consentimiento de administrador**. Este botón no está disponible si la cuenta actual no es de un administrador de inquilinos. Una vez se han concedido los permisos, la siguiente notificación se muestra en el portal:

    ![Notificación de concesión de permisos correcta](media/aad-integration/permissions-granted.png)

3. En el panel izquierdo de la aplicación de Azure AD, seleccione **Autenticación**.

    - En **Tipo de cliente predeterminado**, seleccione **Sí** en **Treat the client as a public client** (Tratar el cliente como un cliente público).

5. En el panel izquierdo de la aplicación de Azure AD, tome nota del id. de la aplicación. Al implementar un clúster de AKS habilitado para Azure AD, este valor se denomina id. de aplicación cliente.

   ![Obtención del identificador de la aplicación](media/aad-integration/application-id-client.png)

## <a name="get-the-tenant-id"></a>Obtención del identificador de inquilino

Por último, obtenga el id. de su inquilino de Azure. Este valor se utiliza al crear el clúster de AKS.

En Azure Portal, seleccione **Azure Active Directory** > **Propiedades** y tome nota del **Id. de directorio**. Al crear un clúster de AKS habilitado para Azure AD, este valor se denomina id. de inquilino.

![Obtención del identificador de inquilino de Azure](media/aad-integration/tenant-id.png)

## <a name="deploy-the-aks-cluster"></a>Implementación de un clúster de AKS

Para crear un grupo de recursos para el clúster de AKS, use el comando [az group create][az-group-create].

```azurecli
az group create --name myResourceGroup --location eastus
```

Use el comando [az aks create][az-aks-create] para implementar el clúster de AKS. A continuación, reemplace los valores en el siguiente comando de ejemplo. Utilice los valores recopilados al crear las aplicaciones de Azure AD para el id. de aplicación de servidor, secreto de la aplicación, id. de la aplicación cliente e id. de inquilino.

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

Un clúster de AKS tarda unos minutos en crearse.

## <a name="create-an-rbac-binding"></a>Creación de un enlace de RBAC

> [!NOTE]
> El nombre del enlace del rol de clúster distingue mayúsculas de minúsculas.

Para poder usar una cuenta de Azure Active Directory con un clúster de AKS, debe crear un enlace de rol o de rol de clúster. Los roles definen los permisos que se deben otorgar, y los enlaces aplican dichos roles a los usuarios deseados. Estas asignaciones se pueden aplicar a un espacio de nombres especificado o a todo el clúster. Para obtener más información, consulte [Uso de la autorización de RBAC][rbac-authorization].

En primer lugar, use el comando [az aks get-credentials][az-aks-get-credentials] con el argumento `--admin` para iniciar sesión en el clúster con acceso de administrador.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

A continuación, cree ClusterRoleBinding para una cuenta de Azure AD a la que quiera conceder acceso al clúster de AKS. El ejemplo siguiente asigna a la cuenta acceso completo a todos los espacios de nombres del clúster:

- Si el usuario al que concede el enlace de RBAC se encuentra en el mismo inquilino de Azure AD, asigne permisos basados en el nombre principal de usuario (UPN). Luego, vaya al paso para crear el manifiesto YAML para ClusterRoleBinding.

- Si el usuario se encuentra en un inquilino distinto de Azure AD, en su lugar, consulte y use la propiedad **objectId**. Si es necesario, obtenga el valor de objectId de la cuenta de usuario necesaria mediante el comando [az ad user show][az-ad-user-show]. Proporcione el nombre principal de usuario (UPN) de la cuenta requerida:

    ```azurecli-interactive
    az ad user show --upn-or-object-id user@contoso.com --query objectId -o tsv
    ```

Cree un archivo, como *rbac-aad-user.yaml* y, luego, pegue el siguiente contenido. En la última línea, reemplace **userPrincipalName_or_objectId** por el UPN o el id de objeto. La elección depende de si el usuario es el mismo inquilino de Azure AD o no.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: userPrincipalName_or_objectId
```

Aplique el enlace mediante el comando [kubectl apply][kubectl-apply], tal como se muestra en el ejemplo siguiente:

```console
kubectl apply -f rbac-aad-user.yaml
```

También se puede crear un enlace de rol para todos los miembros de un grupo de Azure AD. Los grupos de Azure AD se especifican mediante el id. de objeto de grupo, tal y como se muestra en el siguiente ejemplo.

Cree un archivo, como *rbac-aad-group.yaml* y pegue el siguiente contenido. Actualice el identificador del objeto de grupo con uno de su inquilino de Azure AD:

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

Aplique el enlace mediante el comando [kubectl apply][kubectl-apply], tal como se muestra en el ejemplo siguiente:

```console
kubectl apply -f rbac-aad-group.yaml
```

Para obtener más información sobre cómo proteger un clúster de Kubernetes con RBAC, consulte [Uso de la autorización de RBAC][rbac-authorization].

## <a name="access-the-cluster-with-azure-ad"></a>Acceso al clúster con Azure AD

Extraiga el contexto para el usuario sin derechos administrativos mediante el comando [az aks get-credentials][az-aks-get-credentials].

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Después de ejecutar el comando `kubectl`, se le pedirá que se autentique con Azure. Siga las instrucciones que se muestran en la pantalla para finalizar el proceso, tal como puede ver en el ejemplo siguiente:

```console
$ kubectl get nodes

To sign in, use a web browser to open https://microsoft.com/devicelogin. Next, enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-1   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-2   Ready     agent     1h        v1.13.5
```

Cuando finalice el proceso, el token de autenticación se almacenará en caché. Solo se le solicitará que vuelva a iniciar sesión cuando el token expire o cuando se vuelva a crear el archivo de configuración de Kubernetes.

Si ve un mensaje de error de autorización después de iniciar sesión correctamente, compruebe los siguientes criterios:

```console
error: You must be logged in to the server (Unauthorized)
```


- Se ha definido el identificador o UPN del objeto adecuado, en función de si la cuenta de usuario está en el mismo inquilino de Azure AD o no.
- El usuario no es miembro de más de 200 grupos.
- El secreto definido en el registro de aplicación del servidor coincide con el valor configurado mediante `--aad-server-app-secret`.

## <a name="next-steps"></a>Pasos siguientes

Para usar los usuarios y grupos de Azure AD con el fin de controlar el acceso a los recursos de clúster, consulte [Controlar el acceso a recursos de clúster mediante el control de acceso basado en roles y las identidades de Azure AD en AKS][azure-ad-rbac].

Para obtener más información sobre cómo proteger los clústeres de Kubernetes, consulte [Opciones de acceso e identidad para AKS][rbac-authorization].

Para obtener más información sobre el control de recursos e identidades, consulte [Procedimientos recomendados para la autenticación y autorización en AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
