---
title: Incorporación de atributos personalizados para flujos de usuario de Azure AD
description: Obtenga información acerca de cómo personalizar los atributos de los flujos de usuario de registro de autoservicio.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f187cc47d9c64c8257cc097734fa41e10629f1c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594714"
---
# <a name="define-custom-attributes-for-user-flows-preview"></a>Definición de atributos personalizados para flujos de usuario (versión preliminar)
|     |
| --- |
| La característica de atributos de usuario personalizados es una característica en vista previa pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Para cada aplicación, puede tener requisitos diferentes para la información que quiere recopilar durante el registro. El inquilino de Azure AD incluye un conjunto integrado de información almacenada en atributos, como el nombre propio, los apellidos, la ciudad y el código postal. Con Azure AD, puede extender el conjunto de atributos almacenados en una cuenta de invitado cuando el usuario externo se registre a través de un flujo de usuario.

Puede crear atributos personalizados en Azure Portal y usarlos en los flujos de usuario de registro de autoservicio. También puede leer y escribir estos atributos mediante [Microsoft Graph API](https://docs.microsoft.com/azure/active-directory-b2c/manage-user-accounts-graph-api). Microsoft Graph API admite la creación y actualización de un usuario con atributos de extensión. Los atributos de extensión en Graph API se denominan mediante la convención `extension_<Application-client-id>_attributename`. Por ejemplo:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

Puede encontrar `<Application-client-id>` en la página de **Registros de aplicaciones** junto a **Id. de aplicación (cliente)** . Este id. es específico de su inquilino.

## <a name="create-a-custom-attribute"></a>Creación de un atributo personalizado

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. Seleccione **Atributos de usuario personalizados (versión preliminar)** . Se muestran los atributos de usuario disponibles.

   ![Selección de atributos de usuario para el registro](media/user-flow-add-custom-attributes/user-attributes.png)

5. Para agregar un atributo, seleccione **Agregar**.
6. En el panel **Agregar un atributo**, escriba los valores siguientes:

   - **Nombre**: proporcione un nombre para el atributo personalizado (por ejemplo, "ShoeSize").
   - **Tipo de datos**: elija un tipo de datos (**Cadena**, **Booleano** o **Int**).
   - **Descripción**: también puede escribir una descripción del atributo personalizado para uso interno. Esta descripción no es visible para el usuario.

   ![Agregar un atributo](media/user-flow-add-custom-attributes/add-an-attribute.png)

7. Seleccione **Crear**.

El atributo personalizado ya está disponible en la lista de atributos de usuario y puede usarlo en los flujos de usuario. Solo se crea la primera vez que se utiliza en cualquier flujo de usuario y no cuando se agrega a la lista de atributos de usuario.

Después de crear un usuario mediante un flujo de usuario que usa el atributo personalizado recién creado, el objeto se puede consultar en el [Explorador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer). Ahora debe ver **ShoeSize** en la lista de atributos que se recopilan durante el viaje de suscripción y en el token enviado de vuelta a la aplicación. Para obtener información sobre cómo incluir estas notificaciones en el token devuelto a la aplicación, consulte [Configuración de notificaciones opcionales de extensión de directorio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims#configuring-directory-extension-optional-claims).

## <a name="next-steps"></a>Pasos siguientes

[Incorporación de un flujo de usuario de registro de autoservicio a una aplicación](self-service-sign-up-user-flow.md)