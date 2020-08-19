---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/07/2020
ms.author: glenga
ms.openlocfilehash: 2936d22eacef73daef4433b3fd296dd4757fa410
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2020
ms.locfileid: "88031137"
---
Si no puede usar paquetes de extensión, utilice Azure Functions Core Tools localmente para instalar los paquetes de extensión específicos que requiere el proyecto. 

> [!NOTE]
> Para instalar manualmente las extensiones utilizando, debe tener instalado el SDK de .NET Core 2.x.

Cuando se instalan las extensiones explícitamente, se agrega un archivo de proyecto .NET denominado extensions.csproj a la raíz del proyecto. Este archivo define el conjunto de paquetes NuGet requeridos por las funciones. Aunque puede trabajar con las [referencias de paquetes de NuGet](/nuget/consume-packages/package-references-in-project-files) en este archivo, las herramientas principales le permiten instalar extensiones sin tener que editar manualmente el archivo.

Hay varias maneras de usar las herramientas principales para instalar las extensiones necesarias en el proyecto local. 

#### <a name="install-all-extensions"></a>Instalación de todas las extensiones 

Utilice el comando siguiente para agregar automáticamente todos los paquetes de extensiones que usan los enlaces del proyecto local:

```dotnetcli
func extensions install
```
El comando lee el archivo *function.json* para ver qué paquetes necesita, los instala y, luego, recompila el proyecto de extensiones. Agrega los nuevos enlaces en la versión actual, pero no actualiza los enlaces existentes. Use la opción `--force` para actualizar los enlaces existentes a la versión más reciente cuando instale otros nuevos.

#### <a name="install-a-specific-extension"></a>Instalación de una extensión específica

Utilice el siguiente comando para instalar un paquete de extensiones específico en una versión específica, en este caso la extensión de almacenamiento:

```dotnetcli
func extensions install --package Microsoft.Azure.WebJobs.Extensions.Storage --version 4.0.2
```