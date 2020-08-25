---
title: Controles de ejemplo de plano técnico de HIPAA HITRUST
description: Asignación de los controles de los ejemplos de plano técnico de HIPAA HITRUST. Cada control se asigna a una o varias directivas de Azure que ayudan en la evaluación.
ms.date: 08/03/2020
ms.topic: sample
ms.openlocfilehash: 10b771e3cfb18a28bd720332a26e13bb1d1f6022
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2020
ms.locfileid: "88209413"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>Asignación de los controles de los ejemplos de plano técnico de HIPAA HITRUST

En el siguiente artículo se ofrece información detallada sobre cómo se asignan los ejemplos de plano técnico de Azure Blueprints a los controles de HIPAA HITRUST. Para más información acerca de los controles, consulte [HIPAA HITRUST](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

Las asignaciones que se presentan a continuación son para los controles de **HIPAA HITRUST**. Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Muchos de los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la **\[versión preliminar\]: Auditar los controles HITRUST/HIPAA**, que es una iniciativa de directiva integrada.

> [!IMPORTANT]
> Cada control que se muestra a continuación está asociado a una o varias definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas pueden ayudarle a [evaluar el cumplimiento](../../../policy/how-to/get-compliance-data.md) del control; sin embargo, a menudo no hay una coincidencia 1:1 o completa entre un control y una o varias directivas. Como tal, el **cumplimiento** con Azure Policy solo se refiere a las propias directivas; esto no garantiza que sea totalmente compatible con todos los requisitos de un control. Además, el estándar de cumplimiento incluye controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los controles y las definiciones de Azure Policy para este ejemplo de plano técnico de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md).

## <a name="control-against-malicious-code"></a>Control contra código malintencionado

Este proyecto le ayuda a administrar la protección de los puntos de conexión, incluida la protección del código malintencionado, mediante la asignación de definiciones de [Azure Policy](../../../policy/overview.md) que supervisan que no falte la protección de los puntos de conexión en las máquinas virtuales en Azure Security Center y aplican la solución antimalware de Microsoft en las máquinas virtuales Windows.

- Microsoft Antimalware para Azure debe estar configurado para actualizar automáticamente las firmas de protección
- Supervisión de la falta de Endpoint Protection en Azure Security Center
- La solución de protección del punto de conexión debe instalarse en las máquinas virtuales
- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales


## <a name="management-of-removable-media"></a>Administración de medios extraíbles

La organización, según el nivel de clasificación de los datos, registra los medios (incluidos los equipos portátiles) antes de su uso, impone restricciones razonables sobre cómo utilizar dichos medios y proporciona un nivel adecuado de protección física y lógica (incluido el cifrado) para los medios que contengan información cubierta, hasta que esta se destruye o se limpia correctamente.

- Se debe permitir el cifrado de datos transparente en bases de datos SQL
- El cifrado de discos debe aplicarse en máquinas virtuales
- Los discos sin asignar deben cifrarse
- Exigencia de cifrado en las cuentas de Data Lake Store.
- El protector de TDE de SQL Server debe estar cifrado con su propia clave
- El protector de TDE de la instancia administrada SQL debe estar cifrado con su propia clave

## <a name="control-of-operational-software"></a>Control del software operativo 

La organización identifica el software no autorizado en el sistema de información, incluidos servidores, estaciones de trabajo y equipos portátiles, y emplea una directiva de permitir todo, denegar por excepción para prohibir la ejecución de software no autorizado conocido en el sistema de información. Igualmente revisa y actualiza la lista de software no autorizado periódicamente, al menos una vez al año.

- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas
- Se deben corregir las vulnerabilidades en las configuraciones de seguridad de contenedor
- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales
- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales

## <a name="change-control-procedures"></a>Procedimientos de control de cambios

La integridad de todas las imágenes de máquina virtual se garantiza en todo momento mediante el registro y la generación de una alerta para cualquier cambio realizado en las imágenes de máquina virtual, y la puesta a disposición de los propietarios y/o clientes empresariales, mediante métodos electrónicos (por ejemplo, portales o alertas), de los resultados de un cambio o movimiento y de la validación posterior de la integridad de la imagen.

- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Directivas de auditoría del sistema - Seguimiento detallado"
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las VM Windows en "Directivas de auditoría del sistema - Seguimiento detallado"

## <a name="control-of-technical-vulnerabilities"></a>Control de vulnerabilidades técnicas 

Existe un estándar de configuración protegido para todos los componentes del sistema y de la red.

- La evaluación de vulnerabilidades debe estar activada en sus servidores de SQL Server.
- La evaluación de vulnerabilidad debe estar habilitada en las instancias administradas de SQL.
- \[Versión preliminar\] La evaluación de vulnerabilidad debe estar habilitada en Virtual Machines.
- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades
- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas
- Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales
- Se deben corregir las vulnerabilidades en las configuraciones de seguridad de contenedor
- Se deben corregir las vulnerabilidades de las bases de datos SQL
- \[Versión preliminar\]: Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes

## <a name="segregation-in-networks"></a>Segregación en redes

Las puertas de enlace de seguridad de la organización (por ejemplo, los firewalls) aplican las directivas de seguridad y están configuradas para filtrar el tráfico entre dominios, bloquean el acceso no autorizado y se usan para mantener la segregación entre segmentos de red cableados internos, inalámbricos internos y externos (por ejemplo, Internet), incluidas las redes perimetrales y aplican directivas de control de acceso para cada uno de los dominios.

- Las subredes deben estar asociadas con un grupo de seguridad de red.
- Las máquinas virtuales deben estar conectadas a una red virtual aprobada
- Las máquinas virtuales deben estar asociadas a un grupo de seguridad de red
- Service Bus debe usar un punto de conexión del servicio de red virtual
- App Service debe usar un punto de conexión del servicio de red virtual
- SQL Server debe usar un punto de conexión del servicio de red virtual
- El centro de eventos debe usar un punto de conexión del servicio de red virtual
- Cosmos DB debe usar un punto de conexión de servicio de red virtual
- Key Vault debe usar un punto de conexión del servicio de red virtual
- Las subredes de la puerta de enlace no se deben configurar con un grupo de seguridad de red
- Las cuentas de almacenamiento deben usar un punto de conexión del servicio de red virtual
- \[Versión preliminar\]: Container Registry debe usar un punto de conexión del servicio de red virtual
- Las recomendaciones de protección de redes adaptables se deben aplicar en máquinas virtuales con acceso interno

## <a name="network-connection-control"></a>Control de conexión de red

El tráfico de red se controla de acuerdo con la Directiva de control de acceso de la organización mediante el firewall y otras restricciones relacionadas con la red para cada punto de acceso a la red o la interfaz administrada del servicio de telecomunicaciones externo.

- Se debe habilitar la transferencia segura a las cuentas de almacenamiento
- Debe usarse la versión más reciente de TLS en la aplicación de API.
- La versión más reciente de TLS debe usarse en la aplicación web.
- La versión más reciente de TLS debe usarse en la aplicación de funciones.
- Acceso a Function App solo a través de HTTPS
- Acceso a la aplicación web solo a través de HTTPS
- Acceso a API App solo a través de HTTPS
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos PostgreSQL
- Solo se deben habilitar las conexiones seguras a Redis Cache
- Las subredes deben estar asociadas con un grupo de seguridad de red.
- Se deben proteger las reglas de NSG para las aplicaciones web en IaaS
- Se deben proteger las reglas del grupo de seguridad de red para máquinas virtuales accesibles desde Internet
- Las máquinas virtuales deben estar conectadas a una red virtual aprobada
- Las máquinas virtuales deben estar asociadas a un grupo de seguridad de red

## <a name="network-controls"></a>Controles de red

La organización utiliza canales de comunicación seguros y cifrados al migrar servidores físicos, aplicaciones o datos a servidores virtualizados.

- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.
- Las recomendaciones de protección de redes adaptables se deben aplicar en máquinas virtuales con acceso interno
- Service Bus debe usar un punto de conexión del servicio de red virtual
- App Service debe usar un punto de conexión del servicio de red virtual
- SQL Server debe usar un punto de conexión del servicio de red virtual
- El centro de eventos debe usar un punto de conexión del servicio de red virtual
- Cosmos DB debe usar un punto de conexión de servicio de red virtual
- Key Vault debe usar un punto de conexión del servicio de red virtual
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento
- Las cuentas de almacenamiento deben usar un punto de conexión del servicio de red virtual
- \[Versión preliminar\]: Container Registry debe usar un punto de conexión del servicio de red virtual

