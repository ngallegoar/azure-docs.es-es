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
ms.openlocfilehash: e084256d9c2043d4382ca180ef3178175b301367
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91745453"
---
Los siguientes límites se aplican a los **temas** de Azure Event Grid (temas del sistema, personalizados y de asociados). 

| Resource | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado o de asociado (entrada) | 5000 eventos por segundo o 1 MB por segundo (lo que se cumpla primero)<br/>No se aplica a los temas del sistema. |
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


