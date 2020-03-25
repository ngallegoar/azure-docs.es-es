---
author: tomarcher
ms.service: jenkins
ms.topic: include
ms.date: 03/03/2020
ms.author: tarcher
ms.openlocfilehash: e9b8ad7a7fcc499f8760b56e6a737be8a6a9e06c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79199775"
---
## <a name="prerequisites"></a>Prerequisites

* Una suscripción de Azure
* Acceso a SSH en la línea de comandos del equipo (por ejemplo, el shell de Bash o [PuTTY](https://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Creación de la máquina virtual de Jenkins a partir de la plantilla de solución
Jenkins admite un modelo en el cual el servidor de Jenkins delega el trabajo a uno o más agentes para permitir que una única instalación de Jenkins pueda hospedar un gran número de proyectos o proporcionar diferentes entornos necesarios para realizar compilaciones o pruebas. Los pasos descritos en esta sección le ayudarán a instalar y configurar un servidor de Jenkins en Azure.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-jenkins"></a>Conexión a Jenkins

Navegue a la máquina virtual (por ejemplo, `http://jenkins2517454.eastus.cloudapp.azure.com/`) en el explorador web. No se puede acceder a la consola de Jenkins a través de una HTTP no segura. Por ello, se proporcionan instrucciones en la página para acceder a la consola de forma segura desde su equipo mediante un túnel SSH.

![Desbloquear jenkins](./media/jenkins-install-solution-template-steps/jenkins-ssh-instructions.png)

Configure el túnel mediante el comando `ssh` en la página de la línea de comandos, reemplazando `username` por el nombre del usuario administrador de la máquina virtual elegido anteriormente al configurar la máquina virtual desde la plantilla de solución.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Después de haber iniciado el túnel, vaya a `http://localhost:8080/` en la máquina local. 

Obtenga la contraseña inicial mediante la ejecución del comando siguiente en la línea de comandos mientras está conectado a través de SSH a la máquina virtual de Jenkins.

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Desbloquee el panel de Jenkins por primera vez con la contraseña inicial.

![Desbloquear jenkins](./media/jenkins-install-solution-template-steps/jenkins-unlock.png)

Seleccione **Instalar los complementos sugeridos** en la siguiente página y, a continuación, cree un usuario administrador de Jenkins que se pueda utilizar para acceder al panel de este.

![Jenkins ya está listo.](./media/jenkins-install-solution-template-steps/jenkins-welcome.png)

El servidor Jenkins ya está preparado para compilar código.

## <a name="create-your-first-job"></a>Creación del primer trabajo

Seleccione **Create new jobs** (Crear nuevos trabajos) en la consola de Jenkins, a continuación, asígnele el nombre **mySampleApp** y seleccione **Freestyle project** (Proyecto de estilo libre) y, finalmente, seleccione **Aceptar**.

![Crear un trabajo](./media/jenkins-install-solution-template-steps/jenkins-new-job.png) 

Seleccione la pestaña **Source Code Management** (Administración del código fuente), habilite **Git** y escriba la siguiente dirección URL en el campo **Repository URL** (URL del repositorio): `https://github.com/spring-guides/gs-spring-boot.git`

![Definir el repositorio Git](./media/jenkins-install-solution-template-steps/jenkins-job-git-configuration.png) 

Seleccione la pestaña **Build** (Compilación), después, seleccione **Add build step** (Agregar el paso de compilación) y seleccione la opción **Invoke Gradle script** (Invocar script de Gradle). Seleccione **Use Gradle Wrapper** (Usar contenedor de Gradle). A continuación, escriba `complete` en **Wrapper location** (Ubicación del contenedor) y `build` en **Tasks** (Tareas).

![Usar el contenedor de Gradle para compilar](./media/jenkins-install-solution-template-steps/jenkins-job-gradle-config.png) 

Seleccione **Advanced** (Avanzado) y, a continuación, escriba `complete` en el campo **Root Build script** (Script de compilación raíz). Seleccione **Guardar**.

![Establecer configuraciones avanzadas en el paso de compilación del contenedor de Gradle](./media/jenkins-install-solution-template-steps/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Compilación del código

Seleccione **Build Now** (Compilar ahora) para compilar el código y empaquetar la aplicación de ejemplo. Cuando se complete la compilación, seleccione el vínculo del **área de trabajo** del proyecto.

![Vaya al área de trabajo para obtener el archivo JAR de la compilación](./media/jenkins-install-solution-template-steps/jenkins-access-workspace.png) 

Vaya a `complete/build/libs` y asegúrese de que `gs-spring-boot-0.1.0.jar` está allí para comprobar que la compilación se realizó correctamente. El servidor de Jenkins ya está listo para compilar sus propios proyectos de Azure.

## <a name="troubleshooting-the-jenkins-solution-template"></a>Solución de problemas de la plantilla de solución de Jenkins

Si detecta algún error con la plantilla de solución de Jenkins, registre un problema en [el repositorio de GitHub para Jenkins](https://github.com/azure/jenkins/issues).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Agregar máquinas virtuales de Azure como agentes de Jenkins](/azure/jenkins/jenkins-azure-vm-agents)