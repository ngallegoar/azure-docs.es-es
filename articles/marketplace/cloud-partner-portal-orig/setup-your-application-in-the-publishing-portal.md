---
title: Configuración de la aplicación en el portal de publicación
description: Instrucciones para configurar una aplicación en el Portal de publicación en la nube.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 2676dbf72309eeb51be1f08e7bae2c1502cc671e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280208"
---
# <a name="setup-your-application-in-the-publishing-portal"></a>Configuración de la aplicación en el portal de publicación

Ya está listo para configurar la aplicación en el portal de publicación.

## <a name="login-and-create-a-new-offer"></a>Inicio de sesión y creación de una nueva oferta

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En la barra de navegación de la izquierda, haga clic en "+ Nueva oferta" y seleccione "Dynamics 365 for Customer Engagement".

![Selección de una nueva oferta](./media/CRMScreenShot14.png)

1. Ahora se abre una vista del "Editor" para una nueva oferta, donde puede empezar a crearla.

![Pantalla Nueva oferta](./media/CRMScreenShot15.png)

1. Los "formularios" que se tienen que rellenar aparecen a la izquierda en la vista del "Editor". Cada "formulario" consta de un conjunto de campos que hay que rellenar. Los campos obligatorios están marcados con un asterisco rojo (\*).

Hay cuatro formularios principales para crear una oferta de Dynamics 365 for Customer Engagement

* Configuración de oferta
* Información técnica
* Detalles del escaparate
* Contactos

## <a name="fill-out-the-offer-settings-form"></a>Rellenado del formulario de configuración de oferta

El formulario de configuración de oferta es un formulario básico en el que se especifica la configuración de la oferta. Los diferentes campos se describen a continuación.

### <a name="offer-id"></a>Id. de oferta

Se trata de un identificador único de la oferta en un perfil del publicador. Este identificador será visible en las direcciones URL de producto. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. Este campo queda bloqueado en cuanto se lanza una oferta.

Por ejemplo, si un publicador **"contoso"** crea una oferta con el identificador de oferta **"sample-WebApp"** , aparecerá en AppSource como "https:\//appsource.microsoft.com./marketplace/apps/contoso.sample-WebApp?tab=Overview".

### <a name="publisher-id"></a>Id. de publicador

Esta lista desplegable permite elegir el perfil del publicador en el que desea publicar esta oferta. Este campo queda bloqueado en cuanto se lanza una oferta.

### <a name="name"></a>Nombre

