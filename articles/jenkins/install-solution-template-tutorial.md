---
title: 'Tutorial: Creación de un servidor de Jenkins en Azure'
description: En este tutorial, instalará Jenkins en una máquina virtual Linux de Azure a partir de la plantilla de solución de Jenkins y compilará una aplicación Java de ejemplo.
keywords: jenkins, azure, devops, portal, máquina virtual, plantilla de solución
ms.topic: tutorial
ms.date: 03/03/2020
ms.openlocfilehash: 3fa8dcbcc0edcc987fa9c4e76fbdb9d7a2f3396c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78274534"
---
# <a name="tutorial-create-a-jenkins-server-on-an-azure-linux-vm"></a>Tutorial: Creación de un servidor de Jenkins en una máquina virtual Linux de Azure 

Este tutorial muestra cómo instalar [Jenkins](https://jenkins.io) en una máquina virtual Ubuntu Linux con las herramientas y complementos configurados para que funcionen con Azure. Cuando haya terminado, dispondrá de un servidor de Jenkins en ejecución en Azure que permitirá compilar una aplicación Java de ejemplo desde [GitHub](https://github.com).

> [!div class="checklist"]
> * Instalar y configurar un servidor de Jenkins en Azure
> * Acceso a la consola de Jenkins mediante un túnel SSH
> * Creación de un proyecto de Freestyle
> * Compilación del código y empaquetamiento de la aplicación de ejemplo

[!INCLUDE [jenkins-install-solution-template-include](../../includes/jenkins-install-solution-template-steps.md)]