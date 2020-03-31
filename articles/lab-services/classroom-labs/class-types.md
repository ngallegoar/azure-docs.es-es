---
title: Tipos de clase de ejemplo en Azure Lab Services | Microsoft Docs
description: Proporciona algunos tipos de clases para los que puede configurar laboratorios mediante Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 27619a69a1f7fbded8ce6430afc2b8e9a8b4a00c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79296734"
---
# <a name="class-types-overview---azure-lab-services"></a>Información general sobre tipos de clase: Azure Lab Services

Azure Lab Services le permite configurar rápidamente un entorno de laboratorio educativo en la nube. En los artículos de esta sección se proporcionan instrucciones sobre cómo configurar varios tipos de laboratorios de clase mediante Azure Lab Services.

## <a name="deep-learning-in-natural-language-processing"></a>Aprendizaje profundo en el procesamiento del lenguaje natural

Puede configurar un laboratorio centrado en el aprendizaje profundo en el procesamiento de lenguaje natural (NLP) mediante Azure Lab Services. El procesamiento de lenguaje natural (NLP) es una forma de inteligencia artificial (AI) que permite a los equipos utilizar la traducción, el reconocimiento de voz y otras capacidades de comprensión de lenguajes. Los alumnos que sigan una clase de NLP obtienen una máquina virtual Linux para aprender a aplicar algoritmos de red neuronal para desarrollar modelos de aprendizaje profundo que se utilizan para analizar el lenguaje humano escrito.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio centrado en el aprendizaje profundo en el procesamiento de lenguaje natural mediante Azure Lab Services](class-type-deep-learning-natural-processing.md).

## <a name="shell-scripting-on-linux"></a>Generación de scripts en shell en Linux

Puede configurar un laboratorio para enseñar el scripting de shell en Linux. El scripting es una parte útil de la administración del sistema que permite a los administradores evitar tareas repetitivas. En este escenario de ejemplo, la clase cubre scripts bash tradicionales y scripts mejorados. Los scripts mejorados son scripts que combinan comandos bash y Ruby. Este enfoque permite a Ruby pasar datos y comandos bash para interactuar con el shell.

Los alumnos que sigan estas clases de scripting obtienen una máquina virtual Linux para conocer los aspectos básicos de Linux y también familiarizarse con el scripting de shell de bash. La máquina virtual Linux incluye el acceso al escritorio remoto habilitado y con los editores de texto [gedit](https://help.gnome.org/users/gedit/stable/) y [Visual Studio Code](https://code.visualstudio.com/) instalados.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Scripting de shell en Linux](class-type-shell-scripting-linux.md).

## <a name="ethical-hacking"></a>Piratería ética

Puede configurar un laboratorio para una clase que se centre en la parte forense de la piratería ética. Las pruebas de penetración son una práctica que usa la comunidad de piratería ética y que se producen cuando alguien intenta obtener acceso al sistema o a la red para mostrar los puntos vulnerables que un atacante malintencionado podría aprovechar.

En una clase de piratería ética, los alumnos pueden aprender técnicas modernas para protegerse frente a los puntos vulnerables. Cada alumno obtiene una máquina virtual host de Windows Server que tiene dos máquinas virtuales anidadas: una máquina virtual con una imagen de [Metasploitable3](https://github.com/rapid7/metasploitable3) y otra con una imagen de [Kali Linux](https://www.kali.org/). La máquina virtual Metasploitable se usa para la explotación.  La máquina virtual Kali de Linux proporciona acceso a las herramientas necesarias para ejecutar tareas forenses.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio para impartir una clase de piratería ética](class-type-ethical-hacking.md).

## <a name="database-management"></a>Administración de bases de datos
Los conceptos de las bases de datos son uno de los cursos introductorios en la mayoría de los departamentos de informática de las universidades. Puede configurar un laboratorio para una clase de administración de bases de datos básica en Azure Lab Services. Por ejemplo, puede configurar una plantilla de máquina virtual en un laboratorio con un servidor de base de datos [MySQL](https://www.mysql.com/) o un servidor de [SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019).

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio para enseñar administración de bases de datos relacionales](class-type-database-management.md).

## <a name="python-and-jupyter-notebooks"></a>Python y cuadernos de Jupyter Notebook
Puede configurar una máquina de plantilla en Azure Lab Services con las herramientas necesarias para enseñar a los alumnos cómo usar [cuadernos de Jupyter Notebook](http://jupyter-notebook.readthedocs.io). Jupyter Notebooks es un proyecto de código abierto que le permite combinar fácilmente texto enriquecido y código fuente [Python](https://www.python.org/) ejecutable en un solo lienzo denominado cuaderno. Al ejecutar un cuaderno, se genera un registro lineal de entradas y salidas.  Dichas salidas pueden incluir texto, tablas de información, gráficos de dispersión y mucho más.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio para enseñar ciencia de datos con Python y cuadernos de Jupyter Notebook](class-type-jupyter-notebook.md).

## <a name="mobile-app-development-with-android-studio"></a>Desarrollo de aplicaciones móviles con Android Studio
Puede configurar un laboratorio en Azure Lab Services para enseñar una clase introductoria de desarrollo de aplicaciones móviles. Esta clase se centra en las aplicaciones móviles de Android que se pueden publicar en [Google Play Store](https://play.google.com/store/apps).  Los alumnos aprenderán a usar [Android Studio](https://developer.android.com/studio) para compilar aplicaciones.  El [Emulador de Visual Studio para Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) se usa para probar la aplicación localmente.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio para enseñar el desarrollo de aplicaciones móviles con Android Studio](class-type-mobile-dev-android-studio.md).


## <a name="next-steps"></a>Pasos siguientes

Vea los artículos siguientes:

- [Configuración de un laboratorio centrado en el aprendizaje profundo en el procesamiento de lenguaje natural mediante Azure Lab Services](class-type-deep-learning-natural-processing.md)
- [Generación de scripts de shell en Linux](class-type-shell-scripting-linux.md)
- [Piratería ética](class-type-ethical-hacking.md)