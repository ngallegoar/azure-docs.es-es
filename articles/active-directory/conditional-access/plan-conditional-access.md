---
title: Planeamiento de directivas de acceso condicional en Azure Active Directory | Microsoft Docs
description: En este artículo aprenderá a planear directivas de acceso condicional para Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1c75d5022432a9a57b30aabec4dd2c4f76f2f29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671833"
---
# <a name="how-to-plan-your-conditional-access-deployment-in-azure-active-directory"></a>Instrucciones: Planeamiento de la implementación del acceso condicional en Azure Active Directory

Planear la implementación del acceso condicional es fundamental para asegurarse de que consigue la estrategia de acceso necesaria para las aplicaciones y los recursos de su organización. La mayor parte de la fase de planeamiento de la implementación debe dedicarse al diseño de varias directivas necesarias para conceder o bloquear el acceso a los usuarios según las condiciones de su elección. En este documento se explican los pasos que debe seguir para implementar directivas de acceso condicional seguras y eficaces. Antes de comenzar, asegúrese de que comprende cómo funciona el [acceso condicional](overview.md) y cuándo se debe usar.

## <a name="what-you-should-know"></a>Qué debería saber

Piense en el acceso condicional como un marco que permite controlar el acceso a las aplicaciones y los recursos de su organización, en lugar de como una característica independiente. Por esto, el acceso condicional requiere la configuración de algunas características adicionales. Por ejemplo, puede configurar una directiva que responda a un [nivel de riesgo de inicio de sesión](../identity-protection/howto-identity-protection-configure-risk-policies.md) determinado. Sin embargo, las directivas basadas en un nivel de riesgo de inicio de sesión requieren la habilitación de [Azure Active Directory Identity Protection](../identity-protection/overview-identity-protection.md).

Si se necesitan otras características, también necesitara las licencias asociadas. Por ejemplo, si bien el acceso condicional es una característica de Azure AD Premium P1, Identity Protection requiere licencia de Azure AD Premium P2.

Hay dos tipos de directivas de acceso condicional: de base de referencia y estándar. Las [directivas de base de referencia](baseline-protection.md) son las directivas de acceso condicional predefinidas. El objetivo de estas directivas es garantizar que al menos tiene el nivel de base de referencia de seguridad habilitado. Directivas de base de referencia. Las directivas de base de referencia están disponibles en todas las versiones de Azure AD y proporcionan solo opciones de personalización limitadas. Si un escenario requiere más flexibilidad, deshabilite la directiva de base de referencia e implemente los requisitos en una directiva personalizada estándar.

En una directiva de acceso condicional estándar, puede personalizar toda la configuración para que se ajuste a sus requisitos empresariales. Las directivas estándar requieren licencia de Azure AD Premium P1.

>[!NOTE]
> Se recomienda usar el dispositivo de Azure AD basado en la directiva de acceso condicional para obtener la mejor aplicación después de la autenticación inicial del dispositivo. Esto incluye cerrar sesiones si el dispositivo está fuera de cumplimiento y del flujo de código de dispositivo.

## <a name="draft-policies"></a>Borradores de directivas

El acceso condicional de Azure Active Directory permite proteger las aplicaciones en la nube en otro nivel. En este nuevo nivel, la manera de acceder a una aplicación en la nube se basa en una evaluación dinámica de la directiva, en lugar de en una configuración de acceso estática. Con una directiva de acceso condicional se define la respuesta (**hacer esto**) a una condición de acceso (**cuando esto suceda**).

![Motivo y respuesta](./media/plan-conditional-access/10.png)

Con este modelo de planeamiento puede definir todas las directivas de acceso condicional que quiera implementar. El ejercicio de planeación:

- Le ayuda a hacer un esquema de las respuestas y condiciones de cada directiva.
- Sirve para crear un catálogo de directivas de acceso condicional bien documentado para la organización. 

Puede usar el catálogo para evaluar si su implementación de directivas refleja los requisitos empresariales de su organización. 

Use la siguiente plantilla de ejemplo para crear directivas de acceso condicional para su organización:

