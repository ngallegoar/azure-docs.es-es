---
title: Inicio rápido de JavaScript de Metrics Advisor
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 10/14/2020
ms.author: mbullwin
ms.openlocfilehash: a4426e9ca40b21c79e5e34f782be4ff2d07c7061
ms.sourcegitcommit: 7a7b6c7ac0aa9dac678c3dfd4b5bcbc45dc030ca
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2020
ms.locfileid: "93186972"
---
[Documentación de referencia](https://docs.microsoft.com/javascript/api/overview/azure/ai-metrics-advisor-readme-pre?view=azure-node-preview&preserve-view=true) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/metricsadvisor/ai-metrics-advisor/README.md) | [Paquete (npm)](https://www.npmjs.com/package/@azure/ai-metrics-advisor) | [Ejemplos](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/metricsadvisor/ai-metrics-advisor/samples)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)
* La versión actual de [Node.js](https://nodejs.org/)
* Una vez que tenga la suscripción de Azure, <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title="cree un recurso de Metrics Advisor"  target="_blank">create a Metrics Advisor resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para implementar la instancia de Metrics Advisor.  
* Su propia base de datos SQL con datos de series temporales.
  
> [!TIP]
> * Puede encontrar ejemplos de Metrics Advisor de JavaScript en [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/metricsadvisor/ai-metrics-advisor/samples).
> * El recurso de Metrics Advisor puede tardar entre 10 y 30 minutos en implementar una instancia de servicio para que pueda usarla. Haga clic en **Ir al recurso** una vez que se implemente correctamente. Después de la implementación, puede empezar a usar la instancia de Metrics Advisor con el portal web y la API de REST. 
> * Puede encontrar la dirección URL de la API de REST en Azure Portal, en la sección **Información general** de su recurso. Tendrá este aspecto:
>    * `https://<instance-name>.cognitiveservices.azure.com/`
    
## <a name="setting-up"></a>Instalación

### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `npm init` para crear una aplicación de nodo con un archivo `package.json`. 

```console
npm init
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Instale el paquete NPM `@azure/ai-metrics-advisor`:

```console
npm install @azure/ai-metrics-advisor@1.0.0-beta.1
```

el archivo `package.json` de la aplicación se actualizará con las dependencias.

Cree un archivo llamado `index.js` e importe las bibliotecas siguientes:

> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo en [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/metricsadvisor/ai-metrics-advisor/samples/javascript), que contiene los ejemplos de código de este inicio rápido.

Cree variables para el punto de conexión y la clave de Azure del recurso. 

> [!IMPORTANT]
> Vaya a Azure Portal. Si el recurso de Metrics Advisor que creó en la sección **Requisitos previos** se implementó correctamente, haga clic en el botón **Ir al recurso** en **Pasos siguientes**. Sus claves de suscripción y su punto de conexión se encuentran en la página **Key and Endpoint** (Clave y punto de conexión) del recurso, en **Administración de recursos**. <br><br>Para recuperar la clave de API, debe ir a [https://metricsadvisor.azurewebsites.net](https://metricsadvisor.azurewebsites.net). Seleccione los valores apropiados: **Directory** (Directorio), **Subscriptions** (Suscripciones) y **Workspace** (Área de trabajo) del recurso y seleccione **Comenzar**. A continuación, podrá recuperar las claves de API de [https://metricsadvisor.azurewebsites.net/api-key](https://metricsadvisor.azurewebsites.net/api-key).   
>
> Recuerde quitar la clave del código cuando haya terminado y no hacerla nunca pública. En el caso de producción, considere la posibilidad de usar alguna forma segura de almacenar las credenciales, y acceder a ellas. Para más información, consulte el artículo sobre la [seguridad](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) de Cognitive Services.

```javascript
subscriptionKey = "<paste-your-metrics-advisor-key-here>";
apiKey ="<paste-your-metrics-advisor-api-key-here>";
endpoint = "<paste-your-metrics-advisor-endpoint-here>";
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales del SDK de JavaScript para Metrics Advisor.

|Nombre|Descripción|
|---|---|
| [MetricsAdvisorClient](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-ai-metrics-advisor/1.0.0-beta.1/classes/metricsadvisorclient.html) | **Se usa para:** <br> - Enumerar pendientes <br> - Enumerar la causa raíz de los incidentes <br> - Recuperar los datos de series temporales originales y los datos de series temporales enriquecidos por el servicio. <br> - Enumerar alertas <br> - Agregar comentarios para optimizar el modelo |
| [MetricsAdvisorAdministrationClient](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-ai-metrics-advisor/1.0.0-beta.1/classes/metricsadvisoradministrationclient.html)| **Permite:** <br> - Administrar fuentes de distribución de datos <br> -Crear, configurar, recuperar, enumerar y eliminar configuraciones de alertas de anomalías <br> - Administrar enlaces  |
| [DataFeed](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-ai-metrics-advisor/1.0.0-beta.1/interfaces/datafeed.html)| **Lo que Metrics Advisor ingiere de su origen de datos. `DataFeed` contiene filas de:** <br> - Marcas de tiempo <br> - Cero o más dimensiones <br> - Una o varias medidas  |
| [Métrica](https://azuresdkdocs.blob.core.windows.net/$web/javascript/azure-ai-metrics-advisor/1.0.0-beta.1/interfaces/metric.html) | `Metric` es una medida cuantificable que se usa para supervisar y evaluar el estado de un proceso empresarial específico. Puede ser una combinación de varios valores de serie temporal divididos en dimensiones. Por ejemplo, una métrica de mantenimiento de la Web podría contener dimensiones para recuento de usuarios y el mercado es-es. |

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes tareas con la biblioteca cliente de Metrics Advisor para JavaScript:

* [Autenticar el cliente](#authenticate-the-client)
* [Adición de una fuente de distribución de datos](#add-a-data-feed)
* [Comprobar el estado de la ingesta](#check-ingestion-status)
* [Configurar la detección de anomalías](#configure-anomaly-detection)
* [Crear un enlace](#create-a-hook)
* [Crear una configuración de alertas](#create-an-alert-configuration)
* [Consultar la alerta](#query-the-alert)

## <a name="authenticate-the-client"></a>Autenticar el cliente

Una vez que tenga las dos claves y la dirección del punto de conexión, puede usar la clase MetricsAdvisorKeyCredential para autenticar a los clientes como se indica a continuación:

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorClient,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);
const client = new MetricsAdvisorClient(endpoint, credential);
const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
```

## <a name="add-a-data-feed"></a>Adición de una fuente de distribución de datos

Metrics Advisor admite la conexión de distintos tipos de orígenes de datos. Este es un ejemplo para ingerir datos de SQL Server.

Reemplace `sql_server_connection_string` por su propia cadena de conexión de SQL Server y reemplace `query` por una consulta que devuelva los datos en una sola marca de tiempo. También tendrá que ajustar los valores de `metric` y `dimension` en función de los datos personalizados.

> [!IMPORTANT]
> La consulta debe devolver como máximo un registro para cada combinación de dimensión, en cada marca de tiempo. Y todos los registros devueltos por la consulta deben tener las mismas marcas de tiempo. El asesor de métricas ejecutará esta consulta para cada marca de tiempo que deba ingerir los datos. Consulte [Preguntas frecuentes sobre las consultas](../../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data) para más información y ejemplos. 

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  subscriptionKey = "<paste-your-metrics-advisor-key-here>";
  apiKey ="<paste-your-metrics-advisor-api-key-here>";
  endpoint = "<paste-your-metrics-advisor-endpoint-here>";
  const sqlServerConnectionString ="<sql_server_connection_string>";
  const sqlServerQuery ="<query>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);

  const created = await createDataFeed(adminClient, sqlServerConnectionString, sqlServerQuery);
  console.log(`Data feed created: ${created.id}`);
}

async function createDataFeed(adminClient, sqlServerConnectionString, sqlServerQuery) {
  const metric = [
    {
      name: "revenue",
      displayName: "revenue",
      description: "Metric1 description"
    },
    {
      name: "cost",
      displayName: "cost",
      description: "Metric2 description"
    }
  ];
  const dimension = [
    { name: "city", displayName: "city display" },
    { name: "category", displayName: "category display" }
  ];
  const dataFeedSchema = {
    metrics: metric,
    dimensions: dimension,
    timestampColumn: null
  };
  const dataFeedIngestion = {
    ingestionStartTime: new Date(Date.UTC(2020, 5, 1)),
    ingestionStartOffsetInSeconds: 0,
    dataSourceRequestConcurrency: -1,
    ingestionRetryDelayInSeconds: -1,
    stopRetryAfterInSeconds: -1
  };
  const granualarity = {
    granularityType: "Daily"
  };
  const source = {
    dataSourceType: "SqlServer",
    dataSourceParameter: {
      connectionString: sqlServerConnectionString,
      query: sqlServerQuery
    }
  };
  const options = {
    rollupSettings: {
      rollupType: "AutoRollup",
      rollupMethod: "Sum",
      rollupIdentificationValue: "__CUSTOM_SUM__"
    },
    missingDataPointFillSettings: {
      fillType: "SmartFilling"
    },
    accessMode: "Private",
    admins: ["xyz@example.com"]
  };

  console.log("Creating Datafeed...");
  const result = await adminClient.createDataFeed({
    name: "test_datafeed_" + new Date().getTime().toFixed(),
    source,
    granularity,
    schema: dataFeedSchema,
    ingestionSettings: dataFeedIngestion,
    options
  });

  return result;
}
```

## <a name="check-ingestion-status"></a>Comprobación del estado de la ingesta

Tras iniciar la ingesta de datos, podemos comprobar el estado de la misma.

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const dataFeedId = "<data feed id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
  await checkIngestionStatus(
    adminClient,
    dataFeedId,
    new Date(Date.UTC(2020, 8, 1)),
    new Date(Date.UTC(2020, 8, 12))
  );
}

async function checkIngestionStatus(adminClient, datafeedId, startTime, endTime) {
  // This shows how to use for-await-of syntax to list status
  console.log("Checking ingestion status...");
  for await (const status of adminClient.listDataFeedIngestionStatus(
    datafeedId,
    startTime,
    endTime
  )) {
    console.log(`  [${status.timestamp}] ${status.status} - ${status.message}`);
  }
}
```

## <a name="configure-anomaly-detection"></a>Configuración de la detección de anomalías

Necesitamos una configuración de detección de anomalías para determinar si un punto de la serie temporal es una anomalía. Aunque se aplica automáticamente una configuración de detección predeterminada a cada métrica, se pueden ajustar los modos de detección que se utilizan en los datos. Para ello, es preciso crear una configuración de detección de anomalías.

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const metricId =  "<metric id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);

  const detectionConfig = await configureAnomalyDetectionConfiguration(adminClient, metricId);
  console.log(`Detection configuration created: ${detectionConfig.id}`);
}

async function configureAnomalyDetectionConfiguration(adminClient, metricId) {
  console.log(`Creating an anomaly detection configuration on metric '${metricId}'...`);
  return await adminClient.createMetricAnomalyDetectionConfiguration({
    name: "test_detection_configuration" + new Date().getTime().toString(),
    metricId,
    wholeSeriesDetectionCondition: {
      smartDetectionCondition: {
        sensitivity: 100,
        anomalyDetectorDirection: "Both",
        suppressCondition: {
          minNumber: 1,
          minRatio: 1
        }
      }
    },
    description: "Detection configuration description"
  });
}
```

### <a name="create-a-hook"></a>Creación de un enlace

Los enlaces se usan para suscribirse a alertas en tiempo real. En este ejemplo, se crea un webhook en la que el servicio Metrics Advisor puede publicar la alerta.

```javascript

const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
  const hook = await createWebhookHook(adminClient);
  console.log(`Webhook hook created: ${hook.id}`);
}

async function createWebhookHook(adminClient) {
  console.log("Creating a webhook hook");
  const hook = {
    hookType: "Webhook",
    name: "web hook " + new Date().getTime().toFixed(),
    description: "description",
    hookParameter: {
      endpoint: "https://example.com/handleAlerts",
      username: "username",
      password: "password"
      // certificateKey: "certificate key",
      // certificatePassword: "certificate password"
    }
  };

  return await adminClient.createHook(hook);
}
```

##  <a name="create-an-alert-configuration"></a>Creación de una configuración de alertas

En este ejemplo se muestra cómo configurar las condiciones que necesita una alerta para desencadenarse y los enlaces que se usan como destino de una alerta.

```javascript
const {
  MetricsAdvisorKeyCredential,
  MetricsAdvisorAdministrationClient
} = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const detectionConfigId = "<detection id>";
  const hookId = "<hook id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const adminClient = new MetricsAdvisorAdministrationClient(endpoint, credential);
  const alertConfig = await configureAlertConfiguration(adminClient, detectionConfigId, [hookId]);
  console.log(`Alert configuration created: ${alertConfig.id}`);
}

async function configureAlertConfiguration(adminClient, detectionConfigId, hookIds) {
  console.log("Creating a new alerting configuration...");
  const metricAlertingConfig = {
    detectionConfigurationId: detectionConfigId,
    alertScope: {
      scopeType: "All"
    },
    alertConditions: {
      severityCondition: { minAlertSeverity: "Medium", maxAlertSeverity: "High" }
    },
    snoozeCondition: {
      autoSnooze: 0,
      snoozeScope: "Metric",
      onlyForSuccessive: true
    }
  };
  return await adminClient.createAnomalyAlertConfiguration({
    name: "test_alert_config_" + new Date().getTime().toString(),
    crossMetricsOperator: "AND",
    metricAlertConfigurations: [metricAlertingConfig],
    hookIds,
    description: "Alerting config description"
  });
}
```

## <a name="query-the-alert"></a>Consulta de la alerta

```javascript
const { MetricsAdvisorKeyCredential, MetricsAdvisorClient } = require("@azure/ai-metrics-advisor");

async function main() {
  // You will need to set these environment variables or edit the following values
  const endpoint = "<service endpoint>";
  const subscriptionKey = "<subscription key>";
  const apiKey = "<api key>";
  const alertConfigId = "<alert config id>";
  const credential = new MetricsAdvisorKeyCredential(subscriptionKey, apiKey);

  const client = new MetricsAdvisorClient(endpoint, credential);

  const alertIds = await queryAlerts(
    client,
    alertConfigId,
    new Date(Date.UTC(2020, 8, 1)),
    new Date(Date.UTC(2020, 8, 12))
  );

  if (alertIds.length > 1) {
    // query anomalies using an alert id.
    await queryAnomaliesByAlert(client, alertConfigId, alertIds[0]);
  } else {
    console.log("No alerts during the time period");
  }
}

async function queryAlerts(client, alertConfigId, startTime, endTime) {
  let alertIds = [];
  for await (const alert of client.listAlertsForAlertConfiguration(
    alertConfigId,
    startTime,
    endTime,
    "AnomalyTime"
  )) {
    alertIds.push(alert.id);
  }

  return alertIds;
}

async function queryAnomaliesByAlert(client, alertConfigId, alertId) {
  console.log(
    `Listing anomalies for alert configuration '${alertConfigId}' and alert '${alertId}'`
  );
  for await (const anomaly of client.listAnomaliesForAlert(alertConfigId, alertId)) {
    console.log(
      `  Anomaly ${anomaly.severity} ${anomaly.status} ${anomaly.dimension} ${anomaly.timestamp}`
    );
  }
}
```

### <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node` en el archivo de inicio rápido.

```console
node index.js
```