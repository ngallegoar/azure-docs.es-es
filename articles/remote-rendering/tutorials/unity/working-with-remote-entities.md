---
title: Trabajo con entidades remotas en Unity
description: Tutorial que muestra cómo trabajar con entidades de Azure Remote Rendering.
author: florianborn71
ms.author: flborn
ms.date: 02/01/2020
ms.topic: tutorial
ms.openlocfilehash: db1f6a53121e05b29f7e3441af027985a141bc2e
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310208"
---
# <a name="tutorial-working-with-remote-entities-in-unity"></a>Tutorial: Trabajo con entidades remotas en Unity

[Tutorial: Configuración de un proyecto de Unity desde cero](project-setup.md) Aquí se mostró cómo configurar un nuevo proyecto de Unity para trabajar con Azure Remote Rendering. En este tutorial, examinaremos la funcionalidad más común que necesitan todos los usuarios de Azure Remote Rendering.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Seleccionar objetos mediante proyecciones de rayos.
> * Invalidar los estados de los objetos como el color del tono, el estado de la selección y la visibilidad.
> * Eliminar entidades remotas.
> * Mover entidades remotas.
> * Usar planos de corte para examinar el interior de los objetos.

## <a name="prerequisites"></a>Prerrequisitos

* Este tutorial se basa en [Tutorial: Configuración de un proyecto de Unity desde cero](project-setup.md).

> [!TIP]
> El [repositorio de ejemplos de Azure Remote Rendering](https://github.com/Azure/azure-remote-rendering) contiene proyectos de Unity preparados para todos los tutoriales de la carpeta *Unity* que se pueden usar como referencia.

## <a name="pick-objects"></a>Selección de objetos

Queremos interactuar con objetos, por lo que lo primero que necesitamos es seleccionar los objetos que están debajo del cursor del mouse.

Cree un [script](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) llamado **RemoteRaycaster** y reemplace todo su contenido por el código siguiente:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;

[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRaycaster : MonoBehaviour
{
    public double MaxDistance = 30.0;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        arrService = GetComponent<ARRServiceUnity>();
    }

    private async Task<Entity> RemoteRayCast(Vector3 origin, Vector3 dir)
    {
        Entity entity = null;

        var raycast = new RayCast(origin.toRemotePos(), dir.toRemoteDir(), MaxDistance, HitCollectionPolicy.ClosestHit);

        var hits = await arrService.CurrentActiveSession.Actions.RayCastQueryAsync(raycast).AsTask();

        if (hits != null)
        {
            foreach (var hit in hits)
            {
                var hitEntity = hit.HitEntity;
                if (hitEntity == null)
                {
                    continue;
                }

                entity = hitEntity;
                break;
            }
        }

        return entity;
    }

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
        var ray = Camera.main.ScreenPointToRay(position);

        Raycast(ray.origin, ray.direction);
    }

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var entity = await RemoteRayCast(origin, direction);
        if (entity != null)
        {
            Debug.Log("Object Hit: " + entity.Name);
        }
    }
}
```

Agregue este componente al objeto *RemoteRendering* de la escena.

> [!WARNING]
>
> El componente *RemoteRaycaster* requiere asociar un componente *ARRServiceUnity* al mismo objeto. *ARRServiceUnity* es una clase auxiliar para acceder a algunas funciones de Azure Remote Rendering más fácilmente. Sin embargo, solo puede haber una instancia de este componente en la escena. Por lo tanto, asegúrese de agregar todos los componentes que requieren *ARRServiceUnity* al mismo GameObject.
> Si desea acceder a la funcionalidad de Azure Remote Rendering desde varios objetos de juego, agregue el componente *ARRServiceUnity* solo a uno de ellos y haga referencia a él en los otros scripts, o bien acceda directamente a la funcionalidad de Azure Remote Rendering.

Pulse el botón de reproducción, conéctese a una sesión y cargue un modelo. Ahora señale los objetos de la escena y vea la salida de la consola. Debería imprimir el nombre de objeto de cada parte sobre la que mantenga el puntero.

## <a name="highlight-objects"></a>Resalte de objetos

Como paso siguiente, queremos proporcionar comentarios visuales, a qué partes de un modelo apunta el usuario. Para lograrlo, adjuntamos un elemento [HierarchicalStateOverrideComponent](../../overview/features/override-hierarchical-state.md) a la entidad que hemos elegido. Este componente se puede usar para habilitar o deshabilitar varias características en un objeto. Aquí se usa para establecer un color de tono y habilitar la [representación del contorno](../../overview/features/outlines.md).

Cree otro archivo de script llamado **RemoteModelEntity** y reemplace su contenido por el código siguiente:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

public class RemoteModelEntity : MonoBehaviour
{
    public Color HighlightColor = new Color(1.0f, 0.0f, 0.0f, 0.1f);

    public Entity Entity => localSyncObject != null ? localSyncObject?.Entity : null;

    private RemoteEntitySyncObject localSyncObject = null;
    private ARRHierarchicalStateOverrideComponent localStateOverride = null;

    public void OnEnable()
    {
        localSyncObject = GetComponent<RemoteEntitySyncObject>();
        localStateOverride = GetComponent<ARRHierarchicalStateOverrideComponent>();

        if (localStateOverride == null)
        {
            localStateOverride = gameObject.AddComponent<ARRHierarchicalStateOverrideComponent>();
            var remoteStateOverride = localSyncObject.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

            if (remoteStateOverride == null)
            {
                // if there is no HierarchicalStateOverrideComponent on the remote side yet, create one
                localStateOverride.Create(RemoteManagerUnity.CurrentSession);
            }
            else
            {
                // otherwise, bind our local stateOverride component to the remote component
                localStateOverride.Bind(remoteStateOverride);
            }
        }

        localStateOverride.RemoteComponent.TintColor = HighlightColor.toRemote();
    }

    public void OnDisable()
    {
        SetStateOverride(false);

        if (localStateOverride.IsComponentValid)
        {
            localStateOverride.RemoteComponent.Enabled = false;
            localStateOverride.enabled = false;
        }
    }

    private void SetStateOverride(bool on)
    {
        if (localStateOverride.IsComponentValid)
        {
            localStateOverride.RemoteComponent.UseTintColorState = on ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
            localStateOverride.RemoteComponent.SelectedState = on ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
        }
    }

    public void SetFocus(bool on)
    {
        SetStateOverride(on);
    }
}
```
> [!CAUTION]
> No asigne este script a ningún objeto de juego, ya que el código siguiente lo asignará mediante programación.

