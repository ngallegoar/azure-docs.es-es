---
title: 'Tutorial: Creación de una nueva aplicación de HoloLens Unity'
description: En este tutorial, aprenderá a crear una aplicación de HoloLens Unity mediante Azure Spatial Anchors.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 07/05/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: e1abb759c80e770f1e650c232b6b2e21232b7e6f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75457730"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-hololens-unity-app-using-azure-spatial-anchors"></a>Tutorial: instrucciones paso a paso para crear una aplicación de HoloLens Unity mediante Azure Spatial Anchors

En este tutorial se muestra cómo crear una aplicación de HoloLens Unity con Azure Spatial Anchors.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, asegúrese de disponer de los siguientes elementos:

1. Una máquina Windows con <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 o superior</a> instalado, con la carga de trabajo **Desarrollo de la plataforma universal de Windows** y el componente **Windows 10 SDK (10.0.18362.0 o versiones posteriores)** y <a href="https://git-scm.com/download/win" target="_blank">Git para Windows</a>.
2. La [extensión de Visual Studio para C++/WinRT (VSIX)](https://aka.ms/cppwinrt/vsix) para Visual Studio instalada desde [Visual Studio Marketplace](https://marketplace.visualstudio.com/).
3. Un dispositivo HoloLens con el [modo de desarrollador](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) habilitado. Este artículo requiere un dispositivo HoloLens con la [actualización de octubre de 2018 de Windows 10](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (también conocida como RS5). Para actualizar a la versión más reciente en HoloLens, abra la aplicación **Settings** (Configuración), vaya a **Update & Security** (Actualización y seguridad) y, a continuación, seleccione el botón **Check for updates** (Buscar actualizaciones).

## <a name="getting-started"></a>Introducción

Lo primero es configurar el proyecto y la escena de Unity:
1. Inicie Unity.
2. Seleccione **Nuevo**.
4. Asegúrese de que la opción **3D** está seleccionada.
5. Asigne un nombre al proyecto y escriba una **ubicación** donde guardarlo.
6. Haga clic en **Crear proyecto**.
7. Guarde la escena predeterminada vacía en un nuevo archivo con: **File** > **Save As** (Archivo > Guardar como).
8. Asigne a la nueva escena el nombre **Principal** y presione el botón **Save** (Guardar).

**Configuración del proyecto**

A continuación, se van a establecer algunos valores de configuración del proyecto de Unity que nos ayuden a trabajar con el SDK de Windows Holographic para el desarrollo.

En primer lugar, vamos a ver la configuración de calidad de nuestra aplicación.
1. Seleccione **Edit** > **Project Settings** > **Quality** (Editar > Configuración del proyecto > Calidad).
2. En la columna que aparece bajo el logotipo de **Windows Store** (Tienda Windows), haga clic en la flecha situada en la fila **Default** (Predeterminado) y seleccione **Very Low** (Muy baja). Sabrá que la configuración se aplica correctamente cuando el cuadro de la columna **Windows Store** (Tienda Windows) y la fila **Very Low** (Muy baja) estén en verde.

Es necesario informar a Unity que la aplicación que estamos intentando exportar debe crear una vista envolvente en lugar de una vista 2D. Crearemos una vista envolvente, para lo cual habilitaremos la compatibilidad con Virtual Reality en Unity con destino el SDK de Windows 10.

1. Vaya a **Edit** > **Project Settings** > **Player** (Editar > Configuración del proyecto > Reproductor).
2. En **Inspector Panel** (Panel de inspector) de **Player Settings** (Configuración del Reproductor), seleccione el icono de **Windows Store** (Tienda Windows).
3. Expanda el grupo **XR Settings** (Configuración de XR).
4. En la sección **Rendering** (Representación), active la casilla **Virtual Reality Supported** (Se admite Virtual Reality) para agregar una nueva lista del **SDK de Virtual Reality**.
5. Compruebe que **Windows Mixed Reality** (Mixed Reality de Windows) aparece en la lista. En caso contrario, seleccione el botón **+** situado en la parte inferior de la lista y elija **Windows Mixed Reality** (Mixed Reality de Windows).

> [!NOTE]
> Si no ve el icono de la Tienda Windows, asegúrese bien de que ha seleccionado el back-end de scripting de .NET de la Tienda Windows. Si no, es posible que deba volver a instalar Unity con la instalación correcta de Windows.

**Verificar la configuración de back-end de script**
1. Vaya a **Edit** > **Project Settings** > **Player** (Editar > Configuración del proyecto > Reproductor) (puede que aún tenga abierto el **Reproductor** del paso anterior).
2. En **Inspector Panel** (Panel de inspector) de **Player Settings** (Configuración del Reproductor), seleccione el icono de **Windows Store** (Tienda Windows).
3. En la sección de configuración **Other Settings** (Otra configuración), asegúrese de que **Scripting Backend** (Back-end de scripting) esté establecido en **IL2CPP**.

**Establecimiento de funcionalidades**
1. Vaya a **Edit** > **Project Settings** > **Player** (Editar > Configuración del proyecto > Reproductor) (puede que aún tenga abierto el **Reproductor** del paso anterior).
2. En **Inspector Panel** (Panel de inspector) de **Player Settings** (Configuración del Reproductor), seleccione el icono de **Windows Store** (Tienda Windows).
3. En la sección de configuración **Publishing Settings** (Configuración de publicación), active **InternetClientServer** y **SpatialPerception**.

**Configuración de la cámara virtual principal**
1. En **Hierarchy Panel** (Panel de jerarquía), seleccione **Main Camera** (Cámara principal).
2. En **Inspector**, establezca su posición de transformación en **0,0,0**.
3. Busque la propiedad **Clear Flags** (Borrar marcas) y cambie la lista desplegable de **Skybox** a **Solid Color** (Color sólido).
4. Haga clic en el campo **Background** (Fondo) para abrir un selector de colores.
5. Establezca **R, G, B, and A** (R, G, B y A) en **0**.
6. Seleccione **Add Component** (Agregar componente) y busque **Spatial Mapping Collider**.

**Creación del script**
1. En el panel **Project** (Proyecto), cree una carpeta, **Scripts**, en la carpeta **Assets** (Activos).
2. Haga clic con el botón derecho en la carpeta y seleccione **Create (Crear) >** , **C# Script** (Script de C#). Asígnele el nombre **AzureSpatialAnchorsScript**.
3. Vaya a **GameObject** -> **Create Empty** (Crear vacío).
4. Selecciónelo y, en **Inspector**, cambie su nombre de **GameObject** a **MixedRealityCloud**. Seleccione **Add Component** (Agregar componente) y agregue **AzureSpatialAnchorsScript**.

**Creación del objeto prefabricado de esfera**
1. Vaya a **GameObject** -> **3D Object** -> **Sphere** (GameObject > Objeto 3D > Esfera).
2. En el **Inspector**, establezca su escala en **0,25, 0,25, 0,25**.
3. Buscar el objeto **Sphere** en el panel de **jerarquía**. Haga clic en él y arrástrelo a la carpeta **Assets** carpeta en el panel **Project** (Proyecto).
4. Haga clic con el botón derecho y elija **Eliminar** la esfera original que ha creado en el panel **Hierarchy** (Jerarquía).

Ahora debería tener una esfera prefabricada en su panel **Project** (Proyecto).

## <a name="trying-it-out"></a>Prueba
Para probar que todo funciona, compile la aplicación en **Unity** e impleméntela desde **Visual Studio**. Para ello, siga el capítulo 6 del curso [**MR Basics 100: Introducción a Unity**](https://docs.microsoft.com/windows/mixed-reality/holograms-100#chapter-6---build-and-deploy-to-device-from-visual-studio). Verá la pantalla de inicio de Unity y, luego, una opción para borrar la pantalla.

## <a name="place-an-object-in-the-real-world"></a>Colocación de un objeto en el mundo real
Vamos a crear y colocar un objeto mediante la aplicación. Abra la solución de Visual Studio que se creó cuando se [implementó nuestra aplicación](#trying-it-out).

En primer lugar, agregue las importaciones siguientes a `Assembly-CSharp (Universal Windows)\Scripts\AzureSpatialAnchorsScript.cs`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=19-24)]

Luego, agregue las siguientes variables de miembro a la clase `AzureSpatialAnchorsScript`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=26-42,48-52,60-79)]

