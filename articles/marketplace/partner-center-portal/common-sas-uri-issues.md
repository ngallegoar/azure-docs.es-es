---
title: Problemas comunes y correcciones de los identificadores URI de SAS - Azure Marketplace
description: Problemas comunes detectados y resoluciones recomendadas al trabajar con firmas de acceso compartido.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: anbene
ms.author: mingshen
ms.date: 04/09/2020
ms.openlocfilehash: 95ce37d92adc3d09c5a09944b094df7971831198
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86110732"
---
# <a name="common-sas-uri-issues-and-fixes"></a>Problemas comunes y correcciones de los identificadores URI de SAS

A continuación, se presentan los problemas comunes detectados al trabajar con firmas de acceso compartido (que se usan para identificar y compartir los discos duros virtuales cargados para la solución), junto con las resoluciones sugeridas.

| **Problema** | **Mensaje de error** | **Revisión** |
| --------- | ------------------- | ------- |
| *Error al copiar imágenes* |  |  |
| "?" no se encuentra en el identificador URI de SAS | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice el identificador URI de SAS con las herramientas recomendadas. |
| Los parámetros "st" y "se" no están en el identificador URI de SAS | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice el identificador URI de SAS con los valores **Fecha de inicio** y **Fecha de finalización** adecuados. |
| "sp=rl" no aparece en el identificador URI de SAS | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice el identificador URI de SAS con los permisos establecidos como `Read` y `List`. |
| El identificador URI de SAS tiene espacios en blanco en el nombre del disco duro virtual | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Actualice el identificador URI de SAS para quitar los espacios en blanco. |
| Error de autorización del identificador URI de SAS | `Failure: Copying Images. Not able to download blob due to authorization error.` | Revise y corrija el formato del URI de SAS. Vuelva a generarlo si es necesario. |
| Los parámetros "st" y "se" del identificador URI de SAS no tienen una especificación de fecha y hora completa | `Failure: Copying Images. Not able to download blob due to incorrect SAS Uri.` | Los parámetros **Fecha de inicio** y **Fecha de finalización** del identificador URI de SAS (las substrings `st` y `se`) deben tener un formato de fecha y hora completo, como `11-02-2017T00:00:00Z`. Las versiones abreviadas no son válidas (algunos comandos de la CLI de Azure pueden generar valores abreviados de forma predeterminada). |
|  |  |  |

Para obtener información detallada, vea [Uso de firmas de acceso compartido (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
