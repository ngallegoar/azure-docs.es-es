---
title: Creación de una oferta de aplicaciones de Azure en marketplace comercial
description: Cómo crear una nueva oferta de aplicaciones de Azure para mostrarla o venderla en Azure Marketplace, AppSource, o bien a través del programa Proveedor de soluciones en la nube (CSP) mediante el portal de Marketplace comercial del Centro de partners de Microsoft.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 516ebce176f53a0495ea493f5327658162e7ea9e
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867282"
---
# <a name="create-an-azure-application-offer"></a>Creación de una oferta de aplicación de Azure

Los pasos para publicar una oferta de la aplicación de Azure en Marketplace comercial se describen aquí.

## <a name="azure-application-offer-type"></a>Tipo de oferta de la aplicación de Azure

En este tema se describen los aspectos básicos sobre las ofertas de la aplicación de Azure.  Debe estar familiarizado con estos conceptos antes de iniciar el proceso de publicar una nueva oferta de la aplicación de Azure en Marketplace.

### <a name="publishing-overview"></a>Introducción a la publicación

El vídeo [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Creación de plantillas de soluciones y aplicaciones administradas para Azure Marketplace) es una introducción al tipo de oferta de la aplicación de Azure:

* Qué tipos de oferta están disponibles;
* Qué recursos técnicos se requieren;
* Cómo crear una plantilla de Azure Resource Manager;
* Desarrollo y prueba de la interfaz de usuario de la aplicación;
* Cómo publicar la oferta de la aplicación;
* El proceso de revisión de la aplicación.

### <a name="types-of-azure-application-plans"></a>Tipos de planes de la aplicación de Azure

Hay dos tipos de planes de la aplicación de Azure, aplicaciones administradas y plantillas de solución.

* La **plantilla de solución** es uno de los principales mecanismos para publicar una solución en Marketplace. Este tipo de plan se usa cuando la solución requiere automatización adicional de implementación y configuración para algo más que una sola máquina virtual (VM).  Con una plantilla de solución, puede automatizar el suministro de más de un recurso, incluyendo las máquinas virtuales, las redes y los recursos de almacenamiento para proporcionar soluciones IaaS complejas.  Para más información sobre la creación de plantillas de solución, consulte la documentación de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

