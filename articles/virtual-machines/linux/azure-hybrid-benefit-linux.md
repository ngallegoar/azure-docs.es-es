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
ms.author: mathapli
ms.openlocfilehash: 8437c83faf8dfcec0a21add2006b6cf627447dd1
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94516448"
---
# <a name="public-preview-azure-hybrid-benefit--how-it-applies-for-linux-virtual-machines"></a>Versión preliminar pública: Ventaja híbrida de Azure: cómo se aplica a las máquinas virtuales Linux

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

La Ventaja híbrida de Azure está actualmente en fase de versión preliminar para las VM Linux. Una vez que obtenga acceso a la versión preliminar, puede habilitar la ventaja mediante la CLI de Azure.

### <a name="public-preview"></a>Vista previa pública

Ventaja híbrida de Azure (para Linux) está actualmente en fase de versión preliminar pública. Puede usar los pasos siguientes para habilitar la ventaja de las distribuciones de Red Hat y SUSE. 

### <a name="red-hat-customers"></a>Para clientes de Red Hat

Ventaja híbrida de Azure para RHEL está disponible para los clientes que tienen suscripciones de RHEL activas o no utilizadas que son válidas para su uso en Azure y que han habilitado una o varias de esas suscripciones para su uso en Azure con el programa [Red Hat Cloud Access](https://www.redhat.com/en/technologies/cloud-computing/cloud-access). 

1.  Habilite una o varias de las suscripciones de RHEL válidas para su uso en Azure la [interfaz de cliente de Red Hat Cloud Access](https://access.redhat.com/management/cloud).
1.  A continuación, las suscripciones de Azure que ha proporcionado durante el proceso de habilitación de Red Hat Cloud Access podrán usar la característica Ventaja híbrida de Azure.
1.  Aplique la Ventaja híbrida de Azure a cualquiera de las máquinas virtuales RHEL de pago por uso existentes, así como a todas las nuevas máquinas virtuales RHEL que implemente desde imágenes de pago por uso de Azure Marketplace.
1.  Siga los [pasos siguientes](https://access.redhat.com/articles/5419341) recomendados para configurar los orígenes de actualización para las máquinas virtuales RHEL y para las directrices de cumplimiento de las suscripciones de RHEL.


### <a name="suse-customers"></a>Para clientes de SUSE

1.    Regístrese en el programa de nube pública de SUSE.
1.    Aplique la ventaja a las máquinas virtuales existentes a través de CLI de Azure.
1.    Registrar las VM que reciben la ventaja en un origen de actualizaciones independiente.


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
Puede ver el estado de la Ventaja híbrida de Azure (AHB) de una máquina virtual de dos maneras: mediante la CLI de Azure o mediante Instance Metadata Service de Azure (Azure IMDS).


### <a name="azure-cli"></a>CLI de Azure

Para este fin se puede usar el comando `az vm get-instance-view`. Busque un campo licenseType en la respuesta. Si el campo licenseType existe y el valor es "RHEL_BYOS"o "SLES_BYOS", la VM tiene habilitada la ventaja.

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm
```

### <a name="azure-instance-metadata-service"></a>Servicio de metadatos de instancia de Azure

Desde la propia VM, puede consultar los metadatos atestiguados de IMDS para determinar el valor licenseType de la VM. Un valor licenseType de "RHEL_BYOS" o "SLES_BYOS" indicará que la VM tiene habilitada la ventaja. Obtenga más información sobre los metadatos atestiguados [aquí](./instance-metadata-service.md#attested-data)

## <a name="compliance"></a>Cumplimiento

### <a name="red-hat"></a>Red Hat

Los clientes que usan Ventaja híbrida de Azure para RHEL acuerdan los [términos legales](http://www.redhat.com/licenses/cloud_CSSA/Red_Hat_Cloud_Software_Subscription_Agreement_for_Microsoft_Azure.pdf) estándar y la [declaración de privacidad](http://www.redhat.com/licenses/cloud_CSSA/Red_Hat_Privacy_Statement_for_Microsoft_Azure.pdf) asociados a las ofertas de RHEL de Azure Marketplace.

Los clientes que usan Ventaja híbrida de Azure para RHEL tienen tres opciones para proporcionar actualizaciones de software y revisiones a esas máquinas virtuales:

1.  [Red Hat Update Infrastructure (RHUI)](../workloads/redhat/redhat-rhui.md) (opción predeterminada)
1.  Red Hat Satellite Server
1.  Red Hat Subscription Manager

Los clientes que eligen la opción RHUI pueden seguir usándola como el origen de actualización principal para sus máquinas virtuales RHEL de AHB sin necesidad de adjuntar suscripciones de RHEL a esas máquinas virtuales.  Los clientes que elijan la opción RHUI son responsables de garantizar el cumplimiento de las suscripciones de RHEL.

Los clientes que elijan Red Hat Satellite Server o Red Hat Subscription Manager deben quitar la configuración de RHUI y, a continuación, adjuntar una suscripción de RHEL habilitada para el acceso a la nube a sus máquinas virtuales RHEL de AHB.  

Puede encontrar más información sobre el cumplimiento de las suscripciones de Red Hat, las actualizaciones de software y los orígenes de máquinas virtuales RHEL de AHB se pueden encontrar [aquí](https://access.redhat.com/articles/5419341).

### <a name="suse"></a>SUSE

Para usar la Ventaja híbrida de Azure para las VM de SLES, primero debe registrarse en el programa de nube pública de SUSE. Obtenga más información sobre el programa aquí. Una vez que haya adquirido las suscripciones de SUSE, debe registrar las VM en esas suscripciones en su propio origen de actualizaciones mediante el Centro de servicios al cliente de SUSE, Subscription Management Tool o SUSE Manager.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
*P: ¿Puedo usar un tipo de licencia "RHEL_BYOS" con una imagen de SLES o viceversa?*

A. Lamentablemente, no. Si intenta especificar un tipo de licencia que no coincida correctamente con la distribución que se ejecuta en la VM, no se actualizarán los metadatos de facturación. Sin embargo, si especifica por accidente el tipo de licencia incorrecto, pero actualiza la VM de nuevo al tipo de licencia correcto, podrá seguir habilitando la ventaja.

*P: Me he registrado en Red Hat Cloud Access, pero todavía no puedo habilitar la ventaja en mis VM de RHEL. ¿Qué puedo hacer?*

A. El registro de la suscripción a Red Hat Cloud Access puede tardar un tiempo en propagarse de Red Hat a Azure. Si sigue viendo el error después de un día laborable, póngase en contacto con Soporte técnico de Microsoft.

## <a name="common-issues"></a>Problemas comunes
En esta sección se incluye una lista de problemas comunes que se pueden encontrar y los pasos para su mitigación.

| Error | Mitigación |
| ----- | ---------- |
| "The action could not be completed because our records show that you have not successfully enabled Red Hat Cloud Access on your Azure subscription…" (No se pudo completar la acción porque nuestros registros muestran que no ha habilitado correctamente Red Hat Cloud Access en la suscripción a Azure…) | Para usar la ventaja con VM de RHEL, primero debe registrar las suscripciones de Azure en Red Hat Cloud Access. Visite este vínculo para obtener más información sobre cómo registrar las suscripciones de Azure en Red Hat Cloud Access.

## <a name="next-steps"></a>Pasos siguientes
* Aprenda a crear y actualizar máquinas virtuales y a agregar tipos de licencia (RHEL_BYOS, SLES_BYOS) para Ventaja híbrida de Azure con la [CLI de Azure aquí.](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest&preserve-view=true)
