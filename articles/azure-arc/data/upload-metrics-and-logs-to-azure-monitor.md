---
title: Carga de datos de uso, métricas y registros en Azure Monitor
description: Carga del inventario de recursos, los datos de uso, las métricas y los registros en Azure Monitor
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 869bfcb87aa4846674db233c4268e9269929cd04
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92320162"
---
# <a name="upload-usage-data-metrics-and-logs-to-azure-monitor"></a>Carga de datos de uso, métricas y registros en Azure Monitor

Puede exportar periódicamente la información de uso con fines de facturación, supervisión de métricas y registros, y después cargarla en Azure.  La exportación y la carga de cualquiera de estos tres tipos de datos también creará y actualizará los recursos del controlador de datos, la instancia administrada de SQL y el grupo de servidores de Hiperescala de SQL en Azure.

> [!NOTE] 
> Durante el período de versión preliminar, los servicios de datos habilitados para Azure Arc no suponen ningún costo.

## <a name="prerequisites"></a>Requisitos previos

Necesitará tener instaladas la CLI de Azure y [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)].  [Instale las herramientas](./install-client-tools.md).

Antes de cargar los datos en Azure, debe asegurarse de que su suscripción de Azure tiene registrado el proveedor de recursos Microsoft.AzureData.

Puede comprobarlo mediante la ejecución del siguiente comando:

```console
az provider show -n Microsoft.AzureData -o table
```

Si el proveedor de recursos no está registrado actualmente en la suscripción, puede registrarlo mediante la ejecución del siguiente comando.  El comando puede tardar un minuto o dos en completarse.

```console
az provider register -n Microsoft.AzureData --wait
```

## <a name="upload-usage-data"></a>Carga de datos de uso

La información de uso, como el inventario y el uso de recursos, se puede cargar en Azure con el siguiente método de dos pasos:

1. Exporte los datos de uso mediante comando `azdata export`, como se indica a continuación:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #run the export command
   azdata arc dc export --type usage --path usage.json
   ```
   Este comando crea un archivo `usage.json` con todos los recursos de datos habilitados para Azure Arc, como las instancias administradas de SQL y las instancias de Hiperescala de PostgreSQL, etc., que se crean en el controlador de datos.

2. Cargue los datos de uso mediante el comando `azdata upload`.

   > [!NOTE]
   > Espere al menos 24 horas después de crear el controlador de datos de Azure Arc antes de ejecutar la carga.

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #run the upload command
   azdata arc dc upload --path usage.json
   ```

## <a name="upload-metrics-and-logs"></a>Carga de métricas y registros

Con los servicios de datos de Azure Arc, opcionalmente puede cargar las métricas y los registros en Azure Monitor a fin de agregar y analizar métricas, registros, generar alertas, enviar notificaciones o desencadenar acciones automatizadas. 

El envío de datos a Azure Monitor también permite almacenar datos de supervisión y registro fuera del sitio y a gran escala, lo que permite el almacenamiento a largo plazo de los datos para realizar análisis avanzados.

Si tiene varios sitios con servicios de datos de Azure Arc, puede usar Azure Monitor como ubicación central para recopilar todos los registros y las métricas de todos los sitios.

### <a name="before-you-begin"></a>Antes de empezar

Hay algunos pasos de instalación única que son necesarios para habilitar los escenarios de carga de registros y métricas:

1. Cree una entidad de servicio o una aplicación de Azure Active Directory, lo que incluye crear un secreto de acceso de cliente y asigne la entidad de servicio al rol "Publicador de métricas de supervisión" en las suscripciones donde se encuentran los recursos de la instancia de base de datos.
2. Cree un área de trabajo de Log Analytics, obtenga las claves y establezca la información en variables de entorno.

El primer elemento es necesario para cargar las métricas y el segundo para cargar los registros.

Siga estos comandos para crear la entidad de servicio de carga de métricas y asignarla a los roles "Publicador de métricas de supervisión" y "Colaborador" para que la entidad de servicio pueda cargar métricas y realizar operaciones de creación y carga.

## <a name="create-service-principal-and-assign-roles"></a>Creación de una entidad de servicio y asignación de roles

