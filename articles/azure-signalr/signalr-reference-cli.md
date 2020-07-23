---
title: 'Ejemplos de la CLI de Azure: Azure SignalR Service'
description: Siga ejemplos reales para obtener información sobre la CLI de Azure para Azure SignalR Service. Aprenderá a crear una instancia de SignalR Service con más servicios de Azure.
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 99ebe9c078ef4775e8f0df0545fb0af4b3d1057b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515247"
---
# <a name="azure-cli-reference"></a>Referencia de la CLI de Azure

En la tabla siguiente se incluyen vínculos a scripts de Bash para Azure SignalR Service creados con la CLI de Azure.

| Script | Descripciones |
|-|-|
|**Creación**||
| [Creación de una instancia nueva de SignalR Service y un nuevo grupo de recursos](scripts/signalr-cli-create-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio.  |
|**Integración**||
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR](scripts/signalr-cli-create-with-app-service.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación web y un nuevo plan de App Service para hospedar una aplicación web ASP.NET Core que usa la instancia de SignalR Service. La aplicación web se configura con una configuración de aplicación para conectar con el nuevo recurso de SignalR Service. |
| [Creación de una instancia nueva de SignalR Service y una aplicación web configurada para usar SignalR y GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Crea un nuevo recurso de Azure SignalR Service en un nuevo grupo de recursos con un nombre aleatorio. También agrega una nueva aplicación web de Azure y un nuevo plan para hospedar una aplicación web ASP.NET Core que usa la instancia de SignalR Service. La aplicación web está configurada con los valores de la aplicación para la cadena de conexión al nuevo recurso de SignalR Service y los secretos de cliente para admitir la [autenticación de GitHub](https://developer.github.com/v3/guides/basics-of-authentication/), como se muestra en el [tutorial de autenticación](signalr-concept-authenticate-oauth.md). La aplicación web también está configurada para usar un origen de implementación de repositorio de Git local. |
| | |
