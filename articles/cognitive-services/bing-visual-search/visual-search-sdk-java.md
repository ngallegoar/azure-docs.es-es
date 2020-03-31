---
title: 'Inicio rápido: Biblioteca cliente de Bing Visual Search para Java | Microsoft Docs'
description: Cargue una imagen utilizando el SDK de Bing Visual Search y obtenga conclusiones sobre ella.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: 5a6d39fa6e10ad0ad102f9d25ffd252ec9e0fa8a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379508"
---
# <a name="quickstart-bing-visual-search-client-library-for-java"></a>Inicio rápido: Biblioteca cliente de Bing Visual Search para Java

Introducción a la biblioteca cliente de Bing Visual Search para Java. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas. 

La biblioteca cliente de Bing Visual Search para Java se puede usar para:

* Cargar una imagen para enviar una solicitud de búsqueda visual.
* Obtener el token de información de una imagen y etiquetas de búsqueda visual.

[Documentación de referencia](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingvisualsearch?view=azure-java-stable) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingVisualSearch) | [Artifact (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-visualsearch/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* La versión actual de [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* La [herramienta de compilación de Gradle](https://gradle.org/install/) u otro administrador de dependencias

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

Después de obtener una clave de la suscripción de evaluación o el recurso, [cree una variable de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para ella denominada `BING_SEARCH_V7_SUBSCRIPTION_KEY`.

### <a name="create-a-new-gradle-project"></a>Creación de un proyecto de Gradle

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `gradle init` desde el directorio de trabajo. Este comando creará archivos de compilación esenciales para Gradle, como *build.gradle.kts*, que se usa en el runtime para crear y configurar la aplicación.

```console
gradle init --type basic
```

Cuando se le solicite que elija un **DSL**, seleccione **Kotlin**.

Busque *build.gradle.kts* y ábralo con el IDE o el editor de texto que prefiera. A continuación, se copia en esta configuración de compilación:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingVisualSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-visualsearch:1.0.2-beta")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Cree una carpeta para la aplicación de ejemplo. En el directorio de trabajo, ejecute el siguiente comando:

```console
mkdir -p src/main/java
```

Cree una carpeta para la imagen que desea cargar en la API. Coloque la imagen dentro de la carpeta **resources**.

```console
mkdir -p src/main/resources
``` 

Vaya a la nueva carpeta y cree un archivo llamado *BingVisualSearchSample.java*. Ábralo en el editor o el IDE que prefiera y agregue las siguientes instrucciones `import`:

[!code-java[Import statements](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=imports)]

Luego, cree una clase.

```java
public class BingVisualSearchSample {
}
```

En el método `main` de la aplicación, cree variables para el punto de conexión y la clave de Azure del recurso. Si ha creado la variable de entorno después de haber iniciado la aplicación, deberá cerrar y volver a abrir el editor, el IDE o el shell que lo ejecuta para acceder a la variable. Después, cree un `byte[]` para la imagen que va a cargar. Cree un bloque `try` para los métodos que va a definir más adelante y cargue la imagen y conviértala en bytes mediante `toByteArray()`.

[!code-java[Main method](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=main)]


### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

En este inicio rápido se usa el administrador de dependencias Gradle. Puede encontrar la biblioteca de cliente y la información de otros administradores de dependencias en el [repositorio central de Maven](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/).

En el archivo *build.gradle.kts* del proyecto, asegúrese de incluir la biblioteca cliente como una instrucción `implementation`. 

```kotlin
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-visualsearch:1.0.2-beta")
    compile("com.google.code.gson:gson:2.8.5")
}
```

## <a name="code-examples"></a>Ejemplos de código

En estos fragmentos de código se muestra cómo realizar las siguientes tareas con la biblioteca cliente de Bing Visual Search y Java:

* [Autenticar el cliente](#authenticate-the-client)
* [Envío de una solicitud de búsqueda visual](#send-a-visual-search-request)
* [Impresión del token de Image Insight y etiquetas de búsqueda visual](#print-the-image-insight-token-and-visual-search-tags)

## <a name="authenticate-the-client"></a>Autenticar el cliente

> [!NOTE]
> En este inicio rápido se da por supuesto que ha [creado una variable de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la clave de Bing Visual Search, denominada `BING_SEARCH_V7_SUBSCRIPTION_KEY`.


En el método principal, asegúrese de usar la clave de suscripción para crear una instancia de un objeto [BingVisualSearchAPI](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.bingvisualsearchapi?view=azure-java-stable).

```csharp
BingVisualSearchAPI client = BingVisualSearchManager.authenticate(subscriptionKey);
```

## <a name="send-a-visual-search-request"></a>Envío de una solicitud de búsqueda visual

En un nuevo método, envíe la matriz de bytes de imagen (que se creó en el método `main()`) mediante el método [bingImages().visualSearch()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.bingimages.visualsearch?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_visualsearch_BingImages_visualSearch__) del cliente. 

[!code-java[visualSearch() method](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=visualSearch)]

## <a name="print-the-image-insight-token-and-visual-search-tags"></a>Impresión del token de Image Insight y etiquetas de búsqueda visual

Compruebe si el valor del objeto [ImageKnowledge](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.models.imageknowledge?view=azure-java-stable) es NULL. Si no lo es, imprima el token de información de la imagen, el número de etiquetas, el número de acciones y el primer tipo de acción.

[!code-java[Print token and tags](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=printVisualSearchResults)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Puede compilar la aplicación con:

```console
gradle build
```

Ejecute la aplicación con el objetivo `run`:

```console
gradle run
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Compilar una aplicación web de una sola página](tutorial-bing-visual-search-single-page-app.md)
