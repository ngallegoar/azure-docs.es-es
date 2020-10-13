---
title: Conceptos de SMS en Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Obtenga información sobre los conceptos de SMS de Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 35398d60008ac52ba16dca0a0201f8c2f2101a0f
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91758564"
---
# <a name="sms-concepts"></a>Conceptos de SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Azure Communication Services le permite enviar y recibir mensajes de texto SMS mediante las bibliotecas cliente de SMS de Communication Services. Estas bibliotecas cliente se pueden usar para admitir escenarios de servicio al cliente, recordatorios de citas, autenticación en dos fases y otros requisitos de comunicación en tiempo real. Los SMS de Communication Services permiten enviar mensajes de manera confiable a la vez que exponen las conclusiones sobre la entrega y tasa de respuesta de las campañas.

Entre las principales características de las bibliotecas cliente de SMS de Azure Communication Services se incluyen:

-  Experiencia de instalación **sencilla** para agregar la capacidad de SMS a las aplicaciones.
- Compatibilidad con mensajes de **alta velocidad** mediante números gratuitos para los casos de uso de A2P (aplicación a persona) en Estados Unidos.
- Conversaciones **bidireccionales** para admitir escenarios como la asistencia al cliente, las alertas y los recordatorios de citas.
- **Entrega confiable** con informes de entrega en tiempo real para los mensajes que se envían desde la aplicación.
- **Análisis** realizar un seguimiento de los patrones de uso y la involucración de los clientes.
- Compatibilidad con la administración de **cancelaciones** para detectar automáticamente y respetar las cancelaciones de números gratuitos. Las cancelaciones de participación para números gratuitos de Estados Unidos son obligatorias y las aplican los operadores de Estados Unidos.
  - STOP: Si un destinatario del mensaje de texto desea dejar de participar, puede enviar "STOP" al número de teléfono gratuito. El operador envía la siguiente respuesta predeterminada para STOP: *"MENSAJE DE RED: Ha respondido con la palabra "STOP", lo que bloquea todos los mensajes de texto enviados desde este número. Volver a responder "UNSTOP" para recibir mensajes de nuevo".*
  - START o UNSTOP: si el destinatario desea volver a suscribirse a los mensajes de texto desde un número gratuito, puede enviar "START" o "UNSTOP" al número de teléfono gratuito. El operador envía la siguiente respuesta predeterminada para los mensajes START y UNSTOP: *"MENSAJE DE RED: Ha respondido "unstop" y comenzará a recibir mensajes de nuevo desde este número".*
  - Azure Communication Services detectará el mensaje de detención y bloqueará todos los demás mensajes al destinatario. El informe de entrega indicará que se ha producido un error en la entrega con el mensaje de estado "Remitente bloqueado para el destinatario especificado".
  - Los mensajes STOP, UNSTOP y START se le retransmitirán de vuelta. Azure Communication Services recomienda supervisar e implementar estas cancelaciones de participación para garantizar que no se envíen más mensajes a los destinatarios que hayan optado por no recibir las comunicaciones.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al envío de SMS](../../quickstarts/telephony-sms/send.md)

Puede que los siguientes documentos le resulten interesantes:

- Familiarícese con la [biblioteca cliente de SMS](../telephony-sms/sdk-features.md)
- Obtención de un [número de teléfono](../../quickstarts/telephony-sms/get-phone-number.md) compatible con SMS
- [Planeación de una solución de SMS](../telephony-sms/plan-solution.md)
