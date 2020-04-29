---
title: Comprobación del estado de Azure Active Directory Domain Services | Microsoft Docs
description: Obtenga información sobre cómo comprobar el estado de un dominio administrado de Azure Active Directory Domain Services (Azure AD DS) y comprender los mensajes de estado mediante Azure Portal.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: 27ab14af25704a4f7fb46aa5e86cdaf881c49442
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80655639"
---
# <a name="check-the-health-of-an-azure-active-directory-domain-services-managed-domain"></a>Comprobación del estado de un dominio administrado de Azure Active Directory Domain Services

Azure Active Directory Domain Services (Azure AD DS) ejecuta algunas tareas en segundo plano para mantener el dominio administrado en buen estado y actualizado. Estas tareas incluyen la realización de copias de seguridad, la aplicación de actualizaciones de seguridad y la sincronización de datos de Azure AD. Si hay problemas con el dominio administrado de Azure AD DS, es posible que estas tareas no se ejecuten por completo. Para revisar y resolver los problemas, puede comprobar el estado de mantenimiento de un dominio administrado de Azure AD DS mediante Azure Portal.

En este artículo se muestra cómo ver el estado de mantenimiento de Azure AD DS y comprender la información o las alertas que se muestran.

## <a name="view-the-health-status"></a>Visualización del estado de mantenimiento

El estado de mantenimiento de un dominio administrado de Azure AD DS se visualiza mediante Azure Portal. Se puede ver información sobre la hora de la última copia de seguridad y la sincronización con Azure AD, junto con las alertas que indican un problema con el estado del dominio administrado. Para ver el estado de mantenimiento de un dominio administrado de Azure AD DS, complete los pasos siguientes:

1. En Azure Portal, busque y seleccione **Azure AD Domain Services**.
1. Seleccione el dominio administrado de Azure AD DS, por ejemplo, *aaddscontoso.com*.
1. En el lado izquierdo de la ventana de recursos de Azure AD DS, seleccione **Estado**. En la captura de pantalla de ejemplo siguiente se muestra un dominio administrado de Azure AD DS en buen estado y el estado de la última copia de seguridad y la sincronización de Azure AD:

    ![Descripción general de la página de estado en Azure Portal que muestra el estado de Azure Active Directory Domain Services](./media/check-health/health-page.png)

La marca de tiempo *Última evaluación* de la página de estado muestra cuándo se comprobó por última vez el dominio administrado de Azure AD DS. El estado de un dominio administrado de Azure AD DS se evalúa cada hora. Si hace algún cambio en un dominio administrado de Azure AD DS, espere hasta el siguiente ciclo de evaluación para ver el estado de mantenimiento actualizado.

El estado de la parte superior derecha indica el estado general del dominio administrado de Azure AD DS. Los factores de estado se tienen en cuenta en todas las alertas existentes en su dominio. En la siguiente tabla se detallan los indicadores de estado disponibles:

| Status | Icono | Explicación |
| --- | :----: | --- |
| En ejecución | <img src= "./media/active-directory-domain-services-alerts/running-icon.png" width = "15" alt="Green check mark for running"> | El dominio administrado de Azure AD DS se ejecuta correctamente y no tiene ninguna alerta crítica ni de advertencia. El dominio puede tener alertas informativas. |
| Requiere atención (advertencia) | <img src= "./media/active-directory-domain-services-alerts/warning-icon.png" width = "15" alt="Yellow exclamation mark for warning"> | No hay ninguna alerta crítica en el dominio administrado, de Azure AD DS, pero hay una o varias alertas de advertencia que se deben abordar. |
| Requiere atención (crítico) | <img src= "./media/active-directory-domain-services-alerts/critical-icon.png" width = "15" alt="Red exclamation mark for critical"> | Hay una o varias alertas críticas en el dominio administrado de Azure AD DS que se deben abordar. También puede tener alertas de advertencia o informativas. |
| Implementando | <img src= "./media/active-directory-domain-services-alerts/deploying-icon.png" width = "15" alt="Blue circular arrows for deploying"> | Se está implementando el dominio de Azure AD DS. |

## <a name="understand-monitors-and-alerts"></a>Descripción de los monitores y las alertas

