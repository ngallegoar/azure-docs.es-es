---
title: Lenguajes admitidos en Azure Functions
description: Obtenga información sobre los lenguajes admitidos (GA) y los que se encuentran en versión preliminar, y las maneras de ampliar el desarrollo de Functions a otros lenguajes.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 91a6ea886c3828678771b24d69bb7987af1fb105
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83994908"
---
# <a name="supported-languages-in-azure-functions"></a>Lenguajes admitidos en Azure Functions

En este artículo se explican los niveles de soporte que se ofrecen para los lenguajes que puede usar con Azure Functions. También se describen las estrategias para crear funciones mediante lenguajes no admitidos de forma nativa.

## <a name="levels-of-support"></a>Niveles de soporte

Hay dos niveles de compatibilidad:

* **Disponibilidad general (GA)** : totalmente compatible y aprobado para su uso en producción.
* **Versión preliminar**: aún no cuenta con soporte pero se espera que llegue al estado de disponibilidad general en el futuro.

## <a name="languages-by-runtime-version"></a>Lenguaje por versión de entorno de tiempo de ejecución 

Hay disponibles [tres versiones del entorno en tiempo de ejecución de Azure Functions](functions-versions.md). En la tabla siguiente se indica qué lenguajes se admiten en cada versión del sistema de tiempo de ejecución.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

## <a name="custom-handlers-preview"></a>Controladores personalizados (versión preliminar) 

Los controladores personalizados son servidores web ligeros que reciben eventos del host de Azure Functions. Cualquier lenguaje que admita primitivas de HTTP puede implementar un controlador personalizado. Esto significa que los controladores personalizados se pueden usar para crear funciones en lenguajes que no se admiten oficialmente. Para obtener más información, consulte [Controladores personalizados de Azure Functions (versión preliminar)](functions-custom-handlers.md).

## <a name="language-extensibility"></a>Extensibilidad de lenguaje

A partir de la versión 2.x, el entorno de tiempo de ejecución está diseñado para ofrecer [extensibilidad de lenguaje](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Los lenguajes JavaScript y Java en el entorno de ejecución 2.x están incluidos con esta extensibilidad.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo desarrollar funciones en los lenguajes admitidos, consulte los siguientes recursos:

+ [Referencia para desarrolladores de la biblioteca de clases C#](functions-dotnet-class-library.md)
+ [Referencia para desarrolladores de scripts en C#](functions-reference-csharp.md)
+ [Referencia para desarrolladores de Java](functions-reference-java.md)
+ [Referencia para desarrolladores de JavaScript](functions-reference-node.md)
+ [Referencia para desarrolladores de PowerShell](functions-reference-powershell.md)
+ [Referencia para desarrolladores de Python](functions-reference-python.md)
+ [Referencia para desarrolladores de TypeScript](functions-reference-node.md#typescript)