Antes de continuar, necesitamos establecer la esfera prefabricada que creamos en nuestra variable miembro de spherePrefab. Vuelva a **Unity**.
1. En **Unity**, seleccione el objeto **MixedRealityCloud** en el panel **Hierarchy** (Jerarquía).
2. Haga clic en el objeto prefabricado **Sphere** que guardó en el panel de **proyecto**. Arrastre la **esfera** en la que hizo clic en el área del **objeto prefabricado esfera** bajo **Azure Spatial Anchors Script (Script)** en el panel **Inspector**.

Ahora debería tener el objeto **Sphere** establecido como prefabricado en el script. Compílelos en **Unity** y, luego, abra de nuevo la solución de **Visual Studio** resultante, como ha hecho en [Prueba](#trying-it-out).

En **Visual Studio**, abra el archivo `AzureSpatialAnchorsScript.cs`. Agregue el código siguiente al método `Start()`. Este código enlazará `GestureRecognizer`, que detectará cuándo hay una pulsación en el aire y llamará a `HandleTap`.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-90,93&highlight=4-10)]

Ahora es necesario agregar el siguiente método `HandleTap()` debajo de `Update()`. Realizará un lanzamiento de rayos (ray casting) y obtendrá un punto de posicionamiento en el que colocar una esfera.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-277,299-300,304-312)]

