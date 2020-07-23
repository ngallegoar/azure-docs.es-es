---
title: Configuración de la caja de seguridad para Azure Data Box
description: Obtenga información sobre cómo usar Caja de seguridad del cliente con Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.topic: how-to
ms.date: 07/10/2020
ms.author: alkohli
ms.subservice: pod
ms.openlocfilehash: 9c95760d03db976b59537adcecbe39a942b72126
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209119"
---
# <a name="use-customer-lockbox-for-azure-data-box-preview"></a>Uso de Caja de seguridad del cliente para Azure Data Box (versión preliminar)

Azure Data Box se usa para transferir datos de clientes con Azure como origen y destino. Hay situaciones en las que el Soporte técnico de Microsoft podría requerir acceso a los datos del cliente durante una solicitud de soporte técnico. Puede usar Caja de seguridad del cliente como interfaz para revisar y aprobar o rechazar dichas solicitudes de acceso a los datos. 

Este artículo trata sobre cómo se inician y siguen las solicitudes de la Caja de seguridad del cliente para los pedidos de importación y exportación de Data Box. La información del artículo se aplica tanto a los dispositivos de Azure Data Box como a los de Azure Data Box Heavy. 

## <a name="devops-workflow-for-data-access"></a>Flujo de trabajo de DevOps para el acceso a datos

El equipo de operaciones de soporte técnico y Data Box de Microsoft no suele tener acceso a los datos del cliente. Tratan de resolver los problemas mediante herramientas estándar y telemetría. <!--The only scenarios where there is a need to access customer data is when there is an issue with the data that needs to be fixed. For example, if the data is copied to a wrong folder or is in an incorrect format and is likely to result in an upload or download failure, then Microsoft will try to access your data in the Azure datacenter.--> 

Si los problemas no se pueden resolver y requieren que el Soporte técnico de Microsoft investigue o repare los datos, se solicita acceso con privilegios elevados a través del portal Just-in-Time (JIT). El portal de JIP valida el nivel de permiso, proporciona autenticación multifactor y también incluye una aprobación de los aprobadores internos de Microsoft. Por ejemplo, el aprobador podría ser DevOps Manager. 

Una vez que la solicitud de acceso con privilegios elevados se aprueba a través del portal JIT, si ha habilitado la Caja de seguridad, Microsoft también solicitará su consentimiento explícito para acceder a los datos. El acceso se solicita y supervisa a través del servicio de Caja de seguridad del cliente en el portal. 

Si no ha habilitado la Caja de seguridad, su consentimiento no es necesario para acceder a los datos.


## <a name="prerequisites-for-access-request"></a>Requisitos previos para la solicitud de acceso

Antes de comenzar, asegúrese de que:

1. Ha creado un orden de Azure Data Box según las instrucciones de:
    1. [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-ordered.md) para los pedidos de importación.
    1. [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-export-ordered.md) para los pedidos de exportación.

2. Ha configurado Caja de seguridad del cliente para Data Box. Se trata de un servicio opcional. 

    1. Caja de seguridad del cliente está actualmente en versión preliminar para el servicio Data Box. Para habilitar Caja de seguridad del cliente para Data Box para su organización, regístrese en la [versión preliminar pública de Azure de Caja seguridad del cliente](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Kwz02N6XVCoKNpxIpqE_hUNzlTUUNYVkozOVlFNVRSWDVHRkkwTFQyViQlQCN0PWcu).
    2. La Caja de seguridad del cliente está disponible automáticamente para todos los clientes que tengan un plan de Soporte técnico de Azure con un nivel mínimo de tipo Desarrollador. <!--How do you enable Lockbox? change this for Azure Data Box, perhaps you need a different support plan When you have an eligible support plan, no action is required by you to enable Customer Lockbox. Customer Lockbox requests are initiated by a Microsoft engineer if this action is needed to progress a support ticket that is filed from somebody in your organization.-->

3. Ya se abrió una solicitud de servicio o una incidencia de soporte técnico para este problema. Para obtener información sobre la incidencia de soporte técnico, consulte [Envío de una solicitud de servicio para Data Box](data-box-disk-contact-microsoft-support.md).


## <a name="track-approve-request-via-lockbox"></a>Seguimiento y aprobación de una solicitud a través de Caja de seguridad

Para realizar el seguimiento y aprobación de una solicitud de acceso a los datos del cliente, siga estos pasos:

1. Microsoft detecta que hay un problema durante la carga o descarga de los datos en el centro de datos de Azure. Por ejemplo, el pedido de Data Box se detiene durante la fase de **copia de datos**. 

    El ingeniero de soporte técnico se conecta a Data Box a través de la sesión de soporte técnico e intenta solucionar el problema con las herramientas estándar y la telemetría. Si los discos de Data Box están bloqueados y no se puede acceder a los recursos compartidos, el ingeniero de soporte técnico crea una solicitud de Caja de seguridad. 
 
2. Cuando se crea la solicitud, normalmente la notificación se envía al administrador de la suscripción, pero también puede configurar un grupo para las notificaciones. 

3. Puede ver la solicitud de la caja de seguridad en Azure Portal para aprobarla. 

    ![Solicitud en Azure Portal](./media/data-box-customer-lockbox/3-lockbox-request-azure-portal.png)

    Para aprobar la solicitud de la caja de seguridad desde el portal, puede seleccionar la opción **Aprobar**.

    ![Aprobar la solicitud](./media/data-box-customer-lockbox/4-lockbox-request-details-azure-portal.png)


    Una vez aprobada la solicitud, los discos del dispositivo se desbloquean y se puede acceder a los recursos compartidos en la sesión de soporte técnico.

4. El ingeniero de soporte técnico resuelve el problema de carga y, a continuación, deshabilita la sesión de soporte técnico.

Una vez resuelto el problema, el trabajo de copia de datos continúa hasta su finalización.


## <a name="next-steps"></a>Pasos siguientes

- [Caja de seguridad del cliente de Microsoft Azure](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

<!--- [Approve, audit support access requests to VMs using Customer Lockbox for Azure](https://azure.microsoft.com/blog/approve-audit-support-access-requests-to-vms-using-customer-lockbox-for-azure/)-->

