---
title: Requisitos por tipo de publicación | Azure
description: En este artículo se describen los criterios de idoneidad y los requisitos de publicación de los asociados que intentan comprender cómo publicar aplicaciones en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: dsindona
ms.openlocfilehash: c9936e9c406e262c06d9016b88f8999e46dcb917
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684353"
---
# <a name="requirements-by-listing-type"></a>Requisitos por tipo de publicación  
Los requisitos de contenido de marketing y técnicos varían en función del escaparate, el tipo de oferta y el tipo de anuncio. Revise las siguientes especificaciones para verificar el cumplimiento.  
1. Requisitos de escaparate:  
    *   [AppSource](#storefront-requirements-appsource)  
    *   [Azure Marketplace](#storefront-requirements-azure-marketplace)  
2. Requisitos por tipo de publicación y tipo de oferta:  
    *   Para más información sobre los tipos de publicación y los tipos de oferta, visite la página Determinar el tipo de anuncio adecuado para su oferta en [docs.microsoft.com/azure/marketplace/determine-your-listing-type](./determine-your-listing-type.md).  

## <a name="storefront-requirements-appsource"></a>Requisitos de escaparate: AppSource  
En la tabla siguiente se describen los requisitos previos para publicar en AppSource.  

| Requisito | Detalles | Obligatorio o recomendado |  
|:--- |:--- |:--- |  
| ***Azure Active Directory (Azure AD)*** | La aplicación debe permitir el inicio de sesión único (SSO) federado de Azure Active Directory con consentimiento habilitado.<ul> <li>Para más información sobre cómo habilitar el SSO federado de Azure AD, visite la página Configuración del inicio de sesión único en aplicaciones que no están en la Galería de aplicaciones de Azure Active Directory en [docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).</li> </ul> | Obligatorio |   
| ***Integración con Servicios en la nube de Microsoft*** | Su aplicación debe integrarse con otros servicios en la nube de Microsoft, como Microsoft Power BI, Cortana Intelligence o servicios de Microsoft Azure.<ul> <li>Un ejemplo de un servicio de Microsoft Cloud es Internet de las cosas.</li> </ul> | Recomendado |  
| ***Audiencia*** | La aplicación debe estar destinada a usuarios de línea de negocio y propietarios de empresas. | Obligatorio | 
| ***Aplicación de software como servicio (SaaS) para la empresa*** | La aplicación debe cumplir los siguientes requisitos.<ul> <li>Ser una aplicación SaaS de línea de negocio</li> <li>Centrarse en el proceso empresarial</li> <li>Dirigirse a los clientes de la empresa</li> <li>Permitir a los usuarios usar las credenciales profesionales para iniciar sesión, como el nombre de usuario y la contraseña</li> </ul> | Obligatorio |  
| ***Período de evaluación gratuita y experiencia de evaluación*** | La aplicación debe incluir una de las siguientes opciones para que un cliente pueda usar la aplicación de forma gratuita durante un tiempo limitado.<ul> <li>Proporcionar un método `try`, par que los clientes puedan iniciar una prueba de la aplicación en AppSource</li> <li>Proporcionar una opción `request trial` en AppSource, para que los clientes puedan solicitar una versión de evaluación de la aplicación</li> </ul>La prueba gratuita que proporcione debe ofrecer al cliente un período de tiempo previamente establecido para probar la aplicación sin ningún coste adicional. | Obligatorio |  
| ***Solución de fácil configuración y lista para usar*** | La aplicación debe ser fácil y rápida de configurar e instalar sin necesidad de realizar ninguna personalización. | Obligatorio |  
| ***Administración de clientes potenciales*** | Habilite CRM para aceptar los datos de clientes potenciales antes de recibir clientes potenciales desde el escaparate.<ul> <li>Ejemplos de CRM son Marketo, Microsoft Dynamics o Salesforce</li> </ul> | Obligatorio |  
| ***Directiva de privacidad y términos de uso*** | La aplicación debe proporcionar un vínculo a la página de la directiva de privacidad mediante una dirección URL pública. Los términos de uso deben especificarse como texto durante la publicación. | Obligatorio |  
| ***Soporte técnico*** | La aplicación debe proporcionar un vínculo a la página de asistencia al cliente mediante una dirección URL pública. Si la aplicación es una versión de evaluación, debe proporcionar soporte técnico sin costo adicional durante el período de evaluación. | Obligatorio |  

## <a name="storefront-requirements-azure-marketplace"></a>Requisitos de escaparate: Azure Marketplace  
A continuación, se indican los requisitos previos para los tipos de publicación en Azure Marketplace.  

| Requisito | Detalles | Tipo de aplicación |  
|:--- |:--- |:--- |  
| ***Directivas de participación*** | La aplicación debe seguir las directivas de participación de Azure Marketplace.<ul> <li>Para más información sobre las directivas de participación, visite la página Directivas de participación de Azure Marketplace en [azure.microsoft.com/support/legal/marketplace/participation-policies](https://azure.microsoft.com/support/legal/marketplace/participation-policies).</li></ul> | list<br />transacción<br />evaluación |  
| ***Integración con Microsoft*** | Su oferta debe usar o ampliar los tipos de servicio de Microsoft Azure, como proceso, red o almacenamiento. La oferta debe orientarse a una de las categorías existentes de Azure Marketplace, como bases de datos, seguridad o red.<ul> <li>Para más información sobre las ofertas de Marketplace, viste la página de aplicaciones de Marketplace en [azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps).</li> </ul> | list<br />transacción<br />evaluación |  
| ***Audiencia*** | La oferta debe orientarse a profesionales de TI, desarrolladores de nube u otros roles de cliente técnicos. | list<br />transacción<br />evaluación |  
| ***Administración de clientes potenciales*** | Habilite el sistema de CRM (Marketo, Microsoft Dynamics o Salesforce) para aceptar datos de clientes potenciales antes de recibir clientes potenciales del escaparate. | list<br />transacción<br />evaluación |  
| ***Directiva de privacidad y términos de uso*** | La aplicación debe proporcionar un vínculo a la página de la directiva de privacidad mediante una dirección URL pública. Los términos de uso deben especificarse como texto durante la publicación. | list<br />transacción<br />evaluación |  
| ***Soporte técnico*** | La oferta debe proporcionar un vínculo a la página de asistencia al cliente mediante una dirección URL pública. Si la oferta es una versión de evaluación, debe proporcionar soporte técnico sin costo adicional durante el período de evaluación. | transacción<br />evaluación |    

## <a name="non-transact-listings"></a>Publicaciones que no son de tipo transacción  
En esta sección se describen todos los tipos de ofertas que no utilizan el tipo de publicación de transacción. 

### <a name="list"></a>List  
El tipo de publicación de anuncio incluye los siguientes tipos de oferta en los escaparates del Marketplace.  

| Tipo de oferta | Escaparate | Detalles |  
|:---        |:---        |:---     |  
| Servicios de consultoría | AppSource | Requisitos: AppSource: Lista: Servicios de consultoría |  
| Servicios de consultoría | Azure Marketplace | Requisitos: Azure Marketplace: Lista: Servicios de consultoría |  
| Ponerse en contacto conmigo | AppSource | [](#) |  
| Ponerse en contacto conmigo | Azure Marketplace | Requisitos: AppSource: Lista: Ponerse en contacto conmigo |  

#### <a name="requirements-appsource-list-consulting-service"></a>Requisitos: AppSource: Lista: Servicios de consultoría  

| Requisitos | Detalles |  
|:--- |:--- |  
| Características de la oferta de servicios | El servicio de consultoría debe cumplir los siguientes criterios.<ul> <li>Entregarse como un compromiso de ámbito fijo, duración fija y precio fijo o gratuito.</li> <li>Orientarse principalmente a preventas.</li> <li>Limitarse a un único cliente.</li> <li>Ejecutarse in situ.</li> </ul> |  
| Requisitos de asociados para servicios de consultoría | Debe cumplir los criterios en el área pertinente del servicio.<table><tr><th>Área de solución</th><th>Criterios</th></tr><tr><td>Dynamics 365 for Customer Engagement</td><td>Tener competencia Silver o Gold de Administración de las relaciones con el cliente.</td></tr><tr><td>Dynamics 365 for Finance and Operations, Enterprise Edition</td><td>Tener competencia Silver o Gold de planificación de recursos y obtener ingresos de las operaciones en la nube en los doce últimos meses por un valor de 25 000 USD o más.</td></tr><tr><td>Dynamics 365 for Finance and Operations, Business Edition</td><td>Actuar como proveedor de servicios en la nube (CSP) o asociado de registro digital (DPOR) para uno o varios clientes.</td></tr><tr><td>Power BI</td><td>Cumplir los criterios de los asociados de soluciones.</td></tr><tr><td>PowerApps</td><td>Tener una solución Presentación de asociado.</td></tr></table><ul> <li>Para más información sobre la administración de las relaciones con el cliente, visite la página de Administración de relaciones con el cliente en la nube en [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency).</li> <li>Para más información sobre el planeamiento de recursos, visite la página de planeamiento de recursos empresariales en [partner.microsoft.com/membership/enterprise-resource-planning-competency](https://partner.microsoft.com/membership/enterprise-resource-planning-competency).</li> <li>Para más información sobre CSP, visite la página Proveedor de soluciones en la nube en [partner.microsoft.com/cloud-solution-provider](https://partner.microsoft.com/cloud-solution-provider).</li> <li>Para más información sobre DPOR, visite la página Socio de registro digital y asociación del socio en [partner.microsoft.com/membership/digital-partner-of-record](https://partner.microsoft.com/membership/digital-partner-of-record).</li> <li>Para más información sobre los criterios de asociados de soluciones, visite el documento de información general sobre los asociados de soluciones y los incentivos que encontrará en [https://partner.microsoft.com/en-us/membership/partner-incentives](https://partner.microsoft.com/en-us/membership/partner-incentives).</li> <li>Para más información sobre la presentación de asociados, visite la página Presentación de asociado en [powerapps.microsoft.com/partner-showcase](https://powerapps.microsoft.com/partner-showcase).</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Requisitos: Azure Marketplace: Lista: Servicios de consultoría  

| Requisitos | Detalles |  
|:--- |:--- |  
| Características de la oferta de servicios | El servicio de consultoría debe cumplir los siguientes criterios.<ul> <li>Entregarse como un compromiso de ámbito fijo, duración fija y precio fijo o gratuito.</li> <li>Orientarse principalmente a preventas.</li> <li>Limitarse a un único cliente.</li> <li>Ejecutarse in situ.</li> </ul> |  
| Requisitos de asociados para servicios de consultoría | Debe tener una competencia Silver o Gold en alguna de las siguientes competencias en el área pertinente de su servicio. <table><tr><th>Área de solución</th><th>Competencia</th></tr><td>Plataforma e infraestructura de nube</td><td>Plataforma de nube<br />Centro de datos</td><tr><td>Desarrollo de aplicaciones e ISV</td><td>Desarrollo de aplicaciones<br />Integración de aplicaciones<br />DevOps</td></tr><tr><td>Administración de datos y análisis</td><td>Análisis de datos<br />Plataforma de datos</td></tr></table><ul> <li>Para más información sobre las competencias, visite la página sobre competencias a través de Microsoft Partner Network disponible en [partner.microsoft.com/membership/competencies](https://partner.microsoft.com/membership/competencies).</li> <li>Para más información sobre la publicación de anuncios, visite la página Servicios de consultoría de Azure Marketplace en [docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services).</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---

### <a name="trial"></a>Versión de prueba  

| Tipo de oferta | Escaparate | Detalles |  
|:---        |:---        |:---     |  
| Versión de evaluación gratuita/SaaS | AppSource | Requisitos por tipo de publicación: Versión de prueba |  
| Versión de evaluación gratuita/SaaS | Azure Marketplace | Requisitos: Azure Marketplace: versión de evaluación: Versión de evaluación gratuita/SaaS |  
| Demostración interactiva | AppSource | Requisitos por tipo de publicación: Versión de prueba |  
| Demostración interactiva | Azure Marketplace | [Requisitos: Azure Marketplace: versión de evaluación: Demostración interactiva](#requirements-azure-marketplace-trial-interactive-demo) |  
| Versión de prueba | AppSource | Requisitos por tipo de publicación: Versión de prueba |  
| Versión de prueba | Azure Marketplace | [Requisitos: Azure Marketplace: versión de evaluación: Versión de prueba](#requirements-azure-marketplace-trial-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Requisitos: Azure Marketplace: Versión de prueba  

| Requisito | Detalles |  
|:--- |:--- |  
| Período de evaluación gratuita y experiencia de evaluación | El cliente puede usar la aplicación de forma gratuita durante un tiempo limitado.<br /><br />El cliente no tiene que pagar ninguna tarifa de licencia o suscripción por la oferta o aplicación. El cliente no tiene que pagar por los servicios o productos de Microsoft subyacentes. Todas las opciones de evaluación se implementan en la suscripción a Azure. Dispone de una versión de evaluación que le permite ejercer el control exclusivo de la administración y optimización de costos.<br /><br />Puede elegir una evaluación gratuita, una demostración interactiva o una versión de prueba. Con independencia de lo que elija, la evaluación gratuita debe ofrecer al cliente una cantidad de tiempo establecida previamente para probar la aplicación sin ningún costo adicional.<ul> <li>Para iniciar el proceso de creación de una versión limitada de prueba, envíe un correo electrónico a [amp-testdrive@microsoft.com](mailto:amp-testdrive@microsoft.com).</li> </ul>Nota: Las experiencias de evaluación de SaaS de Azure Marketplace deben permitir a los clientes usar credenciales profesionales para iniciar sesión.<ul> <li>Para más información, visite la sección de experiencias de prueba de AppSource en [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences).</li> </ul> |  
| Solución de fácil configuración y lista para usar | La aplicación debe ser fácil y rápida de configurar e instalar. |  
| Disponibilidad/tiempo de actividad | La aplicación o plataforma SaaS debe tener un tiempo de actividad de al menos un 99,9 %. |  
| Azure Active Directory | La oferta debe permitir el inicio de sesión único (SSO) federado de Azure Active Directory (Azure AD) con consentimiento habilitado. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Requisitos: Azure Marketplace: versión de evaluación: Versión de evaluación gratuita/SaaS  

| Ventaja | Requisito |  
|:--- |:--- |  
| Permite que un cliente pruebe el producto antes de comprar con un método automático para pasar al uso de pago. También permite pruebas de concepto para la participación del cliente y de forma conjunta con los equipos de ventas de Microsoft. | La solución es una máquina virtual o una plantilla de solución.<br /><br />La solución es una oferta SaaS y se ofrece un producto SaaS multiinquilino.<br /><br />Tiene una experiencia de primera ejecución para conseguir que un cliente se ponga en marcha rápidamente.<br /><br />Tiene un único inquilino, pero va a agregar clientes como usuarios invitados. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Requisitos: Azure Marketplace: versión de evaluación: Demostración interactiva  

| Ventaja | Requisito |  
|:--- |:--- |  
| Permite a los clientes ver la solución en acción sin la complejidad de la configuración. | La solución requiere una configuración compleja que resultaría difícil de conseguir durante el período de prueba. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Requisitos: Azure Marketplace: versión de evaluación: Versión de prueba  

| Ventaja | Requisito |  
|:--- |:--- |  
| Permite que un cliente pruebe el producto antes de comprarlo.<br /><br />Proporciona una experiencia guiada de la solución con opciones preconfiguradas.<br /><br />A continuación, se indican las ventajas adicionales que proporciona el uso de una versión limitada de prueba.<ul> <li>Los usuarios afinan el 27 % de las búsquedas en Marketplace para mostrar únicamente las ofertas con versión de prueba.</li> <li>Las ofertas con versión de prueba generan un 38 % más clientes potenciales que las ofertas sin ella.</li> <li>El 36 % de las adquisiciones de nuevos clientes en Marketplace procede de clientes que realizan esta versión de prueba.</li> <li>Las versiones de prueba permiten que los vendedores de campo de Microsoft comprendan mejor el producto de cara a los trabajos de venta conjunta.</li> </ul> | La solución es una máquina virtual, una plantilla de solución o una aplicación SaaS con un solo inquilino, o resulta complicado aprovisionarla. <br /><br />No tiene un método para convertir la versión de prueba a una oferta de pago. |  

---

## <a name="transact-specific-listings"></a>Aplicaciones específicas de transacción

### <a name="transact"></a>Transacción  

| Tipo de oferta | Escaparate | Detalles |   
|:---        |:---        | :--- |  
| Aplicaciones de Azure: aplicación administrada | Azure Marketplace | Requisitos: Azure Marketplace: Transacción: Aplicaciones de Azure: aplicación administrada |  
| Aplicaciones de Azure: Plantilla de solución | Azure Marketplace | Requisitos: Azure Marketplace: Transacción: Aplicaciones de Azure: Plantilla de solución |  
| Contenedores | Azure Marketplace | [Requisitos: Azure Marketplace: Transacción: Contenedor](#requirements-azure-marketplace-transact-container) |  
| Aplicación SaaS  | Azure Marketplace | [Requisitos: Azure Marketplace: Transacción: Aplicación SaaS](#requirements-azure-marketplace-transact-saas-app) |  
| Máquina virtual | Azure Marketplace | [Requisitos: Azure Marketplace: Transacción: Máquina virtual](#requirements-azure-marketplace-transact-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Requisitos: Azure Marketplace: Transacción: Contenedor  

| Requisito | Detalles |  
|:--- |:--- |  
| Facturación y medición | Admita el modelo de facturación gratuito o traiga su propia licencia (BYOL). |  
| Imagen basada en Docker | Su imagen de contenedor debe basarse en el formato de imagen de Docker y deben extraerse de los registros de contenedor de Azure. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Requisitos: Azure Marketplace: Transacción: Aplicación SaaS  

| Requisito | Detalles |  
|:--- |:--- |  
| Facturación y medición | El precio de su oferta se fija en una tarifa plana mensual. Los precios y las opciones *true-up* basados en uso no se admiten en este momento. |  
| Cancelación | El cliente puede cancelar su oferta en cualquier momento. |  
| Página de aterrizaje de transacción | Hospede una página de aterrizaje de transacción con marca compartida de Azure. La página de aterrizaje permite a los clientes crear y administrar su cuenta de servicio de SaaS. |  
| API de suscripción de software como servicio | Proporcione un servicio que interactúe con la suscripción SaaS para crear, actualizar y eliminar una cuenta de usuario y un plan de servicio. Todos los cambios críticos en la API deben admitirse dentro de un plazo de 24 horas. Los cambios no críticos en la API se actualizarán de forma periódica. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Requisitos: Azure Marketplace: Transacción: Máquina virtual  

| Requisito | Detalles |  
|:--- |:--- | 
| Facturación y medición | Su máquina virtual debe admitir las opciones de facturación mensual BYOL o de pago por uso. |  
| Disco duro virtual (VHD) compatible con Azure | Las máquinas virtuales deben estar basadas en Windows o Linux.<ul> <li>Para obtener más información acerca de cómo crear un VHD de Linux, consulte [Distribuciones de Linux aprobadas en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Para más información acerca de la creación de un VHD de Windows, consulte [Creación de un disco duro virtual compatible con Azure](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |  

## <a name="next-steps"></a>Pasos siguientes
*   Visite la página [Guía del anunciante de Azure Marketplace y AppSource](./marketplace-publishers-guide.md).  

