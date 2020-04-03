---
title: Puntuación de seguridad de Azure Security Center
description: Descripción de Puntuación de seguridad de Azure Security Center y sus controles de seguridad
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2020
ms.author: memildin
ms.openlocfilehash: d7eea9cd83e72b6ffeaae319a8e87c065015e6b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415842"
---
# <a name="enhanced-secure-score-preview-in-azure-security-center"></a>Puntuación de seguridad mejorada (versión preliminar) en Azure Security Center

## <a name="introduction-to-secure-score"></a>Introducción a Puntuación de seguridad

Azure Security Center tiene dos objetivos principales: ayudarle a entender la situación de su seguridad actual y a mejorar esta de forma eficaz. La característica principal de Security Center que le ayuda a conseguir estos objetivos es Puntuación de seguridad.

Security Center evalúa continuamente los recursos, suscripciones y la organización en busca de problemas de seguridad. A continuación, agrega todos los resultados a una sola puntuación para que pueda conocer de un vistazo la situación de la seguridad actual: cuanto mayor sea la puntuación, menor será el nivel de riesgo identificado. Use la puntuación para realizar un seguimiento de los esfuerzos y proyectos para mejorar la seguridad de la organización. 

La página Puntuación de seguridad de Security Center incluye lo siguiente:

- **La puntuación**: la puntuación de seguridad se muestra como un valor de porcentaje, pero los valores subyacentes también están claros:

    [![Puntuación de seguridad mostrada como un valor de porcentaje con los números subyacentes claros](media/secure-score-security-controls/secure-score-with-percentage.png)](media/secure-score-security-controls/secure-score-with-percentage.png#lightbox)

- **Controles de seguridad**: cada control es un grupo lógico de recomendaciones de seguridad relacionadas y refleja las superficies de ataque vulnerables. Un control es un conjunto de recomendaciones de seguridad con instrucciones que le ayudan a implementar esas recomendaciones. La puntuación solo mejora cuando corrige *todas* las recomendaciones para un solo recurso de un control.

    Para ver de inmediato cómo protege su organización cada superficie de ataque individual, revise las puntuaciones de cada control de seguridad.

    Para más información, consulte [Cálculo de la puntuación de seguridad](secure-score-security-controls.md#how-the-secure-score-is-calculated) a continuación. 


>[!TIP]
> Las versiones anteriores de Security Center otorgaban puntos en el nivel de recomendación: cuando se corregía una recomendación para un único recurso, la puntuación de seguridad mejoraba. Actualmente, la puntuación solo mejora si se corrigen *todas* las recomendaciones para un solo recurso de un control. Por tanto, la puntuación solo mejora si mejora la seguridad de un recurso.
> Aunque esta versión mejorada todavía está en versión preliminar, la experiencia de puntuación de seguridad anterior está disponible como una opción en Azure Portal. 


## <a name="locating-your-secure-score"></a>Ubicación de la puntuación de seguridad

Security Center muestra la puntuación de forma destacada: es lo primero que aparece en la página de información general. Si hace clic en la página dedicada de puntuación de seguridad, verá la puntuación desglosada por suscripción. Haga clic en una suscripción única para ver la lista detallada de las recomendaciones prioritarias y del posible impacto que su corrección tendrá en la puntuación de la suscripción.

## <a name="how-the-secure-score-is-calculated"></a>Cálculo de la puntuación de seguridad 

La contribución de cada control de seguridad en la puntuación de seguridad total aparece con claridad en la página de recomendaciones.

[![La puntuación de seguridad mejorada presenta controles de seguridad](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Para obtener todos los puntos posibles de un control de seguridad, todos sus recursos deben cumplir todas las recomendaciones de seguridad de dicho control. Por ejemplo, Security Center tiene varias recomendaciones sobre cómo proteger los puertos de administración. En el pasado, podía corregir algunas de esas recomendaciones relacionadas e interdependientes y dejar otras sin resolver, y con ello, la puntuación de seguridad mejoraba. Analizado objetivamente, era fácil argumentar que la seguridad no mejoraría realmente hasta que se resolvieran todas. Ahora, es necesario corregirlas todas para que la puntuación de seguridad mejore.

Por ejemplo, el control de seguridad denominado "Aplicar actualizaciones del sistema" tiene una puntuación máxima de seis puntos, como puede ver en la información sobre herramientas sobre el potencial valor de mejora del control:

[![Control de seguridad "Aplicar actualizaciones del sistema"](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

El valor potencial del control de seguridad "Aplicar actualizaciones del sistema" en la captura de pantalla anterior muestra "2 % (1 punto)". Esto significa que si corrige todas las recomendaciones de este control, la puntuación aumentará en un 2 % (en este caso, un punto). Por motivos de sencillez, los valores de la columna de mejora potencial de la lista de recomendaciones se redondean a números enteros. La información sobre herramientas muestra los valores exactos:

* **Puntuación máxima**: el número máximo de puntos que puede obtener al completar todas las recomendaciones de un control. La puntuación máxima de un control indica la importancia relativa de ese control. Use los valores de puntuación máxima para evaluar la prioridad de los problemas. 
* **Mejora potencial**: los puntos restantes disponibles para el usuario en el control. Para que estos puntos se agreguen a la puntuación de seguridad, corrija todas las recomendaciones del control. En el ejemplo anterior, el punto que se muestra para este control es, en realidad, 0,96 puntos.
* **Puntuación actual**: la puntuación actual de este control. Cada control contribuye a la puntuación total. En este ejemplo, el control contribuye con 5,04 puntos al total. 

### <a name="calculations---understanding-your-score"></a>Cálculos: Descripción de la puntuación

|Métrica|Fórmula y ejemplo|
|-|-|
|**Puntuación actual del control de seguridad**|<br>![Ecuación para calcular la puntuación actual de un control de seguridad](media/secure-score-security-controls/security-control-scoring-equation.png)<br><br>Cada control de seguridad individual contribuye a la puntuación de seguridad. Cada recurso afectado por una recomendación dentro del control contribuye a la puntuación actual de este. La puntuación actual de cada control es una medida del estado de los recursos que están *dentro* del control.<br>![Información sobre herramientas que muestra los valores usados al calcular la puntuación actual del control de seguridad](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>En este ejemplo, la puntuación máxima de 6 se divide entre 78, ya que esta es la suma de los recursos correctos e incorrectos.<br>6 / 78 = 0,0769<br>La multiplicación de esa cifra por el número de recursos correctos (4) da como resultado la puntuación actual:<br>0.0769 * 4 = **0.31**<br><br>|
|**Puntuación segura**<br>Suscripción única|<br>![Ecuación para calcular la puntuación de seguridad actual](media/secure-score-security-controls/secure-score-equation.png)<br><br>![Puntuación de seguridad de una suscripción única con todos los controles habilitados](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>En este ejemplo, hay una suscripción única con todos los controles de seguridad disponibles (una puntuación máxima posible de 60 puntos). La puntuación muestra 28 puntos de los 60 posibles y los 32 puntos restantes se reflejan en las cifras de "Posible aumento de puntuación" de los controles de seguridad.<br>![Lista de controles y posible aumento de puntuación](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Puntuación segura**<br>Varias suscripciones|<br>La puntuación actual de todos los recursos de todas las suscripciones se suma y el cálculo es el mismo que para una suscripción única.<br><br>Cuando se ven varias suscripciones, la puntuación de seguridad evalúa todos los recursos de todas las directivas habilitadas y agrupa su impacto combinado en la puntuación máxima de cada control de seguridad.<br>![Puntuación de seguridad de varias suscripciones con todos los controles habilitados](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>La puntuación combinada **no** es un promedio; en vez de eso, es la posición evaluada del estado de todos los recursos de todas las suscripciones.<br>También en este caso, si va a la página de recomendaciones y suma los puntos posibles disponibles, observará que esta es la diferencia entre la puntuación actual (24) y la puntuación máxima disponible (60).|
||||

## <a name="improving-your-secure-score"></a>Mejora de la puntuación de seguridad

Para mejorar la puntuación de seguridad, corrija las recomendaciones de seguridad de la lista de recomendaciones. Puede corregir cada recomendación manualmente para cada recurso o utilizar la opción **Corrección rápida** (si está disponible) para aplicar rápidamente una corrección de una recomendación a un grupo de recursos. Para más información, consulte [Corrección de recomendaciones](security-center-remediate-recommendations.md).

>[!IMPORTANT]
> Solo las recomendaciones integradas afectan a la puntuación de seguridad.

## <a name="security-controls-and-their-recommendations"></a>Controles de seguridad y sus recomendaciones

En la tabla siguiente se enumeran los controles de seguridad de Azure Security Center. Para cada control, puede ver el número máximo de puntos que puede sumar a la puntuación de seguridad si corrige *todas* las recomendaciones enumeradas en el control, para *todos* los recursos. 

> [!TIP]
> Si desea filtrar u ordenar esta lista de forma diferente, cópiela y péguela en Excel.

|Control de seguridad|Puntos máximos de puntuación de seguridad|Recomendaciones|
|----------------|:-------------------:|---------------|
|**Habilitación de MFA**|10|- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción<br>- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción<br>- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción|
|**Protección de puertos de administración**|8|- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales<br>- Las máquinas virtuales deben estar asociadas a un grupo de seguridad de red<br>- Se deben cerrar los puertos de administración en las máquinas virtuales|
|**Aplicar actualizaciones del sistema**|6|- Los problemas de estado del agente de supervisión se deben resolver en las máquinas<br>- El agente de supervisión debe instalarse en conjuntos de escalado de máquinas virtuales<br>- El agente de supervisión debe instalarse en las máquinas<br>- La versión del sistema operativo debe actualizarse para los roles de servicio en la nube<br>- Se deben instalar las actualizaciones del sistema en los conjuntos de escalado de máquinas virtuales<br>- Se deben instalar las actualizaciones del sistema en las máquinas<br>- Las máquinas deben reiniciarse para aplicar las actualizaciones del sistema<br>- Los servicios de Kubernetes deben actualizarse a una versión de Kubernetes no vulnerable<br>- Un agente de supervisión debe estar instalado en las máquinas virtuales|
|**Corrección de vulnerabilidades**|6|- La opción Advanced Data Security debe estar habilitada en los servidores SQL Server<br>- Las vulnerabilidades de las imágenes de Azure Container Registry deben corregirse.<br>- Se deben corregir las vulnerabilidades de las bases de datos SQL<br>- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades<br>- La evaluación de vulnerabilidades debe estar habilitada en las instancias administradas de SQL<br>- La evaluación de vulnerabilidades debe estar habilitada en sus servidores de SQL Server<br>- La solución de evaluación de vulnerabilidades debe instalarse en sus máquinas virtuales|
|**Habilitación del cifrado de datos en reposo**|4|- El cifrado de discos debe aplicarse en las máquinas virtuales<br>- El cifrado de datos transparente en bases de datos SQL debe estar habilitado<br>- Las variables de cuenta de Automation deben cifrarse<br>- Se debe establecer la propiedad ClusterProtectionLevel en EncryptAndSign en los clústeres de Service Fabric<br>- El protector de TDE de SQL Server debe estar cifrado con su propia clave|
|**Cifrado de los datos en tránsito**|4|- Se debe acceder a la aplicación de API App solo a través de HTTPS<br>- Acceso a Function App solo a través de HTTPS<br>- Solo se deben habilitar las conexiones seguras a Redis Cache<br>- Se debe habilitar la transferencia segura a las cuentas de almacenamiento<br>- Acceso a la aplicación web solo a través de HTTPS|
|**Administración de acceso y permisos**|4|- Debe designar un máximo de tres propietarios para la suscripción<br>- Las cuentas en desuso se deben eliminar de la suscripción (versión preliminar)<br>- Las cuentas en desuso con permisos de propietario se deben eliminar de la suscripción (versión preliminar)<br>- Las cuentas externas con permisos de propietario se deben eliminar de la suscripción (versión preliminar)<br>- Las cuentas externas con permisos de lectura se deben eliminar de la suscripción<br>- Las cuentas externas con permisos de escritura se deben eliminar de la suscripción (versión preliminar)<br>- Debe haber más de un propietario asignado a la suscripción<br>- Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes (versión preliminar)<br>- Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente|
|**Corregir configuraciones de seguridad**|4|- Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes.<br>- Las vulnerabilidades en las configuraciones de seguridad de contenedor deben corregirse<br>- Se deben corregir las vulnerabilidades en la configuración de seguridad de las máquinas<br>- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales<br>- Un agente de supervisión debe estar instalado en las máquinas virtuales<br>- El agente de supervisión debe instalarse en las máquinas<br>- El agente de supervisión debe instalarse en conjuntos de escalado de máquinas virtuales<br>- Los problemas de estado del agente de supervisión se deben resolver en las máquinas|
|**Restricción de los accesos de red no autorizados**|4|- El reenvío de IP en la máquina virtual debe estar deshabilitado<br>- Deben definirse los intervalos IP autorizados en los servicios de Kubernetes (versión preliminar)<br>- (EN DESUSO) El acceso a App Services debe estar restringido (versión preliminar)<br>- (EN DESUSO) Se deben proteger las reglas de las aplicaciones web en los grupos de seguridad de red de IaaS<br>- Las máquinas virtuales deben estar asociadas a un grupo de seguridad de red<br>- CORS no debe permitir que todos los recursos accedan a la aplicación de API<br>- CORS no debe permitir que todos los recursos accedan a Function App<br>- CORS no debe permitir que todos los recursos accedan a las aplicaciones web<br>- Se debe desactivar la depuración remota para la aplicación de API<br>- Se debe desactivar la depuración remota para Function App<br>- Se debe desactivar la depuración remota para las aplicaciones web<br>- Se debe restringir el acceso a los grupos de seguridad de red permisivos con máquinas virtuales accesibles desde Internet<br>- Se deben proteger las reglas del grupo de seguridad de red para máquinas virtuales accesibles desde Internet|
|**Aplicación de controles de aplicaciones adaptables**|3|- Se deben habilitar los controles de aplicaciones adaptables en las máquinas virtuales<br>- Un agente de supervisión debe estar instalado en las máquinas virtuales<br>- El agente de supervisión debe instalarse en las máquinas<br>- Los problemas de estado del agente de supervisión se deben resolver en las máquinas|
|**Aplicación de la clasificación de datos**|2|- Los datos confidenciales de las bases de datos SQL deben clasificarse (versión preliminar)|
|**Protección de aplicaciones contra ataques DDoS**|2|- Se debe habilitar DDoS Protection estándar|
|**Habilitar Endpoint Protection**|2|- Los errores de estado de Endpoint Protection se deben corregir en los conjuntos de escalado de máquinas virtuales<br>- Los problemas de estado de Endpoint Protection se deben resolver en las máquinas<br>- La solución Endpoint Protection se debe instalar en los conjuntos de escalado de máquinas virtuales<br>- Instale la solución Endpoint Protection en máquinas virtuales<br>- Los problemas de estado del agente de supervisión se deben resolver en las máquinas<br>- El agente de supervisión debe instalarse en conjuntos de escalado de máquinas virtuales<br>- El agente de supervisión debe instalarse en las máquinas<br>- Un agente de supervisión debe estar instalado en las máquinas virtuales<br>- Instale la solución Endpoint Protection en las máquinas|
|**Habilitar auditoría y registro**|1|- La auditoría de SQL Server debe estar habilitada<br>- Los registros de diagnóstico de App Services deben estar habilitados<br>- Los registros de diagnóstico de Azure Data Lake Store deben estar habilitados<br>- Se deben habilitar los registros de diagnóstico en Azure Stream Analytics<br>- Se deben habilitar los registros de diagnóstico en las cuentas de Batch<br>- Los registros de diagnóstico de Data Lake Analytics deben estar habilitados<br>- Los registros de diagnóstico de Event Hubs deben estar habilitados<br>- Los registros de diagnóstico de IoT Hub deben estar habilitados<br>- Los registros de diagnóstico en Key Vault deben estar habilitados<br>- Los registros de diagnóstico de Logic Apps deben estar habilitados<br>- Los registros de diagnóstico del servicio Search deben estar habilitados<br>- Los registros de diagnóstico en Service Bus deben estar habilitados<br>- Los registros de diagnóstico de los conjuntos de escalado de máquinas virtuales deben estar habilitados<br>- Las reglas de alerta de métricas deben configurarse en las cuentas de Batch<br>- La opción de configuración Auditoría de SQL debería tener configurado Action-Groups para capturar las actividades críticas<br>- Debe configurar los servidores SQL Server para que realicen una retención de la auditoría durante más de 90 días.|
|**Implementación de procedimientos recomendados de seguridad**|0|- Se debe restringir el acceso a las cuentas de almacenamiento con configuraciones de red virtual y firewall<br>- Todas las reglas de autorización, excepto RootManageSharedAccessKey, se deben eliminar del espacio de nombres de Event Hubs<br>- Se debe aprovisionar el administrador de Azure Active Directory para servidores SQL Server<br>- Se deben definir las reglas de autorización de la instancia de Event Hubs<br>- Se deben migrar las cuentas de almacenamiento a los nuevos recursos de Azure Resource Manager<br>- Se deben migrar las máquinas virtuales a los nuevos recursos de Azure Resource Manager<br>- La configuración de seguridad de datos avanzada para SQL Server debe contener una dirección de correo electrónico para recibir alertas de seguridad<br>- La seguridad avanzada de datos debe estar habilitada en las instancias administradas<br>- Todos los tipos de Advanced Threat Protection deben habilitarse en la configuración de Advanced Data Security de la instancia administrada de SQL<br>- Las notificaciones por correo electrónico para administradores y propietarios de suscripciones deben estar habilitadas en la configuración de seguridad avanzada de datos de SQL Server<br>- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de SQL Server<br>- Las subredes deben estar asociadas con un grupo de seguridad de red<br>- Todos los tipos de Advanced Threat Protection deben habilitarse en la configuración de Advanced Data Security del servidor SQL Server|
||||

## <a name="secure-score-faq"></a>Preguntas frecuentes sobre la puntuación de seguridad

### <a name="why-has-my-secure-score-gone-down"></a>¿Por qué ha dejado de funcionar mi puntuación de seguridad?
Security Center ha cambiado a una puntuación de seguridad mejorada (actualmente en estado de versión preliminar) que incluye los cambios en la forma de calcular la puntuación. Ahora, debe resolver todas las recomendaciones de un recurso para recibir puntos. Las puntuaciones también han cambiado a una escala de 0-10.

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Si solo se corrigen tres de las cuatro recomendaciones de un control de seguridad, ¿cambiará mi puntuación de seguridad?
No. No cambiará hasta que corrija todas las recomendaciones para un único recurso. Para obtener la puntuación máxima de un control, debe corregir todas las recomendaciones de todos los recursos.

### <a name="is-the-previous-experience-of-the-secure-score-still-available"></a>¿La experiencia de Puntuación de seguridad anterior todavía está disponible? 
Sí. Durante un tiempo se podrán ejecutar en paralelo para facilitar la transición. Se prevé que el modelo anterior se elimine gradualmente con el tiempo. 

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Si una recomendación no es aplicable a mí y la deshabilito en la directiva, ¿se cumplirá mi control de seguridad y se actualizará mi puntuación de seguridad?
Sí. Se recomienda deshabilitar las recomendaciones cuando no son aplicables a su entorno. Para obtener instrucciones sobre cómo deshabilitar una recomendación específica, consulte [Deshabilitar las directivas de seguridad](https://docs.microsoft.com/azure/security-center/tutorial-security-policy#disable-security-policies).

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Si un control de seguridad ofrece cero puntos a mi puntuación de seguridad, ¿debería ignorarlo?
En algunos casos verá una puntuación máxima del control mayor que cero, pero el impacto es nulo. Cuando la puntuación incremental para corregir recursos es insignificante, se redondea a cero. Aún así, no ignore estas recomendaciones ya que pueden aportarle mejoras en seguridad. La única excepción es el control de "procedimiento recomendado adicional". La corrección de estas recomendaciones no aumentará la puntuación, pero mejorará la seguridad en general.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se describe la puntuación de seguridad y los controles de seguridad que presenta. Para obtener material relacionado, consulte los siguientes artículos:

- [Más información sobre los distintos elementos de una recomendación](security-center-recommendations.md)
- [Recomendaciones de corrección](security-center-remediate-recommendations.md)