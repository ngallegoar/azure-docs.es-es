---
title: Cuotas en la versión preliminar de Azure Static Web Apps
description: Más información sobre las cuotas asociadas a la versión preliminar de Azure Static Web Apps
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 6fa70eba5875c32d41fa1cfc8daf9b1cdf8f19df
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "83599114"
---
# <a name="quotas-in-azure-static-web-apps-preview"></a>Cuotas en la versión preliminar de Azure Static Web Apps

Existen las siguientes cuotas para la versión preliminar de Azure Static Web Apps.

> [!IMPORTANT]
> Azure Static Web Apps se encuentra en versión preliminar pública y no está diseñado para su uso en producción.

| Característica                     | Plan gratuito        |
|-----------------------------|------------------|
| Ancho de banda incluido          | 100 GB al mes |
| Ancho de banda de uso por encima del límite           | No disponible      |
| Aplicaciones por suscripción a Azure | 10               |
| Tamaño de la aplicación                    | 100 MB           |
| Entornos de preproducción | 1                |
| Dominios personalizados              | 1                |
| Authorization<br><br>Con roles personalizados y reglas de enrutamiento | Máximo de 25 usuarios finales invitados y roles asignados |
| Azure Functions             | Disponible        |
| Contrato de nivel de servicio                         | None             |

## <a name="github-storage"></a>Almacenamiento de GitHub

Los paquetes y las Acciones de GitHub usan el almacenamiento de GitHub, que tiene su propio conjunto de cuotas. Cuando crea un sitio de Azure Static Web Apps, GitHub almacena los artefactos para el sitio incluso después de la implementación.

Para obtener más información, consulte los siguientes recursos.

- [Administración del espacio de almacenamiento de acciones](https://github.community/t5/GitHub-Actions/Managing-Actions-storage-space/td-p/38944)
- [Acerca de la facturación para las Acciones de GitHub](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions#about-billing-for-github-actions)
- [Administración del límite de gasto para las acciones de GitHub](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/managing-your-spending-limit-for-github-actions)

## <a name="next-steps"></a>Pasos siguientes

- [Información general](overview.md)
