---
title: 'Visualización de la entidad de servicio de una identidad administrada: CLI de Azure: Azure AD'
description: Instrucciones detalladas para ver la entidad de servicio de una identidad administrada mediante la CLI de Azure.
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
ms.custom: devx-track-azurecli
ms.openlocfilehash: ce3a35562bdef34f44ef1093a3196ea7afb0bd9b
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92892050"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Visualización de la entidad de servicio de una identidad administrada mediante la CLI de Azure

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, obtendrá información sobre cómo visualizar la entidad de servicio de una identidad administrada mediante la CLI de Azure.

Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="prerequisites"></a>Prerrequisitos

- Si no está familiarizado con las identidades administradas para los recursos de Azure, vea [¿Qué son las identidades administradas para recursos de Azure?](overview.md).

- Habilite la [identidad asignada por el sistema en una máquina virtual](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) o [aplicación](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

## <a name="view-the-service-principal"></a>Visualización de la entidad de servicio

El comando siguiente muestra cómo ver la entidad de servicio de una aplicación o máquina virtual con la identidad administrada habilitada. Reemplazar `<Azure resource name>` por sus propios valores.

```azurecli-interactive
az ad sp list --display-name <Azure resource name>
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo administrar las entidades de servicio de Azure AD con la CLI de Azure, vea [az ad sp](/cli/azure/ad/sp).
