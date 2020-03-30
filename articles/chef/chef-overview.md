---
title: Uso de Chef con Azure
description: Introducción al uso de Chef para configurar y probar la infraestructura de Azure
keywords: azure, chef, devops, máquinas virtuales, introducción, automatizar
ms.date: 02/22/2020
ms.topic: article
ms.openlocfilehash: c22faa69bf8f42111d328a9c156dc1a2432731b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586349"
---
# <a name="using-chef-with-azure"></a>Uso de Chef con Azure
[Chef](https://www.chef.io) es una plataforma de automatización eficaz que transforma la infraestructura de máquinas virtuales de Azure en código. Chef automatiza cómo se configura, implementa y administra la infraestructura a través de la red, independientemente de su tamaño.

En este artículo se describen las ventajas de usar Chef para administrar la infraestructura de Azure.

## <a name="chef-extension-on-azure"></a>Extensión de Chef en Azure
Aprovisione una máquina virtual con el cliente de Chef que se ejecuta como servicio en segundo plano con la [extensión de Chef](https://docs.microsoft.com/azure/chef/chef-extension-portal) en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040). Una vez aprovisionadas, estas máquinas virtuales están listas para que un servidor de Chef las administre.

## <a name="chef-cloud-shell"></a>Chef en Cloud Shell
Use la estación de trabajo de Chef directamente en Azure Cloud Shell. Ejecute todas las utilidades de Chef e InSpec directamente desde Cloud Shell. Puede utilizar los comandos de Chef desde:

* [chef](https://docs.chef.io/ctl_chef.html)
* [kitchen](https://docs.chef.io/ctl_kitchen.html)
* [inspec](https://www.inspec.io/docs/reference/cli/)
* [knife](https://docs.chef.io/knife.html)
* [cookstyle](https://docs.chef.io/cookstyle.html)
* [chef-run](https://www.chef.sh/docs/chef-workstation/getting-started/)

Combine nuestras utilidades de comandos con las demás herramientas disponibles en Cloud Shell, como `git`, `az-cli` y `terraform` y escriba la automatización de infraestructura y cumplimiento desde el explorador.

## <a name="automate-infrastructure-apps-and-compliance-with-one-platform"></a>Automatización de la infraestructura, las aplicaciones y el cumplimiento en una plataforma
Las empresas requieren velocidad y seguridad para competir en el mercado digital. Chef y Microsoft ayudan a los usuarios, los equipos y las empresas a realizar todas estas acciones. Con una plataforma, Chef Automate, ahora puede automatizar y entregar continuamente su infraestructura, aplicaciones y el cumplimiento en sus recursos de Microsoft.

## <a name="test-drive-chef-automate-on-azure"></a>Versión de prueba de Chef Automate en Azure
Con tecnología de Chef, la [solución de Azure Marketplace Chef Automate](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/chef-software.chef-automate) permite crear, implementar y administrar la infraestructura y las aplicaciones de forma colaborativa. Con tan solo un clic, obtiene acceso inmediato a todas las características comerciales incluidas con Chef Automate, consigue visibilidad de un extremo a otro de toda la flota, habilita el cumplimiento continuo y administra todos los cambios con un flujo de trabajo unificado.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una máquina virtual Windows en Azure mediante Chef](chef-automation.md)
