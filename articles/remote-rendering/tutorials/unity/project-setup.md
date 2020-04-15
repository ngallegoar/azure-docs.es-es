---
title: Configuración de un proyecto de Unity desde cero
description: Explique cómo configurar un proyecto de Unity desde cero para su uso con Azure Remote Rendering.
author: florianborn71
ms.author: flborn
ms.date: 01/30/2020
ms.topic: tutorial
ms.openlocfilehash: 33801316e4c0446865169560bb42f98052acba70
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678656"
---
# <a name="tutorial-setting-up-a-unity-project-from-scratch"></a>Tutorial: Configuración de un proyecto de Unity desde cero

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Configurar un proyecto Unity desde cero para Azure Remote Rendering.
> * Crear y detener sesiones de representación.
> * Reutilizar las sesiones existentes.
> * Conectarse y desconectarse de sesiones.
> * Cargar modelos en una sesión de representación.
> * Mostrar estadísticas de conexión.

## <a name="prerequisites"></a>Prerrequisitos

Para este tutorial, necesitará:

* La información de su cuenta (identificador de cuenta, clave de cuenta, identificador de suscripción). Si no tiene una cuenta, [cree una](../../how-tos/create-an-account.md).
* Windows SDK 10.0.18362.0 [(descargar)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* La versión más reciente de Visual Studio 2019 [(descargar)](https://visualstudio.microsoft.com/vs/older-downloads/)
* GIT [(descargar)](https://git-scm.com/downloads)
* Unity 2019.3.1 [(descargar)](https://unity3d.com/get-unity/download)
  * Instale estos módulos en Unity:
    * **UWP**: compatibilidad con la compilación de la Plataforma universal de Windows
    * **IL2CPP**: compatibilidad con la compilación de Windows (IL2CPP)

> [!TIP]
> El [repositorio de ejemplos de Azure Remote Rendering](https://github.com/Azure/azure-remote-rendering) contiene proyectos preparados de Unity para todos los tutoriales. Esos proyectos se pueden usar como referencia.

## <a name="create-a-new-unity-project"></a>Creación de un proyecto de Unity

En Unity Hub, cree un proyecto.
En este ejemplo, supondremos que el proyecto se crea en una carpeta denominada `RemoteRendering`.

![ventana de proyecto nuevo](media/new-project.png)

## <a name="configure-the-projects-manifest"></a>Configuración del manifiesto del proyecto

Debe modificar el archivo `Packages/manifest.json` que se encuentra en la carpeta del proyecto de Unity. Abra el archivo en un editor de texto y anexe las líneas que se enumeran a continuación:

```json
{
  "scopedRegistries": [
    {
      "name": "Azure Mixed Reality Services",
      "url": "https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM/",
      "scopes": ["com.microsoft.azure"]
    }
   ],
  "dependencies": {
    "com.microsoft.azure.remote-rendering": "0.1.11",
    "com.unity.render-pipelines.universal": "7.2.1",
    ...existing dependencies...
  }
}
```

El paquete de canalización de representación universal es opcional, pero se recomienda por motivos de rendimiento.
Después de que haya modificado y guardado el manifiesto, Unity se actualiza automáticamente. Confirme que los paquetes se han cargado en la ventana *Project*:

![confirmar las importaciones de paquetes](media/confirm-packages.png)

## <a name="ensure-you-have-the-latest-version-of-the-package"></a>Asegúrese de que tiene la versión más reciente del paquete

Los siguientes pasos garantizan que en el proyecto se usa la versión más reciente del paquete de representación remota.
1. Seleccione el paquete en la ventana Project y haga clic en el icono del paquete: ![Seleccionar el icono del paquete](media/package-icons.png)
1. En Inspector, haga clic en "View in Package Manager" (Ver en administrador de paquetes): ![inspector de paquetes](media/package-properties.png)
1. En la página del administrador de paquetes para el paquete de representación remota, vea si está disponible el botón de actualización. Si lo está, al hacer clic en él actualizará el paquete a la versión más reciente disponible: ![El paquete de Azure Remote Rendering en el administrador de paquetes](media/package-manager.png)
1. A veces, la actualización del paquete puede provocar errores en la consola. En ese caso, prueba a cerrar y volver a abrir el proyecto.

## <a name="configure-the-camera"></a>Configuración de la cámara

Seleccione el nodo **Main Camera** (Cámara principal).

1. Restablezca su el valor de *Transform* (Transformación):

    ![restablecer transformación de la cámara](media/camera-reset-transform.png)

1. En **Clear flags** (Borrar marcas), seleccione *Solid Color* (Color sólido).

1. En **Background** (Fondo), seleccione *Black* (Negro).

1. En **Clipping Planes** (Planos de recorte), seleccione *Cerca = 0,3* y *Lejos = 20*. Esto significa que la representación recortará la geometría que está a menos de 30 cm o a más de 20 m.

    ![Propiedades de la cámara de Unity](media/camera-properties.png)

## <a name="adjust-the-project-settings"></a>Ajuste de la configuración del proyecto

1. Abra *Edit > Project Settings...* (Editar > Configuración del proyecto).
1. En la lista de la izquierda, seleccione Quality (Calidad).
1. Cambie el valor de **Default Quality Level** (Nivel de calidad predeterminado) a *Low* (Bajo).

    ![cambiar la configuración de la calidad del proyecto](media/settings-quality.png)

1. Seleccione **Graphics** (Gráficos) a la izquierda.
1. Cambie el valor de **Scriptable Rendering Pipeline** (Canalización de representación mediante script) a *HybridRenderingPipeline*. Omite este paso si no se usa la canalización de representación universal.

    ![cambio de configuración de gráficos del proyecto](media/settings-graphics-lwrp.png) A veces, la interfaz de usuario no rellena la lista de tipos de canalización disponibles desde los paquetes, en cuyo caso el recurso *HybridRenderingPipeline* se debe arrastrar sobre el campo manualmente: ![cambio de configuración de gráficos del proyecto](media/hybrid-rendering-pipeline.png)
1. Seleccione **Player** (Reproductor) a la izquierda.
1. Seleccione la pestaña **Universal Windows Platform settings** (Configuración de Plataforma universal de Windows).
1. Cambie el valor de **XR Settings** (Configuración de XR) para que admita Windows Mixed Reality: ![configuración del reproductor](media/xr-player-settings.png)
1. Seleccione la configuración como en la captura de pantalla anterior:
    1. Habilite **Virtual Reality Supported** (Se admite realidad virtual)
    1. En **Depth Format** (Formato de profundidad), seleccione *16-Bit Depth* (Profundidad de 16 bits)
    1. Habilite **Depth Buffer Sharing** (Uso compartido de búfer de profundidad)
    1. En **Stereo Rendering Mode** (Modo de representación estéreo), seleccione *Single Pass Instanced* (Instancia de paso único)

1. En la misma ventana, encima de *XR Settings* (Configuración de XR), expanda **Publishing Settings** (Configuración de publicación)
1. Desplácese hacia abajo hasta **Capabilities** (Funcionalidades) y seleccione:
    * **InternetClient**
    * **InternetClientServer**
    * **SpatialPerception**
    * Opcional para desarrollo: **PrivateNetworkClientServer**

      Esta opción es necesaria si desea conectar el depurador remoto de Unity al dispositivo.

1. En **Supported Device Families** (Familias de dispositivos compatibles), habilite **Holographic** (Holográfico) y **Desktop** (Escritorio)

1. Si desea usar Mixed Reality Toolkit, consulte la [documentación de MRTK](https://docs.microsoft.com/windows/mixed-reality/unity-development-overview), donde encontrará más información sobre la configuración y las funcionalidades recomendadas.

## <a name="validate-project-setup"></a>Validar la configuración del proyecto

Realice los siguientes pasos para validar que la configuración del proyecto es correcta.

1. Elija la entrada ValidateProject en el menú RemoteRendering de la barra de herramientas del Editor de Unity.
1. Use la ventana de ValidateProject para comprobar y corregir la configuración del proyecto cuando sea necesario.

    ![Validación de un proyecto en el Editor de Unity](media/arr-unity-validation.png)

## <a name="create-a-script-to-initialize-azure-remote-rendering"></a>Creación de un script para inicializar Azure Remote Rendering

Cree un [script](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) y asígnele el nombre **RemoteRendering**. Abra el archivo de script y reemplace todo su contenido por el código siguiente:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

#if UNITY_WSA
    using UnityEngine.XR.WSA;
#endif

// ask Unity to automatically append an ARRServiceUnity component when a RemoteRendering script is attached
[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRendering : MonoBehaviour
{
    // fill out the variables below with your account details

    // AccountDomain must be '<region>.mixedreality.azure.com' - if no '<region>' is specified, connections will fail
    // See the documentation for the list of available regions.
    public string AccountDomain = "westus2.mixedreality.azure.com";
    public string AccountId = "<enter your account id here>";
    public string AccountKey = "<enter your account key here>";

    public uint MaxLeaseTimeHours = 0;
    public uint MaxLeaseTimeMinutes = 10;
    public RenderingSessionVmSize VMSize = RenderingSessionVmSize.Standard;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        // initialize Azure Remote Rendering for use in Unity:
        // it needs to know which camera is used for rendering the scene
        RemoteUnityClientInit clientInit = new RemoteUnityClientInit(Camera.main);
        RemoteManagerUnity.InitializeManager(clientInit);

        // lookup the ARRServiceUnity component and subscribe to session events
        arrService = GetComponent<ARRServiceUnity>();
        arrService.OnSessionErrorEncountered += ARRService_OnSessionErrorEncountered;
        arrService.OnSessionStatusChanged += ARRService_OnSessionStatusChanged;
    }

#if !UNITY_EDITOR
    private void Start()
    {
        StartCoroutine(AutoStartSessionAsync());
    }
#endif

    private void OnDestroy()
    {
        arrService.OnSessionErrorEncountered -= ARRService_OnSessionErrorEncountered;
        arrService.OnSessionStatusChanged -= ARRService_OnSessionStatusChanged;

        RemoteManagerStatic.ShutdownRemoteRendering();
    }

    private void CreateFrontend()
    {
        if (arrService.Frontend != null)
        {
            // early out if the front-end has been created before
            return;
        }

        // initialize the ARR service with our account details
        AzureFrontendAccountInfo accountInfo = new AzureFrontendAccountInfo();
        accountInfo.AccountKey = AccountKey;
        accountInfo.AccountId = AccountId;
        accountInfo.AccountDomain = AccountDomain;

        arrService.Initialize(accountInfo);
    }

    public void CreateSession()
    {
        CreateFrontend();

        // StartSession will call ARRService_OnSessionStarted once the session becomes available
        arrService.StartSession(new RenderingSessionCreationParams(VMSize, MaxLeaseTimeHours, MaxLeaseTimeMinutes));
    }

    public void StopSession()
    {
        arrService.StopSession();
    }

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);
    }

    private void ARRService_OnSessionErrorEncountered(ARRServiceUnity caller, SessionGeneralContext context)
    {
        Debug.LogError($"Session error encountered. Context: {context.ErrorMessage}. Request Cv: {context.RequestCorrelationVector}. Response Cv: {context.ResponseCorrelationVector}");
    }

    private async void LogSessionStatus(AzureSession session)
    {
        if (session != null)
        {
            var sessionProperties = await session.GetPropertiesAsync().AsTask();
            LogSessionStatus(sessionProperties);
        }
        else
        {
            var sessionProperties = arrService.LastProperties;
            Debug.Log($"Session ended: Id={sessionProperties.Id}");
        }
    }

    private void LogSessionStatus(RenderingSessionProperties sessionProperties)
    {
        Debug.Log($"Session '{sessionProperties.Id}' is {sessionProperties.Status}. Size={sessionProperties.Size}" +
            (!string.IsNullOrEmpty(sessionProperties.Hostname) ? $", Hostname='{sessionProperties.Hostname}'" : "") +
            (!string.IsNullOrEmpty(sessionProperties.Message) ? $", Message='{sessionProperties.Message}'" : ""));
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
    }
#endif
}
```

Este script inicializa Azure Remote Rendering, le indica el objeto de cámara que se va a usar para la representación y coloca un botón **Create Session** (Crear sesión) en la ventanilla, cuando se activa *Play Mode* (Modo de reproducción).

> [!CAUTION]
> Si el script se modifica y se guarda con el modo de reproducción activo en Unity, puede inmovilizar Unity y para cerrarlo tendrá que usar el administrador de tareas. Por lo tanto, detenga siempre el modo de reproducción antes de editar el script *RemoteRendering*.

## <a name="test-azure-remote-rendering-session-creation"></a>Prueba de creación de sesiones de Azure Remote Rendering

Cree un elemento GameObject en la escena y agréguele el componente *RemoteRendering*. Rellene los campos *Account Domain* (Dominio de cuenta), *Account Id* (Id. de cuenta) y *Account Key* (Clave de cuenta) apropiados de su cuenta de Remote Rendering:

![Propiedades de los componentes de Remote Rendering](media/remote-rendering-component.png)

Inicie la aplicación en el editor (**presione Play** [Reproducir] o CTRL + P). Debería ver que el botón **Create Session** (Crear sesión) aparece en la ventanilla. Haga clic en él para iniciar la primera sesión de Azure Remote Rendering:

![Creación de la primera sesión](media/test-create.png)

Si se produce un error, asegúrese de que ha escrito correctamente los detalles de la cuenta en las propiedades del componente RemoteRendering. De lo contrario, aparecerá un mensaje en la ventana de la consola que muestra el identificador de sesión que se le ha asignado y que indique que la sesión está actualmente en el estado *Starting* (Iniciándose):

![Salida de inicio de sesión](media/create-session-output.png)

En este momento, Azure aprovisiona automáticamente un servidor e inicia una máquina virtual de representación remota. Normalmente, esta operación **tarda entre 3 y 5 minutos**. Cuando la máquina virtual está lista, se ejecuta la devolución de llamada de `OnSessionStatusChanged` del script de Unity y se imprimirá el estado de la nueva sesión:

![Salida preparada para sesión](media/create-session-output-2.png)

Eso es todo. Por el momento no pasará nada más. Para evitar cargos, siempre debe detener las sesiones cuando dejen de ser necesarias. En este ejemplo, puede hacer clic en el botón **Stop Session** (Detener Sesión) o detener la simulación de Unity. Debido a la propiedad **Auto-Stop Session** (Sesión de detención automática) del componente *ARRServiceUnity*, que está activado de forma predeterminada, la sesión se detendrá automáticamente. Si nada funciona, debido a bloqueos o problemas de conexión, la sesión puede ejecutarse durante todo el tiempo que dure *MaxLeaseTime* antes de que el servidor lo cierre.

> [!NOTE]
> La acción de detener una sesión tendrá un efecto inmediato y no se puede deshacer. Una vez que se haya detenido, tiene que crear una sesión con la misma sobrecarga de inicio.

## <a name="reusing-sessions"></a>Reutilización de sesiones

Desafortunadamente, la creación de una sesión es una operación que consume mucho tiempo. Por tanto, debe intentar crear sesiones con poca frecuencia y reutilizarlas siempre que sea posible.

Inserte el siguiente código en el script *RemoteRendering* y quite las versiones anteriores de las funciones duplicadas:

```csharp
    public string SessionId = null;

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);

        var status = caller.LastProperties.Status;

        // Capture the session id for the starting/ready session so it can be reused
        if (status == RenderingSessionStatus.Ready || status == RenderingSessionStatus.Starting)
        {
            SessionId = session.SessionUUID;
        }
        // Our session is stopped, expired, or in an error state.
        else
        {
            SessionId = null;
        }
    }

    public async void QueryActiveSessions()
    {
        CreateFrontend();

        var allSessionsProperties = await arrService.Frontend.GetCurrentRenderingSessionsAsync().AsTask();

        Debug.Log($"Number of active sessions: {allSessionsProperties.Length}");

        foreach (var sessionProperties in allSessionsProperties)
        {
            if (string.IsNullOrEmpty(SessionId))
            {
                Debug.Log($"Number of active sessions: {allSessionsProperties.Length}");
                SessionId = sessionProperties.Id;
            }

            LogSessionStatus(sessionProperties);
        }
    }

    public void UseExistingSession()
    {
        CreateFrontend();

        // OpenSession will call ARRService_OnSessionStarted once the session becomes available
        arrService.OpenSession(SessionId);
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
    }
#endif
```

> [!CAUTION]
> Antes de ejecutar este código, asegúrese de desactivar la opción **Auto-Stop Session** (Sesión de detención automática) en el componente RemoteRendering. De lo contrario, todas las sesiones que cree se detendrán automáticamente al detener la simulación y se producirá un error al intentar reutilizarla.

Al presionar *Play* (Reproducir), ahora obtiene tres botones en la ventanilla: **Create Session** (Crear sesión), **Query Active Sessions** (Consultar sesiones activas) y **Use Existing Session** (Usar sesión existente). El primer botón siempre crea una sesión. El segundo botón consulta qué sesiones *activas* existen. Si no especificó manualmente un identificador de sesión para intentar usarlo, esta acción lo seleccionará automáticamente para su uso futuro. El tercer botón intenta conectarse a una sesión existente. Cualquiera que haya especificado manualmente a través de la propiedad del componente *Session Id* (Id. de sesión), o bien que haya encontrado *Query Active Sessions* (Consultar sesiones activas).

La función **AutoStartSessionAsync** se utiliza fuera del editor que se pulsa el botón.

> [!TIP]
> Es posible abrir sesiones que se han detenido, han expirado o están en estado de error. Aunque ya no se pueden usar para la representación, puede consultar sus detalles, una vez que haya abierto una sesión inactiva. El código anterior comprueba el estado de una sesión en `ARRService_OnSessionStarted`, para que se detenga automáticamente cuando la sesión se haya vuelto inutilizable.

Con esta funcionalidad ya puede crear y reutilizar las sesiones, lo que debe mejorar considerablemente el flujo de trabajo de desarrollo.

Normalmente, la creación de sesiones se desencadena fuera de la aplicación cliente debido al tiempo necesario para poner en marcha el servidor.

## <a name="connect-to-an-active-session"></a>Conexión a una sesión activa

Hasta ahora hemos creado o abierto sesiones. El siguiente paso es *conectarse* a una sesión. Una vez que se haya establecido la conexión, el servidor de representación generará imágenes y enviará un flujo de vídeo a nuestra aplicación.

Inserte el siguiente código en el script *RemoteRendering* y quite las versiones anteriores de las funciones duplicadas:

```csharp
    private bool isConnected = false;

    private void ARRService_OnSessionStatusChanged(ARRServiceUnity caller, AzureSession session)
    {
        LogSessionStatus(session);

        var status = caller.LastProperties.Status;

        // Capture the session id for the starting/ready session so it can be reused
        if (status == RenderingSessionStatus.Ready || status == RenderingSessionStatus.Starting)
        {
            SessionId = session.SessionUUID;

            // If our session properties are 'Ready' we are ready to start connecting to the runtime of the service.
            if (status == RenderingSessionStatus.Ready)
            {
                session.ConnectionStatusChanged += AzureSession_OnConnectionStatusChanged;
            }
        }
        // Our session is stopped, expired, or in an error state.
        else
        {
            SessionId = null;
            session.ConnectionStatusChanged -= AzureSession_OnConnectionStatusChanged;
        }
    }

    private void AzureSession_OnConnectionStatusChanged(ConnectionStatus status, Result result)
    {
        Debug.Log($"Connection status: '{status}', result: '{result}'");
        isConnected = (status == ConnectionStatus.Connected);
    }

    public void ConnectSession()
    {
        arrService.CurrentActiveSession?.ConnectToRuntime(new ConnectToRuntimeParams());
    }

    public void DisconnectSession()
    {
        if (isConnected)
        {
            arrService.CurrentActiveSession?.DisconnectFromRuntime();
        }
    }

    private void LateUpdate()
    {
        // The session must have its runtime pump updated.
        // The update will push messages to the server, receive messages, and update the frame-buffer with the remotely rendered content.
        arrService.CurrentActiveSession?.Actions.Update();
    }

    private void OnDisable()
    {
        DisconnectSession();
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }

            if (arrService.LastProperties.Status == RenderingSessionStatus.Ready)
            {
                if (!isConnected)
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Connect"))
                    {
                        ConnectSession();
                    }
                }
                else
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Disconnect"))
                    {
                        DisconnectSession();
                    }
                }
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
        while (arrService.CurrentActiveSession == null)
        {
            yield return null;
        }
        ConnectSession();
    }
