---
title: 'Visualización de la entidad de servicio de una identidad administrada en Azure Portal: Azure AD'
description: Instrucciones detalladas para ver la entidad de servicio de una identidad administrada en Azure Portal.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/30/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b25f2691646192faf4d4c133835b456038abd94
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91611935"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Visualización de la entidad de servicio de una identidad administrada en Azure Portal

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, obtendrá información sobre cómo visualizar la entidad de servicio de una identidad administrada mediante Azure Portal.

 > [!NOTE] 
 > Las entidades de servicio son aplicaciones empresariales. 

## <a name="prerequisites"></a>Prerequisites

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para obtener una cuenta gratuita](https://azure.microsoft.com/free/).
- Habilite la [identidad asignada por el sistema en una máquina virtual](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) o [aplicación](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>Visualización de la entidad de servicio

Este procedimiento muestra cómo ver la entidad de servicio de una máquina virtual con la identidad asignada por el sistema habilitada (se aplican los mismos pasos para una aplicación).

1. Haga clic en **Azure Active Directory** y, a continuación, haga clic en **Aplicaciones empresariales**.
2. En **Tipo de aplicación**, seleccione **Todas las aplicaciones** y haga clic en **Aplicar**.
3. En el cuadro de filtro de búsqueda, escriba el nombre del recurso de Azure que tiene habilitada la identidad administrada o elíjalo en la lista presentada.

   ![Visualización de entidades de servicio de identidades administradas en Azure Portal](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Pasos siguientes

[Identidades administradas para recursos de Azure](./overview.md)