---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 55b545471cbe45fe35e28879270e4340cf9d3834
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026746"
---
Al implementar recursos en Azure, dispone de una enorme flexibilidad a la hora de decidir qué tipos de recursos implementar, dónde se encuentran y cómo configurarlos. Sin embargo, esa flexibilidad puede abrirle a más opciones que le gustaría permitir en su organización. Cuando piensa en implementar recursos en Azure, tal vez se pregunte:

* ¿Cómo puedo cumplir los requisitos legales de soberanía de datos en determinados países o regiones?
* ¿Cómo puedo controlar los costos?
* ¿Cómo puedo asegurarme de que alguien no modifique por error un sistema importante?
* ¿Cómo puedo realizar un seguimiento de los costos de los recursos y facturarlos de forma precisa?

En este artículo se da respuesta a esas preguntas. En concreto, puede:

> [!div class="checklist"]
> * Asignar roles a los usuarios y asignar roles a un ámbito, de forma que los usuarios tengan permiso para realizar acciones esperadas, pero no más acciones.
> * Aplicar directivas que recomienden convenciones para los recursos de su suscripción.
> * Bloquear los recursos que son críticos para el sistema.
> * Etiquetar recursos para que pueda realizar su seguimiento según los valores que encajan con su organización.

Este artículo se centra en las tareas que se realizan para implementar el sistema de gobernanza. Para obtener una explicación más amplia de los conceptos, consulte [Sistema de gobernanza en Azure](../articles/governance/index.yml).