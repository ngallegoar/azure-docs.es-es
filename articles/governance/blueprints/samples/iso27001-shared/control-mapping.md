---
title: Controles del ejemplo de plano técnico de servicios compartidos ISO 27001
description: Asignación de controles del ejemplo de plano técnico de servicios compartidos ISO 27001. Cada control se asigna a una o varias directivas de Azure que ayudan en la evaluación.
ms.date: 01/13/2020
ms.topic: sample
ms.openlocfilehash: 6c03da7d5d4caada9ef47a828163a79a003bea93
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "75922521"
---
# <a name="control-mapping-of-the-iso-27001-shared-services-blueprint-sample"></a>Asignación de control del ejemplo de plano técnico de servicios compartidos según la norma ISO 27001

En el siguiente artículo se detalla cómo se asigna el ejemplo de plano técnico de servicios compartidos según la norma ISO 27001 de Azure Blueprints a los controles de ISO 27001. Para más información sobre los controles, consulte [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

Las siguientes asignaciones son para los controles de **ISO 27001:2013**. Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Muchos de los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la iniciativa de directiva integrada **\[Versión preliminar\] Auditar los controles ISO 27001:2013 e implementar extensiones de VM específicas para admitir los requisitos de auditoría**.

> [!IMPORTANT]
> Cada control que se muestra a continuación está asociado a una o varias definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas pueden ayudarle a [evaluar el cumplimiento](../../../policy/how-to/get-compliance-data.md) del control; sin embargo, a menudo no hay una coincidencia 1:1 o completa entre un control y una o varias directivas. Como tal, el **cumplimiento** con Azure Policy solo se refiere a las propias directivas; esto no garantiza que sea totalmente compatible con todos los requisitos de un control. Además, el estándar de cumplimiento incluye controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los controles y las definiciones de Azure Policy para este ejemplo de plano técnico de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/iso27001-shared/control-mapping.md).

## <a name="a612-segregation-of-duties"></a>A.6.1.2 Separación de obligaciones

Tener solo un propietario de la suscripción de Azure no permite la redundancia administrativa. Por el contrario, tener demasiados propietarios de suscripción de Azure aumenta la posibilidad de infracción de seguridad mediante una cuenta de propietario en riesgo. Este plano técnico ayuda a mantener un número adecuado de propietarios de suscripción de Azure mediante la asignación de dos definiciones de [Azure Policy](../../../policy/overview.md) que auditarán el número de estos. La administración de permisos de propietario de suscripción ayuda a implementar la separación adecuada de las tareas.

- \[Versión preliminar\]: Auditoría del número mínimo de propietarios de una suscripción
- \[Versión preliminar\]: Auditar el número máximo de propietarios de una suscripción

## <a name="a821-classification-of-information"></a>A.8.2.1 Clasificación de la información

El [servicio de evaluación de vulnerabilidad de SQL](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) de Azure puede ayudarle a detectar información confidencial almacenada en las bases de datos e incluye recomendaciones para clasificar esa información. Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) para comprobar si las vulnerabilidades identificadas durante el examen de evaluación de vulnerabilidad de SQL se corrigieron.

- \[Versión preliminar\]: Supervisión de los resultados de evaluación de puntos vulnerables de SQL en Azure Security Center

## <a name="a912-access-to-networks-and-network-services"></a>A.9.1.2 Acceso a redes y servicios de red

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para administrar quién tiene acceso a los recursos de Azure. Este plano técnico ayuda a controlar el acceso a los recursos de Azure mediante la asignación de siete definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas auditan el uso de los tipos de recursos y las configuraciones que pueden permitir un acceso más flexible a los recursos.
Conocer los recursos que infringen estas directivas ayuda a tomar medidas correctivas para garantizar que el acceso a los recursos de Azure se limita a los usuarios autorizados.

- \[Versión preliminar\]: Implementación de la extensión de la máquina virtual para auditar las cuentas de máquinas virtuales Linux sin contraseñas
- \[Versión preliminar\]: Implementación de la extensión de la máquina virtual para realizar una autoría de la máquina virtual Linux que permite conexiones remotas desde cuentas sin contraseña
- \[Versión preliminar\]: Auditoría de las cuentas de máquinas virtuales Linux sin contraseñas
- \[Versión preliminar\]: Auditoría de las máquinas virtuales Linux que permiten conexiones remotas de cuentas sin contraseñas
- Auditar el uso de las cuentas de almacenamiento clásicas
- Auditar el uso de máquinas virtuales clásicas
- Auditar las máquinas virtuales que no utilizan discos administrados

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 Administración de los derechos de acceso con privilegios

