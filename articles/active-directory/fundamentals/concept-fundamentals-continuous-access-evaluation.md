---
title: Evaluación continua de acceso en Azure AD
description: Respuesta más rápida a los cambios en el estado del usuario con la evaluación continua de acceso en Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jlu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3713901dd3dd5d17c4e1ddcef529c663b68f5b43
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82112582"
---
# <a name="continuous-access-evaluation"></a>Evaluación continua de acceso

Los servicios de Microsoft, como Azure Active Directory (Azure AD) y Office 365, usan estándares y protocolos abiertos para maximizar la interoperabilidad. Uno de los más importantes es Open ID Connect (OIDC). Cuando una aplicación cliente como Outlook se conecta a un servicio como Exchange Online, las solicitudes de API se autorizan mediante tokens de acceso de OAuth 2.0. De manera predeterminada, los tokens de acceso son válidos durante una hora. Cuando expiran, el cliente se redirige de nuevo a Azure AD para actualizarlos. Esto también proporciona la oportunidad de volver a evaluar las directivas de acceso de los usuarios, por lo que podríamos optar por no actualizar el token debido a una directiva de acceso condicional, o bien porque el usuario se ha deshabilitado en el directorio. 

La expiración y actualización de los tokens es un mecanismo estándar del sector. Dicho esto, los clientes han manifestado dudas sobre el lapso de tiempo entre que cambian las condiciones de riesgo para el usuario (por ejemplo, moverse de la oficina corporativa a la cafetería local, o credenciales de usuario detectadas en el mercado negro) y se pueden aplicar directivas relacionadas con ese cambio. Aunque hemos experimentado con el enfoque directo de duraciones de tokens reducidas, hemos descubierto que pueden degradar las experiencias de usuario y la confiabilidad y no eliminan los riesgos.

La respuesta oportuna a las infracciones de las directivas o a los problemas de seguridad requiere realmente una "conversación" entre el emisor del token, como Azure AD, y el usuario de confianza, como Exchange Online. Esta conversación bidireccional nos proporciona dos funcionalidades importantes. El usuario de confianza puede advertir cuándo han cambiado las cosas, como un cliente que procede de una nueva ubicación, e indicárselo al emisor del token. También proporciona al emisor del token una manera de indicar al usuario de confianza que deje de respetar los tokens de un usuario determinado debido a que la cuenta esté en peligro, se haya deshabilitado u otros problemas. El mecanismo para esta conversación es la Evaluación continua de acceso (CAE).

