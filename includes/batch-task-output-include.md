---
title: archivo de inclusión
description: archivo de inclusión
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186668"
---
Una tarea que se ejecuta en Azure Batch puede generar datos de salida cuando se ejecuta. Los datos de salida de tareas se suelen almacenar para su recuperación por parte de otras tareas del trabajo, la aplicación cliente que ejecuta el trabajo o ambos. Las tareas escriben datos de salida en el sistema de archivos de un nodo de proceso de Batch, pero todos los datos del nodo se pierden al restablecer su imagen inicial o cuando el nodo abandona el grupo. Las tareas también pueden tener un período de retención de archivos, después del cual se eliminan los archivos creados por la tarea. Por estos motivos, es importante conservar el resultado de la tarea, que podría necesitar más adelante, en un almacén de datos como [Azure Storage](https://docs.microsoft.com/azure/storage/).

Para conocer las opciones de cuenta de almacenamiento de Batch, consulte la [introducción a la característica de Batch](../articles/batch/batch-api-basics.md#azure-storage-account).
