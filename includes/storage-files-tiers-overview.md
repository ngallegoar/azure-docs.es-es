---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f4e5c59a5ce7c0d743a574309bb1b9276ce80bea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597861"
---
Azure Files ofrece dos niveles diferentes de almacenamiento, Premium y Estándar, para permitirle adaptar sus recursos compartidos a los requisitos de rendimiento y precio de su escenario:

- **Recursos compartidos de archivos Premium**: Los recursos compartidos de archivos Premium están respaldados por unidades de estado sólido (SSD) y se implementan en el tipo de **cuenta de almacenamiento de FileStorage**. Los recursos compartidos de archivos Prémium proporcionan un alto rendimiento y una baja latencia de forma coherente en menos de 10 milisegundos en la mayoría de las operaciones de E/S para las cargas de trabajo con un uso intensivo de E/S, lo que hace que sean adecuados para una amplia variedad de cargas de trabajo, como bases de datos, hospedaje de sitios web, y entornos de desarrollo. Los recursos compartidos de archivos Premium solo están disponibles en un modelo de facturación aprovisionada. Para más información sobre el modelo de facturación aprovisionado para recursos compartidos de archivos Premium, consulte [Planeamiento de una implementación de Azure Files](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares).
- **Recursos compartidos de archivos estándar**: Los recursos compartidos de archivos estándar están respaldados por unidades de disco duro (HDD) y se implementan en el tipo de **cuenta de almacenamiento de uso general versión 2 (GPv2)** . Los recursos compartidos de archivos estándar ofrecen un rendimiento confiable para cargas de trabajo de E/S menos sensibles a la variabilidad del rendimiento, como recursos compartidos de archivos de uso general y entornos de desarrollo y pruebas. Los recursos compartidos de archivos estándar solo están disponibles en un modelo de facturación de pago por uso.