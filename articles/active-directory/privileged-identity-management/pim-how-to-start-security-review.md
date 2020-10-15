---
title: 'Creación de una revisión de acceso para los roles de Azure AD en PIM: Azure AD | Microsoft Docs'
description: Aprenda a crear una revisión de acceso para los roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b3070a1296b368ea2c56038ec696942416a6fe2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84743888"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Cree una revisión de acceso para los roles de Azure AD en Azure AD Privileged Identity Management (PIM).

Para reducir el riesgo asociado con las asignaciones de roles obsoletas, debe revisar el acceso periódicamente. Puede usar Azure AD Privileged Identity Management (PIM) para crear revisiones de acceso para roles de Azure AD con privilegios. También puede configurar revisiones de acceso periódicas que se produzcan automáticamente.

En este artículo se describe cómo crear una o varias revisiones de acceso para los roles de Azure AD con privilegios.

## <a name="prerequisites"></a>Prerrequisitos

[Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Abrir las revisiones de acceso

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del rol Administrador de roles con privilegios.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. En Administrar, seleccione **Revisiones de acceso** y, a continuación, seleccione **Nueva**.

    ![Roles de Azure AD: lista de revisiones de acceso que muestra el estado de todas las revisiones](./media/pim-how-to-start-security-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Inicio de la revisión de acceso

Cuando haya especificado la configuración de una revisión de acceso, seleccione **Iniciar**. La revisión de acceso aparecerá en la lista con un indicador de su estado.

![Lista de revisiones de acceso que muestra el estado de las revisiones iniciadas](./media/pim-how-to-start-security-review/access-reviews-list.png)

De forma predeterminada, Azure AD envía un correo electrónico a los revisores poco después de iniciar la revisión. Si decide no hacer que Azure AD envíe el correo electrónico, asegúrese de informar a los revisores de que hay una revisión de acceso esperando para que la lleven a cabo. Puede mostrarles las instrucciones sobre cómo [revisar el acceso a los roles de Azure AD](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Administración de la revisión de acceso

En la página **Información general** de la revisión de acceso, puede seguir el progreso de los revisores a medida que completan las revisiones. Los derechos de acceso no se cambian en el directorio hasta que [la revisión finaliza](pim-how-to-complete-review.md).

![Página de información general de revisiones de acceso que muestra los detalles de la revisión](./media/pim-how-to-start-security-review/access-review-overview.png)

Si se trata de una revisión puntual, una vez finalizado el período de revisión de acceso o cuando el administrador detenga la revisión de acceso, siga los pasos sobre [cómo realizar una revisión de acceso de los roles de Azure AD](pim-how-to-complete-review.md) para ver los resultados y aplicarlos.  

Para administrar una serie de revisiones de acceso, vaya a la revisión de acceso y verá los próximos eventos en Revisiones programadas; ahí podrá editar la fecha de finalización o agregar o quitar revisores según corresponda.

Según las selecciones de la **Configuración de finalización**, la aplicación automática se ejecutará después de la fecha de finalización de la revisión o cuando se detenga manualmente la revisión. El estado de la revisión cambiará de **Completado** a estados intermedios como **Aplicando** y, por último, a **Aplicado**. Debería ver que los usuarios denegados, si es que los hay, se eliminan de los roles en unos minutos.

## <a name="next-steps"></a>Pasos siguientes

- [Revisar el acceso a los roles de Azure AD](pim-how-to-perform-security-review.md)
- [Completar una revisión de acceso de roles de Azure AD](pim-how-to-complete-review.md)
- [Crear una revisión de acceso de roles de recursos de Azure](pim-resource-roles-start-access-review.md)