Ahora debemos crear la esfera. La esfera inicialmente será blanca, pero este valor se ajustará más adelante. Agregue el siguiente método `CreateAndSaveSphere()`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-325,390)]

Ejecute la aplicación desde **Visual Studio** para validarla una vez más. Luego, toque la pantalla para crear la esfera blanca y colocarla sobre la superficie que haya elegido.

## <a name="set-up-the-dispatcher-pattern"></a>Configuración del patrón de distribuidor

Cuando se trabaja con Unity, todas las API de Unity, por ejemplo, las API que se usan para realizar actualizaciones de la interfaz de usuario, deben tener lugar en el subproceso principal. Sin embargo, en el código que vamos a escribir, obtendremos devoluciones de llamada en otros subprocesos. Queremos actualizar la interfaz de usuario en estas devoluciones de llamada, por lo que necesitamos una manera de pasar de un subproceso secundario al subproceso principal. Para ejecutar código en el subproceso principal desde un subproceso secundario, vamos a usar el patrón de distribuidor.

Agregaremos una variable de miembro, dispatchQueue, que es una cola de acciones. Insertaremos acciones en la cola y, luego, las quitaremos de la cola y ejecutaremos las acciones en el subproceso principal.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=38-51&highlight=6-9)]

Luego, agregaremos una manera de incorporar una acción a la cola. Agregaremos `QueueOnUpdate()` justo después de `Update()`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=107-117)]

Ahora vamos a usar el bucle Update() para comprobar si hay una acción en cola. Si es así, la quitaremos de la cola y la ejecutaremos.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=95-105&highlight=4-10)]

## <a name="get-the-azure-spatial-anchors-sdk"></a>Obtención del SDK de Azure Spatial Anchors

## <a name="via-unity-package"></a>[Mediante el paquete de Unity](#tab/UnityPackage)