Siga estos comandos para crear la entidad de servicio de carga de métricas y asignarla al rol "Publicador de métricas de supervisión":

Para crear una entidad de servicio, ejecute este comando:

> [!NOTE]
> Para crear una entidad de servicio se necesitan [determinados permisos en Azure](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app).

```console
az ad sp create-for-rbac --name <a name you choose>

#Example:
#az ad sp create-for-rbac --name azure-arc-metrics
```

Salida de ejemplo:

```output
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

Guarde los valores de appId y tenant en una variable de entorno para su uso posterior. 

Para guardar los valores de appId y tenant con PowerShell, siga este ejemplo:

```powershell
$Env:SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'
```

Como alternativa, en Linux o macOS, puede guardar los valores de appId y tenant con este ejemplo:

   ```console
   export SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
   export SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
   export SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

   #Example (using Linux):
   export SPN_CLIENT_ID='2e72adbf-de57-4c25-b90d-2f73f126e123'
   export SPN_CLIENT_SECRET='5039d676-23f9-416c-9534-3bd6afc78123'
   export SPN_TENANT_ID='72f988bf-85f1-41af-91ab-2d7cd01ad1234'
   ```

Ejecute este comando para asignar la entidad de servicio al rol "Editor de métricas de supervisión" en la suscripción donde se encuentran los recursos de la instancia de base de datos:


> [!NOTE]
> Debe usar comillas dobles para los nombres de rol cuando se ejecuta desde un entorno de Windows.


```console
az role assignment create --assignee <appId value from output above> --role "Monitoring Metrics Publisher" --scope subscriptions/<sub ID>
az role assignment create --assignee <appId value from output above> --role 'Contributor' --scope subscriptions/<sub ID>

#Example:
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Monitoring Metrics Publisher" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Contributor' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123