Este plano técnico ayuda a restringir y controlar los derechos de acceso con privilegios mediante la asignación de cuatro definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas externas con propietario, o escribir permisos y cuentas con propietario o permisos que no tienen la autenticación multifactor habilitada. Azure implementa el control de acceso basado en rol (RBAC) para administrar quién tiene acceso a los recursos de Azure. Este plano técnico también asigna tres definiciones de Azure Policy para auditar el uso de la autenticación de Azure Active Directory para SQL Server y Service Fabric. El uso de la autenticación de Azure Active Directory simplifica la administración de permisos y centraliza la administración de identidades de usuarios de base de datos y otros servicios de Microsoft. Este plano técnico también asigna una definición de Azure Policy para auditar el uso de reglas RBAC personalizadas. Conocer dónde se implementan las reglas RBAC personalizadas ayuda a verificar que la implementación se necesita y se realiza correctamente, ya que estas suelen producir errores.

- \[Versión preliminar\]: Auditar las cuentas con permisos de propietario que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Auditar las cuentas con permisos de escritura que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Auditar las cuentas externas de una suscripción que tengan permisos de propietario
- \[Versión preliminar\]: Auditar las cuentas externas de una suscripción que tengan permisos de escritura
- Auditar el aprovisionamiento de un administrador de Azure Active Directory para SQL Server
- Auditar el uso de Azure Active Directory para la autenticación de clientes en Service Fabric
- Auditar el uso de reglas de RBAC personalizadas

## <a name="a924-management-of-secret-authentication-information-of-users"></a>A.9.2.4 Administración de la información de autenticación secreta de los usuarios

Este plano técnico asigna tres definiciones de [Azure Policy](../../../policy/overview.md) para auditar cuentas que no tienen la autenticación multifactor habilitada. La autenticación multifactor ayuda a mantener las cuentas seguras aunque algún dato de la autenticación esté en riesgo. Mediante la supervisión de las cuentas que no tienen la autenticación multifactor habilitada puede identificar las cuentas en mayor o menor riesgo. Este plano técnico también asigna dos definiciones de Azure Policy que auditan los permisos de los archivos con contraseña de máquina virtual Linux y alertan si la configuración es incorrecta. Esta configuración permite tomar medidas correctivas para garantizar que los autenticadores no están en riesgo.

- \[Versión preliminar\]: Auditar las cuentas con permisos de propietario que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Auditar las cuentas con permisos de lectura que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Auditar las cuentas con permisos de escritura que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Implementación de la extensión de máquina virtual para auditar los permisos del archivo de contraseñas de máquina virtual Linux
- \[Versión preliminar\]: Auditoría de que los permisos del archivo/etc/passwd de la máquina virtual Linux se establecen en 0644

## <a name="a925-review-of-user-access-rights"></a>A.9.2.5 Revisión de los derechos de acceso de los usuarios

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para permitirle administrar quién tiene acceso a los recursos de Azure. Mediante Azure Portal, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Este plano técnico asigna cuatro definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas que deberían priorizarse para la revisión, incluidas las cuentas en desuso y externas con permisos elevados.

- \[Versión preliminar\]: Auditar las cuentas en desuso de una suscripción
- \[Versión preliminar\]: Auditar las cuentas en desuso de una suscripción que tengan permisos de propietario
- \[Versión preliminar\]: Auditar las cuentas externas de una suscripción que tengan permisos de propietario
- \[Versión preliminar\]: Auditar las cuentas externas de una suscripción que tengan permisos de escritura

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 Eliminación o ajuste de los derechos de acceso

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para permitirle administrar quién tiene acceso a los recursos de Azure. Con [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) y RBAC puede actualizar los roles de usuario para reflejar los cambios en la organización. Cuando sea necesario, se puede bloquear el inicio de sesión de las cuentas (o eliminar estas), lo cual eliminaría inmediatamente los derechos de acceso a los recursos de Azure. Este plano técnico asigna dos definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas en desuso cuya eliminación debería considerarse.

