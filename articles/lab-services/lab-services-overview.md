---
title: Acerca de Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo Lab Services puede facilitar la creación, administración y protección de laboratorios con máquinas virtuales que pueden usar los desarrolladores, evaluadores, formadores, alumnos y otros usuarios.
ms.topic: overview
ms.date: 06/26/2020
ms.openlocfilehash: 7661b26d16eb2c2acd53c30889ea791689c1eec0
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85442812"
---
# <a name="an-introduction-to-azure-lab-services"></a>Introducción a Azure Lab Services
**Azure Lab Services** le permite crear laboratorios cuya infraestructura la administra Azure. Actualmente, el laboratorio educativo es el único tipo de laboratorio administrado compatible con Azure Lab Services. El servicio gestiona la administración de toda la infraestructura de un tipo de laboratorio administrado, desde la sincronización de las máquinas virtuales hasta la gestión de errores y el escalado de dicha infraestructura. Una vez que un administrador de TI crea una cuenta de laboratorio en Azure Lab Services, un instructor puede configurar rápidamente un laboratorio para la clase, especificar el número y tipo de máquinas virtuales necesarias para los ejercicios de la clase y agregar usuarios a la clase. En cuanto un usuario se registra en la clase puede acceder a la máquina virtual para realizar los ejercicios de la clase.  

## <a name="key-capabilities"></a>Principales capacidades
Azure Lab Services es compatible con las siguientes características o funcionalidades clave:

- **Configuración rápida y flexible de un laboratorio**. Con Azure Lab Services, los propietarios de laboratorios pueden configurar un laboratorio rápidamente según sus necesidades. El servicio ofrece la opción de encargarse de todo el trabajo de infraestructura de Azure para los tipos de laboratorio administrados. El servicio proporciona escalado integrado y resiliencia de la infraestructura para laboratorios que administre el servicio.
- **Experiencia simplificada para usuarios de laboratorios**. Los usuarios pueden registrarse en un laboratorio con un código de registro y acceder a él en cualquier momento para usar sus recursos. 
- **Optimización de costos y análisis**. Un propietario de laboratorio puede establecer programaciones de laboratorio para apagar e iniciar máquinas virtuales automáticamente. El propietario del laboratorio puede establecer una programación para especificar las franjas horarias de disponibilidad de las máquinas virtuales del laboratorio para los usuarios, establecer directivas de uso por usuario o por laboratorio para optimizar costos. 

Si solo quiere insertar lo que necesita en un laboratorio y permitir que el servicio configure y administre la infraestructura necesaria para dicho laboratorio, elija **tipos de laboratorios administrados**. Actualmente, el **laboratorio educativo** es el único tipo de laboratorio administrado que se puede crear con Azure Lab Services.

En las siguientes secciones, se proporcionan más detalles de estos laboratorios. 

## <a name="managed-lab-types"></a>Tipos de laboratorio administrado
Azure Lab Services permite crear laboratorios cuya infraestructura la administra Azure. En este artículo se hace referencia a ellos como tipos de laboratorios administrados. Los tipos de laboratorios administrados ofrecen diferentes tipos de laboratorio que se ajustan a sus necesidades. Actualmente, el único tipo de laboratorio administrado que se admite es el **laboratorio educativo**. 

Los tipos de laboratorios administrados le permiten ponerse en marcha inmediatamente, con una configuración mínima. El servicio controla toda la administración de la infraestructura para el laboratorio, desde la sincronización de máquinas virtuales al control de errores y el escalado de infraestructuras. Para crear un tipo de laboratorio administrado, como puede ser un laboratorio educativo, debe crear antes una cuenta de laboratorio para la organización. La cuenta de laboratorio actúa como cuenta central en la que se administran todos los laboratorios de la organización. 

Al crear y usar recursos de Azure en estos tipos de laboratorios administrados, el servicio crea y administra los recursos en las suscripciones internas de Microsoft. No se crean en su propia suscripción de Azure. El servicio realiza un seguimiento del uso de estos recursos en las suscripciones internas de Microsoft. Este uso se factura a la suscripción de Azure que contiene la cuenta de laboratorio.   

Estos son algunos de los **casos de uso de los tipos de laboratorios administrados**: 

- Proporcione a los alumnos un laboratorio de máquinas virtuales que se configuran exactamente con todo lo necesario para una clase. Asigne a cada alumno un número limitado de horas para el uso de máquinas virtuales para deberes o proyectos personales.
- Configure un grupo de máquinas virtuales de proceso de alto rendimiento para realizar investigaciones de procesos o gráficos intensivos. Ejecute las máquinas virtuales según sus necesidades y limpie las máquinas cuando haya terminado. 
- Desplace un laboratorio de equipos físicos del centro educativo a la nube. Escale automáticamente el número de máquinas virtuales solo al umbral de costo y uso máximo que establezca en el laboratorio.  
- Aprovisione rápidamente un laboratorio de máquinas virtuales para un host de Hackathon. Elimine el laboratorio con un solo clic cuando haya terminado. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes tutoriales para obtener instrucciones paso a paso sobre cómo crear una cuenta de laboratorio y un laboratorio educativo.

- [Tutorial: Configuración de una cuenta de laboratorio](tutorial-setup-lab-account.md)
- [Tutorial: Creación de un laboratorio educativo](tutorial-setup-classroom-lab.md)
