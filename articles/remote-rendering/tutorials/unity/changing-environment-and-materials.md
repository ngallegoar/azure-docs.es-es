---
title: Cambio del entorno y los materiales
description: Tutorial que muestra cómo modificar el mapa del cielo y los materiales de los objetos en una escena de Unity.
author: jakrams
ms.author: jakras
ms.date: 02/03/2020
ms.topic: tutorial
ms.openlocfilehash: 1354ac3cf2c6fc716ac72ae339928fa49171893e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678686"
---
# <a name="tutorial-changing-the-environment-and-materials"></a>Tutorial: Cambio del entorno y los materiales

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Cambiar el mapa del entorno de una escena.
> * Modificar los parámetros de los materiales.
> * Cargar texturas personalizadas.

## <a name="prerequisites"></a>Prerrequisitos

En este tutorial se da por supuesto que ya ha leído [Tutorial: Trabajo con entidades remotas en Unity](working-with-remote-entities.md). Sin embargo, solo necesita un proyecto de Unity con el que pueda conectarse a sesiones y cargar un modelo, como se muestra en [Tutorial: Configuración de un proyecto de Unity desde cero](project-setup.md).

> [!TIP]
> El [repositorio de ejemplos de Azure Remote Rendering](https://github.com/Azure/azure-remote-rendering) contiene proyectos de Unity preparados para todos los tutoriales de la carpeta *Unity* que se pueden usar como referencia.

## <a name="change-the-environment-map"></a>Cambio del mapa del entorno

Azure Remote Rendering admite el uso de [cuadros de cielo](../../overview/features/sky.md) (llamados también "mapas de entorno") para simular la iluminación ambiente. Esto es especialmente útil cuando los objetos usan la *[representación basada en la física](../../overview/features/pbr-materials.md)* , como hacen nuestros modelos de ejemplo. Azure Remote Rendering también incluye varias texturas del cielo integradas que se utilizarán en este tutorial.

Cree un script denominado **RemoteSky** y agréguelo a un nuevo elemento GameObject. Abra el archivo de script y reemplácelo por el código siguiente:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RemoteSky : MonoBehaviour
{
    private int skyIndex = 0;

    private LoadTextureFromSASParams[] builtIns =
    {
        new LoadTextureFromSASParams("builtin://Autoshop", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://BoilerRoom", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://ColorfulStudio", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Hangar", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://IndustrialPipeAndValve", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Lebombo", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://SataraNight", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://SunnyVondelpark", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://Syferfontein", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://TearsOfSteelBridge", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://WhippleCreekRegionalPark", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://WinterRiver", TextureType.CubeMap),
        new LoadTextureFromSASParams("builtin://DefaultSky", TextureType.CubeMap)
    };

    public async void ToggleSky()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        skyIndex = (skyIndex + 1) % builtIns.Length;

        var texture = await RemoteManagerUnity.CurrentSession.Actions.LoadTextureFromSASAsync(builtIns[skyIndex]).AsTask();

        Debug.Log($"Switching sky to: {builtIns[skyIndex].TextureId}");

        var settings = RemoteManagerUnity.CurrentSession.Actions.SkyReflectionSettings;

        settings.SkyReflectionTexture = texture;
    }

    private void OnGUI()
    {
        if (RemoteManagerUnity.IsConnected)
        {
            if (GUI.Button(new Rect(10, Screen.height - 50, 175, 30), "Toggle Sky"))
            {
                ToggleSky();
            }
        }
    }
}
```

Tenga en cuenta que se usa la variante `LoadTextureFromSASAsync` se usa porque se cargan texturas integradas. Si la carga se realiza desde [almacenamientos de blobs vinculados](../../how-tos/create-an-account.md#link-storage-accounts), utilice la variante `LoadTextureAsync`. Un ejemplo de cómo funciona todo esto para los modelos se puede encontrar en la [sección de carga de modelos](../../concepts/models.md#loading-models).

Al ejecutar el código y alternar los distintos mapas del cielo, observará una iluminación completamente diferente en el modelo. Sin embargo, el fondo permanecerá en negro y no podrá ver la textura real del cielo. Esto no es casualidad, ya que la representación de un fondo sería un elemento que provocaría distracción en un dispositivo de realidad aumentada. Para aplicarlo correctamente, debería utilizar texturas del cielo que sean similares a las del entorno en el mundo real, ya que esto ayudará a que los objetos parezcan más reales.

## <a name="modify-materials"></a>Modificación de los materiales

En el tutorial anterior, usamos [componentes de invalidación de estado](../../overview/features/override-hierarchical-state.md) para cambiar el color del tono de los objetos seleccionados. Ahora queremos lograr un efecto similar, pero modificando el [material](../../concepts/materials.md) de un objeto.

En primer lugar, necesitamos un script que elija los objetos, como hicimos en [el segundo tutorial](working-with-remote-entities.md). Si aún no tiene un script **RemoteRaycaster**, créelo ahora. Sustituya su contenido por el código siguiente:

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
}
```

Agregue el componente al objeto de juego *RemoteRendering*. Es responsable de seleccionar los objetos que están bajo el mouse y de agregar componentes de *RemoteModelEntity* a los objetos seleccionados. Esa clase de componente es donde implementamos la funcionalidad de cambio de material real.

Si aún no tiene un script **RemoteModelEntity**, créelo y reemplace su contenido por este código:

```csharp
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

// to prevent namespace conflicts
using ARRTexture = Microsoft.Azure.RemoteRendering.Texture;

public class RemoteModelEntity : MonoBehaviour
{
    private Color HighlightColor = new Color(1.0f, 0.4f, 0.1f, 1.0f);

    public Entity Entity => localSyncObject != null ? localSyncObject?.Entity : null;
    private RemoteEntitySyncObject localSyncObject = null;
    private ARRMeshComponent meshComponent = null;
    private Color4 originalColor;

    public void OnEnable()
    {
        localSyncObject = GetComponent<RemoteEntitySyncObject>();

        meshComponent = GetComponent<ARRMeshComponent>();
        if (meshComponent == null)
        {
            var mesh = localSyncObject.Entity.FindComponentOfType<MeshComponent>();
            if (mesh != null)
            {
                gameObject.BindArrComponent<ARRMeshComponent>(mesh);
                meshComponent = gameObject.GetComponent<ARRMeshComponent>();
            }
        }
        meshComponent.enabled = true;
    }

    public void SetFocus(bool on)
    {
        SetMaterialColor(on);
    }

    private void SetMaterialColor(bool on)
    {
        if (!meshComponent.IsComponentValid)
        {
            return;
        }

        // get the list of materials from the picked mesh
        // we only modify the first material; depending on how the model is authored, this may
        // work exactly as desired, or not ;-)
        var materials = meshComponent.RemoteComponent.Mesh.Materials;
        if (materials == null || materials.Count == 0)
        {
            return;
        }

        // there are different types of materials, cast as necessary
        if (materials[0].MaterialSubType == MaterialType.Color)
        {
            var materialColor = materials[0] as ColorMaterial;

            if (on)
            {
                // modify the color of the material
                originalColor = materialColor.AlbedoColor;
                materialColor.AlbedoColor = HighlightColor.toRemoteColor4();
            }
            else
            {
                materialColor.AlbedoColor = originalColor;
            }
        }
        else if (materials[0].MaterialSubType == MaterialType.Pbr)
        {
            var materialColor = materials[0] as PbrMaterial;

            if (on)
            {
                // modify the color of the material
                originalColor = materialColor.AlbedoColor;
                materialColor.AlbedoColor = HighlightColor.toRemoteColor4();
            }
            else
            {
                materialColor.AlbedoColor = originalColor;
            }
        }
    }
}
```

Al ejecutar este código, se resaltan los objetos a medida que se desplaza con el mouse. El efecto es similar a lo que hicimos en el tutorial 2, pero varía la forma en que lo logramos. Aquí primero obtenemos la lista de materiales del objeto seleccionado y, después, modificamos el primero para que tenga otro color albedo.

> [!IMPORTANT]
> Tenga en cuenta que si este método resalta las partes correctas de un modelo, depende del modo en que este se cree. Funcionará perfectamente si cada objeto usa exactamente un material. Sin embargo, si el modelo no tiene una relación 1:1 entre partes y materiales, el código de Naive anterior no funcionará correctamente.

## <a name="use-a-different-texture"></a>Uso de una textura diferente

Las [texturas](../../concepts/textures.md) suelen formar parte de un modelo de origen. Durante la [conversión de modelos](../../quickstarts/convert-model.md), todas las texturas se convierten al formato de runtime necesario y se empaquetan en el archivo de modelo final. Para reemplazar una textura en el runtime, es preciso guardarla en [formato de archivo DDS](https://en.wikipedia.org/wiki/DirectDraw_Surface) y cargarla en Azure Blob Storage. Consulte en [esta guía de inicio rápido](../../quickstarts/convert-model.md) cómo crear un contenedor de blobs de Azure. Una vez que tenga un contenedor de blobs, puede abrirlo en el Explorador de Azure Storage y usar el método de arrastrar y colocar para cargar el archivo.

En el lado del runtime, hay dos formas distintas de dirigirse a un recurso de textura en Blob Storage:

* Diríjase a la textura por su identificador URI de SAS. Para ello, haga clic con el botón derecho en el archivo cargado y seleccione "**Obtener firma de acceso compartido...** " en el menú contextual. Use este identificador URI de SAS con la variante de la función `LoadTextureFromSASAsync` (consulte el código de ejemplo siguiente).
* Diríjase directamente la textura mediante parámetros de Blob Storage, en caso de que [Blob Storage esté vinculado a la cuenta](../../how-tos/create-an-account.md#link-storage-accounts). En este caso, la función de carga pertinente es `LoadTextureAsync`.

Ahora abra el script **RemoteModelEntity**, agregue el código siguiente y quite las funciones duplicadas:

```csharp
    private string textureFile = ""; //<SAS URI for your texture>
    private ARRTexture originalTexture = null;

    private async void SetMaterialTexture(bool on)
    {
        if (!meshComponent.IsComponentValid)
        {
            return;
        }

        var materials = meshComponent.RemoteComponent.Mesh.Materials;
        if (materials == null || materials.Count == 0)
        {
            return;
        }

        ARRTexture newTexture = originalTexture;

        if (on)
        {
            var textureParams = new LoadTextureFromSASParams(textureFile, TextureType.Texture2D);

            newTexture = await RemoteManagerUnity.CurrentSession.Actions.LoadTextureFromSASAsync(textureParams).AsTask();
        }

        if (materials[0].MaterialSubType == MaterialType.Color)
        {
            var materialColor = materials[0] as ColorMaterial;

            originalTexture = materialColor.AlbedoTexture;
            materialColor.AlbedoTexture = newTexture;
        }
        else if (materials[0].MaterialSubType == MaterialType.Pbr)
        {
            var materialColor = materials[0] as PbrMaterial;

            originalTexture = materialColor.AlbedoTexture;
            materialColor.AlbedoTexture = newTexture;
        }
    }

    public void SetFocus(bool on)
    {
        if (string.IsNullOrEmpty(textureFile))
        {
            SetMaterialColor(on);
        }
        else
        {
            SetMaterialTexture(on);
        }
    }
```

Ejecute este código y mantenga el puntero sobre el modelo. Si el modelo tiene unas coordenadas de UV adecuadas, debería ver que aparece la textura. De lo contrario, es posible que solo perciba un cambio de color.

## <a name="next-steps"></a>Pasos siguientes

Así concluye nuestra serie introductoria sobre cómo usar Azure Remote Rendering con Unity. Como paso siguiente, debe familiarizarse con algunos conceptos fundamentales de Azure Remote Rendering, como [sesiones](../../concepts/sessions.md), [entidades](../../concepts/entities.md) y [modelos](../../concepts/models.md) para conocerlo mejor. También hay varias características, como [luces](../../overview/features/lights.md), [representación del contorno](../../overview/features/outlines.md), [invalidaciones de estado jerárquicas](../../overview/features/override-hierarchical-state.md) y [materiales](../../concepts/materials.md) que debe explorar con más detalle.

> [!div class="nextstepaction"]
> [Interacción con componentes y objetos de juegos de Unity](../../how-tos/unity/objects-components.md)
