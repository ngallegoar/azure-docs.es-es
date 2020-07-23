---
title: Acceso seguro a Azure Data Catalog
description: En este artículo se explica cómo proteger un catálogo de datos y sus activos de datos en Azure Data Catalog.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: how-to
ms.date: 08/01/2019
ms.openlocfilehash: 6a429c09b6f8082c95e29bcea62d27ec4fb46fd3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523390"
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Acceso seguro al catálogo de datos y a los activos de datos

> [!IMPORTANT]
> Esta característica solo está disponible en la edición estándar de Azure Data Catalog.

Azure Data Catalog le permite especificar quién puede tener acceso al catálogo de datos y qué operaciones (registrar, anotar o tomar posesión) pueden realizar en los metadatos en el catálogo. 

## <a name="catalog-users-and-permissions"></a>Usuarios del catálogo y permisos

Para proporcionar a un usuario o a un grupo acceso al catálogo de datos y establecer permisos:

1. En la [página principal de su catálogo de datos](https://www.azuredatacatalog.com), haga clic en **Configuración** en la barra de herramientas.

   ![Botón Configuración de la página principal de Azure Data Catalog](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)

2. En la página Configuración, expanda la sección **Usuarios del catálogo**.

   ![Botón Agregar en Usuarios de Azure Data Catalog](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)

3. Haga clic en **Agregar**.

4. Especifique el **nombre de usuario** completo o el nombre del **grupo de seguridad** en Azure Active Directory (AAD) asociados al catálogo. Use la coma (`,’) como separador si está agregando más de un usuario o grupo.

   ![Usuarios de Azure Data Catalog: usuarios o grupos](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)

5. Pulse **ENTRAR** o **TABULADOR** fuera del cuadro de texto. 

6. Confirme que todos los permisos (**anotar**, **registrar** y **tomar posesión**) se asignan a estos usuarios o grupos de forma predeterminada. Es decir, el usuario o grupo puede [registrar activos de datos]( data-catalog-how-to-register.md), [anotar los activos de datos]( data-catalog-how-to-annotate.md) y [tomar posesión de activos de datos]( data-catalog-how-to-manage.md). 

   ![Usuarios de Azure Data Catalog: permisos predeterminados](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)

7. Para conceder a un usuario o grupo el acceso de solo lectura en el catálogo, desactive la opción **anotar** opción para ese usuario o grupo. Al hacerlo, el usuario o grupo no podrá anotar los activos de datos en el catálogo, pero podrá verlos. 

8. Para denegar a un usuario o grupo el registro de activos de datos, borre la opción **registrar** para ese usuario o grupo.

9. Para denegar a un usuario la toma de posesión de un recurso de datos, desactive la opción **tomar posesión** para ese usuario o grupo. 

10. Para eliminar un usuario/grupo de los usuarios del catálogo, haga clic en **x** para el usuario/grupo en la parte inferior de la lista. 

   ![Usuarios del catálogo de Azure Data Catalog: icono X de Eliminar usuario](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

   > [!IMPORTANT]
   > Se recomienda que agregue grupos de seguridad a usuarios del catálogo en lugar de agregar usuarios directamente y asignar permisos. A continuación, agregue usuarios a los grupos de seguridad que coincidan con sus roles y su acceso requerido al catálogo.

## <a name="special-considerations"></a>Consideraciones especiales

- Los permisos asignados a grupos de seguridad son aditivos. Por ejemplo, un usuario pertenece a dos grupos. En un grupo tiene permisos de anotación y en otro no. Por lo tanto, el usuario tiene permisos de anotación. 
- Los permisos asignados explícitamente a un usuario reemplazan los permisos asignados a los grupos a los que pertenece el usuario. En el ejemplo anterior, agregó explícitamente el usuario a los usuarios del catálogo y no se asignaron permisos de anotación. El usuario no puede anotar activos de datos a pesar de que el usuario sea miembro de un grupo que tiene permisos de anotación.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Data Catalog](data-catalog-get-started.md)
