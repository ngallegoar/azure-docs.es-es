---
title: Caja de seguridad del cliente de Microsoft Azure
description: Introducción técnica de la Caja de seguridad del cliente de Microsoft Azure, que proporciona control sobre el acceso de proveedor en la nube cuando Microsoft necesite tener acceso a los datos del cliente.
author: TerryLanfear
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 09/09/2020
ms.openlocfilehash: 5c24bd80721f626e38dcb886e89231c0b86056df
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89650959"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Caja de seguridad del cliente de Microsoft Azure

> [!NOTE]
> Para usar esta característica, su organización debe tener un [plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/) con un nivel mínimo de tipo**Desarrollador**.

La Caja de seguridad del cliente de Microsoft Azure proporciona una interfaz para los clientes y así permitirles revisar y aprobar o rechazar las solicitudes de acceso de datos de cliente. Se utiliza en casos donde un ingeniero de Microsoft necesita obtener acceso a los datos del cliente durante una solicitud de soporte técnico.

Este artículo trata sobre cómo se inician, siguen y almacenan las solicitudes de la Caja de seguridad del cliente para revisiones y auditorías posteriores.

La Caja de seguridad del cliente ya está disponible con carácter general y actualmente está habilitada para poder acceder desde el escritorio remoto a las máquinas virtuales.

## <a name="supported-services-and-scenarios-in-preview"></a>Servicios y escenarios admitidos en versión preliminar

Los siguientes servicios están actualmente en versión preliminar para Caja de seguridad del cliente:

- API Management
- Azure App Service
- Azure Database for MySQL
- Azure Databricks
- Azure Synapse Analytics
- Cognitive Services
- Container Registry
- Azure Data Factory
- Azure Database for PostgreSQL
- Azure Kubernetes Service
- Azure Data Box
- HDInsight
- Functions
- Azure Storage
- Azure SQL DB
- Explorador de datos de Azure
- Máquinas virtuales (que ahora también abarcan el acceso a volcados de memoria y discos administrados)
- Transferencias de suscripciones de Azure

