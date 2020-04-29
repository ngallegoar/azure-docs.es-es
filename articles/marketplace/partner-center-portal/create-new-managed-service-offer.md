---
title: Creación de una nueva oferta de servicio administrado en Marketplace comercial
description: Cómo crear una oferta de servicio administrado para la lista de Azure Marketplace mediante el portal de Marketplace comercial en el Centro de partners.
author: JnHs
ms.author: jenhayes
manager: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: c82c3e0b6e6d11ccc33bf8556b06aeb04a6d37b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147905"
---
# <a name="create-a-new-managed-service-offer"></a>Creación de una nueva oferta de servicio administrado

> [!IMPORTANT]
> Estamos trasladando la administración de las ofertas de servicios administrados de Cloud Partner Portal al Centro de partners. Hasta que se migren las ofertas, siga las instrucciones que se indican en [Publicación de una oferta de servicios administrados en Azure Marketplace](../../lighthouse/how-to/publish-managed-services-offers.md) para administrar las ofertas en Cloud Partner Portal.

Servicio administrado ofrece ayuda para habilitar escenarios de [Azure Lighthouse](../../lighthouse/overview.md). Cuando un cliente acepta una oferta de servicio administrado, puede incorporar recursos para la [administración de recursos delegados de Azure](../../lighthouse/concepts/azure-delegated-resource-management.md).

