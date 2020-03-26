---
title: 'Tutorial: Creación de un clúster de Red Hat OpenShift en Azure'
description: Aprenda a crear un clúster de Microsoft Azure Red Hat OpenShift con la CLI de Azure
author: jimzim
ms.author: jzim
ms.topic: tutorial
ms.service: container-service
ms.date: 11/04/2019
ms.openlocfilehash: 58fc695707995aafe4d804ffab8beee7c52b4320
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79455305"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Tutorial: Creación de un clúster de Red Hat OpenShift en Azure

Este tutorial es la primera parte de una serie. Aprenderá a crear un clúster de Microsoft Azure Red Hat OpenShift con la CLI de Azure, escalarlo y eliminarlo para limpiar los recursos.

En la primera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Creación de un clúster de Red Hat OpenShift en Azure

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * Creación de un clúster de Red Hat OpenShift en Azure
> * [Escalado de un clúster de Red Hat OpenShift en Azure](tutorial-scale-cluster.md)
> * [Eliminación de un clúster de Red Hat OpenShift en Azure](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Prerrequisitos

> [!IMPORTANT]
> En este tutorial se requiere la versión 2.0.65 de la CLI de Azure.

Antes de empezar este tutorial:

Asegúrese de haber [configurado el entorno de desarrollo](howto-setup-environment.md), lo que incluye lo siguiente:
- Instalar la CLI más reciente (versión 2.0.65 o superior)
- Crear un inquilino si aún no tiene uno
- Crear un objeto de aplicación de Azure si aún no tiene uno
- Creación de un grupo de seguridad
- Crear un usuario de Active Directory para iniciar sesión en el clúster

## <a name="step-1-sign-in-to-azure"></a>Paso 1: Inicio de sesión en Azure

Si ejecuta la CLI de Azure de manera local, abra un shell de comandos de Bash y ejecute `az login` para iniciar sesión en Azure.

```azurecli
az login
```

 Si tiene acceso a varias suscripciones, ejecute `az account set -s {subscription ID}` y reemplace `{subscription ID}` por la suscripción que quiere usar.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>Paso 2: Creación de un clúster de Red Hat OpenShift en Azure

En una ventana de comandos de Bash, establezca las siguientes variables:

> [!IMPORTANT]
> Elija un nombre para el clúster; este nombre debe ser único y estar todo en minúsculas o la creación del clúster dará error.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

Elija una ubicación para crear el clúster. Para obtener una lista de regiones de Azure que admitan OpenShift en Azure, consulte [Regiones admitidas](supported-resources.md#azure-regions). Por ejemplo: `LOCATION=eastus`.

```bash
LOCATION=<location>
```

Establezca `APPID` en el valor que guardó en el paso 5 de [Crear un registro de aplicación de Azure AD](howto-aad-app-configuration.md#create-an-azure-ad-app-registration).

```bash
APPID=<app ID value>
```

Establezca "GROUPID" en el valor que guardó en el paso 10 de [Crear un grupo de seguridad de Azure AD](howto-aad-app-configuration.md#create-an-azure-ad-security-group).

```bash
GROUPID=<group ID value>
```

Establezca `SECRET` en el valor que guardó en el paso 8 de [Creación de un secreto de cliente](howto-aad-app-configuration.md#create-a-client-secret).

```bash
SECRET=<secret value>
```

Establezca `TENANT` en el valor de identificador de inquilino que guardó en el paso 7 de [Creación de un nuevo inquilino](howto-create-tenant.md#create-a-new-azure-ad-tenant).

```bash
TENANT=<tenant ID>
```

Cree el grupo de recursos para el clúster. Ejecute el siguiente comando desde el mismo shell de Bash que usó para definir las variables anteriores:

```azurecli
az group create --name $CLUSTER_NAME --location $LOCATION
```

### <a name="optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network"></a>Opcional: Conexión de la red virtual del clúster a una red virtual existente

Si no necesita conectar la red virtual (VNET) del clúster que crea a una red virtual existente mediante el emparejamiento, omita este paso.

Si el emparejamiento a una red fuera de la suscripción predeterminada, en esa suscripción también deberá registrar el proveedor Microsoft.ContainerService. Para ello, ejecute el comando siguiente en esa suscripción. En caso contrario, si la red virtual que está emparejando se encuentra en la misma suscripción, puede omitir el paso del registro.

`az provider register -n Microsoft.ContainerService --wait`

En primer lugar, obtenga el identificador de la red virtual existente. El identificador tendrá el siguiente formato: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Si no conoce el nombre de red o el grupo de recursos al que pertenece la red virtual existente, vaya a la [hoja Redes virtuales](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) y haga clic en la red virtual. Se mostrará la página Red virtual, con el nombre de la red y el grupo de recursos al que pertenece.

Defina una variable VNET_ID con el siguiente comando de la CLI en un shell de BASH:

```azurecli
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Por ejemplo: `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="optional-connect-the-cluster-to-azure-monitoring"></a>Opcional: Conexión del clúster a Azure Monitor

Primero, obtenga el identificador del área de trabajo de Log Analytics **existente**. El identificador tendrá el siguiente formato:

`/subscriptions/{subscription}/resourceGroups/{resourcegroup}/providers/Microsoft.OperationalInsights/workspaces/{workspace-id}`.

Si no conoce el nombre del área de trabajo de Log Analytics o el grupo de recursos al que pertenece el área de trabajo existente, vaya a [Área de trabajo de Log Analytics](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces) y haga clic en las áreas de trabajo de Log Analytics. Aparece la página del área de trabajo de Log Analytics con el nombre del área de trabajo y el grupo de recursos al que pertenece.

_Para crear un área de trabajo de Log Analytics, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace-cli.md)_ .

Defina una variable WORKSPACE_ID mediante el siguiente comando de la CLI en un shell de BASH:

```azurecli
WORKSPACE_ID=$(az monitor log-analytics workspace show -g {RESOURCE_GROUP} -n {NAME} --query id -o tsv)
```

### <a name="create-the-cluster"></a>Creación del clúster

Ya está listo para crear un clúster. A continuación se creará el clúster en el inquilino de Azure AD especificado y se determinará el objeto de aplicación de Azure AD y el secreto para usar como entidad de seguridad; también se definirá el grupo de seguridad que contiene los miembros con acceso administrativo al clúster.

> [!IMPORTANT]
> Asegúrese de que ha agregado correctamente los permisos adecuados para la aplicación de Azure AD como [se detalla aquí](howto-aad-app-configuration.md#add-api-permissions) antes de crear el clúster

Si **no** va a emparejar el clúster a una red virtual o **no** quiere usar Azure Monitor, utilice el siguiente comando:

```azurecli
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID
```

En caso de que **sí** vaya a emparejar el clúster a una red virtual, use el comando siguiente que agrega la marca `--vnet-peer`:

```azurecli
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID --vnet-peer $VNET_ID
```

Si **quiere** usar Azure Monitor con el clúster, utilice el siguiente comando, que agrega la marca `--workspace-id`:

```azurecli
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID --workspace-id $WORKSPACE_ID
```

> [!NOTE]
> Si obtiene un error indicando que el nombre de host no está disponible, es posible que el nombre del clúster no sea único. Pruebe a eliminar el registro de aplicación original y repita los pasos descritos en [Creación de un nuevo registro de aplicación](howto-aad-app-configuration.md#create-an-azure-ad-app-registration) pero con un nombre de clúster diferente y sin realizar el paso de creación de un usuario y un grupo de seguridad.




Después de unos minutos, finaliza `az openshift create`.

### <a name="get-the-sign-in-url-for-your-cluster"></a>Obtención de la dirección URL de inicio de sesión del clúster

Obtenga la dirección URL para iniciar sesión en el clúster mediante la ejecución del siguiente comando:

```azurecli
az openshift show -n $CLUSTER_NAME -g $CLUSTER_NAME
```

Busque `publicHostName` en la salida, por ejemplo: `"publicHostname": "openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io"`.

La dirección URL de inicio de sesión del clúster será `https://` seguida del valor `publicHostName`.  Por ejemplo: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`.  Este URI se usará en el paso siguiente como parte del URI de redirección del registro de la aplicación.

## <a name="step-3-update-your-app-registration-redirect-uri"></a>Paso 3: Actualización del URI de redirección del registro de la aplicación

Ahora que tiene la dirección URL de inicio de sesión del clúster, establezca el URI de redirección del registro de la aplicación:

1. Abra la hoja [Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview).
2. Haga clic en el objeto de registro de la aplicación.
3. Haga clic en **Agregar un URI de redirección**.
4. Asegúrese de que **TIPO** sea **Web** y establezca el valor de **URI DE REDIRECCIÓN** con el siguiente patrón: `https://<public host name>/oauth2callback/Azure%20AD`. Por ejemplo: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io/oauth2callback/Azure%20AD`
5. Haga clic en **Guardar**

## <a name="step-4-sign-in-to-the-openshift-console"></a>Paso 4: Inicio de sesión en la consola de OpenShift

Ahora está listo para iniciar sesión en la consola de OpenShift para el nuevo clúster. La [consola web de OpenShift](https://docs.openshift.com/aro/architecture/infrastructure_components/web_console.html) le permite visualizar, explorar y administrar el contenido de los proyectos de OpenShift.

Necesitará una nueva instancia del explorador que no haya almacenado en caché la identidad que normalmente usa para iniciar sesión en Azure Portal.

1. Abra una ventana de *incógnito* (Chrome) o *InPrivate* (Microsoft Edge).
2. Vaya a la URL de inicio de sesión que obtuvo anteriormente, por ejemplo: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`.

Inicie sesión con el nombre de usuario que creó en el paso 3 de [Creación de un usuario de Azure Active Directory nuevo](howto-aad-app-configuration.md#create-a-new-azure-active-directory-user).

Aparece el cuadro de diálogo **Permisos solicitados**. Haga clic en **Consentimiento en nombre de la organización** y, luego, haga clic en **Aceptar**.

Al hacerlo, iniciará sesión en la consola del clúster.

![Captura de pantalla de la consola del clúster de OpenShift](./media/aro-console.png)

 Obtenga más información acerca del [uso de la consola de OpenShift](https://docs.openshift.com/aro/getting_started/developers_console.html) para crear imágenes en la documentación de [Red Hat OpenShift](https://docs.openshift.com/aro/welcome/index.html).

## <a name="step-5-install-the-openshift-cli"></a>Paso 5: Instalación de la CLI de OpenShift

La [CLI de OpenShift](https://docs.openshift.com/aro/cli_reference/get_started_cli.html) (u *OC Tools*) proporciona comandos para administrar las aplicaciones y utilidades de nivel inferior para interactuar con los distintos componentes del clúster de OpenShift.

En la consola de OpenShift, haga clic en el signo de interrogación de la esquina superior derecha junto a su nombre de inicio de sesión y seleccione **Command Line Tools** (Herramientas de línea de comandos).  Siga el vínculo **Latest Release** (Versión más reciente) para descargar e instalar la CLI de OC compatible con Linux, macOS o Windows.

> [!NOTE]
> Si no ve el icono de signo de interrogación en la esquina superior derecha, seleccione *Service Catalog* (Catálogo de servicios) o *Consola de aplicación* (Application Console) en la lista desplegable de la parte superior izquierda.
>
> Como alternativa, puede [descargar la CLI de OC](https://www.okd.io/download.html) directamente.

La página **Command Line Tools** (Herramientas de línea de comandos) proporciona un comando con el formato `oc login https://<your cluster name>.<azure region>.cloudapp.azure.com --token=<token value>`.  Haga clic en el botón *Copy to clipboard* (Copiar al Portapapeles) para copiar este comando.  En una ventana de terminal, [establezca la ruta de acceso](https://docs.okd.io/latest/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli) para incluir la instalación local de las herramientas de OC. A continuación, inicie sesión en el clúster mediante el comando de la CLI de OC que copió.

Si no se pudo obtener el valor del token siguiendo los pasos anteriores, obténgalo de: `https://<your cluster name>.<azure region>.cloudapp.azure.com/oauth/token/request`.

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Creación de un clúster de Red Hat OpenShift en Azure

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Escalado de un clúster de Red Hat OpenShift en Azure](tutorial-scale-cluster.md)
