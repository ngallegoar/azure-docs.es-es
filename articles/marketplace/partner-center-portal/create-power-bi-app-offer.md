---
title: Creación de una oferta de aplicación de Power BI en Marketplace comercial de Microsoft
description: Obtenga información sobre cómo crear y publicar una oferta de aplicación de Power BI para Microsoft AppSource.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 17a6b7e46341ee0231ac88b849495cfc7e46ad88
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82839154"
---
# <a name="create-a-power-bi-app-for-microsoft-appsource"></a>Creación de una aplicación de Power BI para Microsoft AppSource

En este artículo se describe cómo crear y publicar una oferta de aplicación de Power BI para Microsoft[AppSource](https://appsource.microsoft.com/).

Antes de comenzar, [cree una cuenta de Marketplace comercial en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), si aún no lo ha hecho. Asegúrese de que la cuenta está inscrita en el programa Marketplace comercial.

## <a name="create-a-new-offer"></a>Crear una nueva oferta

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
2. En el menú de navegación de la izquierda, seleccione **Marketplace comercial** > **Información general**.
3. En la página de información general, seleccione **+ Nueva oferta** > **Aplicación de servicio Power BI**.

   ![Muestra el menú de navegación izquierdo.](./media/new-offer-pbi-app.png)

> [!NOTE]
> Después de publicar una oferta, las modificaciones que se realicen en el Centro de partners solo aparecen en los escaparates cuando la oferta se vuelve a publicar. Asegúrese de volver a realizar la publicación siempre después de realizar cambios.

> [!IMPORTANT]
> Si no se muestra ni está habilitada la opción **Aplicación de servicio Power BI Service App**, la cuenta no tiene permiso para crear este tipo de oferta. Compruebe que cumple todos los [requisitos](create-power-bi-app-overview.md) para este tipo de oferta, incluido el registro para una cuenta de desarrollador.

## <a name="new-offer"></a>Nueva oferta

Escriba un **Identificador de oferta**. Se trata de un identificador único para cada oferta de su cuenta.

- Se muestra a los clientes en la dirección web de la oferta de Marketplace y en las plantillas de Azure Resource Manager, si procede.
- Use solo letras minúsculas y números. Puede incluir guiones y caracteres de subrayado, pero no espacios, y está limitado a 50 caracteres. Por ejemplo, si escribe **test-offer-1**, la dirección web de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- El identificador de oferta no se puede cambiar después de seleccionar **Crear**.

Escriba un **Alias de la oferta**. Este es el nombre que se usa para la oferta en el Centro de partners.

- Este nombre no se usa en Marketplace y es diferente del nombre de la oferta y de otros valores que se muestran a los clientes.
- El alias de la oferta no se puede cambiar después de seleccionar **Crear**.

Seleccione **Crear** para generar la oferta y continuar.

## <a name="offer-overview"></a>Información general de la oferta

Esta página se muestra una representación visual de los pasos necesarios para publicar esta oferta (tanto completados como siguientes), así como el tiempo necesario que debe tardar cada uno en completarse.

Se incluyen vínculos para realizar operaciones en esta oferta en función de la selección que realice. Por ejemplo:

- Si la oferta es un borrador: [Eliminación de una oferta borrador](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Si la oferta está activa: [Detención de la venta de una oferta](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Si la oferta está en versión preliminar: [publicar](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Si no ha completado el cierre de sesión del anunciante: [Cancelación de la publicación](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>Configuración de la oferta

### <a name="connect-lead-management"></a>Conexión de administración de clientes potenciales

Al publicar la oferta en Marketplace con el Centro de partners, debe conectarla a un sistema de administración de relaciones con clientes (CRM). Esto le permite recibir información de contacto del cliente en cuanto alguien expresa interés en el producto o lo usa.

1. Seleccione un destino de clientes potenciales al que quiera que enviemos los clientes potenciales. El Centro de partners admite los siguientes sistemas CRM:

    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) for Customer Engagement
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Si su sistema CRM no aparece en la lista anterior, use [Tabla de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) o [Punto de conexión HTTPS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) para almacenar los datos del cliente potencial. A continuación, exporte los datos al sistema CRM.

2. Conecte su oferta al destino de clientes potenciales cuando la publique en el Centro de partners.
3. Compruebe que la conexión al destino de clientes potenciales está configurada correctamente. Después de publicarla en el Centro de partners, se valida la conexión y se le envía un cliente potencial de prueba. Mientras obtiene una vista previa de la oferta antes de publicarla, también puede probar la conexión de los clientes potenciales tratando de adquirir la oferta en el entorno de versión preliminar.
4. Asegúrese de que la conexión con el destino de clientes potenciales permanece actualizada para no perder ningún cliente potencial.

Estos son algunos recursos adicionales de administración de clientes potenciales:

- [Introducción a la administración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Preguntas frecuentes de la administración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Errores comunes de la configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Introducción a la administración de clientes potenciales](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) en formato PDF (asegúrese de que el bloqueador de elementos emergentes está desactivado).

Seleccione **Guardar borrador** antes de continuar.

## <a name="properties"></a>Propiedades

Esta página permite definir las categorías y los sectores que se usan para agrupar su oferta en Marketplace, la versión de la aplicación y los contratos legales que respaldan su oferta.

### <a name="category"></a>Category

Seleccione un mínimo de una y un máximo de tres categorías. Estas categorías se usan para colocar la oferta en las áreas de búsqueda adecuadas de Marketplace y se muestran en la página de detalles de la oferta. En la descripción de la oferta, indique la forma en que la oferta admite estas categorías.

### <a name="industry"></a>Sector

Opcionalmente, seleccione hasta dos sectores y segmentos verticales en cada sector. Cuando las categorías se usan para mostrar la oferta, el sector y los segmentos verticales se usan en filtros de búsqueda y se aplican en el escaparate. Si la oferta está dirigida a un sector o segmento vertical específico, use la descripción de la oferta para explicar en qué medida la oferta es compatible con los sectores o los segmentos verticales seleccionados. Si la oferta no es específica para un sector, deje esta sección en blanco.

> [!NOTE]
> A medida que trabajamos para incorporar nuevos sectores y segmentos verticales para mejorar la experiencia de descubrimiento de ofertas, es posible que algunos sectores o segmentos verticales no estén todavía visibles en el escaparate. Los sectores y los segmentos verticales marcados con un (*) estarán disponibles en una fecha futura. Todas las ofertas publicadas se pueden descubrir mediante la búsqueda de palabras clave.
<p>&nbsp;

| **Sector** | **Subsector** |
| --- | --- |
| *Automoción | *Automoción |
| Agricultura | *Otros: No segmentados |
| Distribución | *Venta al por mayor<br>Envío de paquetes |
| Educación | *Educación superior<br>* Educación primara y secundaria/K-12<br>*Bibliotecas y museos |
| Servicios financieros | *Mercados de capital y banca<br>* Seguros |
| Government | *Defensa e inteligencia (solía denominarse Seguridad pública y nacional)<br>* Justicia y seguridad pública<br>*Gobierno civil |
| Asistencia sanitaria (solía denominarse Salud) | *Pagador de seguro médico<br>* Proveedor de asistencia sanitaria<br>*Productos farmacéuticos |
| Fabricación y recursos (solía denominarse Fabricación) | *Química y agroquímica<br>* Fabricación discreta<br>*Energía |
| Venta al por menor y bienes de consumo (solía denominarse Venta al por menor) | *Bienes de consumo<br>* Minoristas |
| *Medios y comunicaciones (solía denominarse Medios de comunicación y ocio) | *Medios de comunicación y ocio<br>* Telecomunicaciones |
| Servicios profesionales | *Legal<br>* Servicios profesionales de partners |
| *Arquitectura y construcción (solía denominarse Ingeniería de la edificación) | *Otros: No segmentados |
| *Hostelería y viajes | *Hoteles y ocio<br>* Transporte y viajes<br>*Restaurantes y servicios de hostelería |
| *Otros sectores de la administración pública | *Ingeniería forestal y pecuaria<br>* ONG |
| *Bienes inmuebles | *Otros: No segmentados |

### <a name="legal"></a>Información legal

#### <a name="terms-and-conditions"></a>Términos y condiciones

Si quiere proporcionar sus propios términos y condiciones personalizados, escriba hasta 10 000 caracteres en el cuadro **Términos y condiciones**. Si los términos y condiciones requieren una descripción más larga, escriba un solo vínculo web en el que se puedan encontrar. A los clientes les aparecerá como un vínculo activo.

Los clientes deben aceptar estos términos para poder probar la oferta.

Seleccione **Guardar borrador** antes de pasar a la siguiente sección, Descripción de la oferta.

## <a name="offer-listing"></a>Descripción de la oferta

Aquí puede definir los detalles de la oferta que se muestran en Marketplace. Esto incluye el nombre de la oferta, la descripción, las imágenes, etc.

### <a name="language"></a>Idioma

Seleccione el idioma en el que se mostrará la oferta. La única opción disponible actualmente es **Inglés (Estados Unidos)** .

Tendrá que definir los detalles de Marketplace (como nombre de la oferta, descripción e imágenes) para cada idioma y mercado. Seleccione el nombre de idioma o mercado para proporcionar esta información.

> [!NOTE]
> No es necesario que los detalles de la oferta estén en inglés si la descripción de la oferta comienza con la frase "Esta aplicación solo está disponible en [idioma distinto del inglés]." También es puede proporcionar un vínculo al contenido de la oferta en un idioma distinto al utilizado en la descripción de la oferta.

### <a name="name"></a>Nombre

El nombre que se escribe aquí se muestra como el título de la oferta. Este campo se rellena previamente con el texto que escribió en el cuadro **Alias de la oferta** al crear la oferta. Puede cambiar este nombre más adelante.

El nombre:

- Puede contener marcas comerciales y, además, puede incluir símbolos de marca comercial o copyright.
- No puede tener más de 50 caracteres.
- No puede incluir emojis.

### <a name="search-results-summary"></a>Resumen de los resultados de la búsqueda

Proporcione una breve descripción de la oferta. Puede tener una longitud de hasta 100 caracteres y se usa en los resultados de la búsqueda de Marketplace.

### <a name="description"></a>Descripción

Proporcione una descripción más larga de la oferta (hasta 3000 caracteres). Se muestra a los clientes en la información general de la lista de Marketplace.

Incluya uno o varios de los siguientes elementos en la descripción:

- Valor y beneficios principales que proporciona la oferta
- Asociaciones de categoría o de sector, o ambas
- Oportunidades de compra desde la aplicación
- Cualquier divulgación necesaria

Estas son algunas sugerencias para escribir la descripción:

- Describa claramente la propuesta de valor de la oferta en las primeras frases de la descripción. Incluya los siguientes elementos:
  - Descripción de la oferta.
  - Tipo de usuario que se beneficia de la oferta.
  - Necesidades o problemas del cliente que aborda la oferta.
- Recuerde que estas primeras frases podrían mostrarse en los resultados de la búsqueda.
- No se base exclusivamente en las características y funcionalidades para vender su producto. En su lugar, céntrese en el valor que proporciona la oferta.
- Intente usar un vocabulario específico del sector o palabras relacionadas con las ventajas.

Para que la descripción de la oferta sea más atractiva, use el editor de texto enriquecido para darle formato.

![Uso del editor de texto enriquecido](./media/rich-text-editor.png)

| <center>Cambiar el formato de texto | <center>Agregar viñetas o numeración | <center>Agregar o quitar sangría del texto |
| --- | --- | --- |
| <center>![Uso del editor de texto enriquecido para cambiar el formato del texto](./media/text-editor3.png) |  <center>![Uso del editor de texto enriquecido para agregar listas](./media/text-editor4.png) |  <center>![Uso del editor de texto enriquecido para aplicar sangría](./media/text-editor5.png) |

### <a name="search-keywords"></a>Palabras clave de búsqueda

Escriba un máximo de tres palabras clave de búsqueda opcionales para ayudar a los clientes a buscar su oferta en Marketplace. Para obtener mejores resultados, intente usar también estas palabras clave en la descripción.

### <a name="helpprivacy-web-addresses"></a>Direcciones web de ayuda/privacidad

Proporcione vínculos para ayudar a los clientes a comprender mejor su oferta.

#### <a name="help-link"></a>Vínculo de ayuda

Escriba la dirección web en la que los clientes pueden obtener más información sobre su oferta.

#### <a name="privacy-policy-url"></a>Dirección URL de la directiva de privacidad

Escriba la dirección web de la directiva de privacidad de su organización. Usted es responsable de garantizar que la oferta cumpla con las leyes y los reglamentos de privacidad. También es responsable de publicar una directiva de privacidad válida en su sitio web.

### <a name="contact-information"></a>Información de contacto

Debe proporcionar el nombre, el correo electrónico y el número de teléfono de un **contacto de soporte técnico** y un **contacto de ingeniería**. Esta información no se muestra a los clientes. Está disponible para Microsoft y se puede proporcionar a los asociados del programa Proveedor de soluciones en la nube (CSP).

- Contacto de soporte técnico (se requiere): Para preguntas generales de soporte técnico.
- Contacto de ingeniería (se requiere): Para preguntas técnicas y problemas de certificación.
- Contacto del programa CSP (opcional): Para preguntas de revendedores relacionadas con el programa CSP.

En la sección **Contacto de soporte técnico**, proporcione la dirección web del **sitio web de soporte técnico** en el que los asociados pueden encontrar soporte técnico para la oferta.

### <a name="supporting-documents"></a>Documentos relacionados

Proporcione al menos uno y hasta tres documentos de marketing relacionados en formato PDF. Por ejemplo, notas del producto, folletos, listas de comprobación o presentaciones.

### <a name="marketplace-images"></a>Imágenes de Marketplace

Proporcione logotipos e imágenes para usarlos con la oferta. Todas las imágenes deben estar en formato .PNG. Las imágenes borrosas se rechazarán.

>[!NOTE]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

#### <a name="store-logos"></a>Logotipos de Store

Proporcione los archivos .png del logotipo de la oferta en dos tamaños de píxeles:
- **Pequeño (48 x 48)**
- **Grande (216 x 216)**

Los dos logotipos son necesarios y se usan en lugares diferentes de la lista de Marketplace.

#### <a name="screenshots"></a>Capturas de pantalla

Agregue al menos una y hasta cinco capturas de pantalla que muestren el funcionamiento de la oferta. Cada una debe tener un tamaño de 1280 x 720 píxeles y el formato .png.

#### <a name="videos-optional"></a>Vídeos (opcional)

Agregue hasta cinco vídeos que muestren la oferta. Escriba el nombre del vídeo, su dirección web y una imagen en miniatura en formato .png del vídeo con un tamaño de 1280 x 720 píxeles.

#### <a name="additional-marketplace-listing-resources"></a>Recursos adicionales de la descripción de Marketplace

Para más información sobre la creación de listas de ofertas, vea [Procedimientos recomendados para la publicación de ofertas](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

## <a name="technical-configuration"></a>Configuración técnica

Promocione su aplicación del servicio Power BI en producción y proporcione el vínculo del instalador de la aplicación de Power BI que permite a los clientes instalar la aplicación. Para más información, consulte [Publicación de aplicaciones con paneles e informes en Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).

## <a name="supplemental-content"></a>Contenido complementario

Proporcione información adicional sobre su oferta, lo que nos ayudará a validarla. Esta información no se muestra a los clientes ni se publica en Marketplace.

### <a name="validation-assets"></a>Recursos de validación

De manera opcional, agregue instrucciones (hasta 3000 caracteres) para ayudar al equipo de validación de Microsoft a configurar, conectar y probar la aplicación. Incluya opciones de configuración típicas, cuentas, parámetros u otra información que pueda usarse para probar la opción de conexión de datos. Esta información solo está visible para el equipo de validación y solo se usa para validación.

## <a name="review-and-publish"></a>Revisión y publicación

Una vez que haya completado todas las secciones necesarias de la oferta, puede enviar la oferta para su revisión y publicación.

En la esquina superior derecha del portal, seleccione **Revisar y publicar**.

En la página de revisión, puede:

- Ver el estado de finalización de cada sección de la oferta. No puede realizar la publicación hasta que todas las secciones de la oferta estén marcadas como completas.
  - **No iniciada**: significa que la sección no se ha iniciado y se debe completar.
  - **Incompleta**: significa que la sección tiene errores que deben corregirse o que requiere más información. Vea las secciones anteriores de este documento para obtener instrucciones.
  - **Completa**: la sección tiene todos los datos necesarios y no contiene errores. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- Proporcione instrucciones de prueba al equipo de certificación para asegurarse de que la aplicación se ha probado correctamente. Además, proporcione cualquier nota complementaria que resulte útil para entender su oferta.

Para enviar la oferta para su publicación, seleccione **Publicar**.

Le enviaremos un correo electrónico para avisarle cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Para publicar la oferta al público, o si es una oferta privada, a un público privado, vaya al Centro de partners y seleccione **Publicar**.
