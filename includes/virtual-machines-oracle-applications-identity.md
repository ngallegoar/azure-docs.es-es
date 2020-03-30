---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: c1cca99a04e78c40cdd1061e52e49f45a73c73eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68361272"
---
### <a name="identity-tier"></a>Nivel de identidad 

La asociación entre Microsoft y Oracle le permite configurar una identidad unificada entre Azure, OCI y la aplicación de Oracle. Para el conjunto de aplicaciones de PeopleSoft o JD Edwards EnterpriseOne, se necesita una instancia del servidor HTTP de Oracle (OHS) para configurar el inicio de sesión único entre Azure AD y Oracle IDCS.

OHS actúa como un proxy inverso a la capa de aplicación, lo que significa que todas las solicitudes a las aplicaciones finales pasan por él. Oracle Access Manager WebGate es un complemento del servidor web de OHS que intercepta cada solicitud que va a una aplicación final. Si un recurso al que se está accediendo está protegido (requiere una sesión autenticada), WebGate inicia el flujo de autenticación de OIDC con Identity Cloud Service a través del explorador del usuario. Para más información sobre los flujos compatibles con WebGate de OpenID Connect, consulte la [documentación de Oracle Access Manager](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.3/aiaag/integrating-webgate-oidc-server.html).

Con esta configuración, un usuario que ya haya iniciado sesión en Azure AD puede navegar a la aplicación de PeopleSoft o JD Edwards EnterpriseOne sin tener que volver a iniciar sesión, a través de Oracle Identity Cloud Service. Los clientes que implementen esta solución obtienen las ventajas del inicio de sesión único, incluido un conjunto de credenciales único, una mejor experiencia de inicio de sesión, mayor seguridad y un menor costo del departamento de soporte técnico.

Para más información sobre cómo configurar el inicio de sesión único para JD Edwards EnterpriseOne o PeopleSoft con Azure AD, consulte las [notas del producto de Oracle](https://cloud.oracle.com/iaas/whitepapers/deploy_peoplesoft_jdedwards_across_oci_azure.pdf) asociadas.