---
title: Inicio rápido de Java para Metrics Advisor
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 10/14/2020
ms.author: mbullwin
ms.openlocfilehash: ff8a09e32a44f51571cca93655f91080e5df9a50
ms.sourcegitcommit: 7a7b6c7ac0aa9dac678c3dfd4b5bcbc45dc030ca
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2020
ms.locfileid: "93186961"
---
[Documentación de referencia](https://westus2.dev.cognitive.microsoft.com/docs/services/MetricsAdvisor/) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src) | [Artifact (Maven)](https://search.maven.org/artifact/com.azure/azure-ai-metricsadvisor) | [Ejemplos](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src/samples)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)
* La versión actual de [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* La [herramienta de compilación de Gradle](https://gradle.org/install/) u otro administrador de dependencias.
* Una vez que tenga la suscripción de Azure, <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title="cree un recurso de Metrics Advisor"  target="_blank">create a Metrics Advisor resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para implementar la instancia de Metrics Advisor.  
* Su propia base de datos SQL con datos de series temporales.
  
  
> [!TIP]
> * Puede encontrar ejemplos de Metrics Advisor de Java en [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src/samples).
> * El recurso de Metrics Advisor puede tardar entre 10 y 30 minutos en implementar una instancia de servicio para que pueda usarla. Haga clic en **Ir al recurso** una vez que se implemente correctamente. Después de la implementación, puede empezar a usar la instancia de Metrics Advisor con el portal web y la API de REST. 
> * Puede encontrar la dirección URL de la API de REST en Azure Portal, en la sección **Información general** de su recurso. Tendrá este aspecto:
>    * `https://<instance-name>.cognitiveservices.azure.com/`
    
## <a name="setting-up"></a>Instalación

### <a name="create-a-new-gradle-project"></a>Creación de un proyecto de Gradle

En este inicio rápido se usa el administrador de dependencias Gradle. Puede encontrar más información de la biblioteca cliente en [Maven Central Repository](https://search.maven.org/artifact/com.azure/azure-ai-metricsadvisor).

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `gradle init` desde el directorio de trabajo. Este comando creará archivos de compilación esenciales para Gradle, como *build.gradle.kts* , que se usa en el runtime para crear y configurar la aplicación.

```console
gradle init --type basic
```

Cuando se le solicite que elija un **DSL** , seleccione **Kotlin**.

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Busque *build.gradle.kts* y ábralo con el IDE o el editor de texto que prefiera. A continuación, se copia en esta configuración de compilación. Asegúrese de incluir las dependencias del proyecto.

```kotlin
dependencies {
    compile("com.azure:azure-ai-metricsadvisor:1.0.0-beta.1")
}
```

### <a name="create-a-java-file"></a>Creación de un archivo Java

Cree una carpeta para la aplicación de ejemplo. En el directorio de trabajo, ejecute el siguiente comando:

```console
mkdir -p src/main/java
```

Vaya a la nueva carpeta y cree un archivo denominado *MetricsAdvisorQuickstarts.java*. Ábralo en el editor o el IDE que prefiera y agregue las siguientes instrucciones `import`:

> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo en [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src/samples), que contiene los ejemplos de código de este inicio rápido.

En la clase `MetricsAdvisorQuickstarts` de la aplicación, cree variables para el punto de conexión y la clave del recurso.


> [!IMPORTANT]
> Vaya a Azure Portal. Si el recurso de Metrics Advisor que creó en la sección **Requisitos previos** se implementó correctamente, haga clic en el botón **Ir al recurso** en **Pasos siguientes**. Sus claves de suscripción y su punto de conexión se encuentran en la página **Key and Endpoint** (Clave y punto de conexión) del recurso, en **Administración de recursos**. <br><br>Para recuperar la clave de API, debe ir a [https://metricsadvisor.azurewebsites.net](https://metricsadvisor.azurewebsites.net). Seleccione los valores apropiados: **Directory** (Directorio), **Subscriptions** (Suscripciones) y **Workspace** (Área de trabajo) del recurso y seleccione **Comenzar**. A continuación, podrá recuperar las claves de API de [https://metricsadvisor.azurewebsites.net/api-key](https://metricsadvisor.azurewebsites.net/api-key).   
>
> Recuerde quitar la clave del código cuando haya terminado y no hacerla nunca pública. En el caso de producción, considere la posibilidad de usar alguna forma segura de almacenar las credenciales, y acceder a ellas. Para más información, consulte el artículo sobre la [seguridad](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) de Cognitive Services.

```java
private static String SUBSCRIPTION_KEY = "<replace-with-your-metrics-advisor-subscription-key-here>";
private static String API_KEY = "<replace-with-your-metrics-advisor-api-key-here>"
private static String ENDPOINT = "<replace-with-your-metrics-advisor-endpoint-here>";
```

En el método `Main()` de la aplicación, agregue llamadas para los métodos que se usan en este inicio rápido. Las creará más adelante.

```java
static void Main(string[] args){

    // You will create the below methods later in the quickstart
    exampleTask1();
}
```

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases controlan algunas de las características principales del SDK de Java para Metrics Advisor.

|Nombre|Descripción|
|---|---|
| [MetricsAdvisorClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/MetricsAdvisorClient.html) | **Se usa para:** <br> - Enumerar pendientes <br> - Enumerar la causa raíz de los incidentes <br> - Recuperar los datos de series temporales originales y los datos de series temporales enriquecidos por el servicio. <br> - Enumerar alertas <br> - Agregar comentarios para optimizar el modelo |
| [MetricsAdvisorAdministrationClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/administration/MetricsAdvisorAdministrationClient.html)| **Permite:** <br> - Administrar fuentes de distribución de datos <br> - Configurar la detección de anomalías <br> - Configurar las alertas de anomalías <br> - Administrar enlaces  |
| [DataFeed](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/models/DataFeed.html)| **Lo que Metrics Advisor ingiere de su origen de datos. `DataFeed`contiene filas de:** <br> - Marcas de tiempo <br> - Cero o más dimensiones <br> - Una o varias medidas  |
| [Métrica](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/models/Metric.html) | `Metric` es una medida cuantificable que se usa para supervisar y evaluar el estado de un proceso empresarial específico. Puede ser una combinación de varios valores de serie temporal divididos en dimensiones. Por ejemplo, una métrica de mantenimiento de la Web podría contener dimensiones para recuento de usuarios y el mercado es-es. |

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes tareas con la biblioteca cliente de Metrics Advisor para Java:

* [Autenticar el cliente](#authenticate-the-client)
* [Adición de una fuente de distribución de datos](#add-a-data-feed)
* [Comprobación del estado de la ingesta](#check-the-ingestion-status)
* [Configuración de la detección de anomalías](#configure-anomaly-detection)
* [Creación de un enlace](#create-a-hook)
* [Creación de una configuración de alertas](#create-an-alert-configuration)
* [Consulta de la alerta](#query-the-alert)

## <a name="authenticate-the-client"></a>Autenticar el cliente

### <a name="create-a-metrics-advisor-client-using-metricsadvisorkeycredential"></a>Creación de un cliente de Metrics Advisor mediante MetricsAdvisorKeyCredential

```java
MetricsAdvisorKeyCredential credential = new MetricsAdvisorKeyCredential(SUBSCRIPTION_KEY, API_KEY);
MetricsAdvisorClient metricsAdvisorClient = new MetricsAdvisorClientBuilder()
    .endpoint(ENDPOINT)
    .credential(credential)
    .buildClient();
```

### <a name="create-a-metrics-administration-client-using-metricsadvisorkeycredential"></a>Creación de un cliente de Metrics Administration mediante MetricsAdvisorKeyCredential

```java
MetricsAdvisorKeyCredential credential = new MetricsAdvisorKeyCredential(SUBSCRIPTION_KEY, API_KEY);
MetricsAdvisorAdministrationClient metricsAdvisorAdministrationClient =
    new MetricsAdvisorAdministrationClientBuilder()
        .endpoint(ENDPOINT)
        .credential(credential)
        .buildClient();

``` 

## <a name="add-a-data-feed"></a>Adición de una fuente de distribución de datos

Reemplace `sql_server_connection_string` por su propia cadena de conexión de SQL Server y reemplace `query` por una consulta que devuelva los datos en una sola marca de tiempo. También tendrá que ajustar los valores de `metric` y `dimension` en función de los datos personalizados.

> [!IMPORTANT]
> La consulta debe devolver como máximo un registro para cada combinación de dimensión, en cada marca de tiempo. Y todos los registros devueltos por la consulta deben tener las mismas marcas de tiempo. El asesor de métricas ejecutará esta consulta para cada marca de tiempo que deba ingerir los datos. Consulte [Preguntas frecuentes sobre las consultas](../../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data) para más información y ejemplos. 

```java
final DataFeed createdSqlDataFeed = metricsAdvisorAdministrationClient.createDataFeed(
    "My data feed name",
    new SQLServerDataFeedSource("sql_server_connection_string", "query"),
    new DataFeedGranularity().setGranularityType(DataFeedGranularityType.DAILY),
    new DataFeedSchema(Arrays.asList(
        new Metric().setName("cost"),
        new Metric().setName("revenue")))
        .setDimensions(Arrays.asList(
            new Dimension().setName("category"),
            new Dimension().setName("city"))),
    new DataFeedIngestionSettings(OffsetDateTime.parse("2020-01-01T00:00:00Z")),
    new DataFeedOptions()
        .setDescription("My data feed description")
        .setRollupSettings(
            new DataFeedRollupSettings()
                .setAutoRollup(DataFeedAutoRollUpMethod.SUM, Arrays.asList("cost"), "__CUSTOM_SUM__"))
        .setMissingDataPointFillSettings(
            new DataFeedMissingDataPointFillSettings()
                .setFillType(DataSourceMissingDataPointFillType.SMART_FILLING))
        .setAccessMode(DataFeedAccessMode.PUBLIC));

System.out.printf("Data feed Id : %s%n", createdSqlDataFeed.getId());
System.out.printf("Data feed name : %s%n", createdSqlDataFeed.getName());
System.out.printf("Is the query user is one of data feed administrator : %s%n", createdSqlDataFeed.isAdmin());
System.out.printf("Data feed created time : %s%n", createdSqlDataFeed.getCreatedTime());
System.out.printf("Data feed granularity type : %s%n",
    createdSqlDataFeed.getGranularity().getGranularityType());
System.out.printf("Data feed granularity value : %d%n",
    createdSqlDataFeed.getGranularity().getCustomGranularityValue());
System.out.println("Data feed related metric Ids:");
createdSqlDataFeed.getMetricIds().forEach(metricId -> System.out.println(metricId));
System.out.printf("Data feed source type: %s%n", createdSqlDataFeed.getSourceType());
createdSqlDataFeed.getSchema().getMetrics().forEach(metric -> {
    System.out.printf("metric name: %s metric id:%s", metric.getName(), metric.getId());
});

System.out.printf("Data feed sql server query: %s%n",
    ((SQLServerDataFeedSource) createdSqlDataFeed.getSource()).getQuery());
```

## <a name="check-the-ingestion-status"></a>Comprobación del estado de la ingesta

En este ejemplo se comprueba el estado de la ingesta de un origen de una fuente de distribución de datos proporcionado previamente.

```java
String dataFeedId = "<use-the-data-feed-id-from-createdSqlDataFeed.getId()"; 

metricsAdvisorAdministrationClient.listDataFeedIngestionStatus(
    dataFeedId,
    new ListDataFeedIngestionOptions(
        OffsetDateTime.parse("2020-01-01T00:00:00Z"),
        OffsetDateTime.parse("2020-09-09T00:00:00Z"))
).forEach(dataFeedIngestionStatus -> {
    System.out.printf("Message : %s%n", dataFeedIngestionStatus.getMessage());
    System.out.printf("Timestamp value : %s%n", dataFeedIngestionStatus.getTimestamp());
    System.out.printf("Status : %s%n", dataFeedIngestionStatus.getStatus());
});
```
## <a name="configure-anomaly-detection"></a>Configuración de la detección de anomalías 

En este ejemplo se muestra la forma en que un usuario puede configurar una configuración de detección de anomalías para sus datos.

```java
String metricId = "<metric-id-from-adding-data-feed>";

ChangeThresholdCondition changeThresholdCondition = new ChangeThresholdCondition()
    .setAnomalyDetectorDirection(AnomalyDetectorDirection.BOTH)
    .setChangePercentage(20)
    .setShiftPoint(10)
    .setWithinRange(true)
    .setSuppressCondition(new SuppressCondition().setMinNumber(1).setMinRatio(2));

HardThresholdCondition hardThresholdCondition = new HardThresholdCondition()
    .setAnomalyDetectorDirection(AnomalyDetectorDirection.DOWN)
    .setLowerBound(5.0)
    .setSuppressCondition(new SuppressCondition().setMinNumber(1).setMinRatio(1));

SmartDetectionCondition smartDetectionCondition = new SmartDetectionCondition()
    .setAnomalyDetectorDirection(AnomalyDetectorDirection.UP)
    .setSensitivity(10.0)
    .setSuppressCondition(new SuppressCondition().setMinNumber(1).setMinRatio(2));

final AnomalyDetectionConfiguration anomalyDetectionConfiguration =
    metricsAdvisorAdministrationClient.createMetricAnomalyDetectionConfiguration(
        metricId,
        new AnomalyDetectionConfiguration("My Anomaly detection configuration")
            .setDescription("anomaly detection config description")
            .setWholeSeriesDetectionCondition(
                new MetricWholeSeriesDetectionCondition()
                    .setChangeThresholdCondition(changeThresholdCondition)
                    .setHardThresholdCondition(hardThresholdCondition)
                    .setSmartDetectionCondition(smartDetectionCondition)
                    .setCrossConditionOperator(DetectionConditionsOperator.OR))
    );
```

## <a name="create-a-hook"></a>Creación de un enlace

En este ejemplo se crea un enlace de correo electrónico que recibe alertas de incidentes de anomalías.

```java
Hook emailHook = new EmailHook("email hook")
    .setDescription("my email hook")
    .addEmailToAlert("alertme@alertme.com")
    .setExternalLink("https://adwiki.azurewebsites.net/articles/howto/alerts/create-hooks.html");

final Hook hook = metricsAdvisorAdministrationClient.createHook(emailHook);
EmailHook createdEmailHook = (EmailHook) hook;
System.out.printf("Hook Id: %s%n", createdEmailHook.getId());
System.out.printf("Hook Name: %s%n", createdEmailHook.getName());
System.out.printf("Hook Description: %s%n", createdEmailHook.getDescription());
System.out.printf("Hook External Link: %s%n", createdEmailHook.getExternalLink());
System.out.printf("Hook Emails: %s%n", String.join(",", createdEmailHook.getEmailsToAlert()));
```

##  <a name="create-an-alert-configuration"></a>Creación de una configuración de alertas

En este ejemplo se muestra la forma en que un usuario puede configurar una configuración de alertas para las anomalías detectadas en sus datos.

```java
String detectionConfigurationId1 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
String detectionConfigurationId2 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
String hookId1 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
String hookId2 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

final AnomalyAlertConfiguration anomalyAlertConfiguration
    = metricsAdvisorAdministrationClient.createAnomalyAlertConfiguration(
        new AnomalyAlertConfiguration("My Alert config name")
            .setDescription("alert config description")
            .setMetricAlertConfigurations(
                Arrays.asList(
                    new MetricAnomalyAlertConfiguration(detectionConfigurationId1,
                        MetricAnomalyAlertScope.forWholeSeries()),
                    new MetricAnomalyAlertConfiguration(detectionConfigurationId2,
                        MetricAnomalyAlertScope.forWholeSeries())
                        .setAlertConditions(new MetricAnomalyAlertConditions()
                            .setSeverityCondition(new SeverityCondition()
                                .setMaxAlertSeverity(Severity.HIGH)))
                ))
            .setCrossMetricsOperator(MetricAnomalyAlertConfigurationsOperator.AND)
            .setIdOfHooksToAlert(Arrays.asList(hookId1, hookId2)));
```

## <a name="query-the-alert"></a>Consulta de la alerta

En este ejemplo se muestra la forma en que un usuario puede consultar las alertas desencadenadas para una configuración de detección de alertas y obtener anomalías para esa alerta.

```java
String alertConfigurationId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
metricsAdvisorClient.listAlerts(
    alertConfigurationId,
    new ListAlertOptions(OffsetDateTime.parse("2020-01-01T00:00:00Z"),
        OffsetDateTime.now(),
        TimeMode.ANOMALY_TIME))
    .forEach(alert -> {
        System.out.printf("Alert Id: %s%n", alert.getId());
        System.out.printf("Alert created on: %s%n", alert.getCreatedTime());

        // List anomalies for returned alerts
        metricsAdvisorClient.listAnomaliesForAlert(
            alertConfigurationId,
            alert.getId())
            .forEach(anomaly -> {
                System.out.printf("Anomaly was created on: %s%n", anomaly.getCreatedTime());
                System.out.printf("Anomaly severity: %s%n", anomaly.getSeverity().toString());
                System.out.printf("Anomaly status: %s%n", anomaly.getStatus());
                System.out.printf("Anomaly related series key: %s%n", anomaly.getSeriesKey().asMap());
            });
    });
```

Puede compilar la aplicación con:

```console
gradle build
```
### <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el objetivo `run`:

```console
gradle run
```