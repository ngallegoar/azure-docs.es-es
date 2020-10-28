---
title: 'Tutorial: Implementación de una aplicación de Node.js en una aplicación web de Azure mediante DevOps Starter para Acciones de GitHub'
description: DevOps Starter facilita empezar a trabajar con Azure e implementar la aplicación de Node.js en una aplicación web de Azure en pocos pasos.
ms.author: ninallam
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 08/25/2020
author: ninallam
ms.openlocfilehash: 9a2abf7e714b75f2551a35a220e30c2465d86a49
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92332572"
---
# <a name="tutorial-deploy-nodejs-app-to-azure-web-app-using-devops-starter-for-github-actions"></a>Tutorial: Implementación de una aplicación de Node.js en una aplicación web de Azure mediante DevOps Starter para Acciones de GitHub

DevOps Starter para Acciones de GitHub ofrece una experiencia simplificada en la que puede elegir una aplicación de ejemplo para crear un flujo de trabajo de integración continua (CI) y entrega continua (CD) para la implementación en Azure. 

DevOps Starter también:
* Crea automáticamente recursos de Azure, como una aplicación web de Azure.
* Crea y configura un flujo de trabajo en GitHub que incluye un trabajo de compilación para CI.
* El flujo de trabajo también contiene un trabajo de implementación para CD. 
* Crea un recurso de Azure Application Insights para la supervisión.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Uso de DevOps Starter para implementar una aplicación de Node.js
> * Configuración de GitHub y una suscripción de Azure 
> * Examen del flujo de trabajo de GitHub
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Configurar la supervisión de Azure Application Insights
> * Limpieza de recursos

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="use-devops-starter-to-deploy-a-nodejs-app"></a>Uso de DevOps Starter para implementar una aplicación de Node.js

DevOps Starter crea un flujo de trabajo en GitHub. Puede usar una organización de GitHub existente. DevOps Starter también crea recursos de Azure, como una aplicación web, en la suscripción de Azure de su elección.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el cuadro de búsqueda, escriba **DevOps Starter** y, después, selecciónelo. Haga clic en **Agregar** para crear un recurso.

    ![Panel de DevOps Starter](_img/azure-devops-starter-aks/search-devops-starter.png)

1. Asegúrese de que el proveedor de CI/CD seleccionado es **Acciones de GitHub** .

    ![CICD_provider](_img/azure-devops-project-nodejs/provider-selection.png)

1. Seleccione **Node.js** y, a continuación, seleccione **Siguiente** .

1. En **Elegir un marco de trabajo de la aplicación** , seleccione **Express.js** y, a continuación, seleccione **Siguiente** . El marco de trabajo de la aplicación que eligió en un paso anterior, determina el tipo de destino de implementación del servicio de Azure que está disponible aquí. 

1. Seleccione **Aplicación web de Windows** y, a continuación, seleccione **Siguiente** .

## <a name="configure-github-and-an-azure-subscription"></a>Configuración de GitHub y una suscripción de Azure

1. **Autorice** a GitHub y seleccione una organización de GitHub existente. 

1. Escriba un nombre para el **Repositorio de GitHub** . 

1. Seleccione los servicios de la suscripción a Azure. Si lo desea, puede seleccionar **Cambiar** y, luego, especificar más detalles de la configuración, como la ubicación de los recursos de Azure.
 
1. Escriba un nombre de aplicación web y, a continuación, seleccione **Listo** . Después de unos minutos, la aplicación web de Azure estará lista. Se configura una aplicación de Node.js de ejemplo en un repositorio de la organización de GitHub, se desencadena un flujo de trabajo y la aplicación se implementa en la aplicación web de Azure recién creada.

   ![Enter_GH_details](_img/azure-devops-project-nodejs/gh-details.png)    

   Al finalizar, el panel de DevOps Starter se muestra en Azure Portal. También puede ir al panel directamente desde **Todos los recursos** en Azure Portal. 

   El panel proporciona visibilidad del repositorio de código de GitHub, el flujo de trabajo de CI/CD y la aplicación que se ejecuta en Azure.   

   ![Vista de panel](_img/azure-devops-project-nodejs/full-dashboard.png)

DevOps Starter configura automáticamente un desencadenador que implementa en el repositorio los cambios realizados en el código.
    
## <a name="examine-the-github-workflow"></a>Examen del flujo de trabajo de GitHub

En el paso anterior, DevOps Starter configuró automáticamente un flujo de trabajo completo de GitHub. Explore y personalice el flujo de trabajo según sea necesario. Siga estos pasos para familiarizarse con el flujo de trabajo.

1. A la izquierda del panel de DevOps Starter, seleccione **GitHub workflow** (Flujo de trabajo de GitHub). Este vínculo abre una pestaña del explorador y el flujo de trabajo de GitHub del nuevo proyecto.
    > [!NOTE]
    > No cambie el nombre del archivo de flujo de trabajo. Este nombre debe ser **devops-starter-workflow.yml** para que el panel refleje los cambios.

1. El archivo YAML del flujo de trabajo contiene todas las acciones de GitHub necesarias para compilar e implementar la aplicación. Haga clic en la opción **Edit file** (Editar archivo) para personalizar el archivo de flujo de trabajo.

1. En la pestaña **Code** (Código) del repositorio, haga clic en **Commits** (Confirmaciones). Esta vista muestra las confirmaciones de código asociadas a la implementación concreta.

1. En la pestaña **Actions** (Acciones) del repositorio, puede ver el historial de todas las ejecuciones de flujos de trabajo del repositorio.

