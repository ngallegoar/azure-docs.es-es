---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/10/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 427117fe47294a1db1fa8d3fa1e46ee1efb91b4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128571"
---
## <a name="limitations"></a>Limitaciones

- Actualmente, los conjuntos de escalado de máquinas virtuales no se admiten en los hosts dedicados.
- Los tamaños y tipos de hardware disponibles para hosts dedicados varían según la región. Para más información, consulte la [página de precios](https://aka.ms/ADHPricing) de hosts.

## <a name="create-a-host-group"></a>Creación de un grupo host

Un **grupo host** es un nuevo recurso que representa una colección de hosts dedicados. Puede crear un grupo host en una región y una zona de disponibilidad, y agregarle hosts. Al planear la alta disponibilidad, hay otras opciones. Puede usar una o ambas de las dos opciones siguientes con los hosts dedicados: 
- Abarcar varias zonas de disponibilidad. En este caso, es necesario tener un grupo host en cada una de las zonas que quiera usar.
- Abarcar varios dominios de error que se asignan a bastidores físicos. 
 
En cualquier caso, es necesario proporcionar el número de dominios de error del grupo host. Si no quiere abarcar dominios de error en el grupo, use un número de dominios de error de 1. 

También puede usar zonas de disponibilidad y dominios de error a la vez. 

En este ejemplo, se creará un grupo host con una zona de disponibilidad y dos dominios de error. 


1. Abra el [portal](https://portal.azure.com).
1. Seleccione **Crear un recurso** en la esquina superior izquierda.
1. Busque el **grupo host** y, a continuación, seleccione los **grupos host** en los resultados.
1. En la página de los **grupos host**, seleccione **Crear**.
1. Seleccione la suscripción que quiere usar y, a continuación, seleccione **Crear nuevo** para crear un nuevo grupo de recursos.
1. Escriba *myDedicatedHostsRG* como **nombre** y seleccione **Aceptar**.
1. En el **nombre del grupo host**, escriba *myHostGroup*.
1. En **Ubicación**, seleccione **Este de EE.UU.**
1. Para la **zona de disponibilidad**, seleccione **1**.
1. En el **recuento de dominios de error**, seleccione **2**.
1. Seleccione **Revisar y crear** y, a continuación, espere a la confirmación.
1. Cuando reciba el mensaje de **Validación superada**, seleccione **Crear** para crear el grupo host.

Solo tardará unos minutos en crear el grupo host.

## <a name="create-a-dedicated-host"></a>Creación de un host dedicado

Ahora crearemos un host dedicado en el grupo host. Además de un nombre para el host, se le pedirá que proporcione el SKU del host. El SKU del host registra la serie de máquinas virtuales admitidas, así como la generación de hardware del host dedicado.

Para más información sobre los precios y los SKU de host, consulte [Precios de hosts dedicados de Azure](https://aka.ms/ADHPricing).

Si establece un número de dominios de error para el grupo host, se le pedirá que especifique el dominio de error para su host.  

1. Seleccione **Crear un recurso** en la esquina superior izquierda.
1. Busque un **host dedicado** y, a continuación, seleccione los **hosts dedicados** en los resultados.
1. En la página de **hosts dedicados**, seleccione **Crear**.
1. Seleccione la suscripción que quiere usar.
1. Seleccione *myDedicatedHostsRG* como **grupo de recursos**.
1. En los **detalles de la instancia**, escriba *myHost* en el **nombre** y seleccione *Este de EE. UU.* como ubicación.
1. En **Perfil de hardware**, seleccione *Familia Standard Es3: tipo 1* para **Familia de tamaños**, *myHostGroup* para **Grupo host** y luego *1* para **Dominio de error**. En el resto de los campos, deje los valores predeterminados.
1. Cuando termine, seleccione **Revisar y crear** y, a continuación, espere a la confirmación.
1. Cuando reciba el mensaje de **Validación superada**, seleccione **Crear** para crear el host.


