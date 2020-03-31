---
title: Eliminar Azure Active Directory Domain Services | Microsoft Docs
description: Obtenga información para deshabilitar o eliminar Azure Active Directory Domain Services mediante Azure Portal
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: e1836f91b8afc1bb4f5b7e141949f3724c57c857
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614041"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Obtenga información para deshabilitar o eliminar Azure Active Directory Domain Services mediante Azure Portal

Si ya no necesita un dominio administrado, puede eliminar una instancia de Azure Active Directory Domain Services (Azure AD DS). No hay ninguna opción para desactivar o deshabilitar temporalmente un dominio administrado de Azure AD DS. Al eliminar el dominio administrado de Azure AD DS, no se elimina el inquilino de Azure AD y este no se ve afectado en modo alguno. En este artículo se muestra cómo usar Azure Portal para eliminar un dominio administrado de Azure AD DS.

> [!WARNING]
> **La eliminación es permanente y no se puede deshacer.**
> Cuando se elimina un dominio administrado de Azure AD DS, se realizan los siguientes pasos:
>   * Los controladores del dominio administrado se desabastecerán y se quitarán de la red virtual.
>   * Los datos del dominio administrado se eliminarán permanentemente. Estos datos incluyen unidades organizativas personalizadas, GPO, registros DNS personalizados, entidades de servicio, GMSA y elementos similares que haya creado.
>   * Las máquinas unidas al dominio administrado perderán la relación de confianza con el dominio y deberán separarse de este.
>       * No podrá iniciar sesión en estas máquinas con credenciales corporativas de AD. En su lugar, use las credenciales de administrador local para la máquina.

## <a name="delete-the-managed-domain"></a>Eliminar el dominio administrado

Para eliminar un dominio administrado de Azure AD DS, realice los siguientes pasos:

1. En Azure Portal, busque y seleccione **Azure AD Domain Services**.
1. Seleccione el nombre del dominio administrado de Azure AD DS como, por ejemplo, *aaddscontoso.com*.
1. En la página **Información general**, seleccione **Eliminar**. Para confirmar la eliminación, escriba de nuevo el nombre de dominio del dominio administrado y seleccione **Eliminar**.

La eliminación del dominio administrado de Azure AD DS puede tardar entre 15 y 20 minutos o más.

## <a name="next-steps"></a>Pasos siguientes

Puede de [compartir comentarios][feedback] de las características que le gustaría ver en Azure AD DS.

Para comenzar a usar Azure AD DS de nuevo, consulte [Crear y configurar una instancia de Azure Active Directory Domain Services][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
