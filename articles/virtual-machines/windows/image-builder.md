---
title: Creación de una máquina virtual Windows con Azure Image Builder (versión preliminar)
description: Cree una máquina virtual Windows con Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 07/31/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: e82d82dac833f7455e3d83d7e11c0c57c4eea816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238800"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Vista previa: Creación de una máquina virtual Windows con Azure Image Builder

En este artículo se muestra cómo puede crear una imagen personalizada de Windows mediante el generador de imágenes de máquina virtual de Azure. En el ejemplo de este artículo se usan [personalizadores](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) para personalizar la imagen:
- PowerShell (ScriptUri): para descargar y ejecutar un [script de PowerShell](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1).
- Reinicio de Windows: reinicia la máquina virtual.
- PowerShell (insertado) ejecuta un comando específico. En este ejemplo, se crea un directorio en la máquina virtual mediante `mkdir c:\\buildActions`.
- Archivo: para copiar un archivo de GitHub en la máquina virtual. En este ejemplo se copia [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) en `c:\buildArtifacts\index.html` en la máquina virtual.

También puede especificar un valor de `buildTimeoutInMinutes`. El valor predeterminado es de 240 minutos, y puede aumentar el tiempo de compilación para permitir compilaciones de larga duración.

Se usará una plantilla .json de ejemplo para configurar la imagen. El archivo .json que se usa aquí es: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


> [!IMPORTANT]
> Actualmente, el generador de imágenes de Azure se encuentra en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="register-the-features"></a>Registro de las características

Para usar el generador de imágenes de Azure durante la versión preliminar, tendrá que registrar la nueva característica.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Compruebe el estado del registro de la característica.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Compruebe el registro.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState

az provider show -n Microsoft.Storage | grep registrationState
```

Si no se muestran como registradas, ejecute lo siguiente:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="set-variables"></a>Configuración de variables

Usaremos algunos datos de forma repetida, por lo que crearemos diversas variables para almacenar esa información.


```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

Cree una variable para el id. de suscripción. Puede obtenerlo mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```
## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Este grupo de recursos se usa para almacenar el artefacto de la plantilla de configuración de la imagen y la imagen misma.


```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>Establecer permisos en el grupo de recursos

Otorgue el permiso de "colaborador" de Image Builder para crear la imagen en el grupo de recursos. Sin esto, no se compilará la imagen. 

El valor `--assignee` es el identificador de registro de aplicación para el servicio del generador de imágenes. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-image-configuration-template-example"></a>Descargar el ejemplo de plantilla de configuración de imagen

Se ha creado una plantilla de configuración de imagen con parámetros para que pueda probarla. Descargue el archivo .json de ejemplo y configúrelo con las variables que estableció anteriormente.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

Puede modificar este ejemplo en el terminal con un editor de texto, como `vi`.

```azurecli-interactive
vi helloImageTemplateLinux.json
```

> [!NOTE]
> Para la imagen de origen, siempre debe [especificar una versión](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure); no puede usar `latest`.
> Si agrega o cambia el grupo de recursos adonde se distribuye la imagen, tiene que asegurarse de que los [permisos estén establecidos](#set-permissions-on-the-resource-group) en el grupo de recursos.
 
## <a name="create-the-image"></a>Crear la imagen

Envío de la configuración de la imagen al servicio de generador de imágenes de máquina virtual

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Cuando haya finalizado, devolverá un mensaje de confirmación a la consola y creará `Image Builder Configuration Template` en `$imageResourceGroup`. Puede ver este recurso en el grupo de recursos en Azure Portal si habilita "Mostrar tipos ocultos".

Asimismo, Image Builder creará en segundo plano un grupo de recursos de almacenamiento provisional en la suscripción. Este grupo de recursos se usa para la compilación de la imagen. Tendrá este formato: `IT_<DestinationResourceGroup>_<TemplateName>`

> [!Note]
> No debe eliminar el grupo de recursos de almacenamiento provisional directamente. Primero elimine el artefacto de la plantilla de imagen, esto hará que se elimine el grupo de recursos de almacenamiento provisional.

Si el servicio informa de un error durante el envío de la plantilla de configuración de la imagen:
-  Revise los pasos para [solucionar problemas](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting). 
- Tendrá que eliminar la plantilla con el siguiente fragmento de código antes de reintentar el envío.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>Iniciar la generación de imágenes
Inicie el proceso de compilación de la imagen con [az resource invoke-action](/cli/azure/resource#az-resource-invoke-action).

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Espere hasta que se complete la compilación. Puede tardar unos 15 minutos.

Si encuentra algún error, revise estos pasos para la [solución de problemas](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting).


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree la máquina virtual con la imagen que ha compilado. Reemplace *\<password>* por su propia contraseña para `aibuser` en la máquina virtual.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Comprobación de la personalización

Cree una conexión de Escritorio remoto a la máquina virtual con el nombre de usuario y la contraseña que ha establecido al crear la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba lo siguiente:

```console
dir c:\
```

Debería ver estos dos directorios creados durante la personalización de la imagen:
- buildActions
- buildArtifacts

## <a name="clean-up"></a>Limpieza

Cuando haya terminado, elimine los recursos.

### <a name="delete-the-image-builder-template"></a>Eliminar la plantilla de Image Builder

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

### <a name="delete-the-image-resource-group"></a>Eliminar el grupo de recursos de imagen

```azurecli-interactive
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los componentes del archivo .json que se usan en este artículo, vea [Referencia de la plantilla de generador de imágenes](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
