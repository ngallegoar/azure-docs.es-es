---
title: Integración de Project Acoustics en Unreal y Wwise
titlesuffix: Azure Cognitive Services
description: En este artículo se describe la integración de los complementos de Unreal y Wwise para Project Acoustics en un proyecto.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: e57212a3002390754aaebc5f2aa9ffb10af230a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "72243060"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Integración de Project Acoustics en Unreal y Wwise
En este artículo se describe cómo integrar el paquete de complementos de Project Acoustics en el proyecto de juegos de Unreal y Wwise existente.

Requisitos de software:
* [Unreal Engine](https://www.unrealengine.com/) 4.20+
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/)2018.1
* [Complemento de Wwise para Unreal](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  
  Si va a usar una integración directa del SDK de Wwise en lugar del complemento de Wwise para Unreal, consulte el complemento Unreal de Project Acoustics y ajuste las llamadas API de Wwise.

Para usar Project Acoustics con un motor de audio que no sea Wwise, envíe una solicitud de mejora al [foro de discusión de Project Acoustics](https://github.com/microsoft/ProjectAcoustics/issues). Puede usar el complemento Unreal para Project Acoustics para consultar datos relativos a la acústica y realizar llamadas API a su motor.

## <a name="download-project-acoustics"></a>Descarga de Project Acoustics
Si aún no lo ha hecho, descargue el [paquete de complementos Unreal y Wwise para Project Acoustics](https://www.microsoft.com/download/details.aspx?id=58090).

Este paquete incluye un complemento Unreal Engine y un complemento de mezclador de Wwise. El complemento de Unreal proporciona integración del editor y del entorno de ejecución. Durante el juego, el complemento de Unreal para Project Acoustics calcula parámetros como la oclusión de cada uno de los objetos del juego en cada fotograma. Estos parámetros se convierten en llamadas API de Wwise.

## <a name="integration-steps"></a>Pasos de integración

Siga estos pasos para instalar el paquete e implementarlo en el juego.

### <a name="install-the-project-acoustics-mixer-plug-in"></a>Instalación del complemento de mezclador de Project Acoustics
1. Abra el programa de inicio de Wwise. En la pestaña **Plug-ins** (Complementos), en **Install New Plugins** (Instalar nuevos complementos), seleccione **Add from Directory** (Agregar desde directorio).

    ![Instalación de un complemento en el programa de inicio de Wwise](media/wwise-install-new-plugin.png)

1. Seleccione el directorio *AcousticsWwisePlugin\ProjectAcoustics* del paquete que ha descargado. Contiene el conjunto del complemento de mezclador de Wwise.

   Wwise instalará el complemento. Project Acoustics aparecerá en la lista de complementos de Wwise instalados.  
![Lista de complementos de Wwise instalados después de instalar Project Acoustics](media/unreal-integration-post-mixer-plugin-install.png)

### <a name="dedeploy-wwise-into-your-game"></a>Reimplementación de Wwise en el juego
Vuelva a implementar Wwise en el juego aunque ya lo haya integrado. En este paso se integra el complemento de Wwise para Project Acoustics.

   > [!NOTE]
   > **Complemento del motor:** Si tiene Wwise instalado como un complemento de juego en un proyecto de Unreal en C++, omita este paso. Si, por el contrario, está instalado como un complemento del motor, por ejemplo, porque el proyecto de Unreal es solo un plano técnico, la implementación de Wwise con el complemento de mezclador es más compleja. Cree un proyecto en blanco de Unreal en C++ ficticio. Cierre el editor de Unreal si se abre y siga el procedimiento restante para implementar Wwise en el proyecto ficticio. Luego, copie el complemento de Wwise implementado.
 
1. En el programa de inicio de Wwise, seleccione la pestaña **Unreal Engine**. Seleccione el menú de tres barras (icono) situado junto a **Recent Unreal Engine Projects** (Proyectos de Unreal Engine recientes) y, después, seleccione **Browse for project** (Buscar proyecto). Abra el archivo *.project* del proyecto de juego de Unreal.

    ![Pestaña Unreal en el programa de inicio de Wwise](media/wwise-unreal-tab.png)

1. Seleccione **Integrate Wwise in Project** (Integrar Wwise en proyecto) o **Modify Wwise in Project** (Modificar Wwise en proyecto). Este paso integra los archivos binarios de Wwise en el proyecto, lo que incluye el complemento de mezclador de Project Acoustics.

   > [!NOTE]
   > **Complemento del motor:** Si usa Wwise como complemento del motor y creó el proyecto ficticio tal y como se describió anteriormente, copie la carpeta que Wwise implementó:  *[DummyUProject] \Plugins\Wwise*. Péguela en  *[UESource] \Engine\Plugins\Wwise*. *[DummyUProject]* es la ruta de acceso al proyecto en blanco de Unreal en C++ y *[UESource]* es donde están instalados los archivos de origen de Unreal Engine. Después de copiar la carpeta puede eliminar el proyecto ficticio.

### <a name="add-the-project-acoustics-unreal-plug-in-to-your-game"></a>Incorporación del complemento de Unreal para Project Acoustics en el juego
 
1. Copie la carpeta *Unreal\ProjectAcoustics* en el paquete del complemento. Cree la carpeta *[UProjectDir]\Plugins\ProjectAcoustics*, donde *[UProjectDir]* es la carpeta del proyecto del juego que contiene el archivo *.uproject*.

   > [!NOTE]
   > **Complemento del motor**: Si usa Wwise como complemento de motor, debe usar también Project Acoustics como un complemento de motor de Unreal. En lugar del directorio de destino que se ha indicado anteriormente, use *[UESource]\Engine\Plugins\ProjectAcoustics*.

1. Confirme que ve la carpeta *Wwise* junto con la carpeta *ProjectAcoustics*. Contiene el complemento Wwise, junto con los archivos binarios del complemento de mezclador que ha implementado anteriormente.

### <a name="extend-wwise-unreal-plug-in-functionality"></a>Ampliación de la funcionalidad del complemento de Wwise para Unreal
El complemento de Unreal para Project Acoustics requiere la exposición de un comportamiento adicional de la API del complemento de Wwise para Unreal que siga [estas directrices](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html). Hemos incluido un archivo por lotes para automatizar el procedimiento de la aplicación de revisiones.

* En *Plugins\ProjectAcoustics\Resources*, ejecute *PatchWwise.bat*. En la imagen de ejemplo siguiente se usa nuestro proyecto de ejemplo AcousticsGame.

    ![Ventana del Explorador de Windows en la que aparece resaltado el script para aplicar el parche a Wwise](media/patch-wwise-script.png)

* Si no tiene instalado el SDK de DirectX: En función de la versión de Wwise que use, es posible que tenga que convertir en comentario la línea que contiene `DXSDK_DIR` en *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs*:

    ![El editor de código muestra "DXSDK" convertido en comentario](media/directx-sdk-comment.png)

* Si realiza la compilación con Visual Studio 2019: Para solucionar un error en la vinculación con Wwise, cambie manualmente el valor predeterminado `VSVersion` en  *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs* a **vc150**:

    ![El editor de código que muestra "VSVersion" ha cambiado a "vc150"](media/vsversion-comment.png)

### <a name="build-the-game-and-check-that-python-is-enabled"></a>Compilación del juego y comprobación de que Python está habilitado

1. Compile el juego y asegúrese de que se crea correctamente. Si no se crea, compruebe los pasos anteriores detenidamente antes de continuar.

1. Abra el proyecto en Unreal Editor.

    > [!NOTE]
    > **Complemento del motor:** Si usa ProjectAcoustics como complemento de motor, asegúrese también de que está habilitado en la lista de complementos "integrados".

    Verá un nuevo modo que indica que Project Acoustics se ha integrado.

    ![Modo completo de Acoustics en Unreal](media/acoustics-mode-full.png)

1. Confirme que el complemento Python para Unreal está habilitado para que la integración del editor funcione correctamente.

    ![Extensiones de Python en el editor de Unreal habilitadas](media/ensure-python.png)

### <a name="set-up-your-wwise-project-to-use-project-acoustics"></a>Configurar el proyecto Wwise para usar Project Acoustics

En la descarga de los ejemplos se incluye un proyecto de ejemplo de Wwise. Se recomienda verlo junto con estas instrucciones. Las siguientes capturas de pantalla de este artículo proceden de este proyecto.

#### <a name="bus-setup"></a>Configuración del bus
El complemento de Unreal para Project Acoustics buscará el complemento de mezclador asociado en un bus que tenga el nombre exacto `Project Acoustics Bus`. Cree un bus de audio que tenga este mismo nombre. El complemento de mezclador puede funcionar con diversas configuraciones. Por ahora, damos por hecho que se usará solo para el procesamiento de la reverberación. Este bus transmitirá la señal de reverberación mezclada de todos los orígenes que usan Acoustics. La mezcla se puede realizar de forma ascendente en cualquier estructura que mezcle buses. Aquí se muestra un ejemplo del proyecto de ejemplo de Wwise que se incluye en la descarga de ejemplo.

![Buses de Wwise que muestran el bus de Project Acoustics](media/acoustics-bus.png)

1. Establezca la configuración del canal en el bus en *1.0*, *2.0*, *4.0*, *5.1* o *7.1*. Cualquier otro valor no generará ninguna salida en el bus.

    ![Opciones de configuración del canal para el bus de Project Acoustics](media/acoustics-bus-channel-config.png)

1. Vaya a los detalles del bus de Project Acoustics y asegúrese de que puede ver la pestaña **Mixer Plug-in** (Complemento de mezclador).

    ![Wwise con la pestaña Mixer Plug-in (Complemento de mezclador) del bus de Project Acoustics habilitada](media/mixer-tab-enable.png)

1. Vaya a la pestaña **Mixer Plug-in** (Complemento de mezclador) y agregue el mezclador de Project Acoustics al bus.

    ![Diagrama en el que se muestra cómo agregar el complemento de mezclador de Project Acoustics al bus de Wwise](media/add-mixer-plugin.png)

#### <a name="actor-mixer-hierarchy-setup"></a>Configuración de la jerarquía actor-mezclador
Para lograr el mejor rendimiento posible, Project Acoustics aplica el procesamiento de la señal digital a todos los orígenes al mismo tiempo. Por consiguiente, el complemento debe funcionar como un complemento de mezclador. Wwise requiere que los complementos de mezclador estén en el bus de salida, aunque este suele transportar normalmente la señal de salida sin modificar. Project Acoustics requiere que la señal sin modificar se enrute mediante buses auxiliares, mientras que la señal modificada se transporta en `Project Acoustics Bus`. El siguiente proceso admite la migración gradual a este flujo de señal.

Supongamos que tiene un proyecto con una jerarquía actor-mezclador que contiene *pisadas*, *armas* y demás en el nivel superior. Cada uno tiene un bus de salida correspondiente para su mezcla sin modificar. Supongamos que quiere migrar las pisadas para usar Acoustics. En primer lugar, cree el bus auxiliar correspondiente para que transporte la submezcla sin modificar, que es un elemento secundario del bus de salida de pisadas. Por ejemplo, hemos usado el prefijo "Dry" en la siguiente imagen para organizar los buses, aunque el nombre exacto no es importante. Los medidores o efectos que tenía en el bus de pisadas seguirán funcionando como antes.

![Configuración de mezcla sin modificar recomendada de Wwise](media/wwise-dry-mix-setup.png)

A continuación, modifique la estructura de salida del bus del actor-mezclador de pisadas de la manera siguiente: *Project Acoustics Bus* (Bus de Project Acoustics) se establece en **Output Bus** (Bus de salida) y *Dry_Footsteps* se establece en un bus auxiliar definido por el usuario.

![Configuración recomendada del bus de actor-mezclador de Wwise](media/actor-mixer-bus-settings.png)

Ahora todas las pisadas reciben tratamiento acústico y generan su reverberación en el bus de Project Acoustics. La señal sin efectos se enruta a través de Dry_Footsteps y se espacializa como de costumbre.

Project Acoustics solo se aplica a los sonidos que tengan una ubicación 3D en el mundo. Según la [documentación de Wwise](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/), las propiedades de posición deben establecerse como se muestra. El valor de **3D Spatialization** (Espacialización 3D) puede ser *Position* (Posición) o *Position + Orientation* (Posición+Orientación), lo que sea necesario.

![Configuración recomendada de posicionamiento del actor de Wwise](media/wwise-positioning.png)

**Output Bus** (Bus de salida) no se puede establecer en otro bus que realice la mezcla de forma ascendente hacia *Project Acoustics Bus* (Bus de Project Acoustics). Wwise impone este requisito a los complementos de mezclador.

Si quiere que alguno de los elementos secundarios de la jerarquía actor-mezclador de pisadas no use Acoustics, puede utilizar la opción de "invalidar el elemento principal" para excluirlo.

Si va a usar envíos definidos por el usuario o definidos por el juego para la reverberación en cualquier actor-mezclador del juego, desactívelos en dicho actor-mezclador para que no se aplique la reverberación dos veces.

#### <a name="spatialization"></a>Espacialización
De forma predeterminada, el complemento de mezclador de Wwise para Project Acoustics aplica reverberación por convolución y deja que Wwise realice la espacialización panorámica. Si usa Project Acoustics en esta configuración de solo reverberación predeterminada, puede utilizar cualquier método de configuración de canal y de espacialización en la mezcla sin modificar. Por tanto, puede combinar y usar casi cualquier espacializador con la reverberación de Project Acoustics. Las opciones incluyen [espacializadores binaurales basados en Ambisonics](https://www.audiokinetic.com/products/ambisonics-in-wwise/) y [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound).
 
Project Acoustics incluye un espacializador opcional que admite tanto la panoramización como la representación de HRTF de alta resolución basadas en objetos. Active la casilla **Perform Spatialization** (Realizar espacialización) en la configuración del complemento de mezclador y elija entre *HRTF* o *Panning* (Panorámica). Además, deshabilite los envíos auxiliares definidos por el usuario a todos los buses sin modificar para que el complemento de mezclador de Project Acoustics y Wwise no realicen la espacialización dos veces. El modo de espacialización no se puede cambiar en tiempo real, ya que requiere una regeneración de los bancos de sonido. Reinicie Unreal y vuelva a generar los bancos de sonido antes de seleccionar reproducir para integrar los cambios en la configuración del complemento de mezclador, como la casilla **Perform Spatialization** (Realizar espacialización).

![Configuración de la espacialización del complemento de mezclador de Wwise](media/mixer-spatial-settings.png)

Desafortunadamente, en la actualidad no se admiten otros complementos de espacializador basados en objetos. Se implementan como complementos de mezclador y Wwise no permite que se asignen varios complementos de mezclador a un solo actor-mezclador.  

### <a name="audio-setup-in-unreal"></a>Configuración del audio en Unreal
1. En primer lugar, debe simular mediante "bake" el nivel de juego para producir un recurso acústico, que se colocará en *Content\Acoustics*. Consulte el [Tutorial de simulación mediante "bake" de Unreal](unreal-baking.md). Algunos niveles previamente simulados mediante "bake" se incluyen en el paquete de ejemplos.

1. Cree un actor de espacio de Acoustics en su escena. No cree más de uno de estos actores por nivel, ya que cada uno de ellos representa la acústica de todo el nivel.

    ![Creación de un actor de espacio de Acoustics en el editor de Unreal](media/create-acoustics-space.png)

1. Asigne el recurso de datos simulado de Acoustics mediante "bake" a la ranura de datos de Acoustics del actor de espacio de Acoustics. ¡La escena tiene ahora acústica!

    ![Asignación de recursos de Acoustics en el editor de Unreal](media/acoustics-asset-assign.png)

1. Agregue un actor vacío. Configúrelo como se indica a continuación.

    ![El editor de Unreal muestra el uso de los componentes de Acoustics en un actor vacío](media/acoustics-component-usage.png)

       
    <sup>1</sup> Agregue un componente de audio de Acoustics al actor. Este componente agrega una funcionalidad de Project Acoustics al componente de audio de Wwise.
        
    <sup>2</sup> El cuadro **Play on Start** (Reproducir al iniciar) está seleccionado de forma predeterminada. Este valor desencadena un evento de Wwise asociado al iniciar el nivel.</li>
         
    <sup>3</sup> Use la casilla **Show Acoustics Parameters** (Mostrar parámetros de Acoustics) para imprimir la información de depuración en pantalla acerca de la fuente.

    ![El panel Acoustics del editor de Unreal en la fuente de sonido con los valores de depuración habilitados](media/debug-values.png)

    <sup>4</sup> Asigne un evento de Wwise por flujo de trabajo habitual de Wwise.
       
    <sup>5</sup> Asegúrese de que la opción **Use Spatial Audio** (Usar audio espacial) está desactivada. Si usa Project Acoustics para un componente de audio concreto, no puede usar simultáneamente el motor de audio espacial de Wwise para la acústica.</li>
       
Está listo. ¡Muévase por la escena y explore los efectos acústicos!

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el [Tutorial de diseño de Unreal y Wwise en Project Acoustics](unreal-workflow.md).
* Aprenda a [realizar simulaciones mediante "bake"](unreal-baking.md) en las escenas de un juego.
