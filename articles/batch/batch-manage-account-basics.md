---
title: Administración de la cuenta
description: Obtenga información sobre los componentes de los que consta una cuenta de Azure Batch.
ms.topic: how-to
ms.date: 03/30/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ef87fab099c6ff7402b351664ee15a198d69a4b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "83722973"
---
# <a name="manage-your-batch-account"></a>Administración de una cuenta de Batch

Una cuenta de Batch es una entidad identificada de forma exclusiva en el servicio Batch. Todo el procesamiento se asocia con una cuenta de Batch.

Puede crear una cuenta de Azure Batch mediante [Azure Portal](batch-account-create-portal.md) o mediante programación, como con la [biblioteca Batch Management .NET](batch-management-dotnet.md). Al crear la cuenta, puede asociar una cuenta de Azure Storage para almacenar los datos o aplicaciones de entrada y salida relacionados con los trabajos.

Se pueden ejecutar varias cargas de trabajo de Batch en una sola cuenta de Batch, o bien distribuir las cargas de trabajo entre cuentas de Batch que se encuentren en la misma suscripción, pero en diferentes regiones de Azure.

## <a name="components-of-the-batch-account"></a>Componentes de la cuenta de Batch

La cuenta de Batch le permite ejecutar de forma eficaz en Azure trabajos por lotes de informática de alto rendimiento (HPC) en paralelo y a gran escala. Dentro de la cuenta que administra:

- Las aplicaciones que está ejecutando.

- La asignación de grupos y nodos dentro de los grupos.

- El número y los tipos de tareas. 

- La entrada y salida de datos. No es necesario instalar software adicional para administrar las tareas.

- Al crear la cuenta de Batch se le pedirá que le asigne un nombre. Este nombre es su identificador y, una vez asignado, no se puede cambiar.

- Para cambiar el nombre de una cuenta, debe eliminarla y crear una cuenta de Batch nueva.

- La cuenta se crea dentro de la suscripción que quiere usar.

- Use la cuenta para identificar y recuperar las claves de cuenta principal y secundaria de cualquier cuenta de Batch de su suscripción.

- La cuenta mantiene información sobre la asignación de grupos y las cuotas de núcleos.  

- La cuenta contiene información de ubicación.

- La cuenta identifica la cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

- Cree una cuenta de Batch con [Azure Portal](batch-account-create-portal.md).
- Cree una cuenta de Batch mediante programación, por ejemplo, con la [biblioteca Batch Management .NET](batch-management-dotnet.md).
- [Configure o deshabilite el acceso remoto a nodos de proceso en un grupo de Azure Batch](pool-endpoint-configuration.md).
- [Ejecución de tareas de preparación y liberación de trabajos en nodos de proceso de Batch](batch-job-prep-release.md)