- \[Versión preliminar\]: Auditar las cuentas en desuso de una suscripción
- \[Versión preliminar\]: Auditar las cuentas en desuso de una suscripción que tengan permisos de propietario

## <a name="a942-secure-log-on-procedures"></a>A.9.4.2 Procedimientos seguros de inicio de sesión

Este plano técnico asigna tres definiciones de Azure Policy para auditar cuentas que no tienen la autenticación multifactor habilitada. Azure Multi-Factor Authentication proporciona más seguridad, ya que requiere una segunda forma de autenticación y ofrece autenticación segura. Mediante la supervisión de las cuentas que no tienen la autenticación multifactor habilitada puede identificar las cuentas en mayor o menor riesgo.

- \[Versión preliminar\]: Auditar las cuentas con permisos de propietario que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Auditar las cuentas con permisos de lectura que no tengan MFA habilitada en una suscripción
- \[Versión preliminar\]: Auditar las cuentas con permisos de escritura que no tengan MFA habilitada en una suscripción

## <a name="a943-password-management-system"></a>A.9.4.3 Sistema de administración de contraseñas

Este plano técnico ayuda a instaurar contraseñas seguras mediante la asignación de diez definiciones de [Azure Policy](../../../policy/overview.md) que auditan las máquinas virtuales Windows que no tienen unos requisitos mínimos de seguridad de contraseña y de otros tipos. Conocer las máquinas virtuales que infringen la directiva de seguridad de las contraseñas ayuda a tomar medidas correctivas para garantizar que las contraseñas de todas las cuentas de usuario de máquina virtual cumplen la directiva.

- \[Versión preliminar\]: Implementación de la extensión de la máquina virtual para auditar que la máquina virtual Windows exige requisitos de complejidad de contraseñas
- \[Versión preliminar\]: Implementación de la extensión de la máquina virtual para auditar la duración máxima de la contraseña de 70 días de la máquina virtual Windows
- \[Versión preliminar\]: Implementación de la extensión de la máquina virtual para auditar la duración mínima de la contraseña de 1 día de la máquina virtual Windows
- \[Versión preliminar\]: Implementación de la extensión de la máquina virtual para auditar las contraseñas de la máquina virtual Windows que deben tener al menos 14 caracteres
- \[Versión preliminar\]: Implementación de la extensión de la máquina para auditar la máquina virtual Windows no debe permitir las 24 contraseñas anteriores
- \[Versión preliminar\]: Auditoría de la máquina virtual Windows para aplicar los requisitos de complejidad de contraseñas
- \[Versión preliminar\]: Auditoría de la duración máxima de la contraseña de 70 días para la máquina virtual Windows
- \[Versión preliminar\]: Auditoría de la duración mínima de la contraseña de 1 día para la máquina virtual Windows
- \[Versión preliminar\]: Auditoría de que las contraseñas de la máquina virtual Windows deben tener al menos 14 caracteres
- \[Versión preliminar\]: Auditoría de que la máquina virtual Windows no permite las 24 contraseñas anteriores

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 Directiva sobre el uso de controles criptográficos

Este plano técnico ayuda a instaurar la directiva sobre el uso de controles criptográficos mediante la asignación de trece definiciones de [Azure Policy](../../../policy/overview.md) que instauran controles criptográficos específicos y auditan el uso de configuraciones criptográficas poco seguras.
Conocer dónde los recursos de Azure pueden tener configuraciones criptográficas subóptimas ayuda a tomar medidas correctivas para garantizar que los recursos se configuran conforme a la directiva de seguridad de la información. En concreto, las directivas asignadas por este plano técnico requieren cifrado para las cuentas de Blob Storage y Data Lake Storage y cifrado de datos transparente en las bases de datos SQL; auditan el cifrado que falta en las cuentas de almacenamiento, las bases de datos SQL, los discos de máquina virtual y las variables de cuenta de Automation; auditan las conexiones no seguras a cuentas de almacenamiento, Function App, Web Apps, API Apps, y Redis Cache; auditan el cifrado de contraseñas no seguras de las máquinas virtuales y la comunicación sin cifrado de Service Fabric.

