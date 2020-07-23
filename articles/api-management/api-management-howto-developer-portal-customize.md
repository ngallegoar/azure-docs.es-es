---
title: Acceso a la versión administrada del portal para desarrolladores y su personalización - Azure API Management | Microsoft Docs
description: Aprenda a usar la versión administrada del portal para desarrolladores en API Management.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/05/2020
ms.author: apimpm
ms.openlocfilehash: 345b4950e25f48eb1a48c5e583908d0f771771ba
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206246"
---
# <a name="access-and-customize-developer-portal"></a>Acceso al portal para desarrolladores y su personalización

El portal para desarrolladores es un sitio web totalmente personalizable que se genera automáticamente con la documentación de las API. Aquí, los consumidores de API pueden detectar las API, aprender a usarlas y solicitar acceso.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * acceder a la versión administrada del portal para desarrolladores;
> * navegar por su interfaz administrativa;
> * personalizar el contenido;
> * publicar los cambios;
> * ver el portal publicado.

Puede encontrar más detalles del portal para desarrolladores en [Introducción al portal para desarrolladores de Azure API Management](api-management-howto-developer-portal.md).

![Modo de administrador del portal para desarrolladores de API Management](media/api-management-howto-developer-portal-customize/cover.png)

## <a name="prerequisites"></a>Requisitos previos

- Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
- Importe y publique una instancia de Azure API Management. Para obtener más información, consulte [Importación y publicación](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-the-portal-as-an-administrator"></a>Acceso al portal como administrador

Siga los pasos siguientes para acceder a la versión administrada del portal.

1. Vaya a la instancia de servicio de API Management en Azure Portal.
1. Haga clic en el botón **Nuevo portal para desarrolladores (versión preliminar)** de la barra de navegación superior. Se abrirá una nueva pestaña del explorador con una versión administrativa del portal.

## <a name="understand-the-portals-administrative-interface"></a>Descripción de la interfaz administrativa del portal

### <a name="default-content"></a>Contenido predeterminado 

Si accede al portal por primera vez, se aprovisionará automáticamente y en segundo plano el contenido predeterminado. El contenido predeterminado se ha diseñado para presentar las funcionalidades del portal y minimizar la cantidad de personalizaciones necesarias para personalizarlo. Puede obtener más información sobre lo que se incluye en el contenido del portal en [Introducción al portal para desarrolladores de Azure API Management](api-management-howto-developer-portal.md).

### <a name="visual-editor"></a>Editor visual

Puede personalizar el contenido del portal con el editor visual. Las secciones de menú de la izquierda permiten crear o modificar las páginas, los elementos multimedia, los diseños, los menús, los estilos o la configuración de sitios web. Los elementos de menú de la parte inferior permiten cambiar entre las ventanillas (por ejemplo, móvil o escritorio), ver los elementos del portal visibles para los usuarios autenticados o anónimos, o guardar o deshacer acciones.

Puede agregar filas a una página haciendo clic en un icono azul con un signo más. Los widgets (por ejemplo, texto, imágenes o lista de API) se pueden agregar presionando un icono de color gris con un signo más. Puede reorganizar los elementos de una página con la interacción de arrastrar y colocar. 

### <a name="layouts-and-pages"></a>Diseños y páginas

![Páginas y diseños](media/api-management-howto-developer-portal-customize/pages-layouts.png)

Los diseños definen cómo se muestran las páginas. Por ejemplo, en el contenido predeterminado, hay dos diseños: uno se aplica a la página principal; y el otro, a todas las páginas restantes.

Para aplicar un diseño a una página, se hace coincidir la plantilla de dirección URL con la dirección URL de la página. Por ejemplo, el diseño con una plantilla de dirección URL de `/wiki/*` se aplicará a cada página con el segmento `/wiki/`: `/wiki/getting-started`, `/wiki/styles` y así sucesivamente.

En la imagen anterior, el contenido que pertenece al diseño está marcado en azul, mientras que la página se marca en rojo. Las secciones de menú se marcan respectivamente.

### <a name="styling-guide"></a>Guía de estilos

![Guía de estilos](media/api-management-howto-developer-portal-customize/styling-guide.png)

La guía de estilos es un panel creado teniendo en cuenta a los diseñadores. Permite supervisar todos los elementos visuales del portal y aplicar estilos en ellos. Los estilos son jerárquicos: muchos elementos heredan propiedades de otros elementos. Por ejemplo, los elementos de botón usan colores para el texto y el fondo. Para cambiar el color de un botón, debe cambiar la variante de color original.

Para editar una variante, haga clic en ella y seleccione el icono del lápiz que aparece encima. Una vez realizados los cambios en la ventana emergente, ciérrela.

### <a name="save-button"></a>Botón Guardar

![Botón Guardar](media/api-management-howto-developer-portal-customize/save-button.png)

Siempre que realice un cambio en el portal, deberá guardarlo manualmente. Para ello, seleccione el botón **Guardar** del menú de la parte inferior. Al guardar los cambios, el contenido modificado se carga automáticamente en el servicio de API Management.

## <a name="customize-the-portals-content"></a>Personalización del contenido del portal

Antes de que el portal esté disponible para los visitantes, debe personalizar el contenido generado automáticamente. Entre los cambios recomendados se incluyen los diseños, los estilos y el contenido de la página principal.

> [!NOTE]
> Debido a las consideraciones sobre integración, las páginas siguientes no pueden quitarse ni moverse a una dirección URL diferente: `/404`, `/500`, `/captcha`, `/change-password`, `/config.json`, `/confirm/invitation`, `/confirm-v2/identities/basic/signup`, `/confirm-v2/password`, `/internal-status-0123456789abcdef`, `/publish`, `/signin`, `/signin-sso` y `/signup`.

### <a name="home-page"></a>Página de inicio

La página de **inicio** predeterminada se rellena con contenido ficticio. Puede quitar todas las secciones con el contenido o mantener la estructura y ajustar los elementos uno a uno. Reemplace las imágenes y el texto generados por los suyos y asegúrese de que los vínculos señalan a las ubicaciones deseadas.

### <a name="layouts"></a>Diseños

Reemplace el logotipo generado automáticamente en la barra de navegación por el suyo.

### <a name="styling"></a>Estilos

Aunque no es necesario ajustar ningún estilo, puede ajustar elementos determinados. Por ejemplo, cambie el color principal para que coincida con el de la marca.

### <a name="customization-example"></a>Ejemplo de personalización

En el siguiente vídeo se muestra cómo editar el contenido del portal, personalizar el aspecto del sitio web y publicar los cambios.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="publish-the-portal"></a><a name="publish"> </a>Publicación del portal

Para que el portal y los últimos cambios estén disponibles para los visitantes, debe publicarlos.

1. Asegúrese de guardar los cambios haciendo clic en el icono de **Guardar**.
1. Haga clic en **Publicar sitio web**, en la sección **Operaciones** del menú. Esta operación puede tardar algunos minutos.  
    ![Publicación del portal](media/api-management-howto-developer-portal-customize/publish-portal.png)

> [!NOTE]
> El portal debe volver a publicarse cuando cambie la configuración del servicio de API Management; por ejemplo, al asignar un dominio personalizado, actualizar los proveedores de identidades, configurar la delegación, especificar las condiciones de inicio de sesión y del producto, etc.

## <a name="visit-the-published-portal"></a>Visita del portal publicado

Después de publicar el portal, puede acceder a él en la misma dirección URL que el panel administrativo, por ejemplo, `https://contoso-api.developer.azure-api.net`. Véalo en una sesión de explorador independiente (modo de exploración de incógnito o privada) como visitante externo.

## <a name="apply-the-cors-policy-on-apis"></a>Aplicación de la directiva CORS en las API

Debe habilitar CORS (uso compartido de recursos entre orígenes) en las API para que los visitantes del portal puedan probar las API con la consola interactiva integrada. Consulte [este artículo de la documentación](api-management-howto-developer-portal.md#cors) para más detalles.

## <a name="next-steps"></a>Pasos siguientes
- [¿Quiere optimizar y ahorrar en el gasto en la nube?](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)

Obtenga más información sobre el portal para desarrolladores:

- [Introducción al portal para desarrolladores de Azure API Management](api-management-howto-developer-portal.md)