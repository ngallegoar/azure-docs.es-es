---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: a33cc604d45d51a63a73ea6ed58abc67269437d7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80673120"
---
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python" 
Agregue una función al proyecto mediante el comando siguiente, donde el argumento `--name` es el nombre único de la función (HttpExample) y el argumento `--template` especifica el desencadenador de esta (HTTP). 

```
func new --name HttpExample --template "HTTP trigger"
```  
::: zone-end  
::: zone pivot="programming-language-csharp"
`func new` crea un archivo de código HttpExample.cs.
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"
`func new` crea una subcarpeta que coincide con el nombre de la función que contiene un archivo de código apropiado para el lenguaje elegido del proyecto y un archivo de configuración denominado *function.json*.
::: zone-end