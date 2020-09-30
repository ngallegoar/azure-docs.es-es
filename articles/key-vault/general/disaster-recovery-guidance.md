---
title: 'Qué hacer si se produce una interrupción del servicio de Azure que afecte a Azure Key Vault: Azure Key Vault | Microsoft Docs'
description: Descubra qué hacer en caso de que se produzca una interrupción del servicio de Azure que afecte a Azure Key Vault.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: sudbalas
ms.openlocfilehash: 27d8d4de308fe7cf6e6f36dd33f33bb73c495073
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90983221"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Redundancia y disponibilidad de Azure Key Vault

Azure Key Vault tiene varias capas de redundancia para garantizar la disponibilidad de las claves y los secretos para su aplicación, aunque se produzcan errores de componentes individuales del servicio.

> [!NOTE]
> Esta guía se aplica a los almacenes. Los grupos de HSM administrados usan un modelo diferente de alta disponibilidad y recuperación ante desastres. Para más información, consulte [Guía de recuperación ante desastres de HSM administrado](../managed-hsm/disaster-recovery-guide.md).

El contenido del almacén de claves se replica dentro de la región y en una región secundaria que se encuentre a una distancia mínima de 241 km pero dentro de la misma ubicación geográfica para mantener una alta durabilidad de las claves y los secretos. Consulte el artículo sobre las [regiones emparejadas de Azure](../../best-practices-availability-paired-regions.md) para obtener más información sobre pares de regiones específicas.


Si se produce un error en algún componente individual dentro del servicio del almacén de claves, los componentes alternativos de la región se encargan de atender la solicitud para garantizar que no se pierde funcionalidad. No es necesario realizar ninguna acción para iniciar este proceso, ya que se realiza automáticamente y es transparente para el usuario.

En el caso excepcional de que toda una región de Azure pase a estar no disponible, las solicitudes efectuadas a Azure Key Vault de esa región se enrutarán automáticamente (un proceso conocido como *conmutación por error*) a una región secundaria. Cuando la región primaria vuelva a estar disponible, las solicitudes se enrutarán a ella nuevamente (*conmutación por recuperación*). Conviene insistir en que no es necesaria ninguna acción, ya que este proceso se realiza automáticamente.

En este diseño de alta disponibilidad, Azure Key Vault no requiere ningún tiempo de inactividad para las actividades de mantenimiento.

Hay algunas advertencias que deben tenerse en cuenta:

* La conmutación por error de región, en caso de producirse, puede tardar varios minutos en completarse. Es posible que se produzca un error en las solicitudes realizadas durante este tiempo anterior a la conmutación por error.
* Durante la conmutación por error, el almacén de claves está en modo de solo lectura. Las solicitudes compatibles con este modo son las siguientes:
  * Enumeración de certificados
  * Obtención de certificados
  * Enumeración de secretos
  * Obtención de secretos
  * Enumeración de claves
  * Obtención de (propiedades de) claves
  * Cifrado
  * Descifrado
  * Encapsulado
  * Desencapsulado
  * Verify
  * Firma
  * Copia de seguridad

* Durante la conmutación por error, no podrá realizar cambios en las propiedades del almacén de claves. No podrá cambiar la directiva de acceso ni los valores y la configuración del firewall.

* Cuando tenga lugar la conmutación por error, todos los tipos de solicitudes (incluidas de lectura *y* escritura) pasarán a estar disponibles.