## <a name="security-of-network-services"></a>Seguridad de Network Services

Los servicios acordados proporcionados por un administrador o proveedor de servicios de red se administran y supervisan formalmente para asegurarse de que se proporcionan de forma segura.

- \[Versión preliminar\]: El agente de recopilación de datos del tráfico de red debe estar instalado en las máquinas virtuales Windows
- \[Versión preliminar\]: El agente de recopilación de datos del tráfico de red debe estar instalado en el servicio Network Watcher de las máquinas virtuales Linux

## <a name="information-exchange-policies-and-procedures"></a>Directivas y procedimientos de intercambio de información

La organización restringe el uso de medios de almacenamiento portátiles controlados por la organización por parte de personas autorizadas en sistemas de información externos.

- Asegúrese de que la aplicación web tenga la opción "Client Certificates (Incoming client certificates)" activada.
- CORS no debe permitir que todos los recursos accedan a las aplicaciones web
- CORS no debe permitir que todos los recursos accedan a Function App
- CORS no debería permitir que todos los recursos accedan a la aplicación de API.
- Se debe desactivar la depuración remota para las aplicaciones web
- Se debe desactivar la depuración remota para Function App
- Se debe desactivar la depuración remota para API Apps

## <a name="on-line-transactions"></a>Transacciones en línea

La organización requiere el uso de cifrado entre cada una de las partes implicadas en la transacción así como el uso de firmas electrónicas por parte de las mismas. La organización garantiza que el almacenamiento de los detalles de la transacción se encuentra fuera de los entornos de acceso público (por ejemplo, en una plataforma de almacenamiento que se encuentre en la intranet de la organización), y que no se conserva y expone en un medio de almacenamiento directamente accesible desde Internet. Cuando se usa una entidad de confianza (por ejemplo, para la emisión y el mantenimiento de firmas digitales y/o certificados digitales), la seguridad se integra e inserta en todo el proceso de administración de un extremo a otro del certificado o la firma.

- Se debe habilitar la transferencia segura a las cuentas de almacenamiento
- Debe usarse la versión más reciente de TLS en la aplicación de API.
- La versión más reciente de TLS debe usarse en la aplicación web.
- La versión más reciente de TLS debe usarse en la aplicación de funciones.
- Acceso a Function App solo a través de HTTPS
- Acceso a la aplicación web solo a través de HTTPS
- Acceso a API App solo a través de HTTPS
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos PostgreSQL
- Solo se deben habilitar las conexiones seguras a Redis Cache
- \[Versión preliminar\]: se implementan los requisitos previos para realizar una auditoría de las VM Windows que no contengan los certificados especificados en la raíz de confianza
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las VM Windows que no contengan los certificados especificados en la raíz de confianza

## <a name="user-password-management"></a>Administración de contraseñas de usuario

Las contraseñas se cifran durante la transmisión y el almacenamiento en todos los componentes del sistema.

- \[Versión preliminar\] Audit VMs with insecure password security settings (Realizar una auditoría de las VM que tengan una configuración de seguridad de contraseñas no segura)

## <a name="user-authentication-for-external-connections"></a>Autenticación de usuario para conexiones externas

Los métodos de autenticación seguros, como multi-factor, Radius o Kerberos (para el acceso con privilegios) y CHAP (para el cifrado de credenciales para métodos de acceso telefónico), se implementan para todas las conexiones externas a la red de la organización.

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción
- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.

## <a name="user-identification-and-authentication"></a>Identificación y autenticación de usuario

