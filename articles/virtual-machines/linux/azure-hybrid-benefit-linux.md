---
title: Ventaja híbrida de Azure y VM Linux
description: La Ventaja híbrida de Azure le permite ahorrar dinero en máquinas virtuales Linux que se ejecutan en Azure.
services: virtual-machines
documentationcenter: ''
author: mathapli
manager: westonh
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/22/2020
ms.author: alsin
ms.openlocfilehash: d62eaf96354627e0c1e4e0a31bb16fb3265f66ac
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91279780"
---
# <a name="preview-azure-hybrid-benefit--how-it-applies-for-linux-virtual-machines"></a>Vista previa: Ventaja híbrida de Azure: cómo se aplica a las máquinas virtuales Linux

## <a name="overview"></a>Información general

La Ventaja híbrida de Azure le permite migrar con mayor facilidad sus máquinas virtuales (VM) de Red Hat Enterprise Linux (RHEL) y SUSE Linux Enterprise Server (SLES) a Azure mediante su propia suscripción de software de Red Hat o SUSE ya existente. Con esta ventaja, solo paga por los costos de infraestructura de la VM, ya que el precio del software está incluido en la suscripción de RHEL o SLES. La ventaja se aplica a todas las imágenes de pago por uso (PAYG) de Marketplace de RHEL y SLES.

> [!IMPORTANT]
> La Ventaja híbrida de Azure para máquinas virtuales de Linux está actualmente en fase de versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="benefit-description"></a>Descripción de la ventaja

A través de la Ventaja híbrida de Azure, puede migrar más fácilmente los servidores locales RHEL y SLES a Azure mediante la conversión de las VM PAYG de RHEL y SLES existentes en Azure en facturación de traiga su propia suscripción (BYOS). Normalmente, las VM implementadas a partir de imágenes PAYG en Azure cobrarán tanto un precio de infraestructura como un precio de software. Con el Ventaja híbrida de Azure, las VM PAYG se pueden convertir en un modelo de facturación BYOS sin necesidad de una reimplementación, lo que evita todo riesgo de tiempo de inactividad.

:::image type="content" source="./media/ahb-linux/azure-hybrid-benefit-cost.png" alt-text="Visualización de los costos de la Ventaja híbrida de Azure en máquinas virtuales de Linux.":::

Al habilitar la ventaja en una VM de RHEL o SLES, ya no se le cobrará el precio de software adicional en el que incurre normalmente una VM PAYG. En su lugar, la VM empezará a emitir un cargo de BYOS, que incluye solo el precio de hardware de proceso y ningún precio de software.

Si lo desea, también puede convertir una VM que tenga habilitada la ventaja nuevamente en el modelo de facturación PAYG.

## <a name="scope-of-azure-hybrid-benefit-eligibility-for-linux-vms"></a>Ámbito de la idoneidad de Ventaja híbrida de Azure para VM Linux

La Ventaja híbrida de Azure está disponible para todas las imágenes PAYG de Marketplace de RHEL y SLES. La ventaja aún no está disponible para las imágenes BYOS de Marketplace de RHEL o SLES ni para imágenes personalizadas.

Las instancias reservadas, los hosts dedicados y las ventajas híbridas de SQL no son válidas para la Ventaja híbrida de Azure si ya está usando la ventaja con las VM Linux.

## <a name="how-to-get-started"></a>Primeros pasos

La Ventaja híbrida de Azure está actualmente en fase de versión preliminar para las VM Linux. Una vez que obtenga acceso a la versión preliminar, puede habilitar la ventaja mediante Azure Portal o la CLI de Azure.

### <a name="preview"></a>Vista previa

