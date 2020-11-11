---
title: Creación de planes para una oferta de máquina virtual en Azure Marketplace
description: Aprenda a crear planes para una oferta de máquina virtual en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: bc5e98484560fcc15e0ea3e289069c84687f158c
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93040585"
---
# <a name="how-to-create-plans-for-a-virtual-machine-offer"></a>Procedimientos para crear planes para una oferta de máquina virtual

Puede proporcionar una serie de opciones de planes dentro de la misma oferta en el Centro de partners. Una oferta requiere al menos un plan (anteriormente denominada una SKU), que puede variar en términos de audiencia de monetización, regiones de Azure, características o imágenes de máquina virtual.

Puede crear hasta 100 planes para cada oferta, de los cuales hasta 45 pueden ser privados. Más información sobre los planes privados en [Ofertas privadas en el marketplace comercial de Microsoft](private-offers.md).

Después de crear los planes, la pestaña **Plan overview** (Información general del plan) muestra lo siguiente:

- Nombres de los planes
- Modelos de licencia
- Audiencia (pública o privada)
- Estado de publicación actual
- Acciones disponibles

Las acciones que están disponibles en el panel **Plan overview** (Información general del plan) varían en función del estado actual del plan.

- Si el estado del plan es un borrador, seleccione **Eliminar borrador**.
- Si el estado del plan se publica en directo, seleccione **Stop sell plan** (Detener el plan de venta) o **Sync private audience** (Sincronizar audiencia privada).

## <a name="create-a-new-plan"></a>Creación de un nuevo plan

Seleccione **Crear nuevo plan** en la parte superior. Aparece el cuadro de diálogo **Nuevo plan**.

En el cuadro **Id. de plan** , cree un identificador de plan único para cada plan de esta oferta. Este identificador será visible para los clientes en la dirección web del producto. Use solo letras minúsculas y números, guiones o caracteres de subrayado, y un máximo de 50 caracteres.

> [!NOTE]
> El id. de oferta no se puede cambiar después de seleccionar **Crear**.

En el cuadro **Nombre del plan** , escriba un nombre para este plan. Los clientes verán este nombre al decidir qué plan van a seleccionar en su oferta. Cree un nombre único que destaque claramente las diferencias entre los planes. Por ejemplo, puede especificar **Windows Server** con los planes *Pago por uso* , *BYOL* , *Avanzado* y *Enterprise*.

Seleccione **Crear**.

## <a name="plan-setup"></a>Configuración del plan

Establezca la configuración de alto nivel para el tipo de plan, especifique si reutiliza la configuración técnica de otro plan e identifique las regiones de Azure en las que debe estar disponible el plan. Las selecciones que se realicen aquí afectarán a los campos mostrados en otros paneles para el mismo plan.

### <a name="reuse-a-technical-configuration"></a>Reutilización de una configuración técnica

Si tiene más de un plan del mismo tipo y los paquetes son idénticos entre ellos, puede seleccionar **This plan reuses technical configuration from another plan** (Este plan reutiliza la configuración técnica de otro plan). Esta opción le permite seleccionar uno de los otros planes del mismo tipo para esta oferta y reutilizar su configuración técnica.

> [!NOTE]
> Cuando reutilice la configuración técnica de otro plan, toda la pestaña **Configuración técnica** desaparecerá de este plan. Los detalles de configuración técnica del otro plan, incluidas las actualizaciones que realice en el futuro, se usarán también para este plan. Esta configuración no se puede modificar una vez publicado el plan.

### <a name="azure-regions"></a>Regiones de Azure

El plan debe estar disponible al menos en una región de Azure.

Seleccione la opción **Azure global** para que el plan esté disponible para los clientes de todas las regiones globales de Azure que tengan la integración de marketplace comercial. Para más información, consulte [Disponibilidad geográfica y compatibilidad con monedas para Marketplace comercial](marketplace-geo-availability-currencies.md).

Seleccione la opción **Azure Government** para que el plan esté disponible en la región de [Azure Government](../azure-government/documentation-government-welcome.md). Esta región proporciona acceso controlado para los clientes de entidades tribales, locales, estatales o federales de Estados Unidos, así como para los asociados aptos para atenderlas. Como publicador, es responsable de los controles de cumplimiento, las medidas de seguridad y los procedimientos recomendados. Azure Government usa redes y centros de datos aislados físicamente (ubicados solo en Estados Unidos).

