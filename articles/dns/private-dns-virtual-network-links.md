---
title: ¿Qué es un subrecurso de vínculo de red virtual de Azure DNS Private Zones?
description: Información general de los subrecursos de vínculo de red virtual de una zona privada de Azure DNS
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: 9181ef93dfedbc28b297bef48a0bc37ba6d69798
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75646768"
---
# <a name="what-is-a-virtual-network-link"></a>¿Qué es un vínculo de red virtual?

Una vez que se crea una zona DNS privada en Azure, no es accesible inmediatamente desde ninguna red virtual. Debe vincularla a una red virtual antes de que una máquina virtual hospedada en esa red pueda acceder a la zona DNS privada.
Para vincular una zona DNS privada a una red virtual, debe crear un vínculo de red virtual bajo la zona DNS privada. Cada zona DNS privada tiene una colección de recursos secundarios de vínculo de red virtual. Cada uno de estos recursos representa una conexión a una red virtual.

Puede vincular una red virtual a una zona DNS privada como red virtual de registro o como red virtual de resolución.

## <a name="registration-virtual-network"></a>Red virtual de registro

Cuando [crea un vínculo](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal#link-the-virtual-network) entre una zona DNS privada y una red virtual, tiene la opción de activar el [registro automático](./private-dns-autoregistration.md) de los registros DNS para las máquinas virtuales. Si elige esta opción, la red virtual se convierte en una red virtual de registro para la zona DNS privada. Se crea automáticamente un registro DNS para las máquinas virtuales que se implementan en la red. Los registros DNS se crean para las máquinas virtuales que ya se han implementado en la red virtual. Desde la perspectiva de la red virtual, la zona DNS privada se convierte en la zona de registro de esa red virtual.
Una zona DNS privada puede tener varias redes virtuales de registro; sin embargo, cada red virtual puede tener exactamente una zona de registro asociada.

## <a name="resolution-virtual-network"></a>Resolución de una red virtual

Al crear un vínculo de red virtual en una zona DNS privada y elegir no habilitar el registro automático de los registros DNS, la red virtual se trata como una red virtual de solo resolución. Los registros DNS de las máquinas virtuales implementadas en estas redes no se crearán automáticamente en la zona DNS privada vinculada. Sin embargo, las máquinas virtuales implementadas en este tipo de red pueden consultar correctamente los registros DNS de la zona DNS privada. Puede crear estos registros manualmente o se pueden rellenar desde otras redes virtuales que se hayan vinculado como redes de registro con la zona DNS privada.
Una zona DNS privada puede tener varias redes virtuales de resolución y una red virtual puede tener varias zonas de resolución asociadas.

## <a name="limits"></a>límites

Para conocer el número de redes de registro y de resolución que puede vincular a zonas DNS privadas, consulte [Límites de Azure DNS](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits)

## <a name="other-considerations"></a>Otras consideraciones

* No se admiten las redes virtuales implementadas mediante el modelo de implementación clásica.

* Solo puede crear un vínculo entre una zona DNS privada y una red virtual.

* Cada vínculo de red virtual en una zona DNS privada debe tener un nombre único en el contexto de la zona DNS privada. Puede tener vínculos con el mismo nombre en distintas zonas DNS privadas.

* Después de crear un vínculo de red virtual, compruebe el campo "Estado del vínculo" del recurso de vínculo de red virtual. En función del tamaño de la red virtual, puede tardar unos minutos antes de que el vínculo sea operativo y el estado del vínculo cambie a *Completado*.

* Cuando se elimina una red virtual, se eliminan automáticamente todos los vínculos de red virtual y los registros DNS registrados automáticamente asociados a él en diferentes zonas DNS privadas.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo vincular una red virtual a una zona DNS privada mediante [Azure Portal](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal#link-the-virtual-network)

* Obtenga información sobre cómo crear una zona privada en Azure DNS mediante [Azure PowerShell](./private-dns-getstarted-powershell.md) o la [CLI de Azure](./private-dns-getstarted-cli.md).

* Obtenga información sobre algunos [escenarios de zona privada](./private-dns-scenarios.md) habituales que pueden realizarse con zonas privadas en Azure DNS.

* Para ver algunas preguntas y respuestas comunes sobre las zonas privadas en Azure DNS, como el comportamiento específico que se puede esperar con determinadas clases de operaciones, consulte [Preguntas frecuentes de DNS privado](./dns-faq-private.md).
