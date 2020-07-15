---
title: Actualización automática de Mobility Service en Azure Site Recovery
description: Descripción general de la actualización automática de Mobility Service al replicar VM de Azure mediante Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/02/2020
ms.author: rajanaki
ms.openlocfilehash: b57ce89979225015dc87bbfb17f9603897ef6d6b
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135836"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Actualización automática de Mobility Service en Azure para la replicación de Azure

Azure Site Recovery usa una cadencia de lanzamiento mensual para solucionar cualquier problema y mejorar las funciones existentes o agregar otras nuevas. Para mantenerse actualizado con el servicio, debe planear la implementación de revisiones cada mes. Para evitar la sobrecarga asociada con cada actualización, puede permitir que Site Recovery administre las actualizaciones de los componentes.

Tal como se menciona en la [arquitectura de recuperación ante desastres de Azure a Azure](azure-to-azure-architecture.md), el servicio Mobility se instala en todas las máquinas virtuales de Azure (VM) que tienen habilitada la replicación desde una región de Azure a otra. Cuando usa actualizaciones automáticas, cada nueva versión actualiza la extensión de Mobility Service.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-automatic-updates-work"></a>Cómo funcionan las actualizaciones automáticas

Cuando usa Site Recovery para administrar las actualizaciones, se implementa un runbook global (que usan los servicios de Azure) a través de una cuenta de automatización creada en la misma suscripción que el almacén. Cada almacén usa una cuenta de automatización. Para cada máquina virtual de un almacén, el runbook comprueba si hay actualizaciones automáticas activas. Si hay disponible una versión más reciente de la extensión del servicio Mobility, se instalará la actualización.

La programación predeterminada del runbook se produce diariamente a las 12:00 a. m. en la zona horaria de la geografía de la máquina virtual replicada. También puede cambiar el horario del runbook a través de la cuenta de automatización.

