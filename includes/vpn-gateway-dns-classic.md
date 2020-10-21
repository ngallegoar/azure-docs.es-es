---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/08/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0c5e3a095b312d36cb05a78e84a13bc2a73d95d5
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91875593"
---
La configuración de DNS no es un paso necesario de esta configuración, pero el servidor DNS es necesario si quiere resolver los nombres de las VM. La especificación de un valor no crea un servidor DNS nuevo. La dirección IP del servidor DNS que especifique debe ser un servidor DNS que pueda resolver los nombres de los recursos a los que se conecta.

Después de crear la red virtual, puede agregar la dirección IP de un servidor DNS para controlar la resolución de nombres. Abra la configuración de su red virtual, seleccione los servidores DNS y agregue la dirección IP del servidor DNS que quiere utilizar para la resolución de nombres.

1. Busque la red virtual en el portal.
2. En la página de la red virtual, en la sección **Configuración**, haga clic en **Servidores DNS**.
3. Agregue un servidor DNS.
4. Para guardar la configuración, haga clic en **Guardar** en la parte superior de la página.