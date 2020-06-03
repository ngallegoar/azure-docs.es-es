---
title: Creación de una oferta de Dynamics 365 for Operations en el portal de Marketplace comercial
description: Cómo crear una nueva oferta de Dynamics 365 for Operations para mostrarla o venderla en Azure Marketplace, AppSource, o bien a través del programa Proveedor de soluciones en la nube (CSP) mediante el portal de Marketplace comercial del Centro de partners de Microsoft.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 13e05a8771be162ebe37cc79fc93cfa404183d1d
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83846842"
---
# <a name="create-a-dynamics-365-for-operations-offer"></a>Creación de una oferta de Dynamics 365 for Operations

En este tema se explica cómo crear una nueva oferta de Dynamics 365 for Operations. [Microsoft Dynamics 365 for Finance and Operations](https://dynamics.microsoft.com/finance-and-operations) es un servicio de planeación de recursos empresariales (ERP) que admite finanzas avanzadas, operaciones, fabricación y administración de la cadena de suministro. Todas las ofertas de Dynamics 365 for Operations deben pasar por nuestro proceso de certificación.

Antes de comenzar, [cree una cuenta de Marketplace comercial en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), si aún no lo ha hecho. Asegúrese de que su cuenta está inscrita en el programa marketplace comercial.

>[!NOTE]
> Una vez publicada una oferta, las modificaciones de esta realizadas en el Centro de partners solo se actualizarán en el sistema y en los escaparates después de volver a publicarla. Asegúrese de enviar la oferta para su publicación después de realizar los cambios.

## <a name="create-a-new-offer"></a>Crear una nueva oferta

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
2. En el menú de navegación, en la izquierda, seleccione **Marketplace comercial** > **Información general**.
3. En la página de Información general, seleccione **+ Nueva oferta** > **Dynamics 365 for Operations**.

    ![Muestra el menú de navegación izquierdo.](./media/new-offer-dynamics-365-ops.png)

> [!NOTE]
> Después de publicar una oferta, las modificaciones que se realicen en el Centro de partners solo aparecen en los escaparates cuando la oferta se vuelve a publicar. Asegúrese de volver a realizar la publicación siempre después de realizar cambios.

## <a name="new-offer"></a>Nueva oferta

Escriba un **Identificador de oferta**. Se trata de un identificador único para cada oferta de su cuenta.

- Se muestra a los clientes en la dirección web de la oferta de Marketplace y en las plantillas de Azure Resource Manager, si procede.
- Use solo letras minúsculas y números. Puede incluir guiones y caracteres de subrayado, pero no espacios, y está limitado a 50 caracteres. Por ejemplo, si escribe **test-offer-1**, la dirección web de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- El identificador de oferta no se puede cambiar después de seleccionar **Crear**.

Escriba un **Alias de la oferta**. Este es el nombre que se usa para la oferta en el Centro de partners.

- Este nombre no se usa en Marketplace y es diferente del nombre de la oferta y de otros valores que se muestran a los clientes.

Seleccione **Crear** para generar la oferta y continuar.

## <a name="offer-setup"></a>Configuración de la oferta

### <a name="how-do-you-want-potential-customers-to-interact-with-this-listing-offer"></a>¿Cómo quiere que los clientes potenciales interactúen con esta oferta en la lista?

Seleccione la opción que quiere usar para esta oferta.

#### <a name="get-it-now-free"></a>Obténgalo ahora (de forma gratuita)

Presente su oferta a los clientes de forma gratuita proporcionándoles una dirección URL válida (que comienza por *http* o *https*) desde donde pueden acceder a su aplicación.  Por ejemplo: `https://contoso.com/my-app`

#### <a name="free-trial-listing"></a>Evaluación gratuita (anuncio)

Presente su oferta a los clientes con un vínculo a una evaluación gratuita proporcionándoles una dirección URL válida (que empiece por *http* o *https*) desde la que puedan obtener una evaluación. Por ejemplo, `https://contoso.com/trial/my-app`. Las ofertas que incluyen evaluaciones gratuitas se crean, administran y configuran desde su servicio y no tienen suscripciones administradas por Microsoft.

> [!NOTE]
> Los tokens que la aplicación recibirá a través del vínculo de evaluación gratuita solo se pueden usar para obtener información de usuario mediante Azure Active Directory (Azure AD) para automatizar la creación de cuentas en la aplicación. No se admiten las cuentas de Microsoft para la autenticación con este token.

#### <a name="contact-me"></a>Ponerse en contacto conmigo

Recopile información de contacto de los clientes mediante la conexión de su sistema de Administración de relaciones con los clientes (CRM). El cliente le pedirá permiso para compartir su información. Estos datos del cliente, junto con el nombre e identificador de la oferta y el origen de Marketplace donde la ha encontrado, se envían al sistema CRM que se ha configurado. Para obtener más información acerca de cómo configurar el sistema CRM, vea [Clientes potenciales](#customer-leads).

### <a name="test-drive"></a>Versión de prueba

Una versión de prueba es una excelente manera de presentar la oferta a clientes potenciales, ya que se les brinda la oportunidad de «probar antes de comprar», lo que aumenta la conversión y la generación de clientes potenciales altamente cualificados. [Más información sobre las versiones de prueba](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Para habilitar una versión de prueba durante un período de tiempo determinado, seleccione la casilla **Habilitar una versión de prueba**. Para eliminar la versión de prueba de la oferta, desactive esta casilla. Configure el entorno de la versión de prueba en la sección [Configuración técnica de la versión de prueba](#test-drive-technical-configuration) más adelante en este tema.

Para más información, consulte [Versión de prueba de la oferta en el Marketplace comercial](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive).

#### <a name="type-of-test-drive"></a>Tipo de versión de prueba

Seleccione entre las siguientes opciones:

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Una plantilla de implementación que contiene todos los recursos de Azure que componen la solución. Los productos que se ajustan a este escenario usan solo recursos de Azure.
- **[Dynamics 365 for Business Central](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft hospeda y mantiene el servicio de la unidad de prueba (lo que incluye el aprovisionamiento y la implementación) del sistema de planeamiento de recursos empresariales Business Central (finanzas, operaciones, cadena de suministros, CRM, etc.).  
- **[Dynamics 365 for Customer Engagement](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft hospeda y mantiene el servicio de la versión de prueba (incluidos aprovisionamiento e implementación) de un sistema de Customer Engagement (ventas, servicio, servicio de proyecto, servicio de campo, etc.).  
- **[Dynamics 365 for Operations](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft hospeda y mantiene el servicio de la unidad de prueba (incluye el aprovisionamiento y la implementación) del sistema de planeamiento de recursos empresariales Finance and Operations (finanzas, operaciones, fabricación, cadena de suministros, etc.).
- **[Aplicación lógica](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : una plantilla de implementación que abarca todas las arquitecturas de soluciones complejas. Todos los productos personalizados deben usar este tipo de versión de prueba.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Un vínculo insertado en un panel personalizado. Los productos que deseen realizar una demostración de un objeto visual interactivo de Power BI deben usar este tipo de versión de prueba. Todo lo que necesita para la carga aquí es la dirección URL insertada de Power BI.

#### <a name="additional-test-drive-resources"></a>Recursos adicionales de una unidad de prueba

- [Procedimientos recomendados de marketing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Procedimientos técnicos recomendados](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Información general](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF, asegúrese de que el bloqueador de elementos emergentes está desactivado)

### <a name="customer-leads"></a>Clientes potenciales

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Para obtener más información, consulte [Introducción a la administración de clientes potenciales](./commercial-marketplace-get-customer-leads.md).

Seleccione **Guardar borrador** antes de continuar.

## <a name="properties"></a>Propiedades

Esta página le permite definir las categorías y los sectores que se usan para agrupar su oferta en el marketplace, la versión de la aplicación y los contratos legales que dan soporte a su oferta.

### <a name="category"></a>Category

Seleccione un mínimo de una y un máximo de tres categorías. Estas categorías se usarán para colocar la oferta en las áreas de búsqueda adecuadas de Marketplace. Asegúrese de indicar la forma en que la oferta da soporte a estas categorías en la descripción de la oferta.

### <a name="industry"></a>Sector

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>Versión de la aplicación

Escriba el número de versión de la oferta. Los clientes verán esta versión en la página de detalles de la oferta.

### <a name="terms-and-conditions"></a>Términos y condiciones

Proporcione sus propios términos y condiciones legales en el campo **Términos y condiciones**. También puede proporcionar la dirección URL donde se pueden encontrar los términos y condiciones. Los clientes deberán aceptar estos términos para poder probar la oferta.

Seleccione **Guardar borrador** antes de continuar.

## <a name="offer-listing"></a>Descripción de la oferta

La página muestra los idiomas en los que se mostrará la oferta. La única opción disponible actualmente es **Inglés (Estados Unidos)** .

Tendrá que definir los detalles de Marketplace (nombre de la oferta, descripción, imágenes, etc.) para cada idioma y mercado. Seleccione el nombre de idioma o mercado para proporcionar esta información.

> [!NOTE]
> No es necesario que el contenido del anuncio de la oferta (por ejemplo, la descripción, los documentos, las capturas de pantallas, los términos de uso, etc.) esté en inglés siempre que la descripción de la oferta empiece por la frase "Esta aplicación solo está disponible en [idioma distinto del inglés]". También es aceptable proporcionar una *dirección URL de vínculo útil* para ofrecer contenido en un idioma distinto del que se usa en el contenido del anuncio de la oferta.

### <a name="name"></a>Nombre

El nombre que escriba aquí se mostrará a los clientes como el título de la descripción de la oferta. Este campo se ha rellenado previamente con el texto introducido para **Alias de la oferta** cuando creó la oferta, pero puede cambiar este valor. Este nombre puede contener marcas comerciales (y puede incluir símbolos de marca comercial o copyright). El nombre no puede tener más de 50 caracteres ni incluir emojis.

### <a name="short-description"></a>Descripción breve

Proporcione una breve descripción de la oferta (hasta 100 caracteres). Esta descripción se puede usar en los resultados de la búsqueda de Marketplace.

### <a name="description"></a>Descripción

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

### <a name="search-keywords"></a>Palabras clave de búsqueda

Opcionalmente, puede escribir un máximo de tres palabras clave de búsqueda para ayudar a los clientes a buscar su oferta en Marketplace. Para obtener mejores resultados, intente usar también estas palabras clave en la descripción.

### <a name="products-your-app-works-with"></a>Productos con los que funciona su aplicación

Si quiere permitir que los clientes sepan que su aplicación funciona con productos específicos, escriba hasta tres nombres de productos aquí.

### <a name="support-urls"></a>Direcciones URL de soporte técnico

Esta sección le permite proporcionar vínculos para ayudar a los clientes a comprender mejor su oferta.

#### <a name="help-link"></a>Vínculo de ayuda

Escriba la dirección URL en la que los clientes pueden obtener más información sobre su oferta.

#### <a name="privacy-policy-url"></a>Dirección URL de la directiva de privacidad

Escriba la dirección URL a la directiva de privacidad de su organización. Usted es el responsable no solo de garantizar que la aplicación cumple la normativa y la legislación en lo tocante a la privacidad, sino también de proporcionar una directiva de privacidad válida.

### <a name="contacts"></a>Contactos

En esta sección, va a proporcionar el nombre, el correo electrónico y el número de teléfono de un **contacto de soporte técnico** y un **contacto de ingeniería**. Esta información no se muestra a los clientes, pero estará disponible para Microsoft y puede proporcionarse a los asociados de CSP.

En la sección **Contacto de soporte técnico**, proporcione la **dirección URL de soporte técnico** en la que los asociados de CSP pueden encontrar soporte técnico para la oferta.

### <a name="supporting-documents"></a>Documentos relacionados

Proporcione al menos un documento de marketing relacionado (y un máximo de tres) aquí, como notas del producto, folletos, listas de comprobación o presentaciones. Estos documentos deben estar en formato PDF.

### <a name="marketplace-images"></a>Imágenes de Marketplace

En esta sección, puede proporcionar logotipos e imágenes que se usarán al mostrar la oferta al cliente. Todas las imágenes deben estar en formato .PNG.

>[!Note]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

#### <a name="store-logos"></a>Logotipos de Store

Proporcione el logotipo de la oferta en dos tamaños de píxel:

- **Pequeño (48 x 48)**
- **Grande (216 x 216)**

#### <a name="hero"></a>Imagen prominente

La imagen prominente es opcional. Si proporciona una, debe medir 815 x 290 píxeles.

#### <a name="screenshots"></a>Capturas de pantalla

Agregue capturas de pantallas que muestren el funcionamiento de la oferta. Se requiere al menos una captura de pantalla y puede agregar un máximo de cinco. Todas las capturas de pantallas deben tener 1280 x 720 píxeles.

#### <a name="videos"></a>Vídeos

Opcionalmente, puede agregar hasta cuatro vídeos que muestren la oferta. Estos vídeos se deben hospedar en YouTube o Vimeo. En cada uno de ellos, escriba el nombre del vídeo, su dirección URL y una imagen en miniatura del vídeo (1280 x 720 píxeles).

#### <a name="additional-marketplace-listing-resources"></a>Recursos adicionales de la descripción de Marketplace

[Procedimientos recomendados la descripción de ofertas en Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

Seleccione **Guardar borrador** antes de continuar.

## <a name="availability"></a>Disponibilidad

Esta página proporciona opciones sobre dónde y cómo hacer que la oferta esté disponible.

### <a name="markets"></a>Mercados

Esta sección le permite especificar los mercados en los que la oferta debe estar disponible. Para ello, seleccione **Editar mercados**, que mostrará la ventana emergente **Selección de mercado**.

De forma predeterminada, no se selecciona ningún mercado. Seleccione al menos un mercado para publicar la oferta. Haga clic en **Seleccionar todo** para que su oferta esté disponible en cada mercado posible o seleccione los mercados específicos que quiera agregar. Al acabar, seleccione **Guardar**.

Las selecciones aquí solo se aplican a las nuevas adquisiciones. Si algún usuario ya tiene su aplicación en un determinado mercado y posteriormente lo quita, las personas que ya tienen la oferta en dicho mercado podrán seguir utilizándola, pero ningún cliente nuevo en ese mercado podrá obtener su oferta.

> [!IMPORTANT]
> Es su responsabilidad cumplir los requisitos legales locales, incluso si esos requisitos no aparecen aquí ni en el Centro de partners.

Tenga en cuenta que, incluso si selecciona todos los mercados, las leyes y restricciones locales u otros factores pueden impedir que se muestren determinadas ofertas en algunos países y regiones.

### <a name="preview-audience"></a>Público preliminar

Antes de publicar su oferta en la oferta de Marketplace más amplia, primero deberá ponerla a disposición de un **público preliminar** limitado. Escriba una **clave de ocultación** (cualquier cadena que use solo letras minúsculas o números) aquí. Los miembros del público preliminar pueden usar esta clave de ocultación como token para ver una versión preliminar de la oferta en Marketplace.

Después, cuando esté listo para que la oferta esté disponible y quite la restricción de versión preliminar, deberá quitar la **tecla de ocultación** y volver a publicarla.

Seleccione **Guardar borrador** antes de continuar.

## <a name="technical-configuration"></a>Configuración técnica

Esta página especifica los detalles técnicos usados para conectarse a la oferta. Esta conexión nos permite aprovisionar su oferta para el cliente final, en caso de que este elija adquirirla.

### <a name="solution-identifier"></a>Identificador de la solución

Proporcione el identificador de la solución (GUID) para la solución.

Para encontrar el identificador de la solución:
1. En Microsoft Dynamics Lifecycle Services (LCS), seleccione **Administración de soluciones**.
2. Seleccione la solución y, a continuación, busque el **identificador de la solución** en la **información general del paquete**. Si el identificador está en blanco, seleccione **Editar**, vuelva a publicar el paquete e inténtelo de nuevo.

### <a name="release-version"></a>Versión de lanzamiento

Seleccione la versión de Dynamics 365 for Finance and Operations con la que funciona esta solución.

Seleccione **Guardar borrador** antes de continuar.

## <a name="test-drive-technical-configuration"></a>Configuración técnica de la versión de prueba

Esta página permite configurar una demostración («versión de prueba») que permite a los clientes probar la oferta antes de adquirirla. Puede encontrar más información al respecto en el artículo [¿Qué es la versión de prueba?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Para habilitar una versión de prueba, seleccione la pestaña**Habilitar una versión de prueba** y marque la casilla[Configuración de la oferta](#test-drive). Para eliminar la versión de prueba de la oferta, desactive esta casilla.

Los siguientes tipos de versiones de prueba están disponibles, cada uno de ellos con sus propios requisitos de configuración técnica.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Aplicación lógica](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (la configuración técnica no es necesaria)

### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Configuración técnica para la versión de prueba de Azure Resource Manager

Una plantilla de implementación que contiene todos los recursos de Azure que componen la solución. Los productos que se ajustan a este escenario usan solo recursos de Azure. Más información acerca de cómo configurar una [versión de prueba de Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regiones** (obligatorias): Actualmente hay 26 regiones admitidas de Azure en las que se puede usar la versión de prueba. La habitual es que desee que su versión de prueba esté disponible en las regiones en que prevé un mayor número de clientes, con el fin de que puedan seleccionar la región más cercana para lograr el mejor rendimiento posible. Tendrá que asegurarse de que su suscripción puede implementar todos los recursos necesarios en cada una de las regiones que seleccione.

- **Instancias**: Seleccione el tipo (activo o inactivo) y el número de instancias disponibles, que se multiplicarán por el número de regiones en las que está disponible la oferta.

    **Activa**: Este tipo de instancia se implementa y está en espera de acceso por cada región seleccionada. Los clientes pueden acceder al instante a las instancias *frecuentes* de una versión prueba, en lugar de tener que esperar a una implementación. La contrapartida es que estas instancias se ejecutan siempre en su suscripción de Azure, por lo que incurrirán en un mayor costo de tiempo de actividad. Se recomienda tener al menos una instancia *frecuente*, ya que la mayoría de los clientes no quieren esperar a que terminen las implementaciones completas, lo que genera un descenso en la utilización por parte de los clientes si no hay ninguna instancia *frecuente*.

    **Inactiva**: Este tipo de instancia representa el número total de instancias que se pueden implementar por región. Las instancias inactivas requieren que se implemente toda la plantilla de la versión de prueba de Resource Manager en el momento en que el cliente solicita la versión de prueba, por lo que las instancias *inactivas* tardan mucho más tiempo en cargarse que las *activas*. A cambio solo tiene que pagar por la duración de la versión de prueba, *no* se ejecuta de forma ininterrumpida en su suscripción de Azure como las instancias *activas*.

- **Versión de prueba de la plantilla de Azure Resource Manager**: Cargue el archivo. zip que contiene la plantilla de Azure Resource Manager.  Para más información acerca de cómo crear una plantilla de Azure Resource Manager, consulte el artículo del tutorial de inicio rápido [Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Duración de la versión de prueba** (obligatorio): Especifique la duración en que la versión de prueba permanecerá activa, en número de horas. La versión de prueba finaliza automáticamente cuando finaliza este período de tiempo. Esta duración solo se puede indicar con un número completo de horas (p. ej. "2" horas; "1,5" no es un valor válido).

### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Configuración técnica de la versión de prueba de Dynamics 365

Microsoft puede eliminar la complejidad de configurar una versión de prueba hospedando y manteniendo el aprovisionamiento y la implementación del servicio de mediante este tipo de versión de prueba. La configuración de este tipo de versión de prueba hospedada es el mismo, independientemente de que esta destinado a un público de Business Central, Customer Engagement u Operations.

- **Número máximo de versiones de prueba simultáneas** (obligatorio): Establezca el número máximo de clientes que puede usar la versión de prueba al mismo tiempo. Cada usuario simultáneo consumirá una licencia de Dynamics 365 mientras la versión de prueba esté activa, por lo que deberá asegurarse de que tiene suficientes licencias disponibles para admitir el límite máximo establecido. Se recomienda un valor entre 3 y 5.

- **Duración de la versión de prueba** (obligatorio): Especifique la duración en que la versión de prueba permanecerá activa. Para ello debe definir el número de horas. Después de este número de horas, la sesión finalizará y dejará de consumir una de las licencias. Se recomienda un valor que oscile entre 2 y 24 horas, en función de la complejidad de la oferta. Esta duración solo se puede indicar con un número completo de horas (p. ej. "2" horas; "1,5" no es un valor válido).  El usuario puede solicitar una nueva sesión si se agota el tiempo y desea volver a acceder a la versión de prueba.

- **URL de la instancia** (obligatorio): la dirección URL en la que el cliente comenzará su versión de prueba. Normalmente es la dirección URL de la instancia de Dynamics 365 en la que se ejecuta la aplicación con los datos de ejemplo instalados (por ejemplo, `https://testdrive.crm.dynamics.com`).

- **Dirección URL de API web de la instancia** (obligatorio): Recupere la dirección URL de la API web de la instancia de Dynamics 365. Para ello, inicie sesión en su cuenta de Microsoft 365 y vaya a **Configuración** \&gt; **Personalización** \&gt; **Recursos para desarrolladores** \&gt; **API web de instancia (URL raíz de servicio)** . Copie la dirección URL que se encuentra aquí (por ejemplo, `https://testdrive.crm.dynamics.com/api/data/v9.0`).

- **Nombre de rol** (obligatorio) : Proporcione el nombre del rol de seguridad que ha definido en su versión de prueba de Dynamics 365 personalizada. Se le asignará al usuario durante su versión de prueba (por ejemplo, test-drive-role).

### <a name="technical-configuration-for-logic-app-test-drive"></a>Configuración técnica de la versión de prueba de Logic App

Los productos personalizados deben usar este tipo de plantilla de implementación de versión de prueba, que engloba varias arquitecturas de soluciones complejas. Para más información acerca de cómo configurar versiones de prueba de Logic App, visite [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) y [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) en GitHub.

- **Regiones** (obligatorio, lista desplegable de selección única): Actualmente hay 26 regiones admitidas de Azure en las que se puede usar la versión de prueba. Los recursos de la aplicación lógica se implementarán en la región que seleccione. Si la aplicación lógica tiene recursos personalizados almacenado en una región concreta, asegúrese de que se selecciona aquí dicha región. La mejor forma de asegurarse de que tiene recursos personalizados para su región es implementar totalmente Logic App de forma local en la suscripción de Azure en el portal y comprobar que funciona antes de realizar esta selección.

- **Número máximo de versiones de prueba simultáneas** (obligatorio): Establezca el número máximo de clientes que puede usar la versión de prueba al mismo tiempo. Estas versiones de prueba ya están implementadas, lo que permitiendo a los clientes acceder al instante a ellas sin tener que esperar que se implementen.

- **Duración de la versión de prueba** (obligatorio): Especifique la duración en que la versión de prueba permanecerá activa, en número de horas. La versión de prueba termina automáticamente al finalizar este período.

- **Nombre del grupo de recursos de Azure:** (obligatorio): Escriba el[nombre del grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) en el que se guarda la versión de prueba de la aplicación lógica.

- **Asignar nombre de aplicación lógica** (obligatorio): escriba el nombre de la aplicación lógica que asigna la versión de prueba al usuario. Dicha aplicación lógica debe guardarse en el grupo de recursos de Azure anterior.

- **Desaprovisionar nombre de aplicación lógica** (obligatorio): escriba el nombre de la aplicación lógica que desaprovisiona la versión de prueba una vez que el cliente ha finalizado. Dicha aplicación lógica debe guardarse en el grupo de recursos de Azure anterior.

### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>No es necesaria configuración técnica para las versiones de prueba de Power BI

Los productos que desean demostrar que un objeto visual interactivo de Power BI puede usar un vínculo insertado para compartir un panel personalizado como versión de prueba, no se necesita ninguna configuración técnica. Más información acerca de cómo configurar aplicaciones de la plantilla[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview).

### <a name="deployment-subscription-details"></a>Detalles de suscripción de implementación

Para implementar la versión de prueba en su nombre, cree y proporcione una suscripción de Azure independiente y única. (no es necesario para las versiones de prueba de Power BI).

- **ID de suscripción de Azure** (obligatorio para Azure Resource Manager y aplicaciones lógicas): Escriba el identificador de la suscripción para conceder acceso a los servicios de la cuenta de Azure para la generación de informes y facturación del uso de los recursos. Se recomienda considerar la posibilidad de [crear una suscripción de Azure independiente](https://docs.microsoft.com/azure/billing/billing-create-subscription) que se use para las versiones de prueba, en caso de que no se tenga ninguna. Para buscar su identificador de la suscripción de Azure inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a la pestaña **Suscripciones** del menú izquierdo. Al seleccionar la pestaña se mostrará el identificador de la suscripción (por ejemplo, "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **ID del inquilino de Azure AD** (obligatorio): Escriba el identificador de inquilino [de Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in). Para buscar dicho identificador, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Active Directory del menú de la izquierda, seleccione **Propiedades** y busque el número de **identificador de directorio** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). El ID de inquilino de su organización también lo puede buscar mediante la dirección URL de su nombre de dominio en: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Nombre del inquilino de Azure AD** (obligatorio para Dynamic 365): Escriba el nombre de inquilino de Azure Active Directory (AD). Para buscar este nombre, inicie sesión en [Azure Portal](https://portal.azure.com/) y en la esquina superior derecha se mostrará su nombre del inquilino, debajo de su nombre de cuenta.

- **Identificador de aplicación de Azure AD** (obligatorio): Escriba el identificador de la aplicación [de Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in). Para buscarlo, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Active Directory del menú de la izquierda, seleccione **Registros de aplicaciones** y busque el número de **Id. de aplicación** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Secreto del cliente de la aplicación Azure AD** (obligatorio): Escriba el secreto del cliente[de su aplicación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets). Para encontrar este valor, inicie sesión en [Azure Portal](https://portal.azure.com/). Seleccione la pestaña **Azure Active Directory** en el menú izquierdo, seleccione **Registros de aplicaciones** y, después, seleccione la versión de prueba de su aplicación. A continuación, seleccione **Certificados y secretos**, seleccione **Nuevo secreto de cliente**, escriba una descripción, seleccione **Nunca** en **Expira** y, después, elija **Agregar**. No olvide copiar el valor. (No abandone la página sin haber copiado el valor, ya que, de lo contrario no tendrá acceso al mismo).

Seleccione **Guardar borrador** antes de continuar.

### <a name="test-drive-marketplace-listings"></a>Listas de Marketplace de la versión de prueba

La opción **Lista de Marketplace** que se encuentra en la pestaña **Versión de prueba** muestra los idiomas en los que está disponible la versión de prueba. La única ubicación disponible actualmente es **Inglés (Estados Unidos)** . Seleccione el nombre de idioma para especificar la información que describe la experiencia de la versión de prueba.

- **Descripción** (obligatoria): describa la versión de prueba, lo que se demostrará, los objetivos con los que experimentará el usuario, las características que se van a explorar y cualquier información de interés que ayude al usuario a determinar si debe adquirir la oferta. En este campo se puede escribir hasta 3000 caracteres de texto.

- **Información de acceso** (obligatorio para las versiones de prueba de Azure Resource Manager y aplicaciones lógicas): explique lo que un cliente necesita saber para acceder a esta versión de prueba y usarla. se muestra un escenario para usar la oferta y exactamente lo que el cliente debe saber para acceder a las características en la versión de prueba. En este campo se puede escribir hasta 10 000 caracteres de texto.

- **Manual del usuario** (obligatorio): tutorial detallado de la experiencia con la versión de prueba. Debe abarcar exactamente lo que desee que el cliente aprenda al usar la versión de prueba y puede servir como referencia para las preguntas que puedan surgirle. El archivo debe estar en formato PDF y hay que asignarle un nombre (255 caracteres como máximo) después de cargarlo.

- **Videos** (opcional): Se pueden cargar vídeos en YouTube o Vimeo y hacer referencia a ellos aquí con un vínculo o una imagen en miniatura (533 × 324 píxeles) para que los clientes puedan ver toda la información que pueda ayudarles a entender mejor la versión de prueba, incluida la forma de usar correctamente las características de la oferta y comprender los escenarios que destacan sus ventajas.
  - **Nombre** (se requiere)
  - **URL** (obligatorio, solo YouTube o Vimeo)
  - **En miniatura** (la imagen debe estar en formato PNG y tener 533 x 324 píxeles)

Seleccione **Guardar borrador** antes de continuar.

## <a name="supplemental-content"></a>Contenido complementario

Esta página le permite proporcionar información adicional sobre su oferta, lo que nos ayudará a validarla. Esta información no se muestra a los clientes ni se publica en Marketplace.

### <a name="validation-assets"></a>Recursos de validación

Debe cargar un [informe de análisis de personalización (CAR)](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/dev-tools/customization-analysis-report) en esta sección. Este informe se genera mediante el análisis de los modelos de personalización y extensión, en función de un conjunto predefinido de reglas de procedimientos recomendados.

Este archivo debe estar en formato .xls o .xlsx. Si tiene más de un informe, puede cargar un archivo ZIP que contenga todos los informes.

### <a name="does-solution-include-localizations"></a>¿La solución incluye localizaciones?

Seleccione **Sí** si la solución permite el uso de estándares y directivas locales (por ejemplo, si se ajusta a las distintas reglas de nómina que requieren diferentes países o regiones). De lo contrario, seleccione **No**.

### <a name="does-solution-enable-translations"></a>Does solution enable translation(s)? (¿La solución permite traducciones?)

Responda **Sí** si el texto de la solución se puede traducir en otros idiomas. De lo contrario, seleccione **No**.

Seleccione **Guardar borrador** antes de continuar.

## <a name="publish"></a>Publicar

### <a name="submit-offer-to-preview"></a>Envío de la oferta para tener una versión preliminar

Una vez que haya completado todas las secciones necesarias de la oferta, seleccione **Revisar y publicar** en la esquina superior derecha del portal.

Si es la primera vez que publica esta oferta, puede:

- Ver el estado de finalización de cada sección de la oferta.
    - **No iniciada**: Significa que la sección no se ha modificado y se debe completar.
    - **Incompleta**: Significa que la sección tiene errores que deben corregirse o que requiere más información. Vuelva a las secciones y actualícelas.
    - **Completa**: Significa que la sección está completa, que se han proporcionado todos los datos necesarios y que no hay ningún error. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- En la sección **Notas para la certificación**, envíe instrucciones para la realización de pruebas al equipo de certificación para asegurarse de que la aplicación se prueba correctamente, además de todas las notas adicionales que pueden resultar útiles para conocer la aplicación.
- Enviar la oferta para su publicación, para lo que debe seleccionar **Enviar**. Le enviaremos un correo electrónico que le avise cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Vuelva al Centro de partners y seleccionar **Transmitir** la oferta para publicar la oferta para el público (o si es una oferta privada, al público privado).

## <a name="next-step"></a>Paso siguiente

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