Para empezar a crear ofertas de Servicio administrado, asegúrese de que primero [crea una cuenta del Centro de partners](./create-account.md) y después abre el [panel de Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), con la página **Información general** seleccionada. Debe tener un [nivel de competencia de plataforma en la nube Silver o Gold](https://partner.microsoft.com/membership/cloud-platform-competency) o ser un [MSP experto de Azure](https://partner.microsoft.com/membership/azure-expert-msp) para publicar una oferta de servicio administrado.

![Panel de Marketplace comercial en el Centro de partners](./media/new-offer-overview.png)

>[!Note]
> Una vez publicada una oferta, las modificaciones de esta realizadas en el Centro de partners solo se actualizarán en el sistema y en los escaparates después de volver a publicarla. Asegúrese de enviar la oferta para su publicación después de realizar los cambios.

## <a name="create-a-new-offer"></a>Crear una nueva oferta

Seleccione el botón **+ Nueva oferta** y, a continuación, seleccione el elemento de menú **Servicio administrado**. Aparecerá el cuadro de diálogo **Nueva oferta**.

### <a name="offer-id-and-alias"></a>Identificador y alias de la oferta

- **Id. de oferta**: Identificador único para cada oferta de su cuenta. Este identificador será visible para los clientes en la dirección URL de la oferta de Marketplace. Este identificador solo puede contener caracteres alfanuméricos en minúsculas (puede incluir guiones y guiones bajos, pero no espacios en blanco), se limita a 50 caracteres y no se puede cambiar después de seleccionar **Crear**.  Por ejemplo, si escribe *test-offer-1* aquí, la dirección URL de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Alias de la oferta**: nombre que se usa para hacer referencia a la oferta desde el Centro de partners. Este nombre no se usará en Marketplace y es diferente del nombre de la oferta y otros valores que se mostrarán a los clientes. Este valor no se puede cambiar después de seleccionar **Crear**.

Una vez que escriba el **identificador** y el **alias de la oferta**, seleccione **Crear**. Después podrá trabajar en todas las partes de la oferta.

## <a name="offer-setup"></a>Configuración de la oferta

En la página **Configuración de la oferta** se solicita la siguiente información. Después de completar estos campos, asegúrese de seleccionar **Guardar**.

## <a name="connect-lead-management"></a>Conexión de administración de clientes potenciales

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Tenga en cuenta que, según las [directivas de certificación de servicios administrados](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services), se requiere un **Destino de clientes potenciales**. Con esta acción se creará un registro en el sistema CRM cada vez que un cliente implemente la oferta.

Para obtener más información, consulte [Introducción a la administración de clientes potenciales](./commercial-marketplace-get-customer-leads.md).

No olvide **guardar** la página antes de pasar a la siguiente sección.

## <a name="properties"></a>Propiedades

La página **Propiedades** le permite definir las categorías que se usan para agrupar su oferta en Marketplace y los contratos legales que dan soporte a la oferta. Después de completar esta página, seleccione **Guardar**.

### <a name="category"></a>Category

Seleccione un mínimo de una y un máximo de cinco categorías, que se utilizarán para colocar la oferta en las áreas de búsqueda de Marketplace adecuadas. Asegúrese de indicar la forma en que la oferta da soporte a estas categorías en la descripción de la oferta.

### <a name="terms-and-conditions"></a>Términos y condiciones

Proporcione sus propios términos y condiciones legales en el campo **Términos y condiciones**. También puede proporcionar la dirección URL donde se pueden encontrar los términos y condiciones. Los clientes deberán aceptar estos términos para poder probar la oferta.

## <a name="offer-listing"></a>Descripción de la oferta

La página **Descripción de la oferta** le permite definir los detalles de Marketplace (nombre de la oferta, descripción, imágenes etc.) para cada oferta.

> [!NOTE]
> No es necesario que el contenido del anuncio de la oferta (por ejemplo, la descripción, los documentos, las capturas de pantallas, los términos de uso, etc.) esté en inglés siempre que la descripción de la oferta empiece por la frase "Esta aplicación solo está disponible en [idioma distinto del inglés]". También es aceptable proporcionar una *dirección URL de vínculo útil* para ofrecer contenido en un idioma distinto del que se usa en el contenido del anuncio de la oferta.

### <a name="name"></a>Nombre

El nombre que escriba aquí se mostrará a los clientes como el título de la descripción de la oferta. Este campo se ha rellenado previamente con el texto introducido para **Alias de la oferta** cuando creó la oferta, pero puede cambiar este valor. Este nombre puede contener marcas comerciales (y puede incluir símbolos de marca comercial o copyright). El nombre no puede tener más de 50 caracteres ni incluir emojis.

### <a name="search-results-summary"></a>Resumen de los resultados de la búsqueda

Especifique una descripción breve de la oferta (100 caracteres como máximo), que se va a usar en los resultados de la búsqueda de Marketplace.

### <a name="long-summary"></a>Resumen largo

Proporcione una descripción más larga de la oferta (hasta 256 caracteres). Este resumen largo también puede usarse en los resultados de la búsqueda de Marketplace.

### <a name="description"></a>Descripción

Proporcione una descripción más larga de la oferta (hasta 3.000 caracteres). Esta descripción se mostrará a los clientes en la información general de la lista de Marketplace. Incluya una propuesta de valor de la oferta, los beneficios clave, las asociaciones de la categoría o sector, las oportunidades de compra en la aplicación y todas las divulgaciones necesarias.

Algunos consejos para escribir la descripción:  

- Describa claramente la propuesta de valor de la oferta en las primeras frases de su descripción. Incluya los siguientes elementos en la propuesta de valor:
  - Descripción de la oferta.
  - Tipo de usuario que se beneficia de la oferta.
  - Necesidad o preocupación del cliente que aborda la oferta.
- Tenga en cuenta que estas primeras frases podrían mostrarse en los resultados de los motores de búsqueda.  
- No se base exclusivamente en las características y funcionalidades para vender su producto. En su lugar, céntrese en el valor que proporciona.  
- Use tanto como se pueda un vocabulario específico del sector o palabras relacionadas con las ventajas.

Para que la descripción de la oferta sea más atractiva, use el editor de texto enriquecido para darle formato.

![Uso del editor de texto enriquecido](./media/text-editor2.png)

Siga las instrucciones siguientes para usar el editor de texto enriquecido:

- Para cambiar el formato del contenido, resalte el texto al que quiera dar formato y seleccione un estilo de texto, como se muestra aquí:

     ![Uso del editor de texto enriquecido para cambiar el formato del texto](./media/text-editor3.png)

- Para agregar una lista con viñetas o numerada al texto, use las opciones siguientes:

     ![Uso del editor de texto enriquecido para agregar listas](./media/text-editor4.png)

- Para agregar o quitar la sangría del texto, use las opciones siguientes:

     ![Uso del editor de texto enriquecido para aplicar sangría](./media/text-editor5.png)

### <a name="privacy-policy-link"></a>Vínculo de la directiva de privacidad

Escriba la dirección URL a la directiva de privacidad de la organización (hospedada en su sitio). Usted es el responsable no solo de garantizar que la aplicación cumple la normativa y la legislación en lo tocante a la privacidad, sino también de proporcionar una directiva de privacidad válida.

### <a name="useful-links"></a>Vínculos útiles

Proporcione documentos en línea complementarios opcionales sobre su solución. Para agregar más vínculos útiles, haga clic en **+ Agregar un vínculo**.

### <a name="contact-information"></a>Información de contacto

En esta sección, debe proporcionar el nombre, el correo electrónico y el número de teléfono de un **contacto de soporte técnico** y un **contacto de ingeniería**. Esta información no se muestra a los clientes, pero estará disponible para Microsoft y puede proporcionarse a los asociados de CSP.

### <a name="support-urls"></a>Direcciones URL de soporte técnico

Si tiene sitios web de soporte técnico para **clientes globales de Azure** o **clientes de Azure Government**, proporcione estas direcciones URL aquí.

### <a name="marketplace-images"></a>Imágenes de Marketplace

En esta sección, puede proporcionar logotipos e imágenes que se usarán al mostrar la oferta al cliente. Todas las imágenes deben estar en formato .PNG.

>[!Note]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

#### <a name="marketplace-logos"></a>Logotipos de Marketplace

Se requieren cuatro tamaños de logotipo: **Pequeño (40 x 40)** , **Medio (90 x 90)** , **Grande (115 x 115)** y **Ancho (255 x 155)** . Siga estas instrucciones para los logotipos:

- El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.
- Los colores del tema del portal son el blanco y el negro. No use estos colores como fondo de los logotipos. Use un color que haga que su logotipo destaque en el portal. Nosotros recomendamos usar colores primarios simples.
- Si usa un fondo transparente, asegúrese de que el logotipo y el texto no son blancos, negros ni azules.
- La apariencia del logotipo debe ser homogénea y evitar degradados. No utilice un fondo degradado en el logotipo.
- No coloque texto en el logotipo, ni siquiera el nombre de su empresa o de la marca.
- Asegúrese de que el logotipo no quede estirado.

#### <a name="screenshots"></a>Capturas de pantalla

Agregue hasta cinco capturas de pantalla que muestren el funcionamiento de la oferta. Todas las capturas de pantallas deben tener 1280 x 720 píxeles.

#### <a name="videos"></a>Vídeos

Opcionalmente, puede agregar hasta cinco vídeos que muestren la oferta. Estos vídeos se deben hospedar en YouTube o Vimeo. En cada uno de ellos, escriba el nombre del vídeo, su dirección URL y una imagen en miniatura del vídeo (1280 x 720 píxeles).

#### <a name="additional-marketplace-listing-resources"></a>Recursos adicionales de la descripción de Marketplace

- [Procedimientos recomendados la descripción de ofertas en Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Vista previa

Antes de publicar su oferta en la oferta de Marketplace más amplia, primero deberá ponerla a disposición de un público preliminar limitado. Esto le permite confirmar cómo se muestra la oferta en Azure Marketplace antes de ponerla a disposición de los clientes. Los equipos de soporte técnico y de ingeniería de Microsoft también podrán ver su oferta durante este período de versión preliminar.

Para definir el público preliminar, escriba los identificadores de suscripción de Azure en la sección **Público preliminar**. Puede escribir hasta 10 identificadores de suscripción manualmente o cargar un archivo .csv con hasta 100 identificadores de suscripción.

Los clientes asociados con estas suscripciones podrán ver la oferta en Azure Marketplace antes de que se publique. Asegúrese de incluir sus propias suscripciones aquí para poder obtener la versión preliminar de la oferta.

## <a name="plan-overview"></a>Información general del plan

Cada oferta debe tener uno o varios planes (a veces denominados SKU). Puede agregar varios planes para admitir diferentes conjuntos de características a precios diferentes o para personalizar un plan específico para un público limitado de clientes específicos. Los clientes pueden ver los planes disponibles para ellos en la oferta principal.

En la página **Información general del plan**, seleccione **+ Crear nuevo plan**. A continuación, escriba un **Id. de plan** y un **nombre de plan**. Ambos valores solo puede contener caracteres alfanuméricos en minúsculas, guiones y guiones bajos, con un máximo de 50 caracteres. Estos valores pueden ser visibles para los clientes y no se pueden cambiar después de publicar la oferta.

Seleccione **Crear** una vez que haya escrito estos valores para seguir trabajando en el plan. Hay tres secciones para completar: **Lista del plan**, **Precios y disponibilidad** y **Configuración técnica**.

### <a name="plan-listing"></a>Lista del plan

En primer lugar, proporcione un **Resumen de resultados de búsqueda** para el plan. Se trata una descripción breve del plan (100 caracteres como máximo), que se va a usar en los resultados de la búsqueda de Marketplace.

A continuación, escriba una **descripción** que proporcione una explicación más detallada del plan.

### <a name="pricing-and-availability"></a>Precios y disponibilidad

Actualmente, solo hay un modelo de precios que se puede usar para la oferta de servicio administrado: **Traiga su propia licencia (BYOL)** . Esto significa que facturará a sus clientes directamente por los costos relacionados con esta oferta y Microsoft no le aplicará ningún cargo.

La sección **Visibilidad del plan** le permite indicar si este plan debe ser [privado](../../marketplace/private-offers.md). Si deja el cuadro **Este plan es privado** desactivado, el plan no se restringirá a clientes específicos (o a un número determinado de clientes).

Para que este plan esté disponible solo para clientes específicos, seleccione **Sí**. Al hacerlo, tendrá que identificar a los clientes proporcionando sus identificadores de suscripción. Se pueden especificar de uno en uno (hasta 10 suscripciones) o mediante la carga de un archivo. csv (hasta 10.000 suscripciones en todos los planes). Asegúrese de incluir sus propias suscripciones aquí para poder probar y validar la oferta.

> [!IMPORTANT]
> Una vez publicado un plan como público, no puede cambiarlo a privado. Para controlar qué clientes pueden aceptar su oferta y delegar recursos, use un plan privado. Con un plan público, no puede restringir la disponibilidad a determinados clientes ni a un determinado número de clientes, aunque puede dejar de vender el plan por completo si decide hacerlo. Puede [quitar el acceso a una delegación](../../lighthouse/how-to/remove-delegation.md) después de que un cliente acepte una oferta solo si incluyó una **autorización** con la **definición de rol** establecida en [Rol para eliminar la asignación de registros de servicios administrados](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) al publicar la oferta. También puede ponerse en contacto con el cliente y solicitarle [que quite el acceso](../../lighthouse/how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

### <a name="technical-configuration"></a>Configuración técnica

Esta sección del plan crea un manifiesto con información de autorización para administrar los recursos del cliente. Esta información es necesaria para habilitar la [administración de recursos delegados de Azure](../../lighthouse/concepts/azure-delegated-resource-management.md).

Asegúrese de revisar los [inquilinos, roles y usuarios de los escenarios de Azure Lighthouse](../../lighthouse/concepts/tenants-users-roles.md#best-practices-for-defining-users-and-roles) para comprender qué roles se admiten y cuáles son los procedimientos recomendados para definir las autorizaciones.

> [!NOTE]
> Como se indicó anteriormente, los usuarios y los roles en las entradas de **Autorización** se aplicarán a todos los clientes que compren el plan. Si quiere limitar el acceso a un cliente específico, deberá publicar un plan privado para uso exclusivo.

#### <a name="manifest"></a>Manifest

En primer lugar, proporcione una **Versión** para el manifiesto. Use el formato *n.n.n* (por ejemplo, 1.2.5).

A continuación, escriba su **Id. de inquilino**. Se trata de un GUID asociado con el identificador del inquilino de Azure Active Directory (Azure AD) de la organización; es decir, el inquilino de administración desde el que obtendrá acceso a los recursos de sus clientes. Si no lo tiene a mano, para buscarlo, mantenga el mouse sobre el nombre de la cuenta en la parte superior derecha de Azure Portal o seleccione **Cambiar directorio**.

Si publica una nueva versión de la oferta y necesita crear un manifiesto actualizado, seleccione **+ Nuevo manifiesto**. Asegúrese de aumentar el número de versión de la versión anterior del manifiesto.

#### <a name="authorization"></a>Authorization

Las autorizaciones definen las entidades del inquilino de administración que pueden tener acceso a los recursos y las suscripciones para los clientes que compran el plan. A cada una de estas entidades se le asigna un rol integrado que concede niveles de acceso específicos.

Puede crear hasta veinte autorizaciones para cada plan.

> [!TIP]
> En la mayoría de los casos, querrá asignar roles a una entidad de servicio o un grupo de usuarios de Azure AD, en lugar de a una serie de cuentas de usuario individuales. Esto le permite agregar o quitar el acceso de usuarios individuales sin tener que actualizar y volver a publicar el plan cuando cambien los requisitos de acceso. Al asignar roles a grupos de Azure AD, [asegúrese de que el **tipo de grupo** sea **Seguridad** y no **Office 365**](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Para recomendaciones adicionales, consulte [Inquilinos, roles y usuarios en escenarios de Azure Lighthouse](../../lighthouse/concepts/tenants-users-roles.md).

Para cada **Autorización**, deberá proporcionar la siguiente información. Puede seleccionar **+ Agregar autorización** tantas veces como sea necesario para agregar más definiciones de roles y usuarios.

- **Id. de objeto de Azure AD**: identificador de Azure AD de un usuario, un grupo de usuarios o una aplicación al que se concederán determinados permisos (según se define en la definición de roles) para los recursos de los clientes.
- **Nombre para mostrar de objeto de Azure AD**: nombre descriptivo para ayudar al cliente a entender el propósito de esta autorización. El cliente verá este nombre al delegar recursos.
- **Role Definition**: seleccione uno de los roles integrados de Azure AD disponibles en la lista. Este rol determinará los permisos que el usuario del campo **Id. de objeto de Azure AD** tendrá en los recursos de los clientes. Para obtener descripciones de estos roles, consulte los [roles integrados](../../role-based-access-control/built-in-roles.md) y la [compatibilidad de roles con la administración de recursos delegados de Azure](../../lighthouse/concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management).
  > [!NOTE]
  > A medida que se agreguen nuevos roles integrados a Azure, estarán disponibles aquí, aunque pueden retrasarse un poco en aparecer.
- **Roles asignables**: esta opción solo aparece si ha seleccionado Administrador de acceso de usuario en **Definición de roles** para esta autorización. En ese caso, debe agregar uno o varios roles asignables aquí. El usuario del campo **Id. de objeto de Azure AD** podrá asignar estos roles a [identidades administradas](../../active-directory/managed-identities-azure-resources/overview.md), algo necesario para [implementar directivas que pueden corregirse](../../lighthouse/how-to/deploy-policy-remediation.md). Tenga en cuenta que no se aplicará a este usuario ningún otro permiso asociado normalmente al rol Administrador de acceso de usuario.

> [!TIP]
> Para asegurarse de que puede [quitar el acceso a una delegación](../../lighthouse/how-to/remove-delegation.md), si es necesario, incluya una **autorización** con la **definición de rol** establecida en [Rol para eliminar la asignación de registros de servicios administrados](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role). Si este rol no está asignado, solo un usuario puede quitar los recursos delegados del inquilino del cliente.

Una vez que haya completado todas las secciones del plan, puede seleccionar **+ Crear nuevo plan** tantas veces como necesite para crear planes adicionales. Cuando finalice, seleccione **Guardar**.

## <a name="publish"></a>Publicar

### <a name="submit-offer-to-preview"></a>Envío de la oferta para tener una versión preliminar

Una vez que haya completado todas las secciones necesarias de la oferta, seleccione **Publicar** en la esquina superior derecha del portal. Se le redirigirá a la página **Review and publish** (Revisar y publicar).

Si es la primera vez que publica esta oferta, puede:

- Ver el estado de finalización de cada sección de la oferta.
  - *No iniciada*: significa que la sección no se ha modificado y se debe completar.
  - *Incompleta*: significa que la sección tiene errores que deben corregirse o que requiere más información. Vuelva a las secciones y actualícelas.
  - *Completa*: significa que la sección está completa, que se han proporcionado todos los datos necesarios y que no hay ningún error. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- En la sección **Notas para la certificación**, envíe instrucciones para la realización de pruebas al equipo de certificación para asegurarse de que la aplicación se prueba correctamente, además de todas las notas adicionales que pueden resultar útiles para conocer la aplicación.
- Enviar la oferta para su publicación, para lo que debe seleccionar **Enviar**. Le enviaremos un correo electrónico cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Vuelva al Centro de partners y seleccionar **Transmitir** la oferta para publicar la oferta para el público (o si es una oferta privada, al público privado).

### <a name="customer-experience-and-offer-management"></a>Experiencia del cliente y administración de ofertas

Cuando un cliente implemente su oferta, podrá delegar uno o varios grupos de recursos o suscripciones para la [administración de recursos delegada de Azure](../../lighthouse/concepts/azure-delegated-resource-management.md). Para obtener más información acerca de este proceso, consulte [Proceso de incorporación del cliente](../../lighthouse/how-to/publish-managed-services-offers.md#the-customer-onboarding-process).

Puede [publicar una versión actualizada de la oferta](update-existing-offer.md) en cualquier momento. Por ejemplo, puede querer agregar una nueva definición de roles a una oferta publicada previamente. Al hacerlo, los clientes que ya hayan agregado la oferta verán un icono en la página [**Proveedores de servicios**](../../lighthouse/how-to/view-manage-service-providers.md) de Azure Portal que les informa que hay disponible una actualización. Cada cliente podrá revisar los cambios y decidir si quiere actualizar a la nueva versión.

## <a name="next-steps"></a>Pasos siguientes

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
- [Más información acerca de Azure Lighthouse](../../lighthouse/overview.md)