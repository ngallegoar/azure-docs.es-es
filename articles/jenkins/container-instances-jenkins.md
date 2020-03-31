---
title: Compilación de Jenkins en una instancia de contenedor
description: Obtenga información sobre cómo configurar un servidor de Jenkins para ejecutar trabajos de compilación a petición en Azure Container Instances
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: 8bb84895fb581053248fbad326ea7b2c8d1873a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617744"
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Uso de Azure Container Instances como agente de compilación de Jenkins

Azure Container Instances (ACI) proporciona un entorno a petición, ampliable y aislado para ejecutar cargas de trabajo en contenedores. Gracias a estos atributos, ACI es una plataforma excelente para ejecutar trabajos de compilación de Jenkins a gran escala. Este artículo le guía para implementar y usar un servidor Jenkins preconfigurado con ACI como destino de compilación.

Para más información sobre Azure Container Instances, consulte el artículo [acerca de Azure Container Instances](../container-instances/container-instances-overview.md).

## <a name="deploy-a-jenkins-server"></a>Implementación de un servidor Jenkins

1. En Azure Portal, seleccione **Crear un recurso** y busque **Jenkins**. Seleccione la oferta de Jenkins con un editor de **Microsoft** y, luego, seleccione **Crear**.

2. Escriba la siguiente información en el formulario de **conceptos básicos** y seleccione **Aceptar**.

   - **Name**: escriba un nombre para la implementación de Jenkins.
   - **Nombre de usuario**: escriba un nombre para el usuario administrador de la máquina virtual de Jenkins.
   - **Tipo de autenticación**: se recomienda usar una clave pública SSH para la autenticación. Si selecciona esta opción, pegue una clave pública SSH que, luego, se utilizará para iniciar sesión en la máquina virtual de Jenkins.
   - **Suscripción**: Seleccione una suscripción de Azure.
   - **Grupo de recursos**: Cree un grupo de recursos o seleccione uno existente.
   - **Ubicación**: seleccione una ubicación para el servidor de Jenkins.

   ![Configuración básica para implementar el portal de Jenkins](./media/container-instances-jenkins/jenkins-portal-01.png)

