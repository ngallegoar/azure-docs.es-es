---
title: Hospedaje del código fuente de la aplicación móvil en la nube con GitHub y Azure DevOps
description: Conozca los servicios de Microsoft para hospedar el código de la aplicación móvil en la nube.
author: codemillmatt
ms.assetid: 12a8a079-9b3c-4faf-2222-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 66d8980ab53010af0703d789fbe791c60a32052d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240959"
---
# <a name="cloud-hosted-source-code-management-services"></a>Servicios de administración del código fuente hospedados en la nube
Si tiene equipos de desarrollo con varios miembros que trabajan con el mismo código base, debe encontrar un lugar adecuado para hospedar el código. Almacenar los datos en la nube y tener un repositorio centralizado permite a todo el mundo cargar, editar y administrar los archivos de código. También pueden interactuar con otros desarrolladores en proyectos. El código puede estar accesible de inmediato con independencia de dónde se encuentre, siempre que tenga conexión a Internet.

El hospedaje en la nube es mucho más sencillo que las opciones locales. Requiere menos configuración de hardware y permite a las organizaciones realizar el proceso de implementación de una manera más ágil.

## <a name="benefits-of-hosting-source-code-in-the-cloud"></a>Ventajas de hospedar el código fuente en la nube
- **Almacenamiento en la nube centralizado:** consulte y administre los datos desde cualquier lugar.
- **Mejor colaboración y código más limpio:** Haga un seguimiento del código dentro de los equipos y administre los proyectos a fin de garantizar la entrega continua de software excelente.
- **Participación más sencilla:** contribuya fácilmente a sus proyectos.
- **Ciclo de versión más rápido:** trabaje más rápido en sus equipos y contribuya fácilmente a sus proyectos.
- **Reduzca el costo:** no se preocupe del mantenimiento de su propio hardware, servidores, VPN, etc.

Use los siguientes servicios para hospedar los datos de la aplicación en la nube.

## <a name="github"></a>GitHub
[GitHub](https://github.com/) es un servicio de hospedaje de repositorio de código abierto que hospeda proyectos de código fuente en varios lenguajes de programación. GitHub realiza un seguimiento de los distintos cambios realizados en cada iteración.

**Características principales**
- Use el hospedaje del código con todo el código en un solo lugar. Tanto sin son privados, públicos o de código abierto, todos los repositorios están equipados con herramientas para ayudarle a hospedar el código, controlar sus versiones y publicarlo.
- Revise el código y use las herramientas de revisión integradas para hacer de la revisión del código una parte esencial de cualquier proceso en equipo.
    - Proteja las ramas, proponga cambios y solicite revisiones.
    - Detecte diferencias, haga comentarios en contexto y reciba información clara.
- Coordine antes los procesos, permanezca alineado y sea más productivo con las herramientas de administración de proyectos de GitHub:
    - Obtenga una visión global del proyecto.
    - Use paneles de tareas justo al lado del código en GitHub.
    - Arrastre las tarjetas para permitirle asignar problemas o solicitudes de incorporación de cambios a los miembros del equipo.
    - Defina hitos para organizar y realizar un seguimiento del progreso.
    - Escriba notas para capturar ideas que podrían ser útiles pero que no pertenecen a un problema o una solicitud de incorporación de cambios determinados.
- Descubra y elija fácilmente las herramientas adecuadas para mejorar la comunicación y la automatización del trabajo en el [Marketplace de GitHub](https://github.com/marketplace) (el lugar de compra de las aplicaciones).
- Administrar y ampliar los equipos mediante: 
    - Roles de usuario para ayudar a organizar los equipos y los permisos de acceso.
    - Herramientas de subprocesos de discusión para mantener centradas las conversaciones sobre temas y equipos.
    - Instrucciones de la comunidad para configurar rápidamente nuevos miembros del equipo con una cuenta.
- Examine y puntúe con estrellas los proyectos más populares para seguirlos.
- Cree redes para aprender de otras personas del sector.

**Referencias**
- [GitHub](https://github.com/)
- [Guías de GitHub](https://guides.github.com/)
- [Foro de la comunidad de GitHub](https://github.community/)
- [Marketplace de GitHub](https://github.com/marketplace)

## <a name="azure-devops"></a>Azure DevOps
[Azure DevOps](https://azure.microsoft.com/services/devops/) admite [Azure Repos](https://azure.microsoft.com/services/devops/repos/) y el [control de versiones de Team Foundation (TFVC)](https://docs.microsoft.com/azure/devops/repos/tfvc/index?view=azure-devops) como opciones de control del código fuente. Dispone de repositorios privados gratuitos ilimitados con revisiones de código en colaboración, administración avanzada de archivos, búsqueda de código y directivas de rama para garantizar la alta calidad del código. Azure Repos es muy adecuado para proyectos pequeños, así como para grandes organizaciones que necesitan compatibilidad nativa con Azure Active Directory y directivas avanzadas.
    
**Características principales**
- Use un repositorio ilimitado de código fuente de GIT hospedado en la nube para sus repositorios públicos y privados.
    - Obtenga compatibilidad con cualquier cliente GIT.
    - Use webhooks y la integración con la API.
- Ponga en marcha la próxima compilación con una solicitud de incorporación de cambios en el repositorio.
    - Colabore para compilar mejor el código mediante hilos de discusiones y la integración continua de cada cambio.
    - Configure la integración y la entrega continuas (CI/CD) para desencadenar automáticamente compilaciones, pruebas e implementaciones cada vez que se complete una solicitud de incorporación de cambios. Puede usar Azure Pipelines o sus herramientas.
    - Proteja la calidad del código con directivas de rama.
- Mantenga un control de versiones centralizado con el control de versiones de Team Foundation, que incluye la revisión del código.
- Conéctese al código con Xcode, Eclipse, IntelliJ, Android Studio, Visual Studio, Visual Studio Code, etc.
- Use una búsqueda semántica eficaz de código.
- Disfrute de las ventajas de las características preparadas para la empresa. Azure DevOps tiene integración nativa con Azure Active Directory, lo que simplifica el proceso de administrar el acceso a los repositorios de código.
- Garantice la calidad del código con directivas de rama, como el número mínimo de revisiones de código, los requisitos para una compilación correcta y la aplicación de estrategias de combinación de GIT.
- Conecte su entorno de desarrollo favorito para acceder a los repositorios y administrar el trabajo.

**Referencias**
- [Introducción a Azure Repos](https://azure.microsoft.com/services/devops/repos/) 
- [Documentación de Azure Repos](/azure/devops/repos/?view=azure-devops)