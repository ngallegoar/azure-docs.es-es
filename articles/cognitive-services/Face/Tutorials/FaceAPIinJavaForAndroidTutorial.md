---
title: 'Tutorial: Detección y enmarcado de las caras de una imagen con Android SDK'
titleSuffix: Azure Cognitive Services
description: En este tutorial, creará una aplicación Android sencilla que usa el servicio Face para detectar y enmarcar caras en una imagen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 8d5bef141f83eedaa996bb63c1fb814aeb6af197
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76165976"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: Creación de una aplicación Android para detectar y enmarcar caras en una imagen

En este tutorial, creará una aplicación Android que usa el servicio Azure Face, con el SDK de Java, para detectar rostros humanos en una imagen. La aplicación muestra una imagen seleccionada y dibuja un marco alrededor de cada cara detectada.

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> - Creación de una aplicación Android
> - Instalación de la biblioteca cliente de Face
> - Usar la biblioteca cliente para detectar caras en una imagen
> - Dibujar un marco alrededor de cada cara detectada

![Captura de pantalla de Android de una foto con las caras enmarcadas por un rectángulo rojo](../Images/android_getstarted2.1.PNG)

El código de ejemplo completo está disponible en el repositorio [Cognitive Services Face Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) de GitHub.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. 

## <a name="prerequisites"></a>Prerequisites

- Una clave de suscripción de Face. Puede obtener una clave de la suscripción de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). O bien, siga las instrucciones para la [creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para poder suscribirse al servicio Face y obtener la clave. Después, [cree variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la cadena de punto de conexión del servicio y la clave denominadas `FACE_SUBSCRIPTION_KEY` y `FACE_ENDPOINT`, respectivamente.
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).
- [Android Studio](https://developer.android.com/studio/) con el nivel de API 22, o posterior (requerido por la biblioteca cliente de Face).

## <a name="create-the-android-studio-project"></a>Creación del proyecto de Android Studio

Siga estos pasos para crear un nuevo proyecto de aplicación de Android.

1. En Android Studio, seleccione **Start a new Android Studio project** (Iniciar un proyecto de Android Studio).
1. En la pantalla **Create Android Project** (Crear un proyecto de Android), puede modificar los campos predeterminados, si es necesario, y después haga clic en **Next** (Siguiente).
1. En la pantalla **Target Android Devices** (Dispositivos Android de destino), utilice el selector de lista desplegable para elegir **API 22**, o posterior, y, después, haga clic en **Next** (Siguiente).
1. Seleccione **Empty Activity** (Actividad vacía) y, después, haga clic en **Next** (Siguiente).
1. Desactive la opción **Backwards Compatibility** (Compatibilidad con versiones anteriores) y haga clic en **Finish** (Finalizar).

## <a name="add-the-initial-code"></a>Adición del código inicial

### <a name="create-the-ui"></a>Creación de la interfaz de usuario

Abra *activity_main.xml*. En el Editor de diseño, seleccione la pestaña **Text** (Texto) y sustituya el contenido por el código siguiente.

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?name=snippet_activitymain)]

### <a name="create-the-main-class"></a>Creación de la clase principal

Abra *MainActivity.java* y sustituya las instrucciones `import` existentes por el siguiente código.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_imports)]

Luego, reemplace el contenido de la clase **MainActivity** por el código siguiente. Así se crea un controlador de eventos en **Button** que inicia una nueva actividad para permitir al usuario seleccionar una imagen. Muestra la imagen en **ImageView**.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_mainactivity_methods)]

### <a name="try-the-app"></a>Prueba de la aplicación

Marque como comentario la llamada a **detectAndFrame** en el método **onActivityResult**. Luego, pulse **Run** (Ejecutar) en el menú para probar la aplicación. Cuando se abra la aplicación, ya sea en un emulador o en un dispositivo conectado, haga clic en **Browse** (Examinar) en la parte inferior. Debería aparecer el cuadro de diálogo de selección de archivos del dispositivo. Elija una imagen y compruebe que se muestra en la ventana. Luego, cierre la aplicación y vaya al paso siguiente.

![Captura de pantalla de Android de una foto con caras](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Incorporación del SDK de Face

### <a name="add-the-gradle-dependency"></a>Incorporación de la dependencia de Gradle

En el panel **Project** (Proyecto), utilice el selector de lista desplegable para seleccionar **Android**. Expanda **Gradle Scripts** (Scripts de Gradle) y abra *build.gradle (Module: app)* . Agregue una dependencia para la biblioteca cliente de Face, `com.microsoft.projectoxford:face:1.4.3`, tal como se muestra en la siguiente captura de pantalla y, después, haga clic en **Sync Now** (Sincronizar ahora).

![Captura de pantalla de Android Studio del archivo build.gradle de aplicación](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Incorporación del código de proyecto relacionado con Face

Vuelva a **MainActivity.java** y agregue las siguientes instrucciones `import`:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_face_imports)]

Luego, inserte el siguiente código en la clase **MainActivity**, encima del método **onCreate**:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_mainactivity_fields)]

En el panel **Project** (Proyecto), expanda **app**, a continuación, **manifests** y abra *AndroidManifest.xml*. Inserte el siguiente elemento como elemento secundario directo del elemento `manifest`:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?name=snippet_manifest_entry)]

## <a name="upload-image-and-detect-faces"></a>Carga de imagen y detección de caras

La aplicación detectará las caras mediante una llamada al método **faceClient.Face.DetectWithStreamAsync**, que incluye la API de REST de [detección](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) y devuelve una lista de instancias de **Face**.

Cada instancia de **Face** devuelta contiene un rectángulo para indicar su ubicación, junto con una serie de atributos de cara opcionales. En este ejemplo, solo se solicitan los rectángulos de las caras.

Inserte los dos métodos siguientes en la clase **MainActivity**. Cuando se completa la detección de caras, la aplicación llama al método **drawFaceRectanglesOnBitmap** para modificar **ImageView**. Dicho método se definirá a continuación.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_detection_methods)]

## <a name="draw-face-rectangles"></a>Dibujar rectángulos faciales

Inserte el siguiente método auxiliar en la clase **MainActivity**. Este método dibuja un rectángulo alrededor de cada rostro detectado y utiliza las coordenadas del rectángulo de cada instancia de **Face**.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_drawrectangles)]

Por último, quite la marca de comentario de la llamada al método **detectAndFrame** de **onActivityResult**.

## <a name="run-the-app"></a>Ejecución la aplicación

Ejecute esta aplicación y busque una imagen con una cara. Espere unos segundos para permitir que el servicio Face responda. Debe ver un rectángulo rojo en cada una de las caras de la imagen.

![Captura de pantalla de Android de rostros con rectángulos rojos dibujados alrededor](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido el proceso básico para usar el SDK de Java de Face y ha creado una aplicación que detecta y enmarca las caras de una imagen. Después, obtendrá más información acerca de los detalles de la detección de caras.

> [!div class="nextstepaction"]
> [Detección de caras en una imagen](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
