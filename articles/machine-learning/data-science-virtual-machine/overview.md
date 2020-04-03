---
title: Qué es Azure Data Science Virtual Machine
titleSuffix: Azure Data Science Virtual Machine
description: Componentes y escenarios de análisis fundamentales para instancias de Data Science Virtual Machine en Windows y Linux.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 12/31/2019
ms.openlocfilehash: a5fbcc1eef8717fdb1aa7f914c3e0ba6594fc27a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281806"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>¿Qué es Azure Data Science Virtual Machine de Azure para Linux y Windows?

Data Science Virtual Machine (DSVM) es una imagen de máquina virtual personalizada en la plataforma en la nube de Azure diseñada específicamente para realizar ciencia de datos. Tiene muchas herramientas de ciencias de datos populares preinstaladas y preconfiguradas para impulsar la creación de aplicaciones inteligentes para análisis avanzado. 

El DSVM está disponible en:
+ **Windows Server 2019**
+ **Ubuntu 18.04 LTS**
+ Windows Server 2016
+ Ubuntu 16.04 LTS y CentOS 7.4


> [!NOTE]
> Todas las herramientas de máquina virtual para el aprendizaje profundo se han incluido en Data Science Virtual Machine. 


## <a name="why-choose-the-dsvm"></a>¿Por qué elegir DSVM?
El objetivo de Data Science Virtual Machine es proporcionar a los profesionales de datos de todos los niveles de aptitud y de distintos sectores, un entorno de ciencia de datos preconfigurado y libre de problemas. En lugar de implementar un área de trabajo comparable por su cuenta, puede aprovisionar un DSVM. Esta opción puede ahorrarle días, o incluso _semanas_, en los procesos de instalación, configuración y administración de paquetes. Después de que se ha asignado la DSVM, puede comenzar a trabajar inmediatamente en su proyecto de ciencia de datos.

## <a name="sample-use-cases"></a>Casos de uso de ejemplo

A continuación, se muestran algunos casos de uso comunes para los clientes de DSVM.

### <a name="moving-data-science-workloads-to-the-cloud"></a>Traslado de cargas de trabajo de ciencia de datos a la nube

DSVM proporciona una configuración de línea de base para los equipos de ciencia de datos que quieren reemplazar sus escritorios locales con un escritorio en la nube administrado, asegurándose de que todos los científicos de datos de un equipo tengan una configuración coherente con la que comprobar los experimentos y promover la colaboración. Al reducirse la carga del administrador del sistema, también se reducen los costos. Esta reducción de la carga ahorra el tiempo necesario para evaluar, instalar y mantener los paquetes de software para realizar análisis avanzados.

### <a name="data-science-training-and-education"></a>Educación y formación de ciencia de datos
Los instructores y educadores que imparten clases de ciencia de datos proporcionan normalmente una imagen de máquina virtual. La imagen garantiza que los alumnos tienen una configuración coherente y que las muestras funcionan según lo previsto. 

Data Science Virtual Machine crea un entorno a petición con una configuración coherente que simplifica los desafíos de incompatibilidad y soporte técnico. En los casos en donde estos entornos tienen que crearse con frecuencia, especialmente para las clases de aprendizaje más cortas, estos entornos se benefician sustancialmente.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Capacidad elástica a petición para proyectos de gran escala
Tanto los hackathons/competiciones como la exploración y el modelado de datos a gran escala requieren capacidad de hardware para la escalabilidad horizontal, normalmente durante poco tiempo. Data Science Virtual Machine puede ayudar a replicar el entorno de la ciencia de datos rápidamente a petición, en servidores con escalabilidad horizontal que permiten experimentos que los recursos informáticos de alta potencia pueden ejecutar.

### <a name="custom-compute-power-for-azure-notebooks"></a>Capacidad de proceso personalizada para Azure Notebooks
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) es un servicio hospedado gratuito para desarrollar, ejecutar y compartir cuadernos de Jupyter Notebook en la nube sin necesidad de instalación. El nivel de servicio gratuito está limitado a 4 GB de memoria y 1 GB de datos. 

Para liberar todos los límites, puede asociar un proyecto de Notebooks a una instancia de Data Science Virtual Machine o a cualquier otra máquina virtual que se ejecute en un servidor de Jupyter. Si inicia sesión en Azure Notebooks con una cuenta mediante Azure Active Directory (por ejemplo, una cuenta corporativa), Notebooks muestra automáticamente las instancias de Data Science Virtual Machine en las suscripciones asociadas a esa cuenta. Puede [asociar una instancia de Data Science Virtual Machine a Azure Notebooks](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) para ampliar la potencia de proceso disponible.

### <a name="short-term-experimentation-and-evaluation"></a>Evaluación y experimentación a corto plazo
Puede usar DSVM para evaluar o aprender sobre las nuevas [herramientas](./tools-included.md) de ciencia de datos, especialmente en algunos de nuestros [ejemplos y tutoriales](./dsvm-samples-and-walkthroughs.md) publicados.


### <a name="deep-learning-with-gpus"></a>Aprendizaje profundo con GPU
En Data Science Virtual Machine, los modelos de entrenamiento pueden usar algoritmos de aprendizaje profundo en hardware basado en unidades de procesamiento de gráficos (GPU). Aprovechando las funcionalidades de escalado de máquinas virtuales de la plataforma Azure, DSVM le ayuda a usar hardware basado en GPU en la nube según sus necesidades. Puede cambiar a una máquina virtual basada en GPU cuando vaya a entrenar modelos grandes o cuando necesite cálculos a alta velocidad y desee mantener el mismo disco del SO. Puede elegir cualquiera de las SKU de máquina virtual habilitada para GPU de la serie N con DSVM. Tenga en cuenta que las cuentas gratuitas de Azure no admiten los SKU de máquinas virtuales habilitadas para GPU.

La edición de Windows Server 2016 de la DSVM viene preinstalada con controladores de GPU, marcos y las versiones de GPU de los marcos de aprendizaje profundo. En la edición Linux, el aprendizaje profundo en las GPU está habilitado en las DSVM de CentOS y Ubuntu. 

También puede implementar la edición de Ubuntu, CentOS o Windows 2016 de Data Science VM en una máquina virtual de Azure no basada en GPU. En este caso, todos los marcos de aprendizaje profundo se revertirán al modo CPU.
 
[Más información sobre los marcos de aprendizaje profundo y de inteligencia artificial](dsvm-tools-deep-learning-frameworks.md).

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>¿Qué incluye DSVM?

Consulte [aquí](tools-included.md) una lista completa de herramientas en DSVM de Windows y Linux.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información en estos artículos:

+ Windows:
  + [Configuración de una instancia de DSVM de Windows](provision-vm.md)
  + [Diez cosas que puede hacer en Windows Data Science Virtual Machine](vm-do-ten-things.md)

+ Linux:
  + [Configuración de una instancia de DSVM de Linux (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Configuración de una instancia de DSVM de Linux (CentOS)](linux-dsvm-intro.md)
  + [Ciencia de datos en una DSVM de Linux](linux-dsvm-walkthrough.md)
