---
title: 'Integración de Time Series Insights con Supervisión remota: Azure | Microsoft Docs'
description: En este tema de procedimientos obtendrá información sobre cómo configurar Time Series Insights para una solución de Supervisión remota existente que aún no incluya Time Series Insights.
author: Philmea
manager: timlt
ms.author: philmea
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 752529454a5b6293d9cbfdf8378b46947aed5a0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77564651"
---
# <a name="integrate-azure-time-series-insights-with-remote-monitoring"></a>Integración de Azure Time Series Insights con Supervisión remota

Azure Time Series Insights es un servicio de análisis, almacenamiento y visualización totalmente administrado que permite administrar los datos de serie temporal a escala de IoT en la nube. Puede usar Time Series Insights para almacenar y administrar datos de serie temporal, explorar y visualizar eventos al mismo tiempo, realizar análisis de la causa principal y comparar varios sitios y recursos.

El acelerador de soluciones de supervisión remota proporciona ahora la implementación e integración automáticas con Time Series Insights. En este tema de procedimientos obtendrá información sobre cómo configurar Time Series Insights para una solución de Supervisión remota existente que aún no incluya Time Series Insights.

> [!NOTE]
> Time Series Insights actualmente no está disponible en la nube de Azure China. Las nuevas implementaciones del acelerador de soluciones de supervisión remota en la nube de Azure China usan Cosmos DB para todo el almacenamiento.

## <a name="prerequisites"></a>Prerequisites

Para completar este procedimiento, es preciso ya haber implementado una solución de Supervisión remota:

* [Implementación del acelerador de soluciones de supervisión remota](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Creación de un grupo de consumidores

Cree un grupo de consumidores dedicado en IoT Hub para usarlo en el streaming de datos a Time Series Insights.

> [!NOTE]
> Las aplicaciones usan grupos de consumidores para extraer datos de Azure IoT Hub. Cada grupo de consumidores permite hasta cinco consumidores de salida. Debe crear un grupo de consumidores para cada cinco receptores de salida y puede crear hasta treinta y dos grupos de consumidores.

1. En Azure Portal, haga clic en el botón Cloud Shell.

1. Ejecute el siguiente comando para crear un grupo de consumidores. Use el nombre de la instancia de IoT Hub en la implementación de Supervisión remota, y el nombre de la implementación de supervisión remota como nombre del grupo de recursos:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="deploy-time-series-insights"></a>Implementación de Time Series Insights

A continuación, implemente Time Series Insights como recurso adicional en la solución de Supervisión remota y conéctelo a la instancia de IoT Hub.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione **Crear un recurso** > **Internet de las cosas** > **Time Series Insights**.

    ![Nuevo entorno de Time Series Insights](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights.png)

1. Para crear el entorno de Time Series Insights, use los valores de la tabla siguiente:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre del entorno | En la captura de pantalla siguiente se usa el nombre **contorosrmtsi**. Elija un nombre exclusivo de su elección al completar este paso. |
    | Subscription | Seleccione la suscripción de Azure en el menú desplegable. |
    | Resource group | **Usar existente**. Seleccione el nombre de su grupo de recursos de Supervisión remota existente. |
    | Location | Se usa **Este de EE. UU**. Cree el entorno en la misma región que la solución de Supervisión remota, si es posible. |
    | SKU |**S1** |
    | Capacity | **1** |

    ![Creación del entorno de Time Series Insights](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights-create.png)

1. Haga clic en **Crear**. El entorno puede tardar unos instantes en crearse.

## <a name="create-event-source"></a>Creación de un origen de eventos

Cree un origen del evento para conectarlo a IoT Hub. Asegúrese de utilizar el grupo de consumidores creado en los pasos anteriores. Time Series Insights requiere que cada servicio tenga un grupo de consumidores dedicado que no esté en uso por otro servicio.

1. Vaya al nuevo entorno de Time Series Insights.

1. En el lado izquierdo, seleccione **Orígenes de eventos**.

    ![Visualización de orígenes de eventos](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources.png)

1. Haga clic en **Agregar**.

    ![Adición de orígenes de eventos](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources-add.png)

1. Para configurar el centro de IoT como un nuevo origen del evento, use los valores de la tabla siguiente:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre de origen de eventos | En la captura de pantalla siguiente se usa el nombre **contosorm-iot-hub**. Use un nombre exclusivo de su elección al completar este paso. |
    | Source | **IoT Hub** |
    | Opción de importación | **Usar IoT Hub desde suscripciones disponibles** |
    | Id. de suscripción | Seleccione la suscripción de Azure en el menú desplegable. |
    | Nombre de IoT Hub | **contosorma57a6**. Use el nombre del centro de IoT de la solución de supervisión remota. |
    | Nombre de directiva de IoT Hub | **iothubowner** Asegúrese de que la directiva utilizada es una directiva de propietario. |
    | Clave de la directiva de IoT Hub | Este campo se rellena automáticamente. |
    | Grupo de consumidores de IoT Hub | **timeseriesinsights** |
    | Formato de serialización de eventos | **JSON**     | 
    | Nombre de la propiedad de marca de tiempo | Déjelo en blanco |

    ![Creación de un origen del evento](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-source-create.png)

1. Haga clic en **Crear**.

## <a name="configure-the-data-access-policy"></a>Configuración de la directiva de acceso de datos

Para asegurarse de que todos los usuarios que tienen acceso a la solución de Supervisión remota puedan explorar los datos en el explorador de Time Series Insights, agregue la aplicación y los usuarios en las directivas de acceso de datos en Azure Portal. 

1. En la lista de navegación, elija **Grupos de recursos**.

1. Elija el grupo de recursos **ContosoRM**.

1. Elija **contosormtsi** en la lista de recursos de Azure.

1. Haga clic en **Directivas de acceso a datos** para ver la lista actual de asignaciones de roles.

1. Elija **Agregar** para abrir el panel **Select User Rule** (Seleccionar regla de usuario) panel.

   Si no tiene permisos para asignar roles, no verá la opción **Agregar**.

1. En la lista desplegable **Rol**, seleccione un rol como **Lector** y **Colaborador**.

1. En la lista **Seleccionar**, seleccione un usuario, grupo o aplicación. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Seleccione **Guardar** para crear la asignación de roles. Transcurridos unos instantes, a la entidad de seguridad se le asigna el rol en las directivas de acceso a datos.

> [!NOTE]
> Si necesita conceder a otros usuarios acceso al explorador de Time Series Insights, puede utilizar estos pasos para [conceder acceso a datos](../time-series-insights/time-series-insights-data-access.md#grant-data-access).

## <a name="configure-azure-stream-analytics"></a>Configuración de Azure Stream Analytics 

El siguiente paso es configurar el microservicio del Administrador de Azure Stream Analytics para dejar de enviar mensajes a Cosmos DB y almacenarlos solo en Time Series Insights. Omita este paso si desea duplicar los mensajes en Cosmos DB.

1. En la lista de navegación, elija **Grupos de recursos**.

1. Elija el grupo de recursos **ContosoRM**.

1. Busque el trabajo de streaming de Azure Stream Analytics (ASA) en la lista de recursos. El nombre del recurso comienza con **streamingjobs-** .

1. En la parte superior, haga clic en el botón para detener los trabajos de streaming de ASA.

1. Edite la consulta de ASA y quite las cláusulas **SELECT**, **INTO** y **FROM** que apuntan al streaming de mensajes en Cosmos DB. Estas cláusulas deben estar en la parte inferior de la consulta y tienen un aspecto similar al ejemplo siguiente:

    ```sql
    SELECT
            CONCAT(T.IoTHub.ConnectionDeviceId, ';', CAST(DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) AS nvarchar(max))) as id,
            1 as [doc.schemaVersion],
            'd2cmessage' as [doc.schema],
            T.IoTHub.ConnectionDeviceId as [device.id],
            'device-sensors;v1' as [device.msg.schema],
            'StreamingJobs' as [data.schema],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', System.Timestamp) as [device.msg.created],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) as [device.msg.received],
            udf.removeUnusedProperties(T) as Data
    INTO
        Messages
    FROM
        DeviceTelemetry T PARTITION BY PartitionId TIMESTAMP BY T.EventEnqueuedUtcTime
    ```

6. Reinicie los trabajo de streaming de Azure Stream Analytics.

7. Para extraer los cambios más recientes del microservicio del Administrador de Azure Stream Analytics, escriba el siguiente comando en el símbolo del sistema:

.NET: 

```cmd/sh
docker pull azureiotpcs/asa-manager-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/asa-manager-java:1.0.2
```

## <a name="configure-the-telemetry-microservice"></a>Configuración del microservicio de telemetría

Para extraer el microservicio de telemetría más reciente, escriba el siguiente comando en el símbolo del sistema:

.NET:

```cmd/sh
docker pull azureiotpcs/telemetry-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/telemetry-java:1.0.2
```

## <a name="optional-configure-the-web-ui-to-link-to-the-time-series-insights-explorer"></a>*[Opcional]*  Configuración de la interfaz de usuario web para vincularse con el Explorador de Time Series Insights

Para ver fácilmente los datos en el Explorador de Time Series Insights, se recomienda personalizar la interfaz de usuario para vincularse con facilidad al entorno. Para ello, extraiga los últimos cambios en la interfaz de usuario web con el comando siguiente:

```cmd/sh
docker pull azureiotpcs/pcs-remote-monitoring-webui:1.0.2
```

## <a name="configure-the-environment-variables"></a>Configuración de las variables de entorno

Para completar la integración de Time Series Insights, deberá configurar el entorno de la implementación para los microservicios actualizados.

### <a name="basic-deployments"></a>Implementaciones básicas

Configure el entorno de la implementación `basic` para los microservicios actualizados.

1. En Azure Portal, haga clic en el icono de **Azure Active Directory** en el panel de navegación izquierdo.

1. Haga clic en **Registros de aplicaciones**.

1. Busque su aplicación **ContosoRM** y haga clic en ella.

1. Vaya a **configuración** > **Claves** y, luego, cree una nueva clave para la aplicación. No olvide copiar el valor de clave en una ubicación segura.

1. Extraiga el [archivo yaml de Docker Compose más reciente](https://github.com/Azure/pcs-cli/tree/5a9b4e0dbe313172eff19236e54a4d461d4f3e51/solutions/remotemonitoring/single-vm) del repositorio de GitHub con la etiqueta "latest". 

1. Use SSH en la VM siguiendo los pasos descritos en [Uso de claves SSH con Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

1. Una vez conectado, escriba `cd /app`.

1. Agregue las siguientes variables de entorno para cada microservicio en el archivo yaml de Docker Compose y el script `env-setup` en la VM:

    ```sh
    PCS_TELEMETRY_STORAGE_TYPE=tsi
    PCS_TSI_FQDN={TSI Data Access FQDN}
    PCS_AAD_TENANT={AAD Tenant Id}
    PCS_AAD_APPID={AAD application Id}
    PCS_AAD_APPSECRET={AAD application key}
    ```

1. Navegue hasta el **servicio de telemetría** y también edite el archivo de Docker Compose mediante la adición de las mismas variables de entorno anteriores.

1. Navegue hasta el **servicio de administrador de ASA** y edite el archivo de Docker Compose al agregando `PCS_TELEMETRY_STORAGE_TYPE`.

1. Reinicie los contenedores de Docker con `sudo ./start.sh` desde la VM.

> [!NOTE]
> La configuración de variables de entorno de arriba es válida para las versiones de Supervisión remota anteriores a la 1.0.2

### <a name="standard-deployments"></a>Implementaciones estándar

Configure el entorno de la implementación `standard`para los microservicios actualizados anteriores.

1. En la línea de comandos, ejecute `kubectl proxy`. Para más información, consulte el [acceso a la API de Kubernetes](https://kubernetes.io/docs/tasks/access-kubernetes-api/http-proxy-access-api/#using-kubectl-to-start-a-proxy-server).

1. Abra la consola de administración de Kubernetes.

1. Busque la asignación de configuración para agregar las siguientes nuevas variables de entorno para TSI:

    ```yaml
    telemetry.storage.type: "tsi"
    telemetry.tsi.fqdn: "{TSI Data Access FQDN}"
    security.auth.serviceprincipal.secret: "{AAD application service principal secret}"
    ```

4. Edite el archivo de plantilla yaml para el pod del servicio de telemetría:

    ```yaml
    - name: PCS_AAD_TENANT
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.tenant
    - name: PCS_AAD_APPID
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.audience
    - name: PCS_AAD_APPSECRET
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.serviceprincipal.secret
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    - name: PCS_TSI_FQDN
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.tsi.fqdn
    ```

5. Edite el archivo de plantilla yaml para el pod del servicio del Administrador de ASA:

    ```yaml
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    ```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo explorar los datos y diagnosticar una alerta en el explorador de Time Series Insights, consulte nuestro tutorial sobre cómo [llevar a cabo un análisis de causa principal](iot-accelerators-remote-monitoring-root-cause-analysis.md).

* Para obtener información sobre cómo explorar y consultar datos en el explorador de Time Series Insights, consulte la documentación sobre el [Explorador de Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
