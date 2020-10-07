---
title: 'Visualización de la entidad de servicio de una identidad administrada mediante PowerShell: Azure AD'
description: Instrucciones detalladas para ver la entidad de servicio de una identidad administrada mediante PowerShell.
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
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 15ccc0faa4d74a2ef95aca00a6257f27b9a209c3
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91611952"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Visualización de la entidad de servicio de una identidad administrada mediante PowerShell

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, obtendrá información sobre cómo visualizar la entidad de servicio de una identidad administrada mediante PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prerequisites

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para obtener una cuenta gratuita](https://azure.microsoft.com/free/).
- Habilite la [identidad asignada por el sistema en una máquina virtual](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) o [aplicación](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity).
- Para ejecutar los scripts de ejemplo, tiene dos opciones:
    - Use [Azure Cloud Shell](../../cloud-shell/overview.md), que puede abrir mediante el botón **Probar** en la esquina superior derecha de los bloques de código.
    - Ejecute scripts localmente instalando la versión más reciente de [Azure PowerShell](/powershell/azure/install-az-ps) y, a continuación, inicie sesión en Azure con `Connect-AzAccount`.

## <a name="view-the-service-principal"></a>Visualización de la entidad de servicio

El comando siguiente muestra cómo ver la entidad de servicio de una aplicación o máquina virtual con la identidad asignada por el sistema habilitada. Reemplazar `<Azure resource name>` por sus propios valores.

```azurepowershell-interactive
Get-AzADServicePrincipal -DisplayName <Azure resource name>
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo ver las entidades de servicio de Azure AD mediante PowerShell, consulte [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal).
