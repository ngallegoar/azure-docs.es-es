---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2020
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: bcc55156ca1d03614a4ff9767d6cf3f2603c06ca
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027808"
---
Agregue compatibilidad en su entorno de desarrollo preferido mediante los métodos siguientes.

| Entorno de desarrollo  | Tipo de aplicación      | Para agregar compatibilidad |
|--------------------------|-----------------------|----------------|
| Visual Studio            | Biblioteca de clases de C#      | [Instalar el paquete de NuGet](../articles/azure-functions/functions-bindings-register.md#vs) |
| Visual Studio Code       | Basada en [Core Tools](../articles/azure-functions/functions-run-local.md) | [Registrar la agrupación de extensiones](../articles/azure-functions/functions-bindings-register.md#extension-bundles)<br><br>Se recomienda instalar la [extensión Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack). |
| Cualquier otro editor o IDE     | Basada en [Core Tools](../articles/azure-functions/functions-run-local.md) | [Registrar la agrupación de extensiones](../articles/azure-functions/functions-bindings-register.md#extension-bundles) |
| Azure Portal             | Solo en línea en el portal | Se instala al agregar un enlace<br /><br /> Consulte [Actualización de las extensiones](../articles/azure-functions/functions-bindings-register.md) para actualizar las extensiones de enlace existentes si tener que volver a publicar la aplicación de función. |