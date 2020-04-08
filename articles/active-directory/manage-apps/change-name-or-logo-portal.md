---
title: Cambio del nombre o del logotipo de una aplicación empresarial en Azure AD
description: Cómo cambiar el nombre o el logotipo de una aplicación empresarial personalizada en Azure Active Directory
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d23849df75d1ab239eb269b462abb21df196e7e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79138508"
---
# <a name="change-the-name-or-logo-of-an-enterprise-application-in-azure-active-directory"></a>Cambio del nombre o del logotipo de una aplicación empresarial en Azure Active Directory

Cambiar el nombre o el logotipo de una aplicación empresarial personalizada en Azure Active Directory (Azure AD) es fácil. Debe tener los permisos adecuados para realizar estos cambios y debe ser el creador de la aplicación personalizada.

## <a name="how-do-i-change-an-enterprise-applications-name-or-logo"></a>¿Cómo puedo cambiar el nombre o el logotipo de una aplicación empresarial?

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) con una cuenta que tenga el rol de administrador global en el directorio. Aparecerá la página del **Centro de administración de Azure Active Directory**.
2. En el panel izquierdo, seleccione **Aplicaciones empresariales**. Aparecerá la lista de sus aplicaciones empresariales.
3. Seleccione una aplicación. Aparecerá la página de información general de la aplicación.
4. En el panel de información general de la aplicación, bajo el encabezado **Administrar**, seleccione **Propiedades**. Aparecerá la página **Propiedades**.
5. Si quiere cambiar el nombre, seleccione el cuadro **Nombre**, escriba el nombre nuevo y presione **ENTRAR**.
6. Si quiere cambiar el logotipo, busque el campo **Logotipo** y seleccione el icono de carpeta junto al cuadro **Seleccionar un archivo**, que está debajo de la imagen del logotipo actual de la aplicación.

   ![Selección del comando Propiedades](./media/change-name-or-logo-portal/change-logo.png)

   Si no va a cambiar el logotipo, vaya al paso 8.
7. En el selector de archivos, seleccione el archivo que quiera usar como nuevo logotipo. El nombre del archivo aparecerá en el cuadro debajo de la imagen del logotipo actual.

   > [!NOTE]
   > Azure requiere que la imagen de logotipo sea un archivo PNG y limita el ancho, el alto y el tamaño del archivo. Los logotipos personalizados deben tener exactamente 215 &times; 215 píxeles de tamaño y estar en formato PNG. Se recomienda usar un fondo de color sólido sin transparencia en el logotipo de la aplicación para que aparezca mejor para los usuarios.
8. Seleccione **Guardar**. Si elige un nuevo logotipo, la imagen del campo **Logotipo** cambiará para reflejar el nuevo archivo de logotipo.

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Visualización de los grupos y miembros de la organización en Azure Active Directory](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Asignar un usuario o grupo a una aplicación empresarial](assign-user-or-group-access-portal.md)
* [Quitar una asignación de usuario o grupo de una aplicación empresarial](remove-user-or-group-access-portal.md)
* [Deshabilitar los inicios de sesión de los usuarios de una aplicación empresarial](disable-user-sign-in-portal.md)
