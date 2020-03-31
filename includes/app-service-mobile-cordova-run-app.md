---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 8d7731480b6239c572d39f52b6a0217d2ac48d25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "66240222"
---
1. Vaya al archivo de la solución del proyecto de cliente (.sln) y ábralo con Visual Studio.

2. En Visual Studio, elija la plataforma de la solución (Android, iOS o Windows) en la lista desplegable situada junto a la flecha de inicio. Para seleccionar un dispositivo de implementación o un emulador específicos, haga clic en la lista desplegable de la flecha verde. Puede usar la plataforma Android y el emulador Rippler predeterminados. Para tutoriales más avanzados (por ejemplo, las notificaciones de inserción), deberá seleccionar un dispositivo o emulador compatibles.

3. Abra el archivo `ToDoActivity.java` en esta carpeta ZUMOAPPNAME/aplicación/src/main/java/com/example/zumoappname. El nombre de la aplicación es `ZUMOAPPNAME`.

4. Vaya a [Azure Portal](https://portal.azure.com/) y diríjase a la aplicación móvil que ha creado. En la hoja `Overview`, busque la dirección URL que es el punto de conexión público de la aplicación móvil. Por ejemplo, el nombre de sitio para el nombre de mi aplicación "test123" será https://test123.azurewebsites.net.

5. Vaya al archivo `index.js` en ZUMOAPPNAME/www/js/index.js y, en el método `onDeviceReady()`, reemplace el parámetro `ZUMOAPPURL` por el punto de conexión público anterior.

    `client = new WindowsAzure.MobileServiceClient('ZUMOAPPURL');`
    
    se convierte en
    
    `client = new WindowsAzure.MobileServiceClient('https://test123.azurewebsites.net');`
    
6. Presione F5 o haga clic en la flecha verde para compilar y ejecutar su aplicación Cordova. Si ve en el emulador un cuadro de diálogo de seguridad que solicita el acceso a la red, acéptelo.

7. Después de que la aplicación se inicia en el dispositivo o el emulador, escriba texto significativo en **Enter new text** (Escribir nuevo texto), por ejemplo, *Completar el tutorial* y, luego, haga clic en el botón **Add** (Agregar).

El back-end inserta datos de la solicitud en la tabla TodoItem de SQL Database y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.

Puede repetir los pasos del 3 al 5 para otras plataformas.