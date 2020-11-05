---
title: Uso de las API REST con el fin de realizar la integración continua y la entrega continua (CI/CD) para Azure Stream Analytics en IoT Edge
description: Obtenga información sobre cómo implementar una canalización de integración e implementación continuas para Azure Stream Analytics mediante API REST.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: a7e56758a1a76933d6bb18883aa15ce33ce2e89e
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130925"
---
# <a name="implement-cicd-for-stream-analytics-on-iot-edge-using-apis"></a>Implementación de CI/CD para Stream Analytics en IoT Edge mediante API

Puede habilitar la integración e implementación continuas para trabajos de Azure Stream Analytics mediante API REST. En este artículo se brindan ejemplos sobre qué API usar y cómo usarlas. Las API REST no son compatibles con Azure Cloud Shell.

## <a name="call-apis-from-different-environments"></a>Llamada a las API desde distintos entornos

Las API REST se pueden llamar tanto desde Linux como desde Windows. En los comandos siguientes se muestra la sintaxis adecuada de la llamada API. En secciones posteriores de este artículo se describirá el uso específico de las API.

### <a name="linux"></a>Linux

En Linux, puede usar los comandos `Curl` o `Wget`:

```bash
curl -u { <username:password> }  -H "Content-Type: application/json" -X { <method> } -d "{ <request body> }" { <url> }   
```

```bash
wget -q -O- --{ <method> } -data="<request body>" --header=Content-Type:application/json --auth-no-challenge --http-user="<Admin>" --http-password="<password>" <url>
```
 
### <a name="windows"></a>Windows

En Windows, use PowerShell: 

```powershell 
$user = "<username>" 
$pass = "<password>" 
$encodedCreds = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $user,$pass))) 
$basicAuthValue = "Basic $encodedCreds" 
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]" 
$headers.Add("Content-Type", 'application/json') 
$headers.Add("Authorization", $basicAuthValue) 
$content = "<request body>" 
$response = Invoke-RestMethod <url> -Method <method> -Body $content -Headers $Headers 
echo $response 
```
 
## <a name="create-an-asa-job-on-edge"></a>Creación de un trabajo de ASA en Edge 
 
Para crear un trabajo de Stream Analytics, llame al método PUT con la API Stream Analytics.

|Método|URL de la solicitud|
|------|-----------|
|PUT|`https://management.azure.com/subscriptions/{\**subscription-id**}/resourcegroups/{**resource-group-name**}/providers/Microsoft.StreamAnalytics/streamingjobs/{**job-name**}?api-version=2017-04-01-preview`|
 
Ejemplo del comando mediante **curl** :

```curl
curl -u { <username:password> } -H "Content-Type: application/json" -X { <method> } -d "{ <request body> }" https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/Microsoft.StreamAnalytics/streamingjobs/{jobname}?api-version=2017-04-01-preview  
``` 
 
Ejemplo del cuerpo de la solicitud en JSON:

```json
{ 
  "location": "West US", 
  "tags": { "key": "value", "ms-suppressjobstatusmetrics": "true" }, 
  "sku": {  
      "name": "Standard" 
    }, 
  "properties": { 
    "sku": {  
      "name": "standard" 
    }, 
       "eventsLateArrivalMaxDelayInSeconds": 1, 
       "jobType": "edge", 
    "inputs": [ 
      { 
        "name": "{inputname}", 
        "properties": { 
         "type": "stream", 
          "serialization": { 
            "type": "JSON", 
            "properties": { 
              "fieldDelimiter": ",", 
              "encoding": "UTF8" 
            } 
          }, 
          "datasource": { 
            "type": "GatewayMessageBus", 
            "properties": { 
            } 
          } 
        } 
      } 
    ], 
    "transformation": { 
      "name": "{queryName}", 
      "properties": { 
        "query": "{query}" 
      } 
    }, 
    "package": { 
      "storageAccount" : { 
        "accountName": "{blobstorageaccountname}", 
        "accountKey": "{blobstorageaccountkey}" 
      }, 
      "container": "{blobcontaine}]" 
    }, 
    "outputs": [ 
      { 
        "name": "{outputname}", 
        "properties": { 
          "serialization": { 
            "type": "JSON", 
            "properties": { 
              "fieldDelimiter": ",", 
              "encoding": "UTF8" 
            } 
          }, 
          "datasource": { 
            "type": "GatewayMessageBus", 
            "properties": { 
            } 
          } 
        } 
      } 
    ] 
  } 
} 
```
 
Para más información, consulte la [documentación de la API](/rest/api/streamanalytics/).  
 
## <a name="publish-edge-package"></a>Publicación de un paquete de Edge 
 
Para publicar un trabajo de Stream Analytics en IoT Edge, llame al método POST mediante la API Edge Package Publish.

|Método|URL de la solicitud|
|------|-----------|
|POST|`https://management.azure.com/subscriptions/{\**subscriptionid**}/resourceGroups/{**resourcegroupname**}/providers/Microsoft.StreamAnalytics/streamingjobs/{**jobname**}/publishedgepackage?api-version=2017-04-01-preview`|

