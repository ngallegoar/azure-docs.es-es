---
title: Creación de una nueva oferta de SaaS en el marketplace comercial de Microsoft
description: Cómo crear una oferta de software como servicio (SaaS) para mostrarla o venderla en Azure Marketplace, Microsoft AppSource o bien mediante el programa Proveedor de soluciones en la nube (CSP) mediante el portal del marketplace comercial de Microsoft Partner Center.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: b85846419ce5f6cd4093e198d83240817c95754a
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82983064"
---
# <a name="create-a-new-saas-offer-in-the-commercial-marketplace"></a>Creación de una oferta de SaaS en marketplace comercial

Para empezar a crear ofertas de software como servicio (SaaS) en marketplace comercial, asegúrese de primero [crear una cuenta del Centro de partners](./create-account.md) y después abrir el [panel Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), con la pestaña **Información general** seleccionada.

## <a name="create-a-new-offer"></a>Crear una nueva oferta

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
2. En el menú de navegación izquierdo, seleccione **Marketplace comercial** > **Información general**.
3. En la página Información general, seleccione **+ Nueva oferta** > **Software como servicio**.

   ![Muestra el menú de navegación izquierdo.](./media/new-offer-saas.png)

> [!NOTE]
> Después de publicar una oferta, las modificaciones que se realicen en el Centro de partners solo aparecen en los escaparates cuando la oferta se vuelve a publicar. Asegúrese de volver a realizar la publicación siempre después de realizar cambios.

## <a name="new-offer"></a>Nueva oferta

Escriba un **Identificador de oferta**. Se trata de un identificador único para cada oferta de su cuenta.

- Se muestra a los clientes en la dirección web de la oferta de Marketplace y en las plantillas de Azure Resource Manager, si procede.
- Use solo letras minúsculas y números. Puede incluir guiones y caracteres de subrayado, pero no espacios, y está limitado a 50 caracteres. Por ejemplo, si escribe **test-offer-1**, la dirección web de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- El identificador de oferta no se puede cambiar después de seleccionar **Crear**.

Escriba un **Alias de la oferta**. Este es el nombre que se usa para la oferta en el Centro de partners.

- Este nombre no se usa en Marketplace y es diferente del nombre de la oferta y de otros valores que se muestran a los clientes.
- El alias de la oferta no se puede cambiar después de seleccionar **Crear**.

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

Seleccione **Crear** para generar la oferta y continuar.

## <a name="offer-overview"></a>Información general de la oferta

**Estado de publicación** muestra una representación visual de los pasos necesarios para publicar esta oferta y el tiempo que tardará en completarse cada uno de ellos. Los iconos de los pasos de publicación que no se hayan completados aparecerán atenuados.

El menú **Información general de la oferta** contiene una lista de vínculos para realizar operaciones en esta oferta. Esta lista de operaciones cambiará en función de la selección que realice para la oferta.  

- Si la oferta es un borrador: eliminar borrador
- Si la oferta está activa: detener venta de oferta
- Si la oferta está en versión preliminar: publicar
- Si no ha completado el cierre de sesión del anunciante: cancelar publicación

## <a name="offer-setup"></a>Configuración de la oferta

En esta página se solicita la siguiente información.

- **¿Desea vender a través de Microsoft?** (Sí/No)
    - **Sí**, me gustaría vender a través de Microsoft y que Microsoft hospede transacciones en mi nombre
    - **No**, preferiría solo mostrar mi oferta a través de los marketplaces y procesar las transacciones de forma independiente.

### <a name="sell-through-microsoft"></a>Venta mediante Microsoft

La venta a través de Microsoft ofrece a los clientes una mejor experiencia de hallazgo y adquisición, permite que Microsoft hospede transacciones de Marketplace en su nombre y aprovecha las funcionalidades de comercio disponibles globalmente de Microsoft.

#### <a name="saas-offer-requirements"></a>Requisitos de la oferta de SaaS

Para enumerar las ofertas de software como servicio (SaaS) con Marketplace comercial en el Centro de partners, se deben cumplir los siguientes criterios:

- La oferta debe usar [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) para la administración y autenticación de identidades.
- La oferta debe usar las [API de suministro de SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) para integrarse en Azure Marketplace.

#### <a name="saas-pricing-and-billing-options"></a>Precios de SaaS y opciones de facturación

Con las soluciones de SaaS que se ejecutan en la suscripción de Azure del editor, los honorarios de licencias que pagan los clientes incluyen el costo de la infraestructura en la que se implementa el software. El uso de la infraestructura de Azure se administra y se factura directamente al asociado. El cliente no ve los honorarios de uso de infraestructura reales. Los publicadores deben agrupar los honorarios de uso de la infraestructura de Azure en los precios de las licencias de software. 

Las ofertas de SaaS admiten la facturación mensual o anual en función de una cuota fija, por usuario, o cargos por consumo mediante el servicio de facturación medido. Marketplace comercial de Microsoft opera según un modelo de agencia, en el que los editores establecen los precios, Microsoft factura a los clientes y Microsoft paga los ingresos al editor, a la vez que retiene una cuota correspondiente a la agencia.

Este es un ejemplo de desglose de costos y pagos para demostrar el modelo de agencia:

