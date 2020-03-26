---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2b8d60da507aa1eb312d1a8505f756ff337085ff
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77592443"
---
En la tabla siguiente se enumeran los límites que se aplican a los recursos de Azure IoT Hub Device Provisioning Service.

| Resource | Límite |
| --- | --- |
| Servicios máximos de aprovisionamiento de dispositivos por suscripción de Azure | 10 |
| Número máximo de inscripciones | 1 000 000 |
| Número máximo de registros | 1 000 000 |
| Número máximo de grupos de inscripciones | 100 |
| Número máximo de CA | 25 |
| Número máximo de centros de IoT vinculados | 50 |
| Tamaño máximo de mensaje | 96 KB|

> [!NOTE]
> Para aumentar el número de inscripciones y registros del servicio de aprovisionamiento, póngase en contacto con [Soporte técnico de Microsoft](https://azure.microsoft.com/support/options/).

> [!NOTE]
> No se puede aumentar el número máximo de entidades de certificación.

El servicio de aprovisionamiento de dispositivos limita las solicitudes cuando se superan las cuotas siguientes.

| Limitación | Valor por unidad |
| --- | --- |
| Operaciones | 200/min/servicio |
| Registros de dispositivos | 200/min/servicio |
| Operación de sondeo de dispositivos | 5/10 s/dispositivo |
