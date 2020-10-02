---
title: Descripción de las estaciones de trabajo seguras administradas por Azure con Azure Active Directory
description: Más información sobre las estaciones de trabajo seguras y administradas por Azure y su importancia.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4298518dae9afdf197321073846fa1ab9262ec0a
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90601391"
---
# <a name="understand-secure-azure-managed-workstations"></a>Descripción de las estaciones de trabajo seguras administradas por Azure

Las estaciones de trabajo seguras y aisladas son de una importancia vital para la seguridad de los roles con acceso a información confidencial como administradores, desarrolladores y operadores de servicios críticos. Si se pone en peligro la seguridad de la estación de trabajo de cliente, se pueden producir errores en muchos controles y garantías de seguridad o estos pueden dejar de ser eficaces.

Este documento explica qué necesita para la creación de una estación de trabajo segura, lo cual se conoce a menudo como estación de trabajo de acceso con privilegios (PAW). El artículo también incluye instrucciones detalladas para configurar los controles de seguridad iniciales. En esta guía se describe cómo la tecnología basada en la nube puede administrar el servicio. Se basa en funcionalidades de seguridad que se incluyeron en Windows 10RS5, Microsoft Defender Advanced Threat Protection (ATP), Azure Active Directory y Microsoft Intune.

> [!NOTE]
> En este artículo se explica el concepto de una estación de trabajo segura y su importancia. Si ya está familiarizado con el concepto y desea pasar directamente a la implementación, visite [Implementación de una estación de trabajo segura](howto-azure-managed-workstation.md).

## <a name="why-secure-workstation-access-is-important"></a>¿Por qué es importante el acceso a una estación de trabajo segura?

La rápida adopción de servicios en la nube y la posibilidad de trabajar desde cualquier lugar ha creado un nuevo método para aprovecharse de las vulnerabilidades de seguridad. Aprovechándose de las vulnerabilidades de unos controles de seguridad débiles en los dispositivos con los que trabajan los administradores, los atacantes pueden acceder a recursos con privilegios.

