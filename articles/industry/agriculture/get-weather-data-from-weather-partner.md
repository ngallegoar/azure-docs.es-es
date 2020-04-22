---
title: Obtener datos meteorológicos de asociados meteorológicos
description: En este artículo se describe cómo obtener datos meteorológicos de los asociados.
author: sunasing
ms.topic: article
ms.date: 03/31/2020
ms.author: sunasing
ms.openlocfilehash: 66fa4e7d3edf839ab1e497e86362bcfc979dc279
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81265072"
---
# <a name="get-weather-data-from-weather-partners"></a>Obtener datos meteorológicos de asociados meteorológicos

Azure FarmBeats le ayuda a reunir datos meteorológicos de los proveedores de datos meteorológicos mediante un marco de conectores basado en Docker. Con este marco, los proveedores de datos meteorológicos implementan un elemento de Docker que se puede integrar con FarmBeats. Actualmente, se admiten los siguientes proveedores de datos meteorológicos:

[Datos de NOAA de Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/)

Los datos meteorológicos se pueden usar para generar información procesable y compilar modelos de inteligencia artificial o aprendizaje automático en FarmBeats.

## <a name="before-you-start"></a>Antes de comenzar

Para obtener datos meteorológicos, asegúrese de que ha instalado FarmBeats. **La integración meteorológica es compatible con la versión 1.2.11 o posterior**. Para instalar Azure FarmBeats, vea [Instalación de FarmBeats](https://aka.ms/farmbeatsinstalldocumentation).

## <a name="enable-weather-integration-with-farmbeats"></a>Habilitación de la integración meteorológica con FarmBeats

Para empezar a obtener datos meteorológicos en el centro de datos de FarmBeats, siga estos pasos:

1. Vaya a Swagger en el centro de datos de FarmBeats (https://yourdatahub.azurewebsites.net/swagger) ).

2. Vaya a /Partner API y realice una solicitud POST con la carga de entrada siguiente:

   ```json
   {  
 
     "dockerDetails": {  
       "credentials": { 
         "username": "<credentials to access private docker - not required for public docker>", 
         "password": "<credentials to access private docker – not required for public docker>"  
       },  
       "imageName" : "<docker image name. Default is azurefarmbeats/fambeats-noaa>",
       "imageTag" : "<docker image tag, default:latest>",
       "azureBatchVMDetails": {  
         "batchVMSKU" : "<VM SKU. Default is standard_d2_v2>",  
         "dedicatedComputerNodes" : 1,
         "nodeAgentSKUID": "<Node SKU. Default is batch.node.ubuntu 18.04>"  
       }
     },
     "partnerCredentials": {  
       "key1": "value1",  
       "key2": "value2"  
     },  
     "partnerType": "Weather",  
     "name": "<Name of the partner>",  
     "description": "<Description>",
     "properties": {  }  
   }  
   ```

   Por ejemplo, para obtener datos meteorológicos de NOAA en Azure Open Datasets, use la carga siguiente. Puede modificar el nombre y la descripción según sus preferencias.

   ```json
   {
 
     "dockerDetails": {
       "imageName": "azurefarmbeats/farmbeats-noaa",
       "imageTag": "latest",
       "azureBatchVMDetails": {
         "batchVMSKU": "standard_d2_v2",
         "dedicatedComputerNodes": 1,
         "nodeAgentSKUID": "batch.node.ubuntu 18.04"
       }
     },
     "partnerType": "Weather",
     "name": "ods-noaa",
     "description": "NOAA data from Azure Open Datasets registered as a Weather Partner"
   }  
   ```

   > [!NOTE]
   > Para más información sobre el objeto Partner, vea el [apéndice](get-weather-data-from-weather-partner.md#appendix).

   En el paso anterior, se aprovisionarán los recursos para permitir la ejecución de Docker en el entorno de FarmBeats del cliente.  

   El aprovisionamiento de los recursos anteriores tarda entre 10 y 15 minutos aproximadamente.

3. Compruebe el estado del objeto /Partner que creó en el paso 2. Para ello, realice una solicitud GET en /Partner API y compruebe el **estado** del objeto partner. Una vez que FarmBeats aprovisiona el asociado correctamente, el estado se establece en **activo**.

4. Vaya a /JobType API y realice ahí mismo una solicitud GET. Busque los trabajos meteorológicos que se crean como parte del proceso de adición de asociados en el paso 2. El campo **pipelineName** de los trabajos meteorológicos tendrá el formato siguiente: "partner-name_partner-type_job-name".

5. Ahora la instancia de FarmBeats tiene un asociado de datos meteorológicos activo y puede ejecutar trabajos para solicitar datos meteorológicos para una ubicación (latitud y longitud) y un intervalo de fechas determinados. Las propiedades JobType tendrá detalles sobre los parámetros necesarios para ejecutar trabajos meteorológicos.

   Por ejemplo, para los datos de NOAA de Azure Open Datasets, se crearán las siguientes propiedades JobType:

   - get_weather_data (obtener datos meteorológicos de ISD/históricos)
   - get_weather_forecast_data (obtener datos meteorológicos de GFS/pronóstico)

6. Anote el **identificador** y los parámetros de las propiedades JobType.

7. Vaya a /Jobs API y realice una solicitud POST en /Jobs con la carga de entrada siguiente:

   ```json
    {
         "typeId": "<id of the JobType>",
         "arguments": {
           "additionalProp1": {},
           "additionalProp2": {},
           "additionalProp3": {}
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
       }
   ```

   Por ejemplo, para ejecutar **get_weather_data**, use la siguiente carga:

   ```json
   {
 
         "typeId": "<id of the JobType>",
         "arguments": {
               "latitude": 47.620422,
               "longitude": -122.349358,
               "start_date": "yyyy-mm-dd",
               "end_date": "yyyy-mm-dd"
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
   }
   ```

8. El paso anterior ejecutará los trabajos meteorológicos tal como se define en el Docker de asociados e ingerirá los datos meteorológicos en FarmBeats. Puede comprobar el estado del trabajo mediante una solicitud GET en /Jobs y buscar **currentState** en la respuesta. Cuando haya terminado, currentState se establece en **Succeeded**.

## <a name="query-ingested-weather-data"></a>Consulta de los datos meteorológicos ingeridos

Una vez completados los trabajos meteorológicos, puede consultar los datos meteorológicos ingeridos para crear modelos o información procesable. Hay dos maneras de acceder a los datos meteorológicos y consultarlos desde FarmBeats:

- La API y
- Time Series Insights (TSI).

### <a name="query-using-rest-api"></a>Consulta mediante API REST

Para consultar datos meteorológicos mediante la API de REST de FarmBeats, siga estos pasos:

1. En Swagger en el centro de datos de FarmBeats (https://yourdatahub.azurewebsites.net/swagger) ), vaya a /WeatherDataLocation API y realice una solicitud GET. La respuesta tendrá objetos /WeatherDataLocation creados para la ubicación (latitud/longitud) especificada como parte de la ejecución del trabajo. Tome nota del **identificador** y de la propiedad **weatherDataModelId** de los objetos.

2. Realice una solicitud GET/{id} en /WeatherDataModel API para la propiedad **weatherDataModelId**, como se indicó en el paso 1. El "Modelo de datos meteorológicos" tiene todos los metadatos y los detalles sobre los datos meteorológicos ingeridos. Por ejemplo, **Weather Measure** dentro del objeto **Modelo de datos meteorológicos** contiene detalles sobre qué información meteorológica se admite y en qué tipos y unidades. Por ejemplo,

   ```json
   {
       "name": "Temperature <name of the weather measure - this is what we will receive as part of the queried weather data>",
       "dataType": "Double <Data Type - eg. Double, Enum>",
       "type": "AmbientTemperature <Type of measure eg. AmbientTemperature, Wind speed etc.>",
       "unit": "Celsius <Unit of measure eg. Celsius, Percentage etc.>",
       "aggregationType": "None <either of None, Average, Maximum, Minimum, StandardDeviation, Sum, Total>",
       "description": "<Description of the measure>"
   }
   ```

   Tome nota de la respuesta de la llamada GET/{id} para el modelo de datos meteorológicos.

3. Vaya a **Telemetry** API y realice una solicitud POST con la carga de entrada siguiente:

   ```json
   {
       "weatherDataLocationId": "<id from step 1 above>",
     "searchSpan": {
       "from": "2020-XX-XXT07:30:00Z",
       "to": "2020-XX-XXT07:45:00Z"
     }
   }
   ```

4. La respuesta que contiene los datos meteorológicos que están disponibles para el intervalo de tiempo especificado tendrá el siguiente aspecto:

   ```json
   {
     "timestamps": [
       "2020-XX-XXT07:30:00Z",
       "2020-XX-XXT07:45:00Z"
     ],
     "properties": [
       {
         "values": [
           "<id of the weatherDataLocation>",
           "<id of the weatherDataLocation>"
         ],
         "name": "Id",
         "type": "String"
       },
       {
         "values": [
           29.1,
           30.2
         ],
         "name": "Temperature <name of the WeatherMeasure as defined in the WeatherDataModel object>",
         "type": "Double <Data Type of the value - eg. Double>"
       }
     ]
   }
   ```

En el ejemplo anterior, la respuesta tiene datos para dos marcas de tiempo junto con el nombre de medida ("Temperature") y los valores de los datos meteorológicos notificados en las dos marcas de tiempo. Tendrá que consultar el modelo de datos meteorológicos asociado (como se describe en el paso 2 anterior) para interpretar el tipo y la unidad de los valores notificados.

### <a name="query-using-azure-time-series-insights-tsi"></a>Consulta mediante Azure Time Series Insights (TSI)

FarmBeats utiliza [Azure Time Series Insights (TSI)](https://azure.microsoft.com/services/time-series-insights/) para ingerir, almacenar, consultar y visualizar datos a escala de IoT, muy contextualizados y optimizados para series temporales.

Los datos meteorológicos se reciben en EventHub y luego se insertan en un entorno de TSI dentro del grupo de recursos de FarmBeats. Después, los datos se pueden consultar directamente desde TSI. Para más información, vea la [documentación de TSI](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).

Siga los pasos para visualizar datos en TSI:

1. Vaya a **Azure Portal** > **FarmBeats DataHub resource group** (Grupo de recursos de centro de datos de FarmBeats) > seleccione el entorno de **Time Series Insights** (tsi-xxxx) > **Directivas de acceso a datos**. Agregue un usuario con acceso de lector o colaborador.

2. Vaya a la página **Información general** del entorno de **Time Series Insights** (tsi-xxxx) y seleccione **URL del Explorador de Time Series Insights**. Ahora puede visualizar los datos meteorológicos ingeridos.

Además del almacenamiento, la consulta y la visualización de los datos meteorológicos, TSI también permite la integración en un panel de Power BI. Para más información, vea [Visualización de datos de Time Series Insights en Power BI](https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi).

## <a name="appendix"></a>Apéndice

|        Asociado   |  Detalles   |
| ------- | -------             |
|     DockerDetails - imageName         |          Nombre de la imagen de Docker. Por ejemplo, docker.io/azurefarmbeats/farmbeats-noaa (imagen en hub.docker.com) O myazureacr.azurecr.io/mydockerimage (imagen en Azure Container Registry), etc. Si no se proporciona ningún registro, el valor predeterminado es hub.docker.com.      |
|          DockerDetails - imageTag             |         Nombre de etiqueta de la imagen de Docker. El valor predeterminado es "latest".     |
|  DockerDetails - credentials      |  Credenciales para acceder al elemento de Docker privado. Esto lo proporcionará el asociado al cliente.   |
|  DockerDetails - azureBatchVMDetails - batchVMSKU     |    SKU de la máquina virtual de Azure Batch. Consulte [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para todas las máquinas virtuales Linux disponibles. Los valores válidos son: "Small', 'ExtraLarge', 'Large', 'A8', 'A9', 'Medium', 'A5', 'A6', 'A7', 'STANDARD_D1', 'STANDARD_D2', 'STANDARD_D3', 'STANDARD_D4', 'STANDARD_D11', 'STANDARD_D12', 'STANDARD_D13', 'STANDARD_D14', 'A10', 'A11', 'STANDARD_D1_V2', 'STANDARD_D2_V2', 'STANDARD_D3_V2', 'STANDARD_D4_V2', 'STANDARD_D11_V2', 'STANDARD_D12_V2', 'STANDARD_D13_V2', 'STANDARD_D14_V2', 'STANDARD_G1', 'STANDARD_G2', 'STANDARD_G3', 'STANDARD_G4', 'STANDARD_G5', 'STANDARD_D5_V2', 'BASIC_A1', 'BASIC_A2', 'BASIC_A3', 'BASIC_A4', 'STANDARD_A1', 'STANDARD_A2', 'STANDARD_A3', 'STANDARD_A4', 'STANDARD_A5', 'STANDARD_A6', 'STANDARD_A7', 'STANDARD_A8', 'STANDARD_A9', 'STANDARD_A10', 'STANDARD_A11', 'STANDARD_D15_V2', 'STANDARD_F1', 'STANDARD_F2', 'STANDARD_F4', 'STANDARD_F8', 'STANDARD_F16', 'STANDARD_NV6', 'STANDARD_NV12', 'STANDARD_NV24', 'STANDARD_NC6', 'STANDARD_NC12', 'STANDARD_NC24', 'STANDARD_NC24r', 'STANDARD_H8', 'STANDARD_H8m', 'STANDARD_H16', 'STANDARD_H16m', 'STANDARD_H16mr', 'STANDARD_H16r', 'STANDARD_A1_V2', 'STANDARD_A2_V2', 'STANDARD_A4_V2', 'STANDARD_A8_V2', 'STANDARD_A2m_V2', 'STANDARD_A4m_V2', 'STANDARD_A8m_V2', 'STANDARD_M64ms', 'STANDARD_M128s', 'STANDARD_D2_V3'. **El valor predeterminado es "standard_d2_v2"** .  |
|    DockerDetails - azureBatchVMDetails - dedicatedComputerNodes   |  No. nodos de ejecución dedicados para el grupo de Batch. El valor predeterminado es 1. |
|    DockerDetails - azureBatchVMDetails - nodeAgentSKUID          |    Id. de SKU del agente del nodo de Azure Batch. Actualmente, solo se admite el agente del nodo de Batch "batch.node.ubuntu 18.04".    |
| DockerDetails - partnerCredentials | Credenciales para llamar a la API de asociados en Docker. El asociado debe proporcionar esta información a sus clientes en función del mecanismo de autenticación admitido; por ejemplo, nombre de usuario/contraseña o claves de API. |
| partnerType | "Meteorología" (otros tipos de asociados en FarmBeats son "sensor" e "imagen").  |
|  name   |   Nombre deseado del asociado en el sistema FarmBeats.   |
|  description |  Descripción   |

## <a name="next-steps"></a>Pasos siguientes

Ya ha consultado datos de sensores de la instancia de Azure FarmBeats. A continuación, aprenda a [generar mapas](generate-maps-in-azure-farmbeats.md#generate-maps) para las granjas.
