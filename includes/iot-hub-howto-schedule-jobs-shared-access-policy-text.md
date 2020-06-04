---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e493d1c4f5851ee510ea83e706afce5fbb6f487e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "70049078"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-schedule-jobs.md files-->

En este artículo se creará un servicio de back-end que programa un trabajo para que invoque un método directo en un dispositivo, programa un trabajo para actualizar el dispositivo gemelo y supervisa el progreso de cada trabajo. Para realizar estas operaciones, el servicio necesita los permisos de **lectura del registro** y **escritura en el registro**. De manera predeterminada, todas las instancias del centro de IoT se crean con una directiva de acceso compartido denominada **registryReadWrite** que concede estos permisos.
