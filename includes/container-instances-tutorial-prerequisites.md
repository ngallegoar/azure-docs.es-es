---
title: archivo de inclusión
description: archivo de inclusión
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 10fb9e8169b7f4159ccbf4a0ff36021f6033f811
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75552431"
---
Para completar este tutorial es preciso cumplir los siguientes requisitos:

**CLI de Azure**: debe tener la versión 2.0.29 de la CLI de Azure, cualquier versión posterior, instalada en el equipo local. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

**Docker**: en este tutorial se supone que el usuario tiene un conocimiento básico de los principales conceptos de Docker, como los contenedores, las imágenes de contenedor y los comandos básicos de `docker`. Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker][docker-get-started].

**Docker**: para completar este tutorial, es preciso que el motor de Docker esté instalado localmente. Docker dispone de paquetes que configuran el entorno de Docker en [macOS][docker-mac], [Windows][docker-windows] y [Linux][docker-linux].

> [!IMPORTANT]
> Dado que Azure Cloud Shell no incluye el demonio de Docker, para completar este tutorial *es preciso* instalar tanto la CLI de Azure como el motor de Docker en el *equipo local*. Azure Cloud Shell no se puede instalar para este tutorial.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
