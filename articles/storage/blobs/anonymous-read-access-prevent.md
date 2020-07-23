---
title: Impedir el acceso de lectura público anónimo a contenedores y blobs
titleSuffix: Azure Storage
description: ''
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/06/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: 90d7cd65bbc07524391f34fe0efce2b044664cef
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209159"
---
# <a name="prevent-anonymous-public-read-access-to-containers-and-blobs"></a>Impedir el acceso de lectura público anónimo a contenedores y blobs

El acceso de lectura público anónimo a contenedores y blobs en Azure Storage es una manera cómoda de compartir datos, pero también puede suponer un riesgo para la seguridad. Es importante obrar con prudencia al permitir el acceso anónimo y entender cómo evaluar el acceso anónimo a los datos. La complejidad operativa, el error humano o el ataque malintencionado contra los datos a los que se puede tener acceso público pueden dar lugar a costosas vulnerabilidades de los datos. Microsoft recomienda permitir el acceso anónimo solo cuando sea necesario para el escenario de la aplicación.

De forma predeterminada, una cuenta de almacenamiento permite a los usuarios con los permisos adecuados configurar el acceso público a contenedores y blobs. Puede deshabilitar esta funcionalidad en el nivel de la cuenta de almacenamiento, de modo que los contenedores y blobs de la cuenta no se puedan configurar para el acceso público.

En este artículo se describe cómo analizar las solicitudes anónimas a una cuenta de almacenamiento y cómo evitar el acceso anónimo a la cuenta de almacenamiento o a alguno de sus contenedores.

## <a name="detect-anonymous-requests-from-client-applications"></a>Detección de solicitudes anónimas desde aplicaciones cliente

Al deshabilitar el acceso de lectura público a una cuenta de almacenamiento, se arriesga a rechazar las solicitudes a contenedores y blobs que están configurados actualmente para el acceso público. Cuando se deshabilita el acceso público a una cuenta de almacenamiento, se invalida la configuración de acceso público de todos los contenedores de esta. Además, todas las solicitudes anónimas futuras a esa cuenta generarán un error.

Para comprender de qué forma la deshabilitación del acceso público puede afectar a las aplicaciones cliente, Microsoft recomienda habilitar el registro y las métricas de esa cuenta y analizar los patrones de solicitudes anónimas a lo largo de un intervalo de tiempo. Use métricas para determinar el número de solicitudes anónimas a la cuenta de almacenamiento y utilice registros para determinar a qué contenedores se tiene acceso anónimo.

### <a name="monitor-anonymous-requests-with-metrics-explorer"></a>Supervisión de las solicitudes anónimas con el Explorador de métricas

Para realizar el seguimiento de las solicitudes anónimas a una cuenta de almacenamiento, use el Explorador de métricas de Azure situado en Azure Portal. Para más información sobre el Explorador de métricas, consulte [Introducción al Explorador de métricas de Azure](../../azure-monitor/platform/metrics-getting-started.md).

Siga estos pasos para crear una métrica que realice el seguimiento de las solicitudes anónimas:

1. Vaya a la cuenta de almacenamiento en Azure Portal. En la sección **Supervisión**, seleccione **Métricas**.
1. Seleccione **Agregar métrica**. En el cuadro de diálogo **Métrica**, especifique los valores siguientes:
    1. Deje el campo Ámbito establecido en el nombre de la cuenta de almacenamiento.
    1. Establezca **Espacio de nombres de métricas** en *Blob*. Esta métrica informará de las solicitudes realizadas solo al almacenamiento de blobs.
    1. Establezca el campo **Métrica** en *Transacciones*.
    1. Establezca el campo **Agregación** en *Suma*.

    La nueva métrica mostrará la suma del número de transacciones en el almacenamiento de blobs a lo largo de un intervalo de tiempo determinado. La métrica resultante aparece como se muestra en la siguiente imagen:

    :::image type="content" source="media/anonymous-read-access-prevent/configure-metric-blob-transactions.png" alt-text="Captura de pantalla que muestra cómo configurar la métrica para sumar transacciones de blobs":::

1. A continuación, seleccione el botón **Agregar filtro** para filtrar la métrica de las solicitudes anónimas.
1. En el cuadro de diálogo **Filtro**, especifique los valores siguientes:
    1. Establezca el valor **Propiedad** en *Autenticación*.
    1. Establezca el campo **Operador** en el signo igual (=).
    1. Establezca el campo **Valores** en *Anónimos*.
