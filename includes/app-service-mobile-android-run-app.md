---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: 63c54f8af91b6b4a76ba49d5e6fc7b3cda9f5b98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "66240309"
---
1. Abra el proyecto mediante **Android Studio**, con **Importar proyecto (Eclipse ADT, Gradle, etc.)** . Asegúrese de que realice esta selección de importación para evitar los errores del JDK.

2. Abra el archivo `ToDoActivity.java` en esta carpeta ZUMOAPPNAME/aplicación/src/main/java/com/example/zumoappname. El nombre de la aplicación es `ZUMOAPPNAME`.

3. Vaya a [Azure Portal](https://portal.azure.com/) y diríjase a la aplicación móvil que ha creado. En la hoja `Overview`, busque la dirección URL que es el punto de conexión público de la aplicación móvil. Por ejemplo, el nombre de sitio para el nombre de mi aplicación "test123" será https://test123.azurewebsites.net.

4. En el método `onCreate()`, reemplace el parámetro `ZUMOAPPURL` por el punto de conexión público anterior.
    
    `new MobileServiceClient("ZUMOAPPURL", this).withFilter(new ProgressFilter());` 
    
    se convierte en
    
    `new MobileServiceClient("https://test123.azurewebsites.net", this).withFilter(new ProgressFilter());`
    
5. Presione el botón **Ejecutar "aplicación"** para compilar el proyecto e iniciar la aplicación en el simulador de Android.

4. En la aplicación, escriba un texto significativo, como *Realizar el tutorial* y luego haga clic en el botón "Agregar". Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta los datos de la solicitud en la tabla SQL TodoItem y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.
    ![Inicio rápido de Android](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)