---
title: Configurar un laboratorio para enseñar ciencia de datos con Python y Jupyter Notebooks | Microsoft Docs
description: Aprenda a configurar un laboratorio para enseñar ciencia de datos mediante Python y Jupyter Notebooks
services: lab-services
documentationcenter: na
author: emaher
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2019
ms.author: enewman
ms.openlocfilehash: 222f5647248d27c3bdfabd0feaeb66dd9f543652
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257732"
---
# <a name="set-up-a-lab-to-teach-data-science-with-python-and-jupyter-notebooks"></a>Configurar un laboratorio para enseñar ciencia de datos con Python y Jupyter Notebooks

En este artículo se describe cómo configurar una máquina de plantilla en Lab Services con las herramientas necesarias para enseñar a los alumnos cómo usar [Jupyter Notebooks](http://jupyter-notebook.readthedocs.io).  Jupyter Notebooks es un proyecto de código abierto que le permite combinar fácilmente texto enriquecido y código fuente [Python](https://www.python.org/) ejecutable en un solo lienzo denominado cuaderno.  Al ejecutar un cuaderno, se genera un registro lineal de entradas y salidas.  Dichas salidas pueden incluir texto, tablas de información, gráficos de dispersión y mucho más.

## <a name="lab-configuration"></a>Configuración del laboratorio

Para configurar este laboratorio, para empezar, necesita una suscripción a Azure y una cuenta de laboratorio. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. Una vez que obtenga una suscripción a Azure, puede crear una nueva cuenta de laboratorio en Azure Lab Services. Para obtener más información sobre la creación de una nueva cuenta de laboratorio, consulte el [tutorial para configurar una cuenta de laboratorio](tutorial-setup-lab-account.md).  También puede usar una cuenta de laboratorio existente.

### <a name="lab-account-settings"></a>Configuración de la cuenta de laboratorio

Habilite la configuración que se describe en la tabla siguiente para la cuenta de laboratorio. Para obtener más información sobre cómo habilitar imágenes de Marketplace, consulte [Especificación de las imágenes de Marketplace disponibles para los creadores de laboratorios](specify-marketplace-images.md).

| Configuración de la cuenta de laboratorio | Instructions |
| ------------------- | ------------ |
| Imagen de Marketplace | Habilite la imagen [Data Science Virtual Machine - Windows 2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-win-2019) para usarla con la cuenta de laboratorio. |

>[!TIP]
>Este artículo se centrará en la configuración de una máquina de plantilla que use el sistema operativo Windows Server.  También es posible configurar una clase de ciencia de datos con Python y Jupyter Notebook mediante imágenes de [Data Science Virtual Machine para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804) de Azure Marketplace.

### <a name="lab-settings"></a>Configuración del laboratorio

Use las opciones de la tabla siguiente al configurar un laboratorio educativo.  Para obtener más información sobre cómo crear un laboratorio de clase, consulte el [tutorial Configuración de un laboratorio educativo](tutorial-setup-classroom-lab.md).

| Configuración del laboratorio | Valor/instrucciones |
| ------------ | ------------------ |
|Tamaño de la máquina virtual| GPU pequeña (proceso) Este tamaño es más adecuado para aplicaciones de proceso intensivo y uso intensivo de la red; por ejemplo, la inteligencia artificial y el aprendizaje profundo. |
|Imagen de máquina virtual| Data Science Virtual Machine: Windows 2016|

## <a name="template-machine"></a>Máquina de plantilla

La imagen [Data Science Virtual Machine - Windows 2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-win-2019) proporciona las herramientas y arquitectura de aprendizaje profundo necesarios para este tipo de clase.  La imagen incluye instancias de Jupyter Notebook y Visual Studio Code.  [Jupyter Notebook](http://jupyter-notebook.readthedocs.io) es una aplicación web que permite a los científicos de datos tomar datos sin procesar, ejecutar cálculos y ver los resultados en el mismo entorno.  Para nuestra máquina de plantilla, la aplicación web se ejecutará localmente.  [Visual Studio Code](https://code.visualstudio.com/) es un IDE que proporciona una experiencia interactiva enriquecida al escribir y probar un cuaderno.  Para obtener más información, consulte [Trabajar con cuadernos de Jupyter Notebook en Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support).

La tarea restante para configurar la clase consiste en proporcionar cuadernos locales.  Para obtener instrucciones sobre cómo usar las muestras de Azure Machine Learning, consulte [cómo configurar un entorno con Jupyter Notebook](../../machine-learning/how-to-configure-environment.md#jupyter).  También puede proporcionar sus propios cuadernos en la máquina de plantilla.  Los cuadernos se copiarán en todas las máquinas de los alumnos cuando se publique la plantilla.

## <a name="cost-estimate"></a>Estimación del costo

Vamos a cubrir una posible estimación de costos para esta clase.  Usaremos una clase de 25 alumnos.  Hay 20 horas de tiempo de clase programado.  Además, cada alumno obtiene una cuota de 10 horas para deberes o tareas fuera del tiempo de clase programado.  El tamaño de la máquina virtual que hemos elegido es el de GPU pequeña (proceso), que tiene 139 unidades de laboratorio.

A continuación se incluye un ejemplo de una posible estimación del costo para esta clase:

25 alumnos \* (20 horas programadas + 10 horas de cuota) \* 139 unidades de laboratorio \* 0,01 USD por hora = 1042,5 USD

Para más detalles sobre los precios, consulte [Precios de Azure Lab Services](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Conclusión

En este artículo, se describen los pasos para crear un laboratorio para una clase de Jupyter Notebook. Puede usar una configuración similar para otras clases de aprendizaje automático.

## <a name="next-steps"></a>Pasos siguientes

Los pasos siguientes son comunes a la configuración de cualquier laboratorio.

- [Creación y administración de plantillas](how-to-create-manage-template.md)
- [Incorporación de usuarios](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Establecimiento de la cuota](how-to-configure-student-usage.md#set-quotas-for-users)
- [Establecimiento de la programación](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Vínculos de registro por correo electrónico para los alumnos](how-to-configure-student-usage.md#send-invitations-to-users)
