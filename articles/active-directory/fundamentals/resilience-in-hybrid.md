---
title: Compilación de una autenticación híbrida más resistente en Azure Active Directory
description: Una guía para arquitectos y administradores de TI sobre la compilación de una infraestructura híbrida resistente.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c45b362bc37df71346fc3b635c8ae4a51f62cdc
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919138"
---
# <a name="build-resilience-in-your-hybrid-architecture"></a>Aumento de la resistencia en la arquitectura híbrida

La autenticación híbrida permite a los usuarios obtener acceso a los recursos basados en la nube con sus identidades controladas en el entorno local. Una infraestructura híbrida incluye componentes en la nube y locales.

* Los componentes en la nube incluyen Azure AD, recursos y servicios de Azure, las aplicaciones basadas en la nube de la organización y aplicaciones SaaS.

* Entre los componentes locales se incluyen las aplicaciones locales, recursos como las bases de datos SQL, y un proveedor de identidades como Windows Server Active Directory. 

> [!IMPORTANT]
> A medida que planea la resistencia en la infraestructura híbrida, es fundamental minimizar las dependencias y los únicos puntos de error. 

Microsoft ofrece tres mecanismos para la autenticación híbrida. Las opciones se muestran por orden de resistencia. Si es posible, se recomienda implementar la sincronización de hash de contraseñas.

* La [sincronización de hash de contraseñas](../hybrid/whatis-phs.md) (PHS) usa Azure AD Connect para sincronizar la identidad y un hash del hash de la contraseña en Azure AD, lo que permite a los usuarios iniciar sesión en recursos basados en la nube con su contraseña controlada en el entorno local. PHS solo tiene dependencias locales para la sincronización, no para la autenticación.

* La [autenticación de paso a través](../hybrid/how-to-connect-pta.md) (PTA) redirige a los usuarios a Azure AD para el inicio de sesión. A continuación, el nombre de usuario y la contraseña se validan con Active Directory local, a través de un agente que se implementa en la red corporativa. PTA tiene una superficie local de sus agentes de PTA de Azure AD que residen en servidores locales.

* Los clientes de [federación](../hybrid/whatis-fed.md) implementan un servicio de federación, como AD FS, y, a continuación, Azure AD valida la aserción SAML generada por el servicio de federación. La federación tiene la mayor dependencia en la infraestructura local y, por lo tanto, más puntos de error. 

   
‎Puede usar uno o varios de estos métodos en su organización. Para más información, consulte [Selección del método de autenticación adecuado para la solución de identidad híbrida de Azure AD](../hybrid/choose-ad-authn.md). Este artículo contiene un árbol de decisión que puede ayudarle a decidir la metodología.

## <a name="password-hash-synchronization"></a>Sincronización de hash de contraseña

La opción de autenticación híbrida más sencilla y resistente para Azure AD es la [sincronización de hash de contraseñas](../hybrid/whatis-phs.md), que no tiene ninguna dependencia de infraestructura de identidad local al procesar solicitudes de autenticación. Una vez que las identidades con hashes de contraseña se sincronizan con Azure AD, los usuarios pueden autenticarse en los recursos en la nube sin depender de los componentes de identidad locales. 

![Diagrama de la arquitectura de PHS](./media/resilience-in-hybrid/admin-resilience-password-hash-sync.png)

Si elige esta opción de autenticación, no se producirá ninguna interrupción cuando los componentes de identidad locales dejen de estar disponibles. La interrupción local puede producirse por muchas razones, como errores de hardware, interrupciones del suministro eléctrico, desastres naturales y ataques de malware. 

### <a name="how-do-i-implement-phs"></a>¿Cómo se implementa PHS?

Para implementar PHS, vea los siguientes recursos:

* [Implementación de la sincronización de hash de contraseñas con Azure AD Connect](../hybrid/how-to-connect-password-hash-synchronization.md)

* [Habilitación de la sincronización de hash de contraseñas](../hybrid/how-to-connect-password-hash-synchronization.md)

Si sus requisitos son tales que no puede usar PHS, use la autenticación de paso a través.

## <a name="pass-through-authentication"></a>Autenticación de paso a través