#endif
```

Para probar esta funcionalidad:

1. Pulse **Play** (Reproducir) en Unity.
1. Abra una sesión:
    1. Si ya tiene una sesión, pulse **Query Active Sessions** (Consultar sesiones activas) y, después, **Use Existing session** (Usar sesión existente).
    1. De lo contrario, pulse **Create Session** (Crear sesión).
1. Pulse **Connect** (Conectar).
1. A los pocos segundos, la salida de la consola debería imprimir que está conectado.
1. Por ahora, no debería ocurrir nada más.
1. Presione **Disconnect** (Desconectar) o detenga el modo de reproducción de Unity.

>[!NOTE]
> Varios usuarios pueden *abrir* una sesión para consultar su información, pero no puede haber más de uno *conectado* a una sesión a la vez. Si ya está conectado otro usuario, se producirá un **error de protocolo de enlace** en la conexión.

## <a name="load-a-model"></a>Carga de un modelo

Inserte el siguiente código en el script *RemoteRendering* y quite las versiones anteriores de las funciones duplicadas:

```csharp

    public string ModelName = "builtin://Engine";

    private Entity modelEntity = null;
    private GameObject modelEntityGO = null;

#if UNITY_WSA
    private WorldAnchor modelWorldAnchor = null;
#endif

    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();

        // get the game object representation of this entity
        modelEntityGO = modelEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the entity will sync translations with the server
        var sync = modelEntityGO.GetComponent<RemoteEntitySyncObject>();
        sync.SyncEveryFrame = true;

        // set position to an arbitrary distance from the parent
        modelEntityGO.transform.position = Camera.main.transform.position + Camera.main.transform.forward * 2;
        modelEntityGO.transform.localScale = Vector3.one;

