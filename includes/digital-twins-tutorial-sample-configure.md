---
author: baanders
description: Archivo de inclusión para los tutoriales de Azure Digital Twins (configuración del proyecto de ejemplo)
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: f5b332415f1abf7d4c1002bdd4f3bfcef12f1267
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93136324"
---
## <a name="configure-the-sample-project"></a>Configuración del proyecto de ejemplo

A continuación, configure una aplicación cliente de ejemplo que interactuará con su instancia de Azure Digital Twins.

Vaya en la máquina hasta el archivo que descargó anteriormente desde [*Ejemplos de un extremo a otro de Azure Digital Twins*](/samples/azure-samples/digital-twins-samples/digital-twins-samples) (y descomprímalo si aún no lo ha hecho).

Una vez que esté dentro de la carpeta, vaya a _AdtSampleApp_. Abra _**AdtE2ESample.sln**_ en Visual Studio 2019. 

En Visual Studio, seleccione el archivo _SampleClientApp > **appsettings.json**_ para abrirlo en la ventana de edición. Este servirá como archivo JSON predefinido con las variables de configuración necesarias para ejecutar el proyecto.

En el cuerpo del archivo, cambie el valor de `instanceUrl` a la dirección URL del *nombre de host* de la instancia de Azure Digital Twins (con *https://* al principio, como se muestra a continuación).

```json
{
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```

Guarde y cierre el archivo. 

Después, configure el archivo *appsettings.json* para que se copie al directorio de salida cuando compile *SampleClientApp*. Para ello, seleccione el archivo *appsettings.json* con el botón derecho y elija *Propiedades.* . En el inspector *Propiedades* , cambie el valor de la propiedad *Copiar en el directorio de resultados* a *Copiar si es posterior*.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Extracto de la ventana de Visual Studio que muestra el panel Explorador de soluciones con appsettings.json resaltado y el panel Propiedades con la propiedad Copiar en el directorio de salida establecida en Copiar si es posterior" border="false" lightbox="../articles/digital-twins/media/includes/copy-config.png":::

Mantenga el _**AdtE2ESample**_ proyecto abierto en Visual Studio para seguir utilizándolo en el tutorial.