- \[Versión preliminar\]: Auditoría del acceso solo mediante HTTPS para una instancia de Function App
- \[Versión preliminar\]: Auditoría del acceso solo mediante HTTPS para una aplicación web
- \[Versión preliminar\]: Auditoría del acceso solo mediante HTTPS para una aplicación de API
- \[Versión preliminar\]: Auditoría de la falta del cifrado de blob de las cuentas de almacenamiento
- \[Versión preliminar\]: La implementación de la extensión de la máquina virtual para auditar la máquina virtual Windows no debería almacenar las contraseñas mediante el cifrado reversible
- \[Versión preliminar\]: La auditoría de la máquina virtual Windows no debería almacenar las contraseñas mediante el cifrado reversible
- \[Versión preliminar\]: Supervisión de discos de máquinas virtuales sin cifrar en Azure Security Center
- Auditar la habilitación del cifrado de variables de cuentas de Automation
- Auditar la habilitación solo de conexiones seguras a Redis Cache
- Auditar la transferencia segura a cuentas de almacenamiento
- Auditar la configuración de la propiedad ClusterProtectionLevel como EncryptAndSign en Service Fabric
- Auditar el estado del cifrado de datos transparente
- Se debe permitir el cifrado de datos transparente en bases de datos SQL

## <a name="a1241-event-logging"></a>A.12.4.1 Registro de eventos

Este plano técnico ayuda a garantizar que los eventos del sistema se registran; para ello, se asignan siete definiciones de [Azure Policy](../../../policy/overview.md) que auditan las configuraciones de registro de los recursos de Azure.
Los registros de diagnóstico proporcionan conclusiones detalladas sobre las operaciones que se realizaron en recursos de Azure.

- \[Versión preliminar\]: Auditoría de implementación de Dependency Agent: imagen de la VM (SO) no mostrada
- \[Versión preliminar\]: Auditoría de implementación de Dependency Agent en VMSS: la imagen de la VM (SO) no está en la lista
- \[Versión preliminar\]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- \[Versión preliminar\]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- Auditar la configuración de diagnóstico
- Auditar configuración de auditoría de SQL en el nivel de servidor
- La configuración de seguridad avanzada de datos debe estar habilitada en SQL Server

## <a name="a1243-administrator-and-operator-logs"></a>A.12.4.3 Registros de administrador y operador

Este plano técnico le permite garantizar que los eventos del sistema se registran. Para ello, se asignan siete definiciones de Azure Policy que auditan las configuraciones de registro de los recursos de Azure. Los registros de diagnóstico proporcionan conclusiones detalladas sobre las operaciones que se realizaron en recursos de Azure.

- \[Versión preliminar\]: Auditoría de implementación de Dependency Agent: imagen de la VM (SO) no mostrada
- \[Versión preliminar\]: Auditoría de implementación de Dependency Agent en VMSS: la imagen de la VM (SO) no está en la lista
- \[Versión preliminar\]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- \[Versión preliminar\]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- Auditar la configuración de diagnóstico
- Auditar configuración de auditoría de SQL en el nivel de servidor
- La configuración de seguridad avanzada de datos debe estar habilitada en SQL Server

## <a name="a1244-clock-synchronization"></a>A.12.4.4 Sincronización del reloj

Este plano técnico le permite garantizar que los eventos del sistema se registran. Para ello, se asignan siete definiciones de Azure Policy que auditan las configuraciones de registro de los recursos de Azure. Los registros de Azure se basan en relojes internos sincronizados para crear un registro con correlación de tiempo de eventos entre recursos.

- \[Versión preliminar\]: Auditoría de implementación de Dependency Agent: imagen de la VM (SO) no mostrada
- \[Versión preliminar\]: Auditoría de implementación de Dependency Agent en VMSS: la imagen de la VM (SO) no está en la lista
- \[Versión preliminar\]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- \[Versión preliminar\]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- Auditar la configuración de diagnóstico
- Auditar configuración de auditoría de SQL en el nivel de servidor
- La configuración de seguridad avanzada de datos debe estar habilitada en SQL Server

## <a name="a1251-installation-of-software-on-operational-systems"></a>A.12.5.1 Instalación de software en sistemas operativos

