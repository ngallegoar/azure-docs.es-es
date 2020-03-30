---
title: Actualización a la versión 2 del SDK de administración de .NET para Azure Search
titleSuffix: Azure Cognitive Search
description: Actualización a la versión 2 del SDK de administración de Azure Search para .NET a partir de versiones anteriores. Obtenga información sobre las novedades y los cambios de código necesarios.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b18e9688141ee64eb7dfcb82ce58db198e324b5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847530"
---
# <a name="upgrading-versions-of-the-azure-search-net-management-sdk"></a>Actualización de las versiones del SDK de administración de .NET para Azure Search

> [!Important]
> Este contenido todavía está en construcción. La versión 3.0 del SDK de administración de Azure Search para .NET está disponible en NuGet. Estamos trabajando en la actualización de esta guía de migración para explicar cómo actualizar a la nueva versión. 
>

Si usa la versión 1.0.2 o posterior del [SDK de administración de Azure Search para .NET](https://aka.ms/search-mgmt-sdk), en este artículo se le ayudará a actualizar la aplicación para que use la versión 2.

La versión 2 del SDK de administración de Azure Search para .NET contiene algunos cambios con respecto a las versiones anteriores. La mayoría son de menor importancia, por lo que cambiar el código solo precisará del mínimo esfuerzo. Vea [Pasos para actualizar](#UpgradeSteps) , a fin de obtener instrucciones sobre cómo cambiar el código para usar la nueva versión del SDK.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Novedades de la versión 2
La versión 2 del SDK de administración de Azure Search para .NET tiene como destino la misma versión disponible con carácter general de la API de REST de administración de Azure Search que las versiones anteriores del SDK, específicamente 2015-08-19. Los cambios realizados en el SDK son estrictamente cambios del lado cliente para mejorar la facilidad de uso del SDK mismo. Estos cambios incluyen los siguientes:

* `Services.CreateOrUpdate` y sus versiones asincrónicas ahora sondean automáticamente el `SearchService` de aprovisionamiento y no se devuelven hasta que el aprovisionamiento de servicio se completa. Esto evita que tenga que escribir el código de sondeo.
* Si de todos modos desea sondear manualmente el aprovisionamiento de servicio, puede usar el método `Services.BeginCreateOrUpdate` nuevo o una de sus versiones asincrónicas.
* Se agregaron los métodos `Services.Update` nuevos y sus versiones asincrónicas al SDK. Estos métodos usan HTTP PATCH para admitir la actualización incremental de un servicio. Por ejemplo, ahora puede escalar un servicio si pasa a esos métodos una instancia `SearchService` que solo contiene las propiedades `partitionCount` y `replicaCount` deseadas. La forma anterior de llamar a `Services.Get`, modificando el elemento `SearchService` devuelto y pasándolo a `Services.CreateOrUpdate`, se sigue admitiendo, pero ya no es necesaria. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Pasos para actualizar
En primer lugar, actualice la referencia de NuGet para `Microsoft.Azure.Management.Search` mediante la Consola del Administrador de paquetes NuGet, o bien haga clic con el botón derecho en las referencias del proyecto y seleccione "Administrar paquetes NuGet" en Visual Studio.

Una vez que NuGet ha descargado los nuevos paquetes y sus dependencias, recompile el proyecto. Dependiendo de cómo se estructura el código, se podrá volver a compilar correctamente. Si lo consigue, ya estará listo para empezar.

Si se produce un error en la compilación, podría deberse a que implementó algunas de las interfaces del SDK (por ejemplo, para las pruebas unitarias), las que han cambiado. Para resolver esto, deberá implementar los métodos nuevos como `BeginCreateOrUpdateWithHttpMessagesAsync`.

Una vez que haya solucionado los errores de compilación, puede realizar cambios a la aplicación para aprovechar las ventajas de la nueva funcionalidad, si lo desea. Las nuevas características en el SDK se detallan en [Novedades de la versión 2](#WhatsNew).

## <a name="next-steps"></a>Pasos siguientes
Agradecemos sus comentarios sobre el SDK. Si tiene problemas, envíe sus preguntas a [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-cognitive-search?tab=Newest). Si encuentra un error, puede enviarlo al [repositorio de GitHub del SDK de .NET para Azure](https://github.com/Azure/azure-sdk-for-net/issues). Asegúrese de que etiqueta el título de la incidencia con "[search]".
