---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a5bde1a56bf6a1f5fca4b775c7c8e9bb7477eb6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "66240266"
---
1. Abra el proyecto de cliente descargado mediante Xcode.

2. Vaya a [Azure Portal](https://portal.azure.com/) y diríjase a la aplicación móvil que ha creado. En la hoja `Overview`, busque la dirección URL que es el punto de conexión público de la aplicación móvil. Por ejemplo, el nombre de sitio para el nombre de mi aplicación "test123" será https://test123.azurewebsites.net.

3. Para proyectos Swift, abra el archivo `ToDoTableViewController.swift` en esta carpeta: ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift. El nombre de la aplicación es `ZUMOAPPNAME`.

4. En el método `viewDidLoad()`, reemplace el parámetro `ZUMOAPPURL` por el punto de conexión público anterior.

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    se convierte en
    
    `let client = MSClient(applicationURLString: "https://test123.azurewebsites.net")`
    
5. Para proyectos de Objective-C, abra el archivo `QSTodoService.m` en esta carpeta: ZUMOAPPNAME/ZUMOAPPNAME. El nombre de la aplicación es `ZUMOAPPNAME`.

6. En el método `init`, reemplace el parámetro `ZUMOAPPURL` por el punto de conexión público anterior.

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    se convierte en
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.azurewebsites.net"];`

7. Presione el botón **Ejecutar** para compilar el proyecto e iniciar la aplicación en el simulador de iOS.

8. En la aplicación, haga clic en el icono con el signo más ( **+** ), escriba texto que tenga sentido, como *Completar el tutorial* y, luego, haga clic en el botón para guardar. Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta los datos de la solicitud en la tabla SQL TodoItem y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.

   ![Aplicación de inicio rápido que se ejecuta en iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
