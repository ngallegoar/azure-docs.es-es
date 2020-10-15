---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186116"
---
En este ejemplo se usa el servicio [Twilio](https://www.twilio.com/) para enviar mensajes SMS a un teléfono móvil. Azure Functions ya compatible con Twilio a través del [enlace de Twilio](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), el ejemplo utiliza esa característica.

Lo primero que necesita es una cuenta de Twilio. Puede crear una gratis en https://www.twilio.com/try-twilio. Cuando tenga una cuenta, agregue los tres **valores de configuración de la aplicación** siguientes a la aplicación de función.

| Nombre del valor de configuración de la aplicación | Descripción del valor |
| - | - |
| **TwilioAccountSid**  | Identificador de seguridad de la cuenta de Twilio |
| **TwilioAuthToken**   | Token de autenticación de la cuenta de Twilio |
| **TwilioPhoneNumber** | Número de teléfono asociado a la cuenta de Twilio, que se utiliza para enviar mensajes SMS. |