1. Seleccione **Latest run** (Última ejecución) para ver todos los trabajos que se ejecutaron en el flujo de trabajo.

1. Haga clic en los **trabajos** para ver los registros detallados de la ejecución del flujo de trabajo. Los registros contienen información útil sobre el proceso de implementación. Pueden verse durante y después de las implementaciones.

1. Haga clic en la pestaña **Pull request** (Solicitud de incorporación de cambios) para ver todas las solicitudes de incorporación de cambios que hay en el repositorio.

## <a name="commit-code-changes-and-execute-cicd"></a>Confirmación de los cambios de código y ejecución de CI/CD

DevOps Starter crea un repositorio en GitHub. Para ver el repositorio y realizar cambios en el código de la aplicación, siga estos pasos:

1. En el lado izquierdo del panel de DevOps Starter, seleccione el vínculo de la rama maestra. Este vínculo abre una vista al repositorio de GitHub recién creado.

1. Para ver la dirección URL de clonación del repositorio, seleccione **Clonar** en la parte superior derecha del explorador. Puede clonar el repositorio de Git en su IDE favorito. En los pasos siguientes, puede usar el explorador web para realizar los cambios en el código y confirmarlos directamente en la rama maestra.

1. En el lado izquierdo del explorador, vaya al archivo **/Application/views/index.pug** .

1. Seleccione **Editar** y realice un cambio en alguna parte del texto.
    Por ejemplo, cambie parte del texto de una de las etiquetas.

1. Seleccione **Confirmar** y guarde los cambios.

1. En el explorador, vaya al panel de DevOps Starter.   
Ahora, debería ver un trabajo de compilación del flujo de trabajo de GitHub en curso. Los cambios que acaba de realizar se compilan e implementan automáticamente mediante un flujo de trabajo de GitHub.

1. Una vez que se complete la implementación, actualice la aplicación para comprobar los cambios.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar la supervisión de Azure Application Insights

Con Azure Application Insights puede supervisar fácilmente el rendimiento y la utilización de la aplicación. DevOps Starter configura automáticamente un recurso de Application Insights para la aplicación. Puede seguir configurando diversas alertas y supervisando las funcionalidades según sea necesario.

1. En Azure Portal, vaya al panel de DevOps Starter. 

1. En la parte inferior derecha, seleccione el vínculo de **Application Insights** para la aplicación. Se abre el panel **Application Insights** . Esta vista contiene información sobre la supervisión de la disponibilidad, el rendimiento y el uso de la aplicación.

   ![El panel Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Seleccione **Intervalo de tiempo** y, después, elija **Última hora** . Para filtrar los resultados, seleccione **Actualizar** . Ya puede ver toda la actividad de los últimos 60 minutos. 
    
1. Para salir del intervalo de tiempo, seleccione **x** .

1. Seleccione **Alertas** y, después, **Agregar alerta de métrica** . 

1. Escriba el nombre de la alerta.

1. En la lista desplegable **Métrica** , examine las distintas métricas de alertas. La alerta predeterminada es para un **tiempo de respuesta de servidor mayor de 1 segundo** . Puede configurar fácilmente diversas alertas para mejorar las funcionalidades de supervisión de la aplicación.

1. Seleccione la casilla **Notify via Email owners, contributors, and readers** (Notificar a través de correo electrónico a lectores, colaboradores y propietarios). Si lo desea, puede realizar acciones adicionales cuando se muestre una alerta mediante la ejecución de una aplicación de lógica de Azure.

1. Seleccione **Aceptar** para crear la alerta. Poco después la alerta aparece como activa en el panel. 

1. Salga del área **Alertas** y vuelva al panel **Application Insights** .

1. Seleccione **Disponibilidad** y, después, **Agregar prueba** . 

1. Escriba el nombre de una prueba y seleccione **Crear** . Así se crea la prueba de ping simple para comprobar la disponibilidad de la aplicación. Después de unos minutos, los resultados de la prueba están disponibles y el panel de Application Insights muestra un estado de disponibilidad.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si va a realizar pruebas, limpie los recursos para que no se acumulen costos de facturación. Cuando dejen de ser necesarios, puede eliminar la máquina virtual de Azure y los recursos relacionados que ha creado en este tutorial. Para ello, use la funcionalidad **Eliminación** del panel de DevOps Starter. 

> [!IMPORTANT]
> El siguiente procedimiento elimina permanentemente los recursos. La funcionalidad *Eliminación* destruye los datos que crea el proyecto en DevOps Starter en Azure y no se podrán recuperar. Utilice este procedimiento cuando haya leído detenidamente las indicaciones.

1. En Azure Portal, vaya al panel de DevOps Starter.
1. En la parte superior derecha, seleccione **Eliminar** . 
1. En el mensaje, seleccione **Sí** para *eliminar permanentemente* los recursos.

Si lo desea, puede modificar el flujo de trabajo para satisfacer las necesidades de su equipo. También puede usar este patrón de CI/CD como plantilla para otros repositorios. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Uso de DevOps Starter para implementar una aplicación de Node.js
> * Configuración de GitHub y una suscripción de Azure 
> * Examen del flujo de trabajo de GitHub
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Configurar la supervisión de Azure Application Insights
> * Limpieza de recursos

Para más información sobre las Acciones de GitHub y los flujos de trabajo, consulte:

> [!div class="nextstepaction"]
> [Personalización del flujo de trabajo de GitHub](https://docs.github.com/actions/configuring-and-managing-workflows/configuring-and-managing-workflow-files-and-runs)
