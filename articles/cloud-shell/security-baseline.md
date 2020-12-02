---
title: Línea de base de seguridad de Azure para Cloud Shell
description: La línea de base de seguridad de Cloud Shell proporciona instrucciones de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/19/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 24856e458858ad86d953b50709669823b35794fc
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96348269"
---
# <a name="azure-security-baseline-for-cloud-shell"></a>Línea de base de seguridad de Azure para Cloud Shell

Esta línea de base de seguridad aplica las instrucciones de [Azure Security Benchmark, versión 1.0](../security/benchmarks/overview-v1.md) a Cloud Shell. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure.
El contenido se agrupa por los **controles de seguridad** definidos en Azure Security Benchmark y las directrices relacionadas aplicables a Cloud Shell. Se han excluido los **controles** no aplicables a Cloud Shell.

 
Para ver cómo Cloud Shell se asigna por completo a Azure Security Benchmark, consulte el [archivo completo de asignación de línea de base de seguridad de Cloud Shell](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de las redes

*Para obtener más información, consulte [Azure Security Benchmark: Seguridad de redes](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: Protección de los recursos de Azure dentro de las redes virtuales

**Guía**: Los clientes pueden implementar Azure Cloud Shell en una red virtual propiedad del cliente.

Al hacerlo, se debe crear una red virtual o usar una ya existente. Asegúrese de que la red virtual elegida tenga aplicado un grupo de seguridad de red a sus subredes y configurados los controles de acceso a la red específicamente para los puertos y orígenes de confianza de la aplicación.

- [Implementación de Cloud Shell en una instancia de Azure Virtual Network](private-vnet.md)

- [Creación de un grupo de seguridad de red con reglas de seguridad](../virtual-network/tutorial-filter-network-traffic.md)

- [Implementación y configuración de Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="identity-and-access-control"></a>Control de identidades y acceso

*Para obtener más información, consulte [Azure Security Benchmark: Identidad y control de acceso](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Use el inicio de sesión único (SSO) con Azure Active Directory

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que utiliza la misma autorización que para acceder a Azure Portal; en este caso, un inicio de sesión único en Azure Portal también le autenticará con Cloud Shell. 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Use la autenticación multifactor para todo el acceso basado en Azure Active Directory

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que utiliza la misma autorización que para acceder a Azure Portal; en este caso, cualquier MFA necesaria para conectarse a Azure Portal también será necesaria en Cloud Shell. 

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

## <a name="vulnerability-management"></a>Administración de vulnerabilidades

*Para obtener más información, consulte [Azure Security Benchmark: Administración de vulnerabilidades](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Ejecute herramientas de análisis de vulnerabilidades automatizado

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor.

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de ejecutar herramientas de examen de vulnerabilidades automatizadas en el software que se ejecuta en el entorno.  

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="53-deploy-an-automated-patch-management-solution-for-third-party-software-titles"></a>5.3: Implementación de una solución de administración de revisiones automatizada de títulos de software de terceros

**Guía**: No aplicable; Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor.

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de la administración de las revisiones de software que se ejecutan en su entorno.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Compare los exámenes de vulnerabilidades opuestos

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor.

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de corregir las vulnerabilidades detectadas mediante sus exámenes de vulnerabilidades de software. Exporte los resultados de análisis en intervalos coherentes y compare los resultados con análisis anteriores para comprobar que se han corregido las vulnerabilidades. Al usar recomendaciones de administración de vulnerabilidades sugeridas por Azure Security Center, puede dinamizar en el portal de la solución seleccionada para ver los datos de análisis históricos.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Use un proceso de clasificación de riesgos para priorizar la corrección de las vulnerabilidades detectadas

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor.

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de corregir las vulnerabilidades detectadas mediante sus exámenes de vulnerabilidades de software.  Use un programa de puntuación de riesgos común (por ejemplo, Common Vulnerability Scoring System) o la clasificación de riesgos predeterminada proporcionada por su herramienta de análisis de terceros. 

- [Publicación de NIST: sistema de puntuación común de vulnerabilidades](https://www.nist.gov/publications/common-vulnerability-scoring-system)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="inventory-and-asset-management"></a>Inventario y administración de recursos

*Para obtener más información, consulte [Azure Security Benchmark: Administración de recursos y del inventario](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definición y mantenimiento de un inventario de los recursos de Azure aprobados

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor y las herramientas.  El cliente puede instalar sus propias herramientas en su propia imagen según sus necesidades organizativas, y las herramientas no requieren permisos `sudo` durante la instalación.

Se recomienda a los clientes crear un inventario de software aprobado que se instale mediante Azure Cloud Shell según las necesidades de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Supervisión de aplicaciones de software no aprobadas en recursos de proceso

**Guía**: Azure Cloud Shell es un servicio gratuito sin recursos propiedad del cliente.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor y las herramientas. 

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de supervisar las aplicaciones de software que se ejecutan en el entorno para garantizar que están aprobadas según la directiva de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Eliminación de aplicaciones de software y recursos de Azure no aprobadas

**Guía**: Azure Cloud Shell es un servicio gratuito sin recursos propiedad del cliente.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor y las herramientas. 

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de supervisar las aplicaciones de software que se ejecutan en el entorno para garantizar que el software no aprobado se administra según la directiva de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="68-use-only-approved-applications"></a>6.8: Uso exclusivo de aplicaciones aprobadas

**Guía**: Azure Cloud Shell es un servicio gratuito sin recursos propiedad del cliente.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor y las herramientas.  Es posible que el cliente no pueda quitar herramientas específicas.

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o aplicaciones en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de supervisar las aplicaciones que se ejecutan en el entorno para garantizar que estén aprobadas según la directiva de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Mantenimiento de un inventario de títulos de software aprobados

**Guía**: No aplicable; Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor.

Azure Cloud Shell permite a los clientes instalar sus propias herramientas o software en su propia imagen según sus necesidades organizativas.

Los clientes son responsables de mantener un inventario del software aprobado que se ejecuta en el entorno para garantizar que lo está según la directiva de la organización.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="612-limit-users-ability-to-execute-scripts-in-compute-resources"></a>6.12: Limitación de la capacidad de los usuarios para ejecutar scripts en los recursos de proceso

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Las acciones que se realizan en Cloud Shell funcionan igual que las acciones realizadas desde las mismas herramientas o lenguajes que se ejecutan en un entorno local.  Las acciones procedentes de herramientas y lenguajes individuales deben estar restringidas; los clientes no pueden restringir el acceso a Cloud Shell ni restringir lo que está disponible para un usuario.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Segregación física o lógica de aplicaciones de alto riesgo

**Guía**: Azure Cloud Shell se puede aislar en una red virtual del cliente.

- [Implementación de Cloud Shell en una instancia de Azure Virtual Network](private-vnet.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="secure-configuration"></a>Configuración segura

*Para obtener más información, consulte [Azure Security Benchmark: Configuración segura](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: Elimine la exposición de credenciales no intencionada

**Guía**: Cloud Shell permite la ejecución, creación y carga de scripts en el entorno de Cloud Shell.  Se recomienda migrar las credenciales a Azure Key Vault.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="malware-defense"></a>Defensa contra malware

*Para obtener más información, consulte [Azure Security Benchmark: defensa contra malware](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-antimalware-software"></a>8.1: Uso de software antimalware administrado centralmente

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor y las herramientas.  El cliente puede instalar sus propias herramientas en su propia imagen según sus necesidades organizativas, y las herramientas no requieren permisos `sudo` durante la instalación.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="83-ensure-antimalware-software-and-signatures-are-updated"></a>8.3: Garantía de la actualización del software y las firmas antimalware

**Guía**: Azure Cloud Shell es una experiencia de línea de comandos basada en explorador que se usa para la administración interactiva de los recursos de nube.  Cada contenedor de cliente es efímero; en cada sesión se usa un contenedor nuevo.  El equipo de Cloud Shell supervisa y actualiza las imágenes de contenedor y las herramientas.  El cliente puede instalar sus propias herramientas en su propia imagen según sus necesidades organizativas, y las herramientas no requieren permisos `sudo` durante la instalación.

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="incident-response"></a>Respuesta a los incidentes

*Para obtener más información, consulte [Azure Security Benchmark: Respuesta a los incidentes](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

**Instrucciones**: Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.
- [Configuración de las automatizaciones de flujos de trabajo en Azure Security Center](../security-center/security-center-planning-and-operations-guide.md) 
- [Guía para crear su propio proceso de respuesta a incidentes de seguridad](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 
- [Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 
- [El cliente también puede usar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

**Instrucciones**: Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.
Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.
- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md) 
- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md) 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

**Instrucciones**: Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.
- [Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y funcionalidades de TI](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf) 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

**Guía**: La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente. Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.
- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md) 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

**Instrucciones**: Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Sentinel.
- [Configuración de la exportación continua](../security-center/continuous-export.md) 
- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md) 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

**Instrucciones**: use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente respuestas mediante "Logic Apps" en las alertas y recomendaciones de seguridad.
- [Configuración de la automatización de flujo de trabajo y Logic Apps](../security-center/workflow-automation.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="penetration-tests-and-red-team-exercises"></a>Pruebas de penetración y ejercicios del equipo rojo

*Para obtener más información, consulte [Azure Security Benchmark: Pruebas de penetración y ejercicios del equipo rojo](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Realice pruebas de penetración periódicas de los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos

**Guía**: Siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.
- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 
- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e) 

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [prueba comparativa de seguridad de Azure](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).