---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 94cac0932da5880e5e7b8a8fac3870b5bc464af9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75564807"
---
## <a name="register-extensions"></a>Registro de las extensiones

A excepción de los desencadenadores de HTTP y del temporizador, los enlaces de Functions en las versiones 2.x y superiores del runtime se implementan como paquetes de extensión. Tanto en la versión 2.x del runtime de Azure Functions como en las posteriores, debe registrar explícitamente las extensiones de los tipos de enlace que use en sus funciones. Las excepciones son los enlaces de HTTP y los desencadenadores de temporizador, que no requieren extensiones.

Puede instalar las extensiones de enlace individualmente o puede agregar una referencia de un conjunto de extensiones al archivo del proyecto host.json. Los conjuntos de extensiones eliminan la posibilidad de tener problemas de compatibilidad con los paquetes cuando se usan varios tipos de enlace. Este el enfoque recomendado para registrar extensiones de enlace. Los conjuntos de extensiones también eliminan el requisito de instalar el SDK de .NET Core 2.x. 

### <a name="extension-bundles"></a>Conjuntos de extensiones

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Para obtener más información, consulte [Register Azure Functions binding extensions](../articles/azure-functions/functions-bindings-register.md#extension-bundles) (Registrar las extensiones de enlace de Azure Functions). Debe agregar los conjuntos de extensiones a host.json antes de agregar enlaces al archivo functions.json.

### <a name="register-individual-extensions"></a>Registrar extensiones individuales

Si necesita instalar extensiones que no están en un conjunto, puede registrar manualmente paquetes de extensiones individuales para enlaces específicos. 

> [!NOTE]
> Para registrar manualmente las extensiones utilizando `func extensions install`, debe tener instalado el SDK de .NET Core 2.x.

Después de haber actualizado el archivo *function.json* para que incluya todos los enlaces que necesita la función, ejecute el siguiente comando en la carpeta del proyecto.

```bash
func extensions install
```

El comando lee el archivo *function.json* para ver qué paquetes necesita, los instala y, luego, recompila el proyecto de extensiones. Agrega los nuevos enlaces en la versión actual, pero no actualiza los enlaces existentes. Use la opción `--force` para actualizar los enlaces existentes a la versión más reciente cuando instale otros nuevos.