3. En el formulario de **Configuración adicional**, complete los siguientes elementos:

   - **Size**: seleccione la opción de tamaño correcta para la máquina virtual de Jenkins.
   - **Tipo de disco de máquina virtual**: especifique la unidad **HDD** (unidad de disco duro) o **SSD** (unidad de estado sólido) para el servidor de Jenkins.
   - **Red virtual**: seleccione la flecha si quiere modificar la configuración predeterminada.
   - **Subredes**: seleccione la flecha, compruebe la información y seleccione **Aceptar**.
   - **Dirección IP pública**: seleccione la flecha para otorgar un nombre personalizado a la dirección IP pública, configurar la SKU y establecer el método de asignación.
   - **Etiqueta de nombre de dominio**: especifique un valor para crear una dirección URL completa en la máquina virtual de Jenkins.
   - **Tipo de versión de Jenkins**: seleccione el tipo de versión que le convenga entre las siguientes opciones: **LTS**, **Compilación semanal** o **Comprobados por Azure**.

   ![Configuración adicional para implementar el portal de Jenkins](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Para la integración de la entidad de servicio, seleccione **Auto(MSI)** para que las [identidades administradas de los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) creen automáticamente una identidad de autenticación para la instancia de Jenkins. Seleccione **Manual** para obtener credenciales de entidad de servicio propias.

5. Los agentes de la nube configuran una plataforma basada en la nube para los trabajos de compilación de Jenkins. Para este artículo, seleccione **ACI**. Con el agente en la nube ACI, cada trabajo de compilación de Jenkins se ejecuta en una instancia de contenedor.

   ![Configuración de la integración en la nube para la implementación del portal de Jenkins](./media/container-instances-jenkins/jenkins-portal-03.png)

6. Cuando la configuración de la integración haya finalizado, seleccione **Aceptar** y, luego, seleccione **Aceptar** de nuevo en el resumen de validación. Seleccione **Crear** en el resumen de **Términos de uso**. El servidor Jenkins tarda unos minutos en implementarse.

## <a name="configure-jenkins"></a>Configuración de Jenkins

1. En Azure Portal, vaya al grupo de recursos de Jenkins, seleccione la máquina virtual de Jenkins y tome nota del nombre de DNS.

   ![Nombre de DNS en detalles acerca de la máquina virtual de Jenkins](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Vaya al nombre DNS de la VM de Jenkins y copie la cadena de SSH que se devuelve.

   ![Instrucciones de inicio de sesión en Jenkins con la cadena de SSH](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Abra una sesión en el terminal con su sistema de desarrollo y pegue la cadena de SSH del paso anterior. Actualice `username` con el nombre de usuario especificado cuando se implementó el servidor Jenkins.

4. Una vez que se haya conectado la sesión, ejecute el siguiente comando para recuperar la contraseña de administrador inicial:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. Salga de la sesión de SSH y de la ejecución en túnel, y utilice un explorador para ir a `http://localhost:8080`. Pegue la contraseña de administrador inicial en el cuadro y, luego, seleccione **Continuar**.

   ![Pantalla "Desbloquear Jenkins" con el cuadro de dialogo de la contraseña de administrador](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Seleccione **Install suggested plugins** (Instalar complementos sugeridos) para instalar todos los complementos de Jenkins recomendados.

   ![Pantalla "Personalizar Jenkins" con la opción "Instalar complementos sugeridos" seleccionada](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Creación de una cuenta de usuario administrador. Esta cuenta se usa para iniciar sesión en la instancia de Jenkins y trabajar con ella.

   ![Pantalla "Creación del primer usuario administrador", donde se han rellenado las credenciales](./media/container-instances-jenkins/jenkins-portal-07.png)

8. Seleccione **Save and Finish** (Guardar y finalizar) y, luego, seleccione **Start using Jenkins** (Empezar a usar Jenkins), para completar la configuración.

Jenkins ahora está configurado y listo para compilar e implementar código. En este ejemplo se usa una sencilla aplicación Java para mostrar una compilación de Jenkins en Azure Container Instances.

## <a name="create-a-build-job"></a>Creación de un trabajo de compilación

Ahora se crea un trabajo de compilación de Jenkins para mostrar que Jenkins se basa en una instancia Azure Container Instances.

1. Seleccione **New Item** (Nuevo elemento), asigne un nombre al proyecto de compilación, como **aci-demo**, seleccione **Freestyle project** (Proyecto de estilo libre) y, luego, seleccione **OK** (Aceptar).

   ![Cuadro de dialogo para el nombre del trabajo de compilación y la lista de tipos de proyectos](./media/container-instances-jenkins/jenkins-new-job.png)

2. En **General** (General), asegúrese de que **Restrict where this project can be run** (Restringir el ámbito de ejecución del proyecto) está seleccionado. Escriba **linux** como expresión de la etiqueta. Esta configuración garantiza que este trabajo de compilación se ejecuta en la nube de ACI.

   ![Ficha "General" (General) donde se muestran los detalles de configuración](./media/container-instances-jenkins/jenkins-job-01.png)

3. En **Build** (Compilar), seleccione **Add build step** (Agregar paso de compilación) y seleccione **Execute Shell** (Ejecutar Shell). Escriba `echo "aci-demo"` como el comando.

   ![Pestaña "Build" (Compilar) con las respectivas selecciones del paso de compilación](./media/container-instances-jenkins/jenkins-job-02.png)

5. Seleccione **Guardar**.

## <a name="run-the-build-job"></a>Ejecución del trabajo de compilación

Para probar el trabajo de compilación y tener Azure Container Instances como plataforma de compilación, inicie una compilación manualmente.

1. Seleccione **Build Now** (Compilar ahora) para iniciar un trabajo de compilación. El trabajo tarda unos minutos en iniciarse. Debería ver un estado similar a la siguiente imagen:

   ![Información de "Build History" (Historial de compilación) donde se muestra el estado de trabajo](./media/container-instances-jenkins/jenkins-job-status.png)

2. Con el trabajo en ejecución, abra Azure Portal y observe el grupo de recursos de Jenkins. Debería ver que se ha creado una instancia del contenedor. El trabajo de Jenkins se ejecuta en esta instancia.

   ![Instancia del contenedor del grupo de recursos](./media/container-instances-jenkins/jenkins-aci.png)

3. Como Jenkins ejecuta más trabajos que el número configurado de ejecutores de Jenkins (el valor predeterminado es 2), se crean varias instancias de contenedor.

   ![Instancias de contenedor recién creadas](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Cuando todos los trabajos de compilación hayan concluido, las instancias de contenedor se eliminan.

   ![Grupo de recursos donde se han quitado las instancias de contenedor](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Solución de problemas del complemento de Jenkins

Si detecta algún error con los complementos de Jenkins, envíe un problema en [Jenkins JIRA](https://issues.jenkins-ci.org/) para el componente específico.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [CI/CD en Azure App Service](java-deploy-webapp-tutorial.md)