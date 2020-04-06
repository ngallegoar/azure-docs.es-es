---
title: Cifrado de datos en reposo del servicio de voz
titleSuffix: Azure Cognitive Services
description: Cifrado de datos en reposo del servicio de voz.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 3ccc9820f38a8c32d0b390663eb6b4430b42e8f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372085"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Cifrado de datos en reposo del servicio de voz

El servicio de voz cifra de forma automática los datos al guardarlos en la nube. El cifrado del servicio de voz protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y de seguridad de la organización.

## <a name="about-cognitive-services-encryption"></a>Información sobre el cifrado de Cognitive Services

Los datos se cifran y descifran con el cifrado [AES de 256 bits](https://en.wikipedia.org/wiki/FIPS_140-2) compatible con [FIPS 140-2](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). El cifrado y el descifrado son transparentes, lo que significa que el cifrado y el acceso se administran automáticamente. Dado que los datos están protegidos de forma predeterminada, no es necesario modificar el código ni las aplicaciones para utilizar el cifrado.

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

Cuando se usan Custom Speech y Voz personalizada, el servicio de voz puede almacenar los siguientes datos en la nube:  

* Datos de seguimiento de voz (solo si activa el seguimiento para su punto de conexión personalizado)
* Carga de datos de entrenamiento y prueba

De forma predeterminada, sus datos se guardan en el almacenamiento de Microsoft y su suscripción usa claves de cifrado administradas por Microsoft. También tiene la opción de preparar su propia cuenta de almacenamiento. El acceso a la tienda lo administra la identidad administrada y el servicio de voz no puede acceder directamente a sus propios datos, como los datos de seguimiento de voz, datos de entrenamiento de personalización y modelos personalizados.

Para más información sobre la identidad administrada, consulte [¿Qué son las identidades administradas?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Traiga su propio almacenamiento (BYOS) para la personalización y el registro

Para solicitar acceso para traer su propio almacenamiento, rellene y envíe el  [formulario de solicitud del servicio de voz: traiga su propia cuenta de almacenamiento (BYOS)](https://aka.ms/cogsvc-cmk). Una vez que reciba la aprobación, será preciso que cree su propia cuenta de almacenamiento para almacenar los datos necesarios para la personalización y el registro. Al agregar una cuenta de almacenamiento, el recurso del servicio de voz habilitará una identidad administrada asignada por el sistema. Después de que se habilite la identidad administrada asignada por el sistema, este recurso se registrará con Azure Active Directory (AAD). Tras su registro, se concederá acceso a la identidad administrada a la cuenta de almacenamiento. Aquí puede obtener más información acerca de las identidades administradas. Para más información sobre la identidad administrada, consulte [¿Qué son las identidades administradas?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

> [!IMPORTANT]
> Si deshabilita identidades administradas asignadas por el sistema, se quitará el acceso a la cuenta de almacenamiento. Esto hará que las partes del servicio de voz que requieran acceso a la cuenta de almacenamiento dejen de funcionar.  

## <a name="regional-availability"></a>Disponibilidad regional

BYOS está disponible actualmente en estas regiones:

* Centro y Sur de EE. UU.
* Oeste de EE. UU. 2
* Este de EE. UU.

## <a name="next-steps"></a>Pasos siguientes

* [Formulario de solicitud del servicio de voz: traiga su propia cuenta de almacenamiento (BYOS)](https://aka.ms/cogsvc-cmk)
* [¿Qué son las identidades administradas?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
