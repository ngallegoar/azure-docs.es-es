---
title: Configuración de los detalles de la lista de ofertas de máquinas virtuales en Azure Marketplace
description: Aprenda a configurar los detalles de la lista de ofertas de máquinas virtuales en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: 2af96c0c2e850d8e1be67c23e71aa677773c1e03
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283401"
---
# <a name="how-to-configure-virtual-machine-offer-listing-details"></a>Procedimientos para configurar los detalles las propiedades de la lista de ofertas de máquinas virtuales

En la página **Offer listing** (Descripción de la oferta), define los detalles de la oferta, como el nombre, la descripción, los vínculos y los contactos.

> [!NOTE]
> No es necesario que el contenido de la descripción de la oferta, por ejemplo, la descripción, los documentos, las capturas de pantallas y los términos de uso, esté en inglés si la descripción de la oferta comienza por la frase "Esta aplicación solo está disponible en \<non-English language>." También puede proporcionar una dirección URL que vincule a un sitio que ofrezca contenido en un idioma distinto del que se usa en el contenido de la descripción de la oferta.

## <a name="marketplace-details"></a>Detalles del marketplace

### <a name="name"></a>Nombre

El nombre que escriba aquí se muestra a los clientes como el título de la descripción de la oferta. Este campo se rellena automáticamente con el texto que escribió en el cuadro **Offer alias** (Alias de la oferta) al crear la oferta. Puede cambiar este nombre más adelante. El nombre:

- Puede ser una marca comercial. Puede incluir símbolos de marca comercial y copyright.
- No puede contener más de 50 caracteres.
- No puede incluir emojis.

### <a name="search-results-summary"></a>Resumen de los resultados de la búsqueda

Especifique una descripción breve de la oferta que se mostrará en los resultados de búsqueda de Azure Marketplace. Puede contener un máximo de 100 caracteres.

### <a name="long-summary"></a>Resumen largo

Especifique una descripción larga de la oferta que se va a mostrar en los resultados de búsqueda de Azure Marketplace. Puede contener un máximo de 256 caracteres.

### <a name="description"></a>Descripción

[!INCLUDE [Long description-1](includes/long-description-1.md)]

[!INCLUDE [Long description-2](includes/long-description-2.md)]

Use etiquetas HTML para formatear la descripción y hacer que sea más atractiva. Para obtener una lista de las etiquetas permitidas, consulte el artículo sobre las [etiquetas HTML admitidas](supported-html-tags.md).

### <a name="privacy-policy-link"></a>Vínculo de la directiva de privacidad

Escriba la dirección web (URL) de la directiva de privacidad de la organización. Asegúrese de que la oferta cumpla con las leyes y regulaciones de privacidad. También debe publicar una directiva de privacidad válida en su sitio web.

## <a name="useful-links"></a>Vínculos útiles

Proporcione documentos en línea complementarios sobre su oferta. Para agregar un vínculo, seleccione **Add a link** (Agregar un vínculo) y, a continuación, rellene los campos siguientes:

- **Name** : los clientes verán el nombre en la página de detalles.
- **Dirección URL del vínculo** : escriba un vínculo para que los clientes vean el documento en línea.

## <a name="customer-support-links"></a>Vínculos de soporte al cliente

Proporcione el sitio web de soporte técnico en el que los clientes pueden ponerse en contacto con el equipo de soporte técnico.

- Sitio web de soporte técnico global de Azure
- Sitio web de soporte técnico de Azure Government

## <a name="partner-support-contact"></a>Contacto de soporte técnico del asociado

Proporcione información de contacto para que los asociados de Microsoft la usen cuando los clientes abran una incidencia de soporte técnico. Esta información no aparece en Azure Marketplace.

- Nombre
- Email
- Teléfono

## <a name="engineering-contact"></a>Contacto de ingeniería

Proporcione información de contacto para que Microsoft la use en caso de problemas con la oferta, incluidos los problemas de certificación. Esta información no aparece en Azure Marketplace.

- Nombre
- Email
- Teléfono

## <a name="azure-marketplace-media"></a>Elementos multimedia de Azure Marketplace

Proporcione logotipos e imágenes para usarlos con la oferta. Todas las imágenes deben estar en formato PNG. Las imágenes borrosas harán que se rechace el envío.

[!INCLUDE [logo tips](includes/graphics-suggestions.md)]

>[!NOTE]
>Si tiene problemas al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

### <a name="azure-marketplace-logos"></a>Logotipos de Azure Marketplace

Proporcione un archivo PNG para el logotipo de tamaño **Grande** . El Centro de partners lo usará para crear un logotipo **Pequeño** y un logotipo **Mediano** . Opcionalmente, puede reemplazarlos por imágenes diferentes más adelante.

- **Grande** (de 216 x 216 a 350 x 350 píxeles, obligatorio)
- **Mediano** (90 x 90 píxeles, opcional)
- **Pequeño** (48 x 48 píxeles, opcional)

Estos logotipos se usan en distintos lugares de la publicación:

[!INCLUDE [logos-azure-marketplace-only](includes/logos-azure-marketplace-only.md)]

[!INCLUDE [Logo tips](includes/graphics-suggestions.md)]

### <a name="screenshots"></a>Capturas de pantalla

Agregue hasta cinco capturas de pantalla que muestren el funcionamiento de la oferta. Cada captura de pantalla debe tener un tamaño de 1280 x 720 píxeles y el formato PNG. Cada captura de pantalla debe incluir un título.

### <a name="videos"></a>Vídeos

Agregue hasta cinco vídeos que muestren la oferta. Los vídeos se deben hospedar en un servicio de vídeo externo. Escriba el nombre de cada vídeo, su dirección web y una imagen en miniatura en formato PNG del vídeo de 1280 x 720 píxeles.

Para ver más recursos sobre las listas de marketplace, consulte [Procedimientos recomendados para las listas de ofertas de marketplace](gtm-offer-listing-best-practices.md).

Seleccione **Guardar borrador** antes de continuar.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de un plan en Azure Stack Hub](azure-vm-create-plans.md)
