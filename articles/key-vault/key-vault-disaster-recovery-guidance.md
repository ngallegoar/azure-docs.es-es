---
title: 'Qué hacer si se produce una interrupción del servicio de Azure que afecte a Azure Key Vault: Azure Key Vault | Microsoft Docs'
description: Descubra qué hacer en caso de que se produzca una interrupción del servicio de Azure que afecte a Azure Key Vault.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 530ae2b795b4d94802e9f0d3420f7b3af86936ad
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78184645"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Redundancia y disponibilidad de Azure Key Vault

Azure Key Vault tiene varias capas de redundancia para garantizar la disponibilidad de las claves y los secretos para su aplicación, aunque se produzcan errores de componentes individuales del servicio.

El contenido del almacén de claves se replica dentro de la región y en una región secundaria que se encuentre a una distancia mínima de 241 km pero dentro de la misma ubicación geográfica. Así se mantiene la durabilidad de las claves y los secretos. Consulte el artículo sobre las [regiones emparejadas de Azure](../best-practices-availability-paired-regions.md) para obtener más información sobre pares de regiones específicas.

Si se produce un error en algún componente individual dentro del servicio del almacén de claves, los componentes alternativos de la región se encargan de atender la solicitud para garantizar que no se pierde funcionalidad. No es necesario realizar ninguna acción para ello. Se lleva a cabo automáticamente y es transparente para el programador.

En el caso excepcional de que toda una región de Azure pase a estar no disponible, las solicitudes efectuadas a Azure Key Vault de esa región se enrutarán automáticamente (un proceso conocido como *conmutación por error*) a una región secundaria. Cuando la región primaria vuelva a estar disponible, las solicitudes se enrutarán a ella nuevamente (*conmutación por recuperación*). Conviene insistir en que no es necesaria ninguna acción, ya que este proceso se realiza automáticamente.

En este diseño de alta disponibilidad, Azure Key Vault no requiere ningún tiempo de inactividad para las actividades de mantenimiento.

Hay algunas advertencias que deben tenerse en cuenta:

* La conmutación por error de región, en caso de producirse, puede tardar varios minutos en completarse. Las solicitudes realizadas durante este intervalo podrían no efectuarse correctamente mientras no se complete la conmutación.
* Una vez finalizada una conmutación por error, el almacén de claves se encontrará en modo de solo lectura. Las solicitudes compatibles con este modo son las siguientes:
  * Enumeración de almacenes de claves
  * Obtención de propiedades de los almacenes de claves
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
* Cuando tenga lugar la conmutación por error, todos los tipos de solicitudes (incluidas de lectura *y* escritura) pasarán a estar disponibles.