|Cuando *esto* ocurra:|Se debe hacer *esto* otro:|
|-|-|
|Intento de acceso:<br>- A una aplicación en la nube *<br>: por parte de usuarios y grupos*<br>Con:<br>- La condición 1 (por ejemplo, desde fuera de la red corporativa)<br>-La condición 2 (por ejemplo, las plataformas de dispositivo)|Bloquear el acceso a la aplicación|
|Intento de acceso:<br>- A una aplicación en la nube *<br>: por parte de usuarios y grupos*<br>Con:<br>- La condición 1 (por ejemplo, desde fuera de la red corporativa)<br>-La condición 2 (por ejemplo, las plataformas de dispositivo)|Conceder acceso con (AND):<br>- Requisito 1 (por ejemplo, autenticación multifactor)<br>- Requisito 2 (por ejemplo, cumplimiento de los dispositivos)|
|Intento de acceso:<br>- A una aplicación en la nube *<br>: por parte de usuarios y grupos*<br>Con:<br>- La condición 1 (por ejemplo, desde fuera de la red corporativa)<br>-La condición 2 (por ejemplo, las plataformas de dispositivo)|Conceder acceso con (OR):<br>- Requisito 1 (por ejemplo, autenticación multifactor)<br>- Requisito 2 (por ejemplo, cumplimiento de los dispositivos)|

Como mínimo, **cuando esto ocurra** define la entidad de seguridad (**quién**) que intenta acceder a una aplicación en la nube (**qué**). Si es necesario, también puede incluir el **cómo** se realiza el intento de acceso. En el acceso condicional, los elementos que definen el quién, el qué y el cómo se conocen como condiciones. Para más información, consulte [¿Qué son las condiciones en el acceso condicional de Azure Active Directory?](concept-conditional-access-conditions.md) 

Con **se debe hacer esto otro** se define la respuesta de la directiva a una condición de acceso. En la respuesta se bloquea o se concede acceso con requisitos adicionales, por ejemplo, la autenticación multifactor (MFA). Para información general completa, consulte [¿Qué son los controles de acceso en el acceso condicional de Azure Active Directory?](controls.md)  

La combinación de condiciones con controles de acceso representa una directiva de acceso condicional.

![Motivo y respuesta](./media/plan-conditional-access/51.png)

