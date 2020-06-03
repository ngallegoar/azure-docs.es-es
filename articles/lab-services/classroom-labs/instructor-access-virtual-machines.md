---
title: Acceso del formador a las VM en Azure Lab Services
description: En este artículo se muestra cómo los formadores pueden acceder a las VM de sus alumnos desde la vista Formador. Por ejemplo, un profesor ayudante puede ser el formador de una clase, aunque sea alumno en otras.
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
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: 4b4e6bf7b69f1b4c7664a104a0af70d9e486f08a
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83699958"
---
# <a name="access-virtual-machines-as-a-student-from-the-educator-view"></a>Acceso a las máquinas virtuales como alumno desde la vista Formador
En este artículo se muestra cómo los formadores pueden tener acceso a sus VM para las clases a las que asisten como alumnos. 

A continuación se presenta un escenario en el que esta característica resulta útil. Un profesor ayudante es formador de una clase, aunque es un alumno en otras. Además, el profesor ayudante quiere ver las VM de los alumnos, así como tener acceso a ellas, desde la vista Formador, que muestra los laboratorios que posee. 

## <a name="access-vms-from-educator-view"></a>Acceso a las VM desde la vista Formador

1. Inicie sesión en el [sitio web de Azure Lab Services](https://labs.azure.com). Se mostrarán los laboratorios de su propiedad. Estos pueden ser laboratorios que creó usted mismo o en los que el administrador lo asignó como propietario. Para obtener más información, consulte [Cómo agregar otros propietarios a un laboratorio existente](how-to-add-user-lab-owner.md).
2. Para acceder a las máquinas virtuales de las clases a las que asiste como alumno, seleccione el icono del equipo en la esquina superior derecha. Confirme que ve las máquinas virtuales a las que puede acceder como alumno. En el ejemplo siguiente, el usuario es un profesor ayudante para el laboratorio de Python, pero es un alumno en el laboratorio de Java. Por lo tanto, ve la máquina virtual del laboratorio de Java en la lista desplegable. El usuario puede iniciar la máquina virtual y conectarse a ella. 
    
    ![Acceso a máquinas virtuales de alumnos](../media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Conexión a una máquina virtual](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Conexión a una máquina virtual mediante RDP en un equipo Mac](connect-virtual-machine-mac-remote-desktop.md)
- [Conexión a una máquina virtual mediante RDP en un equipo Chromebook](connect-virtual-machine-chromebook-remote-desktop.md).
- [Uso del escritorio remoto para máquinas virtuales con Linux](how-to-use-remote-desktop-linux-student.md)
