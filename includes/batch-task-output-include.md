---
title: archivo de inclusión
description: archivo de inclusión
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: eaa76153fe96b5fd41166b20770e0a969aa9260d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83797169"
---
Una tarea que se ejecuta en Azure Batch puede generar datos de salida cuando se ejecuta. Los datos de salida de tareas se suelen almacenar para su recuperación por parte de otras tareas del trabajo, la aplicación cliente que ejecuta el trabajo o ambos. Las tareas escriben datos de salida en el sistema de archivos de un nodo de proceso de Batch, pero todos los datos del nodo se pierden al restablecer su imagen inicial o cuando el nodo abandona el grupo. Las tareas también pueden tener un período de retención de archivos, después del cual se eliminan los archivos creados por la tarea. Por estos motivos, es importante conservar el resultado de la tarea, que podría necesitar más adelante, en un almacén de datos como [Azure Storage](https://docs.microsoft.com/azure/storage/).

Para conocer las opciones de cuenta de almacenamiento en Batch, consulte [Cuentas de Batch y de Azure Storage](../articles/batch/accounts.md#azure-storage-accounts).
