---
title: Conexión de ExpressRoute a la puerta de enlace de red virtual
description: Pasos para conectar ExpressRoute a la puerta de enlace de red virtual.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 214ef9c01193b238c8e456ef2809f7a2edbdb6c7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91598159"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->

1. Vaya a la nube privada que creó en el tutorial anterior y seleccione **Conectividad**  en **Administrar** y seleccione la pestaña **ExpressRoute**.

1. Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione **+ Solicitar una clave de autorización**.

   :::image type="content" source="../media/expressroute-global-reach/start-request-auth-key.png" alt-text="Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione + Solicitar una clave de autorización." border="true" lightbox="../media/expressroute-global-reach/start-request-auth-key.png":::

1. Vaya a la puerta de enlace de red virtual que creó en el paso anterior y, en **Configuración**, seleccione **Conexiones**. En la página **Conexiones**, seleccione **+ Agregar**.

1. En la página **Agregar conexión**, proporcione valores para los campos y seleccione **Aceptar**. 

   | Campo | Value |
   | --- | --- |
   | **Nombre**  | Escriba un nombre para la conexión.  |
   | **Tipo de conexión**  | seleccione **ExpressRoute**.  |
   | **Canjear autorización**  | Asegúrese de que este cuadro está seleccionado.  |
   | **Puerta de enlace de red virtual** | La puerta de enlace de red virtual que creó antes.  |
   | **Clave de autorización**  | Copie y pegue la clave de autorización de la pestaña de ExpressRoute para el grupo de recursos. |
   | **URI de circuito del mismo nivel**  | Copie y pegue el identificador de ExpressRoute de la pestaña de ExpressRoute para el grupo de recursos.  |

   :::image type="content" source="../media/expressroute-global-reach/open-cloud-shell.png" alt-text="Copie la clave de autorización. Si no hay ninguna clave de autorización, debe crearla; para ello, seleccione + Solicitar una clave de autorización." border="true" lightbox="../media/expressroute-global-reach/open-cloud-shell.png":::

Se crea la conexión entre el circuito ExpressRoute y la red virtual.