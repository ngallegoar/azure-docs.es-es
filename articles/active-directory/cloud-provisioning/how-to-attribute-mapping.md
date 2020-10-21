---
title: Editor de atributos de aprovisionamiento en la nube de Azure AD Connect
description: En este artículo se describe cómo usar el editor de atributos.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/22/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58b7aa0bf8c82990a00b4e41041145a67ee2f02b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91636991"
---
# <a name="azure-ad-connect-cloud-provisioning-attibute-mapping"></a>Asignación de atributos de aprovisionamiento en la nube de Azure AD Connect

El aprovisionamiento en la nube de Azure AD Connect ha agregado una característica nueva que le permitirá asignar fácilmente atributos entre los objetos de usuario o de grupo locales y los objetos de Azure AD.  Esta característica se ha agregado a la configuración de aprovisionamiento en la nube.

Puede personalizar las asignaciones de atributos predeterminadas según sus necesidades empresariales. Esto significa que puede cambiar o eliminar asignaciones de atributos existentes o crear nuevas asignaciones de atributos.  Para obtener una lista de los atributos que se sincronizan, consulte los [atributos que se sincronizan](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

## <a name="understanding-attribute-mapping-types"></a>Información sobre los tipos de asignación de atributos
Con las asignaciones de atributos, puede controlar cómo se rellenan los atributos en Azure AD.
Se admiten cuatro tipos de asignaciones diferentes:

- **Directa** : el atributo de destino se rellena con el valor de un atributo del objeto vinculado en AD.
- **Constante**: el atributo de destino se rellena con una cadena específica que se ha especificado.
- **Expresión** : el atributo de destino se rellena según el resultado de una expresión similar a un script.
  Para obtener más información, consulte [Escritura de expresiones para la asignación de atributos](reference-expressions.md).
- **Ninguno** : el atributo de destino se deja sin modificar. Sin embargo, si el atributo de destino está vacío, se rellena con el valor predeterminado que especifique.

Además de estos cuatro tipos básicos, las asignaciones de atributos personalizadas admiten el concepto de una asignación de valor **predeterminada** opcional. La asignación de valor predeterminada garantiza que un atributo de destino se rellene con un valor si no hay ningún valor en Azure AD ni en el objeto de destino. La configuración más habitual consiste en dejarlo en blanco.

## <a name="understanding-attribute-mapping-properties"></a>Información sobre las propiedades de asignación de atributos

En la sección anterior, ya ha introducido la propiedad de tipo de asignación de atributos.
Además de esta propiedad, las asignaciones de atributos también admiten los siguientes atributos:

- **Atributo de origen**: especifica el atributo de usuario del sistema de origen (por ejemplo, Active Directory).
- **Atributo de destino**: especifica el atributo de usuario del sistema de destino (por ejemplo, Azure Active Directory).
- **Valor predeterminado si es nulo (opcional)** : valor que se enviará al sistema de destino si el atributo de origen es NULL. Este valor solo se aprovisionará cuando se cree un usuario. El "valor predeterminado si es nulo" no se aprovisionará al actualizar un usuario existente.  
- **Aplicar esta asignación**
  - **Siempre**: esta asignación se aplica a las acciones de creación y actualización de usuarios.
  - **Solo durante la creación**: esta asignación se aplica solo a las acciones de creación de usuarios.

> [!NOTE]
> En este documento se describe cómo usar Azure Portal para asignar atributos.  Para obtener información sobre el uso de Graph, consulte [Transformaciones](how-to-transformation.md).

## <a name="using-attribute-mapping"></a>Uso de las asignaciones de atributos
Para usar esta nueva característica, siga los pasos que se indican a continuación.

 1.  En Azure Portal, seleccione **Azure Active Directory**.
 2.  Seleccione **Azure AD Connect**.
 3.  Seleccione **Administrar aprovisionamiento**.

   ![Administración del aprovisionamiento](media/how-to-configure/manage1.png)
 
 4. En **Configuración**, seleccione su configuración.
 5. Seleccione **Click to edit mappings** (Haga clic para editar las asignaciones).  Se abrirá la pantalla de asignación de atributos.

 ![Adición de atributos](media/how-to-attribute-mapping/mapping6.png)
 6.  Haga clic en **Agregar atributo**.

 ![Tipo de asignación](media/how-to-attribute-mapping/mapping1.png)
 
 7. Seleccione el **Tipo de asignación**.  En este ejemplo usaremos Expresión.
 8.  Escriba la expresión en el cuadro.  En este ejemplo, estamos usando `Replace([mail], "@contoso.com", , ,"", ,).`.
 9.  Escriba el atributo de destino.  En este ejemplo, usaremos ExtensionAttribute15.
 10. Seleccione cuándo quiere aplicarlo y, a continuación, haga clic en **Aplicar**.
   
   ![Editar asignaciones](media/how-to-attribute-mapping/mapping2a.png)
 11. En la pantalla asignación de atributos, debería ver la nueva asignación de atributos.  
 12. Haga clic en **Guardar esquema**.

 ![Guardar esquema](media/how-to-attribute-mapping/mapping3.png)

## <a name="test-your-attribute-mapping"></a>Prueba de la asignación de atributos
Para probar la asignación de atributos, puede usar el [ aprovisionamiento a petición](how-to-on-demand-provision.md).  Desde la lista 

1.  En Azure Portal, seleccione **Azure Active Directory**.
2.  Seleccione **Azure AD Connect**.
3.  Seleccione **Administrar aprovisionamiento**.
4. En **Configuración**, seleccione su configuración.
5. En **Validar**, haga clic en el botón **Aprovisionar un usuario**. 
6. En la pantalla del aprovisionamiento a petición,  Escriba el **nombre distintivo** de un usuario o grupo y haga clic en el botón **Aprovisionar**.  
7. Una vez finalizado, verá una pantalla de éxito y 4 casillas de color verde que indican que el elemento se ha aprovisionado correctamente.  
  ![Aprovisionamiento realizado correctamente](media/how-to-attribute-mapping/mapping4.png)
1. En **Realizar una acción** haga clic en **Ver detalles**.  A la derecha, debería ver el nuevo atributo sincronizado y la expresión aplicada.

  ![Realizar acción](media/how-to-attribute-mapping/mapping5.png)

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
- [Escritura de expresiones para la asignación de atributos](reference-expressions.md)
- [Atributos que se han sincronizado](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)