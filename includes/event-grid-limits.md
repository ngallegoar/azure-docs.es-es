---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 05/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e7a0424e4454639211c6494aab0700e75269361
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83720968"
---
Los límites siguientes se aplican a temas del sistema y temas personalizados de Azure Event Grid, *no* a dominios de eventos.

| Resource | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |
| Tamaño del evento | 1 MB. No obstante, las operaciones se cobran en incrementos de 64 KB. Por lo tanto, los eventos de más de 64 KB incurrirán en cargos de operaciones como si fueran varios eventos. Por ejemplo, un evento que tenga 130 KB incurrirá en operaciones como si fueran tres eventos independientes.  |
| Temas por dominio de eventos | 100 000 |
| Suscripciones de eventos por tema dentro de un dominio | 500 |
| Suscripciones de eventos del ámbito de dominio | 50 |
| Tasa de publicación para un dominio de eventos (entrada) | 5000 eventos por segundo |
| Dominios de eventos por suscripción de Azure | 100 |
| Conexiones de punto de conexión privado por tema o dominio | 64 | 
| Reglas de firewall de IP por tema o dominio | 16 | 