Esta operación asincrónica devuelve un estado de 202 hasta que el trabajo se publique correctamente. El encabezado de la respuesta de la ubicación contiene el URI que se usa para obtener el estado del proceso. Mientras se ejecuta el proceso, una llamada al URI del encabezado de la ubicación devuelve un estado de 202. Cuando el proceso finaliza, el URI del encabezado de la ubicación devuelve un estado de 200. 

Ejemplo de una llamada para publicar un paquete de Edge mediante **curl** : 

```bash
curl -d -X POST https://management.azure.com/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/Microsoft.StreamAnalytics/streamingjobs/{jobname}/publishedgepackage?api-version=2017-04-01-preview
```
 
Después de hacer la llamada POST, debe esperar una respuesta con cuerpo vacío. Busque la dirección URL ubicada en el ENCABEZADO de la respuesta y anótela para usarla más adelante.
 
Ejemplo de la dirección URL del ENCABEZADO de la respuesta:

```
https://management.azure.com/subscriptions/{**subscriptionid**}/resourcegroups/{**resourcegroupname**}/providers/Microsoft.StreamAnalytics/StreamingJobs/{**resourcename**}/OperationResults/023a4d68-ffaf-4e16-8414-cb6f2e14fe23?api-version=2017-04-01-preview 
```
Una espera de uno o dos minutos antes de ejecutar el comando siguiente para hacer una llamada API con la dirección URL que encontró en el ENCABEZADO de la respuesta. Reintente el comando si no recibe una respuesta 200.
 
Ejemplo de cómo hacer una llamada API con la dirección URL devuelta mediante **curl** :

```bash
curl -d –X GET https://management.azure.com/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/Microsoft.StreamAnalytics/streamingjobs/{resourcename}/publishedgepackage?api-version=2017-04-01-preview 
```

En la respuesta se incluye la información que se necesita agregar en el script de implementación de Edge. En los ejemplos siguientes se muestra la información que debe recopilar y en qué parte del manifiesto de implementación agregarla.
 
Ejemplo de un cuerpo de respuesta una vez que la publicación se realiza correctamente:

```json
{ 
  edgePackageUrl : null 
  error : null 
  manifest : "{"supportedPlatforms":[{"os":"linux","arch":"amd64","features":[]},{"os":"linux","arch":"arm","features":[]},{"os":"windows","arch":"amd64","features":[]}],"schemaVersion":"2","name":"{jobname}","version":"1.0.0.0","type":"docker","settings":{"image":"{imageurl}","createOptions":null},"endpoints":{"inputs":["\],"outputs":["{outputnames}"]},"twin":{"contentType":"assignments","content":{"properties.desired":{"ASAJobInfo":"{asajobsasurl}","ASAJobResourceId":"{asajobresourceid}","ASAJobEtag":"{etag}","PublishTimeStamp":"{publishtimestamp}"}}}}" 
  status : "Succeeded" 
} 
```

Ejemplo de manifiesto de implementación: 

```json
{ 
  "modulesContent": { 
    "$edgeAgent": { 
      "properties.desired": { 
        "schemaVersion": "1.0", 
        "runtime": { 
          "type": "docker", 
          "settings": { 
            "minDockerVersion": "v1.25", 
            "loggingOptions": "", 
            "registryCredentials": {} 
          } 
        }, 
        "systemModules": { 
          "edgeAgent": { 
            "type": "docker", 
            "settings": { 
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0", 
              "createOptions": "{}" 
            } 
          }, 
          "edgeHub": { 
            "type": "docker", 
            "status": "running", 
            "restartPolicy": "always", 
            "settings": { 
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0", 
              "createOptions": "{}" 
            } 
          } 
        }, 
        "modules": { 
          "<asajobname>": { 
            "version": "1.0", 
            "type": "docker", 
            "status": "running", 
            "restartPolicy": "always", 
            "settings": { 
              "image": "<settings.image>", 
              "createOptions": "<settings.createOptions>" 
            } 
            "version": "<version>", 
             "env": { 
              "PlanId": { 
               "value": "stream-analytics-on-iot-edge" 
          } 
        } 
      } 
    }, 
    "$edgeHub": { 
      "properties.desired": { 
        "schemaVersion": "1.0", 
        "routes": { 
            "route": "FROM /* INTO $upstream" 
        }, 
        "storeAndForwardConfiguration": { 
          "timeToLiveSecs": 7200 
        } 
      } 
    }, 
    "<asajobname>": { 
      "properties.desired": {<twin.content.properties.desired>} 
    } 
  } 
} 
```

Después de la configuración del manifiesto de implementación, consulte [Implementación de módulos de Azure IoT Edge con la CLI de Azure](../iot-edge/how-to-deploy-modules-cli.md) para realizar la implementación.


## <a name="next-steps"></a>Pasos siguientes 
 
* [Azure Stream Analytics en IoT Edge](stream-analytics-edge.md)
* [Tutorial de ASA en IoT Edge](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Desarrollo de trabajos para dispositivos perimetrales de Stream Analytics mediante herramientas de Visual Studio](stream-analytics-tools-for-visual-studio-edge-jobs.md)