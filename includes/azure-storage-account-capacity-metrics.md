---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 48e2d798a3dcf7354e68d07785c2762ba03f39f8
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011173"
---
En esta tabla se muestran [métricas de nivel de cuenta](../articles/azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts).

| Métrica | Descripción |
| ------------------- | ----------------- |
| UsedCapacity | La cantidad de almacenamiento que utiliza la cuenta de almacenamiento. En las cuentas de almacenamiento estándar, es la suma de la capacidad usada por un blob, una tabla, un archivo y una cola. Tanto en las cuentas de almacenamiento Premium como en las cuentas de Blob Storage, coincide con BlobCapacity. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |