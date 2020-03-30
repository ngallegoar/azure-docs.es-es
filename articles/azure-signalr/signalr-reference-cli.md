---
title: 'Ejemplos de la CLI de Azure: Azure SignalR Service'
description: Siga ejemplos reales para obtener información sobre la CLI de Azure para Azure SignalR Service. Aprenderá a crear una instancia de SignalR Service con más servicios de Azure.
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 3c28781362e60d22b0d9a4e3754daaa24e5b61ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74158130"
---
# <a name="azure-cli-reference"></a>Referencia de la CLI de Azure

En la tabla siguiente se incluyen vínculos a scripts de Bash para Azure SignalR Service creados con la CLI de Azure.

| | |
|-|-|
|**Creación**||
| [Creación de una instancia nueva de SignalR Service y un nuevo grupo de recursos](scripts/signalr-cli-create-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio.  |
|**Integración**||
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR](scripts/signalr-cli-create-with-app-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación web y un nuevo plan de App Service para hospedar una aplicación web ASP.NET Core que usa la instancia de SignalR Service. La aplicación web se configura con una configuración de aplicación para conectar con el nuevo recurso de SignalR Service. |
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR y GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación web de Azure y un nuevo plan para hospedar una aplicación web ASP.NET Core que usa la instancia de SignalR Service. La aplicación web está configurada con los valores de la aplicación para la cadena de conexión al nuevo recurso de SignalR Service y los secretos de cliente para admitir la [autenticación de GitHub](https://developer.github.com/v3/guides/basics-of-authentication/), como se muestra en el [tutorial de autenticación](signalr-concept-authenticate-oauth.md). La aplicación web también está configurada para usar un origen de implementación de repositorio de Git local. |
| | |
