---
title: Habilitación del recurso de Azure VMware Solution
description: Obtenga información sobre cómo enviar una solicitud de soporte técnico para habilitar el recurso de Azure VMware Solution. También puede solicitar más nodos en la nube privada de Azure VMware Solution existente.
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 72a7aca97067ce4e9ed0e901e4016c82b3549eb1
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817886"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Habilitación del recurso de Azure VMware Solution
Obtenga información sobre cómo enviar una solicitud de soporte técnico para habilitar el recurso de Azure VMware Solution. También puede solicitar más nodos en la nube privada de Azure VMware Solution existente.

## <a name="eligibility-criteria"></a>Criterios de elegibilidad

* Necesitará un [Contrato Enterprise (EA) de Azure](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-agreements) con Microsoft.
* Necesitará una cuenta de Azure en una suscripción a Azure.


## <a name="enable-azure-vmware-solution-resource"></a>Habilitación del recurso de Azure VMware Solution
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