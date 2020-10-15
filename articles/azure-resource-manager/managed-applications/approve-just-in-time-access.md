---
title: Aprobación de acceso Just-In-Time
description: Describe cómo los consumidores de Azure Managed Applications aprueban las solicitudes de acceso Just-In-Time a una aplicación administrada.
author: MSEvanhi
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 4a3604a3256fb2d3f4253891bbc28c7685748cd2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75649372"
---
# <a name="configure-and-approve-just-in-time-access-for-azure-managed-applications"></a>Configuración y aprobación del acceso Just-In-Time para Azure Managed Applications

Como consumidor de una aplicación administrada, puede que no se sienta cómodo proporcionando al editor acceso permanente al grupo de recursos administrado. Para ofrecerle mayor control sobre la concesión de acceso a los recursos administrados, Azure Managed Applications proporciona una característica denominada acceso Just-In-Time (JIT), que se encuentra actualmente en versión preliminar. Le permite aprobar cuándo y durante cuánto tiempo el editor tiene acceso al grupo de recursos. El editor puede realizar las actualizaciones necesarias durante ese tiempo, tras el cual el acceso del editor expirará.

El flujo de trabajo de concesión de acceso es el siguiente:

1. El editor agrega una aplicación administrada en Marketplace y especifica que el acceso JIT esté disponible.

1. Durante la implementación, permite el acceso JIT a esa instancia de la aplicación administrada.

1. Después de la implementación, puede cambiar la configuración del acceso JIT.

1. El editor envía una solicitud de acceso.

1. Usted aprueba la solicitud.

Este artículo se centra en las acciones que los consumidores llevan a cabo para habilitar el acceso JIT y aprobar solicitudes. Para obtener información sobre la publicación de una aplicación administrada con acceso JIT, consulte el tema sobre la [solicitud de acceso Just-In-Time en Azure Managed Applications](request-just-in-time-access.md).

> [!NOTE]
> Para usar el acceso Just-In-Time, debe tener una [licencia de Azure Active Directory P2](../../active-directory/privileged-identity-management/subscription-requirements.md).

## <a name="enable-during-deployment"></a>Habilitar durante la implementación

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque una entrada de Marketplace para una aplicación administrada con JIT habilitado. Seleccione **Crear**.

1. Al proporcionar valores para la nueva aplicación administrada, el paso **Configuración de JIT** le permite habilitar o deshabilitar el acceso JIT para la aplicación administrada. Seleccione **Sí** en **Habilitar el acceso JIT**. Esta opción está seleccionada de forma predeterminada para las aplicaciones administradas que se han definido con JIT habilitado en Marketplace.

   ![Configurar el acceso](./media/approve-just-in-time-access/configure-jit-access.png)

   Solo puede habilitar el acceso JIT durante la implementación. Si selecciona **No**, el editor obtiene acceso permanente al grupo de recursos administrado. No puede habilitar el acceso JIT posteriormente.

1. Para cambiar la configuración de aprobación predeterminada, seleccione **Personalizar configuración JIT**.

   ![Personalizar el acceso](./media/approve-just-in-time-access/customize-jit-access.png)

   De forma predeterminada, una aplicación administrada con JIT habilitado tiene las siguientes opciones:

   * Modo de aprobación: automático
   * Duración máxima de acceso: 8 horas
   * Aprobadores: ninguno

   Cuando se establece el modo de aprobación en **automático**, los aprobadores reciben una notificación de cada solicitud, pero la solicitud se aprueba automáticamente. Cuando se establece en **manual**, los aprobadores reciben una notificación de cada solicitud, y uno de ellos debe aprobarla.

   La duración máxima de activación especifica la cantidad máxima de tiempo que un editor puede solicitar para acceder al grupo de recursos administrado.

   La lista de aprobadores contiene los usuarios de Azure Active Directory que pueden aprobar solicitudes de acceso JIT. Para agregar un aprobador, seleccione **Agregar aprobador** y busque el usuario.

   Después de actualizar la configuración, seleccione **Guardar**.

## <a name="update-after-deployment"></a>Actualizar después de la implementación

Puede cambiar los valores de aprobación de las solicitudes. Sin embargo, si no habilitó el acceso JIT durante la implementación, no puede habilitarlo posteriormente.

Para cambiar la configuración de una aplicación administrada implementada:

1. En el portal, seleccione la aplicación administrada.

1. Seleccione **Configuración de JIT** y cambie las opciones de configuración según sea necesario.

   ![Cambiar la configuración de acceso](./media/approve-just-in-time-access/change-settings.png)

1. Cuando haya terminado, seleccione **Guardar**.

## <a name="approve-requests"></a>Aprobar solicitudes

Cuando el editor solicite acceso, recibirá una notificación de la solicitud. Puede aprobar las solicitudes de acceso JIT directamente a través de la aplicación administrada, o a través de todas las aplicaciones administradas a través del servicio de Azure AD Privileged Identity Management. Para usar el acceso Just-In-Time, debe tener una [licencia de Azure Active Directory P2](../../active-directory/privileged-identity-management/subscription-requirements.md).

Para aprobar las solicitudes a través de la aplicación administrada:

1. Seleccione **Acceso de JIT** para la aplicación administrada y elija **Aprobar solicitudes**.

   ![Aprobar solicitudes](./media/approve-just-in-time-access/approve-requests.png)
 
1. Seleccione la solicitud que quiere aprobar.

   ![Seleccionar la solicitud](./media/approve-just-in-time-access/select-request.png)

1. En el formulario, proporcione el motivo de la aprobación y seleccione **Aprobar**.

Para aprobar las solicitudes a través de Azure AD Privileged Identity Management:

1. Seleccione **Todos los servicios** y comience a buscar **Azure AD Privileged Identity Management**. Seleccione el servicio entre las opciones disponibles.

   ![Buscar el servicio](./media/approve-just-in-time-access/search.png)

1. Seleccione **Aprobar solicitudes**.

   ![Seleccionar Aprobar solicitudes](./media/approve-just-in-time-access/select-approve-requests.png)

1. Seleccione **Aplicaciones administradas de Azure** y seleccione la solicitud que quiere aprobar.

   ![Seleccionar solicitudes](./media/approve-just-in-time-access/view-requests.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre la publicación de una aplicación administrada con acceso JIT, consulte el tema sobre la [solicitud de acceso Just-In-Time en Azure Managed Applications](request-just-in-time-access.md).