#if UNITY_WSA
        // anchor the model in the world
        modelWorldAnchor = modelEntityGO.AddComponent<WorldAnchor>();
#endif

        // load a model that will be parented to the entity
        // We are using the 'from SAS' flavor because that variant can load the built-in model
        var loadModelParams = new LoadModelFromSASParams(ModelName, modelEntity);
        var async = arrService.CurrentActiveSession.Actions.LoadModelFromSASAsync(loadModelParams);
        async.ProgressUpdated += (float progress) =>
        {
            Debug.Log($"Loading: {progress * 100.0f}%");
        };

        await async.AsTask();
    }

    public void DestroyModel()
    {
        if (modelEntity == null)
        {
            return;
        }

#if UNITY_WSA
        DestroyImmediate(modelWorldAnchor);
#endif

        modelEntity.Destroy();
        modelEntity = null;

        DestroyImmediate(modelEntityGO);
    }

    public void DisconnectSession()
    {
        if (isConnected)
        {
            arrService.CurrentActiveSession?.DisconnectFromRuntime();
        }

        DestroyModel();
    }

#if UNITY_EDITOR
    private void OnGUI()
    {
        if (arrService.CurrentActiveSession == null)
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Create Session"))
            {
                CreateSession();
            }

            if (GUI.Button(new Rect(10, 50, 175, 30), "Query Active Sessions"))
            {
                QueryActiveSessions();
            }

            if (!string.IsNullOrEmpty(SessionId))
            {
                if (GUI.Button(new Rect(10, 90, 175, 30), "Use Existing Session"))
                {
                    UseExistingSession();
                }
            }
        }
        else
        {
            if (GUI.Button(new Rect(10, 10, 175, 30), "Stop Session"))
            {
                StopSession();
            }

            if (arrService.LastProperties.Status == RenderingSessionStatus.Ready)
            {
                if (!isConnected)
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Connect"))
                    {
                        ConnectSession();
                    }
                }
                else
                {
                    if (GUI.Button(new Rect(10, 50, 175, 30), "Disconnect"))
                    {
                        DisconnectSession();
                    }

                    if (modelEntity == null)
                    {
                        if (GUI.Button(new Rect(10, 90, 175, 30), "Load Model"))
                        {
                            LoadModel();
                        }
                    }
                    else
                    {
                        if (GUI.Button(new Rect(10, 90, 175, 30), "Destroy Model"))
                        {
                            DestroyModel();
                        }
                    }
                }
            }
        }
    }
