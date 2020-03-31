---
title: Elección de la plataforma de desarrollo de front-end para compilar las aplicaciones cliente con Visual Studio y servicios de Azure
description: Conozca los lenguajes nativos y multiplataforma admitidos para compilar aplicaciones cliente.
author: codemillmatt
ms.service: vs-appcenter
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 714916c755ce50f6e596cee4be85ceb0b142c789
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240856"
---
# <a name="choose-mobile-development-frameworks"></a>Elección de marcos de desarrollo para aplicaciones móviles
Los desarrolladores pueden usar las tecnologías del lado cliente para compilar por su cuenta las aplicaciones móviles, con marcos y patrones específicos para un enfoque multiplataforma. En función de sus factores de decisión, los desarrolladores pueden crear:
- Aplicaciones nativas de una sola plataforma mediante lenguajes como Objective C y Java
- Aplicaciones multiplataforma mediante Xamarin, .NET y C#
- Aplicaciones híbridas mediante Cordova y sus variantes

## <a name="native-platforms"></a>Plataformas nativas
La creación de una aplicación nativa requiere lenguajes de programación, SDK y entornos de desarrollo específicos de la plataforma, además de otras herramientas que proporcionan los proveedores del sistema operativo.

### <a name="ios"></a>iOS
Creado y desarrollado por Apple, iOS se usa para compilar aplicaciones en dispositivos de Apple, es decir, iPhone y iPad.

- **Lenguajes de programación**: Objective-C, Swift
- **IDE**: Xcode
- **SDK**: SDK de iOS

### <a name="android"></a>Android
Diseñado por Google, es el sistema operativo más popular del mundo: Android se usa para compilar aplicaciones que pueden ejecutarse en una amplia gama de smartphones y tabletas.

- **Lenguaje de programación**: Java, Kotlin 
- **IDE**: Android Studio y herramientas para desarrolladores de Android 
- **SDK**: SDK de Android

### <a name="windows"></a>Windows
- **Lenguaje de programación**: C#
- **IDE**: Visual Studio, Visual Studio Code
- **SDK**: Windows SDK

#### <a name="pros"></a>Ventajas
- Buena experiencia de usuario
- Aplicaciones con capacidad de respuesta con alto rendimiento y la posibilidad de interactuar con bibliotecas nativas
- Aplicaciones muy seguras

#### <a name="cons"></a>Desventajas
- La aplicación solo se ejecuta en una plataforma
- Consume más recursos de desarrollo y resulta caro compilar una aplicación
- Menor reutilización de código

## <a name="cross-platforms-and-hybrid-applications"></a>Aplicaciones multiplataforma e híbridas
Las aplicaciones multiplataforma le ofrecen la posibilidad de escribir aplicaciones móviles nativas una vez, compartir el código y ejecutarlas en iOS, Android y Windows.

### <a name="xamarin"></a>Xamarin
Propiedad de Microsoft, [Xamarin](https://visualstudio.microsoft.com/xamarin/) se usa para crear aplicaciones móviles robustas multiplataforma en C#. Xamarin tiene una biblioteca de clases y un entorno en tiempo de ejecución que funciona en muchas plataformas, como iOS, Android y Windows. También compila aplicaciones nativas (no interpretadas) que ofrecen un alto rendimiento. Xamarin combina todas las capacidades de las plataformas nativas y agrega una serie de características propias muy eficaces.

- **Lenguaje de programación**: C#
- **IDE**: Visual Studio en Windows o Mac

### <a name="react-native"></a>React Native
Publicado por Facebook en 2015, [React Native](https://facebook.github.io/react-native/) es un marco de JavaScript de [código abierto](https://github.com/facebook/react-native) para escribir aplicaciones móviles reales y de representación nativa para iOS y Android. Se basa en React, la biblioteca de JavaScript de Facebook para crear interfaces de usuario. En lugar de dirigirse al explorador, se dirige a las plataformas móviles. React Native usa componentes nativos en lugar de componentes web como bloques de creación.
 
- **Lenguaje de programación**: JavaScript
- **IDE**: Visual Studio Code

### <a name="unity"></a>Unity
 Unity es un motor optimizado para la creación de juegos. Puede usarlo para crear aplicaciones 2D o 3D de alta calidad con C# para plataformas como Windows, iOS, Android y Xbox.

### <a name="cordova"></a>Cordova
Cordova le permite crear aplicaciones híbridas con Visual Studio Tools para Apache Cordova o Visual Studio Code con extensiones para Cordova. Con el enfoque híbrido, puede compartir componentes con sitios web y reutilizar aplicaciones de servidor web con estrategias de aplicaciones web hospedadas basadas en Cordova.

#### <a name="pros"></a>Ventajas
- Mayor facilidad de uso del código al crearse un único código base para varias plataformas
- Atención a un público más amplio en muchas plataformas
- Reducción drástica del tiempo de desarrollo
- Fácil de iniciar y actualizar

#### <a name="cons"></a>Desventajas
- Menor rendimiento
- Falta de flexibilidad
- Cada plataforma tiene un conjunto único de características y funcionalidades para que la aplicación nativa sea más creativa
- Aumento del tiempo de diseño de la interfaz de usuario
- Limitación de la herramienta
