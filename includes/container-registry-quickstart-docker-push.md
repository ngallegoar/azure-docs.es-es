---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 09eaf9465ec3912dea6e1f3ee1693f6bfed50abc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "67186871"
---
## <a name="push-image-to-registry"></a>Insertar la imagen en el registro

Para insertar una imagen en Azure Container Registry, primero debe tener una imagen. Si aún no tiene ninguna imagen de contenedor local, ejecute el siguiente comando [docker pull][docker-pull] para extraer una imagen existente de Docker Hub. Para este ejemplo, extraiga la imagen `hello-world`.

```
docker pull hello-world
```

Para poder insertar una imagen en el registro, debe etiquetarla con el nombre completo del servidor de inicio de sesión de ACR. El nombre del servidor de inicio de sesión tiene el formato *\<nombre-registro\>.azurecr.io* (todo en minúscula), por ejemplo, *mycontainerregistry007.azurecr.io*.

Etiquete la imagen mediante el comando [docker tag][docker-tag]. Reemplace `<acrLoginServer>` por el nombre del servidor de inicio de sesión de la instancia de ACR.

```
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Por último, use el comando [docker push][docker-push] para insertar la imagen en la instancia de ACR. Reemplace `<acrLoginServer>` por el nombre del servidor de inicio de sesión de la instancia de ACR. Este ejemplo crea el repositorio **hello-world**, que contiene la imagen `hello-world:v1`.

```
docker push <acrLoginServer>/hello-world:v1
```

Después de insertar la imagen en el registro de contenedor, quite la imagen `hello-world:v1` de su entorno de Docker local. (Tenga en cuenta que el comando [docker rmi][docker-rmi] no quita la imagen del repositorio **hello-world** del registro de contenedor de Azure).

```
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

