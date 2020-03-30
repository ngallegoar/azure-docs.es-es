---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: ee80c04a6365c2cf337c4033a90df8d2993c299d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76845985"
---
Los límites siguientes se aplican a temas del sistema y temas personalizados de Azure Event Grid, *no* a dominios de eventos.

| Resource | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |
| Solicitudes de publicación | 250 por segundo |
| Tamaño del evento | 1 MB (se cobra como varios eventos de 64 KB) |

Los límites siguientes se aplican solo a dominios de eventos.

| Resource | Límite |
| --- | --- |
| Temas por dominio de eventos | 100 000 |
| Suscripciones de eventos por tema dentro de un dominio | 500 |
| Suscripciones de eventos del ámbito de dominio | 50 |
| Tasa de publicación para un dominio de eventos (entrada) | 5000 eventos por segundo |
| Solicitudes de publicación | 250 por segundo |
| Dominios de eventos por suscripción de Azure | 100 |