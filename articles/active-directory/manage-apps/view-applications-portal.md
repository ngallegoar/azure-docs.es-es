---
title: 'Inicio rápido: Visualización de las aplicaciones del inquilino mediante Azure Active Directory'
description: En este inicio rápido, se utiliza Azure Portal para ver las aplicaciones del inquilino de Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/09/2019
ms.author: mimart
ms.reviewer: arvinh
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d40e968bb7079d50e3fa18889ae996c9b59c90f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74421820"
---
# <a name="quickstart-view-your-azure-active-directory-tenant-applications"></a>Inicio rápido: Visualización de las aplicaciones del inquilino de Azure Active Directory

Esta guía de inicio rápido utiliza Azure Portal para visualizar las aplicaciones del inquilino de Azure Active Directory (Azure AD).

## <a name="before-you-begin"></a>Antes de empezar

Para ver los resultados, debe tener al menos una aplicación en su inquilino de Azure AD. Para agregar una aplicación, consulte la guía de inicio rápido [Adición de una aplicación](add-application-portal.md).

Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global del inquilino de Azure AD, administrador de aplicaciones en la nube o administrador de aplicaciones.

## <a name="find-the-list-of-tenant-applications"></a>Búsqueda de la lista de aplicaciones del inquilino

Las aplicaciones del inquilino de Azure AD están visibles en la sección **Aplicaciones empresariales** de Azure Portal.

Para buscar las aplicaciones del inquilino:

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.
1. En el panel **Azure Active Directory**, seleccione **Aplicaciones empresariales**.
1. En el menú desplegable **Tipo de aplicación**, seleccione **Todas las aplicaciones** y elija **Aplicar**. Aparece una muestra aleatoria de las aplicaciones del inquilino.
1. Para ver más aplicaciones, seleccione **Cargar más** en la parte inferior de la lista. En función del número de aplicaciones del inquilino, puede resultar más fácil [buscar una aplicación específica](#search-for-a-tenant-application) en lugar de desplazarse por la lista.

## <a name="select-viewing-options"></a>Selección de las opciones de visualización

Seleccione las opciones en función de lo que busque.

1. Puede ver las aplicaciones por **tipo de aplicación**, **estado de aplicación** y **visibilidad de aplicación**.
1. En **Tipo de aplicación**, elija una de estas opciones:

    - **Aplicaciones empresariales** muestra las aplicaciones que no son de Microsoft.
    - **Aplicaciones de Microsoft** muestra las aplicaciones de Microsoft.
    - **Todas las aplicaciones** muestra las aplicaciones de Microsoft y las que no son de Microsoft.

1. En **Estado de la aplicación**, elija **Cualquiera**, **Deshabilitada** o **Habilitada**. La opción **Cualquiera** incluye tanto aplicaciones habilitadas como deshabilitadas.
1. En **Visibilidad de la aplicación**, elija **Cualquiera** u **Oculta**. La opción **Ocultas** muestra las aplicaciones que están en el inquilino, pero que los usuarios no pueden ver.
1. Después de elegir las opciones que desee, seleccione **Aplicar**.

## <a name="search-for-a-tenant-application"></a>Búsqueda de una aplicación del inquilino

Para buscar una aplicación concreta:

1. En el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y elija **Aplicar**.
1. Escriba el nombre de la aplicación que desea buscar. Si la aplicación se ha agregado al inquilino de Azure AD, aparece en los resultados de la búsqueda. En este ejemplo, se muestra que GitHub no se ha agregado a las aplicaciones del inquilino.

    ![En el ejemplo se muestra que una aplicación no se ha agregado al inquilino.](media/view-applications-portal/search-for-tenant-application.png)

1. Pruebe a escribir las primeras letras del nombre de una aplicación. En este ejemplo se muestran todas las aplicaciones que empiezan por **Sales**.

    ![En el ejemplo se muestran todas las aplicaciones que empiezan por Sales.](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a ver las aplicaciones de su inquilino de Azure AD. Ha aprendido a filtrar la lista de aplicaciones por tipo, estado y visibilidad de la aplicación. También ha aprendido cómo buscar una aplicación determinada.

Ahora que ha encontrado la aplicación que buscaba, puede continuar con [Incorporación de más aplicaciones a un inquilino](add-application-portal.md). O bien, puede seleccionar la aplicación para ver o editar las propiedades y las opciones de configuración. Por ejemplo, podría configurar el inicio de sesión único.

> [!div class="nextstepaction"]
> [Configuración del inicio de sesión único](configure-single-sign-on-non-gallery-applications.md)