El mal uso de los privilegios y los ataques a la cadena de suministros se encuentran entre los cinco métodos principales que usan los atacantes para infringir la seguridad de las organizaciones. También es la segunda táctica detectada con más frecuencia en los informes de incidentes en 2018 según el [informe de amenazas de Verizon](https://enterprise.verizon.com/resources/reports/dbir/) y el de [inteligencia de seguridad](https://aka.ms/sir).

La mayoría de los atacantes siguen estas fases:

1. Reconocimiento para encontrar una vía de acceso, a menudo específica de un sector.
1. Análisis para recopilar información y para identificar la mejor manera de infiltrarse en una estación de trabajo que se percibe como de valor inferior.
1. Persistencia para buscar un medio de realizar un ataque [lateral](https://en.wikipedia.org/wiki/Network_Lateral_Movement).
1. Filtración de información confidencial.

Durante la fase de reconocimiento, los atacantes suelen infiltrarse en dispositivos que parecen de bajo riesgo o que están infravalorados. Después, usan estos dispositivos vulnerables para encontrar una oportunidad de realizar un ataque lateral y encontrar usuarios y dispositivos administrativos. Una vez que acceden a los roles de usuario con privilegios, los atacantes identifican información de alto valor y logran filtrarlos con éxito.

![Patrones de riesgo típicos](./media/concept-azure-managed-workstation/typical-timeline.png)

Este documento describe una solución que puede ayudarle a proteger los dispositivos informáticos frente a tales ataques laterales. La solución aísla la administración y los servicios de los dispositivos de productividad menos valiosos, rompiendo la cadena antes de que un atacante pueda infiltrarse en un dispositivo con acceso a los recursos confidenciales en la nube. La solución usa servicios nativos de Azure que forman parte de la pila de Microsoft 365 Enterprise:

* Intune para la administración de dispositivos y una lista segura de aplicaciones y direcciones URL
* Autopilot para la configuración, la implementación y la actualización del dispositivo
* Azure AD para administración de usuarios, acceso condicional y autenticación multifactor
* Windows 10 (versión actual) para atestación de estado de dispositivo y experiencia de usuario
* ATP de Defender para la protección, detección y respuesta del punto de conexión administrado en la nube
* Azure Active Directory Privileged Identity Management para administrar la autorización y el acceso con privilegios Just-In-Time (JIT) a los recursos
* Log Analytics y Sentinel para supervisión y alertas

## <a name="who-benefits-from-a-secure-workstation"></a>¿Quién se beneficia de una estación de trabajo segura?

Todos los usuarios y operadores se benefician cuando se usa una estación de trabajo segura. Un atacante que pone en peligro un equipo o dispositivo puede suplantar todas las cuentas almacenadas en caché. Una vez iniciada sesión en el dispositivo, también puede usar las credenciales y tokens. Este riesgo hace que sea importante proteger los dispositivos que se usan para los roles con privilegios, que incluyen derechos administrativos. Los dispositivos con cuentas con privilegios suelen ser el objetivo de los ataques laterales o los ataques de elevación de privilegios. Estas cuentas se pueden usar para una variedad de recursos, como:

* Administrador de sistemas locales o basados en la nube
* Estación de trabajo de desarrollador para sistemas críticos
* Administrador de cuentas en redes sociales con alta exposición
* Estación de trabajo altamente confidencial como un terminal de pago SWIFT
* Estación de trabajo que controla secretos comerciales

Para reducir el riesgo, debe implementar controles de seguridad elevados en las estaciones de trabajo con privilegios elevados que utilizan estas cuentas. Para más información, consulte [Guía de implementación de características de Azure Active Directory](../fundamentals/active-directory-deployment-checklist-p2.md), [Hoja de ruta de Microsoft 365](https://aka.ms/o365secroadmap) y [Hoja de ruta de la protección del acceso con privilegios](https://aka.ms/sparoadmap)).

## <a name="why-use-dedicated-workstations"></a>¿Por qué usar estaciones de trabajo dedicadas?

Aunque es posible agregar seguridad a un dispositivo ya existente, es mejor empezar con una base segura. Para situar a su organización en la mejor posición para mantener un elevado nivel de seguridad, empiece por un dispositivo que sepa que es seguro e implemente un conjunto conocido de controles de seguridad.

El número creciente de vectores de ataque a través de correo electrónico y exploración web hace cada vez más difícil asegurar que un dispositivo es de confianza. En esta guía se supone que una estación de trabajo dedicada está aislada de las operaciones estándar de producción, exploración y correo electrónico. La eliminación de la productividad, la exploración web y el correo electrónico de un dispositivo puede tener un impacto negativo en la productividad. Sin embargo, esta medida de seguridad suele ser aceptable en escenarios en los que las tareas del trabajo no requieren estas operaciones explícitamente y el riesgo de un incidente de seguridad es alto.

> [!NOTE]
> La exploración web aquí hace referencia a un acceso general a sitios web arbitrarios que pueden constituir una actividad de alto riesgo. Este tipo de exploración es significativamente distinto al uso de un explorador web para acceder a un pequeño número de sitios web administrativos conocidos para servicios como Azure, Microsoft 365, otros proveedores en la nube y aplicaciones SaaS.

Las estrategias de contención refuerzan la seguridad aumentando el número y tipo de controles que impiden a un atacante acceder a recursos confidenciales. El modelo descrito en este artículo utiliza un diseño de privilegios con niveles y restringe los privilegios de administración a dispositivos concretos.

## <a name="supply-chain-management"></a>Administración de la cadena de suministro

Para tener una estación de trabajo protegida es fundamental una solución de cadena de suministro en la que use una estación de trabajo de confianza denominada "raíz de confianza". La tecnología que debe tenerse en cuenta en la selección de la raíz del hardware de confianza debe contener las siguientes tecnologías incluidas en los equipos portátiles modernos: 

* [Módulo de plataforma segura (TPM) 2.0](/windows-hardware/design/device-experiences/oem-tpm):
* [Cifrado de unidad BitLocker](/windows-hardware/design/device-experiences/oem-bitlocker)
* [Arranque seguro UEFI](/windows-hardware/design/device-experiences/oem-secure-boot)
* [Controladores y firmware distribuidos mediante Windows Update](/windows-hardware/drivers/dashboard/understanding-windows-update-automatic-and-optional-rules-for-driver-distribution)
* [Virtualización y HVCI habilitados](/windows-hardware/design/device-experiences/oem-vbs)
* [Controladores y aplicaciones HVCI-Ready](/windows-hardware/test/hlk/testref/driver-compatibility-with-device-guard)
* [Windows Hello](/windows-hardware/design/device-experiences/windows-hello-biometric-requirements)
* [Protección de E/S de DMA](/windows/security/information-protection/kernel-dma-protection-for-thunderbolt)
* [Iniciar protección del sistema](/windows/security/threat-protection/windows-defender-system-guard/system-guard-how-hardware-based-root-of-trust-helps-protect-windows)
* [En espera moderna](/windows-hardware/design/device-experiences/modern-standby)

En esta solución, la raíz de confianza se implementará con la tecnología [Microsoft Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) con hardware que cumpla los requisitos técnicos modernos. Para proteger una estación de trabajo, Autopilot le permite aprovechar los dispositivos de Windows 10 optimizados para fabricantes de equipos originales de Microsoft. Estos dispositivos vienen en un buen estado conocido de fábrica. En lugar de restablecer la imagen inicial de un dispositivo potencialmente inseguro, Autopilot puede transformar un dispositivo de Windows en un dispositivo con un estado "preparado para la empresa". Autopilot aplica valores y directivas, instala aplicaciones e incluso cambia la edición de Windows 10. Por ejemplo, Autopilot podría cambiar la instalación de Windows de un dispositivo de Windows 10 Pro a Windows 10 Enterprise para que pueda usar características avanzadas.

![Niveles de una estación de trabajo segura](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Roles y perfiles de dispositivos

Esta guía hace referencia a varios perfiles y roles de seguridad que pueden ayudarle a crear soluciones más seguras para los usuarios, desarrolladores y personal de TI. Estos perfiles suponen un equilibrio entre la facilidad de uso y los riesgos para los usuarios comunes que pueden beneficiarse de una estación de trabajo mejorada o segura. Los valores de configuración que se proporcionan aquí se basan en estándares aceptados del sector. Esta guía muestra cómo reforzar Windows 10 y reducir los riesgos asociados con dispositivos o usuarios comprometidos. Para aprovechar las ventajas de la tecnología de hardware moderna y la raíz del dispositivo de confianza, usaremos [Atestación de estado de dispositivo](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Support-Tip-Using-Device-Health-Attestation-Settings-as-Part-of/ba-p/282643), que se habilita a partir del perfil de **Alta seguridad**. Esta funcionalidad está presente para garantizar que los atacantes no puedan ser persistentes durante el arranque inicial de un dispositivo. Lo hace mediante el uso de directivas y tecnología para ayudarle a administrar las características de seguridad y los riesgos.
![Niveles de una estación de trabajo segura](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Seguridad básica**: una estación de trabajo administrada estándar proporciona un buen punto de partida para el uso doméstico o el de pequeñas empresas. Estos dispositivos se registran en Azure AD y se administran con Intune. Este perfil permite a los usuarios ejecutar todas las aplicaciones y navegar por todos los sitios web. Se debería habilitar una solución antimalware como [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security).

* **Seguridad mejorada**: esta solución protegida, de nivel inicial es adecuada para usuarios domésticos, de pequeñas empresas y desarrolladores en general.

   Una estación de trabajo mejorada basada en directivas es una forma de aumentar la seguridad de un perfil de seguridad bajo. Proporciona una forma segura de trabajar con los datos del cliente a la vez que usa herramientas de productividad como el correo electrónico o la exploración web. Puede usar directivas de auditoría e Intune para supervisar el comportamiento de los usuarios y el uso de perfiles de una estación de trabajo mejorada. Puede implementar el perfil de la estación de trabajo mejorada con el script de Windows 10 (1809) y este aprovecha la protección de malware avanzada mediante [Advanced Threat Protection (ATP)](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection).

* **Seguridad alta**: la manera más eficaz de reducir la superficie de ataque de una estación de trabajo es suprimir la posibilidad de autoadministrarla. La eliminación de los derechos administrativos locales es un paso que mejora la seguridad, pero puede afectar a la productividad si se implementa de forma incorrecta. El perfil de alta seguridad se basa en el perfil de seguridad mejorada con una diferencia importante: la eliminación del administrador local. Este perfil está diseñado para usuarios de alto perfil: ejecutivos, usuarios de datos sobre nóminas e información confidencial, y aprobadores de servicios y procesos.

   El usuario de seguridad alta exige un entorno más controlado pero que, al mismo tiempo, le permite realizar actividades como trabajar con el correo electrónico o la exploración web con una experiencia sencilla de usar. Los usuarios esperan que funcionen características como las cookies, los favoritos y otros métodos abreviados. Sin embargo, es posible que estos usuarios no necesiten modificar o depurar sus dispositivos. Tampoco deberán instalar controladores. El perfil de seguridad alta se implementa mediante el script de seguridad alta de Windows 10 (1809).

* **Especializada**: los atacantes tienen como objetivo a los desarrolladores y administradores de TI ya que estos pueden alterar los sistemas de interés para los atacantes. La estación de trabajo especializada expande las directivas de la estación de trabajo de seguridad alta mediante la administración de las aplicaciones locales y la limitación de los sitios web. También restringe las funcionalidades de productividad de alto riesgo, como ActiveX, Java, complementos de explorador y otros controles de Windows. Puede implementar este perfil con el script DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.

* **Protegida**: un atacante que pone en peligro una cuenta administrativa puede causar daños importantes en el negocio por el robo o alteración de los datos, o la interrupción del servicio. En este estado protegido, la estación de trabajo habilita todos los controles y directivas de seguridad que restringen el control directo de la administración local de aplicaciones. Una estación de trabajo protegida no tiene ninguna herramienta de productividad, por lo que es más difícil poner en peligro este dispositivo. Este método bloquea el vector de ataque más común en ataques de suplantación de identidad: el correo electrónico y las redes sociales. La estación de trabajo protegida se puede implementar con el script Secure Workstation - Windows10 (1809) SecurityBaseline.

   ![Estación de trabajo protegida](./media/concept-azure-managed-workstation/secure-workstation.png)

   Una estación de trabajo protegida proporciona a un administrador una estación de trabajo protegida que tiene un control y una protección de las aplicaciones claros. La estación de trabajo usa la protección de credenciales, dispositivos y frente a vulnerabilidades para proteger al host de comportamientos malintencionados. Todos los discos locales también se cifran con BitLocker.

* **Aislada**: este escenario personalizado, sin conexión, representa el extremo final de la gama. No se proporcionan scripts de instalación en este caso. Puede que tenga que administrar una función crítica para la empresa que requiera un sistema operativo heredado no admitido o no revisado. Por ejemplo, una línea de producción de gran valor o un sistema de soporte vital. Dado que la seguridad es fundamental y los servicios en la nube no están disponibles, puede administrar y actualizar estos equipos manualmente o con una arquitectura aislada de bosque de Active Directory como el entorno de administración de seguridad mejorada (ESAE). En estas circunstancias, considere la posibilidad de eliminar todos los accesos excepto los de las comprobaciones de estado básicas de Intune y ATP.

   * [Requisitos de comunicaciones de red de Intune](/intune/network-bandwidth-use)
   * [Requisitos de comunicaciones de red de ATP](/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Pasos siguientes

[Implementación de una estación de trabajo segura administrada por Azure](howto-azure-managed-workstation.md)
