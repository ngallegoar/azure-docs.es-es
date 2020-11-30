---
title: Compilación de una aplicación de inscripción para Android con React
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo configurar el entorno de desarrollo e implementar una aplicación de inscripción de Face para obtener consentimiento de los clientes.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 11/17/2020
ms.author: pafarley
ms.openlocfilehash: bd2032d565f5bd1fb430449be8b8c08e222f531d
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95029391"
---
# <a name="build-an-enrollment-app-for-android-with-react"></a>Compilación de una aplicación de inscripción para Android con React

En esta guía se muestra cómo empezar a trabajar con la aplicación de inscripción de Face de ejemplo. La aplicación muestra procedimientos recomendados para obtener un consentimiento significativo para inscribir usuarios en un servicio de reconocimiento facial y adquirir datos faciales de gran precisión. Un sistema integrado podría usar una aplicación de inscripción como esta para proporcionar control de acceso sin contacto, comprobación de identidad, seguimiento de la asistencia o una pantalla completa de personalización, en función de los datos faciales.

Cuando se inicia, la aplicación muestra a los usuarios una pantalla de consentimiento detallada. Si el usuario da su consentimiento, la aplicación solicita un nombre de usuario y una contraseña y, después, captura una imagen de alta calidad de la cara mediante la cámara del dispositivo.

La aplicación de inscripción de ejemplo se escribe con JavaScript y el marco de React Native. Actualmente se puede implementar en dispositivos Android. En el futuro, habrá más opciones de implementación.

## <a name="prerequisites"></a>Prerrequisitos 

