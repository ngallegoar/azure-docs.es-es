---
title: 'Inicio rápido: Creación de una instancia de Data Science Virtual Machine para Geo AI'
titleSuffix: Azure Data Science Virtual Machine
description: Configure y cree una instancia de Data Science Virtual Machine para Geo AI en Azure para realizar análisis geoespacial y aprendizaje automático.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: f3ff9bd64f54d8f83fd1889078e8a4c01827d135
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77525896"
---
# <a name="quickstart-set-up-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Inicio rápido: Configuración de una instancia de Geo Artificial Intelligence Virtual Machine en Azure 

Geo AI Data Science Virtual Machine (Geo-DSVM) es una extensión de la conocida máquina virtual [Azure Data Science Virtual Machine](https://aka.ms/dsvm) que se ha configurado especialmente para combinar la inteligencia artificial con el análisis geoespacial. Los análisis geoespaciales en la máquina virtual se realizan con tecnología de [ArcGIS Pro](https://www.arcgis.com/features/index.html). Data Science Virtual Machine (DSVM) permite el entrenamiento rápido de modelos de aprendizaje automático e incluso de aprendizaje profundo. Para desarrollar estos modelos, se usan datos que se enriquecen con información geográfica. Geo-DSVM solo se admite en DSVM de Windows 2016. 

Las herramientas de inteligencia artificial que se incluyen en Geo-DSVM son las siguientes:

- Ediciones de GPU de plataformas de aprendizaje profundo conocidas, como Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2 y Chainer.
- Herramientas para adquirir y preprocesar imágenes y datos de texto.
- Herramientas para actividades de desarrollo, como Microsoft Machine Learning Server Developer Edition, Anaconda Python, cuadernos de Jupyter para Python y R, IDE para Python y R, y bases de datos SQL.
- Software de escritorio ArcGIS Pro de ESRI junto con interfaces de Python y R que pueden trabajar con los datos geoespaciales de sus aplicaciones de inteligencia artificial.
 

## <a name="create-your-geo-ai-data-science-vm"></a>Creación de una máquina virtual de ciencia de datos para inteligencia artificial y análisis geoespacial

Para crear una instancia de Geo AI Data Science VM, siga estos pasos:

1. Vaya al listado de máquinas virtuales en [Azure Portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows).
1. Seleccione **Crear** en la parte inferior para generar un asistente:

   ![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)

1. El asistente requiere una entrada para cada uno de los cuatro pasos. Para más información sobre esta entrada, consulte la sección siguiente.

### <a name="wizard-details"></a>Detalles del asistente ###

**Aspectos básicos**:

- **Name**: El nombre del servidor de ciencia de datos que está creando.
    
- **Nombre de usuario**: identificador de inicio de sesión de la cuenta de administración.
    
- **Contraseña**: contraseña de la cuenta de administrador.
    
- **Suscripción**: Si tiene más de una suscripción, seleccione aquella en la que se creará y facturará la máquina.
    
- **Grupo de recursos**: puede crear uno nuevo o usar un grupo de recursos **vacío** existente de Azure en su suscripción.
    
- **Ubicación**: seleccione el centro de datos más adecuado. Normalmente, es el que tiene la mayor parte de los datos o el que esté más cerca de su ubicación física para disfrutar de un acceso más rápido a la red. Si planea ejecutar aprendizaje profundo en una GPU, debe elegir una de las ubicaciones de Azure que tenga instancias de máquina virtual de GPU de la serie NC. Actualmente esas ubicaciones son: **Este de EE. UU., Centro y norte de EE. UU., Centro y sur de EE. UU., Oeste de EE. UU. 2, Norte de Europa, Oeste de Europa**. Para obtener la lista más reciente, compruebe la página [Productos de Azure por región](https://azure.microsoft.com/regions/services/) y busque **Serie NC** en **Proceso**. 
    
    
**Configuración**: si tiene previsto ejecutar aprendizaje profundo en una GPU en su instancia de Geo-DSVM, seleccione un tamaño de máquina virtual de GPU de la serie NC. En caso contrario, puede elegir una de las instancias basadas en CPU. Cree una cuenta de almacenamiento para su máquina virtual. 
       
**Resumen**: Compruebe que toda la información que ha especificado es correcta.
    
**Comprar**: Para iniciar el proceso de aprovisionamiento, haga clic en **Comprar**. Se proporciona un vínculo a los términos del servicio. No se generan cargos adicionales por el uso de la máquina virtual a excepción de los relativos al proceso por el tamaño de servidor elegido en el paso **Tamaño**. 
 
 >[!NOTE]
 > El aprovisionamiento tardará entre 20 y 30 minutos. El estado del aprovisionamiento se muestra en el Portal de Azure.

 
## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Acceso a la máquina virtual Geo Artificial Intelligence Data Science Virtual Machine

 Después de crear la máquina virtual, está listo para empezar a usar las herramientas que tiene instaladas y preconfiguradas. En el menú Inicio y en el escritorio hay iconos de muchas de las herramientas. Puede acceder a la máquina virtual mediante Escritorio remoto con las credenciales de la cuenta del administrador que configuró en la sección **Aspectos básicos**.

 
## <a name="using-arcgis-pro-installed-in-the-vm"></a>Uso de ArcGIS Pro instalado en la máquina virtual

En Geo-DSVM, el software de escritorio ArcGIS Pro ya está instalado y el entorno está preconfigurado para funcionar con todas las herramientas de DSVM. Al iniciar ArcGIS, se le pedirán las credenciales de su cuenta de ArcGIS. Si ya tiene una cuenta de ArcGIS y tiene licencias para el software, puede usar sus credenciales existentes.  

![Inicio de sesión-Arc-GIS](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

En caso contrario, puede registrarse para obtener una nueva cuenta y licencia de ArcGIS o para una [evaluación gratuita](https://www.arcgis.com/features/free-trial.html). 

![Evaluación-gratuita-ArcGIS](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Después de registrarse para obtener una cuenta estándar o de evaluación gratuita ArcGIS, puede conceder autorización a ArcGIS Pro sobre su cuenta mediante las instrucciones de [Getting Started with ArcGIS Pro](https://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf) (Introducción a ArcGIS Pro).

Después de iniciar sesión en el escritorio ArcGIS Pro con su cuenta de ArcGIS, está listo para empezar a usar las herramientas de ciencia de datos instaladas y configuradas en la máquina virtual para sus proyectos de análisis geoespacial y aprendizaje automático.

## <a name="next-steps"></a>Pasos siguientes

Empiece a usar Geo AI Data Science VM con ayuda del siguiente recurso:

* [Use the Geo AI Data Science VM](use-geo-ai-dsvm.md) (Uso de la máquina virtual de ciencia de datos para inteligencia artificial y análisis geoespacial)
