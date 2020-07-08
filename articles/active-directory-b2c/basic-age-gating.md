---
title: Habilitar la restricción de acceso por edad en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información acerca de cómo identificar a los menores mediante la aplicación.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/13/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6bd93f9062f8446ce20436a7a04e2054aaf5be71
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85386138"
---
# <a name="enable-age-gating-in-azure-active-directory-b2c"></a>Habilitación de la restricción de acceso por edad en Azure Active Directory B2C

>[!IMPORTANT]
>Esta característica está en versión preliminar pública. No use la característica para aplicaciones de producción.
>

La restricción de acceso por edad en Azure Active Directory B2C (Azure AD B2C) le permite identificar a los menores que quieren usar la aplicación. Puede optar por impedir que los menores inicien sesión en la aplicación. Los usuarios también pueden volver a la aplicación e identificar su grupo de edad y su estado de consentimiento parental. Azure AD B2C puede bloquear a los menores sin consentimiento parental. Azure AD B2C también se puede configurar para permitir que la aplicación decida qué hacer con los menores.

Después de habilitar la restricción de acceso por edad en el [flujo de usuario](user-flow-overview.md), se pedirá a los usuarios su fecha de nacimiento y el país o región donde residen. Si inicia sesión un usuario que no ha introducido la información anteriormente, deberá escribirla la próxima vez que inicie sesión. Las reglas se aplican cada vez que un usuario inicia sesión.

Azure AD B2C usa la información que escribe el usuario para identificar si es un menor. A continuación, el campo **ageGroup** se actualiza en su cuenta. El valor puede ser `null`, `Undefined`, `Minor`, `Adult` y `NotAdult`.  Los campos **ageGroup** y **consentProvidedForMinor** se utilizan para calcular el valor de **legalAgeGroupClassification**.

La restricción de acceso por edad implica dos valores de edad: la edad en que alguien ya se no considera un menor y la edad en la que un menor debe tener consentimiento parental. En la tabla siguiente se enumeran las reglas de edad que se usan para la definición de un menor y de un menor que requiere consentimiento.

| País/región | Nombre del país o región | Edad de consentimiento de menores | Edad del menor |
| -------------- | ------------------- | ----------------- | --------- |
| Valor predeterminado | None | None | 18 |
| AE | Emiratos Árabes Unidos | None | 21 |
| AT | Austria | 14 | 18 |
| BE | Bélgica | 14 | 18 |
| BG | Bulgaria | 16 | 18 |
| BH | Bahréin | None | 21 |
| CM | Camerún | None | 21 |
| CY | Chipre | 16 | 18 |
| CZ | República Checa | 16 | 18 |
| DE | Alemania | 16 | 18 |
| DK | Dinamarca | 16 | 18 |
| EE | Estonia | 16 | 18 |
| EG | Egipto | None | 21 |
| ES | España | 13 | 18 |
| VF | Francia | 16 | 18 |
| GB | Reino Unido | 13 | 18 |
| GR | Grecia | 16 | 18 |
| HR | Croacia | 16 | 18 |
| HU | Hungría | 16 | 18 |
| IE | Irlanda | 13 | 18 |
| IT | Italia | 16 | 18 |
| KR | Corea, República de | 14 | 18 |
| LT | Lituania | 16 | 18 |
| LU | Luxemburgo | 16 | 18 |
| LV | Letonia | 16 | 18 |
| MT | Malta | 16 | 18 |
| N/D | Namibia | None | 21 |
| NL | Países Bajos | 16 | 18 |
| PL | Polonia | 13 | 18 |
| PT | Portugal | 16 | 18 |
| RO | Rumanía | 16 | 18 |
| SE | Suecia | 13 | 18 |
| SG | Singapur | None | 21 |
| SI | Eslovenia | 16 | 18 |
| SK | Eslovaquia | 16 | 18 |
| TD | Chad | None | 21 |
| TH | Tailandia | None | 20 |
| TW | Taiwán | None | 20 |
| US | Estados Unidos | 13 | 18 |

## <a name="age-gating-options"></a>Opciones del acceso según la edad

### <a name="allowing-minors-without-parental-consent"></a>Permitir el acceso a menores sin autorización parental

Para los flujos de usuarios que permitan el registro, el inicio de sesión o ambos, puede optar por permitir el acceso de menores sin autorización a la aplicación. A los menores sin autorización parental se les permite iniciar sesión o registrarse de manera normal, y Azure AD B2C emite un token de identificación con la notificación **legalAgeGroupClassification**. Esta notificación define la experiencia que tienen los usuarios, como la recopilación de consentimiento parental y la actualización del campo **consentProvidedForMinor**.

### <a name="blocking-minors-without-parental-consent"></a>Bloquear a menores sin autorización parental

Para los flujos de usuarios que permitan el registro, el inicio de sesión o ambos, puede optar por bloquear el acceso de menores sin autorización a la aplicación. Las siguientes opciones están disponibles para controlar los usuarios bloqueados en Azure AD B2C:

- Volver a enviar un archivo JSON a la aplicación: esta opción envía una respuesta a la aplicación indicando que se bloqueó un menor.
- Mostrar una página de error: se muestra al usuario una página que le indica que no puede acceder a la aplicación.

## <a name="set-up-your-tenant-for-age-gating"></a>Configurar el inquilino para la restricción de acceso por edad

Para poder usar la restricción de acceso por edad en un flujo de usuario, debe configurar el inquilino para que admita propiedades adicionales.

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior. Elija el directorio que contiene el inquilino.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione la opción **Propiedades** del inquilino en el menú de la izquierda.
2. En la sección **Restricción de acceso por edad**, haga clic en **Configurar**.
3. Espere a que finalice la operación y el inquilino tendrá configurada la restricción de acceso por edad.

## <a name="enable-age-gating-in-your-user-flow"></a>Habilitar la restricción de acceso por edad en el flujo de usuario

Una vez que el inquilino esté configurado para usar la restricción de acceso por edad, puede usar esta característica en los [flujos de usuario](user-flow-versions.md) cuando esté habilitada. Para habilitar el acceso según la edad, realice los pasos siguientes:

1. Cree un flujo de usuario con la restricción de acceso por edad habilitada.
2. Después de crear el flujo de usuario, seleccione **Propiedades** en el menú.
3. En la sección **Restricción de acceso por edad**, seleccione **Habilitado**.
4. A continuación, elija cómo quiere administrar los usuarios que se identifican como menores. En **Registrarse o Iniciar sesión**, seleccione `Allow minors to access your application` o `Block minors from accessing your application`. Si el bloqueo menores está seleccionado, elija `Send a JSON back to the application` o `Show an error message`.




