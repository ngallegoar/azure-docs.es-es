---
title: Actualización de Snapshot Debugger de Azure Application Insights
description: Cómo actualizar Snapshot Debugger para aplicaciones .NET a la versión más reciente en Azure App Services o mediante paquetes de Nuget
ms.topic: conceptual
author: pharring
ms.author: pharring
ms.date: 03/28/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6991db6055ca8556adff67f194bac0f4aa04cb06
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77671399"
---
# <a name="upgrading-the-snapshot-debugger"></a>Actualización de Snapshot Debugger

Para proporcionar la máxima seguridad posible para los datos, Microsoft está dejando de utilizar TLS 1.0 y TLS 1.1, que han demostrado ser vulnerables ante determinados atacantes. Si utiliza una versión anterior de la extensión de sitio, deberá actualizarla para que siga funcionando. En este documento se describen los pasos necesarios para actualizar Snapshot Debugger a la versión más reciente. Hay dos rutas de actualización principales en función de si ha habilitado Snapshot Debugger con una extensión de sitio o si usa un SDK/Nuget que se ha agregado a la aplicación. Ambas rutas de actualización se describen a continuación. 

## <a name="upgrading-the-site-extension"></a>Actualización de la extensión de sitio

> [!IMPORTANT]
> Las versiones anteriores de Application Insights usaban una extensión de sitio privada, la _extensión de Application Insights para Azure App Service_. La experiencia actual de Application Insights se habilita estableciendo la configuración de la aplicación para que ponga en marcha una extensión de sitio preinstalada.
> Para evitar conflictos que podrían provocar que el sitio dejase de funcionar, es importante eliminar primero la extensión de sitio privada. Consulte el paso 4 a continuación.

Si ha habilitado Snapshot Debugger mediante la extensión de sitio, puede realizar fácilmente la actualización mediante el siguiente procedimiento:

1. Inicie sesión en Azure Portal.
2. Vaya al recurso que tiene habilitado Snapshot Debugger y Application Insights. Por ejemplo, para una aplicación web, navegue hasta el recurso de App Service:

   ![Captura de pantalla de recurso individual de App Service denominado DiagService01](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Una vez que haya navegado hasta el recurso, haga clic en la hoja Extensiones y espere a que se rellene la lista de extensiones:

   ![Captura de pantalla de las extensiones de App Service que muestra la extensión de Application Insights para Azure App Service instalada](./media/snapshot-debugger-upgrade/application-insights-site-extension-to-be-deleted.png)

4. Si está instalada alguna versión de la _extensión de Application Insights para Azure App Service_, selecciónela y haga clic en Eliminar. Confirme con **Sí** para eliminar la extensión y espere a que se complete la eliminación antes de pasar al paso siguiente.

   ![Captura de pantalla de las extensiones de App Service que muestra la extensión de Application Insights para Azure App Service con el botón Eliminar resaltado](./media/snapshot-debugger-upgrade/application-insights-site-extension-delete.png)

5. Vaya a la hoja Información general del recurso y haga clic en Application Insights:

   ![Captura de pantalla de tres botones. Se selecciona el botón central con el nombre Application Insights](./media/snapshot-debugger-upgrade/application-insights-button.png)

6. Si esta es la primera vez que ve la hoja Application Insights de esta instancia de App Service, se le pedirá que active Application Insights. Seleccione **Activar Application Insights**.
 
   ![Captura de pantalla de la experiencia de la primera vez de la hoja Application Insights, con el botón Activar Application Insights resaltado](./media/snapshot-debugger-upgrade/turn-on-application-insights.png)

7. Se muestra la configuración de Application Insights actual. A menos que desee aprovechar la oportunidad para cambiar la configuración, puede dejarla tal cual. El botón **Aplicar** de la parte inferior de la hoja no está habilitado de forma predeterminada; tendrá activar o desactivar uno de los valores para habilitarlo. En realidad no es necesario cambiar la configuración, basta con cambiar el parámetro y, a continuación, revertir el cambio de inmediato. Recomendamos alternar el parámetro de Profiler y luego seleccionar **Aplicar**.

   ![Captura de pantalla de la página de configuración de App Service de Application Insights con el botón Aplicar resaltado en rojo](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

8. Al hacer clic en **Aplicar**, se le pedirá que confirme los cambios.

    > [!NOTE]
    > El sitio se reiniciará como parte del proceso de actualización.

   ![Captura de pantalla de la solicitud de aplicar supervisión de App Service El cuadro de texto muestra el mensaje: "Ahora vamos a aplicar cambios a la configuración de la aplicación e instalaremos nuestras herramientas para vincular el recurso de Application Insights a la aplicación web. Al hacerlo, se reiniciará el sitio. ¿Quiere continuar?".](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

9. Haga clic en **Sí** para aplicar los cambios y espere a que se complete el proceso.

El sitio ya se ha actualizado y está listo para usarse.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>Actualización de Snapshot Debugger mediante SDK/Nuget

Si la aplicación está usando una versión de `Microsoft.ApplicationInsights.SnapshotCollector` por debajo de la versión 1.3.1, se deberá actualizar a una [versión más reciente](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) para seguir funcionando.