En esta fase, puede obtener acceso a la ventaja rellenando [este](https://aka.ms/ahb-linux-form) formulario. Una vez que rellene el formulario, sus suscripciones a Azure estarán habilitadas para la ventaja, y recibirá una confirmación de Microsoft en un plazo de tres días laborables.

### <a name="red-hat-customers"></a>Para clientes de Red Hat

1.    Rellene el formulario de solicitud de versión preliminar anterior.
1.    Regístrese en el [programa Red Hat Cloud Access](https://aka.ms/rhel-cloud-access).
1.    Habilite sus suscripciones a Azure para Cloud Access y habilite las suscripciones que contienen las VM con las que quiere usar la ventaja.
1.    Aplique la ventaja a las VM existentes mediante Azure Portal o la CLI de Azure.
1.    Registrar las VM que reciben la ventaja en un origen de actualizaciones independiente.

### <a name="suse-customers"></a>Para clientes de SUSE

1.    Rellene el formulario de solicitud de versión preliminar anterior.
1.    Regístrese en el programa de nube pública de SUSE.
1.    Aplique la ventaja a las VM existentes mediante Azure Portal o la CLI de Azure.
1.    Registrar las VM que reciben la ventaja en un origen de actualizaciones independiente.

### <a name="enable-and-disable-the-benefit-in-the-azure-portal"></a>Habilitación y deshabilitación de la ventaja en Azure Portal

Para habilitar la ventaja en las VM existentes, visite la hoja **Configuración** y siga los pasos que se indican. Puede habilitar la ventaja en las nuevas VM durante la experiencia de creación de la VM.

### <a name="enable-and-disable-the-benefit-in-the-azure-cli"></a>Habilitación y deshabilitación de la ventaja en la CLI de Azure

Puede usar el comando "az vm update" para actualizar las VM existentes. En el caso de las VM de RHEL, ejecute el comando con un parámetro --license-type de "RHEL_BYOS". En el caso de las VM de SLES, ejecute el comando con un parámetro --license-type de "SLES_BYOS".

#### <a name="cli-example-to-enable-the-benefit"></a>Ejemplo de la CLI para habilitar la ventaja:
```azurecli
# This will enable the benefit on a RHEL VM
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS

# This will enable the benefit on a SLES VM
az vm update -g myResourceGroup -n myVmName --license-type SLES_BYOS
```
#### <a name="cli-example-to-disable-the-benefit"></a>Ejemplo de la CLI para deshabilitar la ventaja:
Para deshabilitar la ventaja, use un valor de license-type de "None".
```azurecli
# This will disable the benefit on a VM
az vm update -g myResourceGroup -n myVmName --license-type None
```

#### <a name="cli-example-to-enable-the-benefit-on-a-large-number-of-vms"></a>Ejemplo de la CLI para habilitar la ventaja en un gran número de VM
Para habilitar la ventaja en un gran número de VM, puede usar el parámetro `--ids` en la CLI de Azure.

```azurecli
# This will enable the benefit on a RHEL VM. In this example, ids.txt is an
# existing text file containing a delimited list of resource IDs corresponding
# to the VMs using the benefit
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS --ids $(cat ids.txt)
```

En los siguientes ejemplos se muestran dos métodos para obtener una lista de identificadores de recursos: uno en el nivel de grupo de recursos, uno en el nivel de suscripción.
```azurecli
# To get a list of all the resource IDs in a resource group:
$(az vm list -g MyResourceGroup --query "[].id" -o tsv)

# To get a list of all the resource IDs of VMs in a subscription:
az vm list -o json | jq '.[] | {VMName: .name, ResourceID: .id}'
```

## <a name="check-ahb-status-of-a-vm"></a>Comprobación del estado de la AHB de una VM
Puede ver el estado de la Ventaja híbrida de Azure (AHB) de una VM de tres maneras: comprobando en el portal, usando la CLI de Azure o usando Instance Metadata Service de Azure (Azure IMDS).


### <a name="portal"></a>Portal

Vea la hoja Configuración y compruebe el estado de la licencia para ver si la AHB está habilitada para la VM.

### <a name="azure-cli"></a>CLI de Azure

Para este fin se puede usar el comando `az vm get-instance-view`. Busque un campo licenseType en la respuesta. Si el campo licenseType existe y el valor es "RHEL_BYOS"o "SLES_BYOS", la VM tiene habilitada la ventaja.

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm
```

### <a name="azure-instance-metadata-service"></a>Servicio de metadatos de instancia de Azure

Desde la propia VM, puede consultar los metadatos atestiguados de IMDS para determinar el valor licenseType de la VM. Un valor licenseType de "RHEL_BYOS" o "SLES_BYOS" indicará que la VM tiene habilitada la ventaja. Obtenga más información sobre los metadatos atestiguados [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/instance-metadata-service#attested-data)

## <a name="compliance"></a>Cumplimiento

### <a name="red-hat"></a>Red Hat

Para usar la Ventaja híbrida de Azure para las VM de RHEL, primero debe registrarse en el programa Red Hat Cloud Access. Puede hacerlo a través del sitio de Red Hat Cloud Access. Una vez que haya habilitado la ventaja en la VM, debe registrar la VM con su propio origen de actualizaciones, ya sea con el administrador de suscripciones de Red Hat o con Red Hat Satellite. Al registrarse para las actualizaciones, se asegurará de permanecer en un estado admitido.

### <a name="suse"></a>SUSE

Para usar la Ventaja híbrida de Azure para las VM de SLES, primero debe registrarse en el programa de nube pública de SUSE. Obtenga más información sobre el programa aquí. Una vez que haya adquirido las suscripciones de SUSE, debe registrar las VM en esas suscripciones en su propio origen de actualizaciones mediante el Centro de servicios al cliente de SUSE, Subscription Management Tool o SUSE Manager.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
*P: ¿Puedo usar un tipo de licencia "RHEL_BYOS" con una imagen de SLES o viceversa?*

A. Lamentablemente, no. Si intenta especificar un tipo de licencia que no coincida correctamente con la distribución que se ejecuta en la VM, no se actualizarán los metadatos de facturación. Sin embargo, si especifica por accidente el tipo de licencia incorrecto, pero actualiza la VM de nuevo al tipo de licencia correcto, podrá seguir habilitando la ventaja.

*P: Me he registrado en Red Hat Cloud Access, pero todavía no puedo habilitar la ventaja en mis VM de RHEL. ¿Qué puedo hacer?*

A. El registro de la suscripción a Red Hat Cloud Access puede tardar un tiempo en propagarse de Red Hat a Azure. Si sigue viendo el error después de un día laborable, póngase en contacto con Soporte técnico de Microsoft.

## <a name="common-errors"></a>Errores comunes
En esta sección se incluye una lista de errores comunes y pasos para su mitigación.

| Error | Mitigación |
| ----- | ---------- |
| "La suscripción no está registrada para la versión preliminar de Linux de Ventaja híbrida de Azure. Para obtener instrucciones detalladas, consulte https://aka.ms/ahb-linux" | Rellene el formulario en https://aka.ms/ahb-linux-form para registrarse en la versión preliminar de Linux de la Ventaja híbrida de Azure.
| "The action could not be completed because our records show that you have not successfully enabled Red Hat Cloud Access on your Azure subscription…" (No se pudo completar la acción porque nuestros registros muestran que no ha habilitado correctamente Red Hat Cloud Access en la suscripción a Azure…) | Para usar la ventaja con VM de RHEL, primero debe registrar las suscripciones de Azure en Red Hat Cloud Access. Visite este vínculo para obtener más información sobre cómo registrar las suscripciones de Azure en Red Hat Cloud Access.

## <a name="next-steps"></a>Pasos siguientes
* Para empezar a trabajar con la versión preliminar, rellene [este](https://aka.ms/ahb-linux-form) formulario.
