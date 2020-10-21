---
title: Complemento Angular para el SDK de JavaScript de Application Insights
description: Instalación y uso del complemento Angular para el SDK de JavaScript de Application Insights.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: lagayhar
ms.openlocfilehash: 152ba4b1c8a4e09db0bce759f5b67f577ec5d584
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843672"
---
# <a name="angular-plugin-for-application-insights-javascript-sdk"></a>Complemento Angular para el SDK de JavaScript de Application Insights

El complemento Angular para el SDK de JavaScript de Application Insights habilita lo siguiente:

- Seguimiento de cambios de enrutador
- Estadísticas de uso de componentes de Angular

> [!WARNING]
> El complemento Angular NO es compatible con ECMAScript 3 (ES3).

## <a name="getting-started"></a>Introducción

Instalación del paquete de npm:

```bash
npm install @microsoft/applicationinsights-angularplugin-js
```

## <a name="basic-usage"></a>Uso básico

Configure una instancia de Application Insights en el componente de entrada de la aplicación:

```js
import { Component, OnInit } from '@angular/core';
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { AngularPlugin, AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
    private appInsights;
    constructor(
        private router: Router,
        private angularPluginService: AngularPluginService 
    ){
        var angularPlugin = new AngularPlugin();
        this.angularPluginService.init(angularPlugin, this.router);
        this.appInsights = new ApplicationInsights({ config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [angularPlugin],
        extensionConfig: {
            [angularPlugin.identifier]: { router: this.router }
        }
        } });
    }

    ngOnInit() {
        this.appInsights.loadAppInsights();
    }
}

```

Para usar el método `trackMetric` para realizar el seguimiento del uso de componentes de Angular, agregue `AngularPluginService` como proveedor en la lista de proveedores en el archivo `app.module.ts`.

```js
import { AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';

@NgModule({
    ...
  providers: [ AngularPluginService ],
})
export class AppModule { }
```

Para realizar el seguimiento de la duración de un componente, llame a `trackMetric` en el método `ngOnDestroy` de ese componente. Cuando el componente se destruye, se desencadena un evento `trackMetric` que envía el tiempo que el usuario permaneció en la página y el nombre del componente.

```js
import { Component, OnDestroy, HostListener } from '@angular/core';
import { AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent implements OnDestroy {

  constructor(private angularPluginService: AngularPluginService) {}

  @HostListener('window:beforeunload')
  ngOnDestroy() {
    this.angularPluginService.trackMetric();
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre el SDK de JavaScript, consulte la [documentación del SDK de JavaScript de Application Insights](javascript.md).
- [Complemento Angular en GitHub](https://github.com/microsoft/ApplicationInsights-JS/tree/master/extensions/applicationinsights-angularplugin-js)