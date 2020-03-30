---
title: archivo de inclusión
description: archivo de inclusión
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 06/20/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 72a69359d412a7560472fbb73ec525ab5d4a4fce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67325802"
---
1. Inicie sesión en [Azure Portal].

2. Haga clic en **Crear un recurso**.

3. En el cuadro de búsqueda, escriba **Web App**.
    
4. En la lista de resultados, seleccione **Web App** en Marketplace.

5. Seleccione los valores de **Suscripción** y **Grupo de recursos** (seleccione un grupo de recursos existente _o_ cree uno nuevo [con el mismo nombre que el de la aplicación]).

6. Elija un valor de **Nombre** único de la aplicación web.

7. Elija la opción predeterminada **Publicar** como **Código**.

8. En **Pila en tiempo de ejecución**, debe seleccionar una versión en **ASP.NET** o **Nodo**. Se está creando un back-end de .NET, seleccione una versión bajo ASP.NET. De lo contrario, si se dirige a una aplicación basada en nodos, seleccione una de las versiones en Nodo.

9. Seleccione el valor correcto de **Sistema operativo** (Linux o Windows). 

10. Seleccione el valor de **Región** donde quiere que se implemente esta aplicación. 

11. Seleccione el valor adecuado de **Plan de App Service** y haga clic en **Revisar y crear**. 

12. En **Grupo de recursos**, seleccione un grupo de recursos existente _o_ cree uno nuevo (con el mismo nombre que su aplicación).

13. Haga clic en **Crear**. Espere unos minutos para que se implemente el servicio correctamente antes de continuar. Observe el icono de notificaciones (campana) en el encabezado del portal para las actualizaciones de estado.

14. Una vez completada la implementación, haga clic en la sección **Detalles de implementación** y seleccione el recurso de tipo **Microsoft.Web/sites**. Navegará a la aplicación web de App Service que acaba de crear. 

15. Haga clic en la hoja **Configuración** bajo **Configuración** y en **Configuración de la aplicación**, haga clic en el botón **Nueva configuración de la aplicación**.

16. En la página **Agregar o editar la configuración de la aplicación**, en el campo **Nombre** escriba **MobileAppsManagement_EXTENSION_VERSION**, en el campo Valor escriba **Más reciente** y haga clic en Aceptar.

Está listo para usar la aplicación web de App Service recién creada como una aplicación móvil.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/