---
title: Controles de ejemplo de plano técnico de HIPAA HITRUST
description: Asignación de los controles de los ejemplos de plano técnico de HIPAA HITRUST. Cada control se asigna a una o varias directivas de Azure que ayudan en la evaluación.
ms.date: 11/26/2019
ms.topic: sample
ms.openlocfilehash: c6680055cfe41a2f893d46cba797783913d880ce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "75470750"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>Asignación de los controles de los ejemplos de plano técnico de HIPAA HITRUST

En el siguiente artículo se ofrece información detallada sobre cómo se asignan los ejemplos de plano técnico de Azure Blueprints a los controles de HIPAA HITRUST. Para más información acerca de los controles, consulte [HIPAA HITRUST](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

Las asignaciones que se presentan a continuación son para los controles de **HIPAA HITRUST**. Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Muchos de los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la **\[versión preliminar\]: Auditar los controles HITRUST/HIPAA**, que es una iniciativa de directiva integrada.

> [!IMPORTANT]
> Cada control que se muestra a continuación está asociado a una o varias definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas pueden ayudarle a [evaluar el cumplimiento](../../../policy/how-to/get-compliance-data.md) del control; sin embargo, a menudo no hay una coincidencia 1:1 o completa entre un control y una o varias directivas. Como tal, el **cumplimiento** con Azure Policy solo se refiere a las propias directivas; esto no garantiza que sea totalmente compatible con todos los requisitos de un control. Además, el estándar de cumplimiento incluye controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los controles y las definiciones de Azure Policy para este ejemplo de plano técnico de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md).

## <a name="control-against-malicious-code"></a>Control contra código malintencionado

Este proyecto le ayuda a administrar la protección de los puntos de conexión, incluida la protección del código malintencionado, mediante la asignación de definiciones de [Azure Policy](../../../policy/overview.md) que supervisan que no falte la protección de los puntos de conexión en las máquinas virtuales en Azure Security Center y aplican la solución antimalware de Microsoft en las máquinas virtuales Windows.

- Implementación de la extensión de Microsoft IaaS Antimalware para Windows Server
- Se deben habilitar los registros de diagnóstico en las cuentas de Batch
- Se deben instalar actualizaciones del sistema en las máquinas


## <a name="management-of-removable-media"></a>Administración de medios extraíbles

La organización, según el nivel de clasificación de los datos, registra los medios (incluidos los equipos portátiles) antes de su uso, impone restricciones razonables sobre cómo utilizar dichos medios y proporciona un nivel adecuado de protección física y lógica (incluido el cifrado) para los medios que contengan información cubierta, hasta que esta se destruye o se limpia correctamente.

- Exigencia de cifrado en las cuentas de Data Lake Store.
- El protector de TDE de la instancia administrada SQL debe estar cifrado con su propia clave
- El cifrado de discos debe aplicarse en máquinas virtuales
- Se debe permitir el cifrado de datos transparente en bases de datos SQL


## <a name="information-exchange-policies-and-procedures"></a>Directivas y procedimientos de intercambio de información

Los proveedores de servicios en la nube usan una plataforma de virtualización reconocida por el sector y formatos de virtualización estándar (por ejemplo, Open Virtualization Format, OVF) con el fin de garantizar la interoperabilidad, también documentan los cambios personalizados realizados en cualquier hipervisor en uso y todos los enlaces de virtualización específicos de la solución disponibles para la revisión del cliente.

- Implementación de los requisitos previos para realizar una auditoría de las máquinas virtuales Windows que no tengan instaladas las aplicaciones especificadas

## <a name="control-of-operational-software"></a>Control del software operativo 

La organización identifica el software no autorizado en el sistema de información, incluidos servidores, estaciones de trabajo y equipos portátiles, y emplea una directiva de permitir todo, denegar por excepción para prohibir la ejecución de software no autorizado conocido en el sistema de información. Igualmente revisa y actualiza la lista de software no autorizado periódicamente, al menos una vez al año.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Auditar"
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Directivas de auditoría del sistema - Administración de cuentas"

