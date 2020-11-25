---
title: Procedimientos recomendados de cifrado y seguridad de datos - Microsoft Azure
description: Este artículo proporciona un conjunto de procedimientos recomendados para el cifrado y la seguridad de los datos con funcionalidades de Azure integradas.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2020
ms.author: terrylan
ms.openlocfilehash: c88a2cf33cf1149a17af4a82dcf2858822f0c6d4
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94696156"
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Procedimientos recomendados de cifrado y seguridad de datos en Azure
En este artículo se describen los procedimientos recomendados para el cifrado y la seguridad de datos.

Los procedimientos recomendados se basan en un consenso de opinión y son válidos para las funcionalidades y conjuntos de características actuales de la plataforma Azure. Como las opiniones y las tecnologías cambian con el tiempo, este artículo se actualiza de forma periódica para reflejar dichos cambios.

## <a name="protect-data"></a>Protección de datos
Para contribuir a proteger los datos en la nube, debe tener en cuenta los posibles estados que pueden tener los datos y los controles disponibles para ese estado. Los procedimientos recomendados para el cifrado y seguridad de datos de Azure están relacionados con los siguientes estados de datos:

- En reposo: incluye toda la información acerca de los objetos de almacenamiento, contenedores y tipos que existen de forma estática en los soportes físicos, ya sean discos magnéticos u ópticos.
- En tránsito: cuando se transfieren datos entre componentes, ubicaciones o programas, están en tránsito. Algunos ejemplos son la transferencia a través de la red, a través de un bus de servicio (del entorno local a la nube y viceversa, incluidas las conexiones híbridas como ExpressRoute), o durante el proceso de entrada/salida.

## <a name="choose-a-key-management-solution"></a>Elegir una solución de administración de claves

Proteger las claves es esencial para proteger los datos en la nube.

[Azure Key Vault](../../key-vault/general/overview.md) ayuda a proteger las claves criptográficas y los secretos que usan los servicios y aplicaciones en la nube. Key Vault agiliza el proceso de administración de claves y le permite mantener el control de claves que obtienen acceso a sus datos y los cifran. Los desarrolladores pueden crear claves para desarrollo y prueba en minutos y, a continuación, migrarlas a claves de producción. Los administradores de seguridad pueden conceder (y revocar) permisos a las claves según sea necesario.

Puedes usar Key Vault para crear múltiples contenedores seguros denominados almacenes. Estos almacenes están respaldados por HSM. Los almacenes ayudan a reducir las posibilidades de que se produzca una pérdida accidental de información de seguridad centralizando el almacenamiento de los secretos de aplicación. Los almacenes de claves también permiten controlar y registrar el acceso a todo lo que está almacenado en ellos. Azure Key Vault puede administrar la solicitud y renovación de certificados de Seguridad de la capa de transporte (TLS). Proporciona características para una solución sólida para la administración del ciclo de vida de certificados.

Azure Key Vault está diseñado para admitir secretos y claves de aplicación. Key Vault no está pensado para usarse como almacén para las contraseñas de usuario.

A continuación se indican los procedimientos recomendados de seguridad para el uso de Key Vault.

**Procedimiento recomendado**: conceda acceso a usuarios, grupos y aplicaciones en un ámbito concreto.   
**Detalles**: use los roles predefinidos de Azure RBAC. Por ejemplo, para conceder acceso a un usuario para administrar los almacenes de claves, se asignaría el rol predefinido [Colaborador de Key Vault](../../role-based-access-control/built-in-roles.md) a este usuario en un ámbito específico. En este caso, el ámbito caso sería una suscripción, un grupo de recursos o, simplemente, un almacén de claves específico. Si los roles predefinidos no se ajustan a sus necesidades, puede [definir sus propios roles](../../role-based-access-control/custom-roles.md).

