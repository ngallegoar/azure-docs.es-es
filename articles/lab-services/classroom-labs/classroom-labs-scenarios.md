---
title: 'Uso de laboratorios educativos con fines de aprendizaje: Azure Lab Services'
description: En este artículo se describe cómo usar Azure DevTest Labs para crear laboratorios en Azure para escenarios de aprendizaje.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 155806222f9e11fec177487b7147d81054ac06ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76717985"
---
# <a name="use-classroom-labs-for-trainings"></a>Uso de laboratorios educativos con fines de aprendizaje
Puede configurar un laboratorio con fines de aprendizaje. Los laboratorios educativos de Azure Lab Services permiten crear un laboratorio para el aprendizaje donde cada aprendiz usa entornos idénticos y aislados para tal fin. Puede aplicar directivas para garantizar que los entornos de entrenamiento estarán disponibles para cada aprendiz solo cuando sea necesario y cuando contengan los suficientes recursos necesarios como, por ejemplo, máquinas virtuales, para el entrenamiento. 

![Laboratorio educativo](../media/classroom-labs-scenarios/classroom.png)

Los laboratorios educativos cumplen los siguientes requisitos necesarios para llevar a cabo el aprendizaje en cualquier entorno virtual: 

- Los aprendices pueden aprovisionar rápidamente sus entornos de entrenamiento
- Cada máquina de entrenamiento debe ser idéntica
- Los aprendices no pueden ver las máquinas virtuales creadas por otros aprendices
- Se pueden controlar los costos garantizando que los aprendices no obtengan más máquinas virtuales de las necesarias para el entrenamiento y también apagando estas cuando no estén en uso
- Se puede compartir fácilmente el laboratorio de entrenamiento con cada aprendiz
- El laboratorio de entrenamiento se puede volver a usar una y otra vez

En este artículo, aprenderá acerca de las diversas características de Azure Lab Services que se pueden utilizar para cumplir los requisitos de aprendizaje descritos anteriormente y los pasos detallados que puede seguir para configurar un laboratorio para el aprendizaje.  

## <a name="create-the-lab-account-as-a-lab-account-administrator"></a>Creación de una cuenta de laboratorio como un administrador de cuenta de laboratorio
El primer paso para usar Azure Lab Services es crear una cuenta de laboratorio en Azure Portal. Una vez que un administrador de cuenta de laboratorio crea la cuenta de laboratorio, agrega a los usuarios que deseen crear laboratorios al rol **Creador de laboratorio**. Los instructores crean laboratorios con máquinas virtuales para que los alumnos realicen los ejercicios del curso que están impartiendo. Para obtener más información, consulte [Administración de cuentas de laboratorio en Azure Lab Services](how-to-manage-lab-accounts.md).

## <a name="create-and-manage-classroom-labs"></a>Creación y administración de laboratorios educativos
Un instructor que sea miembro del rol Creador de laboratorio en una cuenta de laboratorio puede crear uno o varios laboratorios en la cuenta de laboratorio. Cree y configure una plantilla de máquina virtual con todo el software necesario para realizar los ejercicios del curso. Tome una imagen lista para usar del conjunto de imágenes disponibles para crear un laboratorio educativo y luego personalícelo mediante la instalación del software requerido para ello. Para obtener más información, consulte [Crear y administrar laboratorios educativos](how-to-manage-classroom-labs.md).

## <a name="configure-usage-settings-and-policies"></a>Configuración de los valores y directivas de uso
El creador de laboratorio puede agregar o quitar usuarios del laboratorio; obtener el vínculo de registro para enviar a los usuarios del laboratorio; establecer directivas, como la configuración de cuotas individuales por usuario; o actualizar el número de máquinas virtuales disponibles en el laboratorio, entre otras acciones. Para obtener más información, consulte [Configuración de los valores y directivas de uso](how-to-configure-student-usage.md).

## <a name="create-and-manage-schedules"></a>Creación y administración de programaciones
Las programaciones le permiten configurar un laboratorio educativo de forma que las máquinas virtuales que contiene se inicien y apaguen automáticamente a una hora determinada. Puede definir una programación puntual o periódica. Para obtener más información, consulte [Creación y administración de programaciones para laboratorios educativos en Azure Lab Services](how-to-create-schedules.md).

## <a name="set-up-and-publish-a-template-vm"></a>Configuración y publicación de una máquina virtual de plantilla
En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configure la máquina virtual de la plantilla de modo que esté configurada exactamente con lo que desea proporcionar a los asistentes al aprendizaje. Puede proporcionar un nombre y una descripción de la plantilla que verán los usuarios del laboratorio. Luego, publique la plantilla si desea que las instancias de la plantilla de máquina virtual estén disponibles para los usuarios del laboratorio. Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso es igual al número máximo de usuarios permitidos en el laboratorio, que se puede establecer en la política de uso del laboratorio. Todas las máquinas virtuales tienen la misma configuración que la plantilla. Para obtener más información, consulte [Creación y administración de plantillas educativas en Azure Lab Services](how-to-create-manage-template.md). 

## <a name="use-vms-in-the-classroom-lab"></a>Uso de máquinas virtuales en el laboratorio educativo
Un alumno o asistente a la sesión de aprendizaje se registra en el laboratorio y se conecta a la máquina virtual para realizar los ejercicios del curso. Para obtener más información, consulte [Acceso a un laboratorio educativo en Azure Lab Services](how-to-use-classroom-lab.md).

## <a name="next-steps"></a>Pasos siguientes
Empiece por crear una cuenta de laboratorio en laboratorios educativos siguiendo las instrucciones del artículo: [Tutorial: Configuración de una cuenta de laboratorio con Azure Lab Services](tutorial-setup-lab-account.md).