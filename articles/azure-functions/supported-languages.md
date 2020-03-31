---
title: Lenguajes admitidos en Azure Functions
description: Obtenga información acerca de qué lenguajes se admiten (versión de disponibilidad general) y cuáles son experimentales o están en versión preliminar.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 029ea753439dca3093bf214a5adfb6d58a1fe567
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74942263"
---
# <a name="supported-languages-in-azure-functions"></a>Lenguajes admitidos en Azure Functions

En este artículo se explican los niveles de soporte que se ofrecen para los lenguajes que puede usar con Azure Functions.

## <a name="levels-of-support"></a>Niveles de soporte

Hay tres niveles de soporte:

* **Disponibilidad general (GA)** : totalmente compatible y aprobado para su uso en producción.
* **Versión preliminar**: aún no cuenta con soporte pero se espera que llegue al estado de disponibilidad general en el futuro.
* **Experimental**: no cuenta con soporte y podría abandonarse en el futuro; no se ofrece ninguna garantía de que alcance el estado de versión preliminar o disponibilidad general.

## <a name="languages-by-runtime-version"></a>Lenguaje por versión de entorno de tiempo de ejecución 

Hay disponibles [tres versiones del entorno en tiempo de ejecución de Azure Functions](functions-versions.md). En la tabla siguiente se indica qué lenguajes se admiten en cada versión del sistema de tiempo de ejecución.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Lenguajes experimentales

En la versión 1.x, los lenguajes experimentales no escalan bien y no admiten todos los enlaces.

No use características experimentales para nada importante, porque no se admiten oficialmente. No se deben abrir casos de soporte técnico para problemas con lenguajes experimentales. 

Las versiones posteriores de entornos en tiempo de ejecución no admiten lenguajes experimentales. La compatibilidad para nuevos idiomas solamente se agrega cuando el idioma puede admitirse en producción. 

### <a name="language-extensibility"></a>Extensibilidad de lenguaje

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
