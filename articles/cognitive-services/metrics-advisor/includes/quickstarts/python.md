---
title: Inicio rápido de Python para Metrics Advisor
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 10/14/2020
ms.author: mbullwin
ms.openlocfilehash: cf4404bbfe7e8f0ad664c9ca8dda07ff61be12d9
ms.sourcegitcommit: 7a7b6c7ac0aa9dac678c3dfd4b5bcbc45dc030ca
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2020
ms.locfileid: "93186967"
---
[Documentación de referencia](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-metricsadvisor/latest/azure.ai.metricsadvisor.html) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/metricsadvisor/azure-ai-metricsadvisor/README.md) | [Paquete (PiPy)](https://pypi.org/project/azure-ai-metricsadvisor/) | [Ejemplos](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/metricsadvisor/azure-ai-metricsadvisor/samples/README.md)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)
* [Python 3.x](https://www.python.org/)
* Una vez que tenga la suscripción de Azure, <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title="cree un recurso de Metrics Advisor"  target="_blank">create a Metrics Advisor resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para implementar la instancia de Metrics Advisor.  
* Su propia base de datos SQL con datos de series temporales.
  
> [!TIP]
> * Puede encontrar ejemplos de Metrics Advisor de Python en [GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/samples).
> * El recurso de Metrics Advisor puede tardar entre 10 y 30 minutos en implementar una instancia de servicio para que pueda usarla. Haga clic en **Ir al recurso** una vez que se implemente correctamente. Después de la implementación, puede empezar a usar la instancia de Metrics Advisor con el portal web y la API de REST.
> * Puede encontrar la dirección URL de la API de REST en Azure Portal, en la sección **Información general** de su recurso. Tendrá este aspecto:
>    * `https://<instance-name>.cognitiveservices.azure.com/` 
 
## <a name="setting-up"></a>Instalación

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Después de instalar Python, puede instalar la biblioteca cliente con:

```console
pip install azure-ai-metricsadvisor --pre
```

### <a name="create-a-new-python-application"></a>Creación de una nueva aplicación de Python

Cree un archivo de Python e importe las bibliotecas siguientes.

```python
import os
import datetime
```

Cree variables para el punto de conexión y la clave de Azure del recurso.

> [!IMPORTANT]
> Vaya a Azure Portal. Si el recurso de Metrics Advisor que creó en la sección **Requisitos previos** se implementó correctamente, haga clic en el botón **Ir al recurso** en **Pasos siguientes**. Sus claves de suscripción y su punto de conexión se encuentran en la página **Key and Endpoint** (Clave y punto de conexión) del recurso, en **Administración de recursos**. <br><br>Para recuperar la clave de API, debe ir a [https://metricsadvisor.azurewebsites.net](https://metricsadvisor.azurewebsites.net). Seleccione los valores apropiados: **Directory** (Directorio), **Subscriptions** (Suscripciones) y **Workspace** (Área de trabajo) del recurso y seleccione **Comenzar**. A continuación, podrá recuperar las claves de API de [https://metricsadvisor.azurewebsites.net/api-key](https://metricsadvisor.azurewebsites.net/api-key).   
>
> Recuerde quitar la clave del código cuando haya terminado y no hacerla nunca pública. En el caso de producción, considere la posibilidad de usar alguna forma segura de almacenar las credenciales, y acceder a ellas. Para más información, consulte el artículo sobre la [seguridad](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) de Cognitive Services.

```python
subscription_key = "<paste-your-metrics-advisor-subscription-key-here>"
api_key = "<paste-your-metrics-advisor-api-key-here>"
service_endpoint = "<paste-your-metrics-advisor-endpoint-here>"
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases controlan algunas de las características principales del SDK de Python para Metrics Advisor.

|Nombre|Descripción|
|---|---|
| [MetricsAdvisorClient](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-metricsadvisor/latest/azure.ai.metricsadvisor.html#azure.ai.metricsadvisor.MetricsAdvisorClient) | **Se usa para:** <br> - Enumerar pendientes <br> - Enumerar la causa raíz de los incidentes <br> - Recuperar los datos de series temporales originales y los datos de series temporales enriquecidos por el servicio. <br> - Enumerar alertas <br> - Agregar comentarios para optimizar el modelo |
| [MetricsAdvisorAdministrationClient](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-metricsadvisor/latest/azure.ai.metricsadvisor.html?highlight=metricsadvisoradministrationclient#azure.ai.metricsadvisor.MetricsAdvisorAdministrationClient)| **Permite:** <br> - Administrar fuentes de distribución de datos <br> -Crear, configurar, recuperar, enumerar y eliminar configuraciones de detección de anomalías <br> -Crear, configurar, recuperar, enumerar y eliminar configuraciones de alertas de anomalías <br> - Administrar enlaces  | |
| [DataFeed](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-metricsadvisor/latest/azure.ai.metricsadvisor.models.html?highlight=datafeed#azure.ai.metricsadvisor.models.DataFeed)| **Lo que Metrics Advisor ingiere de su origen de datos. `DataFeed` contiene filas de:** <br> - Marcas de tiempo <br> - Cero o más dimensiones <br> - Una o varias medidas  |
| [Métrica](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-metricsadvisor/latest/azure.ai.metricsadvisor.models.html?highlight=metric#azure.ai.metricsadvisor.models.Metric) | `Metric` es una medida cuantificable que se usa para supervisar y evaluar el estado de un proceso empresarial específico. Puede ser una combinación de varios valores de serie temporal divididos en dimensiones. Por ejemplo, una métrica de mantenimiento de la Web podría contener dimensiones para recuento de usuarios y el mercado es-es. |

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes tareas con la biblioteca cliente de Metrics Advisor para Python:

* [Autenticar el cliente](#authenticate-the-client)
* [Agregar una fuente de distribución de datos](#add-a-data-feed)
* [Comprobar el estado de la ingesta](#check-the-ingestion-status)
* [Instalar la configuración de detección y la configuración de alertas](#configure-anomaly-detection)
* [Crear una configuración de alertas](#create-an-alert-configuration)
* [Resultados de la detección de anomalías de las consultas](#query-the-alert)

## <a name="authenticate-the-client"></a>Autenticar el cliente

El cliente de este ejemplo es un objeto `MetricsAdvisorAdministrationClient` que utiliza un punto de conexión y un objeto `MetricsAdvisorKeyCredential` que contiene las claves. No es necesario copiar este código de ejemplo. Los métodos que cree posteriormente crearán instancias del cliente. Se llama al cliente alternativo `MetricsAdvisorClient`. Se puede encontrar más información sobre este cliente en la [documentación de referencia](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-metricsadvisor/latest/azure.ai.metricsadvisor.html#azure.ai.metricsadvisor.MetricsAdvisorClient).

```python
client = MetricsAdvisorAdministrationClient(service_endpoint,
                                MetricsAdvisorKeyCredential(subscription_key, api_key))
```

## <a name="add-a-data-feed"></a>Adición de una fuente de distribución de datos

En un método nuevo, cree instrucciones de importación como las del ejemplo siguiente. Reemplace `sql_server_connection_string` por su propia cadena de conexión de SQL Server y reemplace `query` por una consulta que devuelva los datos en una sola marca de tiempo. También tendrá que ajustar los valores de `metric` y `dimension` en función de los datos personalizados.

> [!IMPORTANT]
> La consulta debe devolver como máximo un registro para cada combinación de dimensión, en cada marca de tiempo. Y todos los registros devueltos por la consulta deben tener las mismas marcas de tiempo. El asesor de métricas ejecutará esta consulta para cada marca de tiempo que deba ingerir los datos. Consulte [Preguntas frecuentes sobre las consultas](../../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data) para más información y ejemplos. 

Cree un cliente con sus claves y un punto de conexión, y use `client.create_data_feed()` para configurar el nombre, el origen, la granularidad y el esquema. También puede establecer el tiempo de ingesta, la configuración del monitor de acumulación, etc.


```python
def sample_create_data_feed():
    from azure.ai.metricsadvisor import MetricsAdvisorKeyCredential, MetricsAdvisorAdministrationClient
    from azure.ai.metricsadvisor.models import (
        SQLServerDataFeed,
        DataFeedSchema,
        Metric,
        Dimension,
        DataFeedOptions,
        DataFeedRollupSettings,
        DataFeedMissingDataPointFillSettings
    )
    sql_server_connection_string = "<replace-with-your-sql-server-connection-string>"
    query = "<replace-with-metrics-advisor-sql-server-query>"

    client = MetricsAdvisorAdministrationClient(service_endpoint,
                                  MetricsAdvisorKeyCredential(subscription_key, api_key))

    data_feed = client.create_data_feed(
        name="My data feed",
        source=SQLServerDataFeed(
            connection_string=sql_server_connection_string,
            query=query,
        ),
        granularity="Daily",
        schema=DataFeedSchema(
            metrics=[
                Metric(name="cost", display_name="Cost"),
                Metric(name="revenue", display_name="Revenue")
            ],
            dimensions=[
                Dimension(name="category", display_name="Category"),
                Dimension(name="city", display_name="City")
            ],
            timestamp_column="Timestamp"
        ),
        ingestion_settings=datetime.datetime(2019, 10, 1),
        options=DataFeedOptions(
            data_feed_description="cost/revenue data feed",
            rollup_settings=DataFeedRollupSettings(
                rollup_type="AutoRollup",
                rollup_method="Sum",
                rollup_identification_value="__CUSTOM_SUM__"
            ),
            missing_data_point_fill_settings=DataFeedMissingDataPointFillSettings(
                fill_type="SmartFilling"
            ),
            access_mode="Private"
        )
    )

    return data_feed
sample_create_data_feed()
```

## <a name="check-the-ingestion-status"></a>Comprobación del estado de la ingesta

En un método nuevo, cree una instrucción de importación como la del ejemplo siguiente. Reemplace `data_feed_id` por el identificador de la fuente de distribución de datos que ha creado. Cree un cliente con sus claves y su punto de conexión, y use `client.get_data_feed_ingestion_progress()` para obtener el progreso de la ingesta. Imprima los detalles, como las últimas marcas de tiempo activa y correcta.


```python
def sample_get_data_feed_ingestion_progress():
    from azure.ai.metricsadvisor import MetricsAdvisorKeyCredential, MetricsAdvisorAdministrationClient

    data_feed_id = "<replace-with-your-metrics-advisor-data-feed-id>"

    client = MetricsAdvisorAdministrationClient(service_endpoint,
                                  MetricsAdvisorKeyCredential(subscription_key, api_key))

    progress = client.get_data_feed_ingestion_progress(data_feed_id)

    print("Latest active timestamp: {}".format(progress.latest_active_timestamp))
    print("Latest successful timestamp: {}".format(progress.latest_success_timestamp))
sample_get_data_feed_ingestion_progress()
```

## <a name="configure-anomaly-detection"></a>Configuración de la detección de anomalías

En un método nuevo, cree instrucciones de importación como las del ejemplo siguiente. Reemplace `metric_id` por el identificador de la métrica que desea configurar. Cree un cliente con sus claves y su punto de conexión, y use `client.create_metric_anomaly_detection_configuration` para crear una configuración de detección. Las condiciones del umbral especifican los parámetros para la detección de anomalías.

```python
def sample_create_detection_config():
    from azure.ai.metricsadvisor import MetricsAdvisorKeyCredential, MetricsAdvisorAdministrationClient
    from azure.ai.metricsadvisor.models import (
        ChangeThresholdCondition,
        HardThresholdCondition,
        SmartDetectionCondition,
        SuppressCondition,
        MetricDetectionCondition,
    )
    metric_id = "replace-with-your-metric-id"

    client = MetricsAdvisorAdministrationClient(service_endpoint,
                                  MetricsAdvisorKeyCredential(subscription_key, api_key))

    change_threshold_condition = ChangeThresholdCondition(
        anomaly_detector_direction="Both",
        change_percentage=20,
        shift_point=10,
        within_range=True,
        suppress_condition=SuppressCondition(
            min_number=5,
            min_ratio=2
        )
    )
    hard_threshold_condition = HardThresholdCondition(
        anomaly_detector_direction="Up",
        upper_bound=100,
        suppress_condition=SuppressCondition(
            min_number=2,
            min_ratio=2
        )
    )
    smart_detection_condition = SmartDetectionCondition(
        anomaly_detector_direction="Up",
        sensitivity=10,
        suppress_condition=SuppressCondition(
            min_number=2,
            min_ratio=2
        )
    )

    detection_config = client.create_metric_anomaly_detection_configuration(
        name="my_detection_config",
        metric_id=metric_id,
        description="anomaly detection config for metric",
        whole_series_detection_condition=MetricDetectionCondition(
            cross_conditions_operator="OR",
            change_threshold_condition=change_threshold_condition,
            hard_threshold_condition=hard_threshold_condition,
            smart_detection_condition=smart_detection_condition
        )
    )

    return detection_config
sample_create_detection_config()
```


## <a name="create-a-hook"></a>Creación de un enlace

En un método nuevo, cree instrucciones de importación como las del ejemplo siguiente. Cree un cliente con sus claves y su punto de conexión, y use `client.create_hook()` para crear un enlace. Escriba una descripción, una lista de correos electrónicos a los que enviar la alerta y un vínculo externo para recibir la alerta.  

```python
def sample_create_hook():

    from azure.ai.metricsadvisor import MetricsAdvisorKeyCredential, MetricsAdvisorAdministrationClient
    from azure.ai.metricsadvisor.models import EmailHook

    client = MetricsAdvisorAdministrationClient(service_endpoint,
                                  MetricsAdvisorKeyCredential(subscription_key, api_key))

    hook = client.create_hook(
        name="email hook",
        hook=EmailHook(
            description="my email hook",
            emails_to_alert=["alertme@contoso.com"],
            external_link="https://adwiki.azurewebsites.net/articles/howto/alerts/create-hooks.html"
        )
    )

    return hook
sample_create_hook()
```

##  <a name="create-an-alert-configuration"></a>Creación de una configuración de alertas

En un método nuevo, cree instrucciones de importación como las del ejemplo siguiente. Reemplace `anomaly_detection_configuration_id` por el identificador de la configuración de detección de anomalías y reemplace `hook_id` por el enlace que creó anteriormente. Cree un cliente con sus claves y su punto de conexión, y use `client.create_anomaly_alert_configuration()` para crear una configuración de alertas. `metric_alert_configurations` es una lista de objetos `MetricAlertConfiguration` que especifican las condiciones y el ámbito de cada configuración.

```python
def sample_create_alert_config():
    from azure.ai.metricsadvisor import MetricsAdvisorKeyCredential, MetricsAdvisorAdministrationClient
    from azure.ai.metricsadvisor.models import (
        MetricAlertConfiguration,
        MetricAnomalyAlertScope,
        TopNGroupScope,
        MetricAnomalyAlertConditions,
        SeverityCondition,
        MetricBoundaryCondition,
        MetricAnomalyAlertSnoozeCondition
    )
    anomaly_detection_configuration_id = "<replace-with-your-detection-configuration-id"
    hook_id = "<replace-with-your-hook-id>"

    client = MetricsAdvisorAdministrationClient(service_endpoint,
                                  MetricsAdvisorKeyCredential(subscription_key, api_key))

    alert_config = client.create_anomaly_alert_configuration(
        name="my alert config",
        description="alert config description",
        cross_metrics_operator="AND",
        metric_alert_configurations=[
            MetricAlertConfiguration(
                detection_configuration_id=anomaly_detection_configuration_id,
                alert_scope=MetricAnomalyAlertScope(
                    scope_type="WholeSeries"
                ),
                alert_conditions=MetricAnomalyAlertConditions(
                    severity_condition=SeverityCondition(
                        min_alert_severity="Low",
                        max_alert_severity="High"
                    )
                )
            ),
            MetricAlertConfiguration(
                detection_configuration_id=anomaly_detection_configuration_id,
                alert_scope=MetricAnomalyAlertScope(
                    scope_type="TopN",
                    top_n_group_in_scope=TopNGroupScope(
                        top=10,
                        period=5,
                        min_top_count=5
                    )
                ),
                alert_conditions=MetricAnomalyAlertConditions(
                    metric_boundary_condition=MetricBoundaryCondition(
                        direction="Up",
                        upper=50
                    )
                ),
                alert_snooze_condition=MetricAnomalyAlertSnoozeCondition(
                    auto_snooze=2,
                    snooze_scope="Metric",
                    only_for_successive=True
                )
            ),
        ],
        hook_ids=[hook_id]
    )

    return alert_config
```

### <a name="query-the-alert"></a>Consulta de la alerta

En un método nuevo, cree una instrucción de importación como la del ejemplo siguiente. Reemplace `alert_id` por el identificador de la alerta y reemplace `alert_config_id` por el identificador de configuración de la alerta. Cree un cliente con sus claves y su punto de conexión, y use `client.list_anomalies_for_alert()` para enumerar una configuración de alertas. 

```python
def sample_list_anomalies_for_alert(alert_config_id, alert_id):

    from azure.ai.metricsadvisor import MetricsAdvisorKeyCredential, MetricsAdvisorClient
    
    alert_id = "<replace-with-your-alert-id>"
    alert_config_id = "<replace-with-your-alert-configuration-id"
    client = MetricsAdvisorClient(service_endpoint,
                                  MetricsAdvisorKeyCredential(subscription_key, api_key))

    results = client.list_anomalies_for_alert(
            alert_configuration_id=alert_config_id,
            alert_id=alert_id,
        )
    for result in results:
        print("Create on: {}".format(result.created_on))
        print("Severity: {}".format(result.severity))
        print("Status: {}".format(result.status))
```

### <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `python` en el archivo de inicio rápido.

```console
python quickstart-file.py
```