* Una suscripción a Azure ([cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)).  
* Una vez que tenga la suscripción de Azure, [cree un recurso de Face](https://portal.azure.com/#create/Microsoft.CognitiveServicesFace) en Azure Portal para obtener la clave y el punto de conexión. Tras su implementación, seleccione **Ir al recurso**.  
  * Necesitará la clave y el punto de conexión del recurso creado para conectar la aplicación a Face API.  
  * Para el desarrollo y las pruebas locales, puede pegar la clave de API y el punto de conexión en el archivo de configuración. Para la implementación final, almacene la clave de API en una ubicación segura y nunca en el código.  

> [!IMPORTANT]
> Estas claves de suscripción se usan para tener acceso a la API de Cognitive Services. No comparta las claves. Almacénelas de forma segura, por ejemplo, con Azure Key Vault. También se recomienda regenerar estas claves periódicamente. Solo se necesita una clave para realizar una llamada API. Al volver a generar la primera clave, puede usar la segunda clave para seguir teniendo acceso al servicio.

## <a name="set-up-the-development-environment"></a>Configuración del entorno de desarrollo

1. Clone el repositorio de Git para la [aplicación de inscripción de ejemplo](https://github.com/azure-samples/cognitive-services-FaceAPIEnrollmentSample).
1. Para configurar el entorno de desarrollo, siga la <a href="https://reactnative.dev/docs/environment-setup"  title="documentación de React Native"  target="_blank">React Native documentation <span class="docon docon-navigate-external x-hidden-focus"></span></a>. Seleccione **React Native CLI Quickstart** (Inicio rápido de la CLI de React Native) como sistema operativo de desarrollo y **Android** como sistema operativo de destino. Complete las secciones **Installing dependencies** (Instalación de dependencias) y **Android development environment** (Entorno de desarrollo de Android).
1. Abra el archivo env.json en el editor de texto que prefiera, como [Visual Studio Code](https://code.visualstudio.com/) y agregue el punto de conexión y la clave. Puede obtener el punto de conexión y la clave en Azure Portal en la pestaña **Información general** del recurso. Este paso solo se utiliza para las pruebas locales; no inserte su clave de Face API en el repositorio remoto.
1. Ejecute la aplicación mediante el emulador de dispositivo virtual Android desde Android Studio o con su propio dispositivo Android. Para probar la aplicación en un dispositivo físico, siga la <a href="https://reactnative.dev/docs/running-on-device"  title="documentación de React Native"  target="_blank">React Native documentation <span class="docon docon-navigate-external x-hidden-focus"></span></a> pertinente.  


## <a name="create-an-enrollment-experience"></a>Creación de una experiencia de inscripción  

Ahora que ha configurado la aplicación de inscripción de ejemplo, puede adaptarla a sus propias necesidades de inscripción.

Por ejemplo, es posible que desee agregar información específica de la situación en la página de consentimiento:

> [!div class="mx-imgBorder"]
> ![página de consentimiento de la aplicación](./media/enrollment-app/1-consent-1.jpg)

El servicio proporciona comprobaciones de calidad de la imagen para ayudarle a elegir si la imagen tiene una calidad suficiente para inscribir al cliente o intentar el reconocimiento facial. Esta aplicación muestra cómo obtener acceso a los fotogramas desde la cámara del dispositivo, seleccionar los fotogramas de mayor calidad e inscribir la cara detectada en el servicio de Face API. 

Muchos de los problemas de reconocimiento facial se deben a la baja calidad de las imágenes de referencia. Los siguientes son algunos de los factores que pueden degradar el rendimiento del modelo:
* Tamaño de la cara (caras alejadas de la cámara)
* Orientación de la cara (caras giradas o inclinadas con respecto a la cámara)
* Condiciones de iluminación deficientes (poca luz o contraluz) que hacen que la imagen pueda quedar mal expuesta o tener demasiado ruido
* Oclusión (caras parcialmente ocultas u obstruidas), incluidos accesorios como sombreros o gafas gruesas
* Desenfoque (por ejemplo, por un rápido movimiento de la cara al tomar la fotografía). 

> [!div class="mx-imgBorder"]
> ![página de instrucciones de captura de imagen de la aplicación](./media/enrollment-app/4-instruction.jpg)

Observe que la aplicación también ofrece funcionalidad para eliminar la inscripción del usuario y la opción de volver a inscribirlo.

> [!div class="mx-imgBorder"]
> ![página de administración del perfil](./media/enrollment-app/10-manage-2.jpg)

Para ampliar la funcionalidad de la aplicación para abarcar la experiencia de inscripción completa, lea la [información general](enrollment-overview.md) para familiarizarse con características de implementación adicionales y procedimientos recomendados.

## <a name="deploy-the-enrollment-app"></a>Implementación de la aplicación de inscripción

### <a name="android"></a>Android

En primer lugar, asegúrese de que la aplicación está lista para la implementación de producción: quite cualquier clave o secreto del código de la aplicación y asegúrese de que ha seguido los [procedimientos recomendados de seguridad](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security?tabs=command-line%2Ccsharp).

Cuando todo esté listo para publicar la aplicación para producción, generará un archivo APK listo para la publicación, que es el formato de archivo de paquete para aplicaciones Android. Este archivo APK debe estar firmado con una clave privada. Con esta versión de lanzamiento, puede empezar a distribuir la aplicación directamente a sus dispositivos. 

Siga la documentación de <a href="https://developer.android.com/studio/publish/preparing#publishing-build"  title="preparación para la publicación"  target="_blank">Prepare for release <span class="docon docon-navigate-external x-hidden-focus"></span></a> para obtener información sobre cómo generar una clave privada, firmar la aplicación y generar un archivo APK de publicación.  

Una vez creado un archivo APK firmado, consulte la documentación de <a href="https://developer.android.com/studio/publish"  title="publicación de la aplicación"  target="_blank">Publish your app <span class="docon docon-navigate-external x-hidden-focus"></span></a> para obtener más información sobre cómo publicar la aplicación.

## <a name="next-steps"></a>Pasos siguientes  

En esta guía, ha aprendido a configurar el entorno de desarrollo y a empezar a trabajar con la aplicación de inscripción de ejemplo. Si no está familiarizado con React Native, puede leer su [documentación de introducción](https://reactnative.dev/docs/getting-started) para obtener más información general. También puede ser útil familiarizarse con [Face API](Overview.md). Lea las demás secciones de la documentación de la aplicación de inscripción antes de comenzar con el desarrollo.