## <a name="change-control-procedures"></a>Procedimientos de control de cambios

La integridad de todas las imágenes de máquina virtual se garantiza en todo momento mediante el registro y la generación de una alerta para cualquier cambio realizado en las imágenes de máquina virtual, y la puesta a disposición de los propietarios y/o clientes empresariales, mediante métodos electrónicos (por ejemplo, portales o alertas), de los resultados de un cambio o movimiento y de la validación posterior de la integridad de la imagen.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Directivas de auditoría del sistema - Seguimiento detallado"

## <a name="inventory-of-assets"></a>Inventario de recursos 

Se mantiene un inventario de recursos y servicios

- Los registros de diagnóstico de los servicios de búsqueda deben estar habilitados.
- \[Versión preliminar\]: Implementar los requisitos para auditar las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Servidor de red Microsoft"
- \[Versión preliminar\]: Implementar los requisitos para auditar las configuraciones de las máquinas virtuales Windows en "Plantillas administrativas - Red"

## <a name="control-of-technical-vulnerabilities"></a>Control de vulnerabilidades técnicas 

Existe un estándar de configuración protegido para todos los componentes del sistema y de la red.

- Auditoría de máquinas virtuales sin la recuperación ante desastres configurada
- La evaluación de vulnerabilidad debe estar habilitada en las instancias administradas de SQL.
- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades

## <a name="segregation-in-networks"></a>Segregación en redes

Las puertas de enlace de seguridad de la organización (por ejemplo, los firewalls) aplican las directivas de seguridad y están configuradas para filtrar el tráfico entre dominios, bloquean el acceso no autorizado y se usan para mantener la segregación entre segmentos de red cableados internos, inalámbricos internos y externos (por ejemplo, Internet), incluidas las redes perimetrales y aplican directivas de control de acceso para cada uno de los dominios.

- Aprovisionamiento automático del agente de supervisión de seguridad
- Implementar Network Watcher al crear redes virtuales.

## <a name="input-data-validation"></a>Validación de datos de entrada

En el caso de las aplicaciones web de acceso público, se implementan firewalls de nivel de aplicación para controlar el tráfico. En el caso de las aplicaciones de acceso público que no están basadas en Web, la organización ha implementado un firewall basado en la red específico para el tipo de aplicación. Si el tráfico a la aplicación de acceso público está cifrado, el dispositivo se encuentra detrás del cifrado o es capaz de descifrar el tráfico antes del análisis.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Propiedades de Firewall de Windows"


## <a name="network-connection-control"></a>Control de conexión de red

El tráfico de red se controla de acuerdo con la Directiva de control de acceso de la organización mediante el firewall y otras restricciones relacionadas con la red para cada punto de acceso a la red o la interfaz administrada del servicio de telecomunicaciones externo.

- Debe restringirse el acceso a través de puntos de conexión accesibles desde Internet
- El protector de TDE de la instancia administrada SQL debe estar cifrado con su propia clave
- Debe desactivarse la depuración remota para aplicaciones de API

## <a name="network-controls"></a>Controles de red

La organización utiliza canales de comunicación seguros y cifrados al migrar servidores físicos, aplicaciones o datos a servidores virtualizados.

- El cifrado de discos debe aplicarse a las máquinas virtuales
- El protector de TDE de SQL Server debe estar cifrado con su propia clave
- \[Versión preliminar\]: Mostrar los resultados de las auditorías de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Acceso a la red"
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento
- \[Versión preliminar\]: Mostrar los resultados de las auditorías de las configuraciones de las máquinas virtuales Windows en "Propiedades de Firewall de Windows"
- Implementación de la configuración de diagnósticos de grupos de seguridad de red
- Debe restringirse el acceso a través de puntos de conexión accesibles desde Internet

## <a name="sensitive-system-isolation"></a>Aislamiento del sistema confidencial

