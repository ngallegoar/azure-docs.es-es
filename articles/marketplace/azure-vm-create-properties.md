---
title: Configuración de las propiedades de la oferta de máquinas virtuales en Azure Marketplace
description: Aprenda a configurar las propiedades de la oferta de máquinas virtuales en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: 30e5ea4df3de0b8a10c9c1f3b4b27a9e0a19359e
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491212"
---
# <a name="how-to-configure-virtual-machine-offer-properties"></a>Cómo configurar las propiedades de la oferta de máquinas virtuales

En la página **Propiedades** , se definen las categorías que se usan para agrupar la oferta de máquinas virtuales (VM) en Azure Marketplace, la versión de la aplicación y los contratos legales que respaldan la oferta.

## <a name="select-a-category"></a>Seleccionar una categoría

Seleccione las categorías y subcategorías para colocar la oferta en las áreas de búsqueda adecuadas de Marketplace. En la descripción de la oferta, asegúrese de indicar la forma en que la oferta admite estas categorías. Seleccione:

- Al menos una y un máximo de dos categorías, incluidas una categoría principal y una secundaria (opcional).
- Hasta dos subcategorías para cada categoría principal o secundaria. Si no hay ninguna subcategoría aplicable a la oferta, seleccione **No aplicable**.

Vea la lista completa de categorías y subcategorías en [Procedimientos recomendados para la publicación de ofertas](gtm-offer-listing-best-practices.md). Las ofertas de máquina virtual siempre aparecen en la categoría **Proceso** de Azure Marketplace.

## <a name="provide-terms-and-conditions"></a>Adición de términos y condiciones

En **Legal** , indique los términos y condiciones de la oferta. Tiene dos opciones:

- [Uso del Contrato estándar con modificaciones opcionales](#use-the-standard-contract)
- [Uso de términos y condiciones propios](#use-your-own-terms-and-conditions)

Para más información sobre el Contrato estándar y las modificaciones opcionales, consulte [Contrato estándar para marketplace comercial de Microsoft](standard-contract.md). Puede descargar el [Contrato estándar](https://go.microsoft.com/fwlink/?linkid=2041178) en formato PDF (asegúrese de que el bloqueador de elementos emergentes está desactivado).

### <a name="use-the-standard-contract"></a>Uso del Contrato estándar

Para simplificar el proceso de adquisición para los clientes y reducir la complejidad legal para los proveedores de software, Microsoft ofrece un Contrato estándar que se puede usar con las ofertas del marketplace comercial. Al ofrecer el software bajo el Contrato estándar, los clientes solo tienen que leerlo y aceptarlo una vez y no tiene que crear términos y condiciones personalizados.

1. Active la casilla **Use the Standard Contract for Microsoft's commercial marketplace** (¿Quiere usar el Contrato estándar para el marketplace comercial de Microsoft?).

   ![Muestra la casilla Use the Standard Contract for Microsoft's commercial marketplace (¿Quiere usar el Contrato estándar para el marketplace comercial de Microsoft?).](partner-center-portal/media/use-standard-contract.png)
1. En el cuadro de diálogo **Confirmación** , seleccione **Aceptar**. Es posible que tenga que desplazarse hacia arriba para verlo.
1. Seleccione **Guardar borrador** antes de continuar.

   > [!NOTE]
   > Después de publicar una oferta con el Contrato estándar para el marketplace comercial, no puede usar sus propios términos y condiciones personalizados. Puede ofrecer la solución con el Contrato estándar con modificaciones opcionales o con sus propios términos y condiciones.

### <a name="add-amendments-to-the-standard-contract-optional"></a>Adición de modificaciones al Contrato estándar (opcional)

Hay dos tipos de modificaciones disponibles: *universal* y *personalizada*.

#### <a name="add-universal-amendment-terms"></a>Adición de términos de modificación universal

En la casilla **Universal amendment terms to the standard contract for Microsoft's commercial marketplace** (Términos de modificación universales del Contrato estándar de marketplace comercial de Microsoft), escriba los términos de modificación universal en esta casilla. Puede escribir un número ilimitado de caracteres en este cuadro. Estos términos se muestran a los clientes en AppSource, Azure Marketplace o Azure Portal durante el flujo de detección y compra.

#### <a name="add-one-or-more-custom-amendments"></a>Adición de una o varias modificaciones personalizadas

1. En **Custom amendments terms to the Standard Contract for Microsoft's commercial marketplace** (Términos de modificación personalizados del Contrato estándar de Marketplace comercial de Microsoft), seleccione **Agregar término de modificación personalizado (máximo 10)** .
1. En el cuadro **Términos de modificación personalizados** , escriba sus términos de modificación.
1. En el cuadro **Id. de inquilino** , escriba un identificador de inquilino. Solo los clientes asociados a los identificadores de inquilino que se especifiquen para estos términos personalizados los verán en el flujo de compra de la oferta en Azure Portal.
   > [!TIP]
   > El identificador de inquilino identifica al cliente en Azure. Puede pedir a su cliente este identificador y encontrarlo; para ello, vaya a [ **https://portal.azure.com**](https://portal.azure.com) > **Azure Active Directory** > **Propiedades**. El valor del identificador de directorio es el identificador de inquilino (por ejemplo, `50c464d3-4930-494c-963c-1e951d15360e`). También puede buscar el identificador de inquilino de la organización de su cliente mediante su dirección URL de nombre de dominio en [What is my Microsoft Azure and Office 365 tenant ID?](https://www.whatismytenantid.com/) (¿Cuál es mi identificador de inquilino de Microsoft Azure y Office 365?).
1. En el cuadro **Descripción** , opcionalmente, escriba una descripción detallada del identificador de inquilino. Esta descripción le ayudará a identificar al cliente al que está destinada la modificación.
1. Para agregar otro identificador de inquilino, seleccione el vínculo **Add a customer's tenant ID** (Agregar un identificador de inquilino de cliente) y repita los pasos 3 y 4. Puede agregar hasta 20 identificadores de inquilino.
1. Para agregar otro término de modificación, repita los pasos 1 a 5. Puede proporcionar hasta diez términos de modificación personalizados por oferta. 
2. Seleccione **Guardar borrador** antes de continuar.

### <a name="use-your-own-terms-and-conditions"></a>Uso de términos y condiciones propios

También puede usar sus propios términos y condiciones en lugar del contrato estándar. Los clientes deben aceptar estos términos para poder probar la oferta.

1. En **Legal** , asegúrese de que la casilla **Use the Standard Contract for Microsoft's commercial marketplace** (¿Quiere usar el Contrato estándar para el marketplace comercial de Microsoft?) esté desactivada.
1. En el cuadro **Términos y condiciones** , escriba hasta 10 000 caracteres de texto.
1. Seleccione **Guardar borrador** antes de pasar a la siguiente sección, **Descripción de la oferta**.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de un listado de ofertas de máquina virtual](azure-vm-create-listing.md)
