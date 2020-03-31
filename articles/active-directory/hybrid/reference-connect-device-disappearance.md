---
title: Descripción de Azure AD Connect 1.4.xx.x y desaparición del dispositivo | Microsoft Docs
description: En este documento se describe un problema que surge con la versión 1.4.xx.x de Azure AD Connect
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: bc159452c81a673ca4a7ed46aa7eff19fd9209eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73176030"
---
# <a name="understanding-azure-ad-connect-14xxx-and-device-disappearance"></a>Descripción de Azure AD Connect 1.4.xx.x y desaparición del dispositivo
Con la versión 1.4.xx.x de Azure AD Connect, algunos clientes pueden ver que algunos o todos sus dispositivos Windows desaparecen de Azure AD. No hay motivo para preocuparse, ya que estas identidades de dispositivo no se usan en Azure AD durante la autorización de acceso condicional. Este cambio no eliminará ningún dispositivo de Windows que se haya registrado correctamente con Azure AD para Unión a Azure AD híbrido.

Tenga en cuenta que si ve que estas eliminaciones de objetos de equipo o de dispositivo en Azure AD superan el umbral de eliminación de la exportación, se aconseja al cliente que permita su paso. [How To: allow deletes to flow when they exceed the deletion threshold](how-to-connect-sync-feature-prevent-accidental-deletes.md) (Instrucciones: Permitir que fluyan las eliminaciones una vez que superan el umbral de eliminación)

## <a name="background"></a>Información previa
Los dispositivos Windows registrados como "Unión a Azure AD híbrido" se representan en Azure AD como objetos de dispositivo. Estos objetos de dispositivo se pueden usar para obtener el acceso condicional. Los dispositivos Windows 10 se sincronizan con la nube a través de Azure AD Connect, los dispositivos Windows de nivel inferior, en cambio, se registran directamente con AD FS o con un inicio de sesión único de conexión directa.

## <a name="windows-10-devices"></a>Dispositivos Windows 10
Se supone que Azure AD Connect solo sincroniza con la nube los dispositivos Windows 10 con un valor de atributo de tipo userCertificate específico que haya configurado la Unión a Azure AD híbrido. En versiones anteriores de Azure AD Connect este requisito no se aplicaba rigurosamente, lo que generaba objetos de dispositivo innecesarios en Azure AD. Estos dispositivos de Azure AD siempre se han mantenido en el estado "pendiente", ya que no debían estar registrados con Azure AD. 

Esta versión de Azure AD Connect solo sincronizará los dispositivos Windows 10 que estén configurados correctamente para Unión a Azure AD híbrido. Los objetos del dispositivo Windows 10 sin el atributo userCertificate específico de unión a Azure AD se quitarán de Azure AD.

## <a name="down-level-windows-devices"></a>Dispositivos Windows de nivel inferior
Azure AD Connect nunca debe sincronizar [dispositivos Windows de nivel inferior](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices). Los dispositivos de Azure AD sincronizados previamente se eliminarán de Azure AD. Si Azure AD Connect está intentando eliminar [dispositivos Windows de nivel inferior](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices), entonces el dispositivo no es el que creó [Microsoft Workplace Join para MSI que no sean equipos con Windows 10](https://www.microsoft.com/download/details.aspx?id=53554) y no lo podrá usar ninguna otra característica de Azure AD.

Es posible que algunos clientes tengan que volver a visitar [Instrucciones: Planeamiento de la implementación de la unión a Azure Active Directory híbrido](../devices/hybrid-azuread-join-plan.md) para registrar los dispositivos Windows correctamente y asegurarse de que dichos dispositivos puedan participar plenamente en el acceso condicional basado en dispositivos. 

## <a name="how-can-i-verify-which-devices-are-deleted-with-this-update"></a>¿Cómo puedo comprobar los dispositivos que se eliminan con esta actualización?

Para comprobar los dispositivos que se eliminan, puede usar este script de PowerShell: https://gallery.technet.microsoft.com/scriptcenter/Export-Hybrid-Azure-AD-f8e51436

Este script genera un informe sobre los certificados almacenados en objetos de equipo de Active Directory, en concreto, certificados emitidos por la característica de unión a Azure AD híbrido.
Comprueba los certificados presentes en la propiedad UserCertificate de un objeto de equipo en AD y, para cada certificado no expirado presente, valida si el certificado se emitió para la característica de unión a Azure AD híbrido (es decir, el nombre del asunto coincide con CN={ObjectGUID}).
Antes, Azure AD Connect sincronizaría con Azure AD cualquier equipo que incluyera al menos un certificado válido, pero a partir de la versión 1.4 de Azure AD Connect, el motor de sincronización puede identificar certificados de unión a Azure AD híbrida y "filtrará en la nube" el objeto de equipo resultante de la sincronización a Azure AD a menos que haya un certificado de unión a Azure AD híbrido válido.
Los dispositivos de Azure AD que ya se sincronizaron con AD, pero no tienen un certificado de unión a Azure AD híbrido válido, se eliminarán (CloudFiltered=TRUE) por medio del motor de sincronización.

## <a name="next-steps"></a>Pasos siguientes
- [Historial de versiones de Azure AD Connect](reference-connect-version-history.md)