Los recursos compartidos del sistema (por ejemplo, registros, memoria principal, almacenamiento secundario) se liberan de nuevo en el sistema, protegidos contra la divulgación a otros sistemas, aplicaciones o usuarios, y los usuarios no pueden acceder de forma intencionada o accidental a restos de información.

- Las máquinas virtuales deben migrarse a nuevos recursos de Azure Resource Manager

## <a name="security-of-network-services"></a>Seguridad de Network Services

Los servicios acordados proporcionados por un administrador o proveedor de servicios de red se administran y supervisan formalmente para asegurarse de que se proporcionan de forma segura.

- Las máquinas virtuales deben migrarse a nuevos recursos de Azure Resource Manager

## <a name="network-routing-control"></a>Control de enrutamiento de red

Los controles de enrutamiento se implementan mediante puertas de enlace de seguridad (por ejemplo, firewalls) que se usan entre redes internas y externas (por ejemplo, Internet y redes de terceros).

- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales

## <a name="information-exchange-policies-and-procedures"></a>Directivas y procedimientos de intercambio de información

La organización restringe el uso de medios de almacenamiento portátiles controlados por la organización por parte de personas autorizadas en sistemas de información externos.

- Auditar el acceso de red sin restricciones a cuentas de almacenamiento
- Debe desactivarse la depuración remota para aplicaciones web
- Se debe acceder a la aplicación de API App solo a través de HTTPS

## <a name="electronic-messaging"></a>Mensajería electrónica

Las aprobaciones se obtienen antes del uso de servicios públicos externos, como la mensajería instantánea o el uso compartido de archivos.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las máquinas virtuales Linux que no tengan los permisos del archivo de contraseñas establecidos en 0644

## <a name="on-line-transactions"></a>Transacciones en línea

La organización requiere el uso de cifrado entre cada una de las partes implicadas en la transacción así como el uso de firmas electrónicas por parte de las mismas. La organización garantiza que el almacenamiento de los detalles de la transacción se encuentra fuera de los entornos de acceso público (por ejemplo, en una plataforma de almacenamiento que se encuentre en la intranet de la organización), y que no se conserva y expone en un medio de almacenamiento directamente accesible desde Internet. Cuando se usa una entidad de confianza (por ejemplo, para la emisión y el mantenimiento de firmas digitales y/o certificados digitales), la seguridad se integra e inserta en todo el proceso de administración de un extremo a otro del certificado o la firma.

- El cifrado de discos debe aplicarse a las máquinas virtuales
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las máquinas virtuales Windows que no contengan los certificados especificados en la raíz de confianza

## <a name="password-management"></a>Administración de contraseñas

Las contraseñas se cifran durante la transmisión y el almacenamiento en todos los componentes del sistema.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las máquinas virtuales Windows que no tengan habilitada la configuración de complejidad de la contraseña

## <a name="user-authentication-for-external-connections"></a>Autenticación de usuario para conexiones externas

Los métodos de autenticación seguros, como multi-factor, Radius o Kerberos (para el acceso con privilegios) y CHAP (para el cifrado de credenciales para métodos de acceso telefónico), se implementan para todas las conexiones externas a la red de la organización.

- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción
- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales

## <a name="user-identification-and-authentication"></a>Identificación y autenticación de usuario

Los usuarios que han realizado funciones con privilegios (por ejemplo, la administración del sistema) utilizan cuentas independientes para realizar esas funciones con privilegios. Los métodos de autenticación multifactor se usan de acuerdo con la directiva de la organización (por ejemplo, para el acceso remoto a la red).

- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción
- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales

## <a name="privilege-management"></a>Administración de privilegios

El acceso a las funciones de administración o a las consolas administrativas para sistemas que hospedan sistemas virtualizados está restringido al personal aplicando el principio de privilegios mínimos, y se admite mediante controles técnicos.

- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales
- \[Versión preliminar\]: En los servicios de Kubernetes debe usarse el control de acceso basado en rol (RBAC)

## <a name="review-of-user-access-rights"></a>Revisión de los derechos de acceso de los usuarios

