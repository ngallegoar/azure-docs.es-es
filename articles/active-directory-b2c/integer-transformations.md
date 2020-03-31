---
title: Ejemplos de transformación de notificaciones de enteros para directivas personalizadas
titleSuffix: Azure AD B2C
description: Ejemplos de transformación de notificaciones de entero para el esquema de Identity Experience Framework (IEF) de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ece25e95bbbe9f1a1fa591c29ea9ffda0fefd369
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187685"
---
# <a name="integer-claims-transformations"></a>Transformaciones de notificaciones de entero

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos de uso de las transformaciones de notificaciones de enteros del esquema Identity Experience Framework en Azure Active Directory B2C (Azure AD B2C). Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim

Convierte a un tipo de datos Long en un tipo de datos String.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | long | El valor ClaimType que se va a convertir en cadena. |
| OutputClaim | outputClaim | string | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation. |

En este ejemplo, la notificación `numericUserId` con un tipo de valor de Long se convierte en una notificación `UserId` con un tipo de valor de String.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
    - **inputClaim**: 12334 (Long)
- Notificaciones de salida:
    - **outputClaim**: "12334" (cadena)

