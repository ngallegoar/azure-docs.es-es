---
title: archivo de inclusión
description: archivo de inclusión
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 5ebbac39c8850737ea6f9ef333e45d305a520655
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "79461221"
---
## <a name="use-cli-shell"></a>Uso del shell de la CLI

Para ejecutar comandos de la CLI, se recomienda usar [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). **Cloud Shell** es un shell interactivo gratuito que se puede usar para ejecutar los pasos de este artículo. Cloud Shell incluye herramientas comunes de Azure preinstaladas y configuradas para que las use con su cuenta. Ofrece la flexibilidad de poder elegir la experiencia de shell que mejor se adapte a su forma de trabajar. Los usuarios de Linux pueden elegir una experiencia de Bash, mientras que los usuarios de Windows pueden optar por PowerShell.

La CLI también se puede instalar localmente. Consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) para ver las instrucciones específicas para su plataforma.

### <a name="sign-in"></a>Iniciar sesión

El uso de una instalación local de la CLI requiere iniciar sesión en Azure. Este paso no es necesario para Azure Cloud Shell. Inicie sesión con el comando `az login`.

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, deberá abrir una página del explorador y seguir las instrucciones de la línea de comandos para especificar un código de autorización después de ir a https://aka.ms/devicelogin en el explorador.

### <a name="specify-location-of-files"></a>Especificación de la ubicación de los archivos

Muchos de los comandos de la CLI de Media Services permiten usar un parámetro con un nombre de archivo. Si usa **Cloud Shell**, puede cargar el archivo para su unidad en la nube (con Bash o PowerShell). 

![Carga de archivos]

Si usa una CLI local o **Cloud Shell**, deberá especificar la ruta de acceso de archivo según el sistema operativo o Cloud Shell (Bash o PowerShell) que esté usando. A continuación se muestran algunos ejemplos:

Ruta de acceso relativa al archivo de metadatos (todos los sistemas operativos)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Ruta de acceso absoluta del archivo en el sistema operativo Windows y Linux o Mac

* `@ "/usr/home/mytestfile.json"`
*    `@"c:\tmp\user\mytestfile.json"`

Use `{file}` si el comando pide una ruta de acceso al archivo. Por ejemplo, `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Use `@{file}` si el comando va a cargar el archivo especificado. Por ejemplo, `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Carga de archivos]: ./media/media-services-cli/upload-download-files.png
