---
title: Integración de asociados de datos meteorológicos
description: Aprenda cómo puede un proveedor de datos meteorológicos integrarse con FarmBeats.
author: sunasing
ms.topic: article
ms.date: 07/09/2020
ms.author: sunasing
ms.openlocfilehash: f0fbd93e2a5f4e92089e10e75dc17e304ff80bf6
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93147086"
---
# <a name="weather-partner-integration-with-farmbeats"></a>Integración de asociados de datos meteorológicos con FarmBeats

En este artículo se proporciona información sobre el componente Docker de Connector de Azure FarmBeats. Como proveedor de datos meteorológicos, puede utilizar el Docker de Connector para integrarse con FarmBeats. Utilice las API de este para enviar datos meteorológicos a FarmBeats. En FarmBeats, los datos se pueden usar para la fusión de datos y la creación de modelos de aprendizaje automático o de inteligencia artificial.

 > [!NOTE]
 > En este artículo, se usan una [implementación de referencia](https://github.com/azurefarmbeats/noaa_docker) que se ha compilado con Azure Open Datasets y datos meteorológicos de la Administración Nacional Oceánica y Atmosférica (NOAA). Utilizamos igualmente la [imagen de Docker](https://hub.docker.com/r/azurefarmbeats/farmbeats-noaa) correspondiente.

Debe proporcionar un [programa o imagen de Docker adecuado](#docker-specifications) y hospedar la imagen de Docker en un registro de contenedor al que los clientes puedan acceder. Especifique la siguiente información a sus clientes:

- Dirección URL de la imagen de Docker
- Etiqueta de la imagen de Docker
- Claves o credenciales para acceder a la imagen de Docker
- Claves o credenciales de API específicas del cliente para acceder a los datos del sistema
- Detalles de la SKU de máquina virtual (proporcione estos detalles si la imagen de Docker tiene requisitos de máquina virtual específicos. Si no, los clientes pueden elegir entre las SKU de máquina virtual admitidas en Azure.)

Con la información de Docker anterior, el cliente registrará a un asociado de datos meteorológicos en la instancia de FarmBeats. Para más información sobre cómo pueden los clientes utilizar Docker para ingerir datos meteorológicos en FarmBeats, consulte [Obtener datos meteorológicos de asociados meteorológicos](./get-weather-data-from-weather-partner.md).

## <a name="connector-docker-development"></a>Desarrollo del Docker de Connector

**Integración basada en la API REST**

Las API de FarmBeats contienen documentación técnica de Swagger. Para más información sobre las API y sus solicitudes o respuestas correspondientes, consulte [Swagger de FarmBeats](https://aka.ms/farmbeatsswagger). 

Si ya ha instalado FarmBeats, acceda a Swagger de FarmBeats en `https://yourfarmbeatswebsitename-api.azurewebsites.net/swagger`.

Tenga en cuenta que se anexa *-api* al nombre del sitio web de FarmBeats. El punto de conexión de la API es `https://yourfarmbeatswebsitename-api.azurewebsites.net`

### <a name="datahub-lib"></a>Biblioteca del centro de datos

FarmBeats proporciona una biblioteca que puede utilizar. La biblioteca está disponible actualmente como [parte de la implementación de referencia](https://github.com/azurefarmbeats/noaa_docker/tree/master/datahub_lib). Más adelante, estará disponible como un SDK para varios lenguajes.

### <a name="authentication"></a>Authentication

**Autenticación con las API de FarmBeats**

FarmBeats utiliza la autenticación de portador. Para acceder a las API, proporcione un token de acceso en la sección de encabezado de la solicitud. Este es un ejemplo:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

Puede solicitar el token de acceso desde una aplicación de Azure Functions que se ejecute en la instancia de FarmBeats del cliente. La dirección URL de Azure Functions se proporciona al programa Docker como argumento. Para obtener el token de acceso, realice una solicitud `GET` en la dirección URL. La respuesta de la dirección URL contiene el token de acceso. 

Utilice las funciones auxiliares de la biblioteca del centro de datos para obtener el token de acceso. Para más información, consulte la [página de GitHub para la imagen de Docker de NOAA](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/auth/partner_auth_helper.py).

El token de acceso solo es válido durante unas pocas horas. Cuando expire, deberá volver a solicitarlo.

**Autenticación con las API del lado del asociado**

Para autenticarse con las API del lado del asociado mientras se ejecuta el trabajo de Docker, los clientes deben proporcionar las credenciales durante el registro del asociado. Este es un ejemplo:

```json
{
 "partnerCredentials": {
   "key1": "value1",
   "key2": "value2"
   }
}
```
El servicio de API serializa este diccionario y lo almacena en una instancia del [almacén de claves](../../key-vault/general/basic-concepts.md).

[Azure Data Factory](../../data-factory/introduction.md) se utiliza para organizar trabajos meteorológicos. Pone en marcha los recursos para ejecutar el código de Docker. Data Factory también proporciona un mecanismo para insertar datos de forma segura en la máquina virtual donde se ejecuta el trabajo de Docker. Las credenciales de API se almacenan de forma segura en el almacén de claves. 

Las credenciales se leen como cadenas seguras desde el almacén de claves. Se proporcionan como propiedades extendidas en el directorio de trabajo del contenedor de Docker. La ruta de acceso del archivo es */mnt/working_dir/activity.json*. 

El código de Docker puede leer las credenciales de *activity.json* durante el tiempo de ejecución para acceder a las API del lado del asociado para el cliente. En el archivo JSON, las credenciales son similares a este ejemplo de código:

```json
{ 
   "typeProperties" : {
      "extendedProperties" : { 
         "partnerCredentials": "" } 
   } 
}
```
La credencial `partnerCredentials` está disponible en la forma en que el cliente la proporcionó durante el registro del asociado.

La biblioteca de FarmBeats proporciona funciones auxiliares. Utilice estas funciones para leer las credenciales de las propiedades de la actividad. Para más información, consulte la [página de GitHub para la imagen de Docker de NOAA](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/auth/partner_adf_helper.py).

El archivo solo se usa mientras se está ejecutando el código de Docker. Una vez finalizado el código, se elimina el archivo.

Para más información sobre cómo funcionan las canalizaciones y actividades de Data Factory, consulte el artículo sobre [asignación de esquemas y tipos de datos](../../data-factory/copy-activity-schema-and-type-mapping.md).

**Encabezados de solicitud HTTP**

En la tabla siguiente se muestran los encabezados de solicitud más comunes que debe especificar al realizar una llamada API a FarmBeats.

Encabezado | Descripción y ejemplo
--- | ---
Content-Type | Formato de la solicitud. Ejemplo: `Content-Type: application/<format>` <br/>En el caso de las API de centro de datos de FarmBeats, el formato es JSON. Ejemplo: ` Content-Type: application/json`
Authorization | Token de acceso necesario para realizar una llamada API. Ejemplo: `Authorization: Bearer <Access-Token>`
Accept | El formato de respuesta. En el caso de las API de centro de datos de FarmBeats, el formato es JSON. Ejemplo: `Accept: application/json`

## <a name="data-format"></a>Formato de datos

JSON es un formato de datos común independiente del lenguaje que proporciona una representación de texto simple de estructuras de datos arbitrarias. Para más información, consulte [JSON.org](https://json.org).

## <a name="docker-specifications"></a>Especificaciones de Docker

El programa Docker necesita dos componentes: el arranque y el trabajo. El programa puede tener más de un trabajo.

### <a name="bootstrap"></a>Bootstrap

El componente de arranque se debe ejecutar cuando el cliente inicia el registro de Docker en FarmBeats. Se pasan los siguientes argumentos (`arg1` y `arg2`) al programa:

- **Punto de conexión de API de FarmBeats** : punto de conexión de API de FarmBeats para solicitudes de API. Este punto de conexión realiza llamadas API a la implementación de FarmBeats.
- **Dirección URL de Azure Functions** : su propio punto de conexión. Esta dirección URL proporciona el token de acceso para las API de FarmBeats. Puede llamar a `GET` en esta dirección URL para capturar el token de acceso.

El arranque crea los metadatos que los usuarios necesitan para ejecutar los trabajos y obtener los datos meteorológicos. Para más información, consulte la [implementación de referencia](https://github.com/azurefarmbeats/noaa_docker). 

Si actualiza el archivo *bootstrap_manifest.json* , el programa de arranque de referencia creará los metadatos necesarios automáticamente. El programa de arranque crea los siguientes metadatos: 

 > [!NOTE]
 > Si actualiza el archivo *bootstrap_manifest.json* tal como se describe en la [implementación de referencia](https://github.com/azurefarmbeats/noaa_docker), no es necesario que cree los metadatos siguientes. El programa de arranque usará el archivo de manifiesto para crear los metadatos necesarios.

- /**WeatherDataModel** : Los metadatos de WeatherDataModel representan los datos meteorológicos. Corresponden a los conjuntos de datos proporcionados por el origen. Por ejemplo, un modelo DailyForecastSimpleModel podría proporcionar una información de temperatura, humedad y precipitación medias una vez al día. Por el contrario, un modelo DailyForecastAdvancedModel podría proporcionar mucha más información con una granularidad por hora. Puede crear cualquier número de modelos de datos meteorológicos.
- /**JobType** : FarmBeats tiene un sistema de administración de trabajos extensible. Como proveedor de datos meteorológicos, tendrá varios conjuntos de datos y API (por ejemplo, GetDailyForecasts). Puede habilitar estos conjuntos de datos y API en FarmBeats mediante JobType. Una vez que se crea un tipo de trabajo, un cliente puede desencadenar trabajos de ese tipo a fin de obtener datos meteorológicos para su ubicación o granja de servidores de interés. Para más información, consulte JobType y las API de trabajo en [Swagger de FarmBeats](https://aka.ms/farmbeatsswagger).

### <a name="jobs"></a>Trabajos

El componente de trabajos se invoca cada vez que un usuario de FarmBeats ejecuta un trabajo del elemento /JobType creado como parte del proceso de arranque. El comando de ejecución de Docker para el trabajo se define como parte del elemento /JobType que creó.

El trabajo captura los datos del origen y los inserta en FarmBeats. Durante el proceso de arranque, los parámetros necesarios para obtener los datos deben definirse como parte de /JobType.

Como parte del trabajo, el programa tendrá que crear un elemento /WeatherDataLocation basado en el modelo /WeatherDataModel que se creó durante el proceso de arranque. El elemento /WeatherDataLocation corresponde a una ubicación (coordenadas de latitud y longitud) que el usuario proporcionó como parámetro para el trabajo.

### <a name="object-details"></a>Detalles del objeto

WeatherDataModel | Descripción |
--- | ---
Nombre  | Nombre del modelo de datos meteorológicos. |
Descripción  | Proporciona una descripción significativa del modelo. |
Propiedades  | Propiedades adicionales definidas por el proveedor de datos. |
weatherMeasures > Name  | Nombre de la medida meteorológica. Por ejemplo, humidity_max. |
weatherMeasures > DataType  | Double o Enum. Si es Enum, measureEnumDefinition es obligatorio. |
weatherMeasures > measureEnumDefinition  | Solo es necesario si DataType es Enum. por ejemplo, `{ "NoRain": 0, "Snow": 1, "Drizzle": 2, "Rain": 3 }` |
weatherMeasures > Type  | Tipo de datos de telemetría meteorológicos. Por ejemplo, RelativeHumidity. Los tipos definidos por el sistema son AmbientTemperature, NoUnit, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration y PAR. Para agregar más tipos, consulte la sección [Adición de ExtendedType](#add-extendedtype) de este artículo.
weatherMeasures > Unit | Unidad de datos de telemetría meteorológicos. Las unidades definidas por el sistema son NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMole, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolePerMeterSquaredPerSecond e InchesPerHour. Para agregar más unidades, consulte la sección [Adición de ExtendedType](#add-extendedtype) de este artículo.
weatherMeasures > AggregationType  | Tipo de agregación. Los posibles valores son None, Average, Maximum, Minimum, StandardDeviation, Sum y Total.
weatherMeasures > Depth  | La profundidad del sensor en centímetros. Por ejemplo, la medida de la humedad 10 cm bajo el suelo.
weatherMeasures > Description  | Descripción significativa de la medida. 

JobType | Descripción |
--- | ---
Nombre  | Nombre del trabajo. Por ejemplo, Get_Daily_Forecast. El cliente ejecutará este trabajo para obtener datos meteorológicos.|
pipelineDetails > parameters > name  | Nombre del parámetro. |
pipelineDetails > parameters > type | El tipo de parámetro. Entre los valores posibles se incluyen String, Int, Float, Bool y Array. |
pipelineDetails > parameters > isRequired | Valor booleano del parámetro. El valor es true si el parámetro es necesario. En caso contrario, el valor es false. El valor predeterminado es true. |
pipelineDetails > parameters > defaultValue | Valor predeterminado del parámetro. |
pipelineDetails > parameters > description | Descripción del parámetro. |
Propiedades  | Propiedades adicionales del fabricante.
Properties > programRunCommand | Comando de ejecución de Docker. Este comando se ejecuta cuando el cliente ejecuta el trabajo meteorológico. |

WeatherDataLocation | Descripción |
--- | ---
weatherDataModelId  | Identificador del modelo WeatherDataModel correspondiente que se creó durante el proceso de arranque.|
ubicación  | Latitud, longitud y elevación. |
Nombre | Nombre del objeto. |
Descripción | Descripción de la ubicación de datos meteorológicos. |
farmId | (Opcional) Identificador de la granja de servidores. El cliente proporciona este identificador como parte del parámetro job. |
Propiedades  | Propiedades adicionales del fabricante.

Para más información sobre los objetos y sus propiedades, consulte [Swagger de FarmBeats](https://aka.ms/FarmBeatsSwagger).

> [!NOTE]
> Las API devuelven identificadores únicos para cada instancia creada. El traductor para la administración de dispositivos y la sincronización de metadatos debe conservar este identificador.

**Sincronización de metadatos**

El componente Docker de Connector debería poder enviar actualizaciones en los metadatos. Por ejemplo, debería enviar actualizaciones cuando el proveedor meteorológico agrega parámetros nuevos a un conjunto de datos o cuando se agrega una nueva funcionalidad, como una nueva previsión de 30 días.

> [!NOTE]
> No se admite la eliminación para metadatos en el modelo de datos meteorológicos.
>
> Para actualizar los metadatos, debe llamar a `/Get/{ID}` en el modelo de datos meteorológicos. Actualice las propiedades modificadas y, a continuación, realice `/Put/{ID}` para conservar las propiedades que establece el usuario.

## <a name="weather-data-telemetry-specifications"></a>Especificaciones de datos meteorológicos (telemetría)

Los datos meteorológicos se asignan a un mensaje canónico que se inserta en un centro de eventos de Azure para su procesamiento. Azure Event Hubs es un servicio que permite la ingesta de datos en tiempo real (telemetría) de aplicaciones y dispositivos conectados. 

Para enviar datos meteorológicos a FarmBeats, cree un cliente que envíe mensajes a un centro de eventos en FarmBeats. Para más información, consulte el artículo sobre [envío de telemetría a un centro de eventos](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md).

El ejemplo de código de Python siguiente envía telemetría como un cliente a un centro de eventos especificado.

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub connection string provided by customer>"
EVENTHUBNAME = "<EventHub name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

El formato de mensaje canónico es el siguiente:

```json
{
   "weatherstations": [
   {
   "id": "ID of the WeatherDataLocation.",
   "weatherdata": [
   {
     "timestamp": "Timestamp of the data. For historical purposes, this is the time for which the observations are sent. For forecast, this is the time for which data is forecasted. Format is ISO 8601. Default time zone is UTC.",
     "predictiontimestamp": "Timestamp on which the forecast data is predicted. I.e., the time of prediction. Required only for forecast data. Format is ISO 8601. Default time zone is UTC. ",
     "weathermeasurename1": <value>,
     "weathermeasurename2": <value>
     }
     ]
    }
   ]
}
```

A continuación se incluye un mensaje de telemetría de ejemplo:

```json
{
   "weatherstations": [
   {
     "id": "5e4b34e7-bf9e-4f39-xxxx-f3c06d0366ea",
     "weatherdata": [
     {
      "timestamp": "2019-07-10T00:00:00Z",
      "predictiontimestamp": "2019-07-05T00:00:00Z",
      "PrecipitationTotalLiquidEquivalent": 0,
      "AvgPressure": 0,
      "AvgRelativeHumidity": 72
     }
    ] 
  }
 ]
}

```

## <a name="troubleshooting-and-error-management"></a>Solución de problemas y administración de errores

### <a name="error-logging"></a>Registro de errores

El trabajo del asociado se ejecuta en la plataforma existente. Por lo tanto, los errores se registran de la misma manera que los errores de otros trabajos de FarmBeats preexistentes (como GetFarmData y SensorPlacement). La actividad de Data Factory que se ejecuta dentro de la canalización de Data Factory registra `STDERR` y `STDOUT`. Ambos archivos están disponibles en la cuenta de almacenamiento `datahublogs-xxx` dentro del grupo de recursos de FarmBeats.

La biblioteca del centro de datos proporciona funciones auxiliares para habilitar el registro como parte de los registros del centro de datos generales. Para más información, consulte la [página de GitHub para la imagen de Docker de NOAA](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/framework/logger.py).

### <a name="troubleshooting-and-support"></a>Solución de problemas y soporte técnico

Si el cliente no puede recibir datos meteorológicos en la instancia de FarmBeats, proporcione soporte técnico y un mecanismo para solucionar el problema.

## <a name="add-extendedtype"></a>Adición de ExtendedType

FarmBeats permite agregar nuevas unidades y tipos de medidas de sensor. Puede agregar nuevas unidades o tipos actualizando el archivo *bootstrap_manifest.json* en [la implementación de referencia](https://github.com/azurefarmbeats/noaa_docker).

Para agregar un nuevo tipo de WeatherMeasure, por ejemplo, PrecipitationDepth, siga estos pasos.

1. Realice una solicitud `GET` en /ExtendedType mediante la consulta `filter - key = WeatherMeasureType`.
2. Anote el identificador del objeto devuelto.
3. Agregue el nuevo tipo a la lista del objeto devuelto. Realice una solicitud de `PUT` en /ExtendedType{ID} con la siguiente lista nueva. La carga de entrada debe ser la misma que la respuesta recibida anteriormente. La nueva unidad debe anexarse al final de la lista de valores.

Para más información sobre /ExtendedType API, consulte [Swagger de FarmBeats](https://aka.ms/FarmBeatsSwagger).

## <a name="next-steps"></a>Pasos siguientes

Ahora tiene un componente Docker de Connector que se integra con FarmBeats. A continuación, obtenga información sobre cómo [obtener datos meteorológicos](get-weather-data-from-weather-partner.md) mediante la imagen de Docker en FarmBeats. 