**Procedimiento recomendado**: controle a qué tienen acceso los usuarios.   
**Detalles**: El acceso a un almacén de claves se controla a través de dos interfaces diferentes: plano de administración y plano de datos. Los controles de acceso del plano de administración y del plano de datos funcionan de forma independiente.

Use Azure RBAC para controlar a qué tienen acceso los usuarios. Por ejemplo, si desea conceder a una aplicación acceso para usar las claves de un almacén de claves, solo necesita conceder permisos de acceso al plano de datos mediante directivas de acceso de Key Vault y no se necesita acceso a ningún plano de administración para esta aplicación. Por el contrario, si quiere que un usuario pueda leer las propiedades y etiquetas del almacén, pero no tenga acceso a las claves, los secretos o los certificados, puede concederle acceso de lectura mediante Azure RBAC y no se requiere acceso al plano de datos.

**Procedimiento recomendado**: almacene los certificados en el almacén de claves. Los certificados son de gran valor. En las manos equivocadas, la seguridad de los datos o la aplicación puede estar en peligro.   
**Detalles**: Azure Resource Manager puede implementar de manera segura los certificados almacenados en Azure Key Vault para las máquinas virtuales de Azure cuando estas se implementan. Al establecer directivas de acceso adecuadas para el almacén de claves, también controla quién obtiene acceso al certificado. Otra ventaja es que administra todos los certificados desde el mismo sitio en Azure Key Vault. Consulte [Deploy Certificates to VMs from customer-managed Key Vault](/archive/blogs/kv/updated-deploy-certificates-to-vms-from-customer-managed-key-vault) (Implementar certificados en VM desde una instancia de Key Vault administrada por el usuario) para obtener más información.

**Procedimiento recomendado**: asegúrese de que puede recuperar almacenes de claves u objetos de almacén de claves si se eliminan.   
**Detalles**: la eliminación de almacenes de claves u objetos de almacén de claves puede ser involuntaria o malintencionada. Habilite las características de protección de purga y eliminación temporal de Key Vault, especialmente para las claves que se usan para cifrar datos en reposo. La eliminación de estas claves es equivalente a la pérdida de datos, así que, si es necesario, puede recuperar almacenes eliminados y objetos de almacén. Practique las operaciones de recuperación de Key Vault de forma periódica.

> [!NOTE]
> Si un usuario tiene permisos de colaborador (Azure RBAC) en un plano de administración de Key Vault, se puede conceder a sí mismo acceso al plano de datos estableciendo la directiva de acceso al almacén de claves. Se recomienda controlar de forma estricta quién tiene acceso de colaborador a los almacenes de claves, con el fin de garantizar que las personas autorizadas son las únicas que pueden acceder a los almacenes de claves, las claves, los secretos y los certificados, y administrarlos.
>
>

## <a name="manage-with-secure-workstations"></a>Administración con estaciones de trabajo seguras

> [!NOTE]
> El administrador o propietario de la suscripción debe usar una estación de trabajo de acceso seguro o una estación de trabajo con privilegios de acceso.
>
>

Puesto que la mayoría de los ataques van destinados al usuario final, el punto de conexión se convierte en uno de los principales puntos de ataque. Un atacante que ponga en peligro el punto de conexión puede aprovechar las credenciales del usuario para acceder a los datos de la organización. La mayoría de los ataques a los puntos de conexión aprovechan el hecho de que los usuarios finales son administradores en sus estaciones de trabajo locales.

**Procedimiento recomendado**: use una estación de trabajo de administración segura para proteger los datos, las tareas y las cuentas confidenciales.   
**Detalles**: use una [estación de trabajo con privilegios](/windows-server/identity/securing-privileged-access/privileged-access-workstations) para reducir la superficie expuesta a ataques de las estaciones de trabajo. Estas estaciones de trabajo de administración seguras pueden ayudar a mitigar algunos de estos ataques y a garantizar la mayor seguridad de sus datos.

**Procedimiento recomendado**: asegúrese de que los puntos de conexión están protegidos.   
**Detalles**: aplique directivas de seguridad en todos los dispositivos que se usen para consumir datos, independientemente de la ubicación de dichos datos (nube o entorno local).

