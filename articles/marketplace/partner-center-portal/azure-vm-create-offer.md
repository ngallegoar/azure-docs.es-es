---
title: Creación de una oferta de máquina virtual de Azure en Azure Marketplace
description: Aprenda a crear una oferta de máquina virtual en Azure Marketplace con la SKU necesaria.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 31b8960f5617566a72545510cf03771f7a3bfcbd
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82745005"
---
# <a name="create-an-azure-virtual-machine-offer-in-the-azure-marketplace"></a>Creación de una oferta de máquina virtual de Azure en Azure Marketplace

> [!IMPORTANT]
> Estamos trasladando la administración de las ofertas de máquina virtual de Azure de Cloud Partner Portal al Centro de partners. Hasta que se migren las ofertas, siga las instrucciones de [Creación de una oferta de máquina virtual](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-offer) en Cloud Partner Portal para administrar las ofertas.

En este artículo se explica cómo crear y publicar una oferta de máquina virtual de Azure para [Azure Marketplace](https://azuremarketplace.microsoft.com/). Se dirige a máquinas virtuales basadas en Windows y Linux que contienen un sistema operativo, un disco duro virtual (VHD) y hasta 16 discos de datos. Antes de comenzar, debe [crear una cuenta de marketplace comercial en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account), si aún no lo ha hecho. Asegúrese de que la cuenta está inscrita en el programa marketplace comercial.

## <a name="introduction"></a>Introducción

### <a name="publishing-benefits"></a>Ventajas de publicación

La publicación en Azure Marketplace ofrece las siguientes ventajas:

- Promocionar la empresa con la marca de Microsoft
- Llegar a más de 100 millones de usuarios de Office 365 y Dynamics 365 y a más de 200 000 organizaciones en Azure Marketplace.
- Obtener clientes potenciales de alta calidad de estos marketplaces
- Promocionar sus servicios por parte de los equipos de campo y televenta de Microsoft

### <a name="before-you-begin"></a>Antes de empezar

Si todavía no lo ha hecho, consulte [Guía de publicación de ofertas de máquina virtual](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines) y el material sobre máquinas virtuales de Azure:

- Guías de inicio rápido
  - [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/)
  - [Plantillas de inicio rápido de Azure en GitHub](https://github.com/azure/azure-quickstart-templates)
- Tutoriales
  - [Máquinas virtuales Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
  - [Máquinas virtuales Windows](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- Ejemplos
  - [Ejemplos de la CLI de Azure para máquinas virtuales Linux](https://docs.microsoft.com/azure/virtual-machines/linux/cli-samples)
  - [Azure PowerShell para máquinas virtuales Linux](https://docs.microsoft.com/azure/virtual-machines/linux/powershell-samples)
  - [Ejemplos de la CLI de Azure para máquinas virtuales Windows](https://docs.microsoft.com/azure/virtual-machines/windows/cli-samples)
  - [Azure PowerShell para máquinas virtuales Windows](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-quick)

### <a name="fundamentals-in-technical-knowledge"></a>Aspectos básicos de los conocimientos técnicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta.

El equipo de ingeniería debe tener conocimientos sobre las siguientes tecnologías de Microsoft:

- Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/).
- Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
- Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage#storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking#networking).

## <a name="create-a-new-offer"></a>Crear una nueva oferta

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
2. En el menú de navegación izquierdo, seleccione **Marketplace comercial** > **Información general**.
3. En la página Información general, seleccione **+ Nueva oferta** > **Máquina virtual de Azure**.

    ![Muestra el menú de navegación izquierdo.](./media/new-offer-azure-vm.png)

> [!NOTE]
> Después de publicar una oferta, las modificaciones que se realicen en el Centro de partners solo aparecen en los escaparates cuando la oferta se vuelve a publicar. Asegúrese de volver a realizar la publicación siempre después de realizar cambios.

## <a name="new-offer"></a>Nueva oferta

Escriba un **Identificador de oferta**. Se trata de un identificador único para cada oferta de su cuenta.

- Este identificador se muestra a los clientes en la dirección web de la oferta del marketplace y en Azure PowerShell y la CLI de Azure, si procede.
- Use solo letras minúsculas y números. Puede incluir guiones y caracteres de subrayado, pero no espacios, y está limitado a 50 caracteres. Por ejemplo, si escribe **test-offer-1**, la dirección web de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- El identificador de oferta no se puede cambiar después de seleccionar **Crear**.

Escriba un **Alias de la oferta**. Este es el nombre que se usa para la oferta en el Centro de partners.

- Este nombre no se usa en Marketplace y es diferente del nombre de la oferta y de otros valores que se muestran a los clientes.

Seleccione **Crear** para generar la oferta y continuar.

## <a name="offer-setup"></a>Configuración de la oferta

### <a name="test-drive"></a>Versión de prueba

Una versión de prueba es una excelente manera de presentar la oferta a clientes potenciales, ya que se les brinda la oportunidad de "probarla antes de comprarla", lo que aumenta la conversión y la generación de clientes potenciales altamente cualificados. [Más información acerca de las versiones de prueba](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Para habilitar una versión de prueba durante un período de tiempo fijo, seleccione la casilla **Habilitar una versión de prueba**. Para eliminar la versión de prueba de la oferta, desactive esta casilla.

Recursos adicionales de la versión de prueba:

- [Procedimientos técnicos recomendados](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Procedimientos recomendados de marketing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- PDF de [Introducción a las versiones de prueba](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (asegúrese de que el bloqueador de elementos emergentes está desactivado)

### <a name="lead-management"></a>Administración de clientes potenciales

Al publicar la oferta en el marketplace comercial con el Centro de partners, conéctela a un sistema de administración de relaciones con clientes (CRM). Esto le permite recibir información de contacto del cliente en cuanto alguien expresa interés en el producto o lo usa. La conexión a un CRM es necesaria si va a habilitar la opción **Versión de prueba** (consulte la sección anterior), de lo contrario es opcional.

1. Seleccione un destino de clientes potenciales al que quiera que enviemos los clientes potenciales. El Centro de partners admite los siguientes sistemas CRM:
    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) para la involucración del cliente
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Si su sistema CRM no aparece en la lista anterior, use [Tabla de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) o [Punto de conexión HTTPS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) para almacenar los datos del cliente potencial. A continuación, exporte los datos al sistema CRM.

2. Conecte su oferta al destino de clientes potenciales cuando la publique en el Centro de partners.
3. Compruebe que la conexión al destino de clientes potenciales está configurada correctamente. Después de publicarla en el Centro de partners, se valida la conexión y se le envía un cliente potencial de prueba. Mientras obtiene una vista previa de la oferta antes de publicarla, también puede probar la conexión de los clientes potenciales tratando de implementar la oferta en el entorno de versión preliminar.
4. Asegúrese de que la conexión con el destino de clientes potenciales permanece actualizada para no perder ningún cliente potencial.

Seleccione **Guardar borrador** antes de continuar.

## <a name="properties"></a>Propiedades

Esta página permite definir las categorías y los sectores que se usan para agrupar su oferta en Marketplace, la versión de la aplicación y los contratos legales que respaldan su oferta.

### <a name="categories"></a>Categorías

Seleccione un mínimo de una y un máximo de cinco categorías. Estas categorías se usan para colocar la oferta en las áreas de búsqueda adecuadas del marketplace. En la descripción de la oferta, indique la forma en que la oferta admite estas categorías. Las ofertas de máquinas virtuales aparecen en la categoría **Proceso** de Azure Marketplace.

### <a name="legal"></a>Información legal

Debe proporcionar los términos y condiciones de la oferta. Tiene dos opciones:

- Uso de términos y condiciones propios
- Uso del Contrato estándar para marketplace comercial de Microsoft

#### <a name="use-your-own-terms-and-conditions"></a>Uso de términos y condiciones propios

Si quiere proporcionar sus propios términos y condiciones personalizados, escriba hasta 10 000 caracteres de texto en el cuadro **Términos y condiciones**. Si los términos y condiciones requieren una descripción más larga, escriba una única dirección web en la que se puedan encontrar. A los clientes les aparecerá como un vínculo activo.

Los clientes deben aceptar estos términos para poder probar la oferta.

#### <a name="use-the-standard-contract-for-the-microsoft-commercial-marketplace"></a>Uso del Contrato estándar para marketplace comercial de Microsoft

Para simplificar el proceso de adquisición para los clientes y reducir la complejidad legal para los proveedores de software, Microsoft ofrece un Contrato estándar para marketplace comercial. Al ofrecer el software bajo el Contrato estándar, los clientes solo tienen que leerlos y aceptarlos una vez y no tiene que crear términos y condiciones personalizados.

Para usar el Contrato estándar, seleccione la casilla **¿Quiere usar el contrato estándar para Marketplace comercial de Microsoft?** y, a continuación, seleccione **Aceptar** en la ventana emergente (es posible que tenga que desplazarse hacia arriba para verla).

![Muestra la página Información general del Centro de partners con el botón Nueva oferta y la oferta de servicio de consultoría seleccionada.](media/use-standard-contract.png)

> [!NOTE]
> Después de publicar una oferta con el Contrato estándar para marketplace comercial, no puede usar sus propios términos y condiciones personalizados. Puede ofrecer la solución con el Contrato estándar **o** con sus propios términos y condiciones.

Para más información sobre el contrato estándar, consulte Contrato estándar para [marketplace comercial](https://docs.microsoft.com/azure/marketplace/standard-contract) de Microsoft. Puede descargar el [Contrato estándar](https://go.microsoft.com/fwlink/?linkid=2041178) en formato PDF (asegúrese de que el bloqueador de elementos emergentes está desactivado).

##### <a name="standard-contract-amendments"></a>Modificaciones del Contrato estándar

Las modificaciones del Contrato estándar le permiten seleccionar los términos del Contrato estándar por motivos de simplicidad y crear los términos para su producto o empresa. Los clientes solo necesitan revisar las modificaciones del contrato, si ya han revisado y aceptado el Contrato estándar de Microsoft. Hay dos tipos de modificaciones disponibles: universal y personalizada.

**Modificaciones universales**: se aplican universalmente al Contrato estándar para todos los clientes. Se muestran a cada cliente de la oferta en el flujo de compra. Los clientes deben aceptar los términos del Contrato estándar y las modificaciones antes de que puedan usar su oferta. Puede proporcionar una única modificación universal por oferta. Puede escribir un número ilimitado de caracteres en este cuadro. Estos términos se muestran a los clientes en AppSource, Azure Marketplace o Azure Portal durante el flujo de detección y compra.

**Modificaciones personalizadas**: son modificaciones especiales del Contrato estándar que están destinadas a clientes específicos mediante identificadores de inquilino de Azure. Puede elegir el inquilino al que desea dirigirse. Solo los clientes del inquilino recibirán los términos con modificaciones personalizadas en el flujo de compra de la oferta. Los clientes deben aceptar los términos del Contrato estándar y las modificaciones antes de que puedan usar su oferta.

Seleccione **Agregar términos de modificación personalizados (10 como máximo)** . Puede proporcionar hasta diez términos de modificación personalizados por oferta.

- **Términos de modificación personalizados**: escriba sus propios términos de modificación en el cuadro Términos de modificación personalizados. Puede escribir un número ilimitado de caracteres. Solo los clientes de los identificadores de inquilino que se especifiquen para estos términos personalizados los verán en el flujo de compra de la oferta en Azure Portal.
- **Identificadores de inquilino** (obligatorio): cada modificación personalizada puede tener como destino hasta 20 identificadores de inquilino. Si agrega una modificación personalizada, debe proporcionar al menos un identificador de inquilino, que identifica al cliente en Azure. El cliente puede encontrarlo en Azure, en Propiedades. El valor del identificador de directorio es el identificador de inquilino (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). También puede buscar el identificador de inquilino de la organización del cliente mediante su dirección web de nombre de dominio en [What is my Microsoft Azure and Office 365 tenant ID?](https://www.whatismytenantid.com/) (¿Cuál es mi identificador de inquilino de Microsoft Azure y Office 365?).
- **Descripción** (opcional): proporcione una descripción detallada del identificador de inquilino que le ayude a identificar al cliente al que está destinada la modificación.

> [!NOTE]
> Estos dos tipos de modificaciones se superponen entre sí. Los clientes específicos con modificaciones personalizadas también obtendrán la modificación universal al Contrato estándar durante la compra.

Seleccione **Guardar borrador** antes de continuar.

## <a name="offer-listing"></a>Descripción de la oferta

Esta página le permite definir los detalles de la oferta, como el nombre de la oferta, la descripción, los vínculos y los contactos.

> [!NOTE]
> No es necesario que la oferta (por ejemplo, la descripción, los documentos, las capturas de pantallas y los términos de uso) esté en inglés si la descripción de la oferta comienza por la frase "Esta aplicación solo está disponible en [idioma distinto del inglés]". También puede proporcionar una _dirección web de vínculo de utilidad_ para ofrecer contenido en un idioma distinto del que se usa en el contenido del anuncio de la oferta.

### <a name="marketplace-details"></a>Detalles del marketplace

#### <a name="name"></a>Nombre

El nombre que escriba aquí se muestra a los clientes como el título de la descripción de la oferta. Este campo se rellena previamente con el texto que escribió en el cuadro **Alias de la oferta** al crear la oferta. Puede cambiar este nombre más adelante.

El nombre:

- Puede contener marcas comerciales (y puede incluir símbolos de marca comercial y copyright)
- No puede tener más de 50 caracteres
- No puede incluir emojis.

#### <a name="search-results-summary"></a>Resumen de los resultados de la búsqueda

Breve descripción de la oferta. Puede tener una longitud de hasta 100 caracteres y se usa en los resultados de la búsqueda de Marketplace.

#### <a name="long-summary"></a>Resumen largo

Proporcione una descripción más larga de la oferta. Puede tener una longitud de hasta 256 caracteres y se usa en los resultados de la búsqueda de Marketplace.

#### <a name="description"></a>Descripción

Proporcione una descripción detallada de la oferta, hasta 3 000 caracteres. Se muestra a los clientes en la información general de la lista del marketplace comercial.

Incluya uno o varios de los siguientes elementos en la descripción:

- Valor y beneficios principales de la oferta
- Asociaciones de categoría o de sector o ambas
- Oportunidades de compra desde la aplicación
- Cualquier divulgación necesaria

Estas son algunas sugerencias para escribir la descripción:

- Describa claramente la propuesta de valor de la oferta en las primeras frases de su descripción. Incluya los siguientes elementos:
  - Descripción de la oferta.
  - Tipo de usuario que se beneficia de la oferta.
  - Necesidades o problemas del cliente que resuelve la oferta.
- Recuerde que las primeras frases podrían mostrarse en los resultados del motor de búsqueda.
- No se base exclusivamente en las características y funcionalidades para vender la oferta. En su lugar, céntrese en el valor que proporciona la oferta.
- Use palabras específicas del sector o basadas en las ventajas.

Para que la descripción de la oferta sea más atractiva, use el editor de texto enriquecido para darle formato.

![Uso del editor de texto enriquecido](./media/rich-text-editor.png)

| <center>Cambiar el formato de texto | <center>Agregar viñetas o numeración | <center>Agregar o quitar sangría de texto |
| --- | --- | --- |
| <center>![Uso del editor de texto enriquecido para cambiar el formato del texto](./media/text-editor3.png) |  <center>![Uso del editor de texto enriquecido para agregar listas](./media/text-editor4.png) |  <center>![Uso del editor de texto enriquecido para aplicar sangría](./media/text-editor5.png) |

#### <a name="privacy-policy-link"></a>Vínculo de la directiva de privacidad

Escriba la dirección web (URL) de la directiva de privacidad de la organización. Asegúrese de que la oferta cumple con las leyes y regulaciones de privacidad. También debe publicar una directiva de privacidad válida en su sitio web.

### <a name="useful-links"></a>Vínculos útiles

Proporcione documentos en línea complementarios sobre su oferta. Para agregar un vínculo, seleccione **+ Agregar un vínculo** y, a continuación, rellene los campos siguientes:

- **Nombre**: los clientes verán el nombre en la página de detalles.
- **Vínculo (URL)** : escriba un vínculo para que los clientes vean el documento en línea.

### <a name="customer-support-links"></a>Vínculos de soporte al cliente

Proporcione el sitio web de soporte técnico en el que los clientes pueden ponerse en contacto con el equipo de soporte técnico.

- Sitio web de soporte técnico global de Azure
- Sitio web de soporte técnico de Azure Government

### <a name="partner-support-contact"></a>Contacto de soporte técnico del asociado

Proporcione información de contacto para que los asociados de Microsoft la usen cuando los clientes abran una incidencia de soporte técnico. No se mostrará en el marketplace.

- Nombre
- Email
- Teléfono

### <a name="engineering-contact"></a>Contacto de ingeniería

Proporcione información de contacto para que Microsoft la use en caso de problemas con la oferta, incluidos los problemas de certificación. No se mostrará en el marketplace.

- Nombre
- Email
- Teléfono

### <a name="marketplace-media"></a>Elementos multimedia del marketplace

Proporcione logotipos e imágenes para usarlos con la oferta. Todas las imágenes deben estar en formato PNG. Las imágenes borrosas harán que se rechace el envío.

>[!Note]
>Si tiene un problema al cargar archivos, asegúrese de que la red local no bloquee el servicio https://upload.xboxlive.com que usa el Centro de partners.

#### <a name="marketplace-logos"></a>Logotipos de Marketplace

Proporcione archivos PNG del logotipo de la oferta en los cuatro tamaños de píxel siguientes:

- **Pequeño (48 x 48)**
- **Medio (90 x 90)**
- **Grande (216 x 216)**
- **Ancho (255 x 115)**

Los cuatro logotipos son necesarios y se usan en lugares diferentes de la lista de Marketplace.

#### <a name="screenshots"></a>Capturas de pantalla

Agregue hasta cinco capturas de pantalla que muestren el funcionamiento de la oferta. Cada captura de pantalla debe tener un tamaño de 1280 x 720 píxeles y el formato PNG. Cada captura de pantalla debe incluir un título.

#### <a name="videos"></a>Vídeos

Agregue hasta cinco vídeos que muestren la oferta. Se deben hospedar en un servicio de vídeo externo. Escriba el nombre de cada vídeo, su dirección web y una imagen en miniatura en formato PNG del vídeo de 1280 x 720 píxeles.

Para ver más recursos sobre las listas de marketplace, consulte [Procedimientos recomendados para las listas de ofertas de marketplace](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

Seleccione **Guardar borrador** antes de continuar.

## <a name="preview"></a>Vista previa

En la pestaña Versión preliminar, elija un **público preliminar** limitado para validar la oferta antes de ponerla a disposición de un público más amplio del marketplace.

> [!IMPORTANT]
> Después de revisar la oferta en versión preliminar, seleccione **Publicar** para incluir la oferta como pública en el marketplace comercial.

Los GUID de identificador de suscripción de Azure, junto con una descripción adicional de cada uno, identifican el público preliminar. Los clientes no pueden ver ninguno de estos campos. Puede encontrar el identificador de suscripción de Azure en la página **Suscripciones** de Azure Portal.

Agregue al menos un id. de suscripción de Azure, ya sea de forma individual (hasta 10) o mediante la carga de un archivo CSV (hasta 100). Al agregar estos identificadores de suscripción, puede definir quién puede obtener una vista previa de la oferta antes de publicarla. Si la oferta ya está publicada, todavía puede definir un público preliminar para probar los cambios o las actualizaciones de la oferta.

> [!NOTE]
> Un público preliminar no es el mismo que un público privado. A una audiencia preliminar se le permite acceder a la oferta _antes_ de que se publique en el marketplace. Podrá ver y validar todos los planes, incluidos aquellos que estarán disponibles solo para un público privado después de que su oferta se publique completamente en el marketplace. Un público privado (definido en la pestaña **Precios y disponibilidad** del plan) tiene acceso exclusivo a un plan determinado.

Seleccione **Guardar borrador** antes de continuar con la siguiente sección, Información general del plan.

## <a name="plan-overview"></a>Información general del plan

Puede proporcionar diversas opciones de planes dentro de la misma oferta en el Centro de partners. Estos planes se llamaban anteriormente SKU. Una oferta requiere al menos un plan, que puede diferir en términos de audiencia de monetización, regiones de Azure, características o imágenes de VM.

Después de crear sus planes, la pestaña **Información general del plan** muestra lo siguiente:

- Nombres de los planes
- Modelos de licencia
- Audiencia (pública o privada)
- Estado de publicación actual
- Acciones disponibles

Las acciones disponibles en Información general del plan varían en función del estado actual del plan. Incluyen:

- **Eliminar borrador**: si el estado del plan es un borrador
- **Detener el plan de venta** o **Sincronizar audiencia privada**: si el estado del plan es publicado

### <a name="create-new-plan"></a>Creación de un nuevo plan

Seleccione **+ Crear nuevo plan** en la parte superior. Aparece el cuadro de diálogo **Nuevo plan**.

En el cuadro **Id. de plan**, cree un identificador de plan único para cada plan de esta oferta. Este identificador será visible para los clientes en la dirección web del producto. Use solo letras minúsculas y números, guiones o caracteres de subrayado, y un máximo de 50 caracteres.

> [!NOTE]
> El id. de oferta no se puede cambiar después de seleccionar **Crear**.

En el cuadro **Nombre del plan**, escriba un nombre para este plan. Los clientes verán este nombre al decidir que plan van a seleccionar en su oferta. Cree un nombre único que destaque claramente las diferencias de cada plan. Por ejemplo, puede usar **Windows Server** con los planes **Pago por uso**, **BYOL**, **Avanzado** y **Enterprise**.

Seleccione **Crear**.

### <a name="plan-setup"></a>Configuración del plan

Establezca la configuración de alto nivel para el tipo de plan, si reutiliza la configuración técnica de otro plan y en qué regiones de Azure debe estar disponible el plan. Las selecciones que se realicen aquí afectarán a los campos mostrados en otras pestañas para el mismo plan.

#### <a name="re-use-technical-configuration"></a>Reutilización de la configuración técnica

Si tiene más de un plan del mismo tipo y los paquetes son idénticos entre ellos, puede seleccionar **este plan reutiliza la configuración técnica de otro plan**. Esta opción le permite seleccionar uno de los otros planes del mismo tipo para esta oferta y reutilizar su configuración técnica.

> [!NOTE]
> Cuando reutilice la configuración técnica de otro plan, toda la pestaña **Configuración técnica** desaparecerá de este plan. Los detalles de configuración técnica del otro plan, incluidas las actualizaciones que realice en el futuro, se usarán también para este plan. Esta configuración no se puede modificar una vez publicado este plan.

#### <a name="azure-regions"></a>Regiones de Azure

El plan debe estar disponible al menos en una región de Azure.

Seleccione la opción **Azure global** para que el plan esté disponible para los clientes de todas las regiones globales de Azure que tengan la integración de marketplace comercial. Para obtener más información, consulte [Disponibilidad geográfica y soporte técnico de moneda](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Seleccione la opción **Azure Government** para que el plan esté disponible en la región de [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome). Esta región proporciona acceso controlado para los clientes de entidades tribales, locales, estatales o federales de Estados Unidos, así como para los asociados aptos para abastecerlas. Como publicador, es responsable de los controles de cumplimiento, las medidas de seguridad y los procedimientos recomendados. Azure Government usa redes y centros de datos aislados físicamente (ubicados solo en Estados Unidos).

Antes de publicar en [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), pruebe y valide el plan en el entorno, ya que algunos puntos de conexión pueden ser diferentes. Para configurar y probar el plan, solicite una cuenta de prueba desde [Prueba de Microsoft Azure Government](https://azure.microsoft.com/global-infrastructure/government/request/).

> [!NOTE]
> Una vez que el plan esté publicado y disponible en una región de Azure específica, no se puede quitar esa región.

#### <a name="azure-government-certifications"></a>Certificaciones de Azure Government

Esta opción solo está visible si ha seleccionado **Azure Government**.

Los servicios de Azure Government controlan datos que están sujetos a determinados reglamentos y requisitos gubernamentales. Por ejemplo, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 y CJIS. Para dar a conocer sus certificaciones de estos programas, puede proporcionar hasta 100 vínculos que las describan. Pueden ser vínculos a su anuncio en el programa directamente o a descripciones de su cumplimiento de estos en sus propios sitios web. Estos vínculos solo son visibles para los clientes de Azure Government.

Seleccione **Guardar borrador** antes de continuar.

### <a name="plan-listing"></a>Lista del plan

Aquí es donde se configuran los detalles de la lista del plan. Esta pestaña muestra información específica que puede variar entre los planes de la misma oferta.

#### <a name="plan-name"></a>Nombre del plan

Se rellena previamente con el nombre que asignó al plan cuando lo creó. Este nombre aparece en el marketplace como el título de este plan y está limitado a 100 caracteres.

#### <a name="plan-summary"></a>Resumen del plan

Proporcione un breve resumen del plan (no de la oferta). Este resumen se limita a 100 caracteres.

#### <a name="plan-description"></a>Description del plan

Describa lo que hace que este plan sea único, así como las diferencias entre los planes de la oferta. No describa la oferta, solo el plan. La descripción del plan puede contener hasta 2 000 caracteres.

Seleccione **Guardar borrador** antes de continuar.

### <a name="pricing-and-availability"></a>Precios y disponibilidad

En esta pestaña, configurará lo siguiente:

- Mercados en los que estará disponible este plan
- Precio por hora
- Si desea que el plan sea visible a todos los usuarios o solo a clientes específicos (una audiencia privada)

#### <a name="markets"></a>Mercados

Todos los planes debe estar disponible al menos en un mercado. Seleccione la casilla de cada ubicación de mercado en la que este plan debe estar disponible para su compra (los usuarios de estos mercados pueden implementar la oferta en todas las regiones de Azure seleccionadas en **[Configuración del plan](#plan-setup)** ). El botón **Impuestos remitidos** muestra los países en los que Microsoft lleva a cabo las ventas y la retención de impuestos en su nombre. La publicación en China se limita a planes **Gratis** o **Traiga su propia licencia** (BYOL).

Si ya se han establecido los precios del plan en dólares de Estados Unidos (USD) y agrega otra ubicación de mercado, el precio del nuevo mercado se calculará en función de los tipos de cambio actuales. Revise siempre el precio de cada mercado antes de la publicación. Los precios se pueden consultar mediante el vínculo **Exportar precios (xlsx)** después de guardar los cambios.

Al eliminar un mercado, los clientes de ese mercado que usan implementaciones activas no podrán crear nuevas implementaciones ni escalar verticalmente las implementaciones existentes. Las implementaciones existentes no se verán afectadas.

#### <a name="pricing"></a>Precios

**Modelo de licencias**: seleccione **Plan de facturación mensual en función del uso** para configurar los precios de este plan o **Traiga su propia licencia** para que los clientes puedan usar este plan con su licencia existente.

Para un plan de facturación mensual en función del uso, utilice una de las tres opciones de entrada de precios siguientes:

- **Por núcleo**: proporcione el precio por núcleo en dólares de Estados Unidos (USD). Calcularemos los precios por tamaño de núcleo y los convertiremos a las monedas locales con la tasa de cambio actual.
- **Por tamaño de núcleo**: proporcione los precios por tamaño de núcleo en USD. Convertiremos los precios a las monedas locales con la tasa de cambio actual.
- **Por mercado y tamaño de núcleo**: proporcione los precios para cada tamaño de núcleo para todos los mercados. Puede importar los precios desde una hoja de cálculo.

> [!NOTE]
> Guarde los cambios de los precios para habilitar la exportación de los datos de precios. Después de que se publica un precio para un mercado en el plan, no se puede cambiar más adelante. Asegúrese de que los precios son correctos antes de publicarlos; para ello, exporte la hoja de cálculo de precios y revise el precio en cada mercado.

#### <a name="free-trial"></a>Evaluación gratuita

Puede ofrecer una evaluación gratuita de un mes o de tres meses a los clientes.

#### <a name="visibility"></a>Visibilidad

Puede diseñar cada plan para que este sea visible para todos los usuarios o solo para una audiencia preseleccionada. Asigne la pertenencia a este público restringido mediante los identificadores de suscripción de Azure.

**Público**: todos los usuarios pueden ver el plan.

**Audiencia privada**: haga que el plan sea visible solo para una audiencia preseleccionada. Una vez publicado como un plan privado, puede actualizar la audiencia o cambiarlo a público. Después de hacer que un plan sea público, debe permanecer público; no se puede volver a cambiar a privado.

**Audiencia restringida (identificadores de suscripción de Azure)** : asigne la audiencia que tendrá acceso a este plan privado mediante identificadores de suscripción de Azure. Opcionalmente, incluya una descripción de cada identificador de suscripción de Azure que haya asignado. Agregue hasta 10 identificadores de suscripción manualmente o hasta 20 000 si importa una hoja de cálculo CSV. Los identificadores de suscripción de Azure se representan como GUID y las letras deben estar en minúsculas.

> [!NOTE]
> Una audiencia privada o restringida es diferente del público preliminar que definió en la pestaña **Versión preliminar**. Una audiencia preliminar puede acceder a la oferta _antes_ de que se publique en el marketplace. En tanto que la designación de una audiencia privada solo se aplica a un plan específico, el público preliminar puede ver todos los planes (privados o no) con fines de validación.

#### <a name="hide-plan"></a>Ocultar plan

Si la máquina virtual está diseñada para implementarse solo indirectamente cuando se hace referencia a ella desde otra plantilla de solución o aplicación administrada, seleccione esta casilla para publicar la máquina virtual, pero ocultarla para los clientes que la buscan directamente.

> [!NOTE]
> Los planes ocultos no admiten vínculos de versión preliminar.

Seleccione **Guardar borrador** antes de continuar.

### <a name="technical-configuration"></a>Configuración técnica

Proporcione las imágenes y otras propiedades técnicas asociadas al plan. Para más información, consulte [Creación de un recurso técnico de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-technical-assets).

> [!NOTE]
> Esta pestaña no se muestra si ha configurado este plan para reutilizar paquetes de otro plan en la pestaña **Configuración del plan**.

#### <a name="operating-system"></a>Sistema operativo

- **Familia del sistema operativo**: seleccione entre el sistema operativo **Windows** o **Linux**
- **Versión** o **Proveedor**: elija la versión de Windows o el proveedor de Linux
- **Nombre descriptivo del sistema operativo**: elija un nombre descriptivo para el sistema operativo. Este nombre es visible para los clientes

#### <a name="recommended-vm-sizes"></a>Recommended VM Sizes (Tamaños de máquina virtual recomendados)

Seleccione hasta seis tamaños de máquina virtual recomendados para que se muestren en Azure Marketplace.

#### <a name="open-ports"></a>Abrir puertos

Puertos públicos o privados abiertos en una máquina virtual implementada.

#### <a name="storage-option-for-deployment"></a>Opción de almacenamiento para la implementación

**Opción de implementación de disco**: seleccione el tipo de implementación de disco que pueden usar los usuarios al usar la máquina virtual. Microsoft recomienda limitar la implementación solo a la implementación de discos administrados.

#### <a name="properties"></a>Propiedades

**Compatibilidad con redes aceleradas**: seleccione esta opción si la máquina virtual es compatible con [redes aceleradas](https://go.microsoft.com/fwlink/?linkid=2124513).

#### <a name="vm-images"></a>Imágenes de máquinas virtuales

Proporcione una versión de disco y el identificador URI de SAS para las imágenes de máquina virtual. Agregue hasta 16 discos de datos para cada imagen de máquina virtual. Proporcione solo una nueva versión de imagen por plan en un envío determinado. Una vez publicada una imagen no se puede editar, pero se puede eliminar. La eliminación de una versión impedirá que los usuarios nuevos y existentes implementen una nueva instancia de la versión eliminada.

- **Versión de disco** es la versión de la imagen que se va a proporcionar.
- **Identificador URI de SAS** es la ubicación en Azure Storage en la que ha almacenado el disco duro virtual del sistema operativo.
- Las imágenes del disco de datos también son identificadores URI de SAS de discos duros virtuales almacenados en su almacenamiento de Azure.
- Agregue solo una imagen por envío en un plan.

Independientemente del sistema operativo que use, agregue solo el número mínimo de discos de datos necesarios para la solución. Los usuarios no pueden eliminar los discos que forman parte de una imagen durante la implementación, pero siempre pueden agregar discos durante o después de la implementación.

Seleccione **Guardar borrador** antes de continuar y volver a **Información general del plan**.

## <a name="resell-through-csps"></a>Revender mediante los CSP

Amplíe el alcance de la oferta haciendo que esté disponible para los asociados en el programa [Proveedores de soluciones en la nube](https://azure.microsoft.com/offers/ms-azr-0145p/) (CSP). Todos los planes Traiga su propia licencia (BYOL) se incorporan automáticamente; puede optar por participar en los planes que no sean de BYOL.

Seleccione **Guardar borrador** antes de continuar.

## <a name="test-drive"></a>Versión de prueba

Configure una demostración (versión de prueba) que permita a los clientes probar la oferta antes de adquirirla. Para crear un entorno de demostración que permita a los clientes probar la oferta durante un período de tiempo determinado, consulte [Permitir a los clientes probar una versión de la oferta](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive).

Para habilitar una versión de prueba, seleccione la casilla Habilitar una versión de prueba en la pestaña [Configuración de la oferta](#test-drive). Para eliminar la versión de prueba de la oferta, desactive esta casilla.

Recursos adicionales de la versión de prueba:

- [Procedimientos recomendados de marketing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Procedimientos técnicos recomendados](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Información general](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF, asegúrese de que el bloqueador de elementos emergentes esté desactivado)

Seleccione **Guardar borrador** antes de continuar.

## <a name="review-and-publish"></a>Revisión y publicación

Una vez que haya completado todas las secciones necesarias de la oferta, puede enviarla para su revisión y publicación.

En la esquina superior derecha del portal, seleccione **Revisar y publicar**.

En esta página puede realizar las siguientes acciones:

- Ver el estado de finalización de cada sección de la oferta.
  - **No iniciada**: la sección no se ha iniciado y se debe completar.
  - **Incompleta**: la sección tiene errores que se deben corregir o se requiere que proporcione más información. Consulte las secciones anteriores de este documento para obtener instrucciones sobre cómo actualizar esta sección.
  - **Completa**: la sección está completa y no contiene errores. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- **Notas para la certificación**: proporcione instrucciones de prueba al equipo de certificación para asegurarse de que la aplicación se ha probado correctamente. Proporcione cualquier nota complementaria que resulte útil para entender la aplicación.

Para enviar la oferta para su publicación, seleccione **Revisar y publicar**.

Le enviaremos un correo electrónico para avisarle cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Para publicar la oferta al público (o, si es una oferta privada, a un público privado), vaya al Centro de partners, busque la página **Información general** de la oferta y seleccione **Publicar**. El estado de la oferta aparecerá en la parte superior de la página cuando la publicación esté en curso.

### <a name="errors-and-review-feedback"></a>Errores y comentarios de revisión

El paso **Validación manual** del proceso de publicación representa una revisión exhaustiva de la oferta y los recursos técnicos asociados. Si este proceso detecta errores en la oferta, recibirá un informe de certificación que detalla los problemas. Simplemente realice las correcciones necesarias y vuelva a publicar la oferta.

## <a name="offer-overview"></a>Información general de la oferta

En la página **Información general de la oferta** se muestra una representación visual de los pasos necesarios para publicar esta oferta (tanto completados como en curso), así como el tiempo necesario que debería tardar cada uno en completarse.

En esta página se incluyen vínculos para realizar operaciones en esta oferta en función de la selección que realice. Por ejemplo:

- Si la oferta es un borrador: [Eliminación de una oferta borrador](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Si la oferta está activa: [Detención de la venta de una oferta](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Si la oferta está en versión preliminar: [publicar](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Si no ha completado el cierre de sesión del publicador: [Cancelación de la publicación](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="marketplace-examples"></a>Ejemplos de marketplace

En estos ejemplos se muestra cómo aparece la oferta en Azure Marketplace.

### <a name="azure-marketplace-offer-details"></a>Detalles de la oferta de Azure Marketplace

![Ejemplo de pantalla de detalles de la oferta de Azure Marketplace](media/avm-create1.png)

### <a name="azure-marketplace-search-results"></a>Resultados de la búsqueda en Azure Marketplace

![Ejemplo de pantalla de detalles de la búsqueda en Azure Marketplace](media/avm-create2.png)

### <a name="azure-marketplace-plan-details"></a>Detalles del plan de Azure Marketplace

![Ejemplo de pantalla de detalles del plan de Azure Marketplace](media/avm-create3.png)

### <a name="azure-portal-offer-details"></a>Detalles de la oferta en Azure Portal

![Ejemplo de pantalla de detalles de la oferta en Azure Portal](media/avm-create4.png)

### <a name="azure-portal-search-results"></a>Resultados de la búsqueda de Azure Portal

![Ejemplo de pantalla de resultados de la búsqueda en Azure Portal](media/avm-create5.png)

### <a name="azure-portal-plan-details"></a>Detalles del plan en Azure Portal

![Ejemplo de pantalla de detalles del plan en Azure Portal](media/avm-create6.png)

## <a name="next-step"></a>Paso siguiente

- [Actualización de una oferta existente en Marketplace comercial](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