#else
    public IEnumerator AutoStartSessionAsync()
    {
        CreateSession();
        while (arrService.CurrentActiveSession == null)
        {
            yield return null;
        }
        ConnectSession();
        while (!isConnected)
        {
            yield return null;
        }
        LoadModel();
    }
#endif
```

Ahora, si presiona el botón de reproducir, abre una sesión y se conecta a ella, aparecerá el botón **Load Model** (Cargar modelo). Tras hacer clic en él, la salida de la consola mostrará el progreso de la carga y cuando llegue al 100 % debería ver que aparece el modelo de un motor:

![Modelo cargado en el editor](media/model-loaded-replace-me.png)

[WorldAnchor](https://docs.unity3d.com/ScriptReference/XR.WSA.WorldAnchor.html) es un componente importante que se usa para la [estabilidad de los hologramas](https://docs.microsoft.com/windows/mixed-reality/hologram-stability). Este componente solo tendrá efecto si se implementa en un dispositivo de Mixed Reality.

> [!TIP]
> Si ha leído [Inicio rápido: Conversión de un modelo para la representación](../../quickstarts/convert-model.md), ya sabrá cómo convertir sus propios modelos. Lo único que necesita hacer ahora para representarlo es colocar el identificador URI para un modelo convertido en la propiedad *Model Name* (Nombre de modelo).

## <a name="display-frame-statistics"></a>Visualización de estadísticas de marcos

Azure Remote Rendering realiza un seguimiento de distintos datos relativos a la calidad de la conexión. Esta es una forma rápida de mostrar esta información:

Cree un [script](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) y asígnele el nombre **RemoteFrameStats**. Abra el archivo de script y reemplace todo su contenido por el código siguiente:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class RemoteFrameStats : MonoBehaviour
{
    public Text FrameStats = null;

    ARRServiceStats arrServiceStats = null;

#if UNITY_EDITOR
    private void OnEnable()
    {
        arrServiceStats = new ARRServiceStats();
    }

    void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            FrameStats.text = string.Empty;
            return;
        }

        arrServiceStats.Update(RemoteManagerUnity.CurrentSession);

        if (FrameStats != null)
        {
            FrameStats.text = arrServiceStats.GetStatsString();
        }
    }
#endif
}
```