* La **aplicación administrada** es similar a las plantillas de solución, aunque hay una diferencia importante. En una aplicación administrada, los recursos se implementan en un grupo de recursos que está administrado por el editor de la aplicación. El grupo de recursos está presente en la suscripción del consumidor, pero una identidad en el inquilino del editor tiene acceso al grupo de recursos. Como editor, puede especificar el costo del soporte técnico continuado de la solución. Use aplicaciones administradas para crear y entregar con facilidad a los clientes aplicaciones llave en mano totalmente administradas.  Para más información sobre las ventajas y los tipos de aplicaciones administradas, consulte [Introducción a las aplicaciones administradas de Azure](https://docs.microsoft.com/azure/managed-applications/overview).

Todas las aplicaciones de Azure incluyen al menos dos archivos en la carpeta raíz de un archivo `.zip`:

* Un archivo de plantilla de Resource Manager llamado [mainTemplate.json](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).  Esta es la plantilla que define los recursos que se van a implementar en la suscripción de Azure del cliente.  Para ver ejemplos de plantillas de Resource Manager, consulte la [galería de plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/) o el repositorio [GitHub: Plantillas de inicio rápido de Azure Resource Manager](https://github.com/azure/azure-quickstart-templates) correspondiente.

* Una definición de interfaz de usuario para la experiencia de creación de aplicaciones de Azure llamada [createUiDefinition.json](https://docs.microsoft.com/azure/managed-applications/create-uidefinition-overview).  En la interfaz de usuario, puede especificar los elementos que permiten a los consumidores proporcionar los valores de los parámetros.

Todas las nuevas ofertas de la aplicación de Azure deben incluir un [GUID de atribución de uso del cliente para asociados de Azure](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution).

### <a name="before-you-begin"></a>Antes de empezar

Revise la documentación de la aplicación de Azure siguiente, que proporciona artículos de inicio rápido, tutoriales y ejemplos.

* [Nociones sobre las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
* Guías de inicio rápido:

    * [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/)
    * [Plantillas de inicio rápido de Azure en GitHub](https://github.com/azure/azure-quickstart-templates)
    * [Publicación de definición de aplicación](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Implementación de la aplicación de catálogo de servicios](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

* Tutoriales:

    * [Creación de archivos de definición](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Publicación de la aplicación Marketplace](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

* Ejemplos:

    * [CLI de Azure](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    * [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    * [Soluciones para aplicaciones administradas](https://docs.microsoft.com/azure/managed-applications/sample-projects)

### <a name="fundamentals-in-technical-knowledge"></a>Aspectos básicos de los conocimientos técnicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta.

El equipo de ingeniería debe tener conocimientos sobre las tecnologías de Microsoft siguientes:

* Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/).
* Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
* Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage#storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking#networking).
* Conocimientos prácticos de [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
* Conocimientos prácticos de [JSON](https://www.json.org/).

### <a name="suggested-tools"></a>Herramientas sugeridas

Elija uno o ambos de los entornos de scripting siguientes para ayudar a administrar la aplicación de Azure:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
* [CLI de Azure](https://docs.microsoft.com/cli/azure)

Se recomienda agregar las herramientas siguientes al entorno de desarrollo:

* [Explorador de Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Visual Studio Code](https://code.visualstudio.com/) con las siguientes extensiones:
    * Extensión: [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    * Extensión: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    * Extensión: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Puede revisar las herramientas disponibles en la página [Herramientas de desarrollo de Azure](https://azure.microsoft.com/tools/).  Asimismo, si usa Visual Studio, [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-an-azure-application-offer"></a>Creación de una oferta de aplicación de Azure

Para poder crear una oferta de la aplicación de Azure, primero debe [crear una cuenta de Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) y después abrir el [panel de marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), con la pestaña **Información general** seleccionada.

>[!Note]
>Una vez publicada una oferta, las modificaciones de esta realizadas en el Centro de partners solo se actualizarán en el sistema y en los escaparates después de volver a publicarla.  Asegúrese de enviar la oferta para su publicación después de realizar los cambios.

### <a name="create-a-new-offer"></a>Crear una nueva oferta

Seleccione el botón **+ Nueva oferta** y, a continuación, seleccione el elemento de menú **Aplicación de Azure**. Aparecerá el cuadro de diálogo **Nueva oferta**.

### <a name="offer-id-and-alias"></a>Identificador y alias de la oferta

* **Id. de oferta**: Un identificador único para cada oferta de su cuenta. Los clientes lo verán en la dirección URL de la oferta de Marketplace y en las plantillas de Azure Resource Manager (si procede). <br> <br> El identificador de la oferta debe ser alfanumérico y contener solo minúsculas (puede incluir guiones y caracteres de subrayado, pero no espacios en blanco). Tiene un límite de 50 caracteres y no se puede cambiar después de seleccionar Crear. <br> <br> Por ejemplo, si escribe `test-offer-1` aquí, la dirección URL de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`. 

* **Alias de la oferta**: nombre que se usa para hacer referencia a la oferta desde el Centro de partners. Este nombre no se usará en Marketplace y es diferente del nombre de la oferta y otros valores que se mostrarán a los clientes. Este valor no se puede cambiar después de seleccionar **Crear**.

Una vez que haya escrito el **identificador de la oferta** y el **alias de la oferta**, seleccione **Crear**. Después podrá trabajar en todas las demás partes de la oferta.

## <a name="offer-setup"></a>Configuración de la oferta

En la página **Configuración de la oferta** se solicita la siguiente información. Después de completar estos campos, asegúrese de seleccionar **Guardar**.

### <a name="test-drive"></a>Versión de prueba

Una versión de prueba es una excelente manera de presentar la oferta a clientes potenciales, ya que se les brinda la oportunidad de "probarla antes de comprarla", lo que aumenta la conversión y la generación de clientes potenciales altamente cualificados. [Más información acerca de las versiones de prueba.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Para habilitar una versión de prueba, active la casilla **Habilitar una versión de prueba**. A continuación, deberá configurar un entorno de demostración en la [configuración técnica de la versión de prueba](#types-of-azure-application-plans) para permitir que los clientes prueben su oferta durante un período de tiempo fijo. 

>[!Note]
>Dado que todas las aplicaciones de Azure se implementan mediante una plantilla de Azure Resource Manager, el único tipo de versión de prueba que se puede configurar para una aplicación de Azure es una [versión de prueba basada en Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

#### <a name="additional-test-drive-resources"></a>Recursos adicionales de una unidad de prueba

- [Procedimientos recomendados técnicos de la versión de prueba](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Procedimientos recomendados de marketing de la versión de prueba](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Póster Introducción a las versiones de prueba](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Conexión de administración de clientes potenciales

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Para obtener más información, consulte [Introducción a la administración de clientes potenciales](./commercial-marketplace-get-customer-leads.md).

No olvide **guardar** antes de pasar a la siguiente sección.

## <a name="properties"></a>Propiedades

La página de **Propiedades** le permite definir las categorías y los sectores que se usan para agrupar su oferta en Marketplace, la versión de la aplicación y los contratos legales que dan soporte a su oferta. Después de completar esta página, seleccione **Guardar**.

### <a name="category"></a>Category

Seleccione un mínimo de una y un máximo de tres categorías, que se utilizarán para colocar la oferta en las áreas de búsqueda de Marketplace adecuadas. Asegúrese de indicar la forma en que la oferta da soporte a estas categorías en la descripción de la oferta. 

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Contrato estándar para el Marketplace comercial de Microsoft

[!INCLUDE [Commercial marketplace Standard Contract](./includes/marketplace-contract-text.md)]

## <a name="offer-listing"></a>Descripción de la oferta

La página de descripción de la oferta muestra los idiomas en los que se mostrará la oferta. La única opción disponible actualmente es **Inglés (Estados Unidos)** .

Tendrá que definir los detalles de Marketplace (nombre de la oferta, descripción, imágenes, etc.) para cada idioma y mercado. Seleccione el nombre de idioma o mercado para proporcionar esta información.

> [!NOTE]
> No es necesario que el contenido del anuncio de la oferta (por ejemplo, la descripción, los documentos, las capturas de pantallas, los términos de uso, etc.) esté en inglés siempre que la descripción de la oferta empiece por la frase "Esta aplicación solo está disponible en [idioma distinto del inglés]". También es aceptable proporcionar una *dirección URL de vínculo útil* para ofrecer contenido en un idioma distinto del que se usa en el contenido del anuncio de la oferta.

### <a name="name"></a>Nombre

El nombre que escriba aquí se mostrará a los clientes como el título de la descripción de la oferta. Este campo se ha rellenado previamente con el texto introducido para **Alias de la oferta** cuando creó la oferta, pero puede cambiar este valor. Este nombre puede contener marcas comerciales (y puede incluir símbolos de marca comercial o copyright). El nombre no puede tener más de 50 caracteres ni incluir emojis.

### <a name="summary"></a>Resumen

Especifique una descripción breve de la oferta (100 caracteres como máximo), que se va a usar en los resultados de la búsqueda de Marketplace.

### <a name="long-summary"></a>Resumen largo

Proporcione una descripción más larga de la oferta (hasta 256 caracteres). La descripción se puede usar en los resultados de la búsqueda de Marketplace.

### <a name="description"></a>Descripción

Proporcione una descripción más larga de la oferta (hasta 3.000 caracteres). Esta descripción se mostrará a los clientes en la información general de la lista de Marketplace. Incluya una propuesta de valor de la oferta, los beneficios clave, las asociaciones de la categoría o sector, las oportunidades de compra en la aplicación y todas las divulgaciones necesarias. 

Algunos consejos para escribir la descripción:  

- Describa claramente la propuesta de valor de la oferta en las primeras frases de su descripción. Incluya los siguientes elementos en la propuesta de valor:
  - Descripción del producto
  - Tipo de usuario que se beneficia del producto.
  - Necesidad o preocupación del cliente que aborda el producto.
- Tenga en cuenta que estas primeras frases podrían mostrarse en los resultados de los motores de búsqueda.  
- No se base exclusivamente en las características y funcionalidades para vender su producto. En su lugar, céntrese en el valor que proporciona.  
- Use tanto como se pueda un vocabulario específico del sector o palabras relacionadas con las ventajas. 
- Considere el uso de etiquetas HTML para formatear la descripción y hacer que sea más atractiva.

### <a name="search-keywords"></a>Palabras clave de búsqueda

Opcionalmente, puede escribir un máximo de tres palabras clave de búsqueda para ayudar a los clientes a buscar su oferta en Marketplace. Para obtener mejores resultados, intente usar también estas palabras clave en la descripción.

### <a name="support-urls"></a>Direcciones URL de soporte técnico

Esta sección le permite proporcionar vínculos para ayudar a los clientes a comprender mejor su oferta.

#### <a name="privacy-policy-url"></a>Dirección URL de la directiva de privacidad

Escriba la dirección URL a la directiva de privacidad de su organización. Usted es el responsable no solo de garantizar que la aplicación cumple la normativa y la legislación en lo tocante a la privacidad, sino también de proporcionar una directiva de privacidad válida.

#### <a name="useful-links"></a>Vínculos útiles

Proporcione documentos en línea complementarios opcionales sobre su solución.  Para agregar más vínculos útiles, haga clic en **+ Agregar un vínculo**.

### <a name="contacts"></a>Contactos

En esta sección, debe proporcionar el nombre, el correo electrónico y el número de teléfono de un **contacto de soporte técnico** y un **contacto de ingeniería**. Esta información no se muestra a los clientes, pero estará disponible para Microsoft y puede proporcionarse a los asociados de CSP.

En la sección **Contacto de soporte técnico**, también debe proporcionar la **dirección URL de soporte técnico** en la que los asociados de CSP pueden encontrar soporte técnico para la oferta.

### <a name="marketplace-images"></a>Imágenes de Marketplace

En esta sección, puede proporcionar logotipos e imágenes que se usarán al mostrar la oferta al cliente. Todas las imágenes deben estar en formato .PNG.

>[!Note]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

#### <a name="store-logos"></a>Logotipos de Store

Proporcione el logotipo de la oferta en tres tamaños: **Pequeño (48 x 48)** , **Mediano (90 x 90)** y **Grande (216 x 216)** .

#### <a name="hero"></a>Imagen prominente

La imagen prominente es opcional. Si proporciona una, debe medir 815 x 290 píxeles.

#### <a name="screenshots"></a>Capturas de pantalla

Agregue capturas de pantallas que muestren el funcionamiento de la oferta. Puede agregar hasta cinco capturas de pantalla. Todas las capturas de pantallas deben tener 1280 x 720 píxeles.

#### <a name="videos"></a>Vídeos

Opcionalmente, puede agregar hasta cinco vídeos que muestren la oferta. Estos vídeos se deben hospedar en YouTube o Vimeo. En cada uno de ellos, escriba el nombre del vídeo, su dirección URL y una imagen en miniatura del vídeo (1280 x 720 píxeles).

#### <a name="additional-marketplace-listing-resources"></a>Recursos adicionales de la descripción de Marketplace

- [Procedimientos recomendados la descripción de ofertas en Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Vista previa

La pestaña **Versión preliminar** permite definir un **público preliminar** limitado para validar la oferta antes de ponerla a disposición de un público más amplio de Marketplace.

> [!IMPORTANT]
> Debe seleccionar **Publicar** para que su oferta se publique en vivo para el público de Marketplace después de comprobar que la oferta está en versión preliminar.

Los GUID de identificador de suscripción de Azure, emparejados con una descripción adicional de cada uno, identifican el público preliminar.  Los clientes no pueden ver ninguno de estos campos.

Agregue hasta 10 identificadores de suscripción de Azure manualmente, o bien hasta 100 si carga un archivo CSV.  Al agregar estas suscripciones, define el público al que se le proporcionará acceso preliminar a la oferta antes de que se publique completamente.  Aunque la oferta ya esté publicada, puede definir un público preliminar para probar los cambios o las actualizaciones que ofrece.

>[!Note]
>El público preliminar no es el mismo que un público privado. A una audiencia preliminar se le permite acceder a la oferta *antes* de que se publique en los catálogos de soluciones. También puede elegir crear un plan y ponerlo a disposición únicamente de un público privado (mediante la pestaña Disponibilidad del plan).  El público preliminar podrá ver y validar todos los planes, incluidos aquellos que estarán disponibles solo para un público privado una vez que su oferta se publique completamente en Marketplace.

## <a name="plan-overview"></a>Información general del plan

La pestaña **Información general del plan** le permite ofrecer diversas opciones de planes dentro de la misma oferta. Estos planes (conocidos como SKU en Cloud Partner Portal) pueden diferir en términos de tipo de plan (plantilla de solución frente a aplicación administrada), monetización o público.  Configure al menos un plan para mostrar su oferta en Marketplace.

Una vez creados, verá los nombres de los planes, los identificadores, la disponibilidad (pública o privada), el estado actual de la publicación y las acciones disponibles en esta pestaña.

Las **acciones** disponibles en **Información general del plan** varían en función del estado actual del plan y pueden incluir:

* Si el estado del plan es **Borrador**: eliminar borrador.
* Si el estado del plan es **Activo**: detener venta del plan o sincronizar público privado.

### <a name="create-new-plan"></a>Creación de un nuevo plan

***Identificador de plan***: cree un identificador de plan único para cada plan de esta oferta. Este identificador será visible para los clientes en la dirección URL del producto.  Use solo caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. El identificador de este plan no puede tener más de 50 caracteres. Este identificador no se puede modificar después de seleccionar que se cree.

***Nombre del plan***: los clientes verán este nombre al decidir qué plan van a seleccionar en su oferta. Cree un nombre de oferta único para cada uno de los planes de esta oferta. El nombre del plan se usa para diferenciar los planes de software que pueden formar parte de la misma oferta (p. ej. Nombre de oferta: Windows Server; planes: Windows Server 2016, Windows Server 2019).

### <a name="plan-setup"></a>Configuración del plan

La pestaña **Configuración del plan** permite establecer la configuración de alto nivel para el tipo de plan, aunque reutilice paquetes de otro plan, así como las nubes donde debe estar disponible el plan.  Sus respuestas de esta pestaña afectarán a los campos mostrados en otras pestañas para el mismo plan.

#### <a name="plan-type"></a>Tipo de plan

Tal como se describe en los [tipos de planes de la aplicación de Azure](#types-of-azure-application-plans), seleccione si su plan incluirá una plantilla de solución o una aplicación administrada.

#### <a name="this-plan-reuses-packages"></a>Este plan reutiliza paquetes

Si tiene más de un plan del mismo tipo y los paquetes son idénticos entre ellos, puede seleccionar **this plan reuses packages from another plan** (este plan reutiliza paquetes de otro plan).  Al seleccionar esta opción, podrá seleccionar uno de los otros planes del mismo tipo del que esta oferta pueda reutilizar paquetes. 

>[!Note]
>Cuando reutilice paquetes de otro plan, toda la pestaña Configuración técnica desaparecerá de este plan.  Los detalles de configuración técnica del otro plan, incluidas las actualizaciones que realice en el futuro, se usarán también para este plan. <br> <br> Asimismo, esta configuración no puede modificarse una vez publicado este plan.

#### <a name="cloud-availability"></a>Disponibilidad en la nube

Este plan debe estar disponible al menos en una nube. 

Seleccione la opción **Azure público** para que su solución se pueda implementar en los clientes de todas las regiones públicas de Azure que tengan la integración de Marketplace.  Obtenga más información sobre la [disponibilidad geográfica](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Seleccione la opción **Nube de Azure Government** para que su solución se pueda implementar en la [nube de Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome), una nube de la comunidad gubernamental con acceso controlado para los clientes de asociados tribales, locales, estatales y federales de Estados Unidos y asociados aptos para abastecer a tales entidades.  Como publicador, es responsable de los controles de cumplimiento, las medidas de seguridad y los procedimientos recomendados para atender a esta comunidad en la nube.  Azure Government usa redes y centros de datos aislados físicamente (ubicados solo en Estados Unidos).  Antes de publicar en [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), Microsoft recomienda probar y validar la solución en el entorno, ya que algunos puntos de conexión pueden ser diferentes. Para almacenar y probar la solución, solicite una cuenta de prueba en este [vínculo](https://azure.microsoft.com/global-infrastructure/government/request/).

>[!Note]
>Una vez que se publica un plan como disponible en una nube específica, no se puede quitar esa nube.

**Azure Government Cloud certifications** (Certificaciones de la nube de Azure Government)

Esta opción es visible únicamente si **Nube de Azure Government** está seleccionada en **Disponibilidad en la nube**.

Los servicios de Azure Government controlan datos que están sujetos a determinados reglamentos y requisitos gubernamentales, como FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 y CJIS.  Para dar a conocer sus certificaciones de estos programas, puede proporcionar hasta 100 vínculos donde se describen estas.  Estos vínculos pueden ser vínculos a su anuncio en el programa directamente o a descripciones de su cumplimiento de estos en sus propios sitios web.  Solo los clientes de la nube de Azure Government podrán ver estos vínculos.

## <a name="plan-listing"></a>Lista del plan

En la pestaña **Lista del plan** se muestra la información de la lista específica del plan que puede diferir entre distintos planes de la misma oferta.

### <a name="name"></a>Nombre

Se rellena previamente en función del nombre que asignó al plan cuando lo creó.  Este nombre aparecerá como el título de este "plan de software" que se muestra en Marketplace.  Puede contener un máximo de 100 caracteres.

### <a name="summary"></a>Resumen

Proporcione un breve resumen del plan de software.  Puede contener un máximo de 100 caracteres.

### <a name="description"></a>Descripción

Esta descripción es una oportunidad para explicar lo que hace que este plan de software sea único y las diferencias con respecto a otros planes de software de su oferta. Puede contener un máximo de 2000 caracteres.

Después de completar estos campos seleccione **Guardar**.

## <a name="availability"></a>Disponibilidad

La pestaña **Disponibilidad** solo es visible para los planes de plantilla de solución.  Puede hacer que el plan sea visible para todo el mundo, solo para clientes específicos (un público privado) y ocultarlo para que únicamente lo usen otras plantillas de solución o aplicaciones administradas.

### <a name="plan-audience"></a>Público del plan

Tiene la opción de configurar cada plan para que lo pueda ver todo el mundo o solo un público concreto que elija. Para asignar la pertenencia a este público utilice los identificadores de suscripción de Azure.

**Privacidad / Este plan es privado** (casilla opcional): active esta casilla para hacer que su plan sea privado y visible solo para el público restringido que elija. Una vez que se haya publicado como un plan privado, es posible actualizar el público o elegir que el plan esté disponible para todos los usuarios. Una vez que se publica un plan como visible para todos los usuarios, debe permanecer visible para todos (el plan no se puede volver a configurar como un plan privado).

**Público restringido (identificadores de suscripción de Azure)** : asigne el público que tendrá acceso a este plan privado. El acceso se asigna mediante identificadores de suscripción de Azure y existe la opción de incluir una descripción de cada identificador de suscripción de Azure asignado. Se pueden agregar un máximo de 10 identificadores de suscripción, o 20 000 identificadores de suscripción de clientes si se importa un archivo de hoja de cálculo .csv.  Los identificadores de suscripción de Azure se representan como GUID y las letras deben estar en minúsculas.

>[!Note]
>El público privado (o público restringido) no es el mismo que el público preliminar que definió en la pestaña [**Versión preliminar**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview).  El público preliminar puede acceder a su oferta *antes de* que esta se publique en Marketplace. Aunque la designación de público privado solo se aplica a un plan concreto, el público preliminar puede ver todos los planes (privados o no) con fines de validación.

### <a name="hide-plan"></a>Ocultar plan

Si su plantilla de solución está diseñada para implementarse solo indirectamente cuando se hace referencia a ella a través de otra plantilla de solución o aplicación administrada, active esta casilla para publicar su plantilla de solución, pero ocúltela para los clientes que la buscan directamente.

## <a name="pricing-and-availability"></a>Precios y disponibilidad

La pestaña **Precios y disponibilidad** solo es visible para los planes de la aplicación administrada.  Puede configurar los mercados en los que este plan estará disponible, el precio mensual de la administración de la solución y si desea que el plan sea visible para todo el mundo o solo para clientes específicos (un público privado).

### <a name="markets"></a>Mercados

Todos los planes debe estar disponible al menos en un mercado. Seleccione la casilla de la ubicación del mercado en la que desea que este plan esté disponible. Se incluyen un cuadro de búsqueda y un botón para seleccionar los países con "Impuesto remitido", en los que Microsoft remite de los impuestos de uso y ventas en su nombre, que sirven de ayuda.

Si ya se ha establecido los precios del plan en dólares de Estados Unidos (USD) y agrega otra ubicación del mercado, el precio del nuevo mercado se calculará en función de los tipos de cambio actuales. Revise siempre el precio de cada mercado antes de la publicación. Los precios se pueden consultar mediante el vínculo "Exportar precios (xlsx)" después de guardar los cambios.

### <a name="pricing"></a>Precios

Proporcione el precio mensual de este plan.  El precio se suma a cualquier infraestructura de Azure o costos de software de pago por uso en los que incurren los recursos implementados por esta solución.

Los precios establecidos en USD (USD = dólares de Estados Unidos) se convierten en la moneda local de todos los mercados seleccionados con los tipos de cambio actuales en el momento de guardarlos. Valide estos precios antes de realizar la publicación. Para ello, debe exportar la hoja de cálculo de precios y examinar el precio de cada mercado. Si desea establecer precios personalizados en un mercado individual, modifique e importe la hoja de cálculo de precios. 

>[!Note]
>Primero debe guardar los cambios de precios para habilitar la exportación de los datos de precios.

Revise los precios cuidadosamente antes de la publicación, ya que hay algunas restricciones en lo que se puede cambiar una vez publicado un plan.  

>[!Note]
>Una vez que se publica un precio para un mercado de su plan, no puede cambiarse más adelante.

### <a name="plan-audience"></a>Público del plan

Tiene la opción de configurar cada plan para que lo pueda ver todo el mundo o solo un público concreto que elija. Para asignar la pertenencia a este público utilice los identificadores de suscripción de Azure.

**Privacidad / Este plan es privado** (casilla opcional): active esta casilla para hacer que su plan sea privado y visible solo para el público restringido que elija. Una vez que se haya publicado como un plan privado, es posible actualizar el público o elegir que el plan esté disponible para todos los usuarios. Una vez que se publica un plan como visible para todos los usuarios, debe permanecer visible para todos (el plan no se puede volver a configurar como un plan privado).

**Público restringido (identificadores de suscripción de Azure)** : asigne el público que tendrá acceso a este plan privado. El acceso se asigna mediante identificadores de suscripción de Azure y existe la opción de incluir una descripción de cada identificador de suscripción de Azure asignado. Se pueden agregar un máximo de 10 identificadores de suscripción, o 20 000 identificadores de suscripción de clientes si se importa un archivo de hoja de cálculo .csv.  Los identificadores de suscripción de Azure se representan como GUID y las letras deben estar en minúsculas.

>[!Note]
>El público privado (o público restringido) no es el mismo que el público preliminar que definió en la pestaña [**Versión preliminar**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview).  El público preliminar puede acceder a su oferta *antes de* que esta se publique en Marketplace. Aunque la designación de público privado solo se aplica a un plan concreto, el público preliminar puede ver todos los planes (privados o no) con fines de validación.

## <a name="technical-configuration"></a>Configuración técnica 

La pestaña **Configuración técnica** permite cargar el paquete de implementación que permitirá a los clientes implementar el plan.

>[!Note]
>No se verá esta pestaña si configuró este plan para reutilizar paquetes de otro plan en la pestaña **Configuración del plan**.

### <a name="package-details"></a>Detalles del paquete

La subpestaña **Detalles del paquete** permite editar la versión de borrador de su configuración técnica.

***Versión***: asigne la versión actual de la configuración técnica.  Incremente esta versión cada vez que publique un cambio en esta página. La versión debe tener el formato `{integer}.{integer}.{integer}`.

***Archivo de paquete*** (`.zip`): este paquete contiene todos los archivos de plantilla necesarios para este plan, así como recursos adicionales, empaquetados como archivo `.zip`.

Todos los paquetes del plan de la aplicación de Azure deben incluir estos dos archivos en la carpeta raíz de un archivo `.zip`:

* Un archivo de plantilla de Resource Manager llamado [mainTemplate.json](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).  Esta plantilla automatiza la implementación de recursos en la suscripción de Azure de clientes.  Para ver ejemplos de plantillas de Resource Manager, consulte la [galería de plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/) o el repositorio [GitHub: Plantillas de inicio rápido de Azure Resource Manager](https://github.com/azure/azure-quickstart-templates) correspondiente.

* Una definición de interfaz de usuario para la experiencia de creación de aplicaciones de Azure llamada [createUiDefinition.json](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

Los tamaños de archivo máximos admitidos son:

* Hasta 1 GB en el tamaño del archivo `.zip` comprimido total.

* Hasta 1 GB para cada archivo individual descomprimido dentro del archivo `.zip`.  

Todas las nuevas ofertas de la aplicación de Azure deben incluir un GUID de [atribución de uso del cliente para asociados de Azure](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution).

>[!Note]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

### <a name="previously-published-packages"></a>Paquetes publicados anteriormente 

La subpestaña **Paquetes publicados anteriormente** permite ver todas las versiones publicadas de la configuración técnica.

## <a name="technical-configuration-managed-application-plans-only"></a>Configuración técnica (solo planes de la aplicación administrada)

Los planes de la aplicación administrada presentan una complejidad adicional en la pestaña **Configuración técnica** más allá de los campos **Versión** y **Archivo de paquete** descritos anteriormente. 

### <a name="enable-just-in-time-jit-access"></a>Habilitar acceso Just-In-Time (JIT)

Seleccione esta opción para habilitar acceso Just-In-Time (JIT) para este plan.  El acceso JIT permite solicitar acceso con privilegios elevados a recursos de la aplicación administrada con fines de solución de problemas o mantenimiento. Siempre tendrá acceso de solo lectura a los recursos, pero puede tener mayor acceso durante un período de tiempo específico.  Para obtener más información, consulte [Habilitación y solicitud de acceso Just-In-Time para Azure Managed Applications](https://docs.microsoft.com/azure/managed-applications/request-just-in-time-access).  Para exigir que los consumidores de la aplicación administrada concedan el acceso permanente a la cuenta, deje esta opción desactivada.

>[!Note]
>Asegúrese de actualizar su archivo `createUiDefinition.json` para admitir esta característica.  

### <a name="deployment-mode"></a>Modo de implementación

Seleccione si desea configurar el **modo de implementación incremental** o **completo** al implementar este plan: 

* En el **modo completo**, una reimplementación de la aplicación del cliente provocará la eliminación de recursos en el grupo de recursos administrados si los recursos no se definen en `mainTemplate.json`. 
* En el **modo incremental**, una reimplementación de la aplicación deja los recursos existentes sin cambios.

Para obtener más información sobre los modos de implementación, consulte [Modos de implementación de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/deployment-modes).

### <a name="notification-endpoint-url"></a>Dirección URL del punto de conexión de notificaciones

Especifique un punto de conexión de webhook de HTTPS para recibir notificaciones sobre todas las operaciones CRUD que están en las instancias de aplicación administradas de esta versión del plan.

### <a name="customize-allowed-customer-actions"></a>Personalizar acciones de cliente permitidas

Seleccione esta opción para especificar qué acciones pueden realizar los clientes en los recursos administrados, además de las acciones "`*/read`" que están disponibles de forma predeterminada. 

Enumere las acciones adicionales que desea permitir a su cliente realizar aquí, separadas por signos de punto y coma.  Para más información, consulte [Descripción de las asignaciones de denegación para recursos de Azure](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments).  Puede consultar las acciones disponibles en [Operaciones del proveedor de recursos de Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations). Por ejemplo, para permitir que los consumidores reinicien las máquinas virtuales, agregue `Microsoft.Compute/virtualMachines/restart/action` a las acciones permitidas.

### <a name="global-azure--azure-government-cloud"></a>Azure global o nube de Azure Government

Indique quién debe tener acceso de administración a esta aplicación administrada en cada nube compatible.  Los usuarios, grupos o aplicaciones a los que desea conceder permiso para el grupo de recursos administrados se identifican mediante identidades de Azure Active Directory (AAD).

***Identificador de inquilino de Azure Active Directory***: el identificador de inquilino de AAD (también conocido como id. de directorio) que contiene las identidades de los usuarios, grupos o aplicaciones a los que desea conceder permisos.  Encontrará su identificador de inquilino de AAD en la página [Propiedades de Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties), en Azure Portal.

***Autorizaciones***: agregue el identificador de objeto de Azure Active Directory del usuario, grupo o aplicación a los que desea conceder permiso para el grupo de recursos administrados. Identifique al usuario por su identificador de entidad de seguridad, que encontrará en la [hoja Usuarios de Azure Active Directory, en Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers).

Para cada entidad de seguridad, seleccione uno de los roles integrados de Azure AD de la lista (Propietario o Colaborador). El rol que seleccione describirá los permisos que tendrá la entidad de seguridad en los recursos de la suscripción del cliente. Para más información, consulte [Roles integrados en los recursos de Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).  Para obtener más información sobre el control de acceso basado en rol (RBAC), consulte [Introducción al control de acceso basado en rol en Azure Portal](https://docs.microsoft.com/azure/role-based-access-control/overview).

>[!Note]
>Aunque puede agregar hasta 100 autorizaciones por nube, suele ser más fácil crear un grupo de usuarios de Active Directory y especificar su id. en "ID de la entidad de seguridad".  Esto le permitirá agregar más usuarios al grupo de administración una vez implementado el plan y reducir la necesidad de actualizar el plan solo para agregar más autorizaciones.

### <a name="policy-settings"></a>Configuración de directiva

Aplique [directivas de Azure](https://docs.microsoft.com/azure/governance/policy/overview) a la aplicación administrada para especificar los requisitos de cumplimiento para la solución implementada.  Para las definiciones de directiva y el formato de los valores de parámetro, consulte [ejemplos de Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/index).  Puede configurar un máximo de cinco directivas y solo una instancia de cada opción Directivas.  Algunas directivas requieren parámetros adicionales.  La SKU Estándar es obligatoria para las directivas de auditoría.  El nombre de la directiva está limitado a 50 caracteres.

## <a name="co-sell"></a>Venta conjunta

Proporcionar información en la pestaña de venta conjunta es totalmente opcional para publicar su oferta. Es necesario para lograr los estados Co-sell Ready e IP Co-sell Ready. Los equipos de ventas de Microsoft usarán la información que proporciona para obtener más información sobre su solución al evaluar su idoneidad para las necesidades de los clientes. No está disponible directamente para los clientes.

Para obtener más información sobre cómo completar esta pestaña, consulte [Opción de venta conjunta en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-co-sell).

## <a name="test-drive"></a>Versión de prueba

La pestaña **Versión de prueba** permite configurar una demostración (o "versión limitada de prueba") que permitirá a los clientes probar su oferta antes de comprarla. Puede encontrar más información al respecto en el artículo [¿Qué es la versión de prueba?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)  Si ya no desea ofrecer una versión de prueba de la oferta, vuelva a la página **Configuración de la oferta** y desactive **Habilitar versión de prueba**.

### <a name="technical-configuration"></a>Configuración técnica

Las aplicaciones de Azure usan intrínsecamente el tipo de versión de prueba de Azure Resource Manager.  Consulte [Configuración técnica para la versión de prueba de Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-customer-engagement-offer#technical-configuration-for-azure-resource-manager-test-drive) para obtener más información.

### <a name="deployment-subscription-details"></a>Detalles de suscripción de implementación

Para implementar la versión de prueba en su nombre, cree y proporcione una suscripción de Azure independiente y única. (no es necesario para las versiones de prueba de Power BI).

- **Id. de suscripción de Azure** (necesario para Azure Resource Manager y Logic Apps): Escriba el identificador de la suscripción para conceder acceso a los servicios de la cuenta de Azure para la generación de informes y facturación del uso de los recursos. Se recomienda considerar la posibilidad de [crear una suscripción de Azure independiente](https://docs.microsoft.com/azure/billing/billing-create-subscription) que se use para las versiones de prueba, en caso de que no se tenga ninguna. Para buscar su identificador de la suscripción de Azure inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a la pestaña **Suscripciones** del menú izquierdo. Al seleccionar la pestaña se mostrará el identificador de la suscripción (por ejemplo, "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **Identificador de inquilino de Azure AD** (se requiere): Escriba el [id. de inquilino](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) de Azure Active Directory (AD). Para buscar dicho identificador, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Azure Active Directory del menú de la izquierda, seleccione **Propiedades y busque el número de **identificador de directorio** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). El id. de inquilino de su organización también lo puede buscar mediante la dirección URL de su nombre de dominio en: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Nombre de inquilino de Azure AD** (se requiere para Dynamic 365): Escriba el nombre de Azure Active Directory (AD). Para buscar este nombre, inicie sesión en [Azure Portal](https://portal.azure.com/) y en la esquina superior derecha se mostrará su nombre del inquilino, debajo de su nombre de cuenta.

- **Identificador de aplicación de Azure AD** (se requiere): Escriba el [identificador de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in). Para buscarlo, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Active Directory del menú de la izquierda, seleccione **Registros de aplicaciones** y busque el número de **Id. de aplicación** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Secreto de cliente de aplicación de Azure AD** (se requiere): Escriba el [secreto de cliente](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)de su aplicación de Azure AD. Para encontrar este valor, inicie sesión en [Azure Portal](https://portal.azure.com/). Seleccione la pestaña **Azure Active Directory** en el menú izquierdo, seleccione **Registros de aplicaciones** y, después, seleccione la versión de prueba de su aplicación. A continuación, seleccione **Certificados y secretos**, seleccione **Nuevo secreto de cliente**, escriba una descripción, seleccione **Nunca** en **Expira** y, después, elija **Agregar**. No olvide copiar el valor. (no abandone la página sin haber copiado el valor, ya que, de lo contrario no tendrá acceso al mismo).

No olvide **guardar** antes de pasar a la siguiente sección.

### <a name="test-drive-listings-optional"></a>Anuncios de versión de prueba (opcional)

La opción **Anuncios de versión de prueba** de la pestaña **Versión de prueba** muestra los idiomas (y los mercados) en los que la versión de prueba; actualmente el inglés (Estados Unidos) es la única ubicación disponible. Además, esta página muestra el estado de la descripción específica del idioma y la fecha y hora en que se agregó. Deberá definir los detalles de unidad de prueba (descripción, manual del usuario, vídeos, etc.) para cada idioma o el mercado.

- **Descripción** (se requiere): se describe la versión de prueba, lo que se demostrará, los objetivos con los que experimentará el usuario, las características que se van a explorar y cualquier información relevante que ayude al usuario a determinar si debe adquirir la oferta. En este campo se puede escribir hasta 3000 caracteres de texto. 

- **Información de acceso** (se requiere para las versiones de prueba de Azure Resource Manager y Logic): se explica todo lo que los clientes necesitan saber para acceder a esta versión de prueba y usarla. se muestra un escenario para usar la oferta y exactamente lo que el cliente debe saber para acceder a las características en la versión de prueba. En este campo se puede escribir hasta 10 000 caracteres de texto.

- **Manual del usuario** (se requiere): un tutorial detallado de la experiencia con la versión de prueba. Debe abarcar exactamente lo que desee que el cliente aprenda al usar la versión de prueba y puede servir como referencia para las preguntas que puedan surgirle. El archivo debe estar en formato PDF y hay que asignarle un nombre (255 caracteres como máximo) después de cargarlo.

- **Vídeos: agregar vídeos** (opcional): se pueden cargar vídeos en YouTube o Vimeo y hacer referencia a ellos aquí con un vínculo o una imagen en miniatura (533 × 324 píxeles) para que los clientes puedan ver toda la información que pueda ayudarles a conocer mejor la versión de prueba, incluida la forma de usar correctamente las características de la oferta y comprender los escenarios que destacan sus ventajas.
  - **Nombre** (se requiere)
  - **URL (solo YouTube o Vimeo)** (se requiere)
  - **Miniatura (533 x 324 px)** : El archivo de imagen debe estar en formato PNG.

Después de completar estos campos seleccione **Guardar**.

## <a name="publish"></a>Publicar

### <a name="submit-offer-to-preview"></a>Envío de la oferta para tener una versión preliminar

Una vez que haya completado todas las secciones necesarias de la oferta, seleccione **Publicar** en la esquina superior derecha del portal. Se le redirigirá a la página **Review and publish** (Revisar y publicar). 

Si es la primera vez que publica esta oferta, puede:

- Ver el estado de finalización de cada sección de la oferta.
    - *No iniciada*: significa que la sección no se ha modificado y se debe completar.
    - *Incompleta*: significa que la sección tiene errores que deben corregirse o que requiere más información. Por favor, vuelva a las secciones y actualícelas.
    - *Completa*: significa que la sección está completa, que se han proporcionado todos los datos necesarios y que no hay ningún error. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- Envíe instrucciones para la realización de pruebas al equipo de certificación para asegurarse de que la aplicación se prueba correctamente, además de todas las notas adicionales que pueden resultar útiles para conocer la aplicación.
- Enviar la oferta para su publicación, para lo que debe seleccionar **Enviar**. Le enviaremos un correo electrónico que le avise cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Vuelva al Centro de partners y seleccionar **Transmitir** la oferta para publicar la oferta para el público (o si es una oferta privada, al público privado).

### <a name="errors-and-review-feedback"></a>Errores y comentarios de revisión

El paso de **validación manual** del proceso de publicación representa una revisión extensiva de la oferta y sus recursos técnicos asociados (especialmente la plantilla de Azure Resource Manager), los problemas normalmente aparecen como vínculos de solicitud de incorporación de cambios. Para obtener una explicación sobre cómo ver y responder a estas solicitudes de incorporación de cambios, consulte [Administración de la revisión de comentarios](./azure-apps-review-feedback.md).

Si encontró errores en uno o varios de los pasos de la publicación, debe corregirlos y volver a publicar su oferta.

## <a name="next-steps"></a>Pasos siguientes

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
