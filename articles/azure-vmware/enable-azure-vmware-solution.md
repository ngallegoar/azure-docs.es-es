---
title: Habilitación del recurso de Azure VMware Solution
description: Obtenga información sobre cómo enviar una solicitud de soporte técnico para habilitar el recurso de Azure VMware Solution. También puede solicitar más hosts en la nube privada de Azure VMware Solution existente.
ms.topic: how-to
ms.date: 11/12/2020
ms.openlocfilehash: 526d6b38f4b4e3f6c4806b71b4728dee90cf558a
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96325084"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Habilitación del recurso de Azure VMware Solution
Obtenga información sobre cómo una solicitud de soporte técnico para habilitar el recurso de [Azure VMware Solution](introduction.md). También puede solicitar más hosts en la nube privada de Azure VMware Solution existente.

## <a name="eligibility-criteria"></a>Criterios de elegibilidad

Necesitará una cuenta de Azure en una suscripción a Azure. La suscripción a Azure debe cumplir uno de los siguientes criterios:

* Una suscripción bajo un [Contrato Enterprise (EA) de Azure](../cost-management-billing/manage/ea-portal-agreements.md) con Microsoft.
* Una suscripción administrada por el Proveedor de soluciones en la nube (CSP) bajo un plan de Azure.


## <a name="enable-azure-vmware-solution-for-ea-customers"></a>Habilitación de Azure VMware Solution para clientes de Contrato Enterprise
Antes de crear el recurso de Azure VMware Solution, debe enviar una incidencia de soporte técnico para que se asignen los hosts. Una vez que el equipo de soporte técnico recibe su solicitud, tardan hasta cinco días laborables en confirmarla y asignar los hosts. Si tiene una nube privada de Azure VMware Solution y quiere asignar más hosts, pasará por el mismo proceso.


1. En Azure Portal, en **Ayuda y soporte técnico**, cree una **[solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support)** y proporcione la siguiente información para el vale:
   - **Tipo de problema**: Requisitos previos técnicos
   - **Subscription** (Suscripción): Seleccione su suscripción.
   - **Servicio:** Todos los servicios > Azure VMware Solution
   - **Recurso:** Pregunta general 
   - **Resumen:** Capacidad necesitada
   - **Tipo de problema**: problemas de administración de la capacidad.
   - **Subtipo de problema:** solicitud de cliente de capacidad o cuota de host adicional.

1. En la **descripción** de la incidencia de soporte técnico, en la pestaña **Detalles**, proporcione la siguiente información:

   - POC o producción 
   - Nombre de región
   - Número de hosts
   - Cualquier otro detalle

   >[!NOTE]
   >Azure VMware Solution recomienda como mínimo tres hosts para poner en marcha la nube privada y N+1 hosts para la redundancia. 

1. Seleccione **Revisar + crear** para enviar la solicitud.

   Un representante de soporte técnico tardará hasta cinco días hábiles en confirmar su solicitud.

   >[!IMPORTANT] 
   >Si ya tiene una instancia de Azure VMware Solution y solicita más hosts, tenga en cuenta que necesitamos cinco días laborables para asignarlos. 

1. Antes de aprovisionar los hosts, asegúrese de registrar el proveedor de recursos **Microsoft.AVS** en Azure Portal.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Para ver otras formas de registrar el proveedor de recursos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="enable-azure-vmware-solution-for-csp-customers"></a>Habilitación de Azure VMware Solution para clientes de CSP 

Los CSP deben usar el [Centro de partners de Microsoft](https://partner.microsoft.com) para habilitar Azure VMware Solution para sus clientes. 

   >[!IMPORTANT] 
   >El servicio de Azure VMware Solution no proporciona el multiinquilino necesario. Los asociados de hospedaje que requieren esta función no se admiten. 

1. En el **Centro de partners**, seleccione **CSP** para acceder al área **Clientes**.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Centro de partners de Microsoft, área de cliente" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::

1. Seleccione el cliente y, a continuación, seleccione **Agregar productos**.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Centro de partners de Microsoft" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::

1. Seleccione **Plan de Azure** y, después, seleccione **Agregar al carro**. 

1. Revise y finalice la configuración general de la suscripción al plan de Azure para su cliente. Para más información, consulte la [documentación del Centro de partners de Microsoft](/partner-center/azure-plan-manage).

Después de configurar el plan de Azure y de asignar los permisos de RBAC de vSphere necesarios como CSP, interactuará con Microsoft para habilitar la cuota de una suscripción al plan de Azure. Acceda a Azure Portal desde el Centro de partners mediante el procedimiento **Admin On Behalf Of** (Administrar en nombre de) (AOBO).

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com).

1. Seleccione **CSP** para acceder al área **Clientes**.

1. Expanda los detalles del cliente y seleccione **Portal de administración de Microsoft Azure**.

1. En Azure Portal, en **Ayuda y soporte técnico**, cree una **[solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support)** y proporcione la siguiente información para el vale:
   - **Tipo de problema**: Requisitos previos técnicos
   - **Subscription** (Suscripción): Seleccione su suscripción.
   - **Servicio:** Todos los servicios > Azure VMware Solution
   - **Recurso:** Pregunta general 
   - **Resumen:** Capacidad necesitada
   - **Tipo de problema**: problemas de administración de la capacidad.
   - **Subtipo de problema:** solicitud de cliente de capacidad o cuota de host adicional.

1. En la **descripción** de la incidencia de soporte técnico, en la pestaña **Detalles**, proporcione la siguiente información:

   - POC o producción 
   - Nombre de región
   - Número de hosts
   - Cualquier otro detalle
   - ¿Está pensado para hospedar varios clientes?

   >[!NOTE]
   >Azure VMware Solution recomienda como mínimo tres hosts para poner en marcha la nube privada y N+1 hosts para la redundancia. 

1. Seleccione **Revisar + crear** para enviar la solicitud.

   Un representante de soporte técnico tardará hasta cinco días hábiles en confirmar su solicitud.

   >[!IMPORTANT] 
   >Si ya tiene una instancia de Azure VMware Solution y solicita más hosts, tenga en cuenta que necesitamos cinco días laborables para asignarlos. 

1. Si el proveedor de servicios administra la suscripción, el equipo de administración debe acceder a Azure Portal nuevamente mediante el procedimiento **Administrar en nombre de** (AOBO) desde el Centro de partners. Una vez en Azure Portal, inicie una instancia de [Cloud Shell](../cloud-shell/overview.md), registre al proveedor de recursos **Microsoft.AVS** y continúe con la implementación de la nube privada de Azure VMware Solution.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Para ver otras formas de registrar el proveedor de recursos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

1. Si el cliente administra directamente la suscripción, el registro del proveedor de recursos **Microsoft. AVS** debe realizarlo un usuario con permisos suficientes en la suscripción. Consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md) para obtener más detalles y conocer más formas de registrar el proveedor de recursos. 


## <a name="next-steps"></a>Pasos siguientes

Después de habilitar el recurso de Azure VMware Solution y una vez que la red adecuada esté configurada, puede [crear una nube privada](tutorial-create-private-cloud.md).