Los usuarios que han realizado funciones con privilegios (por ejemplo, la administración del sistema) utilizan cuentas independientes para realizar esas funciones con privilegios. Los métodos de autenticación multifactor se usan de acuerdo con la directiva de la organización (por ejemplo, para el acceso remoto a la red).

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción
- Debe designar un máximo de tres propietarios para la suscripción
- Debe haber más de un propietario asignado a su suscripción
- Implementar los requisitos previos para realizar una auditoría de las VM Windows en que el grupo Administradores contenga algún miembro especificado
- Mostrar los resultados de la auditoría de las máquinas virtuales Windows en las que el grupo Administradores contenga algún miembro especificado
- Implementar los requisitos previos para auditar las máquinas virtuales Windows en las que el grupo Administradores no contenga todos los miembros especificados
- Mostrar los resultados de las auditorías de las máquinas virtuales Windows en las que el grupo Administradores no contenga todos los miembros especificados
- Implementar los requisitos previos para realizar una auditoría de las máquinas virtuales Windows en las que el grupo Administradores no contenga únicamente los miembros especificados
- Mostrar los resultados de la auditoría de las máquinas virtuales Windows en las que el grupo Administradores no contenga únicamente los miembros especificados

## <a name="privilege-management"></a>Administración de privilegios

El acceso a las funciones de administración o a las consolas administrativas para sistemas que hospedan sistemas virtualizados está restringido al personal aplicando el principio de privilegios mínimos, y se admite mediante controles técnicos.

- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.
- Se deben cerrar los puertos de administración en las máquinas virtuales
- Debe designar un máximo de tres propietarios para la suscripción
- Debe haber más de un propietario asignado a su suscripción
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción
- Auditar el uso de reglas de RBAC personalizadas
- En Kubernetes Services se debe usar el control de acceso basado en rol (RBAC)

## <a name="review-of-user-access-rights"></a>Revisión de los derechos de acceso de los usuarios

La organización mantiene una lista documentada de los usuarios autorizados de los recursos de información.

- Auditar el uso de reglas de RBAC personalizadas

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Diagnóstico remoto y protección de puertos de configuración

Los puertos, servicios y aplicaciones similares instalados en un equipo o en sistemas de red que no son específicamente necesarios para la funcionalidad empresarial, se deshabilitan o se quitan.

- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.
- Se deben cerrar los puertos de administración en las máquinas virtuales
- Se debe desactivar la depuración remota para las aplicaciones web
- Se debe desactivar la depuración remota para Function App
- Se debe desactivar la depuración remota para API Apps
- Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales

## <a name="audit-logging"></a>Registro de auditoría

Los registros de mensajes enviados y recibidos se mantienen, incluyendo la fecha, la hora, el origen y el destino del mensaje, pero no su contenido. La auditoría está siempre disponible mientras el sistema está activo y realiza un seguimiento de los eventos clave, el acceso a los datos correctos y erróneos, los cambios de configuración de seguridad del sistema, los privilegios o el uso de las utilidades, las alarmas generadas, la activación y desactivación de los sistemas de protección (por ejemplo, A/V e ID), la activación y desactivación de los mecanismos de identificación y autenticación, y la creación y eliminación de los objetos de nivel de sistema.

- Los registros de diagnóstico de Azure Data Lake Store deben estar habilitados
- Los registros de diagnóstico de Logic Apps deben estar habilitados 
- Los registros de diagnóstico de IoT Hub deben estar habilitados 
- Se deben habilitar los registros de diagnóstico en las cuentas de Batch 
- Los registros de diagnóstico de Virtual Machine Scale Sets deben estar habilitados 
- Los registros de diagnóstico del centro de eventos deben estar habilitados 
- Los registros de diagnóstico de los servicios de Search deben estar habilitados. 
- Los registros de diagnóstico de App Services deben estar habilitados 
- Los registros de diagnóstico de Data Lake Analytics deben estar habilitados 
- Los registros de diagnóstico en Key Vault deben estar habilitados 
- Los registros de diagnóstico de Service Bus deben estar habilitados
- Se deben habilitar los registros de diagnóstico en Azure Stream Analytics
- La auditoría de SQL Server debe estar habilitada
- Auditar la configuración de diagnóstico
- Azure Monitor debe recopilar los registros de actividad de todas las regiones

## <a name="monitoring-system-use"></a>Supervisión del uso del sistema

Los sistemas automatizados implementados en todo el entorno de la organización se usan para supervisar eventos clave y actividades anómalas, y para analizar los registros del sistema, cuyos resultados se revisan periódicamente. La supervisión incluye operaciones con privilegios, acceso autorizado o intentos de acceso no autorizado, incluidos los intentos de acceso a cuentas desactivadas y las alertas o errores del sistema.

