---
title: Introducción a Azure Notebooks (versión preliminar)
description: Ejecute cuadernos de Jupyter Notebook en la nube mediante el servicio gratuito Azure Notebooks (versión preliminar), donde no se requiere realizar ninguna configuración.
ms.topic: overview
ms.date: 04/05/2019
ms.openlocfilehash: d59faaf85a1fdbad776e954e974bafa300f93d85
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94845142"
---
# <a name="overview-of-azure-notebooks-preview"></a>Introducción a Azure Notebooks (versión preliminar)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure Notebooks es un servicio hospedado gratuito para desarrollar y ejecutar cuadernos de Jupyter Notebook en la nube sin necesidad de instalación. [Jupyter](https://jupyter.org/) (anteriormente IPython) es un proyecto de código abierto que permite combinar fácilmente texto Markdown, código ejecutable y datos, gráficos y visualizaciones persistentes en un único lienzo que se puede compartir, denominado el *cuaderno* (imagen cortesía de jupyter.org):

[![Ejemplos de Jupyter Notebooks](https://jupyter.org/assets/jupyterpreview.png)](https://jupyter.org/assets/jupyterpreview.png#lightbox)

Debido a esta potente combinación de código, gráficos y texto explicativo, el uso de Jupyter se ha extendido en escenarios como la instrucción de ciencia de datos, la limpieza y transformación de datos, la simulación numérica, el modelado estadístico y el desarrollo de modelos de aprendizaje automático.

## <a name="hassle-free-experience"></a>Experiencia sin complicaciones

Azure Notebooks le ayuda a comenzar rápidamente a crear prototipos, trabajar en la ciencia de datos, realizar investigación académica o aprender a programar Python:

- Un científico de datos tiene acceso instantáneo a un entorno completo Anaconda sin necesidad de instalación.
- Un profesor puede proporcionar un entorno Python sin complicaciones a los alumnos.
- Un moderador puede dar una conferencia o un seminario web sin pedir a los asistentes que dediquen 45 minutos a instalar software.
- Un desarrollador o un aficionado pueden usar cuadernos como blocs de dictado de código rápido.

Los cuadernos resultan aún más eficaces cuando las personas pueden colaborar en ellos mediante un servicio en la nube accesible por medio de un explorador, como Azure Notebooks (en versión preliminar). En la nube, los usuarios no necesitan instalar Jupyter localmente ni preocuparse de mantener un entorno. La nube también permite compartir cuadernos fácilmente (y los archivos de datos asociados) con otros usuarios autorizados, de forma que se evitan las complicaciones de compartir cuadernos mediante medios externos, como repositorios de control de código fuente. Con Azure Notebooks, los usuarios también pueden copiar (o "clonar") cuadernos en su propia cuenta para experimentar con ellos o modificarlos, lo que es especialmente útil con fines de instrucción.

Dado que Azure Notebooks es una plataforma general de creación, ejecución y uso compartido de código, puede usarla en muchos escenarios diversos:

- Aprender un nuevo lenguaje de programación: pruebe uno de los [tutoriales de FrontPage](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb)
- Aprender sobre la ciencia de datos: pruebe el [libro de Jake VanderPlas](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook).
- [Impartir un curso](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas) para cientos de estudiantes
- Dar un seminario web en línea o una conferencia sin perder tiempo en la instalación 
- Permitir que los usuarios de GitHub carguen y ejecuten directamente cuadernos mediante la [creación de una notificación de lanzamiento de GitHub](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge)
- Realizar [presentaciones de PowerPoint](https://notebooks.azure.com/help/jupyter-notebooks/slides) en las que el código de las diapositivas es ejecutable

En resumen, Azure Notebooks le ayuda a realizar su trabajo de forma más eficaz y, por tanto, a lograr más resultados.

> [!Note]
> Se puede encontrar más información sobre Jupyter propiamente dicho en [jupyter.org](https://jupyter.org/) y en la [documentación de Jupyter](https://jupyter-notebook.readthedocs.io/en/latest/).

## <a name="pricing-and-quotas"></a>Precios y cuotas

Azure Notebooks es un servicio gratuito, pero cada proyecto está limitado a 4 GB de memoria y 1 GB de datos para evitar el uso abusivo. Los usuarios legítimos que superan estos límites ven un desafío Captcha para continuar ejecutando cuadernos.

Para liberar todos los límites, inicie sesión en Azure Notebooks con una cuenta con Azure Active Directory (por ejemplo, una cuenta corporativa). Si esa cuenta está asociada a una suscripción de Azure, puede conectarse a cualquier instancia de Azure Data Science Virtual Machine de esa suscripción. Para más información, consulte [Administración y configuración de proyectos: nivel de proceso](configure-manage-azure-notebooks-projects.md#compute-tier).

Se garantiza que los servidores de cuadernos existen durante un máximo de 8 horas. En la mayoría de los casos, el contenedor no está sujeto a esta limitación y su ejecución no se detiene después de dicho intervalo, pero de vez en cuando es posible que las sesiones de larga duración se cierren para preservar la estabilidad del sistema.

## <a name="available-kernels-and-environments"></a>Entornos y kernels disponibles

Para cada cuaderno, seleccione el kernel (es decir, el entorno en tiempo de ejecución) que se usa para ejecutar todas las celdas de código. Azure Notebooks es compatible con los siguientes kernels:

- Python 2.7 + Anaconda2-5.3.0
- Python 3.6 + Anaconda3-5.3.0
- Python 3.5 + Anaconda3-4.2.0 (va a estar en desuso)
- R 3.4.1 + Microsoft R Open 3.4.1
- F# 4.1.9

Azure Notebooks también incluye paquetes adicionales a las distribuciones base. Los kernels de Python, por ejemplo, incluyen las bibliotecas numpy, pandas, scikit-learn, matplotlib y bokeh.

También puede personalizar un proyecto para crear un entorno para todos los cuadernos de ese proyecto. Para más información, consulte [Inicio rápido: Creación de un proyecto con un entorno personalizado](quickstart-create-jupyter-notebook-project-environment.md).

Además de las distribuciones base, Azure Notebooks viene preinstalado con muchos paquetes adicionales que son útiles para los científicos de datos. También puede instalar sus propios paquetes mediante el proceso típico para cada lenguaje.

## <a name="pre-configured-jupyter-extensions"></a>Extensiones de Jupyter preconfiguradas

Azure Notebooks está preconfigurado con las siguientes extensiones de Jupyter:

- [RISE](https://github.com/damianavila/RISE): una extensión de presentación de Jupyter (también conocida como live_reveal). Para más información, consulte [Run a notebook slideshow](present-jupyter-notebooks-slideshow.md) (Ejecución de una presentación de cuaderno).
- [JupyterLab](https://github.com/jupyterlab/jupyterlab): un completo entorno de cálculo para trabajar con cuadernos de Jupyter Notebook.
- [Altair](https://github.com/ellisonbg/altair): una biblioteca de visualización estadísticas declarativa para Python.
- [BQPlot](https://github.com/bloomberg/bqplot): una plataforma de trazado interactiva para cuadernos de Jupyter Notebook.
- [IpyWidgets](https://github.com/jupyter-widgets/ipywidgets): widgets HTML interactivo para cuadernos de Jupyter Notebook.

## <a name="issues-and-getting-help"></a>Problemas y ayuda

Dado que Azure Notebooks está aún en versión preliminar, el servicio puede experimentar interrupciones temporales que pueden ser más frecuentes o duraderas que en el caso de otros servicios de Azure. Algunas características pueden estar incompletas o contener errores.

En este momento, se recomienda no usar la versión preliminar de Azure Notebooks con aplicaciones críticas para la empresa o cuadernos y datos confidenciales.

Para plantear sus preguntas sobre Azure Notebooks, registre un problema en el [repositorio de GitHub](https://github.com/Microsoft/AzureNotebooks/issues).

## <a name="next-steps"></a>Pasos siguientes  

- [Exploración de cuadernos de ejemplo](azure-notebooks-samples.md)

- Guías de inicio rápido:

  - [Creación y uso compartido de un cuaderno](quickstart-create-share-jupyter-notebook.md)
  - [Clonación de un cuaderno](quickstart-clone-jupyter-notebook.md)
  - [Migración de un cuaderno de Jupyter Notebook local](quickstart-migrate-local-jupyter-notebook.md)
  - [Uso de un entorno personalizado](quickstart-create-jupyter-notebook-project-environment.md)
  - [Inicio de sesión y establecimiento de un identificador de usuario](quickstart-sign-in-azure-notebooks.md)

- Tutoriales:

  - [Creación y ejecución de un cuaderno](tutorial-create-run-jupyter-notebook.md  )

- Artículos de procedimientos:
  
  - [Creación y clonación de proyectos](create-clone-jupyter-notebooks.md)
  - [Configuración y administración de proyectos](configure-manage-azure-notebooks-projects.md)
  - [Instalación de paquetes en un cuaderno](install-packages-jupyter-notebook.md)
  - [Presentación](present-jupyter-notebooks-slideshow.md)
  - [Uso de archivos de datos](work-with-project-data-files.md)
  - [Acceso a recursos de datos](access-data-resources-jupyter-notebooks.md)
  - [Uso de Azure Machine Learning](../machine-learning/samples-notebooks.md)