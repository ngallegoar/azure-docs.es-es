---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: cdcbe993bd1100b2060a1f8d38eb82ac97121c0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74851642"
---
Azure Storage se factura en función del uso que se haga de la cuenta de almacenamiento. Todos los objetos de una cuenta de almacenamiento se facturan juntos como un grupo. 

Para calcular los costos de almacenamiento se tienen en cuenta los siguientes factores: 

* **Región** hace referencia a la región geográfica en la que radica la cuenta.
* **Tipo de cuenta** hace referencia al tipo de cuenta de almacenamiento que se usa. 
* **Nivel de acceso** hace referencia al patrón de uso de datos que ha especificado para la cuenta de uso general v2 o la cuenta de Blob Storage.
* **Capacidad** de almacenamiento hace referencia a la cuantificación de la asignación correspondiente a su cuenta de almacenamiento que utiliza para almacenar datos.
* La **replicación** determina cuántas copias de los datos se conservan al mismo tiempo, y en qué ubicaciones.
* Las **transacciones** se refieren a todas las operaciones de lectura y escritura en Azure Storage.
* La **salida de los datos** se refiere a los datos transferidos fuera de una región de Azure. Cuando una aplicación que no se ejecuta en la misma región que su cuenta de almacenamiento accede a los datos de esta, se le cobra por la salida de los datos. Para obtener información sobre el uso de grupos de recursos para agrupar los datos y servicios en la misma región a fin de limitar los cargos de salida, vea [¿Qué es un grupo de recursos de Azure?](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group). 

La página [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/) proporciona información detallada sobre los precios en función del tipo de cuenta, de la capacidad de almacenamiento, de la replicación y de las transacciones. La página [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/) proporciona información detallada sobre los precios para la salida de datos. Puede usar la [Calculadora de precios de Azure Storage](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) para ayudarle a calcular los costes.

