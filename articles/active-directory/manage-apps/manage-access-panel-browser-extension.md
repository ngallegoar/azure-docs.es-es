---
title: Solución de problemas de la extensión del Panel de acceso de Azure para Internet Explorer | Microsoft Docs
description: Cómo usar la directiva de grupo para implementar el complemento de Internet Explorer para el portal de Mis aplicaciones.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0269c87572e2a9242a54491103ae0fcc3637518
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67723905"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>Solución de problemas de la extensión del Panel de acceso para Internet Explorer

Este artículo le ayudará a solucionar los siguientes problemas:

* No puede tener acceso a sus aplicaciones a través del portal de Mis aplicaciones con Internet Explorer.
* Verá el mensaje "Instalar software" aunque ya haya instalado el software.

Si es administrador, consulte [Implementación de la extensión del Panel de acceso para Internet Explorer mediante la directiva de grupo](deploy-access-panel-browser-extension.md).

## <a name="run-the-diagnostic-tool"></a>Ejecutar la herramienta de diagnóstico

Puede diagnosticar problemas de instalación con la extensión del Panel de acceso mediante la descarga y la ejecución de la herramienta de diagnóstico del Panel de acceso. 

Para descargar e instalar la herramienta de diagnóstico:

1. [Seleccione este vínculo para descargar la herramienta de diagnóstico](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip).
1. Abra el archivo y extraiga el contenido en el equipo.
1. Para ejecutar la herramienta, haga clic con el botón derecho en el archivo denominado *AccessPanelExtensionDiagnosticTool.js* y, después, seleccione **Abrir con** > **Host de script basado en Microsoft Windows**.

    ![Abrir con > Host de script basado en Microsoft Windows](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

1. Revise los resultados de diagnóstico que aparecen y seleccione **Sí** para corregir los problemas. Aparece el cuadro de diálogo **Comprobar resultados** con información sobre qué hacer si la extensión no funciona.  
1. Lea el mensaje y seleccione **Aceptar**.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Comprobar que la extensión del Panel de acceso está activada

Para comprobar que ha habilitado la extensión del Panel de acceso en Internet Explorer:

1. En Internet Explorer, seleccione el **icono de engranaje** en la esquina superior derecha de la ventana y seleccione **Opciones de Internet**.
1. Vaya a la pestaña **Programas** y seleccione **Administrar complementos**.
1. Seleccione **Access Panel Extension** (Extensión del Panel de acceso) en la sección **Microsoft Corporation** y seleccione **Habilitar**.
1. Para guardar los cambios, cierre todas las ventanas del explorador de Internet Explorer que ha abierto. El cambio surte efecto la próxima vez que abra Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Habilitar extensiones para la exploración de InPrivate

Para habilitar extensiones para la exploración de InPrivate:

1. En Internet Explorer, seleccione el **icono de engranaje** en la esquina superior derecha de la ventana y seleccione **Opciones de Internet**.
1. Vaya a la pestaña **Privacidad** y compruebe si se ha desactivado la casilla **Deshabilitar barras de herramientas y extensiones cuando se inicie la exploración de InPrivate**.
1. Para guardar los cambios, cierre todas las ventanas del explorador de Internet Explorer que ha abierto. El cambio surte efecto la próxima vez que abra Internet Explorer.

## <a name="uninstall-the-access-panel-extension"></a>Desinstalar la extensión del Panel de acceso

Para desinstalar la extensión del Panel de acceso desde el equipo:

1. En el Panel de control, busque *Desinstalar*.
1. En los resultados de búsqueda, seleccione **Desinstalar un programa**.

    ![Selección de la opción Desinstalar un programa del panel de control](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

1. En la lista, seleccione **Access Panel Extension** (Extensión del Panel de acceso) y, después, haga clic en el botón **Desinstalar**.

    ![Desinstalar la extensión del Panel de acceso](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)

1. Después, puede intentar instalar la extensión de nuevo para ver si se ha resuelto el problema.

Si se producen problemas al desinstalar la extensión, también puede quitarla con la herramienta [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) (ahora conocida como Microsoft Easy Fix).

## <a name="related-articles"></a>Artículos relacionados

* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](what-is-single-sign-on.md)
* [Implementación de la extensión del Panel de acceso para Internet Explorer mediante la directiva de grupo](deploy-access-panel-browser-extension.md)
