---
title: Excepciones de FabricClient frecuentes
description: Describe las excepciones y errores frecuentes que pueden producir las API FabricClient al realizar operaciones de administración de aplicaciones y clústeres.
author: georgewallace
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: gwallace
ms.openlocfilehash: 7f3c3e072a3a2e4f7723f84b2c70ba0d0ddb9d03
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86258836"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Excepciones y errores frecuentes cuando se trabaja con las API FabricClient
Las API [FabricClient](/dotnet/api/system.fabric.fabricclient) permiten a los administradores de clústeres y aplicaciones realizar tareas administrativas en un clúster, servicio o aplicación de Service Fabric. Por ejemplo, la implementación, actualización o eliminación de aplicaciones, la comprobación del estado de un clúster o la prueba de un servicio. Los desarrolladores de aplicaciones y los administradores de clústeres pueden usar las API FabricClient para desarrollar herramientas para la administración de aplicaciones y clústeres de Service Fabric.

Hay muchos tipos diferentes de operaciones que pueden realizarse mediante FabricClient.  Cada método puede producir excepciones para errores debido a una entrada incorrecta, errores en tiempo de ejecución o problemas de infraestructura transitorios.  Consulte la documentación de referencia de la API para buscar las excepciones que produce un método específico. Sin embargo, existen algunas excepciones que muchas API [FabricClient](/dotnet/api/system.fabric.fabricclient) diferentes pueden producir. En la tabla siguiente se muestran las excepciones que son frecuentes en las API FabricClient.

| Excepción | Se produce cuando |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](/dotnet/api/system.fabric.fabricobjectclosedexception) |El objeto [FabricClient](/dotnet/api/system.fabric.fabricclient) está en un estado cerrado. Deseche el objeto [FabricClient](/dotnet/api/system.fabric.fabricclient) que está usando y cree instancias de un nuevo objeto [FabricClient](/dotnet/api/system.fabric.fabricclient). |
| [System.TimeoutException](/dotnet/core/api/system.timeoutexception) |La operación ha agotado el tiempo de espera. [OperationTimedOut](/dotnet/api/system.fabric.fabricerrorcode) se devuelve cuando la operación tarda más que MaxOperationTimeout en completarse. |
| [System.UnauthorizedAccessException](/dotnet/core/api/system.unauthorizedaccessexception) |Se genera un error de comprobación de acceso de la operación. Se devuelve E_ACCESSDENIED. |
| [System.Fabric.FabricException](/dotnet/api/system.fabric.fabricexception) |Se generó un error en tiempo de ejecución al realizar la operación. Potencialmente, cualquier método FabricClient puede producir [FabricException](/dotnet/api/system.fabric.fabricexception); la propiedad [ErrorCode](/dotnet/api/system.fabric.fabricexception.errorcode) indica la causa exacta de la excepción. Los códigos de error se definen en la enumeración [FabricErrorCode](/dotnet/api/system.fabric.fabricerrorcode) . |
| [System.Fabric.FabricTransientException](/dotnet/api/system.fabric.fabrictransientexception) |Se genera un error en la operación debido a una condición de error transitorio de algún tipo. Por ejemplo, se puede producir un error en la operación porque no se puede obtener acceso temporalmente a un cuórum de réplicas. Las excepciones transitorias corresponden a las operaciones erróneas que se pueden reintentar. |

Algunos errores [FabricErrorCode](/dotnet/api/system.fabric.fabricerrorcode) frecuentes que pueden devolverse en [FabricException](/dotnet/api/system.fabric.fabricexception):

| Error | Condición |
| --- |:--- |
| CommunicationError |Un error de comunicación ha provocado que se produzca un error en la operación; vuelva a intentar la operación. |
| InvalidCredentialType |El tipo de credencial no es válido. |
| InvalidX509FindType |X509FindType no es válido. |
| InvalidX509StoreLocation |La ubicación del almacén X509 no es válida. |
| InvalidX509StoreName |El nombre del almacén X509 no es válido. |
| InvalidX509Thumbprint |La cadena de huella digital de certificado X509 no es válida. |
| InvalidProtectionLevel |El nivel de protección no es válido. |
| InvalidX509Store |El almacén de certificados X509 no se puede abrir. |
| InvalidSubjectName |El nombre de sujeto no es válido. |
| InvalidAllowedCommonNameList |El formato de la cadena de lista de nombres frecuente no es válido. Debe ser una lista separada por comas. |
