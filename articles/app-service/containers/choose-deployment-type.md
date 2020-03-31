---
title: Opciones de implementación para contenedores Linux
description: Decida entre la implementación de contenedores de Docker personalizada, varios contenedores y una plataforma de aplicaciones integrada para App Service en Linux.
keywords: azure app service, aplicación web, linux, oss
author: msangapu-msft
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 7c4025e393c196fb98455bac26e94e5f252840b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687510"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>¿Imagen personalizada, varios contenedores o imagen de plataforma integrada?

[App Service en Linux](app-service-linux-intro.md) ofrece tres formas diferentes de publicar su aplicación en la Web:

- **Implementación de una imagen personalizada**: convierte la aplicación en una imagen de Docker que contiene todos los archivos y dependencias en un paquete listo para ejecutarse.
- **Implementación de varios contenedores**: incluye la aplicación en varios contenedores Docker utilizando un archivo de configuración de Docker Compose.
- **Implementación de aplicaciones con una imagen de plataforma integrada**: nuestras imágenes de plataforma integrada contienen entornos en tiempo de ejecución y dependencias de aplicaciones web comunes, como Node y PHP. Use cualquiera de los [métodos de implementación de Azure App Service](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para implementar la aplicación en el almacenamiento de la aplicación web y, luego, use una imagen de plataforma integrada para ejecutarla.

## <a name="which-method-is-right-for-your-app"></a>¿Qué método es adecuado para su aplicación? 

Los factores principales a tener en cuenta son:

- **Disponibilidad de Docker en el flujo de trabajo de desarrollo**: el desarrollo de una imagen personalizada requiere un conocimiento básico del flujo de desarrollo de Docker. La implementación de una imagen personalizada en una aplicación web requiere la publicación de la imagen personalizada en un host de repositorio, como Docker Hub. Si está familiarizado con Docker y puede agregar tareas de Docker al flujo de trabajo de compilación, o si ya publica su aplicación como una imagen de Docker, una imagen personalizada es casi seguro la mejor opción.
- **Arquitectura de varias capas**: implementa varios contenedores, como una capa de la aplicación web y una capa de API, para separar las funcionalidades mediante varios contenedores. 
- **Rendimiento de la aplicación**: aumenta el rendimiento de la aplicación de varios contenedores mediante una capa de caché como Redis. Para conseguir esto seleccione varios contenedores.
- **Requisitos únicos de tiempo de ejecución**: las imágenes de plataforma integrada están diseñadas para satisfacer las necesidades de la mayoría de aplicaciones, pero están limitadas en cuanto a su capacidad de personalización. La aplicación puede tener dependencias únicas u otros requisitos de tiempo de ejecución que superan a aquellos de los que son capaces las imágenes integradas.
- **Requisitos de compilación**: con la [implementación continua](../deploy-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), puede tener funcionando su aplicación en Azure directamente desde el código fuente. No se necesita ningún proceso de compilación o publicación externo. Sin embargo, hay un límite para la capacidad de personalización y la disponibilidad de las herramientas de compilación en el motor de implementación [Kudu](https://github.com/projectkudu/kudu/wiki). La aplicación puede sobrepasar las funcionalidades de Kudu a medida que sus dependencias o requisitos aumentan para la lógica de compilación personalizada.
- **Requisitos de lectura y escritura en disco**: todas las aplicaciones web se asignan a un volumen de almacenamiento de contenido web. Este volumen, respaldado por Azure Storage, se monta en `/home` en el sistema de archivos de la aplicación. A diferencia de los archivos del sistema de archivos de contenedor, los archivos del volumen de contenido son accesibles en todas las instancias de escalado de una aplicación y las modificaciones se conservan entre reinicios de la aplicación. Sin embargo, la latencia de disco del volumen de contenido es más elevada y más variable que la latencia del sistema de archivos de contenedor local, y el acceso puede verse afectado por las actualizaciones de la plataforma, el tiempo de inactividad imprevisto y los problemas de conectividad de red. Las aplicaciones que requieren mayormente acceso de solo lectura a archivos de contenido pueden beneficiarse de la implementación de imágenes personalizadas, que coloca los archivos en el sistema de archivos de imagen en lugar de en el volumen de contenido.
- **Uso de recursos de compilación**: cuando se implementa una aplicación desde el origen, los scripts de implementación que ejecuta Kudu usan los mismos recursos de proceso y almacenamiento del plan de App Service que la aplicación en ejecución. Las implementaciones de aplicaciones de gran volumen pueden consumir más recursos o tiempo de los previstos. En concreto, muchos flujos de trabajo de implementación generan una gran actividad en los discos del volumen de contenido de la aplicación, que no está optimizado para dicha actividad. Una imagen personalizada proporciona todos los archivos y dependencias de la aplicación a Azure en un único paquete sin necesidad de transferencias de archivos o acciones de implementación adicionales.
- **Necesidad de iteración rápida**: la transformación de una aplicación para Docker requiere pasos de compilación adicionales. Para que los cambios surtan efecto, debe insertar la nueva imagen en un repositorio con cada actualización. Estas actualizaciones se extraen luego en el entorno de Azure. Si uno de los contenedores integrados cumple las necesidades de su aplicación, la implementación desde el origen puede ofrecer un flujo de trabajo de desarrollo más rápido.

## <a name="next-steps"></a>Pasos siguientes

Contenedor personalizado:
* [Ejecución de un contenedor personalizado](quickstart-docker-go.md)

Varios contenedores:
* [Creación de una aplicación de varios contenedores](quickstart-multi-container.md)

Los siguientes artículos le ayudarán a comenzar a trabajar con App Service en Linux a partir de una imagen de plataforma integrada:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](quickstart-php.md)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)