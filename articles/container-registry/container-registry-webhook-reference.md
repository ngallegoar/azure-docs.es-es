---
title: Registro de la referencia del esquema de webhook
description: Referencia de la carga de JSON para las solicitudes de webhook en un registro de contenedor de Azure, que se generan cuando se habilitan webhooks para eventos de inserción o eliminación de artefactos.
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: 8354ef9db24d5825238155ac567d5d829f9b0d7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455964"
---
# <a name="azure-container-registry-webhook-reference"></a>Referencia de webhook de Azure Container Registry

También puede [configurar webhooks](container-registry-webhook.md) para el registro de contenedor que generan eventos cuando se realizan determinadas acciones en el mismo. Por ejemplo, habilite webhooks que se desencadenan cuando una imagen de contenedor o gráfico de Helm se inserta en un registro o se elimina. Cuando se desencadena un webhook, Azure Container Registry emite una solicitud HTTP o HTTPS que contiene información sobre el evento a un punto de conexión que especifique. El punto de conexión, a continuación, puede procesar el webhook y actuar en consecuencia.

Las secciones siguientes detallan el esquema de las solicitudes de webhook generadas por eventos compatibles. Las secciones de evento contienen el esquema de carga para el tipo de evento, una carga de solicitud de ejemplo y uno o más comandos de ejemplo que desencadenarán el webhook.

Para obtener información acerca de cómo configurar webhooks para Azure Container Registry, consulte [Uso de webhooks de Azure Container Registry](container-registry-webhook.md).

## <a name="webhook-requests"></a>Solicitudes de webhook

### <a name="http-request"></a>Solicitud HTTP

Un webhook desencadenado realiza una solicitud `POST` HTTP a la dirección URL del punto de conexión que especificó cuando configuró el webhook.

### <a name="http-headers"></a>Encabezados HTTP

Las solicitudes de webhook incluir un elemento `Content-Type` de `application/json` si no ha especificado un encabezado personalizado `Content-Type` para el webhook.

No se agregan otros encabezados a la solicitud aparte de los encabezados personalizados que pueda haber especificado para el webhook.

## <a name="push-event"></a>Evento de inserción

Webhook que se desencadena cuando se inserta una imagen de contenedor en un repositorio.

### <a name="push-event-payload"></a>Carga del evento de inserción

