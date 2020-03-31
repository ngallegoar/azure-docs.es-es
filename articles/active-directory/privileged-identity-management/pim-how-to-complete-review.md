---
title: 'Finalización de la revisión de acceso de los roles de Azure AD en PIM: Azure AD | Microsoft Docs'
description: Aprenda a completar una revisión de acceso para los roles de Azure AD en Azure AD Privileged Identity Management (PIM) y a ver los resultados.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe2d85d605b9ee418a5709ddcdb448c56be1d918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022273"
---
# <a name="complete-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Finalización de una revisión de acceso de los roles de Azure AD en Privileged Identity Management

Los administradores de roles con privilegios pueden revisar el acceso con privilegios una vez que se [ha iniciado una revisión del acceso](pim-how-to-start-security-review.md).  Privileged Identity Management (PIM) enviará automáticamente un correo electrónico para pedir a los usuarios de la organización de Azure Active Directory (Azure AD) que revisen su acceso. Si algún usuario no ha recibido un correo electrónico, puede enviarle las instrucciones necesarias para [realizar una revisión del acceso](pim-how-to-perform-security-review.md).

Cuando acabe el período de la revisión del acceso o cuando todos los usuarios hayan finalizado su autorrevisión, siga los pasos de este artículo para administrar la revisión y ver los resultados.

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso

1. Vaya a [Azure Portal](https://portal.azure.com/) y seleccione el servicio **Azure AD Privileged Identity Management** en el panel.
1. Seleccione la sección **Revisiones de acceso** del panel.
1. Seleccione la revisión de acceso que quiere administrar.

En la hoja de detalles de la revisión de acceso, hay varias opciones para administrar dicha revisión.

![Botones de la revisión de acceso de Privileged Identity Management: captura de pantalla](./media/pim-how-to-complete-review/review-buttons.png)

### <a name="remind"></a>Recuerde

Si una revisión de acceso está configurada para que los usuarios revisen por sí mismos, el botón **Recordar** envía una notificación.

### <a name="stop"></a>Stop

Todas las revisiones de acceso tienen una fecha de finalización, pero puede usar el botón **Detener** para terminarlas antes. Si para entonces algunos de los usuarios no se han revisado, no podrán hacerlo una vez que se haya detenido la revisión. No se puede reiniciar una revisión después de que se ha detenido.

### <a name="apply"></a>Aplicar

Después de que finaliza una revisión de acceso, bien porque se ha llegado a la fecha de finalización o porque se ha detenido manualmente, el botón **Aplicar** implementa el resultado de la revisión. Si se denegó el acceso de un usuario en la revisión, este es el paso que se quitará su asignación de rol.  

### <a name="export"></a>Exportación

Si desea aplicar los resultados de la revisión del acceso manualmente, puede exportar la revisión. El botón **Exportar** iniciará la descarga de un archivo CSV. Puede administrar los resultados en Excel u otros programas que abran archivos CSV.

### <a name="delete"></a>Eliminar

Si ya no le interesa más la revisión, elimínela. El botón **Eliminar** elimina la revisión del servicio Privileged Identity Management.

> [!IMPORTANT]
> No se le pedirá que confirme este cambio destructivo, por lo que debe comprobar que desea eliminar esa revisión.

## <a name="next-steps"></a>Pasos siguientes

- [Inicio de una revisión de acceso para los roles de Azure AD en Privileged Identity Management](pim-how-to-start-security-review.md)
- [Realización de una revisión de acceso de mis roles de Azure AD en Privileged Identity Management](pim-how-to-perform-security-review.md)
