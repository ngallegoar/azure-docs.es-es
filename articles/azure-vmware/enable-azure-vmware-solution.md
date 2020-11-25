---
title: Habilitación del recurso de Azure VMware Solution
description: Obtenga información sobre cómo enviar una solicitud de soporte técnico para habilitar el recurso de Azure VMware Solution. También puede solicitar más nodos en la nube privada de Azure VMware Solution existente.
ms.topic: how-to
ms.date: 11/12/2020
ms.openlocfilehash: c9c31d8ffbf62a511f4f2ab19f995b6bc0ee0ca4
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695153"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Habilitación del recurso de Azure VMware Solution
Obtenga información sobre cómo una solicitud de soporte técnico para habilitar el recurso de [Azure VMware Solution](introduction.md). También puede solicitar más nodos en la nube privada de Azure VMware Solution existente.

## <a name="eligibility-criteria"></a>Criterios de elegibilidad

Necesitará una cuenta de Azure en una suscripción a Azure. La suscripción a Azure debe cumplir uno de los siguientes criterios:

* Una suscripción bajo un [Contrato Enterprise (EA) de Azure](../cost-management-billing/manage/ea-portal-agreements.md) con Microsoft.
* Una suscripción administrada por el Proveedor de soluciones en la nube (CSP) bajo un plan de Azure.


## <a name="enable-azure-vmware-solution-for-ea-customers"></a>Habilitación de Azure VMware Solution para clientes de Contrato Enterprise
Antes de crear el recurso de Azure VMware Solution, debe enviar una incidencia de soporte técnico para que se asignen los nodos. Una vez que el equipo de soporte técnico recibe su solicitud, se tarda hasta cinco días laborables en confirmar su solicitud y asignar los nodos. Si tiene una nube privada de Azure VMware Solution existente y desea asignar más nodos, repasará el mismo proceso.


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
   - Número de nodos
   - Cualquier otro detalle

   >[!NOTE]
   >Azure VMware Solution recomienda un mínimo de tres nodos para poner en marcha su nube privada y para la redundancia de N+1 nodos. 

1. Seleccione **Revisar + crear** para enviar la solicitud.

   Un representante de soporte técnico tardará hasta cinco días hábiles en confirmar su solicitud.

   >[!IMPORTANT] 
   >Si ya tiene una instancia de Azure VMware Solution existente y solicita más nodos, tenga en cuenta que necesitamos cinco días laborables para asignar los nodos. 

1. Antes de poder aprovisionar los nodos, asegúrese de registrar el proveedor de recursos **Microsoft.AVS** en Azure Portal.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Para ver otras formas de registrar el proveedor de recursos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="enable-azure-vmware-solution-for-csp-customers"></a>Habilitación de Azure VMware Solution para clientes de CSP 

Los CSP deben usar el [Centro de partners de Microsoft](https://partner.microsoft.com) para habilitar Azure VMware Solution para sus clientes. 

   >[!IMPORTANT] 
   >El servicio de Azure VMware Solution no proporciona un entorno multiinquilino y, por lo tanto, no se admiten asociados de hospedaje. 

1. En el **Centro de partners**, seleccione **CSP** para acceder al área **Clientes**.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Centro de partners de Microsoft, área de cliente" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::

1. Seleccione el cliente y, a continuación, seleccione **Agregar productos**.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Centro de partners de Microsoft" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::

1. Seleccione **Plan de Azure** y, después, seleccione **Agregar al carro**. 

1. Revise y finalice la configuración general de la suscripción al plan de Azure para su cliente. Para más información, consulte la [documentación del Centro de partners de Microsoft](https://docs.microsoft.com/partner-center/azure-plan-manage).

Después de configurar el plan de Azure y de asignar los permisos de RBAC necesarios como CSP, interactuará con Microsoft para habilitar la cuota de una suscripción al plan de Azure. Acceda a Azure Portal desde el Centro de partners mediante el procedimiento **Admin On Behalf Of** (Administrar en nombre de) (AOBO).

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
   - Número de nodos
   - Cualquier otro detalle
   - ¿Está pensado para hospedar varios clientes?

   >[!NOTE]
   >Azure VMware Solution recomienda un mínimo de tres nodos para poner en marcha su nube privada y para la redundancia de N+1 nodos. 

1. Seleccione **Revisar + crear** para enviar la solicitud.

   Un representante de soporte técnico tardará hasta cinco días hábiles en confirmar su solicitud.

   >[!IMPORTANT] 
   >Si ya tiene una instancia de Azure VMware Solution existente y solicita más nodos, tenga en cuenta que necesitamos cinco días laborables para asignar los nodos. 

1. Una vez que se haya agregado al plan de Azure y tenga la opción de cuota habilitada, el cliente o el administrador de asociados pueden implementar una nube privada de Azure VMware Solution a través de Azure Portal. Antes de poder aprovisionar los nodos, asegúrese de registrar el proveedor de recursos **Microsoft.AVS** en Azure Portal.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Para ver otras formas de registrar el proveedor de recursos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Pasos siguientes

Después de habilitar el recurso de Azure VMware Solution y una vez que la red adecuada esté configurada, puede [crear una nube privada](tutorial-create-private-cloud.md).