---
title: Introducción a Azure Functions
description: Realice los primeros pasos para trabajar con Azure Functions.
author: craigshoemaker
ms.topic: overview
ms.date: 11/19/2020
ms.author: cshoe
zone_pivot_groups: programming-languages-set-functions-lang-workers
ms.openlocfilehash: 6aba58dad9853714bf26442592f74fc77e39765c
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94975017"
---
# <a name="getting-started-with-azure-functions"></a>Introducción a Azure Functions

## <a name="introduction"></a>Introducción

[Azure Functions](./functions-overview.md) permite implementar la lógica del sistema en bloques de código fácilmente disponibles. Estos bloques de código se denominan "funciones".

Usa los siguientes recursos para empezar.

::: zone pivot="programming-language-csharp"

| Acción | Recursos |
| --- | --- |
| **Crear su primera función** | Use una de las siguientes herramientas:<br><br><li>[Visual Studio Code](./functions-create-your-first-function-visual-studio.md)<li>[Visual Studio](./create-first-function-vs-code-csharp.md)<li>[Línea de comandos](./create-first-function-cli-csharp.md) |
| **Ver una función en ejecución** | <li>[Explorador de ejemplos de Azure](https://docs.microsoft.com/samples/browse/?languages=csharp&expanded=azure&products=azure-functions)<li>[Biblioteca de la comunidad de Azure](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=C%23) |
| **Explorar un tutorial interactivo**| <li>[Elección de la mejor tecnología sin servidor de Azure para su escenario empresarial](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[Marco de buena arquitectura: eficacia del rendimiento](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Ejecución de una función de Azure con desencadenadores](https://docs.microsoft.com/learn/modules/execute-azure-function-with-triggers/) <br><br>Consulte Microsoft Learn para obtener una [lista completa de tutoriales interactivos](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions).|
| **Profundizar en sus conocimientos** | <li>Aprender cómo las funciones [aumentan o disminuyen automáticamente](./functions-scale.md) las instancias para ajustarse a la demanda<li>Explorar los distintos [métodos de implementación](./functions-deployment-technologies.md) disponibles<li>Usar [herramientas de supervisión](./functions-monitoring.md) integradas que ayudan a analizar funciones<li>Consultar la [referencia del lenguaje C#](./functions-dotnet-class-library.md)|

::: zone-end

::: zone pivot="programming-language-java"
| Acción | Recursos |
| --- | --- |
| **Crear su primera función** | Use una de las siguientes herramientas:<br><br><li>[Visual Studio Code](./create-first-function-vs-code-java.md)<li>[Función de Java/Maven con el símbolo del sistema o terminal](./create-first-function-cli-java.md)<li>[Gradle](./functions-create-first-java-gradle.md)<li>[Eclipse](./functions-create-maven-eclipse.md)<li>[IntelliJ IDEA](./functions-create-maven-intellij.md) |
| **Ver una función en ejecución** | <li>[Explorador de ejemplos de Azure](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=java)<li>[Biblioteca de la comunidad de Azure](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=Java) |
| **Explorar un tutorial interactivo**| <li>[Elección de la mejor tecnología sin servidor de Azure para su escenario empresarial](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[Marco de buena arquitectura: eficacia del rendimiento](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Desarrollo de una aplicación con el complemento de Maven para Azure Functions](https://docs.microsoft.com/learn/modules/develop-azure-functions-app-with-maven-plugin/) <br><br>Consulte Microsoft Learn para obtener una [lista completa de tutoriales interactivos](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions).|
| **Profundizar en sus conocimientos** | <li>Aprender cómo las funciones [aumentan o disminuyen automáticamente](./functions-scale.md) las instancias para ajustarse a la demanda<li>Explorar los distintos [métodos de implementación](./functions-deployment-technologies.md) disponibles<li>Usar [herramientas de supervisión](./functions-monitoring.md) integradas que ayudan a analizar funciones<li>Consultar la [referencia del lenguaje Java](./functions-reference-java.md)|
::: zone-end

::: zone pivot="programming-language-javascript"
| Acción | Recursos |
| --- | --- |
| **Crear su primera función** | Use una de las siguientes herramientas:<br><br><li>[Visual Studio Code](./create-first-function-vs-code-node.md)<li>[Terminal y símbolo del sistema de Node.js](./create-first-function-cli-java.md) |
| **Ver una función en ejecución** | <li>[Explorador de ejemplos de Azure](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=javascript%2Ctypescript)<li>[Biblioteca de la comunidad de Azure](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=JavaScript%2CTypeScript) |
| **Explorar un tutorial interactivo** | <li>[Elección de la mejor tecnología sin servidor de Azure para su escenario empresarial](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[Marco de buena arquitectura: eficacia del rendimiento](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Compilación de API sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/build-api-azure-functions/)<li>[Creación de lógica sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/create-serverless-logic-with-azure-functions/)<li>[Refactorización de las API de Node.js y Express a API sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/shift-nodejs-express-apis-serverless/) <br><br>Consulte Microsoft Learn para obtener una [lista completa de tutoriales interactivos](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions).|
| **Profundizar en sus conocimientos** | <li>Aprender cómo las funciones [aumentan o disminuyen automáticamente](./functions-scale.md) las instancias para ajustarse a la demanda<li>Explorar los distintos [métodos de implementación](./functions-deployment-technologies.md) disponibles<li>Usar [herramientas de supervisión](./functions-monitoring.md) integradas que ayudan a analizar funciones<li>Consultar la referencia del lenguaje [JavaScript](./functions-reference-node.md) o [TypeScript](./functions-reference-node.md#typescript)|
::: zone-end

::: zone pivot="programming-language-powershell"
| Acción | Recursos |
| --- | --- |
| **Crear su primera función** | <li>Mediante [Visual Studio Code](./create-first-function-vs-code-powershell.md) |
| **Ver una función en ejecución** | <li>[Explorador de ejemplos de Azure](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=powershell)<li>[Biblioteca de la comunidad de Azure](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=PowerShell) |
| **Explorar un tutorial interactivo** | <li>[Elección de la mejor tecnología sin servidor de Azure para su escenario empresarial](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[Marco de buena arquitectura: eficacia del rendimiento](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Compilación de API sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/build-api-azure-functions/)<li>[Creación de lógica sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/create-serverless-logic-with-azure-functions/)<li>[Ejecución de una función de Azure con desencadenadores](https://docs.microsoft.com/learn/modules/execute-azure-function-with-triggers/) <br><br>Consulte Microsoft Learn para obtener una [lista completa de tutoriales interactivos](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions).|
| **Profundizar en sus conocimientos** | <li>Aprender cómo las funciones [aumentan o disminuyen automáticamente](./functions-scale.md) las instancias para ajustarse a la demanda<li>Explorar los distintos [métodos de implementación](./functions-deployment-technologies.md) disponibles<li>Usar [herramientas de supervisión](./functions-monitoring.md) integradas que ayudan a analizar funciones<li>Consultar la [referencia del lenguaje PowerShell](./functions-reference-powershell.md)|
::: zone-end

::: zone pivot="programming-language-python"
| Acción | Recursos |
| --- | --- |
| **Crear su primera función** | Use una de las siguientes herramientas:<br><br><li>[Visual Studio Code](./functions-create-first-function-vs-code.md?pivots=programming-language-python)<li>[Terminal y símbolo del sistema](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-python) |
| **Ver una función en ejecución** | <li>[Explorador de ejemplos de Azure](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=python)<li>[Biblioteca de la comunidad de Azure](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=Python) |
| **Explorar un tutorial interactivo** | <li>[Elección de la mejor tecnología sin servidor de Azure para su escenario empresarial](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[Marco de buena arquitectura: eficacia del rendimiento](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Compilación de API sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/build-api-azure-functions/)<li>[Creación de lógica sin servidor con Azure Functions](https://docs.microsoft.com/learn/modules/create-serverless-logic-with-azure-functions/) <br><br>Consulte Microsoft Learn para obtener una [lista completa de tutoriales interactivos](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions).|
| **Profundizar en sus conocimientos** | <li>Aprender cómo las funciones [aumentan o disminuyen automáticamente](./functions-scale.md) las instancias para ajustarse a la demanda<li>Explorar los distintos [métodos de implementación](./functions-deployment-technologies.md) disponibles<li>Usar [herramientas de supervisión](./functions-monitoring.md) integradas que ayudan a analizar funciones<li>Consultar la [referencia del lenguaje Python](./functions-reference-python.md)|
::: zone-end

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre la anatomía de una aplicación de Azure Functions](./functions-reference.md)
