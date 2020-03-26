---
title: Introducción a Azure DevOps Projects | Microsoft Docs
description: Conozca el valor de Azure DevOps Projects
services: devops-project
documentationcenter: ''
author: mlearned
manager: gwallace
editor: mlearned
ms.assetid: ''
ms.service: az-devops-project
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/03/2018
ms.author: mlearned
ms.openlocfilehash: d39702f8e30046dd3cf634fc67ed7095471cd629
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "71971488"
---
# <a name="overview-of-azure-devops-projects"></a>Introducción a Azure DevOps Projects

 Con Azure DevOps Projects es fácil empezar a usar Azure. Le ayuda a iniciar su aplicación favorita en el servicio de Azure que elija en unos pocos pasos rápidos desde Azure Portal. 

 DevOps Projects configura todo lo necesario para desarrollar, implementar y supervisar la aplicación. El panel de DevOps Projects se puede usar para supervisar confirmaciones de código, compilaciones e implementaciones, y todo ello, desde una sola vista en Azure Portal.

## <a name="why-should-i-use-devops-projects"></a>¿Por qué debo usar DevOps Projects?

  DevOps Projects automatiza la configuración de toda una canalización de integración continua (CI) y de entrega continua (CD) en Azure.  Puede empezar con el código existente o usar una de las aplicaciones de ejemplo proporcionadas. Luego puede implementar rápidamente dicha aplicación en varios servicios de Azure como Virtual Machines, App Service, Azure Kubernetes Services (AKS), Azure SQL Database y Azure Service Fabric.  

  DevOps Projects realiza todo el trabajo de la configuración inicial de una canalización de DevOps, lo que incluye configurar el repositorio Git inicial y canalizar de CI/CD, canalizar un recurso de Application Insights para la supervisión y proporcionar una sola vista de toda la solución con la creación de un panel de DevOps Projects en Azure Portal.

Puede usar DevOps Projects para:

* Implementar rápidamente la aplicación en Azure
* Automatizar la configuración de una canalización de CI/CD
* Ver cómo se configura correctamente una canalización de CI/CD y aprender a hacerlo
* Personalizar aún más las canalizaciones de versión en función de sus escenarios concretos

## <a name="how-do-i-use-devops-projects"></a>¿Cómo se usa DevOps Projects?

  DevOps Projects está disponible en Azure Portal. Los recursos de DevOps Projects se crean como cualquier otro recurso de Azure desde el portal. DevOps Projects proporciona una experiencia de asistente paso a paso para las distintas opciones de configuración.  

Elija varias opciones de configuración como parte de la instalación inicial. Entre estas opciones se incluyen:

* El uso de la aplicación de ejemplo proporcionada o de su propio código
* Selección del lenguaje de una aplicación
* Elección de un marco de aplicación en función del lenguaje
* Selección de un servicio de Azure (destino de implementación)
* Creación de una organización de Azure DevOps nueva o uso de una existente 
* Elección de una suscripción de Azure
* Elección de la ubicación de los servicios de Azure
* Elección entre varios planes de tarifa de servicios de Azure

Después de usar DevOps Projects también puede eliminar todos los recursos de un solo lugar desde el panel de DevOps Projects en Azure Portal.

## <a name="devops-projects-and-azure-devops-integration"></a>Integración de DevOps Projects y Azure DevOps

DevOps Projects cuenta con tecnología de Azure DevOps. DevOps Projects automatiza todo el trabajo que se necesita en Azure Pipelines para configurar una canalización de CI/CD. Crea un repositorio Git en una organización de Azure DevOps nueva o existente y, luego, confirma una aplicación de ejemplo o el código existente en un nuevo repositorio Git.  

La automatización también establece un desencadenador de integración continua para la compilación, con el fin de que cada confirmación de código inicie una compilación. DevOps Projects crea un desencadenador de implementación continua e implementa cada nueva compilación correcta en el servicio de Azure que elija.  

Las canalizaciones de compilación y versión se pueden personalizar para otros escenarios. Además, puede clonar las canalizaciones de compilación y de versión para usarlas en otros proyectos.

Después de crear la instancia de DevOps Projects, también puede:

* Personalizar la canalización de compilación y versión
* Usar solicitudes de incorporación de cambios para administrar el flujo de código y mantener un alto nivel de calidad
* Probar y compilar cada confirmación antes de fusionar el código para elevar el listón de calidad
* Realizar un seguimiento de los problemas y el trabajo pendiente junto con la aplicación

## <a name="how-do-i-start-using-devops-projects"></a>¿Cómo empiezo a usar DevOps Projects?

* [Introducción a DevOps Projects](https://docs.microsoft.com/azure/devops-project/azure-devops-project-github)

##  <a name="devops-projects-videos"></a>Vídeos de DevOps Projects

* [Creación de CI/CD con Azure DevOps Projects](https://www.youtube.com/watch?v=NuYDAs3kNV8)
