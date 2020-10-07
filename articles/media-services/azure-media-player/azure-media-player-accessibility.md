---
title: Accesibilidad en Azure Media Player
description: Más información sobre la configuración de accesibilidad de Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 2231c2969bbfce1668002ad4f5f719e0b8e13de5
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "81727612"
---
# <a name="accessibility"></a>Accesibilidad #

Azure Media Player trabaja con funcionalidades de lector de pantalla como Narrador de Windows y Apple OSX/iOS VoiceOver. Hay etiquetas alternativas disponibles para los botones de la interfaz de usuario y el lector de pantalla es capaz de leer estas etiquetas alternativas cuando el usuario navega por ellas. Se pueden establecer configuraciones adicionales en el nivel del sistema operativo.

## <a name="captions-and-subtitles"></a>Títulos y subtítulos ##

En este momento, Azure Media Player admite subtítulos solo para eventos bajo petición con formato WebVTT y eventos en directo mediante CEA-708. El formato TTML no se admite actualmente. Los subtítulos permiten a reproductor llegar a un público más amplio; por ejemplo, personas con discapacidades auditivas o usuarios que quieren leer el contenido en un idioma diferente. Los títulos también aumentar la involucración con el vídeo, mejoran la comprensión y permiten ver el vídeo en entornos donde hay limitaciones de sonido, como un área de trabajo.  

## <a name="high-contrast-mode"></a>Modo de contraste alto ##

La interfaz de usuario predeterminada de Azure Media Player es compatible con la mayoría de los modos de visualización de contraste alto de los dispositivos o exploradores. Se pueden establecer configuraciones en el nivel del sistema operativo.

## <a name="mobility-options"></a>Opciones de movilidad ##

### <a name="tabbing-focus"></a>Destacar mediante tabulación ###

La opción para destacar mediante tabulación, que los estándares HTML habituales proporcionan, está disponible en Azure Media Player. Para habilitar la opción para destacar mediante tabulación, debe agregar `tabindex=0` (u otro valor si entiende cómo se ve afectado el orden de tabulación en HTML) al `<video>` del HTML, por ejemplo: `<video ... tabindex=0>...</video>`. En algunas plataformas, la opción para destacar controles solo puede estar presente si estos están visibles y si la plataforma admite estas funcionalidades.

Una vez habilitada la opción para destacar, el usuario final puede navegar eficazmente y controlar el reproductor de vídeo sin depender del mouse. Se puede ir a cualquier menú contextual o elemento de control pulsando la tecla del tabulador y seleccionando con la tecla Entrar o la barra espaciadora. Si pulsa la tecla Entrar o la barra espaciadora en un menú contextual, este se expandirá y el usuario final podrá recorrerlo para seleccionar un elemento de menú. Una vez que tenga el contexto del elemento que desea seleccionar, pulse Entrar o la barra espaciadora de nuevo para completar la selección.

### <a name="hotkeys"></a>HotKeys ###

Azure Media Player admite el control mediante las teclas de acceso rápido. En un explorador web, la única manera de controlar el elemento de vídeo subyacente es destacar el reproductor. Una vez que este se haya destacado, la tecla de acceso rápido puede controlar la funcionalidad del reproductor.  En la tabla siguiente se describen las diversas teclas de acceso rápido y su comportamiento asociado:

| Tecla de acceso rápido              | Comportamiento                                                                |
|----------------------|-------------------------------------------------------------------------|
| F/f                  | El reproductor entra o sale del modo de pantalla completa                                  |
| M/m                  | El volumen del reproductor silenciará o activará el micrófono                                          |
| Flechas arriba y abajo    | El volumen del reproductor aumenta o disminuye                                    |
| Flechas izquierda y derecha | El progreso del vídeo avanza o retrocede                                  |
| 0,1,2,3,4,5,6,7,8,9  | El progreso del vídeo se cambiará a 0 %\- 90 % según la tecla que se pulse |
| Acción de clic         | El vídeo se reproducirá o se pondrá en pausa                                                   |

## <a name="next-steps"></a>Pasos siguientes

<!---Some context for the following links goes here--->
- [Inicio rápido de Azure Media Player](azure-media-player-quickstart.md)