---
title: API de front-end de Azure para la autenticación
description: Explica cómo usar la API de front-end en C# para la autenticación
author: florianborn71
ms.author: flborn
ms.date: 02/12/2010
ms.topic: how-to
ms.openlocfilehash: 04296a3dab61fdb569126abc1bc1f975d69e226d
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679250"
---
# <a name="use-the-azure-frontend-apis-for-authentication"></a>Uso de las API de front-end de Azure para la autenticación

En esta sección, describiremos cómo usar la API en C# para la autenticación.

## <a name="azurefrontendaccountinfo"></a>AzureFrontendAccountInfo

AzureFrontendAccountInfo se usa para configurar la información de autenticación para una instancia de ```AzureFrontend``` en el SDK.

Los campos importantes son:

```cs

    public class AzureFrontendAccountInfo
    {
        // Something akin to "<region>.mixedreality.azure.com"
        public string AccountDomain;

        // Can use one of:
        // 1) ID and Key.
        // 2) AuthenticationToken.
        // 3) AccessToken.
        public string AccountId = Guid.Empty.ToString();
        public string AccountKey = string.Empty;
        public string AuthenticationToken = string.Empty;
        public string AccessToken = string.Empty;
    }

```

Para la parte _region_ del dominio, use una [región que tenga cerca](../reference/regions.md).