Se trata del nombre para mostrar de la oferta. Este es el nombre que se mostrará en [AppSource](https://appsource.microsoft.com/). Puede tener un máximo de 50 caracteres.

Haga clic en "Guardar" para guardar lo que ha hecho hasta ahora. El paso siguiente sería agregar información técnica para la oferta.

## <a name="fill-out-the-technical-info-form"></a>Rellenado del formulario de información técnica


El formulario de información técnica es donde escribirá la información específica para su solución de Dynamics 365 for Customer Engagement. Si mantiene el mouse sobre el formulario podrá ver más información. Observe el ejemplo siguiente.

![Pantalla Información técnica](./media/CRMScreenShot16.png)

### <a name="application-info"></a>Información de la aplicación

La mayoría de los editores dejará estos campos con los valores predeterminados, Usuario, No, No y una dirección URL de configuración de aplicación en blanco, según lo que se muestra en la captura de pantalla anterior.

### <a name="crm-package"></a>Paquete CRM

![Información del paquete CRM](./media/CRMScreenShot17.png)

A continuación aparece una explicación de estos campos:

* File name of your package (Nombre de archivo del paquete): el nombre de archivo que creó en el paso anterior cuando se creó el archivo ZIP que es el paquete CRM de AppSource. En el ejemplo anterior, es "Microsoft\_SamplePackage.zip".
* Url of your package location (Dirección URL de la ubicación del paquete): la dirección URL de la cuenta de Azure Storage que contiene el nombre de archivo de paquete que se especificó anteriormente. Es la dirección URL	 que se creó en el Paso 9 de la sección anterior.
* Is there more than one crm package in your package file (¿Hay más de un paquete de CRM en el archivo de paquete?): Seleccione Yes **SOLO** si admite varias versiones de CRM con distintos paquetes. El valor sería "No" para la mayoría de los socios. Si selecciona Sí, debe crear paquetes de AppSource para cada versión de la solución. _Nota: Aquí NO se pregunta si tiene varios archivos **ZIP**. Si tiene varios archivos solution.zip pero una sola versión, de todos modos debe seleccionar "No". La herramienta de empaquetado los reunirá automáticamente._

### <a name="crm-package-availability"></a>Disponibilidad del paquete CRM

En esta sección, seleccione en qué regiones de CRM estará disponible el paquete. Para información sobre qué regiones de CRM abarcan qué países o regiones, consulte el vínculo: [https://o365datacentermap.azurewebsites.net/](https://o365datacentermap.azurewebsites.net/)

Nota: La implementación en Soberanía "Soberanía y US Gov Cloud" de Alemania REQUIERE PERMISO ESPECIAL y VALIDACIÓN DURANTE LA CERTIFICACIÓN

## <a name="storefront-details"></a>Detalles del escaparate

### <a name="offer-summary"></a>Resumen de la oferta

Este es un resumen de la propuesta de valor de la oferta. Aparecerá en la página de búsqueda de la oferta. Debe tener un máximo de 100 caracteres.

### <a name="offer-description"></a>Descripción de la oferta

Esta es la descripción que aparecerá en la página de detalles de la aplicación. El número máximo de caracteres permitidos es de 1300.

### <a name="industries"></a>Industrias

Seleccione el sector más afín a la aplicación. Si la aplicación está relacionada con varios sectores, puede dejar esta opción en blanco.

### <a name="categories"></a>Categorías

Seleccione las categorías que sean pertinentes para su aplicación. Seleccione un máximo de 3.

### <a name="app-type"></a>Tipo de aplicación

Seleccione el tipo de prueba que la aplicación habilitará en AppSource. "Gratuita" significa que la aplicación es gratuita. "Versión de prueba" significa que los clientes pueden probar su aplicación por un corto período de tiempo en AppSource. "Solicitud de versión de prueba" no es compatible con aplicaciones de Dynamics 365 for Customer Engagement. No seleccione esta opción.

### <a name="help-link-for-your-app"></a>Vínculo de ayuda de la aplicación

Escriba una dirección URL a una página con información de ayuda relacionada para la aplicación.

### <a name="supported-countriesregions"></a>Países y regiones admitidos

En este campo se determinan las regiones en las que estará disponible la versión de prueba.

### <a name="supported-languages"></a>Idiomas compatibles

Seleccione los idiomas que admite la aplicación. Si la aplicación es compatible con idiomas adicionales que no están en esta lista, publique su oferta y envíenos un correo electrónico a [appsource@microsoft.com](mailto:appsource@microsoft.com) para hacérnoslo saber.

### <a name="app-version"></a>Versión de la aplicación

Escriba el número de versión de la aplicación

### <a name="app-release-date"></a>Fecha de lanzamiento de la aplicación

Escriba la fecha de lanzamiento de la aplicación

### <a name="products-your-app-works-with-max-3"></a>Productos con los que funciona su aplicación (máximo de 3)

Indique los productos específicos con los que funciona la aplicación. Puede enumerar tres productos como máximo. Para agregar un producto a la lista, haga clic en el signo más (junto a la opción Nuevo) y se creará un nuevo campo de texto abierto en el que podrá escribir el nombre de un producto con el que funcione la aplicación.

### <a name="search-keywords-max-3"></a>Palabras clave de búsqueda (máximo de 3)

AppSource permite que el cliente haga una búsqueda basada en palabras clave. Puede escribir el conjunto de palabras para las cuales la aplicación se mostrará a los clientes.

Por ejemplo, si la aplicación es "Mi servicio de correo electrónico", Correo electrónico, Correo, Servicio de correo podrían ser algunas palabras clave. Elija las palabras que es probable que los usuarios usen para buscar su aplicación en el cuadro de búsqueda de AppSource.

### <a name="hide-key"></a>Ocultación de la clave

Esta es una clave que se combinará con la dirección URL de la versión preliminar de la oferta para ocultarla de la vista pública. No es una contraseña. Puede especificar cualquier cadena aquí.

### <a name="offer-logo-png-format-48x48"></a>Logotipo de la oferta (formato .png, 48 x 48)

Aparecerá en la página de búsqueda de la aplicación. **Solo se permite el formato .png.** Cargue una imagen .png con una resolución de 48 px \* 48 px.

### <a name="offer-logo-png-format-216x216"></a>Logotipo de la oferta (formato .png, 216 x 216)

Aparecerá en la página de detalles de la aplicación. **Solo se permite el formato .png.** Cargue una imagen .png con una resolución de 216 px \* 216 px.

### <a name="videos"></a>Vídeos

Puede cargar un máximo de cuatro vídeos. Para cada vídeo que quiera cargar, debe completar el nombre del vídeo, la dirección URL (solo para YouTube o Vimeo) y la miniatura que se asociará al vídeo. La miniatura debe estar en formato .png y debe ser de 1280 px \* 720 px. Para agregar vídeos nuevos, haga clic en el signo +. Aparecerán miniaturas de vídeo en la página de detalles de la aplicación.

### <a name="documents"></a>Documentos

Puede subir un máximo de tres documentos en formato PDF. Para cada documento que quiera cargar, debe escribir el nombre del documento y cargarlo. Recuerde que el documento debe estar en formato PDF.

Para agregar documentos nuevos, haga clic en el signo +

### <a name="screenshots"></a>Capturas de pantalla

Son capturas de pantalla que aparecerán en la página de detalles de AppSource para la aplicación.

### <a name="privacy-policy"></a>Directiva de privacidad

Escriba la dirección URL de la directiva de privacidad de la aplicación

### <a name="terms-of-use"></a>Términos de uso

Escriba los términos de uso de la aplicación. Los clientes de AppSource deben aceptar estos términos antes de poder probar la aplicación

### <a name="support-url"></a>Dirección URL de soporte técnico

Escriba la dirección URL de soporte técnico de la aplicación.

### <a name="lead-destination"></a>Destino de clientes potenciales

Seleccione un sistema de CRM donde se guardará el cliente potencial. Seleccione "Tabla de Azure" si tiene uno de los siguientes sistemas de CRM: Salesforce, Marketo o Microsoft Dynamics CRM. El sistema de CRM que seleccione aquí es donde escribiremos los detalles de los usuarios finales que prueben su aplicación en AppSource (clientes potenciales). En función del sistema de CRM que seleccione, haga clic en la dirección URL correspondiente para obtener información sobre cómo completar el siguiente conjunto de campos

* [Tabla de Azure](./cloud-partner-portal-lead-management-instructions-azure-table.md)
* [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
* [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
* [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)

## <a name="storefront-details"></a>Detalles del escaparate

Los detalles de contacto solo se usan para comunicaciones internas entre asociados y Microsoft. Nota: Es importante usar una dirección de correo electrónico que se supervise en estos campos. Usaremos este correo electrónico para comunicarnos con usted con respecto al progreso en la publicación en AppSource. Los clientes solo podrán ver la dirección URL del soporte técnico.