- Azure Monitor debe recopilar los registros de actividad de todas las regiones
- El agente de Log Analytics debe instalarse en máquinas virtuales
- El agente de Log Analytics debe instalarse en Virtual Machine Scale Sets
- \[Versión preliminar\]: Implementar los requisitos previos para realizar una auditoría de las máquinas virtuales Windows en que el agente de Log Analytics no esté conectado según lo esperado
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las máquinas virtuales Windows en que el agente de Log Analytics no esté conectado según lo esperado
- El perfil de registro de Azure Monitor debe recopilar los registros de las categorías "write", "delete" y "action"
- El aprovisionamiento automático del agente de supervisión de Log Analytics debe estar habilitado en su suscripción

## <a name="segregation-of-duties"></a>Separación de obligaciones

La separación de obligaciones se utiliza para limitar el riesgo de modificaciones no autorizada o accidentales de la información y los sistemas. Ninguna persona puede tener acceso a los sistemas de información, modificarlos o usarlos sin autorización o detección. El acceso para usuarios responsables de la administración y los controles de acceso está limitado al mínimo necesario en función de la función y las responsabilidades de cada usuario, y estas personas no pueden tener acceso a las funciones de auditoría relacionadas con estos controles.

- En Kubernetes Services se debe usar el control de acceso basado en rol (RBAC)
- Auditar el uso de reglas de RBAC personalizadas
- \[Versión preliminar\]: se implementan los requisitos previos para auditar las configuraciones de las VM Windows en "Asignación de derechos de usuario"
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las VM Windows en "Asignación de derechos de usuario"
- \[Versión preliminar\]: Implementar los requisitos previos para auditar las configuraciones de las VM Windows en "Opciones de seguridad - Control de cuentas de usuario"
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las VM Windows en "Opciones de seguridad - Control de cuentas de usuario"
- No deben existir roles de propietario de suscripción personalizados.

## <a name="administrator-and-operator-logs"></a>Registros de administrador y operador

La organización garantiza que el registro correcto está habilitado para auditar las actividades del administrador; y revisa los registros de operador y administrador del sistema de forma periódica.

- Debe existir una alerta de registro de actividad para operaciones administrativas específicas.

## <a name="identification-of-risks-related-to-external-parties"></a>Identificación de riesgos relacionados con entidades externas

Las conexiones de acceso remoto entre la organización y las entidades externas se cifran

- Se debe habilitar la transferencia segura a las cuentas de almacenamiento
- Acceso a Function App solo a través de HTTPS
- Acceso a la aplicación web solo a través de HTTPS
- Acceso a API App solo a través de HTTPS
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos PostgreSQL
- Solo se deben habilitar las conexiones seguras a Redis Cache

## <a name="business-continuity-and-risk-assessment"></a>Continuidad del negocio y evaluación de riesgos

La organización identifica sus procesos empresariales críticos, e integra los requisitos de administración de la seguridad de la información pertenecientes a la continuidad del negocio, con otros requisitos de continuidad relativos a tales aspectos como operaciones, personal, materiales, transporte. y recursos.

- Auditoría de máquinas virtuales sin la recuperación ante desastres configurada
- Los objetos del almacén de claves deben poder recuperarse
- \[Versión preliminar\]: Implementar los requisitos previos para auditar las configuraciones de las VM Windows en "Opciones de seguridad - Consola de recuperación"
- \[Versión preliminar\]: Mostrar los resultados de la auditoría de las configuraciones de las máquinas virtuales Windows en "Opciones de seguridad - Consola de recuperación"

## <a name="back-up"></a>Copia de seguridad

Este plano técnico asigna definiciones de Azure Policy que auditan la información de copia de seguridad del sistema de la organización en el sitio de almacenamiento alternativo electrónicamente. Para el envío físico de los metadatos de almacenamiento, considere la posibilidad de usar Azure Data Box.

- La copia de seguridad con redundancia geográfica a largo plazo debe estar habilitada para las instancias de Azure SQL Database.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for PostgreSQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MariaDB.
- Azure Backup debe estar habilitado para Virtual Machines.

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