La información de la cuenta se puede obtener desde el portal, tal como se describe en el párrafo [Recuperación de la información de la cuenta](create-an-account.md#retrieve-the-account-information).

## <a name="azure-frontend"></a>Front-end de Azure

Las clases relevantes son ```AzureFrontend``` y ```AzureSession```. ```AzureFrontend``` se usa para la funcionalidad de administración de cuentas y de nivel de cuenta, lo que incluye la creación de sesiones de representación y conversión de activos. ```AzureSession``` se usa para la funcionalidad de nivel de sesión e incluye: actualización de la sesión, consultas, renovación y retirada.

Cada elemento ```AzureSession``` abierto o creado conservará una referencia al front-end que lo crea. Para apagar correctamente, todas las sesiones se deben desasignar antes que el front-end.

Al desasignar una sesión, no se detendrá la VM en Azure. `AzureSession.StopAsync` se debe invocar explícitamente.

Una vez que se ha creado una sesión y su estado se ha marcado como listo, puede conectarse al entorno en tiempo de ejecución de representación remota con `AzureSession.ConnectToRuntime`.

### <a name="threading"></a>Subprocesos

Todas las llamadas asincrónicas de AzureSession y AzureFrontend se completan en un subproceso en segundo plano, no en el de la aplicación principal.

### <a name="conversion-apis"></a>API de conversión

Para obtener más información sobre el servicio de conversión, consulte [la API REST de conversión de modelos](conversion/conversion-rest-api.md).

#### <a name="start-asset-conversion"></a>Inicia una conversión de recursos.

``` cs
private StartConversionAsync _pendingAsync = null;

void StartAssetConversion(AzureFrontend frontend, string modelName, string modelUrl, string assetContainerUrl)
{
    _pendingAsync = frontend.StartConversionAsync(
        new AssetConversionParams(modelName, modelUrl, assetContainerUrl));
    _pendingAsync.Completed +=
        (StartConversionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to start asset conversion!");
            }
        };

        _pendingAsync = null;
}
```

#### <a name="get-conversion-status"></a>Obtener el estado de conversión

``` cs
private ConversionStatusAsync _pendingAsync = null
void GetConversionStatus(AzureFrontend frontend, string assetId)
{
    _pendingAsync = frontend.GetConversionStatusAsync(assetId);
    _pendingAsync.Completed +=
        (ConversionStatusAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get status of asset conversion!");
            }

            _pendingAsync = null;
        };
}
```

### <a name="rendering-apis"></a>Representación de API

Consulte [la API REST de administración de sesiones](session-rest-api.md) para obtener más información sobre la administración de sesiones.

Una sesión de representación se puede crear dinámicamente en el servicio o un identificador de sesión existente se puede "abrir" en un objeto de AzureSession.

#### <a name="create-rendering-session"></a>Creación de una sesión de representación

``` cs
private CreateSessionAsync _pendingAsync = null;
void CreateRenderingSession(AzureFrontend frontend, RenderingSessionVmSize vmSize, ARRTimeSpan maxLease)
{
    _pendingAsync = frontend.CreateNewRenderingSessionAsync(
        new RenderingSessionCreationParams(vmSize, maxLease));

    _pendingAsync.Completed +=
        (CreateSessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to create session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="open-an-existing-rendering-session"></a>Abrir una sesión de representación existente

Abrir una sesión existente es una llamada sincrónica.

``` cs
void CreateRenderingSession(AzureFrontend frontend, string sessionId)
{
    AzureSession session = frontend.OpenRenderingSession(sessionId);
    // Query session status, etc.
}
```

#### <a name="get-current-rendering-sessions"></a>Obtener las sesiones de representación actuales

``` cs
private SessionPropertiesArrayAsync _pendingAsync = null;
void GetCurrentRenderingSessions(AzureFrontend frontend)
{
    _pendingAsync = frontend.GetCurrentRenderingSessionsAsync();
    _pendingAsync.Completed +=
        (SessionPropertiesArrayAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get current rendering sessions!");
            }
            _pendingAsync = null;
        };
}
```

### <a name="session-apis"></a>API de sesión

#### <a name="get-rendering-session-properties"></a>Obtener propiedades de la sesión de representación

``` cs
private SessionPropertiesAsync _pendingAsync = null;
void GetRenderingSessionProperties(AzureSession session)
{
    _pendingAsync = session.GetPropertiesAsync();
    _pendingAsync.Completed +=
        (SessionPropertiesAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                Console.WriteLine("Failed to get properties of session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="update-rendering-session"></a>Actualización de una sesión de representación

``` cs
private SessionAsync _pendingAsync;
void UpdateRenderingSession(AzureSession session, ARRTimeSpan updatedLease)
{
    _pendingAsync = session.RenewAsync(
        new RenderingSessionUpdateParams(updatedLease));
    _pendingAsync.Completed +=
        (SessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                Console.WriteLine("Rendering session renewed succeeded!");
            }
            else
            {
                Console.WriteLine("Failed to renew rendering session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="stop-rendering-session"></a>Detener una sesión de representación

``` cs
private SessionAsync _pendingAsync;
void StopRenderingSession(AzureSession session)
{
    _pendingAsync = session.StopAsync();
    _pendingAsync.Completed +=
        (SessionAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                Console.WriteLine("Rendering session stopped successfully!");
            }
            else
            {
                Console.WriteLine("Failed to stop rendering session!");
            }
            _pendingAsync = null;
        };
}
```

#### <a name="connect-to-arr-inspector"></a>Conectar con el inspector de ARR

``` cs
private ArrInspectorAsync _pendingAsync = null;
void ConnectToArrInspector(AzureSession session, string hostname)
{
    _pendingAsync = session.ConnectToArrInspectorAsync(hostname);
    _pendingAsync.Completed +=
        (ArrInspectorAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                // Launch the html file with default browser
                string htmlPath = res.Result;
#if WINDOWS_UWP
                UnityEngine.WSA.Application.InvokeOnUIThread(async () =>
                {
                    var file = await Windows.Storage.StorageFile.GetFileFromPathAsync(htmlPath);
                    await Windows.System.Launcher.LaunchFileAsync(file);
                }, true);
#else
                InvokeOnAppThreadAsync(() =>
                {
                    System.Diagnostics.Process.Start("file:///" + htmlPath);
                });
#endif
            }
            else
            {
                Console.WriteLine("Failed to connect to ARR inspector!");
            }
        };
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Crear una cuenta](create-an-account.md)
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
