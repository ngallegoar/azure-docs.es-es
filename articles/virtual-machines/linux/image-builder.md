---
title: Crear una máquina virtual Linux con Azure Image Builder (versión preliminar)
description: Cree una máquina virtual Linux con Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: how-to
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.openlocfilehash: b50b2a9bfca99e1868c083759cce26bb176789f4
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792418"
---
# <a name="preview-create-a-linux-vm-with-azure-image-builder"></a>Vista previa: Crear una máquina virtual Linux con Azure Image Builder

En este artículo se muestra cómo puede crear una imagen personalizada de Linux mediante Azure Image Builder y la CLI de Azure. En el ejemplo de este artículo se usan tres [personalizadores](image-builder-json.md#properties-customize) distintos para personalizar la imagen:

- Shell (ScriptUri): descarga y ejecuta un [script de shell](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh).
- Shell (alineado): ejecuta comandos específicos. En este ejemplo, los comandos alineados incluyen la creación de un directorio y la actualización del sistema operativo.
- Archivo: copia un [archivo de GitHub](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) en un directorio de la máquina virtual.

También puede especificar un valor de `buildTimeoutInMinutes`. El valor predeterminado es de 240 minutos, y puede aumentar el tiempo de compilación para permitir compilaciones de larga duración.

Se usará una plantilla .json de ejemplo para configurar la imagen. El archivo .json que se usa aquí es: [helloImageTemplateLinux.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json). 

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

## <a name="setup-example-variables"></a>Ejemplo de variables de configuración

Usaremos algunos datos de forma repetida, por lo que crearemos diversas variables para almacenar esa información.


```console
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myImageBuilerRGLinux
# Datacenter location - we are using West US 2 in this example
location=WestUS2
# Name for the image - we are using myBuilderImage in this example
imageName=myBuilderImage
# Run output name
runOutputName=aibLinux
```

Cree una variable para el id. de suscripción. Puede obtenerlo mediante `az account show | grep id`.

```console
subscriptionID=<Your subscription ID>
```

## <a name="create-the-resource-group"></a>Cree el grupo de recursos.
Esto se usa para almacenar el artefacto de la plantilla de configuración de la imagen y la misma imagen.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>Establecer permisos en el grupo de recursos
Otorgue el permiso de "colaborador" de Image Builder para crear la imagen en el grupo de recursos. Sin los permisos adecuados, la compilación de la imagen producirá un error. 

El valor `--assignee` es el identificador de registro de aplicación para el servicio del generador de imágenes. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```

## <a name="download-the-template-example"></a>Descargar el ejemplo de plantilla

Se ha creado una plantilla de configuración de imagen de muestra con parámetros para que pueda usarla. Descargue el archivo .json de muestra y configúrelo con las variables que estableció anteriormente.

```bash
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json -o helloImageTemplateLinux.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateLinux.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateLinux.json
sed -i -e "s/<region>/$location/g" helloImageTemplateLinux.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateLinux.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateLinux.json
```

Puede modificar este archivo .json de ejemplo según sea necesario. Por ejemplo, puede aumentar el valor de `buildTimeoutInMinutes` para realizar compilaciones más largas. Puede editar el archivo en Cloud Shell mediante un editor de texto como `vi`.

```bash
vi helloImageTemplateLinux.json
```

> [!NOTE]
> Para la imagen de origen, siempre debe [especificar una versión](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure); no puede usar `latest`.
>
> Si agrega o cambia el grupo de recursos donde se distribuye la imagen, deberá asegurarse de que los [permisos se establecen para el grupo de recursos](#set-permissions-on-the-resource-group).


## <a name="submit-the-image-configuration"></a>Enviar la configuración de la imagen
Envío de la configuración de la imagen al servicio de generador de imágenes de máquina virtual

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateLinux.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Si se completa correctamente, devolverá un mensaje de confirmación y creará un artefacto de plantilla de configuración de Image Builder en $imageResourceGroup. Puede ver el grupo de recursos en el portal si habilita "Mostrar tipos ocultos".

Asimismo, Image Builder crea en segundo plano un grupo de recursos de prueba en la suscripción. Image Builder usa el grupo de recursos de almacenamiento provisional para la creación de la imagen. El nombre del grupo de recursos tendrá este formato: `IT_<DestinationResourceGroup>_<TemplateName>`.

> [!IMPORTANT]
> No elimine el grupo de recursos de almacenamiento provisional directamente. Si elimina el artefacto de la plantilla de imagen, eliminará automáticamente el grupo de recursos de almacenamiento provisional. Para obtener más información, consulte la sección [Limpieza](#clean-up) de este artículo.

Si el servicio informa de un error durante el envío de la plantilla de configuración de la imagen, consulte los pasos de [solución de problemas](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting). También deberá eliminar la plantilla antes de volver a intentar enviar la compilación. Para eliminar la plantilla:

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>Iniciar la generación de imágenes

Inicie la generación de imágenes.


```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateLinux01 \
     --action Run 
```

Espere hasta que la compilación se complete; en este ejemplo, puede tardar entre 10 y 15 minutos.

Si encuentra algún error, revise estos pasos para la [solución de problemas](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting).


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree la máquina virtual con la imagen que ha compilado.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name myVM \
  --admin-username azureuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

Obtenga la dirección IP de la salida de la creación de la máquina virtual y úsela para conectarse mediante SSH a la máquina virtual.

```bash
ssh azureuser@<pubIp>
```

Verá que la imagen se ha personalizado con un mensaje del día en cuanto se ha establecido la conexión SSH.

```output

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

Escriba `exit` cuando haya terminado para cerrar la conexión SSH.

## <a name="check-the-source"></a>Comprobación del origen

En la plantilla de generador de imágenes, en "Propiedades", verá la imagen de origen, el script de personalización que ejecuta y dónde se distribuye.

```bash
cat helloImageTemplateLinux.json
```

Para obtener más información sobre este archivo .json, consulte [Referencia de la plantilla de generador de imágenes](image-builder-json.md).

## <a name="clean-up"></a>Limpieza

Cuando haya terminado, puede eliminar los recursos.

Elimine la plantilla de Azure Image Builder.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Elimine el grupo de recursos de la imagen.

```azurecli
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los componentes del archivo .json que se usan en este artículo, consulte [Referencia de la plantilla de generador de imágenes](image-builder-json.md).
