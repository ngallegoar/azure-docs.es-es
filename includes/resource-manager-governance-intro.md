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
ms.openlocfilehash: d1e7fa1ed1649508f0d4923db8817d17ad556ca1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186245"
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

Este artículo se centra en las tareas que se realizan para implementar el sistema de gobernanza. Para obtener una explicación más amplia de los conceptos, consulte [Sistema de gobernanza en Azure](../articles/security/governance-in-azure.md). 