|Elemento|Tipo|Descripción|
|-------------|----------|-----------|
|`id`|String|El identificador del evento de webhook.|
|`timestamp`|DateTime|La hora en la que se desencadenó el evento de webhook.|
|`action`|String|La acción que desencadenó el evento de webhook.|
|[Destino](#target)|Tipo complejo|El destino del evento que desencadenó el evento de webhook.|
|[Solicitud](#request)|Tipo complejo|La solicitud que generó el evento de webhook.|

### <a name="target"></a><a name="target"></a>destino

|Elemento|Tipo|Descripción|
|------------------|----------|-----------|
|`mediaType`|String|El tipo MIME del objeto al que se hace referencia.|
|`size`|Int32|El número de bytes del contenido. Igual que el campo de longitud.|
|`digest`|String|El resumen del contenido, de acuerdo con la especificación de API HTTP V2 del registro.|
|`length`|Int32|El número de bytes del contenido. Igual que el campo de tamaño.|
|`repository`|String|El nombre del repositorio.|
|`tag`|String|El nombre de etiqueta de imagen.|

### <a name="request"></a><a name="request"></a>solicitud

|Elemento|Tipo|Descripción|
|------------------|----------|-----------|
|`id`|String|El identificador de la solicitud que inició el evento.|
|`host`|String|El nombre de host accesible desde el exterior de la instancia del registro, según lo especificado por el encabezado de host HTTP en las solicitudes entrantes.|
|`method`|String|El método de la solicitud que generó el evento.|
|`useragent`|String|El encabezado de agente de usuario de la solicitud.|

### <a name="payload-example-image-push-event"></a>Ejemplo de carga: evento de inserción de imagen

```JSON
{
  "id": "cb8c3971-9adc-488b-xxxx-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-xxxx-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Comando de la [CLI de Docker](https://docs.docker.com/engine/reference/commandline/cli/) de ejemplo que desencadena el webhook del evento de **inserción** de imagen:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="chart-push-event"></a>Evento de inserción de gráfico

Webhook que se desencadena cuando se inserta un gráfico de Helm en un repositorio.

### <a name="chart-push-event-payload"></a>Carga del evento de inserción de gráfico

|Elemento|Tipo|Descripción|
|-------------|----------|-----------|
|`id`|String|El identificador del evento de webhook.|
|`timestamp`|DateTime|La hora en la que se desencadenó el evento de webhook.|
|`action`|String|La acción que desencadenó el evento de webhook.|
|[Destino](#helm_target)|Tipo complejo|El destino del evento que desencadenó el evento de webhook.|

### <a name="target"></a><a name="helm_target"></a>destino

|Elemento|Tipo|Descripción|
|------------------|----------|-----------|
|`mediaType`|String|El tipo MIME del objeto al que se hace referencia.|
|`size`|Int32|El número de bytes del contenido.|
|`digest`|String|El resumen del contenido, de acuerdo con la especificación de API HTTP V2 del registro.|
|`repository`|String|El nombre del repositorio.|
|`tag`|String|El nombre de la etiqueta del gráfico.|
|`name`|String|El nombre del gráfico.|
|`version`|String|La versión del gráfico.|

### <a name="payload-example-chart-push-event"></a>Ejemplo de carga: evento de inserción del gráfico

```JSON
{
  "id": "6356e9e0-627f-4fed-xxxx-d9059b5143ac",
  "timestamp": "2019-03-05T23:45:31.2614267Z",
  "action": "chart_push",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Comando de la [CLI de Azure](/cli/azure/acr) de ejemplo que desencadena el webhook del evento **chart_push**:

```azurecli
az acr helm push wordpress-5.4.0.tgz --name MyRegistry
```

## <a name="delete-event"></a>Eliminar evento

Webhook que se desencadena cuando se elimina un repositorio de imágenes o un manifiesto. No se desencadena cuando se elimina una etiqueta.

### <a name="delete-event-payload"></a>Carga del evento de eliminación

|Elemento|Tipo|Descripción|
|-------------|----------|-----------|
|`id`|String|El identificador del evento de webhook.|
|`timestamp`|DateTime|La hora en la que se desencadenó el evento de webhook.|
|`action`|String|La acción que desencadenó el evento de webhook.|
|[Destino](#delete_target)|Tipo complejo|El destino del evento que desencadenó el evento de webhook.|
|[Solicitud](#delete_request)|Tipo complejo|La solicitud que generó el evento de webhook.|

### <a name="target"></a><a name="delete_target"></a> destino

|Elemento|Tipo|Descripción|
|------------------|----------|-----------|
|`mediaType`|String|El tipo MIME del objeto al que se hace referencia.|
|`digest`|String|El resumen del contenido, de acuerdo con la especificación de API HTTP V2 del registro.|
|`repository`|String|El nombre del repositorio.|

### <a name="request"></a><a name="delete_request"></a> solicitud

|Elemento|Tipo|Descripción|
|------------------|----------|-----------|
|`id`|String|El identificador de la solicitud que inició el evento.|
|`host`|String|El nombre de host accesible desde el exterior de la instancia del registro, según lo especificado por el encabezado de host HTTP en las solicitudes entrantes.|
|`method`|String|El método de la solicitud que generó el evento.|
|`useragent`|String|El encabezado de agente de usuario de la solicitud.|

### <a name="payload-example-image-delete-event"></a>Ejemplo de carga: evento de eliminación de imagen

```JSON
{
    "id": "afc359ce-df7f-4e32-xxxx-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-xxxx-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Comandos de la [CLI de Azure](/cli/azure/acr) de ejemplo que desencadenan un webhook del evento de **eliminación**:

```azurecli
# Delete repository
az acr repository delete --name MyRegistry --repository MyRepository

# Delete image
az acr repository delete --name MyRegistry --image MyRepository:MyTag
```

## <a name="chart-delete-event"></a>Evento de eliminación de gráfico

Webhook que se desencadena cuando se elimina un gráfico de Helm o un repositorio. 

### <a name="chart-delete-event-payload"></a>Carga del evento de eliminación de gráfico

|Elemento|Tipo|Descripción|
|-------------|----------|-----------|
|`id`|String|El identificador del evento de webhook.|
|`timestamp`|DateTime|La hora en la que se desencadenó el evento de webhook.|
|`action`|String|La acción que desencadenó el evento de webhook.|
|[Destino](#chart_delete_target)|Tipo complejo|El destino del evento que desencadenó el evento de webhook.|

### <a name="target"></a><a name="chart_delete_target"></a> destino

|Elemento|Tipo|Descripción|
|------------------|----------|-----------|
|`mediaType`|String|El tipo MIME del objeto al que se hace referencia.|
|`size`|Int32|El número de bytes del contenido.|
|`digest`|String|El resumen del contenido, de acuerdo con la especificación de API HTTP V2 del registro.|
|`repository`|String|El nombre del repositorio.|
|`tag`|String|El nombre de la etiqueta del gráfico.|
|`name`|String|El nombre del gráfico.|
|`version`|String|La versión del gráfico.|

### <a name="payload-example-chart-delete-event"></a>Ejemplo de carga: evento de eliminación de gráfico

```JSON
{
  "id": "338a3ef7-ad68-4128-xxxx-fdd3af8e8f67",
  "timestamp": "2019-03-06T00:10:48.1270754Z",
  "action": "chart_delete",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Comando de la [CLI de Azure](/cli/azure/acr) de ejemplo que desencadena el webhook del evento **chart_delete**:

```azurecli
az acr helm delete wordpress --version 5.4.0 --name MyRegistry
```

## <a name="next-steps"></a>Pasos siguientes

[Uso de webhooks de Azure Container Registry](container-registry-webhook.md)