Antes de publicar en [Azure Government](../azure-government/documentation-government-manage-marketplace-partners.md), pruebe y valide el plan en el entorno, ya que algunos puntos de conexión pueden ser diferentes. Para configurar y probar el plan, solicite una cuenta de prueba desde la página [Prueba de Microsoft Azure Government](https://azure.microsoft.com/global-infrastructure/government/request/).

> [!NOTE]
> Una vez que el plan esté publicado y disponible en una región de Azure específica, no se puede quitar esa región.

### <a name="azure-government-certifications"></a>Certificaciones de Azure Government

Esta opción solo está visible si ha seleccionado **Azure Government** como la región de Azure en la sección anterior.

Los servicios de Azure Government controlan datos que están sujetos a determinados reglamentos y requisitos gubernamentales. Por ejemplo, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 y CJIS. Para dar a conocer sus certificaciones de estos programas, puede proporcionar hasta 100 vínculos que las describan. Pueden ser vínculos a su anuncio en el programa directamente o a descripciones de su cumplimiento de estos en sus propios sitios web. Estos vínculos solo son visibles para los clientes de Azure Government.

Seleccione **Guardar borrador** antes de continuar.

## <a name="plan-listing"></a>Lista del plan

En esta sección se configuran los detalles de la lista del plan. Este panel muestra información específica que puede variar de otros planes de la misma oferta.

### <a name="plan-name"></a>Nombre del plan

Este campo se rellena automáticamente con el nombre que asignó al plan cuando lo creó. Este nombre aparece en Azure Marketplace como el título de este plan. Tiene un límite de 100 caracteres.

### <a name="plan-summary"></a>Resumen del plan

Proporcione un breve resumen del plan, no de la oferta. Este resumen se limita a 100 caracteres.

### <a name="plan-description"></a>Description del plan

Describa lo que hace que este plan sea único, así como las diferencias entre los planes de la oferta. Describa solo el plan, no la oferta. La descripción del plan puede contener hasta 2 000 caracteres.

Seleccione **Guardar borrador** antes de continuar.

## <a name="pricing-and-availability"></a>Precios y disponibilidad

En este panel, configura:

- Mercados en los que este plan está disponible. Todos los planes debe estar disponible al menos en un [mercado](marketplace-geo-availability-currencies.md).
- Precio por hora.
- Si desea que el plan sea visible para todos los usuarios o solo para clientes específicos (audiencia privada).

### <a name="markets"></a>Mercados

Todos los planes debe estar disponible al menos en un mercado. Seleccione la casilla de cada ubicación de mercado en la que este plan debe estar disponible para comprarlo. (Los usuarios de estos mercados pueden seguir implementando la oferta en todas las regiones de Azure seleccionadas en la sección ["Plan setup"](#plan-setup) [Configuración del plan].) El botón **Tax Remitted** (Impuestos remitidos) muestra los países y regiones en los que Microsoft lleva a cabo las ventas y la retención de impuestos en su nombre. La publicación en China se limita a planes que sean *Gratis* o *Traiga su propia licencia* (BYOL).

Si ya se han establecido los precios del plan en dólares de Estados Unidos (USD) y agrega otra ubicación de mercado, el precio del nuevo mercado se calcula en función de los tipos de cambio actuales. Revise siempre el precio de cada mercado antes de la publicación. Revise los precios seleccionando **Export prices (xlsx)** [Exportar precios (xlsx)] después de guardar los cambios.

Al eliminar un mercado, los clientes de ese mercado que usan implementaciones activas no podrán crear nuevas implementaciones ni escalar verticalmente las implementaciones existentes. Esto no afecta a las implementaciones existentes.

### <a name="pricing"></a>Precios

Para **License model** (Modelo de licencia), seleccione **Usage-based monthly billed plan** (Plan facturado mensual basado en el uso) para configurar los precios de este plan o elija **Traiga su propia licencia** para que los clientes puedan usar este plan con su licencia existente.

Para un plan de facturación mensual en función del uso, utilice una de las tres opciones de entrada de precios siguientes:

- **Por núcleo** : proporcione precios por núcleo en dólares USD. Microsoft calcula los precios por tamaño de núcleo y los convierte a las monedas locales con la tasa de cambio actual.
- **Por tamaño de núcleo** : proporcione precios por tamaño de núcleo en dólares USD. Microsoft calcula los precios y los convierte a las monedas locales con la tasa de cambio actual.
- **Por mercado y tamaño de núcleo** : proporcione precios para cada tamaño de núcleo para todos los mercados. Puede importar los precios desde una hoja de cálculo.

> [!NOTE]
> Guarde los cambios de los precios para habilitar la exportación de los datos de precios. Después de que se publica un precio para un mercado en el plan, no se puede cambiar más adelante. Asegúrese de que los precios sean correctos antes de publicarlos; para ello, exporte la hoja de cálculo de precios y revise los precios en cada mercado.

### <a name="free-trial"></a>Versión de prueba gratuita

Puede ofrecer una *evaluación gratuita* de un mes o de tres o seis meses a los clientes.

### <a name="visibility"></a>Visibilidad

Puede diseñar cada plan para que sea visible para todos los usuarios o solo para una audiencia preseleccionada. Asigne la pertenencia a esta audiencia restringida mediante los identificadores de suscripción de Azure.

**Pública** : todos los usuarios pueden ver el plan.

**Audiencia privada** : haga que el plan sea visible solo para una audiencia preseleccionada. Una vez publicado como un plan privado, puede actualizar la audiencia o cambiarlo a público. Después de hacer que un plan sea público, debe seguir siendo público. No se puede volver a cambiar a un plan privado.

> [!NOTE]
> Una audiencia privada o restringida es diferente de la audiencia preliminar que definió en la pestaña **Versión preliminar**. A una audiencia preliminar se le permite acceder a la oferta _antes_ de que se publique en Azure Marketplace. Mientras que la designación de una audiencia privada solo se aplica a un plan específico, la audiencia preliminar puede ver todos los planes privados y públicos con fines de validación.

**Audiencia restringida (identificadores de suscripción de Azure)** : asigne la audiencia que tendrá acceso a este plan privado mediante los identificadores de suscripción de Azure. Opcionalmente, incluya una descripción de cada identificador de suscripción de Azure que haya asignado. Agregue hasta 10 identificadores de suscripción manualmente o hasta 20 000 si importa una hoja de cálculo CSV. Los identificadores de suscripción de Azure se representan como GUID y todas las letras deben estar en minúsculas.

>[!Note]
>Las ofertas privadas no son compatibles con las suscripciones de Azure que se establecen a través de un revendedor del programa Proveedor de soluciones en la nube (CSP).

### <a name="hide-a-plan"></a>Ocultar un plan

Si la máquina virtual está diseñada para utilizarla solo indirectamente cuando se hace referencia a ella desde otra plantilla de solución o aplicación administrada, seleccione esta casilla para publicar la máquina virtual, pero ocultarla para los clientes que puedan buscarla directamente.

> [!NOTE]
> Los planes ocultos no admiten vínculos de versión preliminar.

Seleccione **Guardar borrador** antes de continuar.

## <a name="technical-configuration"></a>Configuración técnica

Proporcione las imágenes y otras propiedades técnicas que están asociadas al plan. Para más información, consulte los [procedimientos para configurar los detalles de la lista de ofertas de máquinas virtuales](azure-vm-create-listing.md).

> [!NOTE]
> La pestaña **Technical configuration** (Configuración técnica) no se muestra si ha configurado este plan para reutilizar paquetes de otro plan en la pestaña **Plan setup** (Configuración del plan).

### <a name="operating-system"></a>Sistema operativo

En el panel **Sistema operativo** , haga lo siguiente:

- En **Familia del sistema operativo** , seleccione entre el sistema operativo **Windows** o **Linux**.
- En **Versión** o **Distribuidor** , seleccione la versión de Windows o el distribuidor de Linux.
- En **Nombre descriptivo del sistema operativo** , especifique un nombre descriptivo para el sistema operativo. Este nombre es visible para los clientes.

### <a name="recommended-vm-sizes"></a>Recommended VM Sizes (Tamaños de máquina virtual recomendados)

Seleccione hasta seis tamaños de máquina virtual recomendados para que se muestren en Azure Marketplace.

### <a name="open-ports"></a>Abrir puertos

Puertos públicos o privados abiertos en una máquina virtual implementada.

### <a name="storage-option-for-deployment"></a>Opción de almacenamiento para la implementación

En **Disk deployment option** (Opción de implementación de disco), seleccione el tipo de implementación de disco que pueden utilizar los clientes para la máquina virtual. Microsoft recomienda limitar la implementación solo a la **implementación de discos administrados**.

### <a name="properties"></a>Propiedades

En **Support Accelerated Networking** (Compatibilidad con redes aceleradas), seleccione esta opción si la máquina virtual es compatible con [redes aceleradas](https://go.microsoft.com/fwlink/?linkid=2124513).

### <a name="generations"></a>Generaciones

La generación de una máquina virtual define el hardware virtual que usa. En función de las necesidades de sus clientes, puede publicar una máquina virtual de generación 1, una máquina virtual de generación 2 o ambas.

1. Al crear una oferta, seleccione un **tipo de generación** y escriba los detalles de la imagen solicitada:

    :::image type="content" source="./media/create-vm/azure-vm-generations-image-details.png" alt-text="Una vista del cuadro desplegable de generación":::.

2. Para agregar otra generación a un plan, seleccione **Add generation** (Agregar generación):

    :::image type="content" source="./media/create-vm/azure-vm-generations-add.png" alt-text="Vista del enlace &quot;Add Generation&quot; (Agregar generación)":::.

    A continuación, escriba los detalles de la generación:

    :::image type="content" source="./media/create-vm/azure-vm-generations-details.png" alt-text="Una vista de la ventana de los detalles de la generación":::.

    El **Id. de generación** que seleccione se mostrará a los clientes en ubicaciones como las direcciones URL de los productos y las plantillas de ARM (si procede). Use solo caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado; no se puede modificar una vez publicado.

3. Para actualizar una máquina virtual existente que tiene una generación 1 ya publicada, solo tiene que editar los detalles en la página **Configuración técnica** :

    :::image type="content" source="./media/create-vm/azure-vm-generations-updating.png" alt-text="Una vista de la página de configuración técnica del plan":::.

Para obtener más información sobre las diferencias entre las funcionalidades de la generación 1 y generación 2, vea [Compatibilidad para máquinas virtuales de generación 2 en Azure](../virtual-machines/generation-2.md).

### <a name="vm-images"></a>Imágenes de VM

Proporcione una versión de disco y el identificador URI de firma de acceso compartido (SAS) para las imágenes de máquina virtual. Agregue hasta 16 discos de datos para cada imagen de máquina virtual. Proporcione solo una nueva versión de imagen por plan en un envío especificado. Una vez publicada una imagen no se puede editar, pero se puede eliminar. La eliminación de una versión impide que los usuarios nuevos y existentes implementen una nueva instancia de la versión eliminada.

- **Versión del disco** : es la versión de la imagen que se va a proporcionar.
- **Identificador URI de SAS** : es la ubicación en la cuenta de Azure Storage en la que ha almacenado el disco duro virtual del sistema operativo. Para saber cómo obtener el URI de firma de acceso compartido, vea [Obtención del URI de firma de acceso compartido para la imagen de máquina virtual](azure-vm-get-sas-uri.md).
- Las imágenes de discos de datos también son identificadores URI de firma de acceso compartido de discos duros virtuales que se almacenan en sus cuentas de Azure Storage.
- Agregue solo una imagen por envío en un plan.

Independientemente del sistema operativo que use, agregue solo el número mínimo de discos de datos que necesite la solución. Durante la implementación, los usuarios no pueden eliminar los discos que forman parte de una imagen, pero siempre pueden agregar discos durante o después de la implementación.

Seleccione **Guardar borrador** antes de continuar y volver a **Plan overview** (Información general del plan).

## <a name="next-steps"></a>Pasos siguientes

- [Adición de un público en la versión preliminar](azure-vm-create-preview.md)
