---
title: 'Integración continua e implementación continua: Azure IoT Edge'
description: 'Configuración de integración continua e implementación continua: Azure IoT Edge con Azure DevOps, Azure Pipelines'
author: shizn
manager: philmea
ms.author: xshi
ms.date: 08/20/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ac37e9bd10caea5c6e58fc797eac73ce6c714162
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561031"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integración continua e implementación continua en Azure IoT Edge

Puede adoptar fácilmente DevOps con las aplicaciones de Azure IoT Edge con las tareas integradas de Azure IoT Edge en Azure Pipelines. En este artículo se muestra cómo puede usar las características de integración continua e implementación continua de Azure Pipelines para compilar, probar e implementar aplicaciones de forma rápida y eficaz en su instancia de Azure IoT Edge.

![Diagrama: ramas CI y CD para desarrollo y producción](./media/how-to-ci-cd/cd.png)

En este artículo, aprenderá a usar las tareas integradas de Azure IoT Edge para Azure Pipelines para crear dos canalizaciones para la solución de IoT Edge. En las tareas Azure IoT Edge se pueden usar cuatro acciones.

* **Azure IoT Edge: Build Module images** usa el código de su solución de IoT Edge y crea las imágenes de contenedor.
* **Azure IoT Edge: Push Module images** inserta las imágenes del módulo en el registro de contenedor especificado.
* **Azure IoT Edge: Generate Deployment Manifest** usa el archivo deployment.template.json y sus variables para generar el archivo final de manifiesto de implementación de IoT Edge.
* **Azure IoT Edge: Deploy to IoT Edge devices** ayuda a crear implementaciones de IoT Edge en varios dispositivos IoT Edge o en uno solo.

## <a name="prerequisites"></a>Prerrequisitos

