---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: erhopf
ms.openlocfilehash: a542e02c51bf2ce9b3b13bc50a39584ce079ec72
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78330842"
---
1. Inicie Eclipse.

1. En el selector de Eclipse, en el campo **Workspace** (Área de trabajo), escriba el nombre de un nuevo directorio de área de trabajo. Luego, seleccione **Launch** (Iniciar).

   ![Captura de pantalla del selector de Eclipse](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Al cabo de unos segundos, aparece la ventana principal del IDE de Eclipse. Cierre la pantalla de **bienvenida** si hay alguna.

1. En la barra de menús de Eclipse, cree un nuevo proyecto eligiendo **File (Archivo)**  > **New (Nuevo)**  > **Project (Proyecto)** .

1. Aparecerá el cuadro de diálogo **Nuevo proyecto** . Seleccione **Java Project** (Proyecto de Java) y seleccione **Next** (Siguiente).

   ![Captura de pantalla del cuadro de diálogo New Project (Nuevo proyecto), con el proyecto de Java resaltado](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-02-select-wizard.png)

1. Se inicia el asistente para **nuevo proyecto de Java**. En el campo **Project name** (Nombre del proyecto), escriba **quickstart** y elija **JavaSE-1.8** como el entorno de ejecución. Seleccione **Finalizar**.

   ![Captura de pantalla del asistente de nuevo proyecto de Java](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-03-create-java-project.png)

1. Si aparece una ventana titulada **Open Associated Perspective?** (¿Abrir perspectiva asociada?), seleccione **Open Perspective** (Abrir perspectiva).

1. En el **Explorador de paquetes**, haga clic en el proyecto **quickstart**. Elija **Configure (Configurar)**  > **Convert to Maven Project (Convertir en proyecto de Maven)** en el menú contextual.

   ![Captura de pantalla del Explorador de paquetes](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Aparece la ventana **Create new POM** (Crear nuevo POM). En el campo **Group Id** (Identificador de grupo), escriba *com.microsoft.cognitiveservices.speech.samples*, y en el campo **Artifact Id** (Identificador de artefacto), escriba *quickstart*. Después, seleccione **Finalizar**.

   ![Captura de pantalla de la pantalla Create new POM (Crear nuevo POM)](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Abra el archivo *pom.xml* y edítelo.

   * Al final del archivo, antes cerrar la etiqueta de cierre `</project>`, cree un elemento `repositories` con una referencia al repositorio de Maven para el SDK de Voz, tal y como se muestra aquí:

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/pom.xml#repositories)]

   * Agregue también un elemento `dependencies`, con la versión 1.10.0 del SDK de Voz como dependencia:

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/pom.xml#dependencies)]

   * Guarde los cambios.
