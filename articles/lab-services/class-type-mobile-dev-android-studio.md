---
title: Configuración de un laboratorio para enseñar el desarrollo de aplicaciones móviles con Android Studio
titleSuffix: Azure Lab Services
description: Aprenda a configurar un laboratorio para enseñar la clase de desarrollo de aplicaciones móviles de datos que usa Android Studio.  En el artículo también se describen los ajustes que se deben realizar al usar Android Studio en una máquina virtual de Azure.
author: emaher
ms.topic: article
ms.date: 06/26/2020
ms.author: enewman
ms.openlocfilehash: ff996a096cddbb85f1e1c84cd051c18a7ab4ad79
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85444988"
---
# <a name="set-up-a-lab-to-teach-data-mobile-application-development-with-android-studio"></a>Configuración de un laboratorio para enseñar el desarrollo de aplicaciones móviles con Android Studio

En este artículo se muestra cómo configurar una clase de desarrollo de aplicaciones móviles de introducción.  Esta clase se centra en las aplicaciones móviles de Android que se pueden publicar en [Google Play Store](https://play.google.com/store/apps).  Los alumnos aprenderán a usar [Android Studio](https://developer.android.com/studio) para compilar aplicaciones.  El [Emulador de Visual Studio para Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) se usa para probar la aplicación localmente.

## <a name="lab-configuration"></a>Configuración del laboratorio

Para configurar este laboratorio, para empezar, necesita una suscripción a Azure y una cuenta de laboratorio. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. Una vez que obtenga una suscripción a Azure, puede crear una nueva cuenta de laboratorio en Azure Lab Services. Para más información sobre la creación de una nueva cuenta de laboratorio, consulte el [tutorial para configurar una cuenta de laboratorio](tutorial-setup-lab-account.md).  También puede usar una cuenta de laboratorio existente.

Siga el [tutorial para la configuración de un laboratorio de clase](tutorial-setup-classroom-lab.md) para crear un laboratorio y aplique la configuración siguiente:

| Tamaño de la máquina virtual | Imagen |
| -------------------- | ----- |
| Mediano (virtualización anidada) | Windows Server 2019 Datacenter |

## <a name="template-machine-configuration"></a>Configuración de la máquina de plantilla

Una vez creada la máquina de plantilla, [inicie la máquina y conéctese a ella](how-to-create-manage-template.md#update-a-template-vm) para completar las siguientes tareas:

1. Agregar un rol de Hyper-V.
2. Descargar e instalar Java.  
3. Descargar e instalar el Emulador de Visual Studio para Android.
4. Descargar e instalar Android Studio.
5. Configurar el Emulador de Visual Studio para Android Studio.

## <a name="add-hyper-v-role"></a>Agregar un rol de Hyper-V.

Hyper-V debe estar habilitado para la instalación correcta del Emulador de Visual Studio para Android.  Siga las instrucciones del artículo sobre la [habilitación de la virtualización anidada en una plantilla de máquina virtual](how-to-enable-nested-virtualization-template-vm.md).

## <a name="install-java"></a>Instalar Java

Android Studio requiere Java.  Siga los pasos que se indican a continuación para descargar la versión más reciente de Java.

1. Vaya a la [página de descarga de Java](https://www.java.com/download/). Haga clic en el botón de **descarga de Java**.
2. En la página web de Windows de 64 bits para Java, haga clic en el botón **Aceptar e iniciar descarga gratuita**.
3. Cuando aparezca el programa de **instalación de Java**, haga clic en **Instalar**.
4. Espere hasta que el título del instalador cambie a **Configuración de Java - Completar**.  Haga clic en el botón **Cerrar**.

## <a name="install-visual-studio-emulator-for-android"></a>Instalar el Emulador de Visual Studio para Android

Para probar una aplicación de Android localmente, debe usar una versión virtualizada de un dispositivo Android.  Hay algunos emuladores de Android disponibles que permitirán a los desarrolladores probar su aplicación desde su máquina.  Usamos el Emulador de Visual Studio para Android porque admite la virtualización anidada.  Dado que la VM del servicio de laboratorio ya es una máquina virtual, necesitamos un emulador que admita la virtualización anidada.  El emulador integrado para Android Studio no admite la virtualización anidada.  Para ver qué emuladores admiten la virtualización anidada, consulte [Aceleración de hardware para el rendimiento del emulador (Hyper-V y HAXM)](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration).

Siga las instrucciones que se indican a continuación para descargar e instalar el Emulador de Visual Studio para Android.

1. Navegue hasta la página principal del [Emulador de Visual Studio para Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/).
2. Haga clic en el botón de **descargar del emulador**.
3. Cuando se descargue vs_emulatorsetup.exe, ejecute el archivo ejecutable.
4. Cuando aparezca el cuadro de diálogo de instalación de Visual Studio, haga clic en el botón **Instalar**.
5. Espere hasta que el instalador se complete.  Haga clic en el botón **Reiniciar ahora** para reiniciar el equipo y finalizar la instalación.

Inicie el emulador antes de implementar la aplicación mediante Android Studio.  Para obtener más información sobre el Emulador de Visual Studio para Android, consulte la [documentación del Emulador de Visual Studio para Android](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-emulator-for-android).

## <a name="install-android-studio"></a>Instalar Android Studio

Siga las instrucciones que se indican a continuación para descargar e instalar [Android Studio](https://developer.android.com/studio).

1. Navegue a la [página de descarga de Android Studio](https://developer.android.com/studio#downloads).  
    > [!NOTE]
    > Internet Explorer no es compatible con este sitio.
2. Haga clic en el paquete de Android Studio ejecutable de Windows (64 bits).
3. Lea los términos legales que se especifican en el elemento emergente.  Cuando esté listo para continuar, seleccione la casilla **He leído y acepto los términos y condiciones anteriores** y haga clic en el botón **Descargar Android Studio para Windows**.
4. Una vez descargado el archivo ejecutable de instalación de Android Studio, ejecútelo.
5. En la página **Welcome to Android Studio Setup** (Instalación de Android Studio) del instalador de **Android Studio Setup** (Instalación de Android Studio), haga clic en **Siguiente**.
6. En la página **Opciones de configuración**, haga clic en **Siguiente**.
7. En la página **Choose Start Menu Folder** (Elegir la carpeta del menú Inicio), haga clic en **Instalar**.
8. Espere a que termine la instalación.
9. En la página **Instalación finalizada**, haga clic en **Siguiente**.
10. Se muestra la página **Completing Android Studio Setup** (Finalización de la instalación de Android Studio).  Haga clic en **Finalizar**
11. Android Studio se iniciará automáticamente una vez finalizada la instalación.
12. En el cuadro de diálogo **Import Android Settings From** (Importar la configuración de Android de), seleccione **Do not import settings** (No importar la configuración). Haga clic en **OK**.
13. En la página **Welcome** (Bienvenida) del **Android Studio Setup Wizard** (Asistente para la configuración de Android Studio), haga clic en **Siguiente**.
14. En la página **Tipo de instalación**, elija **Estándar**. Haga clic en **Next**.
15. En la página **Select UI Theme** (Seleccionar tema de la interfaz de usuario), seleccione tema deseado. Haga clic en **Next**.
16. En la página **Verify Settings** (Comprobar la configuración), haga clic en **Siguiente**.
17. En la página **Descargando componentes**, espere hasta que se hayan descargado todos los componentes.  Haga clic en **Finalizar**

    > [!IMPORTANT]
    > Se espera que se produzca un error en la instalación de HAXM.  HAXM no admite la virtualización anidada, motivo por el cual se instaló el Emulador de Visual Studio para Android anteriormente en este artículo.

18. Aparecerá el cuadro de diálogo **Welcome to Android Studio** (Le damos la bienvenida a Android Studio) cuando se complete el Asistente para la instalación.

## <a name="configure-android-studio-and-visual-studio-emulator-for-android"></a>Configurar Android Studio y el Emulador de Visual Studio para Android

Android Studio está casi listo para su uso.  Todavía es necesario indicar al Emulador de Visual Studio para Android dónde está instalado Android SDK.  De este modo, todos los emuladores que se ejecuten en Visual Studio para Android se mostrarán como destinos de implementación para la depuración de Android Studio.

Debemos especificar una clave del Registro específica para indicar al Emulador de Visual Studio para Android dónde se encuentra Android SDK.  Para establecer la clave del Registro necesaria, ejecute el siguiente script.  En el script de PowerShell siguiente se supone la ubicación de instalación predeterminada para Android SDK.  Si ha instalado Android SDK en otra ubicación, modifique el valor de `$androidSdkPath` antes de ejecutar el script.

```powershell
$androidSdkPath = Resolve-Path $(Join-Path "$($env:APPDATA)" "../Local/Android/Sdk")

$registryKeyPath = "HKLM:Software\WOW6432NODE\Android Sdk Tools"
New-Item -Path $registryKeyPath
New-ItemProperty -Path $registryKeyPath -Name Path -PropertyType String -Value $androidSdkPath
```

> [!IMPORTANT]
> Reinicie el Emulador de Visual Studio para Android y Android Studio para usar la nueva configuración.

Inicie la versión que necesita en el Emulador de Visual Studio.  Se mostrará como un destino de implementación para la aplicación de Android en Android Studio.  La versión mínima del proyecto Android Studio debe ser compatible con la versión que se ejecuta en el Emulador de Visual Studio para Android.  Ya está listo para crear y depurar proyectos con Android Studio y el Emulador de Visual Studio para Android.  Si tiene algún problema, consulte la solución de problemas del emulador de Android.

## <a name="cost"></a>Coste

Si desea calcular el costo estimado de este laboratorio, puede usar el ejemplo siguiente.
Para una clase de 25 alumnos con 20 horas de clase programadas y 10 horas de cuota para deberes o tareas, el precio del laboratorio sería:  

25 alumnos \* (20 horas programadas + 10 horas de cuota) x 55 unidades de laboratorio x 0,01 USD por hora = 412,5 USD

Para más detalles sobre los precios, consulte [Precios de Azure Lab Services](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="next-steps"></a>Pasos siguientes

Los pasos siguientes son comunes a la configuración de cualquier laboratorio.

- [Creación y administración de plantillas](how-to-create-manage-template.md)
- [Incorporación de usuarios](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Establecimiento de la cuota](how-to-configure-student-usage.md#set-quotas-for-users)
- [Establecimiento de la programación](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Vínculos de registro por correo electrónico para los alumnos](how-to-configure-student-usage.md#send-invitations-to-users)
