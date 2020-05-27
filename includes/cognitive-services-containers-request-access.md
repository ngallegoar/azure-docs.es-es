---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.openlocfilehash: 66bd78c94e6c54d26959778cc059730c13d02629
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83698569"
---
El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar el contenedor. Después de enviar el formulario, el equipo de Azure Cognitive Services lo revisa para asegurarse de que cumple los criterios de acceso al registro de contenedor privado.

> [!IMPORTANT]
> Debe usar una dirección de correo electrónico asociada con una cuenta de Microsoft (MSA) o de Azure Active Directory (Azure AD) en el formulario.

Si se aprueba la solicitud, recibirá un correo electrónico con instrucciones que describen cómo obtener las credenciales y acceder al registro de contenedor privado.

## <a name="log-in-to-the-private-container-registry"></a>Inicio de sesión en el registro de contenedor privado

Hay varias maneras de autenticarse con el registro de contenedor privado para los contenedores de Cognitive Services. Se recomienda que utilice el método de línea de comandos mediante la [CLI de Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Use el comando [docker login](https://docs.docker.com/engine/reference/commandline/login/), como se muestra en el ejemplo siguiente, para iniciar sesión en `containerpreview.azurecr.io`, que es el registro de contenedor privado de los contenedores de Cognitive Services. Reemplace *\<username\>* por el nombre de usuario y *\<password\>* por la contraseña proporcionada en las credenciales que recibió del equipo de Azure Cognitive Services.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Si ha protegido las credenciales en un archivo de texto, puede concatenar el contenido del archivo de texto con el comando `docker login`. Use el comando `cat`, como se muestra en el siguiente ejemplo: Sustituya *\<passwordFile\>* por la ruta y el nombre del archivo de texto que contiene la contraseña. Reemplace *\<username\>* por el nombre de usuario proporcionado en las credenciales.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

