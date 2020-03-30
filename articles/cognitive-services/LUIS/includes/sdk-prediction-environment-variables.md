---
title: archivo de inclusión
description: archivo de inclusión
ms.topic: include
ms.date: 01/23/2020
ms.openlocfilehash: a7f4e0b47b3aaf9924f8c82eb6a9fa37decc0b2a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76772416"
---
### <a name="create-an-environment-variable"></a>Creación de una variable de entorno

Mediante la clave del runtime y el punto de conexión del runtime cree variables de entorno para la autenticación y el acceso:

* `LUIS_RUNTIME_KEY`: la clave de recurso del runtime para autenticar las solicitudes.
* `LUIS_RUNTIME_ENDPOINT`: el punto de conexión del runtime asociado a la clave.
* `LUIS_APP_ID`: el identificador de la aplicación de IoT de LUIS público es `df67dcdb-c37d-46af-88e1-8b97951ca1c2`.
* `LUIS_APP_SLOT_NAME` - `production` o `staging`

Si tiene previsto usar este inicio rápido para acceder a su propia aplicación, debe realizar varios pasos adicionales:
* Crear la aplicación y obtener su identificador
* Asignar la clave del runtime a la aplicación en el portal de LUIS
* Probar que con la dirección URL del explorador puede acceder a la aplicación

Siga las instrucciones adecuadas para su sistema operativo.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx LUIS_RUNTIME_KEY <replace-with-your-resource-key>
setx LUIS_RUNTIME_ENDPOINT <replace-with-your-resource-endpoint>
setx LUIS_APP_ID <replace-with-your-app-id>
setx LUIS_APP_SLOT_NAME <replace-with-production-or-staging>
```

Después de agregar las variables de entorno reinicie la ventana de la consola.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Después de agregar las variables de entorno ejecute `source ~/.bashrc` en la ventana de consola para que los cambios surtan efecto.

#### <a name="macos"></a>[macOS](#tab/unix)

Edite `.bash_profile` y agregue la variable de entorno:

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Después de agregar las variables de entorno ejecute `source .bash_profile` en la ventana de consola para que los cambios surtan efecto.

***