|**Costo de su licencia**|**100 USD al mes**|
|:---|:---|
|Costo de uso de Azure (D1/1 núcleo)|Facturado directamente al publicador, no al cliente|
|Microsoft factura al cliente|100,00 USD al mes (el anunciante debe contar con los costos de paso a través o en que se incurran en el precio de la licencia)|

|**Microsoft factura**|**100 USD al mes**|
|:---|:---|
|Microsoft le paga el 80 % del costo de licencia <br>**En el caso de las aplicaciones SaaS aptas, Microsoft paga el 90 % del costo de la licencia*|80,00 USD al mes <br>*$* 90,00 al mes*|

- En este ejemplo, Microsoft factura USD 100 al cliente por su licencia de software y paga USD 80 al editor.
- Los asociados que se sean aptos para el **precio reducido del servicio Marketplace** verán un precio de transacción reducido en las ofertas de SaaS desde mayo de 2019 hasta junio de 2020. En este escenario, Microsoft factura 100,00 USD por su licencia de software y paga 90,00 USD al anunciante.

> [!NOTE]
> **Precio reducido del servicio Marketplace**: Para determinadas ofertas de SaaS que haya publicado en nuestro Marketplace comercial, Microsoft reducirá su precio del servicio Marketplace del 20 % (como se describe en el Acuerdo para anunciantes de Microsoft) al 10 %. Para que su oferta sea apta, Microsoft debe haber designado que al menos una de sus ofertas esté lista para realizar una venta conjunta por IP o esté clasificada orden de prioridad para realizar una venta conjunta por IP. Para recibir este precio reducido del servicio Marketplace durante el mes, se deben cumplir los criterios de idoneidad al menos cinco (5) días hábiles antes del final de cada mes natural.  El precio reducido del servicio Marketplace no se aplica a las máquinas virtuales, las aplicaciones administradas ni cualquier otro producto que esté disponible a través de nuestro Marketplace comercial. El precio reducido del servicio Marketplace solo estará disponible para las ofertas aptas para los cargos de licencia recaudados por Microsoft entre el 1 de mayo de 2019 y el 30 de junio de 2020. Transcurrido este tiempo, la tarifa del servicio Marketplace volverá a su importe normal.

### <a name="list-through-microsoft"></a>Listas a través de Microsoft

La creación de una lista en Marketplace promocionará su negocio. Si selecciona la opción de mostrar solo su oferta, no realizar transacciones a través de Microsoft significa que Microsoft no participa directamente en las transacciones de las licencias de software. No hay precio de transacción asociado y el anunciante conserva el 100 % del precio de las licencias de software abonado por el cliente. Sin embargo, el anunciante es el responsables de dar soporte a todos los aspectos de la transacción de la licencia de software, entre los que se incluyen, entre otros: la cumplimentación del pedido, la medición, la facturación, el pago y el cobro.

<!-- - **How do you want potential customers to interact with this listing offer?** -->

#### <a name="get-it-now-free"></a>Obténgalo ahora (de forma gratuita)

Publique la oferta para los clientes de manera gratuita proporcionando una dirección válida (que empiece por *http* o *https*), en la que pueda obtener una evaluación mediante [autenticación con un solo clic con Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials). Por ejemplo, `https://contoso.com/saas-app`.

#### <a name="free-trial-listing"></a>Evaluación gratuita (anuncio)

Publique la oferta para los clientes con un vínculo a una evaluación gratuita proporcionando una dirección válida (que empiece por *http* o *https*), en la que pueda obtener una evaluación mediante [autenticación con un solo clic con Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials). Por ejemplo, `https://contoso.com/trial/saas-app`. Las ofertas que incluyen evaluaciones gratuitas se crean, administran y configuran desde su servicio y no tienen suscripciones administradas por Microsoft.

> [!NOTE]
> Los tokens que la aplicación recibirá a través del vínculo de evaluación gratuita solo se pueden usar para obtener información de usuario mediante Azure AD para automatizar la creación de cuentas en la aplicación. No se admiten las cuentas de Microsoft (MSA) para la autenticación con este token.

#### <a name="contact-me"></a>Ponerse en contacto conmigo

