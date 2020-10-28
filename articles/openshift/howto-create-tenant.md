---
title: Creación de un inquilino de Azure AD para Red Hat OpenShift en Azure
description: Aquí se muestra cómo crear un inquilino de Azure Active Directory (Azure AD) para hospedar el clúster de Red Hat OpenShift en Microsoft Azure.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: e949f1ac5259ba35772ce98c2ee88e5ea66c2d84
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221169"
---
# <a name="create-an-azure-ad-tenant-for-azure-red-hat-openshift"></a>Creación de un inquilino de Azure AD para Red Hat OpenShift en Azure

> [!IMPORTANT]
> Red Hat OpenShift en Azure 3.11 se retirará el 30 de junio de 2022. La compatibilidad con la creación de nuevos clústeres de Red Hat OpenShift en Azure 3.11 continúa hasta el 30 de noviembre de 2020. Después de la retirada, los clústeres de Red Hat OpenShift en Azure 3.11 que queden se cerrarán para evitar vulnerabilidades de seguridad.
> 
> Siga esta guía para [crear un clúster de la versión 4 de Red Hat OpenShift en Azure](tutorial-create-cluster.md).
> Si tiene alguna pregunta específica, póngase en [contacto con nosotros](mailto:arofeedback@microsoft.com).

Red Hat OpenShift en Microsoft Azure requiere un inquilino de [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) en el que crear el clúster. Un *inquilino* es una instancia dedicada de Azure AD que reciben una organización o un desarrollador de aplicaciones cuando crean una relación con Microsoft, como al registrarse en Azure, Microsoft Intune o Microsoft 365. Cada inquilino de Azure AD es distinto e independiente de los demás inquilinos de Azure AD y tiene sus propias identidades de trabajo y educativa y registros de aplicaciones.

Si aún no tiene un inquilino de Azure AD, siga estas instrucciones para crear uno.

## <a name="create-a-new-azure-ad-tenant"></a>Creación de un nuevo inquilino de Azure AD

Para crear un inquilino:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con la cuenta que desea asociar con el clúster de Red Hat OpenShift en Azure.
2. Abra la [hoja Azure Active Directory](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) para crear un nuevo inquilino (también conocido como un nuevo directorio de *Azure Active Directory* ).
3. Proporcione un **Nombre de organización** .
4. Proporcione un **Nombre de dominio inicial** . Este tendrá *onmicrosoft.com* anexado a él. Puede volver a usar el valor de *Nombre de organización* .
5. Elija un país o región donde se creará el inquilino.
6. Haga clic en **Crear** .
7. Una vez creado el inquilino de Azure AD, seleccione el vínculo **Haga clic aquí para administrar el nuevo directorio** . El nombre del nuevo inquilino se debe mostrar en la parte superior derecha de Azure Portal:  

    ![Captura de pantalla del portal que muestra el nombre del inquilino en la parte superior derecha][tenantcallout]  

8. Tome nota del *Identificador de inquilino* , para que pueda especificar más adelante dónde desea crear el clúster de Red Hat OpenShift en Azure. Ahora debería ver la hoja de información general de Azure Active Directory del nuevo inquilino en el portal. Seleccione **Propiedades** y copie el valor del **Identificador de directorio** . En el tutorial [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md), se hará referencia a este valor como `TENANT`.

[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="resources"></a>Recursos

Consulte la [documentación de Azure Active Directory](../active-directory/index.yml) para más información sobre los [inquilinos de Azure AD](../active-directory/develop/quickstart-create-new-tenant.md).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a crear a una entidad de servicio, generar un secreto de cliente y una dirección URL de devolución de llamada de autenticación y crear un nuevo usuario de Active Directory para probar aplicaciones en el clúster de Red Hat OpenShift en Azure.

[Creación de un objeto y un usuario de aplicación de Azure AD](howto-aad-app-configuration.md)