La autenticación de paso a través tiene una dependencia en los agentes de autenticación que residen localmente en los servidores. Una conexión persistente, o Service Bus, está presente entre Azure AD y los agentes de PTA locales. El firewall, los servidores que hospedan los agentes de autenticación y Windows Server Active Directory local (u otro proveedor de identidades) son posibles puntos de error. 

![Diagrama de la arquitectura de PTA](./media/resilience-in-hybrid/admin-resilience-pass-through-authentication.png)

### <a name="how-do-i-implement-pta"></a>¿Cómo se implementa PTA?

Para implementar la autenticación de paso a través, vea los siguientes recursos.

* [Funcionamiento de la autenticación de paso a través](../hybrid/how-to-connect-pta-how-it-works.md)

* [Análisis detallado de la seguridad de la autenticación de paso a través](../hybrid/how-to-connect-pta-security-deep-dive.md)

* [Instalación de la autenticación de paso a través de Azure AD](../hybrid/how-to-connect-pta-quick-start.md)

* Si usa PTA, defina una [topología de alta disponibilidad](../hybrid/how-to-connect-pta-quick-start.md).

 ## <a name="federation"></a>Federación

La federación implica la creación de una relación de confianza entre Azure AD y el servicio de federación, que incluye el intercambio de puntos de conexión, certificados de firma de tokens y otros metadatos. Cuando llega una solicitud a Azure AD, lee la configuración y redirige al usuario a los puntos de conexión configurados. En ese momento, el usuario interactúa con el servicio de federación, que emite una aserción SAML validada por Azure AD. 

En el diagrama siguiente, se muestra una topología de los Servicios de federación de Active Directory (AD FS) de una empresa, una implementación que incluye servidores de federación redundante y proxy de aplicación web en varios centros de datos locales. Esta configuración se basa en componentes de infraestructura de red de empresa, como DNS, equilibrio de carga de red con capacidades de afinidad geográfica, firewalls, etc. Todos los componentes y las conexiones locales son susceptibles a errores. Para obtener más información, visite la [documentación de planeamiento de capacidad de AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/design/planning-for-ad-fs-server-capacity).

> [!NOTE]
>  La federación tiene el mayor número de dependencias locales y, por lo tanto, los puntos de error más potenciales. Aunque en este diagrama se muestra AD FS, otros proveedores de identidades locales están sujetos a consideraciones de diseño similares para lograr alta disponibilidad, escalabilidad y conmutación por error.

![Diagrama de la arquitectura de la federación](./media/resilience-in-hybrid/admin-resilience-federation.png)

 ### <a name="how-do-i-implement-federation"></a>¿Cómo se implementa la federación?

Si va a implementar una estrategia de autenticación federada o quiere que sea más resistente, consulte los siguientes recursos.

* [En qué consiste la autenticación federada](../hybrid/whatis-fed.md)

* [Cómo funciona la federación](../hybrid/how-to-connect-fed-whatis.md)

* [Lista de compatibilidad de federación de AD Azure](../hybrid/how-to-connect-fed-compatibility.md)

* Seguimiento de la [documentación de planeamiento de capacidad de AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/design/planning-for-ad-fs-server-capacity)

* [Implementación de AD FS en Azure IaaS](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs)

* [Habilitación de PHS](../hybrid/tutorial-phs-backup.md) junto con la federación

## <a name="next-steps"></a>Pasos siguientes
Recursos de resistencia para administradores y arquitectos
 
* [Aumento de la resistencia con la administración de credenciales](resilience-in-credentials.md)

* [Aumento de la resistencia con estados de dispositivos](resilience-with-device-states.md)

* [Aumento de la resistencia mediante la evaluación continua de acceso (CAE)](resilience-with-continuous-access-evaluation.md)

* [Aumento de la resistencia en la autenticación de usuario externo](resilience-b2b-authentication.md)

* [Aumento de la resistencia en el acceso a la aplicación con Application Proxy](resilience-on-premises-access.md)

Recursos de resistencia para desarrolladores

* [Aumento de la resistencia IAM en las aplicaciones](resilience-app-development-overview.md)

* [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
