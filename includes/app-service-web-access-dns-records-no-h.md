---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 66d3397fae24ee2546dae4eb5d7c9d188f9ede99
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2020
ms.locfileid: "78944098"
---
> [!NOTE]
> Puede utilizar Azure DNS para configurar un nombre DNS personalizado para Azure App Service. Para más información, consulte [Usar Azure DNS para proporcionar la configuración de un dominio personalizado para un servicio de Azure](../articles/dns/dns-custom-domain.md#app-service-web-apps).
>
>

Inicie sesión en el sitio web de su proveedor de dominios.

Busque la página de administración de registros DNS. Cada proveedor de dominios tiene su propia interfaz de registros DNS, así que consulte la documentación del proveedor. Busque áreas del sitio etiquetadas como **Nombre de dominio**, **DNS** o **Administración del servidor del nombres**. 

A menudo, se puede encontrar la página de registros DNS al ver la información de la cuenta y al buscar un vínculo, como **Mis dominios**. Vaya a dicha página y, después, busque un vínculo que se llame algo así como **Zone file**, **DNS Records** o **Advanced configuration**.

La captura de pantalla siguiente es un ejemplo de página de registros DNS:

![Página de registros DNS de ejemplo](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

En la captura de pantalla de ejemplo, seleccione **Agregar** para crear un registro. Algunos proveedores tienen diferentes vínculos para agregar diferentes tipos de registros. De nuevo, consulte la documentación del proveedor.

> [!NOTE]
> En el caso de ciertos proveedores, como GoDaddy, los cambios en los registros DNS no entran en vigor hasta que se selecciona un vínculo **Guardar cambios** independiente. 