Cree una instancia de GameObject y asígnele el nombre *FrameStats*. Asócielo como nodo secundario al objeto *Main Camera* (Cámara principal) y establezca su posición en **x = 0, y = 0, z = 0,325**. Agregue el componente **RemoteFrameStats** al objeto.

Agregue un objeto secundario **UI > Canvas** (UI > Lienzo) al objeto *FrameStats* y establezca sus propiedades de la siguiente manera:

![propiedades del lienzo](media/framestats-canvas.png)

Agregue un objeto **UI > Text** (UI/Texto) como elemento secundario del lienzo y establezca sus propiedades de la siguiente manera:

![propiedades de texto](media/framestats-text.png)

Seleccione el objeto *FrameStats* y rellene el **campo FrameStats**, para lo que debe hacer clic en el icono del círculo y seleccionar el objeto **Text**:

![establecer la propiedad del texto](media/framestats-set-text.png)

Ahora, al conectarse a la sesión remota, el texto debe mostrar las estadísticas del streaming:

![salida de estadísticas de marcos](media/framestats-output.png)

El código deshabilita la actualización de estadísticas fuera del editor, ya que un cuadro de texto con el encabezado bloqueado sería una distracción. En el proyecto del [Inicio rápido](../../quickstarts/render-model.md) puede encontrar una implementación más sofisticada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido todos los pasos necesarios para tomar un proyecto de Unity en blanco y hacer que funcione con Azure Remote Rendering. En el siguiente, veremos cómo trabajar con entidades remotas de forma detallada.

> [!div class="nextstepaction"]
> [Tutorial: Trabajo con entidades remotas en Unity](working-with-remote-entities.md)
