---
title: Bibliotecas cliente y API REST para Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga más información acerca de las bibliotecas cliente de Azure Communication Services y las API REST.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/18/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: f26f3e6e80bf854ff4f57503aa43de2558f07a1e
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94888562"
---
# <a name="client-libraries-and-rest-apis"></a>Bibliotecas cliente y API REST

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Las funcionalidades de Azure Communication Services están organizadas conceptualmente en seis áreas. Algunas áreas tienen bibliotecas cliente de código abierto completamente. La biblioteca cliente de llamadas utiliza interfaces de red propietarias y tiene actualmente formato de código cerrado, y la biblioteca de chat incluye una dependencia de código cerrado. En el [repositorio de GitHub de Azure Communication Services](https://github.com/Azure/communication) hay publicados ejemplos y detalles técnicos adicionales de las bibliotecas cliente.

## <a name="client-libraries"></a>Bibliotecas de clientes

| Ensamblado               | Protocolos             |Código abierto vs. cerrado| Espacios de nombres                          | Funcionalidades                                                      |
| ---------------------- | --------------------- | ---|-------------------------- | --------------------------------------------------------------------------- |
| Azure Resource Manager | REST | Abrir            | Azure.ResourceManager.Communication | Aprovisiona y administra recursos de Communication Services.             |
| Comunes                 | REST | Abrir               | Azure.Communication.Common          | Proporciona tipos base para otras bibliotecas cliente. |
| Administración         | REST | Abrir               | Azure.Communication.Administration  | Administra usuarios, tokens de acceso y números de teléfono, asigna servidores STUN y TURN de acuerdo con las normativas. |
| Chat                   | REST con señalización propietaria | Se abre con el paquete de señalización de código cerrado.    | Azure.Communication.Chat            | Incorpora chat basado en texto en tiempo real a las aplicaciones.  |
| SMS                    | REST | Abrir              | Azure.Communication.SMS             | Envía y recibe mensajes SMS. |
| Llamar                | Transporte propietario | Closed |Azure.Communication.Calling         | Aprovecha la voz, el vídeo, el uso compartido de pantalla y otras capacidades de comunicación de datos en tiempo real.          |

Tenga en cuenta que las bibliotecas cliente de Azure Resource Manager, de administración y de SMS se centran en la integración del servicio y, en muchos casos, surgen problemas de seguridad si se integran estas funciones en aplicaciones de usuario final. Las bibliotecas cliente comunes y de chat son adecuadas para aplicaciones cliente y de servicio. La biblioteca cliente que llama está diseñada para las aplicaciones cliente. Una biblioteca cliente centrada en escenarios de servicio está en fase de desarrollo.

### <a name="languages-and-publishing-locations"></a>Idiomas y ubicaciones de publicación

A continuación se detallan las ubicaciones de publicación para los paquetes de biblioteca cliente individuales. 

| Área           | JavaScript | .NET | Python | Java SE | iOS | Android | Otros                          |
| -------------- | ---------- | ---- | ------ | ---- | -------------- | -------------- | ------------------------------ |
| Azure Resource Manager | -         | [NuGet](https://www.nuget.org/packages/Azure.ResourceManager.Communication)    |   [PyPi](https://pypi.org/project/azure-mgmt-communication/)    |  -  | -              | -  | [Go a través de GitHub](https://github.com/Azure/azure-sdk-for-go/releases/tag/v46.3.0) |
| Comunes         | [npm](https://www.npmjs.com/package/@azure/communication-common)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Common/)    | N/D      | [Maven](https://search.maven.org/search?q=a:azure-communication-common)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases/tag/1.0.0-beta.1)            | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-common)             | -                              |
| Administración | [npm](https://www.npmjs.com/package/@azure/communication-administration)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Administration)    | [PyPi](https://pypi.org/project/azure-communication-administration/)      | [Maven](https://search.maven.org/search?q=a:azure-communication-administration)   | -              | -              | -                            |
| Chat           | [npm](https://www.npmjs.com/package/@azure/communication-chat)        | [NuGet](https://www.nuget.org/packages/Azure.Communication.Chat)     | [PyPi](https://pypi.org/project/azure-communication-chat/)     | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases)  | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | -                              |
| SMS            | [npm](https://www.npmjs.com/package/@azure/communication-sms)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Sms)    | [PyPi](https://pypi.org/project/azure-communication-sms/)       | [Maven](https://search.maven.org/artifact/com.azure/azure-communication-sms)   | -              | -              | -                              |
| Llamar        | [npm](https://www.npmjs.com/package/@azure/communication-calling)         | -      | -      | -     | [GitHub](https://github.com/Azure/Communication/releases/tag/v1.0.0-beta.2)     | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-calling/)            | -                              |
| Documentación de referencia     | [docs](https://azure.github.io/azure-sdk-for-js/communication.html)         | [docs](https://azure.github.io/azure-sdk-for-net/communication.html)      | -      | [docs](http://azure.github.io/azure-sdk-for-java/communication.html)     | (Obj-C) ✔️     | ✔️            | -                              |
## <a name="rest-apis"></a>API de REST

Las API de Communication Services están documentadas junto con otras API REST de Azure en [docs.microsoft.com](/rest/api/azure/). Esta documentación le indicará cómo estructurar los mensajes HTTP y ofrece instrucciones para el uso de Postman. Esta documentación también se ofrece en formato Swagger en [GitHub](https://github.com/Azure/azure-rest-api-specs).

## <a name="additional-support-details"></a>Detalles adicionales de compatibilidad

### <a name="ios-and-android-support-details"></a>Detalles de compatibilidad con iOS y Android

- Las bibliotecas cliente de iOS de Communication Services tienen como destino iOS versión 13+ y Xcode 11+.
- Las bibliotecas cliente de Java para Android se dirigen al nivel de API de Android 21+ y Android Studio 4.0+.

### <a name="net-support-details"></a>Detalles de compatibilidad de .NET

A excepción de las llamadas, los paquetes de Communication Services tienen como destino .NET Standard 2.0, que es compatible con las plataformas que se enumeran a continuación.

Compatibilidad mediante .NET Framework 4.6.1
- Windows 10, 8.1, 8 y 7
- Windows Server 2012 R2, 2012 y 2008 R2 SP1

Compatibilidad mediante .NET Core 2.0:
- Windows 10 (1607+), 7 SP1+, 8.1
- Windows Server 2008 R2 SP1 y versiones posteriores
- Max OS X 10.12+
- Varias versiones o distribuciones de Linux
- UWP 10.0.16299 (RS3) septiembre 2017
- Unity 2018.1
- Mono 5.4
- Xamarin iOS 10.14
- Xamarin Mac 3.8

## <a name="api-stability-expectations"></a>Expectativas de estabilidad de API 

> [!IMPORTANT]
> En esta sección se proporcionan instrucciones sobre las API REST y las bibliotecas cliente marcadas como **estables**. Las API marcadas como versión preliminar o beta se pueden cambiar o dejar de usar **sin previo aviso**. Actualmente, Azure Communication Services se encuentra en **versión preliminar pública** y las API se marcan como tales.

En el futuro, es posible que retiremos las versiones de las bibliotecas cliente de Communication Services y que introduzcamos cambios importantes en nuestras API REST y bibliotecas cliente publicadas. Azure Communication Services *generalmente* sigue dos directivas de compatibilidad para retirar versiones de servicio:

- Se le notificará con una antelación de al menos tres años cuando sea necesario cambiar el código debido a un cambio en la interfaz de Communication Services. Todas las API REST documentadas y las API de la biblioteca cliente generalmente disfrutan de un período de al menos tres años de advertencia antes de que se retiren las interfaces.
- Se le notificará al menos un año antes de que tenga que actualizar los ensamblados de la biblioteca cliente a la versión secundaria más reciente. Estas actualizaciones necesarias no deben requerir ningún cambio en el código porque están en la misma versión principal. Esto es especialmente pertinente en el caso de las bibliotecas de llamadas y chat que tienen componentes en tiempo real que requieren con frecuencia actualizaciones de rendimiento y seguridad. Le recomendamos encarecidamente que mantenga actualizadas sus bibliotecas cliente de Communication Services.

### <a name="api-and-client-library-decommissioning-examples"></a>Ejemplos de retirada de API y biblioteca cliente

**Ha integrado la versión 24 de la API REST de SMS en la aplicación. Publicaciones de Azure Communication Services v25.**

Recibirá una advertencia 3 años antes de que estas API dejen de funcionar y sea obligatorio actualizarlas a v25. Es posible que esta actualización requiera un cambio de código.

**Ha integrado la versión v2.02 de la biblioteca cliente de llamadas en la aplicación. Publicaciones de Azure Communication Services v2.05.**

Es posible que se le pida que actualice a la versión v2.05 de la biblioteca cliente de llamadas en un plazo de 12 meses a partir de la publicación de la v2.05. Debe ser un reemplazo sencillo del artefacto sin necesidad de un cambio de código porque la v2.05 está en la versión principal v2 y no tiene cambios importantes.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte la siguiente información general sobre la biblioteca cliente:

- [Información general de la biblioteca cliente de llamadas](../concepts/voice-video-calling/calling-sdk-features.md)
- [Información general de la biblioteca cliente de chat](../concepts/chat/sdk-features.md)
- [Información general de la biblioteca cliente de SMS](../concepts/telephony-sms/sdk-features.md)

Para empezar a trabajar con Azure Communication Services:

- [Cree recursos de Azure Communication Services](../quickstarts/create-communication-resource.md).
- Genere [tokens de acceso de usuario](../quickstarts/access-tokens.md).