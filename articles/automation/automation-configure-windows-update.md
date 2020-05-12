---
title: Configuración de Windows Update para trabajar con Azure Update Management
description: En este artículo se describen los parámetros de Windows Update que se configuran para trabajar con Azure Update Management.
services: automation
ms.subservice: update-management
ms.date: 05/04/2020
ms.topic: conceptual
ms.openlocfilehash: b9b5f2b19b29eae0132ec01a9f3fb7e8355361f5
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82779457"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Configuración de los parámetros de Windows Update para Update Management

Azure Update Management se basa en el [cliente de Windows Update](https://docs.microsoft.com//windows/deployment/update/windows-update-overview) para descargar e instalar las actualizaciones de Windows. Hay configuraciones específicas que el cliente de Windows Update utiliza al conectarse a Windows Server Update Services (WSUS) o Windows Update. Muchas de estas configuraciones se pueden administrar con:

- Editor de directivas de grupo local
- Directiva de grupo
- PowerShell
- Edición directa del Registro

Update Management respeta muchas de las configuraciones especificadas para controlar el cliente de Windows Update. Si usa la configuración para habilitar las actualizaciones que no son de Windows, Update Management también administrará dichas actualizaciones. Si quiere habilitar la descarga de actualizaciones antes de que se lleve a cabo una implementación de actualizaciones, las implementaciones de actualizaciones pueden ejecutarse de manera más rápida y eficaz, con menos probabilidades de exceder la ventana de mantenimiento.

Para encontrar recomendaciones adicionales sobre cómo configurar WSUS en la suscripción de Azure y mantener actualizadas las máquinas virtuales de Windows de forma segura, revise [Planeamiento de la implementación para actualizar máquinas virtuales Windows en Azure con WSUS](https://docs.microsoft.com/azure/architecture/example-scenario/wsus/).

## <a name="pre-download-updates"></a>Actualizaciones previas a la descarga

Para configurar la descarga automática de las actualizaciones, pero no instalarlas automáticamente, puede usar una directiva de grupo para establecer el valor de [Configurar actualizaciones automáticas](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) en **3**. Esta configuración habilita la descarga de las actualizaciones necesarias en segundo plano y le notifica que las actualizaciones están listas para instalarse. De este modo, Update Management mantiene el control de las programaciones, pero las actualizaciones se pueden descargar fuera de la ventana de mantenimiento de Update Management. Este comportamiento evita errores de tipo **Ventana de mantenimiento superada** en Update Management.

Puede habilitar esta configuración mediante PowerShell mediante la ejecución del siguiente comando:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>Configuración de las opciones de reinicio

Las claves del Registro que se enumeran en [Configuración de actualizaciones automáticas mediante la edición del Registro](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) y [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) pueden hacer que las máquinas se reinicien, incluso si ha especificado **No reiniciar nunca** en la configuración de **Implementación de actualizaciones**. Debe configurar estas claves del Registro para adaptarse a su entorno.

## <a name="enable-updates-for-other-microsoft-products"></a>Habilitar actualizaciones de otros productos de Microsoft

De forma predeterminada, el cliente de Windows Update está configurado para proporcionar actualizaciones solo para Windows. Si habilita la opción **Ofrecerme actualizaciones para otros productos de Microsoft cuando actualice Windows**, también recibirá actualizaciones para otros productos, incluidas las revisiones de seguridad para Microsoft SQL Server u otro software de Microsoft. Esta opción se puede configurar si ha descargado y copiado los [archivos de plantillas administrativas](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra) más recientes disponibles para Windows 2016 y versiones posteriores.

Si ejecuta Windows Server 2012 R2, este valor no se puede configurar mediante una directiva de grupo. Ejecute el comando de PowerShell siguiente en esas máquinas. Update Management cumple esta configuración.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>Valores de configuración de WSUS

Update Management es compatible con la configuración de WSUS. A continuación se muestra la lista de opciones de WSUS que puede configurar para trabajar con Update Management.

### <a name="intranet-microsoft-update-service-location"></a>Ubicación del servicio Microsoft Update en la intranet

Puede especificar orígenes para examinar y descargar actualizaciones en [Ubicación del servicio Microsoft Update en la intranet](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location). De forma predeterminada, el cliente de Windows Update está configurado para descargar las actualizaciones desde Windows Update. Cuando se especifica un servidor de WSUS como origen para las máquinas, si las actualizaciones no se aprueban en WSUS, se produce un error en la implementación de la actualización. 

Para restringir las máquinas solo para ese servicio de actualización interno, configure [No conectar a ubicaciones de Internet de Windows Update](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations). 

## <a name="next-steps"></a>Pasos siguientes

Después de configurar los parámetros de Windows Update, puede programar una implementación de actualizaciones siguiendo las instrucciones de [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
