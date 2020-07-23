---
title: Configuración de la versión mínima necesaria de Seguridad de la capa de transporte (TLS) para una cuenta de almacenamiento
titleSuffix: Azure Storage
description: Configure una cuenta de almacenamiento para que exija una versión mínima de Seguridad de la capa de transporte (TLS) para los clientes que realizan solicitudes en Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/08/2020
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: ab83f0ee656dfc717284c1e26d10dcb814fe1c9e
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209131"
---
# <a name="configure-minimum-required-version-of-transport-layer-security-tls-for-a-storage-account"></a>Configuración de la versión mínima necesaria de Seguridad de la capa de transporte (TLS) para una cuenta de almacenamiento

La comunicación entre una aplicación cliente y una cuenta de Azure Storage se cifra mediante la Seguridad de la capa de transporte (TLS). TLS es un protocolo criptográfico estándar que garantiza la privacidad y la integridad de los datos entre los clientes y los servicios a través de Internet. Para más información acerca de TLS, consulte [Seguridad de la capa de transporte](https://en.wikipedia.org/wiki/Transport_Layer_Security).

Actualmente, Azure Storage admite tres versiones del protocolo TLS: 1.0, 1.1 y 1.2. TLS 1.2 es la versión más segura de TLS. Azure Storage usa TLS 1.2 en puntos de conexión HTTP públicos. Sin embargo, TLS 1.0 y TLS 1.1 se siguen admitiendo gracias a la compatibilidad con versiones anteriores.

De forma predeterminada, las cuentas de Azure Storage permiten a los clientes enviar y recibir datos con la versión más antigua de TLS (TLS 1.0) y con las versiones posteriores. Para aplicar medidas de seguridad más estrictas, puede configurar la cuenta de almacenamiento para que los clientes deban enviar y recibir datos con una versión más reciente de TLS. Si una cuenta de almacenamiento requiere una versión mínima de TLS, se producirá un error en todas las solicitudes realizadas con una versión anterior.

En este artículo se describe cómo configurar una cuenta de almacenamiento para exigir que los clientes envíen solicitudes con una versión mínima de TLS. Para obtener información acerca de cómo especificar una versión concreta de TLS al enviar una solicitud desde una aplicación cliente, consulte [Configuración de la Seguridad de la capa de transporte (TLS) para una aplicación cliente](transport-layer-security-configure-client-version.md).

## <a name="detect-the-tls-version-used-by-client-applications"></a>Detección de la versión de TLS que usan las aplicaciones cliente

Cuando se aplica una versión mínima de TLS para la cuenta de almacenamiento, se corre el riesgo de rechazar las solicitudes de los clientes que envían datos con una versión anterior de TLS. Para comprender cómo la configuración de la versión mínima de TLS puede afectar a las aplicaciones cliente, Microsoft recomienda que habilite el registro para su cuenta de Azure Storage y analice los registros después de un intervalo de tiempo para determinar qué versiones de TLS están usando las aplicaciones cliente.

Para registrar las solicitudes a su cuenta de Azure Storage y determinar qué versión de TLS usa el cliente, puede usar el registro de Azure Storage en Azure Monitor (versión preliminar). Para más información, consulte [Supervisión de Azure Storage](monitor-storage.md).

El registro de Azure Storage en Azure Monitor admite el uso de consultas de registro para analizar los datos de registro. Para consultar los registros, puede usar un área de trabajo de Azure Log Analytics. Para más información sobre las consultas de registro, consulte el [Tutorial: Introducción a las consultas de Log Analytics](../../azure-monitor/log-query/get-started-portal.md).

Para registrar datos de Azure Storage con Azure Monitor y analizarlos con Azure Log Analytics, primero debe crear una configuración de diagnóstico que indique qué tipos de solicitudes y para qué servicios de almacenamiento quiere registrar los datos. Para crear una configuración de diagnóstico en Azure Portal, siga estos pasos:

1. Inscríbase en la [versión preliminar de registro de Azure Storage en Azure Monitor](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u).
1. Cree un área de trabajo de Log Analytics en la suscripción que contenga la cuenta de Azure Storage. Después de configurar el registro de la cuenta de almacenamiento, los registros estarán disponibles en el área de trabajo de Log Analytics. Para obtener más información, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../../azure-monitor/learn/quick-create-workspace.md).
1. Vaya a la cuenta de almacenamiento en Azure Portal.
1. En la sección Supervisión, seleccione **Configuración de diagnóstico (versión preliminar)** .
1. Seleccione el servicio de Azure Storage cuyas solicitudes quiere registrar. Por ejemplo, elija **Blob** para registrar las solicitudes a Blob Storage.
1. Seleccione **Agregar configuración de diagnóstico**.
1. Proporcione un nombre para la configuración de diagnóstico.
1. En **Detalles de la categoría**, en la sección **registro**, elija qué tipos de solicitudes registrar. Puede registrar las solicitudes de lectura, escritura y eliminación. Por ejemplo, al elegir **StorageRead** y **StorageWrite** se registrarán las solicitudes de lectura y escritura del servicio seleccionado.
1. En **Detalles del destino**, seleccione **Enviar a Log Analytics**. Seleccione la suscripción y el área de trabajo de Log Analytics que creó anteriormente, como se muestra en la siguiente imagen.

    :::image type="content" source="media/transport-layer-security-configure-minimum-version/create-diagnostic-setting-logs.png" alt-text="Captura de pantalla que muestra cómo crear una configuración de diagnóstico para el registro de las solicitudes":::

