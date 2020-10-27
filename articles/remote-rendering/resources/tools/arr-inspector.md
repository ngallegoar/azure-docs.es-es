---
title: Herramienta de inspección ArrInspector
description: Manual del usuario de la herramienta ArrInspector
author: florianborn71
ms.author: flborn
ms.date: 03/09/2020
ms.topic: article
ms.openlocfilehash: 5ee9d39db130c76f683ca1f4d1ff64f9ed86d97e
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92204748"
---
# <a name="the-arrinspector-inspection-tool"></a>Herramienta de inspección ArrInspector

ArrInspector es una herramienta basada en web que se usa para inspeccionar una sesión de Azure Remote Rendering en ejecución. Está destinada a fines de depuración, para inspeccionar la estructura de la escena que se está representando, mostrar los mensajes de registro y supervisar el rendimiento en directo en el servidor.

![ArrInspector](./media/arr-inspector.png)

## <a name="connecting-to-the-arrinspector"></a>Conexión a ArrInspector

Una vez que obtenga el nombre de host (que termina en `mixedreality.azure.com`) del servidor de ARR, conéctese mediante [ConnectToArrInspectorAsync](../../how-tos/frontend-apis.md#connect-to-arr-inspector). Esta función crea un objeto `StartArrInspector.html` en el dispositivo en el que se ejecuta la aplicación. Para iniciar ArrInspector, abra el archivo con un explorador (Edge, Firefox o Chrome) en un equipo. El archivo solo es válido durante 24 horas.

Si la aplicación que llama a `ConnectToArrInspectorAsync` ya se está ejecutando en un equipo:

* Si usa la integración de Unity, puede que se inicie automáticamente.
* De lo contrario, encontrará el archivo en *Carpetas de usuarios\\LocalAppData\\[su_aplicación]\\AC\\Temp* .

Si la aplicación se ejecuta en un dispositivo HoloLens:

1. Acceda al dispositivo HoloLens mediante el [Portal de dispositivos Windows](/windows/mixed-reality/using-the-windows-device-portal).
1. Vaya a *Sistema > Explorador de archivos* .
1. Vaya a *Carpetas de usuarios\\LocalAppData\\[su_aplicación]\\AC\\Temp* .
1. Guarde el archivo *StartArrInspector.html* en el equipo.
1. Abra *StartArrInspector.html* para cargar la instancia de ArrInspector de la sesión.

## <a name="the-performance-panel"></a>Panel Rendimiento

![Panel Rendimiento](./media/performance-panel.png)

Este panel muestra gráficos de todos los valores de rendimiento por fotograma que muestra el servidor. Actualmente, se incluyen los valores de hora del fotograma, FPS, uso de CPU y memoria, estadísticas de memoria como el uso general de la RAM, recuentos de objetos, etc.

Para visualizar uno de estos parámetros, haga clic en el botón **Agregar nuevo** y seleccione uno de los valores disponibles que se muestran en el cuadro de diálogo. Esta acción agrega un nuevo gráfico de desplazamiento al panel y realiza un seguimiento de los valores en tiempo real. En el lado derecho puede ver los valores *mínimo* , *máximo* y *actual* .

Para desplazar lateralmente el gráfico, puede arrastrar su contenido con el mouse; sin embargo, el movimiento panorámico horizontal solo es posible cuando ArrInspector está en estado de pausa.

Para hacer zoom, mantenga presionada la tecla CTRL durante el arrastre. El zoom horizontal también se puede controlar con el control deslizante de la parte inferior.

El intervalo vertical se calcula de forma predeterminada en función de los valores que se muestran actualmente, y los valores mínimo y máximo se muestran en los cuadros de texto de la derecha. Si los valores se establecen manualmente, ya sea escribiéndolos directamente en el cuadro de texto o mediante el movimiento panorámico o el zoom, el gráfico usará esos valores. Para restaurar las tramas verticales automáticas, haga clic en el icono de la esquina superior derecha.

![Intervalo vertical](./media/vertical-range.png)

## <a name="the-log-panel"></a>Panel Registro

![Panel Registro](./media/log-panel.png)

El panel Registro muestra una lista de los mensajes de registro generados en el lado servidor. Al realizar la conexión, se mostrarán hasta 200 mensajes de registro anteriores y se imprimirán los nuevos a medida que se generen.

Puede filtrar la lista en función del tipo de registro `[Error/Warning/Info/Debug]` mediante los botones de la parte superior.
![Botones de filtro de registro](./media/log-filter.png)

## <a name="the-timing-data-capture-panel"></a>Panel Timing Data Capture (Captura de datos de tiempo)

![Captura de datos de tiempo](./media/timing-data-capture.png)

Este panel se usa para capturar información de tiempo del servidor y descargarla. El archivo usa el [formato JSON de Chrome Tracing](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/edit). Para inspeccionar los datos, abra Chrome en la dirección URL `Chrome://tracing` y arrastre y coloque el archivo descargado en la página. Los datos de tiempo se recopilan continuamente en un búfer en anillo de tamaño fijo. Cuando se escribe, la captura solo incluye información sobre el pasado inmediato, lo que significa de un par de segundos a unos minutos.

## <a name="the-scene-inspection-panel"></a>Panel Scene Inspection (Inspección de escena)

![Panel Scene Inspection (Inspección de escena)](./media/scene-inspection-panel.png)

Este panel muestra la estructura de la escena representada. La jerarquía de objetos está a la izquierda y el contenido del objeto seleccionado se encuentra a la derecha. El panel es de solo lectura y se actualiza en tiempo real.

## <a name="the-vm-debug-information-panel"></a>Panel VM Debug Information (Información de depuración de VM)

![Panel VM Debug Information (Información de depuración de VM)](./media/state-debugger-panel.png)

Este panel ofrece algunas funciones de depuración.

### <a name="restart-service"></a>Reiniciar servicio

El botón **Reiniciar servicio** reinicia el tiempo de ejecución en la máquina virtual a la que está conectado arrInspector. Cualquier cliente conectado se desconectará y la página arrInspector deberá volver a cargarse para conectarse al servicio reiniciado.

### <a name="collect-debug-information"></a>Recopilar información de depuración

El botón **Collect Debug Information for VM** (Recopilar información de depuración de VM) abre un cuadro de diálogo que permite desencadenar la instancia de ARR para recopilar información de depuración sobre la VM:

![Cuadro de diálogo VM Debug Information (Información de depuración de VM)](./media/state-debugger-dialog.png)

La información de depuración ayuda al equipo de Azure Remote Rendering a analizar los problemas que se producen en una instancia de ARR en ejecución. El cuadro de diálogo tiene un campo de texto para proporcionar detalles adicionales, por ejemplo, los pasos para reproducir un problema.

Después de hacer clic en el botón **Iniciar la recopilación** , el cuadro de diálogo se cerrará y se iniciará el proceso de recopilación. La recopilación de la información en la VM puede tardar unos minutos.

![Recopilación de información de depuración de VM en curso](./media/state-debugger-panel-in-progress.png)

Una vez finalizada la recopilación, recibirá una notificación en la ventana ArrInspector. Esta notificación contiene un identificador que identifica esta colección en particular. Asegúrese de guardar este identificador para pasarlo al equipo de Azure Remote Rendering.

![Recopilación de información de depuración de VM correcta](./media/state-debugger-snackbar-success.png)

> [!IMPORTANT]
> No se puede descargar la información de depuración de la VM, ni acceder a esta de otro modo. Solo el equipo de Azure Remote Rendering tiene acceso a los datos recopilados. Debe ponerse en contacto con nosotros y enviar el identificador de colección, para que podamos investigar el problema que experimenta.

## <a name="pause-mode"></a>Modo de pausa

En la esquina superior derecha, un conmutador permite pausar la actualización directa de los paneles. Este modo puede ser útil para inspeccionar detenidamente un estado específico.

![Modo de pausa](./media/pause-mode.png)

Al volver a habilitar la actualización directa, se restablecen todos los paneles.

## <a name="host-configuration"></a>Configuración de host

De forma predeterminada, la herramienta se conecta al servidor de ARR que se está ejecutando en el mismo host que atiende a ArrInspector. Sin embargo, se puede configurar para inspeccionar otro servidor, suponiendo que ejecuta una instancia de ARR con el puerto de herramientas abierto.

Para ello, acceda al menú principal de la izquierda de la barra de encabezado y seleccione *Configuración de host* . Haga clic en **Add new host** (Agregar un nuevo host) y escriba el nombre y el nombre de host. Para el *nombre de host* use solo el nombre de host que termina en `.mixedreality.azure.com`, no incluya `http://` ni un puerto.

![Configuración de host](./media/host-configuration.png)

Para cambiar rápidamente de un host a otro, use la lista desplegable de la parte superior derecha.

![Combinación de host](./media/host-switch-combo.png)

La lista de hosts se almacena en el almacenamiento local del explorador, de modo que se conserva al volver a abrir el mismo explorador.