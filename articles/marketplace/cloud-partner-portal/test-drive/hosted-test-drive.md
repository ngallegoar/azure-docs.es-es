---
title: Versión de prueba hospedada | Azure Marketplace
description: Configuración y mantenimiento de una versión de prueba hospedada de Marketplace
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6d054064e236e121e02bf58a0eb73b5a62f24a09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278270"
---
# <a name="hosted-test-drive"></a>Versión de prueba hospedada

Una versión de prueba hospedada elimina la complejidad de la configuración haciendo que Microsoft hospede y mantenga el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba. Este artículo es para aquellos anunciantes que tienen su oferta en AppSource o que van a crear una nueva y desean ofrecer una versión de prueba hospedada que se conecte con una instancia de Dynamics 365 for Customer Engagement, Dynamics 365 for Finance and Operations, o Dynamics 365 Business Central.

## <a name="how-to-publish-a-test-drive"></a>Publicación de una versión de prueba

Vaya a la oferta existente o cree una nueva.

Seleccione la opción Versión de prueba en el menú lateral.

Seleccione \'Sí\' en la opción \'Enable a Test Drive\' (Habilitar una versión de prueba).

Proporcione los siguientes campos en la sección \'Detalles\'.

- **Descripción**: Proporcione una introducción de la versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo es compatible con HTML, si desea proporcionar contenido con formato.
- **Manual del usuario**: Cargue un manual del usuario detallado (archivo de tipo .pdf) que ayude a los usuarios de la versión de prueba a aprender a usar la aplicación.
- **Vídeo de demostración de la versión de prueba**: Opcionalmente, puede cargar un vídeo que presente la aplicación.

Conceda permiso a AppSource para aprovisionar y desaprovisionar los usuarios de la versión de prueba del inquilino mediante las instrucciones que puede encontrar [aquí](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md).

En este paso, generará los valores \'Identificador de aplicación de Azure AD\' y \'Clave de aplicación de Azure AD\' que se mencionan a continuación.

Proporcione los siguientes campos en la sección de \'configuración técnica\':

- **Tipo de versión de prueba**: Elija la opción \'Hospedada en Microsoft (por ejemplo Dynamics 365 for Customer Engagement). Esto indica que Microsoft hospedará y mantendrá el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.
- **Número máximo de versiones de prueba simultáneas**: Establezca este campo en el número de usuarios simultáneos que puede tener una versión de prueba activa en cualquier momento dado. Cada usuario consumirá una licencia de Dynamics mientras que esté activa su versión de prueba, por lo que tendrá que garantizar este número de licencias de Dynamics para que estén disponibles para los usuarios de la versión de prueba. Se recomienda un valor entre 3 y 5.
- **Duración de la versión de prueba (horas)** : Establezca este campo en el número de horas que estará activa la versión de prueba para los usuarios. Después de este número de horas, se desaprovisionará al usuario del inquilino. Se recomienda un valor entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y desea volver a acceder a la versión de prueba.
- **Dirección URL de la instancia**: Proporcione una dirección URL por la que el usuario navegará inicialmente cuando inicie la versión de prueba. Esta suele ser la dirección URL de la instancia de Dynamics 365 que contiene la aplicación y los datos de ejemplo. Valor de ejemplo: https:\//testdrive.crm.dynamics.com
- **Identificador de inquilino de Azure AD**: proporcione el identificador del inquilino de Azure para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a \'Azure Active Directory\' -\> Seleccionar propiedades en la hoja del menú -\> Copiar identificador de directorio. Valor de ejemplo: 72f988bf-86f1-41af-91ab-2d7cd0111234
- **Identificador de aplicación de Azure AD**: El identificador de la aplicación de Azure AD que creó en el paso 7. Valor de ejemplo: 53852862-a2ae-4e43-9461-faa49650a096
- **Clave de aplicación de Azure AD**: Secreto de la aplicación de Azure AD que creó en el paso 7. Valor de ejemplo: IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk =
- **Nombre del inquilino de Azure AD**: proporcione el nombre del inquilino de Azure para la instancia de Dynamics 365. Use el formato \<nombreinquilino.\> onmicrosoft.com. Valor de ejemplo: testdrive.onmicrosoft.com
- **Dirección URL de API web de la instancia**: Proporcione la dirección URL de API web para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en la instancia de Microsoft Dynamics 365 y vaya a Configuración -\> Personalización -\> Recursos de desarrollador -\> API web de la instancia (copie esta dirección URL). Valor de ejemplo: https:\//testdrive.crm.dynamics.com/api/data/v9.0
- **Nombre del rol**: proporcione el nombre del rol de Dynamics 365 Security personalizado que ha creado para la versión de prueba. Este es el rol que se asignará a los usuarios durante su versión de prueba. Valor de ejemplo: testdriverole

## <a name="next-steps"></a>Pasos siguientes

Cuando esté listo, **publique** su oferta. Una vez que la aplicación haya pasado la certificación, podrá ver una **versión preliminar** de la oferta. Inicie una versión de prueba en la interfaz de usuario y compruebe que las versiones de prueba se están ejecutando correctamente. Cuando esté satisfecho con la versión preliminar de la oferta, **publíquela**.