Después, tenemos que ampliar *RemoteRaycaster* para agregar el componente *RemoteModelEntity* al objeto que acabamos de elegir.

Agregue el siguiente código a la implementación de **RemoteRaycaster** y quite las funciones duplicadas:

```csharp
    private RemoteModelEntity focusedModel = null;

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var remoteEntity = await RemoteRayCast(origin, direction);

        if (focusedModel != null)
        {
            if (focusedModel.Entity == remoteEntity)
            {
                // picked the same object as before
                return;
            }

            ClearFocus();
        }

        if (remoteEntity == null)
        {
            // picked no object at all
            return;
        }

        // get the instance of a Unity GameObject for the ARR entity
        var entityGO = remoteEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the game object has the RemoteModelEntity component
        focusedModel = entityGO.GetComponent<RemoteModelEntity>();
        if (focusedModel == null)
        {
            focusedModel = entityGO.AddComponent<RemoteModelEntity>();
        }

        focusedModel.SetFocus(true);
    }

    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        focusedModel = null;
    }
```

Ejecute el proyecto y apunte a un modelo, debería ver que obtiene un tono rojo y un contorno de selección blanco.

## <a name="isolate-the-selected-object"></a>Aislamiento del objeto seleccionado

Otro uso de [HierarchicalStateOverrideComponent](../../overview/features/override-hierarchical-state.md) es la capacidad de invalidar la visibilidad. Esto le permite aislar un objeto seleccionado del resto del modelo. Abra el script **RemoteModelEntity**, agregue el código siguiente y quite las funciones duplicadas:

```csharp
    private bool isolated = false;
    private HierarchicalStateOverrideComponent parentOverride = null;

    public void ToggleIsolate()
    {
        SetIsolated(!isolated);
    }

    public void SetIsolated(bool on)
    {
        if (localStateOverride == null || !localStateOverride.IsComponentValid || isolated == on)
        {
            return;
        }

        // find the top most parent object that has a HierarchicalStateOverrideComponent
        if (parentOverride == null)
        {
            var modelRoot = transform;

            while (modelRoot.parent != null)
            {
                modelRoot = modelRoot.parent;

                var parentSyncObject = modelRoot.GetComponent<RemoteEntitySyncObject>();

                var stateOverrideComp = parentSyncObject?.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

                if (stateOverrideComp != null)
                {
                    parentOverride = stateOverrideComp;
                }
            }
        }

        if (parentOverride != null)
        {
            isolated = on;

            parentOverride.HiddenState = isolated ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
            localStateOverride.RemoteComponent.HiddenState = isolated ? HierarchicalEnableState.ForceOff : HierarchicalEnableState.InheritFromParent;
        }
    }

    public void SetFocus(bool on)
    {
        SetStateOverride(on);

        if (!on)
        {
            SetIsolated(false);
        }
    }
```

Este código se basa en tener un componente de invalidación de estado en el objeto de nivel superior de la jerarquía, lo que hace que todos los objetos sean invisibles. Luego, vuelve a invalidar la visibilidad en el objeto seleccionado, con el fin de que ese se vea. Por tanto, es necesario crear un componente de invalidación de estado en el objeto raíz.

Abra el script **RemoteRendering** e inserte el código siguiente al principio de la función *LoadModel*:

```csharp
    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();
        arrService.CurrentActiveSession.Actions.CreateComponent(ObjectType.HierarchicalStateOverrideComponent, modelEntity);

        ...
    }
```

Por último, necesitamos una forma de alternar la visibilidad. Abra el script **RemoteRaycaster** y reemplace la función *Update*:

```csharp
    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        var ray = Camera.main.ScreenPointToRay(Input.mousePosition);

        Raycast(ray.origin, ray.direction);

        if (Input.GetMouseButtonDown(1) && focusedModel != null)
        {
            focusedModel.ToggleIsolate();
        }
    }
```

Ejecute el código y haga clic con el botón derecho en una parte del modelo. El resto del modelo desaparecerá y solo permanecerá visible la parte resaltada.

## <a name="remove-gameobject-instances-of-remote-entities"></a>Eliminación de instancias de GameObject de entidades remotas

Es posible que haya observado que el código crea constantemente objetos, pero nunca los limpia. Esto es algo que también se ve en el panel de jerarquía de objetos. Al expandir la jerarquía de objetos remotos durante la simulación, se pueden ver que aparecen más objetos cada vez que se mantiene el puntero del mouse sobre una nueva parte del modelo.

Tener muchos objetos en una escena afecta negativamente al rendimiento. Siempre debe limpiar los objetos que no sean necesarios.

Inserte el código siguiente en el script **RemoteRaycaster** y quite las funciones duplicadas:

```csharp
    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        CleanHierarchy(focusedModel.gameObject);
        focusedModel = null;
    }

    private void CleanHierarchy(GameObject focusedGO)
    {
        var sync = focusedGO?.GetComponent<RemoteEntitySyncObject>();
        if (sync == null || !sync.IsEntityValid)
        {
            return;
        }

        sync.Entity.DestroyGameObject(EntityExtensions.DestroyGameObjectFlags.DestroyEmptyParents | EntityExtensions.DestroyGameObjectFlags.KeepRemoteRoot);
    }
```

## <a name="move-objects"></a>Movimiento de objetos

Como paso siguiente queremos mover un objeto seleccionado. En el script **RemoteRaycaster**, inserte este código y quite la función duplicada:

```csharp
    private Vector3 lastPosition = Vector3.zero;

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        if (Input.GetMouseButton(0))
        {
            if (focusedModel)
            {
                // note: mousePosition is in 2D screen-space coordinates and has no relation with
                // the 3D object position. This just happens to work good enough for demonstration.
                var delta = Input.mousePosition - lastPosition;
                focusedModel.transform.position += delta * Time.deltaTime;
            }
        }
        else
        {
            Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
            var ray = Camera.main.ScreenPointToRay(position);

            Raycast(ray.origin, ray.direction);

            if (Input.GetMouseButtonDown(1) && focusedModel != null)
            {
                focusedModel.ToggleIsolate();
            }
        }

        lastPosition = Input.mousePosition;
    }
```

> [!IMPORTANT]
> Si ejecuta este código, observará que no sucede nada. Esto se debe a que el cambio de la transformación de un objeto no sincroniza automáticamente el cambio de estado en el servidor, por motivos de rendimiento. En su lugar, tendrá que insertar este cambio de estado en el servidor manualmente, o bien habilitar **SyncEveryFrame** en el componente *RemoteEntitySyncObject*.

Abra el script **RemoteModelEntity** y agregue esta línea:

```csharp
    public void OnEnable()
    {
        ...

        localSyncObject.SyncEveryFrame = true;
    }
```

Si vuelve a ejecutar el código, debería poder hacer clic en un objeto y arrastrarlo.

## <a name="add-a-cut-plane"></a>Incorporación de un plano de corte

La característica final que queremos probar en este tutorial es el uso de [planos de corte](../../overview/features/cut-planes.md). Un plano de corte corta partes de los objetos representados para que se pueda mirar dentro de ellos.

Cree un elemento GameObject en el **CutPlane** de la escena. Cree un script y llámelo **RemoteCutPlane**. Agregue el componente al nuevo elemento GameObject.

Abra el archivo de script y reemplace su contenido por el código siguiente:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RemoteCutPlane : MonoBehaviour
{
    private ARRCutPlaneComponent localCutPlaneComponent = null;
    private RemoteEntitySyncObject remoteEntitySync = null;

    void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            // can't do anything while we are not connected
            return;
        }

        if (localCutPlaneComponent == null)
        {
            localCutPlaneComponent = gameObject.CreateArrComponent<ARRCutPlaneComponent>(RemoteManagerUnity.CurrentSession);
        }

        if (remoteEntitySync == null)
        {
            remoteEntitySync = gameObject.GetComponent<RemoteEntitySyncObject>();
            remoteEntitySync.SyncEveryFrame = true;
        }

        localCutPlaneComponent.RemoteComponent.Normal = Axis.X;
        localCutPlaneComponent.RemoteComponent.FadeLength = 0.025f;
        localCutPlaneComponent.RemoteComponent.FadeColor = new Color4Ub(255, 128, 0, 255);
        localCutPlaneComponent.RemoteComponent.Enabled = true;
    }

    void OnDisable()
    {
        if (localCutPlaneComponent && localCutPlaneComponent.IsComponentValid)
        {
            localCutPlaneComponent.RemoteComponent.Enabled = false;
        }

        if (remoteEntitySync && remoteEntitySync.IsEntityValid)
        {
            remoteEntitySync.SyncEveryFrame = false;
        }
    }
}
```

Al ejecutar el código debería ver cómo se corta y se abre el modelo por el plano. Puede seleccionar el objeto *CutPlane* y moverlo y girarlo en la ventana *Scene* (Escena). Para activar y desactivar el plano de corte, debe deshabilitar el objeto de plano de corte.

## <a name="next-steps"></a>Pasos siguientes

Ya conoce la funcionalidad más importante para interactuar con objetos remotos. En el siguiente tutorial, se examinará cómo se personaliza el aspecto de una escena.

> [!div class="nextstepaction"]
> [Tutorial: Cambio del entorno y los materiales](changing-environment-and-materials.md)