Ahora, descargaremos el SDK de Azure Spatial Anchors. Vaya a la [página de versiones de GitHub de Azure Spatial Anchors](https://github.com/Azure/azure-spatial-anchors-samples/releases). En Assets (Recursos), descargue **AzureSpatialAnchors.unitypackage**. En Unity, vaya a **Assets** (Activos), haga clic en **Import Package** > **Custom Package...** (Importar paquete > Paquete personalizado...). Vaya al paquete y seleccione **Open** (Abrir).

En la nueva ventana **Import Unity Package** (Importar paquete de Unity) que aparece, anule la selección de **Plugins** (Complementos) y, luego, haga clic en **Import** (Importar) en la esquina inferior derecha.

Ahora, es necesario restaurar los paquetes de NuGet para obtener el SDK de Azure Spatial Anchors. Compílelos en **Unity** y, luego, abra y compile de nuevo la solución de **Visual Studio** resultante, como se detalla en [Prueba](#trying-it-out).

## <a name="via-nugetforunity"></a>[Mediante NuGetForUnity](#tab/NuGetForUnity)

En primer lugar, es necesario instalar NuGetForUnity. Vaya a la [página de versiones de GitHub de NuGetForUnity](https://github.com/GlitchEnzo/NuGetForUnity/releases). En Assets (Recursos), descargue el archivo **NuGetForUnity.unitypackage** más reciente. En Unity, vaya a **Assets** (Activos), haga clic en **Import Package** > **Custom Package...** (Importar paquete > Paquete personalizado...). Vaya al paquete y seleccione **Open** (Abrir). Unity instalará ahora NugetForUnity. Si no ve una nueva lista desplegable **NuGet** en Unity, puede que tenga que hacer clic con el botón derecho en **Projects** > **Assets** (Proyectos > Recursos). A continuación, seleccione **Reimport All** (Volver a importar todo).

Cuando haya instalado NuGetForUnity, seleccione **NuGet** > **Manage NuGet Packages** (Administrar paquetes NuGet). Luego, busque Microsoft.Azure.SpatialAnchors.Unity y seleccione **Install** (Instalar).

Ahora tenemos que realizar la compilación para obtener el SDK de Azure Spatial Anchors, dado que el paquete NuGet que se acaba de descargar solo contiene scripts de aplicación auxiliar. Compílelos en **Unity** y, luego, abra y compile de nuevo la solución de **Visual Studio** resultante, como se detalla en [Prueba](#trying-it-out).

---

En la solución de **Visual Studio**, agregue la siguiente importación a `<ProjectName>\Assets\Scripts\AzureSpatialAnchorsScript.cs`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=18-21&highlight=1)]

Luego, agregue las siguientes variables de miembro a la clase `AzureSpatialAnchorsScript`:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=48-63&highlight=6-11)]

## <a name="attach-a-local-azure-spatial-anchor-to-the-local-anchor"></a>Asociación de una instancia local de Azure Spatial Anchors con un anclaje local

Vamos a configurar CloudSpatialAnchorSession de Azure Spatial Anchors. Para comenzar, agregaremos el siguiente método `InitializeSession()` dentro de su clase `AzureSpatialAnchorsScript`. Una vez que se le llama, garantiza que se crea una sesión de Azure Spatial Anchors y que se inicializa correctamente durante el inicio de la aplicación.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=174-202,205-209)]

Ahora es necesario escribir código para controlar las llamadas de delegado. Les agregaremos más código mientras continuamos.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=211-226)]

Ahora, vamos a enlazar el método `initializeSession()` a su método `Start()`.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-93&highlight=12)]

Por último, agregue el código siguiente al método `CreateAndSaveSphere()`. Se asociará una instancia local de Azure Spatial Anchors a la esfera que vamos a colocar en el mundo real.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-338,390&highlight=14-25)]