El estado de mantenimiento de un dominio administrado de Azure AD DS muestra dos tipos de información: *monitores* y *alertas*. Los monitores muestran la hora a la que se completaron las tareas en segundo plano básicas. Las alertas proporcionan información o sugerencias para mejorar la estabilidad del dominio administrado.

### <a name="monitors"></a>Monitores

Los monitores son áreas de un dominio administrado de Azure AD DS que se comprueban periódicamente. Si hay alguna alerta activa para el dominio administrado de Azure AD DS, puede provocar que uno de los monitores informe de un problema. Actualmente, Azure AD Domain Services tiene monitores para las siguientes áreas:

* Copia de seguridad
* Sincronización con Azure AD

#### <a name="backup-monitor"></a>Monitor de copia de seguridad

El monitor de copia de seguridad comprueba que las copias de seguridad periódicas automatizadas del dominio administrado de Azure AD DS se han ejecutado correctamente. En la siguiente tabla se detalla el estado del monitor de copia de seguridad:

| Valor del detalle | Explicación |
| --- | --- |
| Nunca se le realizó una copia de seguridad | Este estado es normal para nuevos dominios administrado de Azure AD DS. La primera copia de seguridad se debe crear veinticuatro horas después de que se implemente el dominio administrado de Azure AD DS. Si este estado persiste, [abra una solicitud de soporte técnico de Azure][azure-support]. |
| La última copia de seguridad se realizó hace entre 1 y 14 días | Este intervalo de tiempo es el estado esperado para el monitor de copia de seguridad. Las copias de seguridad periódicas automatizadas deben realizarse en este período. |
| La última copia de seguridad se realizó hace más de 14 días. | Un intervalo de tiempo de más de dos semanas indica que hay un problema con las copias de seguridad periódicas automatizadas. Las alertas críticas activas pueden evitar que se hagan copias de seguridad del dominio administrado de Azure AD DS. Resuelva todas las alertas activas para el dominio administrado de Azure AD DS. Si el monitor de copia de seguridad no actualiza después el estado para informar de una copia de seguridad reciente, [abra una solicitud de soporte técnico de Azure][azure-support]. |

#### <a name="synchronization-with-azure-ad-monitor"></a>Sincronización con el monitor de Azure AD

Un dominio administrado de Azure AD DS se sincroniza periódicamente con Azure Active Directory. El número de usuarios y objetos de grupo, así como el número de cambios realizados en el directorio de Azure AD desde la última sincronización, afecta al tiempo que tarda en sincronizarse. Si el dominio administrado de Azure AD DS se sincronizó por última vez hace tres días, busque y resuelva las alertas activas. Si el monitor de sincronización no actualiza el estado para mostrar una sincronización reciente después de tratar cualquier alerta activa, [abra una solicitud de soporte técnico de Azure][azure-support].

### <a name="alerts"></a>Alertas

Las alertas se generan para los problemas en un dominio administrado de Azure AD DS que deben solucionarse para que el servicio se ejecute correctamente. Cada alerta explica el problema y proporciona una dirección URL que muestra los pasos específicos para solucionarlo. Para más información sobre las posibles alertas y sus soluciones, consulte [Solución de problemas de alertas](troubleshoot-alerts.md).

Las alertas de estado de mantenimiento se clasifican en los siguientes niveles de gravedad:

 * Las **alertas críticas** hacen referencia a los problemas que afectan gravemente al dominio administrado de Azure AD DS. Estas alertas deben abordarse inmediatamente. La plataforma Azure no puede supervisar, administrar, revisar ni sincronizar el dominio administrado hasta que se resuelvan los problemas.
 * Las **alertas de advertencia** le notifican los problemas que pueden afectar a las operaciones del dominio administrado de Azure AD DS si el problema persiste. Estas alertas también ofrecen recomendaciones para proteger el dominio administrado.
 * Las **alertas informativas** son notificaciones que no afectan negativamente al dominio administrado de Azure AD DS. Las alertas informativas proporcionan información sobre lo que sucede en el dominio administrado.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las alertas que se muestran en la página de estado de mantenimiento, consulte [Resolución de alertas en el dominio administrado][troubleshoot-alerts].

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[troubleshoot-alerts]: troubleshoot-alerts.md