> [!NOTE]
> Si comienza con el [paquete acumulativo de actualizaciones 35](site-recovery-whats-new.md#updates-march-2019), puede elegir una cuenta de automatización existente que puede usar para las actualizaciones. Antes del paquete acumulativo de actualizaciones 35, Site Recovery creó la cuenta de automatización de forma predeterminada. Solo puede seleccionar esta opción al habilitar la replicación para una máquina virtual. No está disponible para una máquina virtual que ya tenga habilitada la replicación. Los valores que seleccione se aplicarán a todas las máquinas virtuales de Azure protegidas en el mismo almacén.

La activación de las actualizaciones automáticas no requiere reiniciar las VM de Azure ni afecta a la replicación en curso.

La facturación de trabajos en la cuenta de automatización se basa en la cantidad de minutos de runtime de los trabajos usados en un mes. La ejecución del trabajo toma entre unos segundos y un minuto cada día aproximadamente, y se establece como unidades gratuitas. De forma predeterminada, se incluyen 500 minutos como unidades gratuitas en una cuenta de automatización, como se muestra en la tabla siguiente:

| Unidades gratuitas incluidas (cada mes) | Price |
|---|---|
| Tiempo de trabajo de 500 minutos | ₹0,14/minuto

## <a name="enable-automatic-updates"></a>Habilitar las actualizaciones automáticas

Site Recovery puede administrar las actualizaciones de la extensión de varias maneras:

- [Administrar como parte del paso para habilitar la replicación](#manage-as-part-of-the-enable-replication-step)
- [Alternar la configuración de actualización de la extensión dentro del almacén.](#toggle-the-extension-update-settings-inside-the-vault)
- [Administrar actualizaciones manualmente](#manage-updates-manually)

### <a name="manage-as-part-of-the-enable-replication-step"></a>Administrar como parte del paso para habilitar la replicación

Cuando habilita la replicación para una VM iniciándola [desde la vista de la VM](azure-to-azure-quickstart.md) o [desde el almacén de Recovery Services](azure-to-azure-how-to-enable-replication.md), puede permitir que Site Recovery administre las actualizaciones para la extensión de Site Recovery o puede administrarlas manualmente.

:::image type="content" source="./media/azure-to-azure-autoupdate/enable-rep.png" alt-text="Configuración de extensiones":::

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Alternar la configuración de actualización de la extensión dentro del almacén

1. En el almacén de Recovery Services, vaya a **Administrar** > **Infraestructura de Site Recovery**.
1. En **Para las máquinas virtuales de Azure** > **Configuración de actualización de extensiones** > **Permitir la administración por parte de Site Recovery**, seleccione **Activado**.

   Para administrar la extensión manualmente, seleccione **Desactivado**.

1. Seleccione **Guardar**.

:::image type="content" source="./media/azure-to-azure-autoupdate/vault-toggle.png" alt-text="Configuración de actualización de extensiones":::

> [!IMPORTANT]
> Si elige la opción **Permitir la administración por parte de Site Recovery**, la configuración se aplica a todas las máquinas virtuales del almacén.

> [!NOTE]
> Cualquiera de las opciones le ofrece información de la cuenta de automatización que se usa para administrar las actualizaciones. Si está usando esta función en un almacén por primera vez, se creará una nueva cuenta de automatización de forma predeterminada. Alternativamente, puede personalizar la configuración y elegir una cuenta de automatización ya existente. Todas las tareas posteriores para habilitar la replicación en el mismo almacén usarán la cuenta de automatización que se creó anteriormente. Actualmente, en el menú desplegable solo se muestran las cuentas de automatización que se encuentran en el mismo grupo de recursos que el almacén.

### <a name="manage-updates-manually"></a>Administrar actualizaciones manualmente

1. Si hay nuevas actualizaciones para la instancia de Mobility Service instalada en sus VM, verá la siguiente notificación: **Hay una nueva actualización del agente de replicación de Site Recovery disponible. Haga clic para instalarla.**

   :::image type="content" source="./media/vmware-azure-install-mobility-service/replicated-item-notif.png" alt-text="Ventana Elementos replicados":::

1. Seleccione la notificación para abrir la página de selección de VM.
1. Elija las VM que quiera actualizar y luego seleccione **Aceptar**. El servicio de actualización de Mobility Service se iniciará por cada VM seleccionada.

   :::image type="content" source="./media/vmware-azure-install-mobility-service/update-okpng.png" alt-text="Lista de máquinas virtuales de elementos replicados":::

## <a name="common-issues-and-troubleshooting"></a>Problemas habituales y soluciones

Si hay un problema con las actualizaciones automáticas, verá una notificación de error en la opción de **problemas de configuración** del panel del almacén.

Si no puede habilitar las actualizaciones automáticas, consulte los siguientes errores comunes y acciones recomendadas:

- **Error**: No tiene permisos para crear una cuenta de ejecución de Azure (entidad de servicio) ni para conceder el rol Colaborador a la entidad de servicio.

  **Acción recomendada**: Asegúrese de que la cuenta de inicio de sesión esté asignada como Colaborador e inténtelo nuevamente. Para más información acerca de la asignación de permisos, consulte la sección sobre permisos necesarios de [Procedimientos: portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app).

  Para solucionar la mayoría de los problemas después de habilitar las actualizaciones automáticas, seleccione **Reparar**. Si el botón de reparación no está disponible, consulte el mensaje de error que se muestra en el panel de configuración de la actualización de la extensión.

  :::image type="content" source="./media/azure-to-azure-autoupdate/repair.png" alt-text="Botón de reparación del servicio de Site Recovery en la configuración de actualización de la extensión":::

- **Error**: la cuenta de ejecución no tiene permiso para obtener acceso al recurso de Recovery Services.

  **Acción recomendada**: elimine y [vuelva a crear la cuenta de ejecución](../automation/manage-runas-account.md). O bien, asegúrese de que la aplicación de Azure Active Directory de la cuenta de ejecución de Automation puede acceder al recurso de Recovery Services.

- **Error**: no se encuentra la cuenta de ejecución. Es posible que la huella digital del certificado y la conexión no sea idéntica, o que alguno de los elementos siguientes se haya eliminado o no se haya creado: la aplicación Azure Active Directory, la entidad de servicio, el rol, el recurso de certificado de Automation o el recurso de conexión de Automation.

  **Acción recomendada**: elimine y [vuelva a crear la cuenta de ejecución](../automation/manage-runas-account.md).

- **Error**: La ejecución de Azure como un certificado que usa la cuenta de automatización está a punto de caducar.

  El certificado autofirmado que creó para la cuenta de ejecución expira en un año a partir de la fecha de creación. Se puede renovar en cualquier momento antes de que expire. Si se ha registrado para recibir notificaciones por correo electrónico, también recibirá correos electrónicos cuando deba realizar cualquier una acción. Este error se mostrará dos meses antes de la fecha de expiración y cambiará a un error crítico si el certificado ha expirado. Una vez que el certificado haya expirado, la actualización automática no funcionará hasta que renueve el mismo.

  **Acción recomendada**: Haga clic en **Reparar** y luego en **Renovar certificado** para resolver este problema.

  :::image type="content" source="./media/azure-to-azure-autoupdate/automation-account-renew-runas-certificate.PNG" alt-text="renew-cert":::

  > [!NOTE]
  > Después de renovar el certificado, actualice la página para mostrar el estado actual.
