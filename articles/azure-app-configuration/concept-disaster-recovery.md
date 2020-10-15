---
title: Resistencia y recuperación ante desastres de Azure App Configuration
description: Aprenda a implementar resistencia y recuperación ante desastres con Azure App Configuration.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 5c62f10d67345d68cde27af7d0a7663b22d978a0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86207189"
---
# <a name="resiliency-and-disaster-recovery"></a>Resistencia y recuperación ante desastres

Actualmente, Azure App Configuration es un servicio regional. Cada almacén de configuración se crea en una región de Azure determinada. Una interrupción en toda la región afecta a todos los almacenes de esa región. App Configuration no ofrece conmutación por error automática en otra región. En este artículo se proporcionan instrucciones generales sobre cómo puede usar varios almacenes de configuración entre regiones de Azure para aumentar la resistencia geográfica de la aplicación.

## <a name="high-availability-architecture"></a>Arquitectura de alta disponibilidad

Para obtener redundancia entre regiones, es preciso crear varios almacenes de App Configuration en diferentes regiones. Con esta configuración, la aplicación tiene al menos un almacén de configuración adicional al que recurrir si el almacén principal deja de estar accesible. El siguiente diagrama ilustra la topología entre la aplicación y sus almacenes de configuración principal y secundario:

![Almacenes con redundancia geográfica.](./media/geo-redundant-app-configuration-stores.png)

La aplicación carga su configuración desde el almacén principal y el secundario en paralelo. Con ello, se aumenta la posibilidad de obtener correctamente los datos de configuración. Usted es responsable de mantener sincronizados los datos de ambos. En las siguientes secciones se explica cómo puede crear resistencia geográfica en la aplicación.

## <a name="failover-between-configuration-stores"></a>Conmutación por error entre los almacenes de configuración

Técnicamente, la aplicación no va a ejecutar una conmutación por error. Va a intentar recuperar el mismo conjunto de datos de configuración de dos almacenes de App Configuration al mismo tiempo. Organice el código de modo que se cargue primero desde el almacén secundario y, luego, desde el almacén principal. Este enfoque garantiza que los datos de configuración del almacén principal tengan prioridad siempre que estén disponibles. El fragmento de código siguiente le muestra cómo puede implementar este tipo de organización en .NET Core:

#### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    
```

#### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                    .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
            })
            .UseStartup<Startup>());
```
---

Observe el parámetro `optional` pasado a la función `AddAzureAppConfiguration`. Cuando se establece en `true`, este parámetro evita que la aplicación se detenga si la función no puede cargar los datos de configuración.

## <a name="synchronization-between-configuration-stores"></a>Sincronización entre los almacenes de configuración

Es importante que los almacenes de configuración con redundancia geográfica tengan todos el mismo conjunto de datos. Hay dos formas de lograrlo:

### <a name="backup-manually-using-the-export-function"></a>Copia de seguridad manual mediante la función Exportar

Puede usar la función **Exportar** de App Configuration para copiar datos del almacén principal al secundario a petición. Esta función está disponible mediante Azure Portal y la CLI.

En Azure Portal, puede insertar un cambio en otro almacén de configuración mediante estos pasos.

1. Vaya a la pestaña **Import/Export** y seleccione **Exportar** > **App Configuration** > **Destino** > **Seleccionar un recurso**.

1. En la nueva hoja que se abre, especifique la suscripción, el grupo de recursos y el nombre del recurso del almacén secundario y, luego, seleccione **Aplicar**.

1. La interfaz de usuario se actualiza para que pueda elegir qué datos de configuración quiere exportar al almacén secundario. Puede dejar el valor de tiempo predeterminado tal cual y establecer **De la etiqueta** y **Etiquetar** en el mismo valor. Seleccione **Aplicar**. Repita este paso para todas las etiquetas del almacén principal.

1. Repita los pasos anteriores cada vez que cambie la configuración.

También puede realizar el proceso de exportación mediante la CLI de Azure. El comando siguiente muestra cómo exportar todas las configuraciones del almacén principal al secundario:

```azurecli
    az appconfig kv export --destination appconfig --name {PrimaryStore} --dest-name {SecondaryStore} --label * --preserve-labels -y
```

### <a name="backup-automatically-using-azure-functions"></a>Copia de seguridad automática con Azure Functions

El proceso de copia de seguridad se puede automatizar mediante Azure Functions. Con ello se aprovecha la integración con Azure Event Grid en App Configuration. Una vez configurada, App Configuration publicará eventos en Event Grid para cualquier cambio realizado en los valores de clave de un almacén de configuración. De este modo, una aplicación de Azure Functions puede escuchar estos eventos y realizar una copia de seguridad de los datos en consecuencia con los mismos. Para más información, consulte el tutorial sobre [Procedimientos para la realización de copias de seguridad de los almacenes de App Configuration automáticamente](./howto-backup-config-store.md).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a mejorar la aplicación para lograr resistencia geográfica en tiempo de ejecución para App Configuration. También puede insertar datos de configuración de App Configuration en el momento de la compilación o la implementación. Para más información, consulte [Integración con una canalización de CI/CD](./integrate-ci-cd-pipeline.md).