Después de crear la configuración de diagnóstico, las solicitudes a la cuenta de almacenamiento se registran posteriormente según esa configuración. Para más información, consulte [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](../../azure-monitor/platform/diagnostic-settings.md).

Puede encontrar una referencia sobre los campos disponibles en los registros de Azure Storage en Azure Monitor en el artículo [Registros de recursos (versión preliminar)](monitor-storage-reference.md#resource-logs-preview).

### <a name="query-logged-requests-by-tls-version"></a>Consulta de las solicitudes registradas por versión de TLS

Los registros de Azure Storage en Azure Monitor incluyen la versión de TLS que se usó para hacer una solicitud a una cuenta de almacenamiento. Use la propiedad **TlsVersion** para comprobar la versión de TLS de una solicitud registrada.

Para recuperar los registros de los últimos 7 días y determinar cuántas solicitudes se realizaron a Blob Storage con cada versión de TLS, abra su área de trabajo de Log Analytics. Luego, pegue la siguiente consulta en una nueva consulta de registro y ejecútela. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AccountName == "<account-name>"
| summarize count() by TlsVersion
```

Los resultados muestran el número de solicitudes realizadas con cada versión de TLS:

:::image type="content" source="media/transport-layer-security-configure-minimum-version/log-analytics-query-version.png" alt-text="Captura de pantalla que muestra los resultados de la consulta de Log Analytics para devolver la versión de TLS":::

### <a name="query-logged-requests-by-caller-ip-address-and-user-agent-header"></a>Consulta de solicitudes registradas por dirección IP del autor de la llamada y por encabezado del agente de usuario

Los registros de Azure Storage en Azure Monitor también incluyen la dirección IP del autor de la llamada y el encabezado del agente de usuario para ayudarle a evaluar qué aplicaciones cliente tienen acceso a la cuenta de almacenamiento. Puede analizar estos valores para determinar si las aplicaciones cliente deben actualizarse para usar una versión más reciente de TLS, o si es aceptable que se produzca un error en la solicitud de un cliente si no se envía con la versión mínima de TLS.

Para recuperar los registros de los últimos 7 días y determinar qué clientes realizaron solicitudes con una versión de TLS anterior a TLS 1.2, pegue la siguiente consulta en una nueva consulta de registro y ejecútela. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AccountName == "<account-name>" and TlsVersion != "TLS 1.2"
| project TlsVersion, CallerIpAddress, UserAgentHeader
```

## <a name="configure-the-minimum-tls-version-for-an-account"></a>Configuración de la versión mínima de TLS para una cuenta

Para configurar la versión mínima de TLS para una cuenta de almacenamiento, use Azure Portal o la CLI de Azure para establecer la versión **minimumTlsVersion** de la cuenta. Esta propiedad está disponible para todas las cuentas de almacenamiento que se crean con el modelo de implementación de Azure Resource Manager. Para más información, consulte [Introducción a las cuentas de almacenamiento](storage-account-overview.md).

# <a name="portal"></a>[Portal](#tab/portal)

Para configurar la versión mínima de TLS de una cuenta de almacenamiento con Azure Portal, siga estos pasos:

1. Vaya a la cuenta de almacenamiento en Azure Portal.
1. Seleccione el valor de **Configuración**.
1. En **Versión mínima de TLS**, use la lista desplegable para seleccionar la versión mínima de TLS necesaria para acceder a los datos de esta cuenta de almacenamiento, tal como se muestra en la siguiente imagen.

    :::image type="content" source="media/transport-layer-security-configure-minimum-version/configure-minimum-version-portal.png" alt-text="Captura de pantalla que muestra cómo configurar la versión mínima de TLS en Azure Portal":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para configurar la versión mínima de TLS de una cuenta de almacenamiento con la CLI de Azure, primero obtenga el identificador de recurso de la cuenta de almacenamiento mediante una llamada al comando [az resource show](/cli/azure/resource#az-resource-show). Luego, llame al comando [az resource update](/cli/azure/resource#az-resource-update) para establecer la propiedad **minimumTlsVersion** de la cuenta de almacenamiento. Los valores válidos para **minimumTlsVersion** son `TLS1_0`, `TLS1_1` y `TLS1_2`.

En el ejemplo siguiente, la versión mínima de TLS se establece en 1.2. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```azurecli-interactive
storage_account_id=$(az resource show \
  --name <storage-account> \
  --resource-group <resource-group> \
  --resource-type Microsoft.Storage/storageAccounts \
  --query id \
  --output tsv)

az resource update \
  --ids $storage_account_id \
  --set properties.minimumTlsVersion="TLS1_2"
```

---

> [!NOTE]
> Después de actualizar la versión mínima de TLS para la cuenta de almacenamiento, el cambio puede tardar hasta 30 segundos en propagarse por completo.

## <a name="check-the-minimum-required-tls-version-for-an-account"></a>Comprobación de la versión mínima de TLS requerida para una cuenta

Para determinar la versión mínima de TLS necesaria configurada para una cuenta de almacenamiento, compruebe la propiedad **minimumTlsVersion** de Azure Resource Manager. Para comprobar esta propiedad en un número elevado de cuentas de almacenamiento a la vez, use Azure Resource Graph Explorer.

La propiedad **minimumTlsVersion** no se establece de forma predeterminada y no devuelve un valor hasta que se establece de forma explícita. De forma predeterminada, la cuenta de almacenamiento permite las solicitudes enviadas con la versión 1.0 o posterior de TLS si el valor de la propiedad es NULL.

### <a name="check-the-minimum-required-tls-version-for-a-single-storage-account"></a>Comprobación de la versión mínima de TLS necesaria para una cuenta de almacenamiento individual

Para comprobar la versión mínima de TLS necesaria en una cuenta de almacenamiento individual mediante la CLI de Azure, llame al comando [az resource show](/cli/azure/resource#az-resource-show) y consulte la propiedad **minimumTlsVersion**:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.minimumTlsVersion \
    --output tsv
```

### <a name="check-the-minimum-required-tls-version-for-a-set-of-storage-accounts"></a>Comprobación de la versión mínima de TLS necesaria para un conjunto de cuentas de almacenamiento

Para comprobar la versión mínima de TLS necesaria en un conjunto de cuentas de almacenamiento con un rendimiento óptimo, puede usar Azure Resource Graph Explorer en Azure Portal. Para más información sobre el uso de Resource Graph Explorer, consulte [Inicio rápido: Ejecución de la primera consulta de Resource Graph mediante Azure Resource Graph Explorer](/azure/governance/resource-graph/first-query-portal).

Si la siguiente consulta se ejecuta en Resource Graph Explorer, este devuelve una lista de cuentas de almacenamiento y muestra la versión mínima de TLS de cada cuenta:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend minimumTlsVersion = parse_json(properties).minimumTlsVersion
| project subscriptionId, resourceGroup, name, minimumTlsVersion
```

---

## <a name="test-the-minimum-tls-version-from-a-client"></a>Prueba de la versión mínima de TLS desde un cliente

Para probar que la versión mínima de TLS necesaria para una cuenta de almacenamiento impida las llamadas realizadas con una versión anterior, puede configurar un cliente para que use una versión anterior de TLS. Para obtener más información acerca de cómo configurar un cliente para que use una versión específica de TLS, consulte [Configuración de la Seguridad de la capa de transporte (TLS) para una aplicación cliente](transport-layer-security-configure-client-version.md).

Cuando un cliente acceda a una cuenta de almacenamiento mediante una versión de TLS que no cumpla con la versión mínima de TLS configurada para la cuenta, Azure Storage devolverá el código de error 400 (solicitud incorrecta) y un mensaje indicando que la versión de TLS que se usó no está permitida para realizar solicitudes en esta cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de Seguridad de la capa de transporte (TLS) para una aplicación cliente](transport-layer-security-configure-client-version.md)
- [Recomendaciones de seguridad para Blob Storage](../blobs/security-recommendations.md)
