---
title: 'Protección de contraseña de Azure AD: Azure Active Directory'
description: Prohibición de contraseñas no seguras en una instancia de Active Directory local con la protección mediante contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b9d07099f8de996181948921330ef6744b302a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848653"
---
# <a name="enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Aplicación de Protección con contraseña de Azure AD para Windows Server Active Directory

La protección mediante contraseña de Azure AD es una característica que mejora las directivas de contraseña en una organización. La implementación local de la protección mediante contraseña usa tanto las listas de contraseñas prohibidas globales como las personalizadas que se almacenan en Azure AD. Se realizan las mismas comprobaciones locales que Azure AD hace para los cambios basados en la nube. Estas comprobaciones se realizan durante los cambios de contraseña y los escenarios de restablecimiento de contraseña.

## <a name="design-principles"></a>Principios de diseño

La protección mediante contraseña de Azure AD está diseñada con estos principios en mente:

* Los controladores de dominio nunca tienen que comunicarse directamente con Internet.
* No se abren nuevos puertos de red en los controladores de dominio.
* No se requiere ningún cambio de esquema de Active Directory. El software usa los objetos de esquema de Active Directory **container** y **serviceConnectionPoint** existentes.
* No hay ningún requisito mínimo de nivel funcional del bosque (DFL/FFL) ni de dominio de Active Directory.
* El software no crea ni requiere ninguna cuenta en los dominios de Active Directory que protege.
* Las contraseñas de usuario no cifradas nunca dejan el controlador de dominio, ya sea durante las operaciones de validación de contraseña o en cualquier otro momento.
* El software no depende de otras características de Azure AD. Por ejemplo, la sincronización de hash de contraseña de Azure AD no tiene ninguna relación y no se requiere para la protección mediante contraseña de Azure AD con la función.
* Se admite la implementación incremental, sin embargo, la directiva de contraseñas se aplicará solamente donde se instale el agente de controlador de dominio. Consulte el siguiente tema para obtener información más detallada.

## <a name="incremental-deployment"></a>Implementación incremental

La protección mediante contraseña de Azure AD admite la implementación incremental entre controladores de dominio en un dominio de Active Directory, pero es importante entender lo que esto significa realmente y cuáles son las ventajas e inconvenientes.

El software del agente controlador de dominio con protección mediante contraseña de Azure AD solo puede validar las contraseñas cuando se instala en un controlador de dominio y únicamente para los cambios de contraseña que se envían a ese controlador de dominio. No es posible controlar qué controladores de dominio eligen las máquinas cliente de Windows para procesar los cambios de contraseña del usuario. Para garantizar un comportamiento coherente y el cumplimiento de la seguridad con protección mediante contraseña universal, el software del agente controlador de dominio DEBE estar instalado en todos los controladores de dominio de un dominio.

Muchas organizaciones desearán realizar pruebas exhaustivas de protección mediante contraseña de Azure AD en un subconjunto de sus controladores de dominio antes de realizar una implementación completa. La protección mediante contraseña de Azure AD admite la implementación parcial, es decir, el software del agente controlador de dominio en un controlador de dominio determinado validará activamente las contraseñas incluso cuando los otros controladores de dominio del dominio no tengan instalado el software del agente de controlador de dominio. Las implementaciones parciales de este tipo no son seguros y su uso no se recomienda salvo para pruebas.

## <a name="architectural-diagram"></a>Diagrama de arquitectura

Es importante comprender los conceptos funcionales y de diseño subyacentes antes de implementar la protección mediante contraseña de Azure AD en un entorno de Active Directory local. En el diagrama siguiente se muestra cómo los componentes de la protección mediante contraseña de Azure AD funcionan conjuntamente:

