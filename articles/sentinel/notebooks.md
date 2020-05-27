---
title: Uso de cuadernos con Azure Sentinel para cuestiones de seguridad
description: En este artículo se describe cómo usar cuadernos con las capacidades de búsqueda de Azure Sentinel.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/25/2019
ms.openlocfilehash: bf63d5c8cb46fd791508af40dcefd7b39d4ba9de
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83652031"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Uso de cuadernos de Jupyter para buscar amenazas de seguridad

La base de Azure Sentinel es el almacén de datos; combina consultas de alto rendimiento y esquemas dinámicos, y tiene capacidad para escalar a volúmenes de datos masivos. Azure Portal y todas las herramientas de Azure Sentinel emplean una API común para acceder a este almacén de datos. Esta misma API está disponible también para herramientas externas, como los cuadernos de [Jupyter](https://jupyter.org/) y Python. Mientras que muchas tareas comunes pueden llevarse a cabo en el portal, Jupyter amplía las posibilidades de lo que se puede hacer con estos datos. Combina una programación completa con una vasta colección de bibliotecas pensadas para el análisis de datos, la visualización y Machine Learning. Estas características hacen que Jupyter sea una herramienta fascinante para la búsqueda e investigación relacionadas con cuestiones de seguridad.

![Cuaderno de ejemplo](./media/notebooks/sentinel-notebooks-map.png)

Hemos integrado la experiencia de Jupyter en Azure Portal, lo que facilita el proceso para crear y ejecutar cuadernos para analizar los datos. La biblioteca *Kqlmagic* actúa como el elemento aglutinador que permite realizar consultas desde Azure Sentinel y ejecutarlas directamente en un cuaderno. En las consultas se usa el [lenguaje de consulta Kusto](https://kusto.azurewebsites.net/docs/query/index.html). Azure Sentinel viene con varios cuadernos, desarrollados por algunos de los analistas de seguridad de Microsoft. Algunos de estos cuadernos se crean para escenarios concretos y se pueden usar tal cual; otros se incluyen como ejemplos para ilustrar las técnicas y características que se pueden copiar o adaptar para usarlas en sus propios cuadernos, y otros se pueden importar desde la comunidad GitHub de Azure Sentinel.

La experiencia de Jupyter integrada usa [Azure Notebooks](https://notebooks.azure.com/) para almacenar, compartir y ejecutar cuadernos. Estos cuadernos también se pueden ejecutar localmente si tiene Jupyter y un entorno de Python en el equipo o en otros entornos de JupyterHub como Azure Databricks.

Los cuadernos constan de dos componentes:

- La interfaz basada en explorador donde se escriben y ejecutan las consultas y el código, y donde se muestran los resultados de la ejecución.
- Un *kernel*, responsable de analizar y ejecutar el código en sí. 

En Azure Notebooks, de forma predeterminada, este kernel se ejecuta en el *almacenamiento y la informática en la nube gratuitos* de Azure. Si los cuadernos incluyen visualizaciones o modelos de Machine Learning complejos, conviene considerar la posibilidad de usar recursos de proceso dedicados más eficaces, como [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM). Los cuadernos de su cuenta son privados, a menos que prefiera compartirlos.

Los cuadernos de Azure Sentinel usan muchas bibliotecas de Python conocidas, como pandas, matplotlib y bokeh, entre otras. Existen otros muchos paquetes de Python que se pueden elegir, que cubren áreas como las siguientes:

- Visualizaciones y gráficos
- Procesamiento de datos y análisis
- Estadísticas y computación numérica
- Aprendizaje automático y aprendizaje profundo

También hemos lanzado una serie de herramientas de seguridad de Jupyter de código abierto en un paquete denominado [msticpy](https://github.com/Microsoft/msticpy/). Este paquete se usa en muchos de los cuadernos incluidos en el producto. Las herramientas de Msticpy están diseñadas expresamente para ayudar a crear cuadernos que se puedan usar en búsquedas e investigaciones, y estamos trabajando activamente en nuevas características y mejoras.

Estos son los cuadernos que se incluyen inicialmente:

- **Investigación guiada - Procesar alertas**: permite evaluar alertas rápidamente analizando la actividad en los hosts afectados.
- **Búsqueda guiada - Explorador de host de Windows**: permite explorar la actividad de la cuenta, las ejecuciones de procesos, la actividad de la red y otros eventos de un host.
- **Búsqueda guiada - Exploración de Office 365**: busca las actividades sospechosas de Office 365 en varios conjuntos de datos de Office 365.

El [repositorio de la comunidad GitHub de Azure Sentinel](https://github.com/Azure/Azure-Sentinel) es la ubicación de los futuros cuadernos de Azure Sentinel creados por Microsoft o procedentes de contribuciones de la comunidad.

Para usar los cuadernos, debe tener una cuenta de Azure Notebooks. Para más información, consulte [Inicio rápido: Inicie sesión y establezca un identificador de usuario](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks) en la documentación de Azure Notebooks. Para crear esta cuenta, puede usar la opción **Sign up for Azure Notebooks** (Registrarse en Azure Notebooks) de la barra de comandos de **Azure Sentinel - Notebooks** (Cuadernos):

> [!div class="mx-imgBorder"]
>![Opción Registrarse en Azure Notebooks](./media/notebooks/sentinel-azure-sign-up-azure-notebooks.png)

Puede ejecutar un cuaderno directamente desde Azure Sentinel o clonar todos los cuadernos de Azure Sentinel en un nuevo proyecto de Azure Notebooks.

## <a name="run-a-notebook-from-azure-sentinel"></a>Ejecución de un cuaderno desde Azure Sentinel
 
1. En Azure Portal, vaya a **Azure Sentinel** > **Threat management (Administración de amenazas)**  > **Notebooks** (Cuadernos), donde puede ver los cuadernos que proporciona Azure Sentinel. 

2. Seleccione cuadernos individuales para leer sus descripciones, los tipos de datos necesarios y los orígenes de datos. Por ejemplo:
    
    > [!div class="mx-imgBorder"]
    > ![iniciar cuaderno](./media/notebooks/sentinel-azure-notebooks-launch.png)

3. Seleccione el cuaderno que quiera usar y, después, seleccione **Iniciar Notebook (versión preliminar)** para clonar y configurar el cuaderno en un nuevo proyecto de Azure Notebooks que se conecta al área de trabajo de Azure Sentinel. Una vez completado el proceso, el cuaderno se abre en Azure Notebooks para que pueda ejecutarlo.

## <a name="clone-azure-sentinel-notebooks-to-a-new-azure-notebooks-project"></a>Clonación de cuadernos de Azure Sentinel en un nuevo proyecto de Azure Notebooks

Este procedimiento crea un proyecto de Azure Notebooks que contiene los cuadernos de Azure Sentinel. Después, puede ejecutar los cuadernos tal cual, o realizar cambios en ellos y luego ejecutarlos.

1. En Azure Portal, vaya a **Azure Sentinel** > **Threat management (Administración de amenazas)**  > **Notebooks** (Cuadernos) y seleccione **Clone Notebooks** (Clonar cuadernos) en la barra de comandos:
  
    > [!div class="mx-imgBorder"]
    >![Opción Clonar cuadernos](./media/notebooks/sentinel-azure-clone-notebooks.png)

2. Cuando aparezca el siguiente cuadro de diálogo, seleccione **Import** (Importar) para clonar el repositorio de GitHub en el proyecto de Azure Notebooks. Si no tiene una cuenta de Azure Notebooks existente, se le pedirá que cree una e inicie sesión en ella.

   ![Importar cuaderno](./media/notebooks/sentinel-notebooks-clone.png)

3. En el cuadro de diálogo **Upload GitHub Repository**, no seleccione **Clone recursively** (Clonar de forma recursiva) porque esta opción hace referencia a repositorios de GitHub vinculados. Para el nombre del proyecto, use el nombre predeterminado o escriba uno nuevo. Después, haga clic en **Import** (Importar) para iniciar la clonación del contenido de GitHub, lo que puede tardar unos minutos en finalizar.

   ![Importar cuaderno](./media/notebooks/sentinel-create-project.png)

4. Abra el proyecto que acaba de crear y abra la carpeta **Notebooks** (Cuadernos) para ver los cuadernos. Por ejemplo:

   ![Importar repositorio](./media/notebooks/sentinel-open-notebook1.png)

A continuación, puede ejecutar los cuadernos desde Azure Notebooks. Para volver a estos cuadernos desde Azure Sentinel, seleccione **Go to your Notebooks** (Ir a sus cuadernos) en la barra de comandos de **Azure Sentinel - Notebooks** (Cuadernos):

> [!div class="mx-imgBorder"]
>![Opción Ir a sus cuadernos](./media/notebooks/sentinel-azure-to-go-notebooks.png)


## <a name="using-notebooks-to-hunt"></a>Uso de cuadernos para la búsqueda

En cada cuaderno se le guiará por el proceso para llevar a cabo una búsqueda o una investigación. Las bibliotecas y otras dependencias necesarias para el cuaderno se pueden instalar desde el propio cuaderno o a través de un sencillo procedimiento de configuración. La configuración que vincula el proyecto del cuaderno con su suscripción de Azure Sentinel se aprovisiona automáticamente en los pasos anteriores.

1. Si ya no está en Azure Notebooks, puede usar la opción **Go to your Notebooks** (Ir a sus cuadernos) de la barra de comandos de **Azure Sentinel - Notebooks** (Cuadernos):
    
    > [!div class="mx-imgBorder"]
    >![Opción Ir a sus cuadernos](./media/notebooks/sentinel-azure-to-go-notebooks.png)
    
    En Azure Notebooks, seleccione **My Projects** (Mis proyectos), después el proyecto que contiene los cuadernos de Azure Sentinel y, por último, la carpeta **Notebooks** (Cuadernos).
    
2. Antes de abrir un cuaderno, tenga en cuenta que, de forma predeterminada, se selecciona Free Compute (Proceso libre) para ejecutar los cuadernos:
    
   ![Seleccionar cuaderno](./media/notebooks/sentinel-open-notebook2.png)
    
    Si ha configurado instancias de Data Science Virtual Machine (DSVM) para usarlas como se explica en la introducción, seleccione la instancia de DSVM y realice la autenticación antes de abrir el primer cuaderno. 

3. Seleccione un cuaderno para abrirlo.
    
    La primera vez que abra un cuaderno, es posible que se le pida que seleccione una versión del kernel. Si no se le solicita, puede seleccionar la versión de kernel en **Kernel** >  **Change kernel** (Cambiar kernel) y luego seleccionar una versión no inferior a la 3.6. La versión de kernel seleccionada se muestra en la parte superior derecha de la ventana del cuaderno:
    
   ![Seleccionar cuaderno](./media/notebooks/sentinel-select-kernel.png)

4. Antes de realizar cambios en el cuaderno que ha descargado, es aconsejable realizar una copia del cuaderno original y trabajar en la copia. Para ello, seleccione **Archivo** > **Crear una copia**. Trabajar en la copia permite actualizar sin riesgos a versiones futuras de los cuadernos, sin sobrescribir ningún dato.
    
    Ahora está listo para ejecutar o editar el cuaderno seleccionado.

Recomendaciones:

- Para una introducción rápida sobre cómo consultar datos en Azure Sentinel, eche un vistazo al cuaderno [GetStarted](https://github.com/Azure/Azure-Sentinel-Notebooks/blob/345cf9f7c8f6137f5af4593a3f9d7568acd6cbc2/DeprecatedNotebooks/Get%20Started.ipynb) en la carpeta principal **Notebooks** (Cuadernos). 

- Encontrará más cuadernos de ejemplo en la subcarpeta **Sample-Notebooks** (Cuadernos de ejemplo). Los cuadernos de ejemplo se han guardado con datos, así será más fácil ver los resultados previstos. Se recomienda ver estos cuadernos en [nbviewer](https://nbviewer.jupyter.org/). 

- La carpeta **HowTos** (Procedimientos) contiene cuadernos que describen tareas como, entre otras muchas, configurar la versión de Python predeterminada, configurar una instancia de DSVM o crear marcadores de Azure Sentinel desde un cuaderno.

Estos cuadernos proporcionados están diseñados como herramientas útiles, pero también como ilustraciones y códigos de ejemplo que los usuarios pueden utilizar para desarrollar sus propios cuadernos.

Agradecemos cualquier comentario, sean sugerencias, peticiones de características, contribuciones de cuadernos, informes de errores o mejoras y adiciones a cuadernos ya existentes. Vaya a la [comunidad GitHub de Azure Sentinel](https://github.com/Azure/Azure-Sentinel) para abrir una incidencia, crear una bifurcación y cargar una contribución.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a usar cuadernos de Jupyter Notebooks en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- [Búsqueda de amenazas con Azure Sentinel, versión preliminar](hunting.md)
- [Realizar un seguimiento de los datos durante una búsqueda](bookmarks.md)
