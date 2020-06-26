---
author: baanders
description: Archivo de inclusión para los tutoriales de Azure Digital Twins (configuración del proyecto de ejemplo)
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 463fb7860eb35a7ff45d3794d200c69685825827
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2020
ms.locfileid: "84737210"
---
## <a name="configure-the-sample-project"></a>Configuración del proyecto de ejemplo

A continuación, configure una aplicación cliente de ejemplo que interactuará con su instancia de Azure Digital Twins. Si aún no ha descargado el proyecto de ejemplo, obténgalo ahora [descargando el repositorio de ejemplos de Azure Digital Twins en forma de archivo ZIP](https://github.com/Azure-Samples/digital-twins-samples/archive/master.zip). 

Vaya al archivo que ha descargado en la máquina y descomprímalo.

Una vez que esté dentro de la carpeta descomprimida, vaya a_digital-twins-samples-master/AdtSampleApp/_ . Abra _**AdtE2ESample.sln**_ en Visual Studio 2019. 

En Visual Studio, use el panel *Explorador de soluciones* para crear una copia del archivo _SampleClientApp > **serviceConfig.json.TEMPLATE**_ (puede usar los menús contextuales para copiar y pegar). Cambie el nombre a la copia de *serviceConfig.json*. Este servirá como archivo JSON predefinido con las variables de configuración necesarias para ejecutar el proyecto.

Seleccione el archivo *serviceConfig.json* para abrirlo en la ventana de edición. Cambie `tenantId` al *identificador de directorio*, `clientId` al *identificador de aplicación* y `instanceUrl` a la dirección URL (con *https://* delante como se muestra a continuación) de *hostName* de su instancia de Azure Digital Twins.

```json
{
  "tenantId": "<your-directory-ID>",
  "clientId": "<your-application-ID>",
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```

Guarde y cierre el archivo. 

Después, configure el archivo *serviceConfig.json* para que se copie al directorio de salida cuando cree *SampleClientApp*. Para ello, seleccione el archivo *serviceConfig.json* con el botón derecho y elija *Propiedades.* En el inspector *Propiedades*, cambie el valor de la propiedad *Copiar en el directorio de resultados* a *Copiar si es posterior*.

:::image type="content" source="../articles/digital-twins/media/include-tutorial/copy-config.png" alt-text="Extracto de la ventana de Visual Studio que muestra el panel Explorador de soluciones con serviceConfig.JSON resaltado y el panel Propiedades con la propiedad Copiar en el directorio de resultados establecida en Copiar si es posterior" border="false":::

Mantenga el _**AdtE2ESample**_ proyecto abierto en Visual Studio para seguir utilizándolo en el tutorial.

