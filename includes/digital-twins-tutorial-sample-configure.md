---
author: baanders
description: Archivo de inclusión para los tutoriales de Azure Digital Twins (configuración del proyecto de ejemplo)
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 3a7c9f61af0124c656f98b64ba9295bc659c572a
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85262364"
---
## <a name="configure-the-sample-project"></a>Configuración del proyecto de ejemplo

A continuación, configure una aplicación cliente de ejemplo que interactuará con su instancia de Azure Digital Twins. Si aún no ha descargado el proyecto de ejemplo, puede obtenerlo ahora en la página de aterrizaje de [Ejemplos de Azure Digital Twins](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples); para ello, seleccione el botón *Descargar ZIP* debajo del título.

Vaya al archivo que ha descargado en la máquina y descomprímalo.

Una vez que esté dentro de la carpeta descomprimida, vaya a _AdtSampleApp/_ . Abra _**AdtE2ESample.sln**_ en Visual Studio 2019. 

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

