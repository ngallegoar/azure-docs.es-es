---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 10/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b884bc72381c98af77f2f49336f3dd5762c68734
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91859755"
---
Los siguientes límites se aplican a los **temas** de Azure Event Grid (temas del sistema, personalizados y de asociados). 

| Resource | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado o de asociado (entrada) | 5000 eventos por segundo o 1 MB por segundo (lo que se cumpla primero) |
| Tamaño del evento | 1 MB  |
| Conexiones de punto de conexión privado por tema  | 64 | 
| Reglas de firewall de IP por tema | 16 | 

Los límites siguientes se aplican a los **dominios** de Azure Event Grid. 

| Resource | Límite |
| --- | --- |
| Temas por dominio de eventos | 100 000 |
| Suscripciones de eventos por tema dentro de un dominio | 500 |
| Suscripciones de eventos del ámbito de dominio | 50 |
| Tasa de publicación para un dominio de eventos (entrada) | 5000 eventos por segundo o 1 MB por segundo (lo que se cumpla primero) |
| Dominios de eventos por suscripción de Azure | 100 |
| Conexiones de punto de conexión privado por dominio | 64 | 
| Reglas de firewall de IP por dominio | 16 | 


