---
title: Habilitar el lanzamiento preconfigurado de características para audiencias de destino
titleSuffix: Azure App Configuration
description: Obtenga información sobre cómo habilitar el lanzamiento preconfigurado de características para audiencias de destino.
ms.service: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.topic: conceptual
ms.date: 8/7/2020
ms.openlocfilehash: d1574b8a3f8cda3341c0aaf355911e2e93a7bcab
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94557474"
---
# <a name="enable-staged-rollout-of-features-for-targeted-audiences"></a>Habilitar el lanzamiento preconfigurado de características para audiencias de destino

Las marcas de característica permiten activar o desactivar dinámicamente la funcionalidad en la aplicación. Los filtros de características determinan el estado de una marca de característica cada vez que se evalúa. La biblioteca `Microsoft.FeatureManagement` incluye `TargetingFilter`, que habilita una marca de característica para una lista especificada de usuarios y grupos, o para un porcentaje especificado de usuarios. `TargetingFilter` es persistente. Esto significa que, una vez que un usuario individual recibe una característica, seguirá viendo esa característica en todas las solicitudes futuras. Puede usar `TargetingFilter` para habilitar una característica para una cuenta específica durante una demostración, para implementar progresivamente nuevas características para los usuarios en grupos o "anillos" y mucho más.

En este artículo, aprenderá a implementar una nueva característica en una aplicación web ASP.NET Core para usuarios y grupos especificados, mediante `TargetingFilter` con Azure App Configuration.

## <a name="create-a-web-application-with-feature-flags-and-authentication"></a>Creación de una aplicación web con marcas de característica y autenticación

Para implementar características basadas en usuarios y grupos, necesitará una aplicación web que permita a los usuarios iniciar sesión.

1. Cree una aplicación web que se autentique en una base de datos local con el siguiente comando:

   ```dotnetcli
   dotnet new mvc --auth Individual -o TestFeatureFlags
   ```

1. Compile, ejecute y, a continuación, seleccione el vínculo **Register** (Registrar) en la esquina superior derecha para crear una nueva cuenta de usuario. Use una dirección de correo electrónico de `test@contoso.com`. En la pantalla **Register Confirmation** (Confirmación del registro), seleccione **Click here to confirm your account** (Haga clic aquí para confirmar su cuenta).

1. Siga las instrucciones del artículo [Guía de inicio rápido: Agregue marcas de característica a una aplicación ASP.NET Core](./quickstart-feature-flag-aspnet-core.md) para agregar una marca de característica a su nueva aplicación web.

1. Habilite o deshabilite la marca de característica en App Configuration. Compruebe que esta acción controla la visibilidad del elemento **Beta** en la barra de navegación.

## <a name="update-the-web-application-code-to-use-targetingfilter"></a>Actualización del código de la aplicación web para usar TargetingFilter

En este momento, puede usar la marca de característica para habilitar o deshabilitar la característica `Beta` para todos los usuarios. Para habilitar la marca de característica para algunos usuarios mientras la deshabilita para otros, actualice el código para usar `TargetingFilter`. En este ejemplo, usará la dirección de correo electrónico del usuario con la sesión iniciada como identificador de usuario y la parte del nombre de dominio de la dirección de correo electrónico como grupo. Agregará el usuario y el grupo a `TargetingContext`. `TargetingFilter` utiliza este contexto para determinar el estado de la marca de característica para cada solicitud.

1. Actualice el paquete `Microsoft.FeatureManagement.AspNetCore` a la versión más reciente.

   ```dotnetcli
   dotnet add package Microsoft.FeatureManagement.AspNetCore
   ```

1. Agregue un archivo *TestTargetingContextAccessor.cs*:

    ```csharp
    using Microsoft.AspNetCore.Http;
    using Microsoft.FeatureManagement.FeatureFilters;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;

    namespace TestFeatureFlags
    {
        public class TestTargetingContextAccessor : ITargetingContextAccessor
        {
            private const string TargetingContextLookup = "TestTargetingContextAccessor.TargetingContext";
            private readonly IHttpContextAccessor _httpContextAccessor;

            public TestTargetingContextAccessor(IHttpContextAccessor httpContextAccessor)
            {
                _httpContextAccessor = httpContextAccessor ?? throw new ArgumentNullException(nameof(httpContextAccessor));
            }

            public ValueTask<TargetingContext> GetContextAsync()
            {
                HttpContext httpContext = _httpContextAccessor.HttpContext;
                if (httpContext.Items.TryGetValue(TargetingContextLookup, out object value))
                {
                    return new ValueTask<TargetingContext>((TargetingContext)value);
                }
                List<string> groups = new List<string>();
                if (httpContext.User.Identity.Name != null)
                {
                    groups.Add(httpContext.User.Identity.Name.Split("@", StringSplitOptions.None)[1]);
                }
                TargetingContext targetingContext = new TargetingContext
                {
                    UserId = httpContext.User.Identity.Name,
                    Groups = groups
                };
                httpContext.Items[TargetingContextLookup] = targetingContext;
                return new ValueTask<TargetingContext>(targetingContext);
            }
        }
    }
    ```