![Funcionamiento conjunto de los componentes de Protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

* El servicio de proxy de Protección con contraseña de Azure AD se ejecuta en cualquier máquina unida a un dominio del bosque de Active Directory actual. Su principal finalidad es reenviar las solicitudes de descarga de la directiva de contraseña de los controladores de dominio a Azure AD. A continuación, devuelve las respuestas de Azure AD al controlador de dominio.
* La DLL del agente de controlador de dominio del filtro de contraseña recibe las solicitudes de validación de contraseña de usuario desde el sistema operativo. Las reenvía al servicio DC Agent que se ejecuta localmente en el controlador de dominio.
* El servicio DC Agent de la protección mediante 1contraseña recibe las solicitudes de validación de contraseña desde la DLL de filtro de contraseña del agente. Las procesa con la directiva de contraseña (disponible localmente) actual y devuelve el resultado: *válida* o *error*.

## <a name="how-password-protection-works"></a>Cómo funciona la protección mediante contraseña

Cada instancia de servicio Password Protection Proxy de Azure AD se anuncia a sí mismo a los controladores de dominio del bosque mediante la creación de un objeto **serviceConnectionPoint** en Active Directory.

Cada servicio DC Agent para la protección mediante contraseña crea también un objeto **serviceConnectionPoint** en Active Directory. Este objeto se utiliza principalmente para los informes y diagnósticos.

El servicio DC Agent es el responsable de iniciar la descarga de una directiva de contraseña desde Azure AD. El primer paso es localizar un servicio Password Protection Proxy de Azure AD consultando en el bosque los objetos **serviceConnectionPoint** del proxy. Cuando se encuentra un servicio de proxy disponible, el agente de controlador de dominio envía una solicitud de descarga de la directiva de contraseña al servicio de proxy. El servicio de proxy a su vez envía la solicitud a Azure AD. El servicio de proxy, a continuación, devuelve la respuesta al servicio DC Agent.

Una vez que el servicio DC Agent recibe una nueva directiva de contraseña de Azure AD, la almacena en una carpeta dedicada en la raíz de su recurso compartido de carpeta *sysvol* de dominio. El servicio DC Agent también supervisa esta carpeta en caso de que las directivas más recientes se repliquen desde otros servicios DC Agent en el dominio.

El servicio siempre solicita una nueva directiva al inicio del servicio. Una vez iniciado el servicio DC Agent, comprueba la antigüedad de la directiva actual disponible de forma local cada hora. Si la directiva es anterior a una hora, el agente de controlador de dominio solicita una nueva directiva de Azure AD a través del servicio de proxy, como se describió anteriormente. Si la directiva actual no es anterior a una hora, el agente de controlador de dominio sigue usando esa directiva.

Cada vez que se descarga una directiva de contraseña de protección mediante contraseña de Azure AD, esa directiva es específica de un inquilino. En otras palabras, las directivas de contraseñas son siempre una combinación de la lista global de contraseñas prohibidas de Microsoft y la lista de contraseñas prohibidas por inquilino personalizada.

El agente de controlador de dominio se comunica con el servicio de proxy por RPC a través de TCP. El servicio de proxy escucha estas llamadas en un puerto RPC dinámico o estático, según esté configurado.

El agente de controlador de dominio nunca escucha en un puerto de red disponible.

El servicio de proxy nunca llama al servicio DC Agent.

El servicio de proxy es sin estado. Nunca almacena en caché las directivas ni cualquier otro estado descargado de Azure.

El servicio DC Agent siempre usa la directiva de contraseña localmente disponible más reciente para evaluar una contraseña de usuario. Si no hay ninguna directiva de contraseña disponible en el controlador de dominio local, la contraseña se acepta automáticamente. Cuando esto ocurre, se registra un mensaje de evento para advertir al administrador.

La protección mediante contraseña de Azure AD no es un motor de aplicación de directivas en tiempo real. Puede que se produzca un retraso desde que se realiza un cambio en la configuración de la directiva de contraseñas en Azure AD hasta que llega a todos los controladores de dominio y se aplica en todos ellos.

La protección mediante contraseña de Azure AD actúa como un complemento para las directivas de contraseña de Active Directory existentes, no es un reemplazo. Esto incluye las demás DLL de filtro de contraseña de terceros que puedan estar instaladas. Active Directory siempre requiere que todos los componentes de validación de contraseñas estén de acuerdo antes de aceptar una contraseña.

## <a name="foresttenant-binding-for-password-protection"></a>Enlace de bosque\inquilino para la protección mediante contraseña

La implementación de la protección mediante contraseña de Azure AD en un bosque de Active Directory requiere el registro de dicho bosque con Azure AD. Cada servicio de proxy que se implementa también debe registrarse con Azure AD. Ambos registros de bosque y de proxy están asociados a un determinado inquilino de Azure AD que se identifica implícitamente con las credenciales utilizadas durante el registro.

El bosque de Active Directory y todos los servicios de proxy implementados dentro de un bosque deben registrarse con el mismo inquilino. No se admite hacer que un bosque de Active Directory ni cualquier servicio de proxy de ese bosque se registre con diferentes inquilinos de Azure AD. Entre los síntomas de esa implementación mal configurada se incluye la imposibilidad de descargar las directivas de contraseña.

## <a name="download"></a>Descargar

Los dos instaladores de agente requeridos para la protección mediante contraseña de Azure AD se pueden descargar del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

## <a name="next-steps"></a>Pasos siguientes
[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