Recopile información de contacto de los clientes mediante la conexión de su sistema de Administración de relaciones con los clientes (CRM). El cliente le pedirá permiso para compartir su información. Estos datos del cliente, junto con el nombre e identificador de la oferta y el origen de Marketplace donde la ha encontrado, se envían al sistema CRM que se ha configurado. Para más información acerca de cómo configurar CRM, consulte [Conexión de administración de clientes potenciales](#connect-lead-management).

#### <a name="example-marketplace-offer-listing"></a>Descripción de la oferta de Marketplace de ejemplo

![Descripción de la oferta de Marketplace de ejemplo con notas](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Habilitación de una versión de prueba

Una versión de prueba es una excelente manera de presentar la oferta a clientes potenciales, ya que se les brinda la oportunidad de "probarla antes de comprarla", lo que aumenta la conversión y la generación de clientes potenciales altamente cualificados. [Más información sobre las versiones de prueba](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Para habilitar una versión de prueba durante un período de tiempo determinado, seleccione la casilla **Habilitar una versión de prueba**. Para eliminar la versión de prueba de la oferta, desactive esta casilla.

Para obtener más información, consulte [Versión de prueba de la oferta en el marketplace comercial](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive).

### <a name="test-drive-resources"></a>Recursos de la versión de prueba

- [Procedimientos recomendados de marketing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Procedimientos técnicos recomendados](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Información general](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF, asegúrese de que el bloqueador de elementos emergentes está desactivado)

### <a name="connect-lead-management"></a>Conexión de administración de clientes potenciales

[!INCLUDE [Connect lead management](./includes/connect-lead-management-a.md)]

#### <a name="additional-lead-management-resources"></a>Recursos adicionales de la administración de clientes potenciales
- [Preguntas frecuentes de la administración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Errores comunes de la configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Póster Introducción a la administración de clientes potenciales](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Seleccione **Guardar borrador** antes de continuar.

## <a name="properties"></a>Propiedades

En esta página se le pide que defina las categorías y los sectores que se usan para agrupar su oferta en los catálogos de soluciones, los contratos legales en los que se basa su oferta y su versión de la aplicación.

### <a name="category"></a>Category

Seleccione entre una categoría, como mínimo, y tres categorías, como máximo, para agrupar la oferta en las áreas de búsqueda apropiadas de marketplace. Describa la forma en que la oferta admite estas categorías en la descripción de la oferta.

### <a name="industries"></a>Industrias

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>Versión de la aplicación

Es un campo opcional que se usa en el Marketplace de AppSource para identificar el número de versión de la oferta.

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Contrato estándar para el Marketplace comercial de Microsoft

Microsoft proporciona una plantilla de contrato estándar.

- **¿Quiere usar el contrato estándar para Marketplace comercial de Microsoft?**

Para simplificar el proceso de adquisición para los clientes y reducir la complejidad legal para los proveedores de software, Microsoft ofrece un contrato estándar para el Marketplace comercial de Microsoft con el fin de ayudar a facilitar las transacciones en Marketplace. En lugar de crear términos y condiciones personalizados, los anunciantes del Marketplace comercial pueden ofrecer su software en virtud del Contrato estándar, que los clientes solo tienen que revisar y aceptar una vez. El Contrato estándar se puede encontrar en https://go.microsoft.com/fwlink/?linkid=2041178.

Puede optar por usar el contrato estándar en lugar de proporcionar sus propios términos y condiciones personalizados activando la casilla "¿Quiere usar el contrato estándar para Marketplace comercial de Microsoft?".

![Uso de la casilla Contrato estándar](./media/use-standard-contract.png)

> [!NOTE]
> Después de publicar una oferta con el contrato estándar para Marketplace comercial de Microsoft, no puede usar sus propios términos y condiciones personalizados. Solo puede elegir una de las dos opciones. Puede ofrecer su solución con el Contrato estándar **o** con sus propios términos y condiciones. Si quiere modificar los términos del contrato estándar, puede hacerlo a través de las modificaciones del contrato estándar.

#### <a name="standard-contract-amendments"></a>Modificaciones del Contrato estándar

Las modificaciones del contrato estándar permiten a los anunciantes seleccionar los términos del contrato estándar por motivos de simplicidad y personalizarlos para su producto o empresa. Los clientes solo necesitan revisar las modificaciones del contrato, si ya han revisado y aceptado el Contrato estándar de Microsoft.

Hay dos tipos de modificaciones para los anunciantes de Marketplace comercial:

- Modificaciones universales: Estas modificaciones se aplican universalmente al Contrato estándar para todos los clientes. Las modificaciones universales se muestran a cada cliente de la oferta en el flujo de compra. Los clientes deben aceptar los términos del Contrato estándar y la modificación antes de que puedan usar su oferta.
- Modificaciones personalizadas: Estas son modificaciones especiales al Contrato estándar que están dirigidas a cliente únicamente a clientes específicos a través de los Id. de inquilino de Azure. Los anunciantes pueden elegir al inquilino al que desean dirigirse. Solo los clientes del inquilino recibirán los términos con modificaciones personalizadas en el flujo de compra de la oferta.  Los clientes deben aceptar los términos del Contrato estándar y las modificaciones antes de que puedan usar su oferta.

>[!NOTE]
> Estos dos tipos de modificaciones se superponen entre sí. Los clientes específicos con modificaciones personalizadas también obtendrán la modificación universal al Contrato estándar durante la compra.

**Términos de modificación universales del Contrato estándar de Marketplace comercial de Microsoft**: Escriba los términos de modificación universal en esta casilla. Puede proporcionar una única modificación universal por oferta. Puede escribir un número ilimitado de caracteres en este cuadro. Estos términos se muestran a los clientes en AppSource, Azure Marketplace o Azure Portal durante el flujo de detección y compra.

**Términos de modificación personalizados del Contrato estándar de Marketplace comercial de Microsoft**: Para empezar, seleccione **Add custom amendment terms** (Agregar términos de modificación personalizados). Puede proporcionar hasta 10 términos de modificación personalizados por oferta.

- **Términos de modificación personalizados**: Escriba sus términos de modificación personalizados en el cuadro Términos de modificación personalizados. Puede escribir un número ilimitado de caracteres en este cuadro. Solo los clientes de los id. de inquilino que se especifiquen para estos términos personalizados recibirán los términos de modificación personalizados en el flujo de compra de la oferta en Azure Portal.  
- **Identificadores de inquilino** (obligatorio): cada modificación personalizada puede tener como destino hasta 20 identificadores de inquilino. Si agrega una modificación personalizada, debe proporcionar al menos un identificador de inquilino. El identificador de inquilino identifica al cliente en Azure. Puede pedir a su cliente este identificador y encontrarlo si navega a portal.azure.com > Azure Active Directory > Propiedades. El valor del identificador de directorio es el identificador de inquilino (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). También puede buscar el identificador de inquilino de la organización de su cliente mediante su dirección URL de nombre de dominio en [What is my Microsoft Azure and Office 365 tenant ID?](https://www.whatismytenantid.com) (¿Cuál es mi identificador de inquilino de Microsoft Azure y Office 365?).
- **Descripción** (opcional): De manera opcional, escriba una descripción detallada del identificador de inquilino que le ayude a identificar al cliente al que está destinada la modificación.

#### <a name="terms-and-conditions"></a>Términos y condiciones

Si quiere proporcionar sus propios términos y condiciones personalizados, puede especificarlos en el campo Términos y condiciones. Puede escribir hasta 10 000 caracteres de texto en este campo. Si los términos y condiciones requieren una descripción más larga, escriba en este campo un vínculo de dirección URL donde se pueden encontrar los términos y condiciones. A los clientes les aparecerá como un vínculo activo.

Los clientes deben aceptar estos términos para poder probar la oferta.

Seleccione **Guardar borrador** antes de continuar.

## <a name="offer-listing"></a>Descripción de la oferta

En esta página se muestran los idiomas (y los mercados) en los que está disponible la oferta, actualmente la única ubicación disponible es Inglés (Estados Unidos). Además, esta página muestra el estado de la descripción específica del idioma y la fecha y hora en que se agregó. Tendrá que definir los detalles de Marketplace (nombre de la oferta, descripción, términos de búsqueda, etc.) para cada idioma y mercado.

> [!NOTE]
> No es necesario que el contenido del anuncio de la oferta (por ejemplo, la descripción de la oferta, los documentos, las capturas de pantallas, los términos de uso y la directiva de privacidad) esté en inglés siempre que la descripción de la oferta empiece por la frase "Esta aplicación solo está disponible en [idioma distinto del inglés]". También es aceptable proporcionar una *dirección URL de vínculo útil* para ofrecer contenido en un idioma distinto del que se usa en el contenido del anuncio de la oferta.

### <a name="offer-listings"></a>Descripción de la oferta

Especifique los detalles que se muestran en Marketplace, lo que incluye descripciones de la oferta y los recursos de marketing.

- **Nombre** (obligatorio): El nombre que se defina aquí aparecerá como título de la descripción de la oferta en los marketplace que haya elegido. El nombre se rellena previamente en función de lo que se haya escrito en **Nueva oferta**. El nombre puede ser una marca comercial. No puede contener emojis (a menos que sean símbolos de marca comercial y copyright), y no puede tener más de 50 caracteres.
- **Resumen** (obligatorio): Especifique una descripción breve de la oferta que se va a usar en los resultados de búsqueda en las descripciones del marketplace. En este campo se puede escribir hasta 100 caracteres de texto.
- **Descripción** (obligatorio): Escriba una descripción de la oferta que se va a mostrar en la información general de las descripciones del marketplace. Considere la posibilidad de incluir una propuesta de valor, los beneficios clave, todas las asociaciones de la categoría o sector, las oportunidades de compra en la aplicación, todas las divulgaciones necesarias y un vínculo para obtener más información. En este campo se puede escribir hasta 3000 caracteres de texto, incluido el marcado. Consulte [Escribir una excelente descripción de la aplicación](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description) para encontrar más sugerencias.
- **Palabras clave de búsqueda**: Escriba un máximo de tres palabras clave de búsqueda que los clientes puedan usar para buscar su oferta en los marketplaces.
- **Instrucciones para las Tareas iniciales**  (obligatorio): Se explica cómo configurar y empezar a usar la aplicación para los clientes potenciales.  Este tutorial de inicio rápido puede contener vínculos a documentación en línea más detallada. En este campo se puede escribir hasta 3000 caracteres de texto.

#### <a name="description"></a>**Descripción**

Este campo es obligatorio. Elementos que debe incluir en  **Descripción**:

* Describa claramente la propuesta de valor de la oferta en las primeras frases de su descripción.  
* Tenga en cuenta que estas primeras frases podrían mostrarse en los resultados de los motores de búsqueda.  
* No se base exclusivamente en las características y funcionalidades para vender su producto. En su lugar, céntrese en el valor que proporciona.  
* Use tanto como se pueda un vocabulario específico del sector o palabras relacionadas con las ventajas.

Los componentes principales de la propuesta de valor deben incluir lo siguiente:

* Descripción del producto
* Tipo de usuario que se beneficia del producto
* Necesidad o preocupación del cliente que aborda el producto.

Para que la descripción de la oferta sea más atractiva, use el editor de texto enriquecido para darle formato.

![Uso del editor de texto enriquecido](./media/rich-text-editor.png)

| <center>Cambiar el formato de texto | <center>Agregar viñetas o numeración | <center>Agregar o quitar sangría del texto |
| --- | --- | --- |
| <center>![Uso del editor de texto enriquecido para cambiar el formato del texto](./media/text-editor3.png) |  <center>![Uso del editor de texto enriquecido para agregar listas](./media/text-editor4.png) |  <center>![Uso del editor de texto enriquecido para aplicar sangría](./media/text-editor5.png) |

#### <a name="links"></a>Vínculos

- **Directiva de privacidad** (obligatorio): Vínculo a la directiva de privacidad de su organización. Usted es el responsable no solo de garantizar que la aplicación cumple la normativa y la legislación en lo tocante a la privacidad, sino también de proporcionar una directiva de privacidad válida
- **Materiales de marketing del programa CSP** (opcional): Si decide ampliar su oferta al programa [Proveedor de soluciones en la nube (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers), debe incluir un vínculo a los materiales de marketing. CSP amplía su oferta a una rango mayor clientes aptos, ya que permite que los asociados de CSP incluyan su oferta en paquetes, la comercialicen y la distribuyan. Los revendedores necesitarán acceder a estos materiales para comercializar su oferta. Para más información, consulte [Servicios de comercialización](https://partner.microsoft.com/reach-customers/gtm).
- **Vínculos útiles** (opcional): Documentos en línea complementarios opcionales acerca de la aplicación o de servicios relacionados que se muestran tras escribir un **Título** y una **Dirección URL**. Para agregar más vínculos útiles, haga clic en **+ Agregar una dirección URL** .

#### <a name="contact-information"></a>Información de contacto

- **Contactos**: En cada contacto de cliente incluya el **Nombre**, **Número de teléfono** y dirección de **Correo electrónico** de un empleado (estos *no* se mostrarán públicamente). Se requiere una **Dirección URL de soporte** para el grupo **Contacto de soporte técnico** (esta *sí* se mostrará públicamente).

    - **Contacto de soporte técnico** (obligatorio): Para preguntas generales de soporte técnico.
    - **Contacto de ingeniería** (obligatorio): Para preguntas técnicas.
    - **Contacto del administrador de canales** (obligatorio): Para preguntas de revendedores relacionadas con el programa CSP.

#### <a name="files-and-images"></a>Archivos e imágenes

- **Documentos** (obligatorio): Agregue a su oferta documentos de marketing relacionados, en formato PDF. Incluya un proporciona un mínimo de un (1) documento por oferta y un máximo de tres (3).
- **Imágenes** (opcional): En todos los marketplaces, hay varios lugares en los que pueden aparecer imágenes del logotipo de su oferta, que requieren los siguientes tamaños en formato PNG:

    - **Pequeño** (48×48, obligatorio)
    - **Mediano** (90×90, obligatorio)
    - **Grande** (216×216, obligatorio)
    - **Ancho (255 x 115)**
    - **Principal** (815×290)

- **Capturas de pantalla** (obligatorio): Agregue capturas de pantalla que muestren la oferta. Se pueden agregar hasta cinco (5) y deben ser de 1280 x 720 píxeles. Todas las imágenes deben estar en formato .PNG.
- **Vídeos** (opcional): Agregue vínculos a vídeos en que se muestre la oferta. Puede usar vínculos a vídeos de YouTube o Vimeo para que acompañen a la oferta para los clientes. También tendrá que incluir una imagen en miniatura del vídeo con un tamaño de 1280 x 720 píxeles y en formato PNG. Puede mostrar un máximo de cuatro vídeos por oferta.

>[!NOTE]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

#### <a name="additional-marketplace-listing-resources"></a>Recursos adicionales de la descripción de Marketplace

- [Procedimientos recomendados la descripción de ofertas en Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

Seleccione **Guardar borrador** antes de continuar.

## <a name="preview-audience"></a>Público preliminar

Esta página le permite definir un **público preliminar** limitado para publicar la oferta antes de ponerla a disposición de un público más amplio del marketplace.

> [!IMPORTANT]
> Después de comprobar que la oferta está en versión preliminar, seleccione **Publicar** para que se publique en vivo para el público de Marketplace.

Agregue un correo electrónico individual de la cuenta de AAD/MSA por línea, junto con una descripción opcional.

Agregue un máximo de 10 direcciones de correo electrónico manualmente o 20 si carga un archivo CSV para las cuentas existentes de Azure Active Directory o de Microsoft (MSA) para ayudar a validar la oferta antes de publicarla. Al agregar estas cuentas, define el público al que se le proporcionará acceso preliminar a la oferta antes de que se publique en Marketplace. Aunque la oferta ya esté publicada, puede definir un público preliminar para probar los cambios o las actualizaciones que ofrece.

> [!NOTE]
> El público preliminar no es el mismo que un público privado. A una audiencia preliminar se le permite acceder a la oferta _antes_ de que se publique en los catálogos de soluciones. También puede elegir crear un plan y ponerlo a disposición únicamente de un público privado. En la pestaña **Lista del plan** , puede definir un público privado con la casilla **This is a private plan** (Este es un plan privado). Puede definir un público privado de hasta 20 000 clientes mediante los identificadores de los inquilinos de Azure.

Seleccione **Guardar borrador** antes de continuar.

## <a name="technical-configuration"></a>Configuración técnica

En esta página se definen los detalles técnicos (ruta de acceso de la dirección URL, webhook, identificador de inquilino e identificador de aplicación) que se usan para conectarse a su oferta. Esta conexión nos permite aprovisionar su oferta para el cliente final, en caso de que este elija adquirirla. En la documentación de las [API de suministro de SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) hay diagramas que describen el uso de los campos recopilados.

- **URL de página de aterrizaje** (obligatorio): Defina la dirección URL del sitio adonde se dirigirá a los clientes después de adquirir la oferta en el marketplace. Dicha URL será el punto de conexión que reciba un token cuando un cliente se enrute a la página. Dicho token se puede intercambiar para aprovisionar detalles mediante el uso de resolver en las API de suministro. Tanto estos detalles como otros que recopile pueden utilizarse como parte de una página web interactiva con los clientes basada en su experiencia para completar el registro y activar su compra.

- **Webhook de conexión** (obligatorio): Para todos los eventos asincrónicos que Microsoft tiene que enviarle en nombre del cliente (por ejemplo, si la suscripción a SaaS ya no es válida), le exigimos que nos proporcione un webhook de conexión. Si todavía no tiene un sistema de webhooks activo, la configuración más sencilla es una aplicación de lógica de punto de conexión HTTP que escuche los eventos que se publiquen en ella y que, después, los gestione de la forma adecuada (por ejemplo, https:\//prod-1westus.logic.azure.com:443/work). Para obtener más información, consulte [Llamada, desencadenamiento o anidamiento de flujos de trabajo con puntos de conexión HTTP en aplicaciones lógicas](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Id. de inquilino de Azure AD** (obligatorio): En Azure Portal, es preciso que [cree una aplicación de Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) para que podamos validar que la conexión entre nuestros dos servicios está detrás de una comunicación autenticada. Para buscar el [identificador de inquilino](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in), vaya a Azure Active Directory, seleccione **Propiedades** y, después, busque el número de **identificador de directorio** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Id. de aplicación de Azure AD** (obligatorio): También necesita su [identificador de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) y una clave de autenticación. Para obtener esos valores, vaya a Azure Active Directory, seleccione **Registros de aplicaciones** y, después, busque el número de **identificador de la aplicación** que se muestra (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). Para buscar la clave de autenticación, vaya a **Configuración** y seleccione **Claves**. Deberá proporcionar una descripción y una duración y, después, se proporcionara un valor numérico.

>[!Note]
>El identificador de aplicación de Azure está asociado al identificador de anunciante, por lo que debe asegurarse de que en todas las ofertas se use el mismo identificador de aplicación.

Seleccione **Guardar borrador** antes de continuar.

## <a name="plan-overview"></a>Información general del plan

Esta página le permite ofrecer una variedad de opciones de planes dentro de la misma oferta. Estos planes (a veces denominados SKU) pueden diferir en lo tocante a versión, monetización o niveles de servicio. Para vender su oferta en Marketplace, debe configurar al menos un plan.

Una vez creados, verá los nombres de los planes, los identificadores, los modelos de precios, disponibilidad (pública o privada), el estado actual de la publicación y las acciones disponibles.

Las **acciones** disponibles en **Información general del plan** varían en función del estado actual del plan y pueden incluir:

- Si el estado del plan es **Borrador**: eliminar borrador
- Si el estado del plan es **Activo**: detener venta del plan o sincronizar público privado

**Crear nuevo plan** (un mínimo de un plan para quienes seleccionen vender a través de Microsoft)

- **Id. de plan:** cree un identificador de plan único para cada plan de esta oferta. Los clientes lo verán en la dirección URL del producto y en las plantillas de Azure Resource Manager (si procede). Use solo caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. El identificador de este plan no puede tener más de 50 caracteres. El identificador no se puede modificar después de seleccionar que se cree.
- **Nombre del plan:** Los clientes verán este nombre al decidir que plan van a seleccionar en su oferta. Cree un nombre de oferta único para cada uno de los planes de esta oferta. El nombre del plan se usa para diferenciar los planes de software que pueden formar parte de la misma oferta (por ejemplo, Nombre de la oferta: Windows Server; planes: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Lista del plan

Esta página le permite definir el nombre y la descripción del plan. <!-- displays the languages (and markets) where your plan is available, currently English (United States) is the only location available. Additionally, this page displays the status of the language-specific listing and the date/time that it was added. You will need to define the marketplace details (offer name, description, search terms, etc.) for each language / market.-->

<!--#### Plan listing details-->

<!--Selecting one of the plan languages will display the **plan listing** information, including **Name** and **Description.** -->

- **Nombre**: Rellenado previamente en función de la entrada **Nuevo plan** en versión preliminar y aparecerá como título del "Plan de software" de su oferta que aparece en el marketplace.
- **Descripción**: Esta descripción es una oportunidad para explicar lo que hace que este plan de software sea único y las diferencias con respecto a otros planes de software de su oferta. Puede contener un máximo de 500 caracteres.

Seleccione **Guardar borrador** antes de continuar.

#### <a name="pricing-and-availability"></a>Precios y disponibilidad

Esta página permite configurar los mercados en los que estará disponible este plan, el modelo de monetización deseado, el precio y el período de facturación. Además, puede indicar si desea que el plan sea visible a todos los usuarios o solo a clientes específicos (una audiencia privada).

#### <a name="markets-optional"></a>Mercados (opcional)

Todos los planes debe estar disponible al menos en un mercado. Seleccione **Editar mercados** y seleccione la casilla de la ubicación del mercado en la que desea que este plan esté disponible. En esta página se incluyen un cuadro de búsqueda y una opción para seleccionar los países con "Impuesto remitido", en los que Microsoft remite de los impuestos de uso y ventas en su nombre.

Si ya se ha establecido los precios del plan en dólares de Estados Unidos (USD) y agrega otra ubicación del mercado, el precio del nuevo mercado se calculará en función de los tipos de cambio actuales. Revise el precio de cada mercado antes de la publicación. Vea los precios mediante el vínculo "Exportar precios (xlsx)" después de guardar los cambios.

Antes de continuar, seleccione **Guardar**.

#### <a name="pricing"></a>Precios

##### <a name="pricing-model"></a>Modelo de precios

**Tarifa plana**: Habilite el acceso a su oferta con un única tarifa plana mensual o anual. A veces esto se denomina precios basados en puesto. Con este modelo de precios, opcionalmente, puede definir planes limitados que usan la API del servicio de medición de Marketplace para cobrar a los clientes según las unidades no estándar.  Para obtener más información sobre la facturación limitada, consulte [facturación limitada mediante el servicio de medición de Marketplace](./saas-metered-billing.md).

**Por usuario**: Habilite el acceso a su oferta con el precio en función del número de usuarios que acceden a la oferta u que ocupan los puestos. Este modelo basado en usuarios le permite establecer el número mínimo y máximo de usuarios permitido en función del precio. De esta forma, se pueden configurar diferentes puntos de precio en función del número de usuarios mediante la configuración de varios planes.  Estos campos son opcionales. Si no se selecciona, se interpretará que el número de usuarios no tiene límite (el mínimo es 1 y el máximo es tantos como el sistema puede admitir). Estos campos se pueden editar como parte de una actualización del plan.

Una vez publicado, la elección de modelo de precios facturación no se puede cambiar. Además, todos los planes de la misma oferta deben compartir el mismo modelo de precios.

##### <a name="user-limits"></a>Límites de usuarios

Si corresponde, seleccione y establezca los límites de mínimo y máximo de usuarios.

##### <a name="billing-term-and-price"></a>Plazo y precio de facturación

Seleccione el **plazo** y el **precio** a los que los clientes deben pagar el precio de la lista. Los clientes deben tener a su disposición al menos un precio mensual o anual, pero también pueden estar disponibles ambas opciones.

Los precios establecidos en USD (USD = dólares de Estados Unidos) se convierten en la moneda local de todos los mercados seleccionados con los tipos de cambio actuales en el momento de guardarlos. Valide estos precios antes de realizar la publicación. Para ello, debe exportar la hoja de cálculo de precios y examinar el precio de cada mercado. Si desea establecer precios personalizados en un mercado individual, modifique e importe la hoja de cálculo de precios. Usted es responsable de validar estos precios y posee estos valores.
*\*Primero debe guardar los cambios de precios para habilitar la exportación de los datos de precios.*

Revise los precios cuidadosamente antes de la publicación, ya que hay algunas restricciones en lo que se puede cambiar una vez publicado un plan:

- Una vez que se publica un plan, el modelo de precios no se puede cambiar.
- Una vez que se publica el periodo de facturación de un plan, no se puede eliminar más tarde.
- Una vez que se publica un precio para un mercado de su plan, no puede cambiarse más adelante.

#### <a name="free-trial"></a>Versión de prueba gratuita

Las ofertas de SaaS a través del Marketplace comercial le permiten proporcionar una evaluación gratuita de un mes al vender a través de Microsoft. Las evaluaciones gratuitas se admiten para todos los modelos y términos de facturación, excepto los planes limitados. Esta opción facilita a los clientes la entrada mediante un mes de acceso gratuito.  Si decide habilitar una evaluación gratuita para los planes dentro de la oferta, el cliente no podrá convertirla en una suscripción de pago antes de que finalice el período inicial de un mes.  Durante este tiempo, los clientes que compren su oferta pueden probar cualquiera de los planes admitidos que tengan la evaluación gratuita habilitada y convertirlos entre ellos.  La conversión a una suscripción de pago se realiza automáticamente al final del período.

>[!NOTE]
>Si el cliente decide convertir a un plan sin evaluaciones gratuitas, se producirá la conversión, pero la evaluación gratuita se perderá de inmediato. Además, una vez que un cliente empieza a pagar un plan, ya no puede volver a obtener una evaluación gratuita de la misma suscripción, aunque lo convierta a una SKU que admita evaluaciones gratuitas.

Aquí puede configurar una evaluación gratuita para cada plan de su oferta. Seleccione la casilla para permitir una evaluación de un mes.

![Casilla de evaluación gratuita de un mes](./media/free-trial-enable.png)

>[!NOTE]
>Una vez que se haya publicado la oferta de transacciones con una evaluación gratuita, no se puede deshabilitar para ese plan. Asegúrese de que esta configuración sea correcta para la primera publicación para evitar tener que volver a crear el plan.

Para obtener información sobre las suscripciones de clientes que actualmente participan en una evaluación gratuita, use la nueva propiedad de API `isFreeTrial`, que se marcará como true o false. Para más información, vea la [API para obtener suscripción de SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#get-subscription).

>[!NOTE]
>Las evaluaciones gratuitas no se admiten para los planes que usan el servicio de medición de Marketplace.

#### <a name="plan-visibility"></a>Visibilidad del plan

Puede configurar cada plan para que lo pueda ver todo el mundo o solo un público concreto que elija. Para asignar la pertenencia a este público utilice los identificadores de inquilino de Azure AD.

##### <a name="privacy"></a>Privacidad

Seleccione **Este plan es privado** para que el plan sea privado y solo pueda verlo el público que elija. Una vez que se haya publicado como un plan privado, es posible actualizar el público o elegir que el plan esté disponible para todos los usuarios. Una vez que se publica un plan como visible para todos los usuarios, debe permanecer visible para todos (puede volver a publicarse como plan privado).

##### <a name="restricted-audience-tenant-ids"></a>**Público restringido (identificadores de inquilino)**

Asigne a la audiencia que tendrá acceso a este plan privado. El acceso se asigna mediante identificadores de inquilino y existe la opción de incluir una descripción de cada identificador de inquilino asignado. Se pueden agregar un máximo de 10 identificadores de inquilino, o 20 000 identificadores de inquilino de clientes si se importa un archivo de hoja de cálculo .csv.

Un inquilino es una representación de una organización, con un identificador representado como un GUID (identificador único global, un número entero de 128 bits utilizado para identificar los recursos). Se trata de una instancia dedicada de Azure AD que una organización o el desarrollador de la aplicación reciben cuando crean una relación con Microsoft, como al registrarse en Azure, Microsoft Intune o Microsoft 365. Cada inquilino de Azure AD es distinto e independiente de los demás inquilinos de Azure AD. Para comprobar el inquilino, inicie sesión en Azure Portal con la cuenta que desee usar para administrar la aplicación. Si tiene un inquilino, iniciará sesión de forma automática y verá el nombre de este directamente bajo el nombre de la cuenta. Mantenga el puntero sobre el nombre de su cuenta en el lado superior derecho de Azure Portal para ver el nombre, el correo electrónico, el directorio e identificador de inquilino (un GUID) y el dominio. Si la cuenta está asociada a varios inquilinos, puede seleccionar el nombre de la cuenta para abrir un menú donde puede cambiar entre los inquilinos. Cada inquilino tiene su propio identificador de inquilino. También puede buscar el identificador de inquilino de su organización mediante una dirección URL de nombre de dominio en [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

Mientras que las ofertas de SaaS usan identificadores de inquilino para definir un público privado, otros tipos de oferta pueden usar los identificadores de suscripción de Azure (que también se representan como GUID).

> [!NOTE]
> El público privado (o un público limitado) no es el mismo que un público preliminar. En la página **[Versión preliminar](#preview-audience)** , puede definir un público preliminar. El público preliminar puede acceder a su oferta *antes de* que esta se publique en Marketplace. Aunque la designación de público privado solo se aplica a un plan concreto, el público preliminar puede ver todos los planes (privados o no), pero solo durante el período de versión preliminar, mientras el plan se prueba y valida.

Seleccione **Guardar borrador** antes de continuar.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Lista de planes de ejemplo en una oferta de Marketplace

![Descripción de plan de Marketplace de ejemplo con notas](./media/marketplace-plan.svg)

## <a name="cloud-solution-provider-csp-reseller-audience"></a>Audiencia de revendedores de Proveedor de soluciones en la nube (CSP)

Si elige esta opción, los proveedores de soluciones en la nube podrán vender su producto como parte de una solución agrupada a sus clientes. Para más información, consulte [Proveedor de soluciones en la nube](https://go.microsoft.com/fwlink/?linkid=2111109).

## <a name="publish"></a>Publicar

Una vez que haya completado todas las secciones necesarias de la oferta, seleccione **Revisar y publicar** en la esquina superior derecha del portal.

### <a name="submit-offer-to-preview"></a>Envío de la oferta para tener una versión preliminar

Si es la primera vez que publica esta oferta, puede:

- Ver el estado de finalización de cada sección de la oferta.
    - **No iniciada**: La sección no se ha modificado y se debe completar.
    - **Incompleta**: La sección tiene errores que deben corregirse o se requiere más información. Tendrá que volver a la sección anterior y actualizarla.
    - **Completa**: La sección está completa, se han proporcionado todos los datos necesarios y no hay ningún error. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- Envíe instrucciones para la realización de pruebas al equipo de certificación para asegurarse de que la aplicación se prueba correctamente, además de todas las notas adicionales que pueden resultar útiles para conocer la aplicación.
- Enviar la oferta para su publicación, para lo que debe seleccionar **Enviar**. Le enviaremos un correo electrónico que le avise cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Debe volver al Centro de partners y seleccionar **Transmitir** la oferta para publicar la oferta para el público (o si es una privada, al público privado).

## <a name="next-step"></a>Paso siguiente

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
