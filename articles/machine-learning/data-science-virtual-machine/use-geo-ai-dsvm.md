---
title: Uso de la inteligencia artificial geoespacial
titleSuffix: Azure Data Science Virtual Machine
description: Aprenda a usar la instancia de Geo AI Data Science Virtual Machine para analizar datos y crear modelos basados en datos geoespaciales.
keywords: aprendizaje profundo, IA, herramientas de ciencia de datos, máquina virtual de ciencia de datos, análisis geoespacial
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 2d7532c51e2963c0dc9f8d02e7a0e32864e80f92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70278412"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Uso de la máquina virtual Geo Artificial Intelligence Data Science Virtual Machine

Utilice la máquina virtual de ciencia de datos para inteligencia artificial y análisis geoespacial para capturar datos para análisis, realizar doma de datos (wrangling) y generar modelos para aplicaciones de inteligencia artificial que consumen información geoespacial. Después de aprovisionar Geo AI Data Science VM e iniciar sesión en ArcGIS Pro con su cuenta de ArcGIS puede empezar a interactuar tanto con el escritorio de ArcGIS como con ArcGIS en línea. Para acceder a ArcGIS se pueden usar las interfaces de Python y un puente del lenguaje R que se configura previamente en Geo-Data Science VM. Para crear unas aplicaciones de inteligencia artificial completas, combine Geo-Data Science VM con los marcos de aprendizaje automático y aprendizaje profundo, y con cualquier otro software de ciencia de datos disponible.  


## <a name="configuration-details"></a>Detalles de configuración

La biblioteca de Python, [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), que se utiliza para hacer de interfaz con ArcGIS, está instalada en el entorno de conda raíz global de Data Science VM que se encuentra en ```c:\anaconda```.

- Si ejecuta Python en un símbolo del sistema, ejecute ```activate``` para realizar la activación en el entorno de Python raíz de conda.
- Si utiliza un IDE o un cuaderno de Jupyter Notebook, puede seleccionar el entorno o el kernel para asegurarse de que se encuentra en el entorno de conda correcto.

El puente de R para ArcGIS se instala como una biblioteca de R denominada [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) en la instancia independiente principal de Microsoft Machine Learning Server que se encuentra en ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio y Jupyter ya están preconfiguradas para usar este entorno de R y tendrán acceso a la biblioteca de R ```arcgisbinding```. 


## <a name="geo-ai-data-science-vm-samples"></a>Ejemplos de máquina virtual de ciencia de datos para inteligencia artificial y análisis geoespacial

Además de ejemplos basados en el marco de aprendizaje automático y aprendizaje profundo de Data Science VM, también se proporciona un conjunto de ejemplos de análisis geoespacial como parte de Geo AI Data Science VM. Estos ejemplos pueden ayudarle a impulsar el desarrollo de aplicaciones de inteligencia artificial mediante datos geoespaciales y el software de ArcGIS:


1. [Introducción al análisis geoespacial con Python](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): un ejemplo introductorio que muestra cómo trabajar con datos geoespaciales mediante la interfaz de Python para ArcGIS proporcionada por la biblioteca [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm). También muestra cómo combinar el aprendizaje automático tradicional con datos geoespaciales y, después, visualizar el resultado en un mapa en ArcGIS.

2. [Introducción al análisis geoespacial con R](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): un ejemplo introductorio muestra cómo trabajar con datos geoespaciales mediante la interfaz de R para ArcGIS que proporciona la biblioteca [arcgisbinding](https://github.com/R-ArcGIS/r-bridge). 

3. [Clasificación de uso del suelo a nivel de píxeles](https://github.com/Azure/pixel_level_land_classification): un tutorial que muestra cómo crear un modelo de red neuronal profunda que acepta una imagen aérea como entrada y devuelve una etiqueta de cobertura del suelo. Ejemplos de etiquetas de la cubierta terrestre son *bosque* y *agua*. El modelo devuelve este tipo de etiqueta para cada píxel de la imagen. 


## <a name="next-steps"></a>Pasos siguientes

Hay ejemplos adicionales que usan la máquina virtual de ciencia de datos disponibles aquí:

* [Muestras](dsvm-samples-and-walkthroughs.md)