#On Windows environment
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Monitoring Metrics Publisher" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Contributor" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
```

Salida de ejemplo:

```console
{
  "canDelegate": null,
  "id": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/<Subscription ID>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="create-a-log-analytics-workspace"></a>Creación de un área de trabajo de Log Analytics

A continuación, ejecute estos comandos para crear un área de trabajo de Log Analytics y establecer la información de acceso en variables de entorno.

> [!NOTE]
> Si ya tiene un área de trabajo, omita este paso.

```console
az monitor log-analytics workspace create --resource-group <resource group name> --workspace-name <some name you choose>

#Example:
#az monitor log-analytics workspace create --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
```

Salida de ejemplo:

```output
{
  "customerId": "d6abb435-2626-4df1-b887-445fe44a4123",
  "eTag": null,
  "id": "/subscriptions/<Subscription ID>/resourcegroups/user-arc-demo/providers/microsoft.operationalinsights/workspaces/user-logworkspace",
  "location": "eastus",
  "name": "user-logworkspace",
  "portalUrl": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "user-arc-demo",
  "retentionInDays": 30,
  "sku": {
    "lastSkuUpdate": "Thu, 30 Jul 2020 22:37:53 GMT",
    "maxCapacityReservationLevel": 3000,
    "name": "pergb2018"
  },
  "source": "Azure",
  "tags": null,
  "type": "Microsoft.OperationalInsights/workspaces"
}
```

## <a name="assign-id-and-shared-key-to-environment-variables"></a>Asignación del identificador y la clave compartida a variables de entorno

Guarde el valor customerId (el id. de área de trabajo) como una variable de entorno para usarla más adelante:

```console
#PowerShell
$Env:WORKSPACE_ID='<the customerId from the 'log-analytics workspace create' command output above>'

#Linux/macOS
export WORKSPACE_ID='<the customerId from the 'log-analytics workspace create' command output above>'

#Example (using Linux)
#export WORKSPACE_ID='d6abb435-2626-4df1-b887-445fe44a4123'
```

Este comando imprimirá las claves de acceso necesarias para conectarse al área de trabajo de Log Analytics:

```console
az monitor log-analytics workspace get-shared-keys --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
```

Salida de ejemplo:

```console
{
  "primarySharedKey": "JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123==",
  "secondarySharedKey": "p2XHSxLJ4o9IAqm2zINcEmx0UWU5Z5EZz8PQC0OHpFjdpuVaI0zsPbTv5VyPFgaCUlCZb2yEbkiR4eTuTSF123=="
}
```

Guarde la clave principal en una variable de entorno para usarla más adelante:

```console
#PowerShell:
$Env:WORKSPACE_SHARED_KEY='<the primarySharedKey value from the 'get-shared-keys' command above'

#Linux/macOS:
export WORKSPACE_SHARED_KEY='<the primarySharedKey value from the 'get-shared-keys' command above'

#Example (using Linux):
export WORKSPACE_SHARED_KEY='JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123=='

```

## <a name="set-final-environment-variables-and-confirm"></a>Establecimiento de las variables de entorno finales y confirmación

Establezca la dirección URL de la autoridad de SPN en una variable de entorno:

```console
#PowerShell
$Env:SPN_AUTHORITY='https://login.microsoftonline.com'

#Linux/macOS:
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

Si quiere, asegúrese de que se hayan establecido todas las variables de entorno necesarias:

```console
#PowerShell
$Env:WORKSPACE_ID
$Env:WORKSPACE_SHARED_KEY
$Env:SPN_TENANT_ID
$Env:SPN_CLIENT_ID
$Env:SPN_CLIENT_SECRET
$Env:SPN_AUTHORITY

#Linux/macOS
echo $WORKSPACE_ID
echo $WORKSPACE_SHARED_KEY
echo $SPN_TENANT_ID
echo $SPN_CLIENT_ID
echo $SPN_CLIENT_SECRET
echo $SPN_AUTHORITY
```

## <a name="upload-metrics-to-azure-monitor"></a>Carga de métricas a Azure Monitor

Para cargar las métricas de las instancias administradas de SQL habilitadas para Azure Arc y los grupos de servidores Hiperescala de PostgreSQL habilitada para Azure Arc, ejecute los siguientes comandos de la CLI:

1. Exporte todas las métricas al archivo especificado:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #export the metrics
   azdata arc dc export --type metrics --path metrics.json
   ```

2. Cargue las métricas en Azure Monitor:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #upload the metrics
   azdata arc dc upload --path metrics.json
   ```

   >[!NOTE]
   >Espere al menos 30 minutos después de que se creen las instancias de datos habilitadas para Azure Arc para la primera carga
   >
   >Asegúrese de `upload` las métricas inmediatamente después de `export` ya que Azure Monitor solo acepta métricas para los últimos 30 minutos. [Más información](../../azure-monitor/platform/metrics-store-custom-rest-api.md#troubleshooting)


Si ve un error que indica "Error al obtener métricas" durante la exportación, compruebe si la recopilación de datos está establecida en ```true```, ejecutando el comando siguiente:

```console
azdata arc dc config show
```

y consulte a "sección de seguridad".

```output
 "security": {
      "allowDumps": true,
      "allowNodeMetricsCollection": true,
      "allowPodMetricsCollection": true,
      "allowRunAsRoot": false
    },
```

Compruebe que las propiedades `allowNodeMetricsCollection` y `allowPodMetricsCollection` estén establecidas en `true`.

## <a name="view-the-metrics-in-the-portal"></a>Visualización de las métricas en el portal

Una vez que se carguen las métricas, puede verlas en Azure Portal.
> [!NOTE]
> Tenga en cuenta que los datos cargados pueden tardar un par de minutos en procesarse antes de que pueda ver las métricas en el portal.


Para ver las métricas en el portal, use este vínculo para abrirlo: <https://portal.azure.com> Después, busque la instancia de base de datos por nombre en la barra de búsqueda:

Puede ver el uso de la CPU en la página Información general o bien, si quiere métricas más detalladas, puede hacer clic en Métricas en el panel de navegación de la izquierda.

Elija SQL Server como el espacio de nombres de la métrica:

Seleccione la métrica que quiera visualizar (también puede seleccionar varias):

Cambie la frecuencia a los últimos 30 minutos:

> [!NOTE]
> Solo puede cargar las métricas de los últimos 30 minutos. Azure Monitor rechaza las métricas de más de 30 minutos.

## <a name="upload-logs-to-azure-monitor"></a>Carga de registros a Azure Monitor

 Para cargar los registros de las instancias administradas de SQL habilitadas para Azure Arc y los grupos de servidores Hiperescala de PostgreSQL habilitada para Azure Arc, ejecute los siguientes comandos de la CLI:

1. Se exportan todos los registros al archivo especificado:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #export the logs
   azdata arc dc export --type logs --path logs.json
   ```

2. Cargue los registros en un área de trabajo de Log Analytics en Azure Monitor:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #Upload the logs
   azdata arc dc upload --path logs.json
   ```

## <a name="view-your-logs-in-azure-portal"></a>Visualización de los registros en Azure Portal

Una vez que se hayan cargado los registros, debería poder consultarlos mediante el explorador de consultas de registros como se indica a continuación:

1. Abra Azure Portal y, después, busque el área de trabajo por su nombre en la barra de búsqueda de la parte superior y selecciónela.
2. Haga clic en Registros en el panel de la izquierda.
3. Haga clic en Comenzar (o bien en los vínculos de la página Comenzar para obtener más información sobre Log Analytics si no está familiarizado con él)
4. Siga el tutorial para obtener más información sobre Log Analytics si es la primera vez que utiliza Log Analytics.
5. Expanda Registros personalizados en la parte inferior de la lista de tablas y verá una tabla denominada "sql_instance_logs_CL".
6. Haga clic en el icono de ojo situado junto al nombre de la tabla.
7. Haga clic en el botón "Ver en el editor de consultas".
8. Ahora tendrá una consulta en el editor de consultas que mostrará los 10 eventos más recientes del registro.
9. Desde aquí, puede experimentar con la consulta de los registros mediante el editor de consultas, establecer alertas, etc.

## <a name="automating-uploads-optional"></a>Automatización de cargas (opcional)

Si quiere cargar métricas y registros de forma programada, puede crear un script y ejecutarlo en un temporizador cada pocos minutos. A continuación se muestra un ejemplo de cómo automatizar las cargas mediante un script de shell de Linux.

En el editor de código o texto que prefiera, agregue el siguiente script al archivo y guárdelo como un archivo ejecutable de script como .sh (Linux o Mac), o bien .cmd, .bat, .ps1.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Conversión del archivo de script en ejecutable

```console
chmod +x myuploadscript.sh
```

Ejecute el script cada 20 minutos:

```console
watch -n 1200 ./myuploadscript.sh
```

También puede usar un programador de trabajos como cron o el Programador de tareas de Windows, o bien un orquestador como Ansible, Puppet o Chef.

## <a name="general-guidance-on-exporting-and-uploading-usage-metrics"></a>Instrucciones generales sobre cómo exportar y cargar el uso, métricas

Las operaciones de creación, lectura, actualización y eliminación (CRUD) en los servicios de datos habilitados para Azure Arc se registran con fines de facturación y supervisión. Hay servicios en segundo plano que supervisan para estas operaciones CRUD y calculan el consumo adecuadamente. El cálculo real del uso o del consumo tiene lugar de forma programada y se realiza en segundo plano. 

Durante la versión preliminar, este proceso se produce por la noche. Las instrucciones generales es cargar el uso solo una vez al día. Cuando la información de uso se exporta y se carga varias veces dentro del mismo período de 24 horas, solo se actualiza el inventario de recursos en Azure Portal, pero no en el uso de recursos.

En el caso de la carga de métricas, Azure Monitor solo acepta los últimos 30 minutos de datos ([Más información](../../azure-monitor/platform/metrics-store-custom-rest-api.md#troubleshooting)). Las instrucciones para cargar métricas es cargar las métricas inmediatamente después de crear el archivo de exportación para que pueda ver todo el conjunto de datos en Azure Portal. Por ejemplo, si exportó las métricas a las 14:00 horas y ejecutó el comando de carga a las 14:50. Como Azure Monitor solo acepta datos de los últimos 30 minutos, es posible que no vea ningún dato en el portal. 

## <a name="next-steps"></a>Pasos siguientes

[Carga de datos de facturación en Azure y visualización en Azure Portal](view-billing-data-in-azure.md)

[Visualización del recurso de controlador de datos de Azure Arc en Azure Portal](view-data-controller-in-azure-portal.md)