Para más información, consulte [¿Cuáles son los requisitos para realizar un trabajo de directiva?](best-practices.md#whats-required-to-make-a-policy-work)

Ahora es un buen momento para decidir sobre la pauta de nomenclatura de las directivas. La pauta de nomenclatura ayuda a encontrar las directivas y a comprender su propósito sin tener que abrirlas en el portal de administrador de Azure. Nombre de la directiva para mostrar:

- Un número de secuencia
- La aplicación en la nube a la que se refiere
- La respuesta
- A quién se aplica
- Cuando se aplica (si procede)
 
![Pauta de nomenclatura](./media/plan-conditional-access/11.png)

Mientras que un nombre descriptivo le ayuda a mantener una visión general de la implementación del acceso condicional, el número de secuencia es útil si tiene que hacer referencia a una directiva en una conversación. Por ejemplo, si habla con un compañero administrador por teléfono, puede pedirle que abra la directiva EM063 para resolver un problema.

Por ejemplo, el siguiente nombre indica que la directiva requiere autenticación multifactor para los usuarios de marketing de redes externas que usen la aplicación Dynamics CRP:

`CA01 - Dynamics CRP: Require MFA For marketing When on external networks`

Además de las directivas activas, también se recomienda implementar directivas deshabilitadas que actúen como [controles de acceso resistentes para escenarios de interrupción/emergencia](../authentication/concept-resilient-controls.md) secundarios. La pauta de nomenclatura para las directivas de contingencia debe incluir algunos elementos más: 

- `ENABLE IN EMERGENCY` al principio para resaltar el nombre entre las otras directivas.
- El nombre de la interrupción a la que se debe aplicar.
- Un número de secuencia de ordenación para ayudar al administrador a saber en qué orden se deben habilitar las directivas. 

Por ejemplo, el siguiente nombre indica que esta directiva es la primera de cuatro directivas que debe habilitar en caso de una interrupción de MFA:

`EM01 - ENABLE IN EMERGENCY, MFA Disruption[1/4] - Exchange SharePoint: Require hybrid Azure AD join For VIP users`

## <a name="plan-policies"></a>Planeación de directivas

Al planear la solución de directiva de acceso condicional, determine si necesita crear directivas para lograr los siguientes resultados. 

### <a name="block-access"></a>Bloquear acceso

La opción de bloquear el acceso resulta eficaz por lo siguiente:

- Altera el resto de asignaciones de un usuario
- Tiene la capacidad de bloquear el inicio de sesión en el inquilino a toda la organización
 
Si desea bloquear el acceso a todos los usuarios, debe excluir de la directiva a al menos un usuario (normalmente, las cuentas de acceso de emergencia). Para más información, consulte el apartado sobre la [selección de usuarios y grupos](block-legacy-authentication.md#select-users-and-cloud-apps).  

### <a name="require-mfa"></a>Requerir MFA

Para simplificar la experiencia de inicio de sesión de los usuarios, puede permitirles iniciar sesión en sus aplicaciones en la nube mediante un nombre de usuario y una contraseña. No obstante, suele haber siempre algún escenario en el que se recomienda una forma más sólida de verificar las cuentas. Con una directiva de acceso condicional se puede limitar el requisito de autenticación multifactor a determinados escenarios. 

Casos de uso comunes para exigir la autenticación multifactor para el acceso:

- [De los administradores](howto-baseline-protect-administrators.md)
- [A aplicaciones específicas](app-based-mfa.md) 
- [Desde ubicaciones de red en las que no se confía](untrusted-networks.md).

### <a name="respond-to-potentially-compromised-accounts"></a>Respuesta ante las cuentas en riesgo

Con las directivas de acceso condicional se pueden implementar respuestas automatizadas a inicios de sesión con identidades que pueden suponer un riesgo. La probabilidad de que una cuenta esté en riesgo se expresa mediante niveles. En Identity Protection se calculan dos niveles de riesgo: riesgo de inicio de sesión o de usuario. Para implementar una respuesta ante el riesgo de inicio de sesión, tiene dos opciones:

- La [condición de riesgo de inicio de sesión](concept-conditional-access-conditions.md#sign-in-risk) de la directiva de acceso condicional
- La [directiva de riesgo de inicio de sesión](../identity-protection/howto-sign-in-risk-policy.md) de identity Protection 

Definir el riesgo de inicio de sesión como condición es el método de preferencia, ya que ofrece más opciones de personalización.

En Identity Protection, el nivel de riesgo de usuario solo está disponible como [directiva de riesgo del usuario](../identity-protection/howto-user-risk-policy.md). 

Para más información, consulte [¿Qué es Azure Active Directory Identity Protection?](../identity-protection/overview.md) 

### <a name="require-managed-devices"></a>Requerir dispositivos administrados

La proliferación de dispositivos compatibles para acceder a recursos en la nube ayuda a mejorar la productividad de los usuarios. Por otro lado, es posible que no desee que se pueda acceder a determinados recursos del entorno con un nivel de protección desconocido. Para estos recursos en cuestión debe hacer que los usuarios solo puedan acceder a ellos mediante un dispositivo administrado. Para más información, consulte [Uso obligatorio de dispositivos administrados para el acceso a aplicaciones en la nube mediante el acceso condicional](require-managed-devices.md). 

### <a name="require-approved-client-apps"></a>Requerir aplicaciones cliente aprobadas

Una de las primeras decisiones que deberá tomar para los escenarios en los que se traen dispositivos propios (BYOD), es si necesita administrar todo el dispositivo o solo los datos que en él se encuentran. Los empleados usan dispositivos móviles para tareas personales y profesionales. Mientras se asegura de que los empleados pueden ser productivos, también puede evitar la pérdida de datos. Con el acceso condicional de Azure Active Directory (Azure AD), puede restringir el acceso a las aplicaciones en la nube a las aplicaciones cliente aprobadas que pueden proteger los datos corporativos. Para más información, consulte [Uso obligatorio de aplicaciones cliente aprobadas para el acceso a aplicaciones en la nube mediante el acceso condicional](app-based-conditional-access.md).

### <a name="block-legacy-authentication"></a>Bloquear la autenticación heredada

Azure AD admite varios de los protocolos de autenticación y autorización usados más comúnmente, incluida la autenticación heredada. ¿Cómo se puede evitar que las aplicaciones que usan la autenticación heredada accedan a los recursos del inquilino? La recomendación es simplemente bloquearlas con una directiva de acceso condicional. Si es necesario, puede permitir que solo ciertos usuarios y ubicaciones de red específicas usen aplicaciones basadas en la autenticación heredada. Para más información, consulte [Bloqueo de autenticación heredada a Azure AD con acceso condicional](block-legacy-authentication.md).

## <a name="test-your-policy"></a>Prueba de la directiva

Antes de implementar una directiva en producción, debe probarla para verificar que se comporta según lo previsto.

1. Creación de usuarios de prueba
1. Creación de un plan de pruebas
1. Configuración de la directiva
1. Evaluación de un inicio de sesión simulado
1. Prueba de la directiva
1. Limpieza

### <a name="create-test-users"></a>Creación de usuarios de prueba

Para probar una directiva, cree un conjunto de usuarios similar al de su entorno. La creación de usuarios de prueba permite verificar que las directivas funcionan según lo previsto antes de que afecten a usuarios reales y puedan impedir su acceso a aplicaciones y recursos. 

Algunas organizaciones tienen inquilinos de prueba para este propósito. Sin embargo, puede resultar difícil volver a crear todas las condiciones y las aplicaciones en un inquilino de prueba para probar completamente el resultado de una directiva. 

### <a name="create-a-test-plan"></a>Creación de un plan de pruebas

El plan de pruebas es importante para tener una comparación entre los resultados previstos y los reales. Antes de probar algo debe tener siempre una previsión. En la siguiente tabla se muestran casos de prueba de ejemplo. Ajuste los escenarios y los resultados previstos en función de la configuración de sus directivas de acceso condicional.

|Directiva |Escenario |Resultado previsto | Resultado |
|---|---|---|---|
|[Exigir la autenticación multifactor desde fuera de la oficina](/azure/active-directory/conditional-access/untrusted-networks)|El usuario autorizado inicia sesión en la *aplicación* desde la oficina / una ubicación de confianza|Al usuario no se le solicita autenticación multifactor| |
|[Exigir la autenticación multifactor desde fuera de la oficina](/azure/active-directory/conditional-access/untrusted-networks)|El usuario autorizado inicia sesión en la *aplicación* desde otro lugar distinto a la oficina / una ubicación de confianza|Al usuario se le solicita autenticación multifactor para iniciar sesión| |
|[Exigir autenticación multifactor (para administradores)](/azure/active-directory/conditional-access/howto-baseline-protect-administrators)|El administrador global inicia sesión en la *aplicación*|Al administrador se le solicita autenticación multifactor| |
|[Inicios de sesión no seguros](/azure/active-directory/identity-protection/howto-sign-in-risk-policy)|El usuario inicia sesión en la *aplicación* mediante el [explorador Tor](/azure/active-directory/active-directory-identityprotection-playbook)|Al administrador se le solicita autenticación multifactor| |
|[Administración de dispositivos](/azure/active-directory/conditional-access/require-managed-devices)|El usuario autorizado intenta iniciar sesión desde un dispositivo autorizado|El acceso se le concede| |
|[Administración de dispositivos](/azure/active-directory/conditional-access/require-managed-devices)|El usuario autorizado intenta iniciar sesión desde un dispositivo no autorizado|El acceso se le bloquea| |
|[Cambio de contraseña en caso de riesgo del usuario](/azure/active-directory/identity-protection/howto-user-risk-policy)|El usuario autorizado intenta iniciar sesión con credenciales en riesgo (inicio de sesión de alto riesgo)|Al usuario se le solicita que cambie la contraseña o se le bloquea el acceso (de conformidad con la directiva)| |

### <a name="configure-the-policy"></a>Configuración de la directiva

La administración de las directivas de acceso condicional es una tarea manual. En Azure Portal puede administrar las directivas de acceso condicional en una ubicación central: la página de acceso condicional. Un punto de entrada a la página de acceso condicional es la sección **Seguridad** del panel de navegación de **Active Directory**. 

![Acceso condicional](media/plan-conditional-access/03.png)

Para más información sobre cómo crear directivas de acceso condicional, consulte [Exigir MFA para aplicaciones específicas con acceso condicional de Azure Active Directory](app-based-mfa.md). Este artículo de inicio rápido le ayudará a:

- Familiarizarse con la interfaz de usuario.
- Obtener una primera idea de cómo funciona el acceso condicional. 

### <a name="evaluate-a-simulated-sign-in"></a>Evaluación de un inicio de sesión simulado

Ahora que ha configurado la directiva de acceso condicional, probablemente quiera saber si funciona según lo previsto. Como primer paso, use la [herramienta de directivas What If](what-if-tool.md) del acceso condicional para simular un inicio de sesión del usuario de prueba. La simulación calcula el impacto que este inicio de sesión tiene en las directivas y genera un informe de simulación.

>[!NOTE]
> Aunque una ejecución simulada da la idea del efecto de una directiva de acceso condicional, no reemplaza a una serie de pruebas reales.

### <a name="test-your-policy"></a>Prueba de la directiva

Ejecute casos de prueba según el plan de pruebas. En este paso se ejecuta una prueba de un extremo a otro de cada directiva con los usuarios de prueba para garantizar que la directiva se comporta correctamente. Utilice los escenarios que creó anteriormente para ejecutar cada prueba.

Es importante asegurarse de que se prueban los criterios de exclusión de las directivas. Por ejemplo, puede excluir a un usuario o grupo de una directiva que requiera MFA. Pruebe si a los usuarios excluidos se les solicita MFA, ya que la combinación de otras directivas puede hacer que se exija para esos usuarios.

### <a name="cleanup"></a>Limpieza

El proceso de limpieza consta de los siguientes pasos:

1. Deshabilite la directiva.
1. Elimine los usuarios y grupos asignados.
1. Elimine los usuarios de prueba.  

## <a name="move-to-production"></a>Paso a producción

Cuando las nuevas directivas estén listas para su entorno, impleméntelas en fases:

- Comunique los cambios internos a los usuarios finales.
- Empiece por un conjunto de usuarios pequeño y verifique que la directiva se comporta según lo previsto.
- Cuando expanda una directiva para incluir más usuarios, continúe con la exclusión de todos los administradores. La exclusión de los administradores garantiza que alguien tiene acceso a la directiva en caso de que se requieran cambios.
- Aplique una directiva a todos los usuarios solo si es necesario.

Como procedimiento recomendado, cree al menos una cuenta de usuario que esté:

- Dedicada a la administración de directivas
- Excluida de todas las directivas

## <a name="rollback-steps"></a>Pasos de reversión

En caso de que necesite revertir las directivas recién implementadas, use una o varias de las siguientes opciones para ello:

1. **Deshabilitar la directiva**: esto garantiza que la directiva no se aplica cuando un usuario intente iniciar sesión. Si quiere que se use, siempre puede volver atrás y habilitar la directiva.

   ![Deshabilitar la directiva](media/plan-conditional-access/07.png)

1. **Excluir un usuario o grupo de una directiva**: si un usuario no puede acceder a la aplicación, puede elegir excluirlo de la directiva.

   ![Excluir usuarios](media/plan-conditional-access/08.png)

   > [!NOTE]
   > Esta opción debe usarse con moderación, solo en situaciones donde el usuario sea de confianza. El usuario debe agregarse de nuevo a la directiva o grupo n cuanto sea posible.

1. **Eliminar la directiva**: si ya no es necesaria, elimínela.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [Documentación sobre el acceso condicional de Azure AD](index.yml) para obtener la información general disponible.
