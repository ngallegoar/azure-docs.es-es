---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b4a1891eadf2e36bcb94b9f605d91f227fa83a3f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027230"
---
En este ejemplo se usa el servicio [Twilio](https://www.twilio.com/) para enviar mensajes SMS a un teléfono móvil. Azure Functions ya compatible con Twilio a través del [enlace de Twilio](../articles/azure-functions/functions-bindings-twilio.md), el ejemplo utiliza esa característica.

Lo primero que necesita es una cuenta de Twilio. Puede crear una gratis en https://www.twilio.com/try-twilio. Cuando tenga una cuenta, agregue los tres **valores de configuración de la aplicación** siguientes a la aplicación de función.

| Nombre del valor de configuración de la aplicación | Descripción del valor |
| - | - |
| **TwilioAccountSid**  | Identificador de seguridad de la cuenta de Twilio |
| **TwilioAuthToken**   | Token de autenticación de la cuenta de Twilio |
| **TwilioPhoneNumber** | Número de teléfono asociado a la cuenta de Twilio, que se utiliza para enviar mensajes SMS. |