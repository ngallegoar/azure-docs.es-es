---
title: 'Problemas conocidos: Azure Digital Twins'
description: Obtenga ayuda para reconocer y mitigar los problemas conocidos de Azure Digital Twins.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
ms.openlocfilehash: 8fce451d9b806d2fa9a4f3d9e1c117de0aaa9fc0
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528310"
---
# <a name="known-issues-in-azure-digital-twins"></a>Problemas conocidos en Azure Digital Twins

En este artículo se proporciona información sobre los problemas conocidos asociados a Azure Digital Twins.

## <a name="managing-event-routes-in-the-azure-portal"></a>Administración de rutas de eventos en Azure Portal

Si ha iniciado sesión en el portal con una [**cuenta de Microsoft (MSA)** ](https://account.microsoft.com/account/Account) personal, como una cuenta de *@outlook.com* , verá una pantalla que indica *You need permission to view event routes* (Necesita permiso para ver las rutas de evento) al intentar administrar las rutas de eventos en el portal, independientemente de su nivel de permiso.

:::image type="content" source="media/troubleshoot-known-issues/event-route-need-permission.png" alt-text="Captura de pantalla del error de permisos en Azure Portal al intentar crear rutas de evento en una instancia de Azure Digital Twins":::

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

Los usuarios que actualmente no pueden administrar las rutas de eventos en el portal aún pueden administrar las rutas de eventos mediante las API de Azure Digital Twins o la CLI. La estrategia recomendada para mitigar este problema es cambiar a una de estas herramientas para la administración de rutas de eventos.

Las instrucciones para esto se pueden encontrar en [*Procedimiento: Administración de puntos de conexión y rutas*](how-to-manage-routes.md).

### <a name="possible-causes"></a>Causas posibles

Ha iniciado sesión en el portal con una [cuenta de Microsoft de (MSA)](https://account.microsoft.com/account/Account) personal, como una cuenta de *@outlook.com* . La administración de rutas de eventos en Azure Portal actualmente solo está disponible para los usuarios de Azure en cuentas de dominio corporativo.

## <a name="400-client-error-bad-request-in-cloud-shell"></a>"400 Error de cliente: Solicitud incorrecta" en Cloud Shell

Los comandos de Cloud Shell pueden producir intermitentemente el error "400 Error de cliente: Solicitud incorrecta de la dirección URL: http://localhost:50342/oauth2/token"seguida del seguimiento de la pila completa.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

Esto puede resolverse si se vuelve a ejecutar el comando `az login` y se completan los pasos de inicio de sesión posteriores.

Después de esto, debería poder ejecutar el comando nuevamente.

### <a name="possible-causes"></a>Causas posibles

Este es el resultado de un problema conocido en Cloud Shell: [*Error intermitente al obtener el token de Cloud Shell con 400 Error de cliente: Solicitud incorrecta*](https://github.com/Azure/azure-cli/issues/11749).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la seguridad y los permisos de Azure Digital Twins:
* [*Conceptos: Seguridad para las soluciones de Azure Digital Twins*](concepts-security.md)