Para habilitar estas ofertas en versión preliminar de Caja de seguridad del cliente para su organización, regístrese en la [versión preliminar pública de Azure de Caja seguridad del cliente](https://aka.ms/customerlockbox/insiderprogram).

## <a name="supported-services-and-scenarios-in-general-availability"></a>Servicios y escenarios admitidos con disponibilidad general

Los siguientes servicios y escenarios ya están disponibles con carácter general para la Caja de seguridad del cliente.

### <a name="remote-desktop-access-to-virtual-machines"></a>Acceso del escritorio remoto a las máquinas virtuales

La Caja de seguridad del cliente está actualmente habilitada para poder acceder desde el escritorio remoto a las máquinas virtuales. Se admiten las siguientes áreas de trabajo:
- Plataforma como servicio (PaaS): Azure Cloud Services (rol web y rol de trabajo)
- Infraestructura como servicio (IaaS): Windows y Linux (solo Azure Resource Manager)
- Conjunto de escalado de máquinas virtuales: Windows y Linux

> [!NOTE]
> Las instancias del modelo clásico de IaaS no son compatibles con la Caja de seguridad del cliente. Si tiene cargas de trabajo ejecutándose en instancias del modelo clásico de IaaS, le recomendamos que las migre de los modelos de implementación Classic a Resource Manager. Para obtener más información, consulte [Migración compatible con la plataforma de recursos de IaaS del modelo clásico al de Azure Resource Manager](../../virtual-machines/windows/migration-classic-resource-manager-overview.md)

#### <a name="detailed-audit-logs"></a>Registros de auditoría detallados

En cuanto a los escenarios que usan el acceso al escritorio remoto, puede usar los registros de eventos de Windows para revisar las acciones del ingeniero de Microsoft. Asimismo, puede usar Azure Security Center para recopilar sus registros de eventos y copiar los datos a su área de trabajo para analizarlos. Para obtener más información, consulte [Recolección de datos en Azure Security Center](../../security-center/security-center-enable-data-collection.md).

## <a name="workflow"></a>Flujo de trabajo

Los siguientes pasos describen un flujo de trabajo típico para una solicitud de la Caja de seguridad del cliente.

1. Un usuario en una organización tiene un problema con su carga de trabajo de Azure.

2. Esta persona soluciona el problema pero, como no puede corregirlo, se abre una incidencia de soporte técnico desde [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). La incidencia de soporte técnico se asigna a un ingeniero del servicio de soporte técnico de Azure.

3. Un ingeniero del soporte técnico de Azure revisa la solicitud de servicio y determina los pasos siguientes para resolver el problema.

4. Si el ingeniero de soporte técnico no puede solucionar el problema mediante la telemetría y las herramientas estándar, el siguiente paso es solicitar permisos con privilegios elevados mediante un servicio de acceso Just-In-Time (JIT). Esta solicitud puede proceder del ingeniero de soporte técnico original. O bien, puede ser de un ingeniero diferente porque el problema se escala al equipo de DevOps de Azure.

5. Una vez que el Ingeniero de Azure envía la solicitud de acceso, el servicio Just-In-Time evalúa la solicitud teniendo en cuenta factores como:
    - El ámbito del recurso
    - Si el solicitante es una identidad aislada o si usa autenticación multifactor.
    - Niveles de permisos

    De acuerdo con la regla JIT, esta solicitud también puede incluir una aprobación de los aprobadores internos de Microsoft. Por ejemplo, el aprobador podría ser el líder del soporte técnico al cliente o el encargado de DevOps.

6. Cuando la solicitud requiere obtener acceso directo a los datos del cliente, se inicia una solicitud de Caja de seguridad del cliente. Por ejemplo, se obtiene acceso desde el escritorio remoto a la máquina virtual de un cliente.

    La solicitud se encuentra ahora en un estado de **Cliente notificado**, según el cual se espera la aprobación del cliente antes de conceder el acceso.

7. En la organización del cliente, el usuario que tiene el [rol de propietario](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) de la suscripción de Azure recibe un correo electrónico de Microsoft para notificarle sobre la solicitud de acceso pendiente. Para las solicitudes de la Caja de seguridad del cliente, esta persona es el aprobador designado.

    Correo electrónico de ejemplo:

    ![Caja de seguridad del cliente de Azure: notificaciones por correo electrónico](./media/customer-lockbox-overview/customer-lockbox-email-notification.png)

8. La notificación por correo electrónico proporciona un enlace a la hoja **Caja de seguridad del cliente** en Azure Portal. Al usar este enlace, el aprobador designado inicia sesión en Azure Portal para ver las solicitudes pendientes que su organización tiene para la Caja de seguridad del cliente:

    ![Caja de seguridad del cliente Azure: página de aterrizaje](./media/customer-lockbox-overview/customer-lockbox-landing-page.png)

   La solicitud permanece en la cola del cliente durante cuatro días. Transcurrido ese tiempo, la solicitud de acceso expira automáticamente y no se concede acceso a los ingenieros de Microsoft.

9. Para obtener los detalles de la solicitud pendiente, el aprobador designado puede seleccionar la solicitud de la caja de seguridad desde **Solicitudes pendientes**:

    ![Caja de seguridad del cliente de Azure: ver la solicitud pendiente](./media/customer-lockbox-overview/customer-lockbox-pending-requests.png)

10. El aprobador designado también puede seleccionar el **ID DE SOLICITUD DE SERVICIO** para ver la solicitud de la incidencia de soporte técnico que creó el usuario original. Esta información proporciona un contexto para el motivo por el cual el Soporte técnico de Microsoft está involucrado y el historial del problema informado. Por ejemplo:

    ![Caja de seguridad del cliente de Azure: ver la incidencia de soporte técnico](./media/customer-lockbox-overview/customer-lockbox-support-ticket.png)

11. Después de revisar la solicitud, el aprobador designado selecciona **Aprobar** o **Denegar**:

    ![Caja de seguridad del cliente de Azure: seleccionar Aprobar o Denegar](./media/customer-lockbox-overview/customer-lockbox-approval.png)

    Como resultado de la selección:
    - **Aprobar**:  Se concede acceso al ingeniero de Microsoft. El acceso se concede durante un período predeterminado de ocho horas.
    - **Denegar**: La solicitud de acceso con privilegios elevados del ingeniero de Microsoft se rechaza y no se realiza ninguna otra acción.

Con fines de auditoría, las acciones realizadas en este flujo de trabajo se registran en los [registros de solicitud de la Caja de seguridad del cliente](#auditing-logs).

## <a name="auditing-logs"></a>Registros de auditoría

Los registros de la Caja de seguridad del cliente se almacenan en los registros de actividad. En Azure Portal, seleccione **Registros de actividad** para ver la información de auditoría relacionada con las solicitudes de la Caja de seguridad del cliente. Puede filtrar acciones específicas, tales como:
- **Denegar la solicitud de la caja de seguridad**
- **Crear la solicitud de la caja de seguridad**
- **Aprobar la solicitud de la caja de seguridad**
- **Expiración de la solicitud de la caja de seguridad**

Por ejemplo:

![Caja de seguridad del cliente de Azure: registros de actividad](./media/customer-lockbox-overview/customer-lockbox-activitylogs.png)

## <a name="customer-lockbox-integration-with-azure-security-benchmark"></a>Integración de Caja de seguridad del cliente con la prueba comparativa de seguridad de Azure

Hemos introducido un nuevo control de línea base ([3.13](../benchmarks/security-control-identity-access-control.md#313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios)) en las pruebas comparativas de seguridad de Azure que cubren la aplicabilidad de Caja de seguridad del cliente. Los clientes ahora pueden aprovechar las pruebas comparativas para revisar la aplicabilidad de Caja de seguridad del cliente para un servicio.

## <a name="exclusions"></a>Exclusiones

Las solicitudes de la Caja de seguridad del cliente no se activan en los siguientes escenarios del soporte técnico de ingeniería:

- Un ingeniero de Microsoft debe realizar una actividad que esté fuera de los procedimientos operativos estándar. Por ejemplo, para recuperar o restaurar servicios en escenarios inesperados o impredecibles.

- Un ingeniero de Microsoft accede a la plataforma de Azure como parte de la solución de problemas y, sin darse cuenta, obtiene acceso a los datos del cliente. Por ejemplo, el equipo de Azure Network realiza la resolución de problemas que resulta en una captura de paquetes en un dispositivo de red. Sin embargo, si el cliente cifró los datos mientras estaba en tránsito, el ingeniero no podrá leerlos.

## <a name="next-steps"></a>Pasos siguientes

La Caja de seguridad del cliente está disponible automáticamente para todos los clientes que tengan un [plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/) con un nivel mínimo de tipo **Desarrollador**.

Cuando tenga un plan de soporte técnico adecuado, no tendrá que hacer nada para habilitar la Caja de seguridad del cliente. Las solicitudes de la Caja de seguridad del cliente de seguridad del cliente las iniciará un ingeniero de Microsoft si esta acción es necesaria para poder seguir con una incidencia de soporte técnico que archive alguien de su organización.