El control de aplicaciones adaptable es la solución de Azure Security Center que permite controlar qué aplicaciones pueden ejecutarse en sus máquinas virtuales ubicadas en Azure. Este plano técnico asigna una definición de Azure Policy que supervisa los cambios en el conjunto de aplicaciones permitidas. Esta funcionalidad le permite controlar la instalación de software y aplicaciones en VM de Azure.

- \[Versión preliminar\]: Supervisión de una posible inclusión de aplicaciones en la lista de permitidos en Azure Security Center

## <a name="a1261-management-of-technical-vulnerabilities"></a>A.12.6.1 Administración de vulnerabilidades técnicas

Este plano técnico ayuda a administrar las vulnerabilidades del sistema de información al asignar cinco definiciones de [Azure Policy](../../../policy/overview.md) que supervisan si faltan actualizaciones del sistema, o si existen vulnerabilidades del sistema operativo, de SQL o de las máquinas virtuales de Azure Security Center. Azure Security Center proporciona funcionalidades de informes que permiten tener información en tiempo real sobre el estado de seguridad de los recursos de Azure implementados.

- \[Versión preliminar\]: Supervisión de la falta de Endpoint Protection en Azure Security Center
- \[Versión preliminar\]: Supervisión de las actualizaciones del sistema que faltan en Azure Security Center
- \[Versión preliminar\]: Supervisión de los puntos vulnerables del sistema operativo en Azure Security Center
- \[Versión preliminar\]: Supervisión de los resultados de evaluación de puntos vulnerables de SQL en Azure Security Center
- \[Versión preliminar\]: Supervisión de los puntos vulnerables de máquinas virtuales en Azure Security Center

## <a name="a1262-restrictions-on-software-installation"></a>A.12.6.2 Restricciones de instalación de software

El control de aplicaciones adaptable es la solución de Azure Security Center que permite controlar qué aplicaciones pueden ejecutarse en sus máquinas virtuales ubicadas en Azure. Este plano técnico asigna una definición de Azure Policy que supervisa los cambios en el conjunto de aplicaciones permitidas. Las restricciones de instalación de software pueden ayudar a reducir la probabilidad de que se introduzcan vulnerabilidades de software.

- \[Versión preliminar\]: Supervisión de una posible inclusión de aplicaciones en la lista de permitidos en Azure Security Center

## <a name="a1311-network-controls"></a>A.13.1.1 Controles de red

Este plano técnico ayuda a administrar y controlar las redes; para ello, asigna una definición de [Azure Policy](../../../policy/overview.md) que supervisa los grupos de seguridad de red con reglas permisivas. Las reglas demasiado permisivas pueden permitir el acceso de red no deseado y deben revisarse. Este plano técnico también asigna tres definiciones de Azure Policy que supervisan las aplicaciones, las cuentas de almacenamiento y los puntos de conexión desprotegidos. Los puntos de conexión y las aplicaciones que no están protegidos por firewall y las cuentas de almacenamiento con acceso sin restricciones pueden permitir el acceso no deseado a la información contenida en el sistema de información.

- \[Versión preliminar\]: Supervisión del acceso de red permisivo en Azure Security Center
- \[Versión preliminar\]: Supervisión de puntos de conexión de red desprotegidos en Azure Security Center
- \[Versión preliminar\]: Supervisión de la aplicación web desprotegida en Azure Security Center
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 Directivas y procedimientos de transferencia de información

El plano técnico ayuda a garantizar que la transferencia de información con los servicios de Azure es segura; para ello, asigna dos definiciones de [Azure Policy](../../../policy/overview.md) para auditar las conexiones no seguras a las cuentas de almacenamiento y a Redis Cache.

- Auditar la habilitación solo de conexiones seguras a Redis Cache
- Auditar la transferencia segura a cuentas de almacenamiento

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de control del plano técnico de los servicios compartidos según la norma ISO 27001, visite los siguientes artículos para obtener información sobre la arquitectura y cómo implementar este ejemplo:

> [!div class="nextstepaction"]
> [Plano técnico de servicios compartidos según la norma ISO 27001: introducción](./index.md)
> [Plano técnico de servicios compartidos según la norma ISO 27001: pasos de implementación](./deploy.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Información acerca del [ciclo de vida del plano técnico](../../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).