1. En la esquina superior derecha, seleccione el intervalo de tiempo del que desea ver la métrica. También puede indicar lo pormenorizada que debe ser la agregación de las solicitudes, especificando intervalos en cualquier punto entre 1 minuto y 1 mes.

Una vez configurada la métrica, las solicitudes anónimas comenzarán a aparecer en el gráfico. En la imagen siguiente se muestran las solicitudes anónimas agregadas en los últimos treinta minutos.

:::image type="content" source="media/anonymous-read-access-prevent/metric-anonymous-blob-requests.png" alt-text="Captura de pantalla que muestra las solicitudes anónimas agregadas al almacenamiento de blobs":::

También puede configurar una regla de alerta para recibir una notificación cuando se realice un determinado número de solicitudes anónimas a su cuenta de almacenamiento. Para más información, vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

### <a name="analyze-logs-to-identify-containers-receiving-anonymous-requests"></a>Análisis de los registros para identificar los contenedores que reciben solicitudes anónimas

Los registros de Azure Storage capturan detalles sobre las solicitudes realizadas a la cuenta de almacenamiento, como el modo en que se autorizó una solicitud. Puede analizar los registros para determinar qué contenedores reciben solicitudes anónimas.

Para registrar las solicitudes a su cuenta de Azure Storage con el fin de evaluar las solicitudes anónimas, puede usar el registro de Azure Storage en Azure Monitor (versión preliminar). Para más información, consulte [Supervisión de Azure Storage](../common/monitor-storage.md).

El registro de Azure Storage en Azure Monitor admite el uso de consultas de registro para analizar los datos de registro. Para consultar los registros, puede usar un área de trabajo de Azure Log Analytics. Para más información sobre las consultas de registro, consulte el [Tutorial: Introducción a las consultas de Log Analytics](../../azure-monitor/log-query/get-started-portal.md).

#### <a name="create-a-diagnostic-setting-in-the-azure-portal"></a>Creación de una configuración de diagnóstico en Azure Portal

Para registrar datos de Azure Storage con Azure Monitor y analizarlos con Azure Log Analytics, primero debe crear una configuración de diagnóstico que indique qué tipos de solicitudes y para qué servicios de almacenamiento quiere registrar los datos. Para crear una configuración de diagnóstico en Azure Portal, siga estos pasos:

1. Inscríbase en la [versión preliminar de registro de Azure Storage en Azure Monitor](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u).
1. Cree un área de trabajo de Log Analytics en la suscripción que contenga la cuenta de Azure Storage. Después de configurar el registro de la cuenta de almacenamiento, los registros estarán disponibles en el área de trabajo de Log Analytics. Para obtener más información, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../../azure-monitor/learn/quick-create-workspace.md).
1. Vaya a la cuenta de almacenamiento en Azure Portal.
1. En la sección Supervisión, seleccione **Configuración de diagnóstico (versión preliminar)** .
1. Seleccione **Blob** para registrar las solicitudes realizadas al almacenamiento de blobs.
1. Seleccione **Agregar configuración de diagnóstico**.
1. Proporcione un nombre para la configuración de diagnóstico.
1. En **Detalles de la categoría**, en la sección **registro**, elija qué tipos de solicitudes registrar. Todas las solicitudes anónimas serán solicitudes de lectura, así que seleccione **StorageRead** para capturar las solicitudes anónimas.
1. En **Detalles del destino**, seleccione **Enviar a Log Analytics**. Seleccione la suscripción y el área de trabajo de Log Analytics que creó anteriormente, como se muestra en la siguiente imagen.

    :::image type="content" source="media/anonymous-read-access-prevent/create-diagnostic-setting-logs.png" alt-text="Captura de pantalla que muestra cómo crear una configuración de diagnóstico para el registro de las solicitudes":::

Después de crear la configuración de diagnóstico, las solicitudes a la cuenta de almacenamiento se registran posteriormente según esa configuración. Para más información, consulte [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](../../azure-monitor/platform/diagnostic-settings.md).