Antes de continuar, será preciso crear el identificador y la clave de la cuenta de Azure Spatial Anchors, en caso de que no los tenga aún. En la sección siguiente se explica cómo obtenerlos.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Carga del anclaje local en la nube

Una vez que tenga el identificador y la clave de la cuenta de Azure Spatial Anchors, vaya y pegue el valor de `Account Id` en `SpatialAnchorsAccountId` y el valor de `Account Key` en `SpatialAnchorsAccountKey`.

Por último, vamos a enlazar todos los elementos. En el método `SpawnNewAnchoredObject()`, agregue la siguiente línea. Esta línea invocará el método `CreateAnchorAsync()` en cuanto se cree la esfera. Una vez que el método realice la devolución, el código siguiente realizará una actualización final a la esfera y su color cambiará a azul.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-391&highlight=26-77)]

Ejecute la aplicación desde **Visual Studio** una vez más. Mueva la cabeza y, luego, pulse en el aire para colocar la esfera. Una vez que tengamos suficientes fotogramas, la esfera pasará a ser amarilla y se iniciará la carga en la nube. Cuando finalice la carga, la esfera pasará a ser azul. Opcionalmente, también podría usar la ventana de salida dentro de **Visual Studio** para supervisar los mensajes de registro que envía la aplicación. Podrá ver el progreso de creación recomendado y el identificador de anclaje que devuelve la nube una vez finalizada la carga.

> [!NOTE]
> Si aparece la excepción "DllNotFoundException: No se puede cargar el archivo DLL "AzureSpatialAnchors": No se ha podido encontrar el módulo especificado", debe seleccionar **Clean** (Limpiar) y **Build** (Compilar) para limpiar y volver a compilar la solución.

## <a name="locate-your-cloud-spatial-anchor"></a>Búsqueda del anclaje espacial de la nube

Una vez que el anclaje se carga en la nube, ya es posible volver a intentar localizarlo. Vamos a agregar el siguiente código al método `HandleTap()`. Este código hará lo siguiente:

* Llame a `ResetSession()`, que detendrá `CloudSpatialAnchorSession` y quitará la esfera azul existente de la pantalla.
* Inicialice de nuevo `CloudSpatialAnchorSession`. Esta acción garantiza que el anclaje que vamos a buscar proviene de la nube, y no es el anclaje local que hemos creado.
* Cree un elemento **Watcher** (Monitor) que buscará el anclaje que hemos cargado en Azure Spatial Anchors.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-305&highlight=13-31,35-36)]

Ahora agreguemos nuestros métodos `ResetSession()` y `CleanupObjects()`. Puede colocarlos debajo de `QueueOnUpdate()`.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=119-172)]

A continuación, debemos enlazar el código que se invocará cuando se encuentre el anclaje que estamos consultando. Dentro de `InitializeSession()`, agregue las siguientes devoluciones de llamada:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=200-206&highlight=4-5)]


Ahora vamos a agregar código con el que se creará y colocará una esfera verde una vez que se encuentre CloudSpatialAnchor. También volverá a habilitar la opción de tocar la pantalla, con el fin de que pueda repetir el escenario completo una vez más: crear otro anclaje local, cargarlo y volver a buscarlo.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=228-265)]

Eso es todo. Ejecute la aplicación desde **Visual Studio** una última vez para probar el escenario completo de extremo a extremo. Mueva el dispositivo y coloque la esfera negra. Luego, siga moviendo la cabeza para capturar los datos del entorno hasta que la esfera se vuelva amarilla. Se cargará el anclaje local y la esfera volverá a ser azul. Por último, toque la pantalla una vez más, con el fin de quitar el anclaje local y, después, consultaremos su homólogo en la nube. Siga desplazando el dispositivo hasta que se encuentre el anclaje espacial en la nube. Debe aparecer una esfera verde en la ubicación correcta, y puede volver a aclarar y repetir todo el escenario.

[!INCLUDE [AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md)]