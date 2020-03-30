---
title: Instalación de la CLI de DC/OS | Microsoft Docs
description: Instale la CLI de DC/OS.
services: container-service
documentationcenter: ''
author: rgardler
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: Contenedores, microservicios, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: e2601d5200f0b8ebcfb856bffb871f01b3acb215
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186963"
---
> [!NOTE]
> Este procedimiento se usa para trabajar con clústeres ACS basados en DC/OS. No es necesario realizarlo en clústeres ACS basados en Swarm.
> 
> 

En primer lugar, [conéctese a su clúster ACS basado en DC/OS](../articles/container-service/container-service-connect.md). Una vez que haya hecho esto, puede instalar la CLI de DC/OS en el equipo cliente con los siguientes comandos:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Si usa una versión antigua de Python, puede que observe algunos mensajes "InsecurePlatformWarnings". Puede omitir estos errores con seguridad.

Para empezar a trabajar sin necesidad de reiniciar el shell, ejecute:

```bash
source ~/.bashrc
```

Este paso no será necesario si inicia shells nuevos.

Ahora puede confirmar que la CLI está instalada:

```bash
dcos --help
```