Puede encontrar una referencia de los campos disponibles en los registros de Azure Storage en Azure Monitor en [Registros de recursos (versión preliminar)](../common/monitor-storage-reference.md#resource-logs-preview).

#### <a name="query-logs-for-anonymous-requests"></a>Consulta de registros de solicitudes anónimas

Los registros de Azure Storage en Azure Monitor incluyen el tipo de autorización que se usó para hacer una solicitud a una cuenta de almacenamiento. En la consulta de registro, filtre por la propiedad **AuthenticationType** para ver las solicitudes anónimas.

Para recuperar los registros de los últimos siete días de solicitudes anónimas al almacenamiento de blobs, abra el área de trabajo de Log Analytics. Luego, pegue la siguiente consulta en una nueva consulta de registro y ejecútela. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AuthenticationType == "Anonymous"
| project TimeGenerated, AccountName, AuthenticationType, Uri
```

También puede configurar una regla de alerta basada en esta consulta para que le informe sobre las solicitudes anónimas. Para más información, consulte [Creación, visualización y administración de alertas de registro mediante Azure Monitor](../../azure-monitor/platform/alerts-log.md).

## <a name="remediate-anonymous-public-access"></a>Medidas para el acceso público anónimo

Después de haber evaluado las solicitudes anónimas a los contenedores y blobs de la cuenta de almacenamiento, puede tomar medidas para limitar o evitar el acceso público. Si algunos contenedores de la cuenta de almacenamiento deben estar disponibles para el acceso público, puede configurar el valor de acceso público para cada contenedor de la cuenta de almacenamiento. Esta opción proporciona el control más pormenorizado sobre el acceso público. Para más información, consulte [Configuración del nivel de acceso público a un contenedor](anonymous-read-access-configure.md#set-the-public-access-level-for-a-container).

Para mejorar la seguridad, puede deshabilitar el acceso público a una cuenta de almacenamiento entera. La configuración de acceso público de una cuenta de almacenamiento invalida la configuración individual de los contenedores de esa cuenta. Al deshabilitar el acceso público a una cuenta de almacenamiento, los contenedores que estén configurados para permitirlo ya no estarán accesibles de forma anónima. Para más información, consulte [Habilitación o deshabilitación del acceso de lectura público a una cuenta de almacenamiento](anonymous-read-access-configure.md#enable-or-disable-public-read-access-for-a-storage-account).

Si en su escenario es preciso que determinados contenedores estén disponibles para el acceso público, puede ser aconsejable mover esos contenedores y sus blobs a cuentas de almacenamiento reservadas para el acceso público. Luego, puede deshabilitar el acceso público a cualquier otra cuenta de almacenamiento.

### <a name="verify-that-public-access-to-a-blob-is-not-permitted"></a>Comprobación de que no se permite el acceso público a un blob

Para comprobar que se deniega el acceso público a un blob específico, puede intentar descargar el blob a través de su dirección URL. Si la descarga se realiza correctamente, el blob sigue estando disponible públicamente. Si el blob no está accesible públicamente porque se ha deshabilitado el acceso público a la cuenta de almacenamiento, verá un mensaje de error que indica que no se permite el acceso público en esta cuenta de almacenamiento.

En el ejemplo siguiente se muestra cómo usar PowerShell para intentar descargar un blob a través de su dirección URL. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```powershell
$url = "<absolute-url-to-blob>"
$downloadTo = "<file-path-for-download>"
Invoke-WebRequest -Uri $url -OutFile $downloadTo -ErrorAction Stop
```

### <a name="verify-that-modifying-the-containers-public-access-setting-is-not-permitted"></a>Comprobación de que no se permite modificar la configuración de acceso público del contenedor

Para comprobar que la configuración de acceso público de un contenedor no se puede modificar una vez deshabilitado el acceso público a la cuenta de almacenamiento, puede intentar modificar la configuración. El cambio de la configuración de acceso público del contenedor producirá un error si el acceso público está deshabilitado en la cuenta de almacenamiento.

En el ejemplo siguiente se muestra cómo usar PowerShell para intentar cambiar la configuración de acceso público de un contenedor. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$containerName = "<container-name>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

Set-AzStorageContainerAcl -Context $ctx -Container $containerName -Permission Blob
```

### <a name="verify-that-creating-a-container-with-public-access-enabled-is-not-permitted"></a>Comprobación de que no se permite la creación de un contenedor con acceso público habilitado

Si el acceso público está deshabilitado en la cuenta de almacenamiento, no podrá crear un contenedor con acceso público habilitado. Para comprobarlo, puede intentar crear un contenedor con acceso público habilitado.

En el ejemplo siguiente se muestra cómo usar PowerShell para intentar crear un contenedor con acceso público habilitado. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:
 
```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$containerName = "<container-name>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

New-AzStorageContainer -Name $containerName -Permission Blob -Context $ctx
```

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de acceso de lectura público anónimo a contenedores y blobs](anonymous-read-access-configure.md).
- [Acceso anónimo a contenedores y blobs públicos con .NET](anonymous-read-access-client.md).