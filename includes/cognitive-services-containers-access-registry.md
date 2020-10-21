---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b9d0d2b97472eb3264f5e4600fddbfc7d3250918
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67704317"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Uso de la CLI de Docker para autenticar un registro de contenedor privado

Puede autenticarse con el registro de contenedor privado de contenedores de Cognitive Services, pero el método recomendado de la línea de comandos es mediante el uso de la [CLI de Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Use el comando [`docker login`](https://docs.docker.com/engine/reference/commandline/login/), como se muestra en el ejemplo siguiente, para iniciar sesión en `containerpreview.azurecr.io`, el registro de contenedor privado de los contenedores de Cognitive Services. Reemplace *\<username\>* por el nombre de usuario y *\<password\>* por la contraseña proporcionada en las credenciales que recibió del equipo de Azure Cognitive Services.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Si ha protegido las credenciales en un archivo de texto, puede concatenar el contenido de dicho archivo de texto, mediante el comando `cat`, al comando `docker login`, tal como se muestra en el ejemplo siguiente. Reemplace *\<passwordFile\>* por la ruta de acceso y el nombre del archivo de texto que contiene la contraseña, y *\<username\>* por el nombre de usuario proporcionado en las credenciales.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