1. En *Startup.cs*, agregue una referencia al espacio de nombres *Microsoft.FeatureManagement.FeatureFilters*:

    ```csharp
    using Microsoft.FeatureManagement.FeatureFilters;
    ```

1. Actualice el método *ConfigureServices* para registrar `TargetingFilter`, después de la llamada a `AddFeatureManagement()`:

    ```csharp
    services.AddFeatureManagement()
            .AddFeatureFilter<TargetingFilter>();
    ```

1. Actualice el método *ConfigureServices* para agregar el elemento `TestTargetingContextAccessor` creado en el paso anterior a la colección de servicios. *TargetingFilter* lo utiliza para determinar el contexto de destino cada vez que se evalúa la marca de característica.

    ```csharp
      services.AddSingleton<ITargetingContextAccessor, TestTargetingContextAccessor>();
    ```

El método *ConfigureServices* completo tendrá el siguiente aspecto:

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
            .AddEntityFrameworkStores<ApplicationDbContext>();
    services.AddControllersWithViews();
    services.AddRazorPages();

    // Add feature management, targeting filter, and ITargetingContextAccessor to service collection
    services.AddFeatureManagement().AddFeatureFilter<TargetingFilter>();
    services.AddSingleton<ITargetingContextAccessor, TestTargetingContextAccessor>();
    }
```

## <a name="update-the-feature-flag-to-use-targetingfilter"></a>Actualización de la marca de característica para usar TargetingFilter

1. En Azure Portal, vaya al almacén de App Configuration y seleccione **Administrador de características**.

1. Seleccione el menú contextual de la marca de característica *Beta* que creó en el inicio rápido. Seleccione **Editar**.

    > [!div class="mx-imgBorder"]
    > ![Editar marca de característica Beta](./media/edit-beta-feature-flag.png)

1. En la pantalla **Editar**, active la casilla **Habilitar la marca de característica** si aún no está seleccionada. A continuación, active la casilla **Usar un filtro de características**.

1. Seleccione el botón de radio **Destino**.

1. Seleccione las opciones siguientes:

    - **Porcentaje predeterminado**: 0
    - **Grupos**: Escriba un **nombre** de _contoso.com_ y un **porcentaje** de _50_
    - **Usuarios**: `test@contoso.com`

    La pantalla de filtro de características tendrá el siguiente aspecto:

    > [!div class="mx-imgBorder"]
    > ![Marca de característica condicional](./media/feature-flag-filter-enabled.png)

    Esta configuración produce el comportamiento siguiente:

    - La marca de característica siempre está habilitada para el usuario `test@contoso.com`, porque `test@contoso.com` aparece en la sección _Usuarios_.
    - La marca de característica está habilitada para el 50 % de los otros usuarios del grupo _contoso.com_, porque _contoso.com_ figura en la sección _Grupos_ con un _porcentaje_ de _50_.
    - La característica está siempre deshabilitada para todos los demás usuarios, porque el _porcentaje predeterminado_ está establecido en _0_.

1. Seleccione **Aplicar** para guardar esta configuración y volver a la pantalla **Administrador de características**.

1. El **filtro de características** de la marca de característica ahora se muestra como *Destino*. Este estado indica que la marca de característica se habilitará o deshabilitará en función de cada solicitud, según los criterios que el filtro de características *Destino* aplique.

## <a name="targetingfilter-in-action"></a>TargetingFilter en acción

Para ver los efectos de esta marca de característica, compile y ejecute la aplicación. Inicialmente, el elemento *Beta* no aparece en la barra de herramientas, porque la opción _porcentaje predeterminado_ está establecida en 0.

Ahora, inicie sesión como `test@contoso.com` con la contraseña que estableció al registrarse. El elemento *Beta* ahora se muestra en la barra de herramientas, puesto que `test@contoso.com` está especificado como usuario de destino.

En el vídeo siguiente se muestra este comportamiento en acción.

> [!div class="mx-imgBorder"]
> ![TargetingFilter en acción](./media/feature-flags-targetingfilter.gif)

Puede crear usuarios adicionales con direcciones de correo electrónico de `@contoso.com` para ver el comportamiento de la configuración del grupo. El 50 % de estos usuarios verán el elemento de *Beta*. El 50 % restante no verá el elemento *Beta*.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a la administración de características](./concept-feature-management.md)