* Un repositorio de Azure Repos. Si no tiene uno, puede [crear un nuevo repositorio de Git en el proyecto](https://docs.microsoft.com/azure/devops/repos/git/create-new-repo?view=vsts&tabs=new-nav).
* Una solución de IoT Edge confirmada e insertada en el repositorio. Si quiere crear una nueva solución de ejemplo para hacer pruebas según este artículo, siga los pasos para [desarrollar y depurar módulos en Visual Studio Code](how-to-vs-code-develop-module.md) o [desarrollar y depurar módulos de C# en Visual Studio](how-to-visual-studio-develop-csharp-module.md).

   En este artículo, todo lo que necesita es la carpeta de la solución creada mediante las plantillas de IoT Edge en Visual Studio Code o Visual Studio. No es necesario compilar, insertar, implementar ni depurar este código antes de continuar. Configurará esos procesos en Azure Pipelines.

   Si está creando una nueva solución, clone el repositorio localmente en primer lugar. A continuación, cuando cree la solución, puede elegir crearlo directamente en la carpeta del repositorio. Puede confirmar e insertar fácilmente los nuevos archivos desde allí.

* Un registro de contenedor donde pueda insertar imágenes del módulo. Puede usar [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o un registro de terceros.
* Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) activo con al menos dispositivos de IoT Edge para hacer pruebas de las fases independientes de implementación de prueba y producción. Puede seguir los artículos de la guía de inicio rápido para crear un dispositivo IoT Edge en [Linux](quickstart-linux.md) o [Windows](quickstart.md)

Para más información sobre el uso Azure Repos, consulte [Share your code with Visual Studio and Azure Repos](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts) (Compartir el código con Visual Studio y Azure Repos).

## <a name="configure-continuous-integration"></a>Configuración de la integración continua

En esta sección, creará una nueva canalización de compilación. Configure la canalización para que se ejecute automáticamente cuando se inserte algún cambio en la solución de IoT Edge de ejemplo y publique registros de compilación.

>[!NOTE]
>En este artículo se usa el diseñador visual de Azure DevOps. Antes de seguir los pasos descritos en esta sección, desactive la característica en vista previa para la nueva experiencia de creación de canalizaciones YAML.
>
>1. En Azure DevOps, seleccione el icono del perfil y seleccione **Características de vista previa**.
>2. Desactive **Nueva experiencia de creación de canalizaciones de YAML**.
>
>Para más información, consulte [Create a build pipeline](https://docs.microsoft.com/azure/devops/pipelines/create-first-pipeline) (Creación de una canalización de compilación).

1. Inicie sesión en la organización de Azure DevOps (**https:\//dev.azure.com/{su organización}/** ) y abra el proyecto que contiene el repositorio de soluciones de IoT Edge.

   En este artículo, hemos creado un repositorio denominado **IoTEdgeRepo**. Ese repositorio contiene **IoTEdgeSolution**, que tiene el código de un módulo denominado **filtermodule**.

   ![Abrir el proyecto de DevOps](./media/how-to-ci-cd/init-project.png)

2. Vaya a Azure Pipelines en el proyecto. Abra la pestaña **Compilaciones** y seleccione **Nueva canalización**. O bien, si ya tiene canalizaciones de compilación, seleccione el botón **Nuevo**. Luego elija **Nueva canalización de compilación**.

    ![Creación de una canalización de compilación](./media/how-to-ci-cd/add-new-build.png)

3. Siga las indicaciones para crear la canalización.

   1. Proporcione la información de origen para la nueva canalización de compilación. Seleccione **GIT de Azure Repos** como origen y, luego, seleccione el proyecto, el repositorio y la rama donde está ubicado el código de la solución de IoT Edge. Después, seleccione **Continuar**.

      ![Seleccione el origen de la canalización](./media/how-to-ci-cd/pipeline-source.png)

   2. Seleccione **Fase vacía** en lugar de una plantilla.

      ![Empezar con un proceso vacío](./media/how-to-ci-cd/start-with-empty.png)

4. Una vez creada la canalización, irá al editor de canalizaciones. En la descripción de la canalización, elija el grupo de agentes correcto en función de la plataforma de destino:

   * Elija si desea compilar los módulos en la plataforma amd64 para contenedores Linux, elija **Hosted Ubuntu 1604**.

   * Si quiere compilar los módulos en la plataforma amd64 para contenedores de Windows 1809, deberá [configurar un agente autohospedado en Windows](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts).

   * Si quiere compilar los módulos en la plataforma arm32v7 o arm64 para contenedores de Linux, deberá [configurar un agente autohospedado en Linux](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/).

     ![Configuración del grupo de agentes de compilación](./media/how-to-ci-cd/configure-env.png)

5. La canalización viene preconfigurada con un trabajo denominado **Trabajo del agente 1**. Seleccione el signo de más ( **+** ) para agregar tres tareas al trabajo: **Azure IoT Edge** dos veces, **Copiar archivos** una vez y **Publicar los artefactos de la compilación** una vez. (Mantenga el mouse sobre el nombre de cada tarea para ver el botón **Agregar**).

   ![Agregar la tarea Azure IoT Edge](./media/how-to-ci-cd/add-iot-edge-task.png)

   Cuando se hayan agregado las cuatro tareas, el Trabajo del agente se verá similar al ejemplo siguiente:

   ![Las tres tareas en la canalización de compilación](./media/how-to-ci-cd/add-tasks.png)

6. Seleccione la primera tarea **Azure IoT Edge** para editarla. Esta tarea compila todos los módulos de la solución con la plataforma de destino que se especificó.

   * **Nombre para mostrar**: Acepte el valor predeterminado **Azure IoT Edge - Build module images**.
   * **Acción**: Acepte el valor predeterminado **Build module images**.
   * **Archivo .template.json**: Seleccione los puntos suspensivos ( **...** ) y navegue hasta el archivo **deployment.template.json** en el repositorio que contiene la solución de IoT Edge.
   * **Plataforma predeterminada**: Seleccione la plataforma adecuada para los módulos en función de su dispositivo de IoT Edge de destino.
   * **Variables de salida**: Las variables de salida incluyen un nombre de referencia que puede usar para configurar la ruta de acceso del archivo donde se generará el archivo deployment.json. Establezca el nombre de referencia en algo fácil de recordar, como **edge**.


   Estas configuraciones usan el repositorio de imágenes y la etiqueta que se definen en el archivo `module.json` para asignar un nombre a la imagen del módulo y etiquetarla. **Compilar imágenes del módulo** también ayuda a reemplazar las variables por el valor exacto que defina en el archivo `module.json`. En Visual Studio o Visual Studio Code, está especificando el valor real en un archivo `.env`. En Azure Pipelines, el valor se establece en la pestaña **Variables de canalización**. Seleccione la pestaña **Variables** y configure el nombre y el valor de la siguiente manera:

    * **ACR_ADDRESS**: la dirección de Azure Container Registry. 

    Si tiene otras variables en el proyecto, puede especificar el nombre y el valor en esta pestaña. **Build module images** (Compilar imágenes del módulo) solo reconoce las variables que están en formato `${VARIABLE}`. Asegúrese de usar este formato en los archivos de `**/module.json`.
    
7. Seleccione la segunda tarea **Azure IoT Edge** para editarla. Esta tarea insertará todas las imágenes del módulo en el registro de contenedor que seleccionó.

   * **Nombre para mostrar**: El nombre para mostrar se actualiza automáticamente cuando cambia el campo de acción.
   * **Acción**: Use la lista desplegable para seleccionar **Push module images**.
   * **Tipo de registro de contenedor**: Seleccione el tipo de registro de contenedor que usa para almacenar las imágenes del módulo. Dependiendo del tipo de registro que elija, el formulario cambia. Si elige **Azure Container Registry**, use las listas desplegables para seleccionar la suscripción de Azure y el nombre del registro de contenedor. Si elige **Generic Container Registry** (Registro de contenedor genérico), seleccione **Nuevo** para crear una conexión del servicio del registro.
   * **Archivo .template.json**: Seleccione los puntos suspensivos ( **...** ) y navegue hasta el archivo **deployment.template.json** en el repositorio que contiene la solución de IoT Edge.
   * **Plataforma predeterminada**: Seleccione la misma plataforma que las imágenes del módulo compilado.

   Si tiene varias instancias de Container Registry para hospedar las imágenes de módulo, tiene que duplicar esta tarea, seleccionar otra instancia de Container Registry y usar **Bypass module(s)** (Omitir módulos) en la configuración avanzada para omitir las imágenes que no son de este registro específico.

8. Seleccione la tarea **Copiar archivos** para editarla. Use esta tarea para copiar archivos en el directorio de almacenamiento provisional de artefactos.

   * **Nombre para mostrar**: Copiar archivos en: carpeta de entrega.
   * **Contenido**: Ponga dos líneas en esta sección, `deployment.template.json` y `**/module.json`. Estos dos tipos de archivos son las entradas para generar el manifiesto de implementación de IoT Edge. Debe copiarse en la carpeta de almacenamiento provisional de artefactos y publicarse para la canalización de versiones.
   * **Carpeta de destino**: coloque la variable `$(Build.ArtifactStagingDirectory)`. Consulte las [variables de compilación](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#build-variables) para obtener información acerca de sus descripciones.

9. Seleccione la tarea **Publicar los artefactos de la compilación** para editarla. Proporcione la ruta de acceso del directorio de almacenamiento provisional de artefactos a la tarea para que la ruta de acceso pueda publicarse en la canalización de versión.

   * **Nombre para mostrar**: Publicar artefacto: drop.
   * **Ruta de acceso para publicar**: coloque la variable `$(Build.ArtifactStagingDirectory)`. Consulte las [variables de compilación](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml#build-variables) para obtener información acerca de sus descripciones.
   * **Nombre del artefacto**: drop.
   * **Ubicación de publicación de artefactos**: Azure Pipelines.

10. Abra la pestaña **Desencadenadores** y marque la casilla para **Habilitar la integración continua**. Asegúrese de que se incluye la rama que contiene el código.

    ![Activación del desencadenador de integración continua](./media/how-to-ci-cd/configure-trigger.png)

11. Guarde la nueva canalización de compilación con el botón **Guardar**.

Esta canalización ahora está configurada para ejecutarse automáticamente al insertar código nuevo en el repositorio. La última tarea, la publicación de los artefactos de la canalización, desencadena una canalización de versión. Continúe con la sección siguiente para compilar la canalización de versión.

## <a name="configure-continuous-deployment"></a>Configuración de la implementación continua

En esta sección, se crea una canalización de versión que está configurada para ejecutarse automáticamente cuando la canalización de compilación coloca artefactos, y mostrará los registros de implementación en Azure Pipelines.

Creación de una nueva canalización y adición de una nueva fase

1. En la pestaña **Versiones**, elija **+ Nueva canalización**. O bien, si ya tiene canalizaciones de versión, elija el botón **+ Nuevo** y seleccione **+ Nueva canalización de versión**.  

    ![Agregar canalización de versión](./media/how-to-ci-cd/add-release-pipeline.png)

2. Cuando se le pida que seleccione una plantilla, elija empezar con una **Fase vacía**.

    ![Empezar con un trabajo vacío](./media/how-to-ci-cd/start-with-empty-job.png)

3. La nueva canalización de versión se inicializa con una fase, llamada **Fase 1**. Cambie el nombre de Fase 1 a **dev** y trátela como un entorno de prueba. Por lo general, las canalizaciones de implementación continua tienen varias fases, incluidas **dev**, **staging** y **prod**. Puede crear más en función de sus prácticas de DevOps. Cierre la ventana de detalles de la fase una vez que haya cambiado el nombre.

4. Vincule la versión con los artefactos de compilación que publica la canalización de compilación. Haga clic en **Agregar** en el área de artefactos.

   ![Agregar artefactos](./media/how-to-ci-cd/add-artifacts.png)  

5. En la página **Agregar un artefacto**, seleccione el tipo de origen **Compilar**. A continuación, seleccione el proyecto y la canalización de compilación que ha creado. Después, seleccione **Agregar**.

   ![Adición de un artefacto de compilación](./media/how-to-ci-cd/add-an-artifact.png)

6. Abra los desencadenadores de artefacto y seleccione el botón de alternancia para habilitar el desencadenador de implementación continua. Ahora, se creará una nueva versión cada vez que haya disponible una nueva compilación.

   ![Configuración del desencadenador de implementación continua](./media/how-to-ci-cd/add-a-trigger.png)

7. La fase **dev** está preconfigurada con un trabajo y cero tareas. En el menú de la canalización, seleccione **Tareas** y, a continuación, elija la fase **dev**.  Seleccione el recuento de trabajos y tareas para configurar las tareas en esta fase.

    ![Configuración de las tareas de dev](./media/how-to-ci-cd/view-stage-tasks.png)

8. En la fase **dev**, debería ver un **Trabajo del agente** predeterminado. Puede configurar los detalles sobre el trabajo del agente, pero la tarea de implementación no distingue la plataforma, por lo que puede elegir **Hosted VS2017** o **Hosted Ubuntu 1604** en el **Grupo de agentes** (o cualquier otro agente administrado por usted mismo).

9. Haga clic en el signo más ( **+** ) para agregar dos tareas. Busque y agregue **Azure IoT Edge** dos veces.

    ![Adición de tareas para dev](./media/how-to-ci-cd/add-task-qa.png)

10. Seleccione la primera tarea de **Azure IoT Edge** y configúrela con los valores siguientes:

    * **Nombre para mostrar**: El nombre para mostrar se actualiza automáticamente cuando cambia el campo de acción.
    * **Acción**: Use la lista desplegable para seleccionar **Generate deployment manifest** (Generar manifiesto de implementación). Al cambiar el valor de la acción, también se actualiza el nombre para mostrar de la tarea para que coincidan.
    * **Archivo .template.json**: especifique la ruta de acceso `$(System.DefaultWorkingDirectory)/Drop/drop/deployment.template.json`. La ruta de acceso se publica desde la canalización de compilación.
    * **Plataforma predeterminada**: elija el mismo valor al compilar las imágenes del módulo.
    * **Ruta de acceso de salida**: especifique la ruta de acceso `$(System.DefaultWorkingDirectory)/Drop/drop/configs/deployment.json`. Esta es la ruta de acceso del archivo de manifiesto de implementación de IoT Edge final.

    Estas configuraciones ayudan a reemplazar las direcciones URL del módulo en el archivo `deployment.template.json`. **Generate deployment manifest** (Generar manifiesto de implementación) también ayuda a reemplazar las variables con el valor exacto que especificó en el archivo `deployment.template.json`. En VS o VS Code, el valor real se especifica en un archivo `.env`. En Azure Pipelines, el valor se establece en la pestaña Variables de canalización de versión. Vaya a la pestaña Variables y configure el Nombre y Valor de la siguiente manera.

    * **ACR_ADDRESS**: la dirección de Azure Container Registry.
    * **ACR_PASSWORD**: la contraseña de Azure Container Registry.
    * **ACR_USER**: el nombre de usuario de Azure Container Registry.

    Si tiene otras variables en el proyecto, puede especificar el nombre y el valor en esta pestaña. **Generate deployment manifest** (Generar manifiesto de implementación) solo puede reconocer las variables del tipo `${VARIABLE}`, por lo que debe asegurarse de usar este formato en sus archivos `*.template.json`.

    ![Configuración de variables para la canalización de versiones](./media/how-to-ci-cd/configure-variables.png)

11. Seleccione la segunda tarea de **Azure IoT Edge** y configúrela con los valores siguientes:

    * **Nombre para mostrar**: El nombre para mostrar se actualiza automáticamente cuando cambia el campo de acción.
    * **Acción**: Use la lista desplegable para seleccionar **Deploy to IoT Edge device** (Implementar en dispositivos IoT Edge). Al cambiar el valor de la acción, también se actualiza el nombre para mostrar de la tarea para que coincidan.
    * **Suscripción de Azure**: Seleccione la suscripción que contiene el centro de IoT.
    * **Nombre de la instancia de IoT Hub**: Seleccione IoT Hub.
    * **Choose single/multiple device** (Elegir dispositivo único/varios): Elija si quiere que la canalización de versión se implemente en uno o varios dispositivos.
      * Si implementa en un único dispositivo, escriba el **IoT Edge device ID** (Id. de dispositivo IoT Edge).
      * Si va a implementar en varios dispositivos, especifique la **condición de destino** del dispositivo. La condición de destino es un filtro para asociar un conjunto de dispositivos de IoT Edge en IoT Hub. Si quiere usar etiquetas de dispositivo como condición, debe actualizar las etiquetas de dispositivo correspondientes con el dispositivo gemelo de IoT Hub. Actualice **IoT Edge deployment ID** (Id. de implementación de IoT Edge) e **IoT Edge deployment priority** (Prioridad de implementación de IoT Edge) en la configuración avanzada. Para más información acerca de cómo crear una implementación para varios dispositivos, consulte [Descripción de las implementaciones automáticas de IoT Edge](module-deployment-monitoring.md).
    * Expanda Configuración avanzada, seleccione **Id. de implementación de IoT Edge** y coloque la variable `$(System.TeamProject)-$(Release.EnvironmentName)`. De esta forma, el proyecto y el nombre de la versión se asignan con el id. de implementación de IoT Edge.

12. Seleccione **Guardar** para guardar los cambios en la nueva canalización de versión. Vuelva a la vista de canalización seleccionando **Canalización** en el menú.

## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Comprobación de CI/CD de IoT Edge con las canalizaciones de compilación y de versión

Para desencadenar un trabajo de compilación, puede insertar una confirmación en el repositorio de código fuente o desencadenarlo manualmente. En esta sección, desencadenará manualmente la canalización de CI/CD para probar que funcione. Luego verifique que la implementación se complete correctamente.

1. Vaya a la canalización de compilación que creó al principio de este artículo.

2. Para desencadenar un trabajo de compilación en la canalización de compilación puede seleccionar el botón **Cola**, como en la siguiente captura de pantalla.

    ![Desencadenador manual](./media/how-to-ci-cd/manual-trigger.png)

3. Seleccione el trabajo de compilación para ver su progreso. Si la canalización de compilación se completa correctamente, se desencadena una versión en la fase **dev**.

    ![Registros de compilación](./media/how-to-ci-cd/build-logs.png)

4. La versión **dev** correcta crea una implementación de IoT Edge en los dispositivos IoT Edge de destino.

    ![Versión de dev](./media/how-to-ci-cd/pending-approval.png)

5. Haga clic en la fase **dev** para ver los registros de versión.

    ![Registros de versión](./media/how-to-ci-cd/release-logs.png)

## <a name="next-steps"></a>Pasos siguientes

* Ejemplo de procedimientos recomendados de DevOps en IoT Edge en [Proyecto de Azure DevOps para IoT Edge](how-to-devops-project.md)
* Puede encontrar información sobre la implementación de IoT Edge en [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).
* Siga los pasos para crear, actualizar o eliminar una implementación en [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-at-scale.md).
