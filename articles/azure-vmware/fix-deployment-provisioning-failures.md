---
title: Obtención de ayuda con los errores de implementación o aprovisionamiento de Azure VMware Solution
description: Cómo obtener la información necesaria de la nube privada de Azure VMware Solution para realizar una solicitud de servicio para los errores de aprovisionamiento o implementación de Azure VMware Solution.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 1f46dde895db417fd2b488a6203d5482e73d3c5e
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92779499"
---
# <a name="get-help-with-azure-vmware-solution-deployment-or-provisioning-failures"></a>Obtención de ayuda con los errores de implementación o aprovisionamiento de Azure VMware Solution

Este artículo le ayuda con los errores de implementación y aprovisionamiento de Azure VMware Solution. Cuando se produzcan errores en la nube privada, deberá abrir una [solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support) en Azure Portal. 

Sin embargo, primero deberá recopilar información importante en Azure Portal:

- Id. de correlación
- Id. de circuito ExpressRoute

## <a name="collect-the-correlation-id"></a>Recopilación del id. de correlación
 
Se genera un id. de correlación cuando crea una nube privada o cualquier recurso en Azure. Cada implementación de Azure Resource Manager también genera un id. de correlación. Este id. permite crear y solucionar más rápido una solicitud de servicio. 
 
Este es un ejemplo de la salida de una implementación de nube privada con errores, con el id. de correlación resaltado.

:::image type="content" source="media/fix-deployment-provisioning-failures/failed-private-cloud-deployment.png" alt-text="Error de implementación de nube privada con id. de correlación.":::

Copie y guarde este id. de correlación para incluirlo en la solicitud de servicio. Para obtener más información, consulte [Creación de la solicitud de soporte técnico](#create-your-support-request) al final de este artículo.

Si el error se produce en las fases previas a la validación, no se generará ningún id. de correlación. En este caso, puede proporcionar la información que usó al crear la nube privada de Azure VMware Solution, que incluye:

- Location
- Resource group
- Nombre del recurso
 
### <a name="collect-a-summary-of-errors"></a>Recopilación de un resumen de errores

Los detalles de todos los errores también pueden ayudar a solucionar el problema. En la pantalla anterior, seleccione el mensaje de advertencia para ver un resumen de los errores.
 
 :::image type="content" source="media/fix-deployment-provisioning-failures/summary-of-errors.png" alt-text="Resumen de errores.":::

De nuevo, copie y guarde este resumen para incluirlo en la solicitud de servicio.
 
### <a name="retrieve-past-deployments"></a>Recuperación de las implementaciones anteriores

Puede recuperar las implementaciones anteriores, incluidas las erróneas, al realizar una búsqueda en el registro de actividad de las implementaciones al que se tiene acceso al seleccionar el icono de notificaciones.

:::image type="content" source="media/fix-deployment-provisioning-failures/open-notifications.png" alt-text="Abrir las notificaciones.":::

En Notificaciones, seleccione **More events in the activity log** (Más eventos en el registro de actividad).

:::image type="content" source="media/fix-deployment-provisioning-failures/more-events-in-activity-log.png" alt-text="Vínculo: Más eventos en el registro de actividad.":::

A continuación, busque el nombre del recurso, u otros datos usados para crearlo, para buscar la implementación con errores y su id. de correlación. En el ejemplo siguiente se muestran los resultados de la búsqueda en un recurso de la nube privada (pc03).
 
:::image type="content" source="media/fix-deployment-provisioning-failures/find-past-deployments.png" alt-text="Búsqueda de las implementaciones de Azure VMware Solution anteriores con errores.":::
 
Al seleccionar el nombre de operación de la implementación con errores se abre una ventana con detalles. Seleccione la pestaña JSON y busque el elemento correlationId. Copie la información e inclúyala en la solicitud de servicio. 
 
## <a name="collect-the-expressroute-id-uri"></a>Recopilación del id. de ExpressRoute (URI)
 
Quizá intentó escalar o emparejar una nube privada existente con el circuito ExpressRoute de la nube privada y se produjo un error. En ese caso, necesitará el id. de ExpressRoute. 

En Azure Portal, seleccione **Conectividad > ExpressRoute** y copie el **Id. de ExpressRoute** en el Portapapeles.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/expressroute-id.png" alt-text="Copia del identificador de ExpressRoute en el portapapeles."::: 
 
> [!NOTE]
> En ocasiones, es posible que se produzcan errores en las comprobaciones anteriores a la implementación y que la única información disponible sea el error y los mensajes de error. Estos pueden ser útiles con muchos errores (por ejemplo, con los problemas relacionados con la cuota) y es importante incluir estos mensajes en la solicitud de soporte técnico. Para recopilarlos, consulte la sección anterior. [Recopilación de un resumen de errores](#collect-a-summary-of-errors).

## <a name="create-your-support-request"></a>Creación de una solicitud de soporte técnico

Para obtener instrucciones generales sobre cómo crear una solicitud de soporte técnico, consulte [Creación de una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md). 

Esta es una guía específica para la creación de una solicitud de servicio para errores de implementación o aprovisionamiento de Azure VMware Solution.

1. Seleccione el icono **Ayuda** y, a continuación, **+ Nueva solicitud de soporte técnico**.

    :::image type="content" source="media/fix-deployment-provisioning-failures/open-sr-on-avs.png" alt-text="Recopilación de un id. de ExpressRoute para la solicitud de servicio.":::

2. Rellene todos los campos obligatorios y, en la pestaña **Aspectos básicos** :

    - En **Tipo de problema** , seleccione **Configuration and Setup Issues** (Problemas de instalación y configuración).

    - En **Subtipo de problema** , seleccione **Aprovisionar una nube privada**.

3. Haga clic en la pestaña **Detalles** :

    - Rellene todos los campos obligatorios.

    - Pegue el id. de correlación o el id. de ExpressRoute en los campos específicos proporcionados. Si no se muestra un campo específico, puede pegarlos en el cuadro de texto bajo la instrucción **Proporcione detalles sobre el problema.**

    - Pegue todos los detalles del error, incluido el resumen de errores que copió, en el cuadro de texto debajo de la instrucción **Proporcione detalles sobre el problema.**

4. Revise la información y seleccione **Crear** para crear la solicitud de servicio.