La organización mantiene una lista documentada de los usuarios autorizados de los recursos de información.

- \[Versión preliminar\]: Mostrar los resultados de las auditorías de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Cuentas"

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Diagnóstico remoto y protección de puertos de configuración

Los puertos, servicios y aplicaciones similares instalados en un equipo o en sistemas de red que no son específicamente necesarios para la funcionalidad empresarial, se deshabilitan o se quitan.

- Se deben cerrar los puertos de administración en las máquinas virtuales
- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales

## <a name="audit-logging"></a>Registro de auditoría

Los registros de mensajes enviados y recibidos se mantienen, incluyendo la fecha, la hora, el origen y el destino del mensaje, pero no su contenido. La auditoría está siempre disponible mientras el sistema está activo y realiza un seguimiento de los eventos clave, el acceso a los datos correctos y erróneos, los cambios de configuración de seguridad del sistema, los privilegios o el uso de las utilidades, las alarmas generadas, la activación y desactivación de los sistemas de protección (por ejemplo, A/V e ID), la activación y desactivación de los mecanismos de identificación y autenticación, y la creación y eliminación de los objetos de nivel de sistema.

- Los registros de diagnóstico del centro de eventos deben estar habilitados
- Se deben instalar las actualizaciones del sistema en los conjuntos de escalado de máquinas virtuales

## <a name="monitoring-system-use"></a>Supervisión del uso del sistema

Los sistemas automatizados implementados en todo el entorno de la organización se usan para supervisar eventos clave y actividades anómalas, y para analizar los registros del sistema, cuyos resultados se revisan periódicamente. La supervisión incluye operaciones con privilegios, acceso autorizado o intentos de acceso no autorizado, incluidos los intentos de acceso a cuentas desactivadas y las alertas o errores del sistema.

- Los registros de diagnóstico de los conjuntos de escalado de máquinas virtuales deben estar habilitados

## <a name="segregation-of-duties"></a>Separación de obligaciones

La separación de obligaciones se utiliza para limitar el riesgo de modificaciones no autorizada o accidentales de la información y los sistemas. Ninguna persona puede tener acceso a los sistemas de información, modificarlos o usarlos sin autorización o detección. El acceso para usuarios responsables de la administración y los controles de acceso está limitado al mínimo necesario en función de la función y las responsabilidades de cada usuario, y estas personas no pueden tener acceso a las funciones de auditoría relacionadas con estos controles.

- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción

## <a name="administrator-and-operator-logs"></a>Registros de administrador y operador

La organización garantiza que el registro correcto está habilitado para auditar las actividades del administrador; y revisa los registros de operador y administrador del sistema de forma periódica.

- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción

## <a name="identification-of-risks-related-to-external-parties"></a>Identificación de riesgos relacionados con entidades externas

Las conexiones de acceso remoto entre la organización y las entidades externas se cifran

- El cifrado de discos debe aplicarse en las máquinas virtuales

## <a name="business-continuity-and-risk-assessment"></a>Continuidad del negocio y evaluación de riesgos

La organización identifica sus procesos empresariales críticos, e integra los requisitos de administración de la seguridad de la información pertenecientes a la continuidad del negocio, con otros requisitos de continuidad relativos a tales aspectos como operaciones, personal, materiales, transporte. y recursos.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Consola de recuperación"

> [!NOTE]
> La disponibilidad de definiciones específicas de Azure Policy puede variar tanto en Azure Government como en otras nubes nacionales. 

## <a name="next-steps"></a>Pasos siguientes

Ya ha leído la asignación de controles del ejemplo de plano técnico de HITRUST/HIPAA. Ahora, consulte los siguientes artículos para ver una introducción y aprender a implementar este ejemplo:

> [!div class="next step action"]
> [Plano técnico de HIPAA HITRUST: Introducción](./control-mapping.md)
> [Plano técnico de HIPAA/HITRUST: pasos para la implementación](./deploy.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Información acerca del [ciclo de vida del plano técnico](../../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).
