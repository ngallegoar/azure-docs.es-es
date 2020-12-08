---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 11/03/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2b7881e7fa1ccbcec1325b2af0a570c86b0585a8
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96017530"
---
Al ejecutar la aplicación de dispositivo de ejemplo más adelante en este tutorial, necesitará los siguientes valores de configuración:

* Ámbito de identificador: En la aplicación de IoT Central, vaya a **Administration > Device Connection** (Administración > Conexión de dispositivos). Anote el valor de **Ámbito de id**.
* Clave principal del grupo: En la aplicación de IoT Central, vaya a **Administration > Device Connection > SAS-IoT-Devices** (Administración > Conexión de dispositivos > SAS-IoT-Devices). Tome nota del valor de **Primary key** (Clave principal) de la firma de acceso compartido.

Use Cloud Shell para generar una clave de dispositivo a partir de la clave SAS de grupo que acaba de recuperar:

```azurecli-interactive
az extension add --name azure-iot
az iot central device compute-device-key  --device-id sample-device-01 --pk <the group SAS primary key value>
```

Anote la clave de dispositivo generada, ya que la usará más adelante en este tutorial.