Microsoft ha sido un participante pionero en la iniciativa del Protocolo de evaluación continua de acceso (CAEP), como parte del grupo de trabajo [Señales y eventos compartidos](https://openid.net/wg/sse/) en OpenID Foundation. Los proveedores de identidades y las entidades de confianza podrán aprovechar las señales y los eventos de seguridad definidos por el grupo de trabajo para volver a autorizar o finalizar el acceso. Es un trabajo fascinante y mejorará la seguridad en muchas plataformas y aplicaciones.

Dado que las ventajas de seguridad son tan buenas, vamos a lanzar una implementación inicial específica de Microsoft en paralelo a nuestro trabajo continuo dentro de los organismos de estándares. A medida que trabajamos para implementar estas capacidades de evaluación continua de acceso (CAE) en los servicios de Microsoft, hemos aprendido mucho y estamos compartiendo esta información con la comunidad de estándares. Esperamos que nuestra experiencia en la implementación pueda ayudar a afianzar un estándar del sector aún mejor y nos comprometemos a implementar ese estándar una vez ratificado, lo que permitirá que todos los servicios participantes se beneficien.

## <a name="how-does-cae-work-in-microsoft-services"></a>¿Cómo funciona CAE en los servicios de Microsoft?

Nos centramos en la implementación inicial de la evaluación continua de acceso para Exchange y Teams. Esperamos ampliar la compatibilidad con otros servicios de Microsoft en el futuro. Comenzaremos a habilitar la evaluación continua de acceso solo para los inquilinos sin directivas de acceso condicional. Usaremos nuestros aprendizajes en esta fase de CAE para informar del lanzamiento continuo de CAE.

## <a name="service-side-requirements"></a>Requisitos del servicio

La evaluación continua de acceso se implementa mediante la habilitación de servicios (proveedores de recursos) para suscribirse a eventos críticos en Azure AD de modo que dichos eventos se puedan evaluar y aplicar casi en tiempo real. Los siguientes eventos se aplicarán en este lanzamiento inicial de CAE:

- La cuenta de usuario se ha eliminado o deshabilitado.
- La contraseña de un usuario ha cambiado o se ha restablecido.
- El administrador revoca explícitamente todos los tokens de actualización de un usuario.
- Azure AD Identity Protection ha detectado un riesgo de usuario elevado.

En el futuro esperamos agregar más eventos, como los cambios de estado de dispositivo y ubicación. **Aunque nuestro objetivo es que el cumplimiento sea instantáneo, en algunos casos se puede observar una latencia de hasta 15 minutos debido al tiempo de propagación de eventos**. 

## <a name="client-side-claim-challenge"></a>Desafío de notificaciones del lado cliente

Antes de la evaluación continua de acceso, los clientes siempre intentaban reproducir el token de acceso desde su memoria caché, siempre y cuando no haya expirado. Con CAE, presentamos un nuevo caso en el que un proveedor de recursos puede rechazar un token aunque no haya expirado. Para informar a los clientes de que omitan su memoria caché aunque los tokens almacenados en caché no hayan expirado, presentamos un mecanismo denominado **desafío de notificaciones**. CAE requiere una actualización de cliente para comprender el desafío de notificaciones. La versión más reciente de las siguientes aplicaciones es compatible con el desafío de notificaciones:

- Outlook para Windows 
- Outlook iOS 
- Outlook Android 
- Outlook Mac 
- Teams para Windows
- Teams iOS 
- Teams Android 
- Teams Mac 

## <a name="token-lifetime"></a>Vigencia del token

Dado que el riesgo y la directiva se evalúan en tiempo real, los clientes que negocien sesiones con reconocimiento de evaluación continua de acceso confiarán en CAE en lugar de en las directivas de vigencia del token de acceso estático existente, lo que significa que la directiva de vigencia del token configurable ya no se respetará en los clientes compatibles con CAE que negocien las sesiones con reconocimiento de CAE.

Aumentaremos la vigencia del token de acceso a 24 horas en las sesiones de CAE. La revocación se basa en eventos críticos y en la evaluación de directivas, no en un período de tiempo arbitrario. Este cambio aumenta la estabilidad de las aplicaciones sin afectar a su postura de seguridad. 

## <a name="example-flows"></a>Flujos de ejemplo

### <a name="user-revocation-event-flow"></a>Flujo de eventos de revocación de usuario:

![Flujo de eventos de revocación de usuario](./media/concept-fundamentals-continuous-access-evaluation/user-revocation-event-flow.png)

1. Un cliente compatible con CAE presenta credenciales o un token de actualización a AAD para solicitar un token de acceso para algún recurso.
1. Se devuelve un token de acceso junto con otros artefactos al cliente.
1. El administrador [revoca explícitamente todos los tokens de actualización de un usuario](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). Se enviará un evento de revocación al proveedor de recursos desde Azure AD.
1. Se presenta un token de acceso al proveedor de recursos. El proveedor de recursos evalúa la validez del token y comprueba si hay algún evento de revocación para el usuario. El proveedor de recursos utiliza esta información para decidir si se concede acceso al recurso.
1. En este caso, el proveedor de recursos deniega el acceso y envía un desafío de notificaciones 401+ al cliente.
1. El cliente compatible con CAE comprende el desafío de notificaciones 401+. Omite las cachés y vuelve al paso 1, enviando su token de actualización junto con el desafío de notificaciones de vuelta a Azure AD. A continuación, Azure AD volverá a evaluar todas las condiciones y solicitará al usuario que vuelva a autenticarse en este caso.
 
## <a name="faqs"></a>Preguntas más frecuentes

### <a name="what-is-the-lifetime-of-my-access-token"></a>¿Cuál es la vigencia de mi token de acceso?

Si no usa clientes compatibles con CAE, la vigencia del token de acceso predeterminada seguirá siendo de una hora, a menos que haya configurado la vigencia del token de acceso con la característica en vista previa (GB) de [vigencia de token configurable (CTL)](../develop/active-directory-configurable-token-lifetimes.md).

Si usa clientes compatibles con CAE que negocien sesiones con reconocimiento de CAE, se sobrescribirá la configuración de CTL de la vigencia del token de acceso, que será de 24 horas.

### <a name="how-quick-is-enforcement"></a>¿Qué rapidez tiene el cumplimiento?

Aunque nuestro objetivo es que el cumplimiento sea instantáneo, en algunos casos se puede observar una latencia de hasta 15 minutos debido al tiempo de propagación de eventos.

### <a name="how-will-cae-work-with-sign-in-frequency"></a>¿Cómo funcionará CAE con la frecuencia de inicio de sesión?

La frecuencia de inicio de sesión se respetará con o sin CAE.

## <a name="next-steps"></a>Pasos siguientes

[Anuncio de evaluación continua de acceso](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933)