## <a name="protect-data-at-rest"></a>Protección de los datos en reposo

El [cifrado de los datos en reposo](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) es un paso obligatorio en lo que respecta a la privacidad de los datos, el cumplimiento y la soberanía de los datos.

**Procedimiento recomendado**: cifre los discos para proteger los datos.   
**Detalles**: use [Azure Disk Encryption](./azure-disk-encryption-vms-vmss.md). Permite que los administradores de TI cifren discos de VM IaaS Windows y Linux. Disk Encryption combina la característica BitLocker de Windows estándar del sector y la característica dm-crypt de Linux para ofrecer el cifrado de volumen para el sistema operativo y los discos de datos.

Azure Storage y Azure SQL Database cifran los datos en reposo de forma predeterminada y muchos servicios ofrecen el cifrado como opción. Puede usar Azure Key Vault para mantener el control de las claves que se usan para acceder a los datos y cifrarlos. Consulte [Compatibilidad con modelo de cifrado de proveedores de recursos de Azure](encryption-atrest.md#azure-resource-providers-encryption-model-support) para obtener más información.

**Procedimientos recomendados**: use el cifrado para mitigar los riesgos relacionados con el acceso no autorizado a los datos.   
**Detalles**: cifre las unidades antes de escribir información confidencial en ellas.

Las organizaciones que no aplican el cifrado de datos están más expuestas a problemas de confidencialidad de los datos. Por ejemplo, los usuarios no autorizados pueden robar datos de las cuentas en peligro u obtener acceso no autorizado a los datos codificados en ClearFormat. Las compañías también tienen que demostrar que son diligentes y que usan los controles de seguridad adecuados para mejorar la seguridad de los datos a fin de cumplir las normas del sector.

## <a name="protect-data-in-transit"></a>Protección de los datos en tránsito

La protección de los datos en tránsito debe ser una parte esencial de su estrategia de protección de datos. Puesto que los datos se desplazan entre muchas ubicaciones, la recomendación general es utilizar siempre los protocolos SSL/TLS para intercambiar datos entre diferentes ubicaciones. En algunas circunstancias, es posible que desee aislar el canal de comunicación completo entre infraestructura local y en la nube mediante una VPN.

Para los datos que se desplazan entre la infraestructura local y Azure, debe plantearse usar medidas de seguridad apropiadas, como HTTPS o VPN. Al enviar tráfico cifrado entre una instancia de Azure Virtual Network y una ubicación local a través de Internet público, use [Azure VPN Gateway](../../vpn-gateway/index.yml).

Estos son los procedimientos recomendados específicos para usar Azure VPN Gateway, SSL/TLS y HTTPS.

**Procedimiento recomendado**: proteja el acceso a una red virtual de Azure desde varias estaciones de trabajo situadas en el entorno local.   
**Detalles**: use [VPN de sitio a sitio](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

**Procedimiento recomendado**: proteja el acceso a una red virtual de Azure desde una estación de trabajo situada en el entorno local.   
**Detalles**: use [VPN de punto a sitio](../../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md).

**Procedimiento recomendado**: mueva los conjuntos de datos grandes a través de un vínculo WAN de alta velocidad dedicado.   
**Detalles**: use [ExpressRoute](../../expressroute/expressroute-introduction.md). Si decide usar ExpressRoute, también puede cifrar los datos en el nivel de aplicación mediante SSL/TLS u otros protocolos para una mayor protección.

**Procedimiento recomendado**: interactúe con Azure Storage a través de Azure Portal.   
**Detalles**: todas las transacciones se realizan a través de HTTPS. También se puede usar la [API de REST Storage](/rest/api/storageservices/) a través de HTTPS para interactuar con [Azure Storage](https://azure.microsoft.com/services/storage/).

Las organizaciones que no protegen los datos en tránsito son más susceptibles a los [ataques del tipo "Man in the middle"](/previous-versions/office/skype-server-2010/gg195821(v=ocs.14)), a la [interceptación](/previous-versions/office/skype-server-2010/gg195641(v=ocs.14)) y al secuestro de sesión. Estos ataques pueden ser el primer paso para obtener acceso a datos confidenciales.

## <a name="secure-email-documents-and-sensitive-data"></a>Proteger el correo electrónico, los documentos y los datos confidenciales

Es recomendable controlar y proteger el correo electrónico, los documentos y los datos confidenciales que comparte fuera de su compañía. [Azure Information Protection](/azure/information-protection/) es una solución basada en la nube que ayuda a las organizaciones a clasificar, etiquetar y proteger sus documentos y correos electrónicos. Esto puede ser automático para los administradores que definen reglas y condiciones, manual para los usuarios o una combinación de ambas opciones cuando los usuarios reciben recomendaciones.

Es posible identificar la clasificación en todo momento, independientemente de dónde se almacenan los datos o con quién se comparten. Las etiquetas incluyen distintivos visuales, como un encabezado, pie de página o marca de agua. Se agregan metadatos a los archivos y encabezados de correo electrónico en texto no cifrado. El texto no cifrado garantiza que otros servicios, como las soluciones para evitar la pérdida de datos, puedan identificar la clasificación y tomar las medidas adecuadas.

La tecnología de protección usa Azure Rights Management (Azure RMS). Esta tecnología está integrada con otras aplicaciones y servicios en la nube de Microsoft, como Microsoft 365 y Azure Active Directory. Esta tecnología de protección usa directivas de autorización, identidad y cifrado. La protección que se aplica mediante Azure RMS se mantiene con los documentos y correos electrónicos, independientemente de la ubicación, ya sea dentro o fuera de la organización, las redes, los servidores de archivos y las aplicaciones.

Esta solución de protección de información le ofrece control sobre sus datos, incluso cuando se comparten con otras personas. También puede usar Azure RMS con sus propias aplicaciones de línea de negocio y soluciones de protección de información de proveedores de software, tanto si estas aplicaciones y soluciones están en un entorno local como si están en la nube.

Se recomienda que:

- [Implemente Azure Information Protection](/azure/information-protection/deployment-roadmap) para su organización.
- Aplique etiquetas que reflejen sus requisitos empresariales. Por ejemplo: aplique la etiqueta "extremadamente confidencial" a todos los documentos y correos electrónicos que contengan datos de alto secreto para clasificar y proteger dichos datos. A continuación, solo los usuarios autorizados podrán acceder a estos datos, con cualquier restricción que especifique.
- Configure [el registro de uso para Azure RMS](/azure/information-protection/log-analyze-usage) para que pueda supervisar cómo usa el servicio de protección su organización.

Las organizaciones con puntos débiles en la [clasificación de datos](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) y la protección de archivos pueden ser más susceptibles a la fuga o el uso incorrecto de datos. Con la protección adecuada de los archivos, puede analizar los flujos de datos para extraer conclusiones sobre su negocio, detectar comportamientos de riesgo y tomar medidas correctivas, realizar un seguimiento del acceso a los documentos, etc.

## <a name="next-steps"></a>Pasos siguientes

Vea [Patrones y procedimientos recomendados de seguridad en Azure](best-practices-and-patterns.md) para obtener más procedimientos recomendados de seguridad que pueda aplicar cuando diseñe, implemente y administre las soluciones en la nube mediante Azure.

En los siguientes recursos se ofrece más información general sobre la seguridad de Azure y los servicios de Microsoft relacionados:
* [Blog del equipo de seguridad de Azure](/archive/blogs/azuresecurity/): ofrece información actualizada sobre lo último en seguridad de Azure
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx): aquí podrá notificar vulnerabilidades de seguridad de Microsoft, incluidos problemas con Azure, o también mediante correo electrónico a secure@microsoft.com.