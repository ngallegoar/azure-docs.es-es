---
title: Pestaña SKU de máquina virtual en Cloud Partner Portal para Azure Marketplace
description: Se describe la pestaña SKU usada al crear una oferta de máquina virtual en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: a8dbadf959107e733a94216c8a01495e6ebf5039
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273127"
---
# <a name="virtual-machine-skus-tab"></a>Pestaña SKU de máquina virtual

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://aka.ms/CreateAzureVMoffer) para administrar las ofertas migradas.

La pestaña **SKU** de la página **Nueva oferta** permite crear una o varias SKU y asociarlas a la nueva oferta.  Las distintas SKU pueden diferenciar una solución por conjuntos de características, tipos de imagen de máquina virtual, rendimiento o escalabilidad, modelos de facturación o alguna otra característica.


## <a name="create-a-sku"></a>Creación de una SKU

Inicialmente, una nueva oferta no tiene ninguna SKU asociada, así que cree una al hacer clic en **Nueva SKU**.

![Botón Nueva SKU en la pestaña Nueva oferta de máquinas virtuales](./media/publishvm_005.png)

<br/>

Aparece el cuadro de diálogo **Nueva SKU**.  Especifique el identificador de la nueva SKU y luego haga clic en **Aceptar**. (Vea a continuación las convenciones de nomenclatura del identificador).  La pestaña **SKU** ahora muestra los campos disponibles para editar.    Un asterisco (*) anexo al nombre del campo indica que es obligatorio.

<!-- TD: This tab has been updated, now has "Old Pricing" and "Simplified Currency Pricing" sections"! -->

![Pestaña SKU en el formulario Nueva oferta para máquinas virtuales](./media/publishvm_006.png)

En la tabla siguiente se describen el propósito, el contenido y el formato de estos campos.  Los campos obligatorios se indican con un asterisco (*).

<!-- TD: I took a new screenshot, and the fields differ somewhat from description in the VM Pub Guide.  Needs review. -->

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
|  *Configuración de SKU*   |    |
| **Id. de SKU\***       | Identificador de esta SKU.  Este nombre tiene un máximo de 50 caracteres, consta de caracteres alfanuméricos en minúscula o guiones (-), pero no puede terminar con un guión.  No puede modificarse una vez publicada la oferta.  |
|  *Detalles de la SKU*   |  |
| **Título\***        | Nombre descriptivo de la oferta para mostrar en Marketplace. La longitud máxima es de 50 caracteres. |
| **Resumen\***      | Descripción concisa de la oferta para mostrar en Marketplace. Tiene una longitud máxima de 100 caracteres. |
| **Descripción\***  | Texto de descripción que proporciona una explicación más detallada de la oferta.  <!-- TD: max len/guidance? 3k characters -->  |
| **Hide this SKU\*** (Ocultar esta SKU) | Indica si la SKU debe estar visible en Marketplace para los clientes.  Puede ocultar la SKU si quiere que solo esté disponible a través de plantillas de soluciones y no para adquirirse por separado.  También podría ser útil para pruebas iniciales o para ofertas temporales o estacionales. |
| **Cloud Availability\*** (Disponibilidad en la nube) | Determina en qué nubes debe estar disponible la SKU.  El valor predeterminado es la versión pública de Azure.  Microsoft Azure Government es una nube de la comunidad gubernamental con acceso controlado para gobiernos tribales, locales, estatales y federales de los Estados Unidos y sus asociados certificados.  Para obtener más información sobre la nube de Government, vea [Welcome to Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) (Introducción a Azure Government). |
| **Is this a Private SKU?\*** (¿Es una SKU privada?) | Indica si la SKU es pública o privada. El valor predeterminado es **No** (pública).  Para obtener más información, vea [SKU privadas](../../cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md). |
| **Country/Region availability\*** (Disponibilidad de país o región) | Determina en qué países o regiones del mundo estará disponible la SKU para su compra. Seleccione al menos una región o un país. <!-- TD: Is this parameter an AMP visibility control or a contractual one, or both? --> |  
|  *Precios*   |  |
| **License Model\*** (Modelo de licencia)| Modelo de facturación estandarizado que se va a usar.  Si selecciona **Usage-based monthly billed SKU** (SKU facturada mensualmente por uso), se abre una sección de acordeón para permitirle especificar detalles de los precios por núcleo y si quiere ofrecer un período de evaluación gratuita.  En esta sección también es posible exportar e importar este plan de precios a Excel. Para obtener más información, vea [Opciones de facturación de Azure Marketplace](../../billing-options-azure-marketplace.md). | 
|  *Imágenes de máquinas virtuales*   |  |
| **Familia del sistema operativo\*** | Indica si la máquina virtual de la solución está basada en Windows o Linux. |
| **Seleccionar el tipo de sistema operativo** | Proveedor o versión concretos del sistema operativo especificado. |
| **Nombre descriptivo del sistema operativo\*** | Nombre del sistema operativo que se va a mostrar a los clientes.  |
| **Recommended VM Sizes\*** (Tamaños de máquina virtual recomendados) | Permite seleccionar hasta seis tamaños de máquina virtual recomendados en una lista estandarizada.  Esta lista se pasa a Azure Portal y a Microsoft Marketplace.  El primer tamaño de máquina virtual de la lista que es válido (para esa suscripción de cliente, región, zona, etc.) se establece como valor predeterminado para ese cliente potencial.  El usuario puede cambiar este tamaño a cualquiera compatible con la imagen de la solución. | 
| **Abrir puertos**| Puertos que se van a abrir y protocolo que se va a admitir de la SKU.  Estas configuraciones deben coincidir con la red virtual que se ha configurado para la red de la máquina virtual de la solución. Estos valores se aplican durante la implementación de la máquina virtual. Pero la configuración de puertos puede modificarse después de publicar una SKU. Para obtener más información, vea [Apertura de puertos en una máquina virtual con Azure Portal](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal). <br/>Las siguientes asignaciones de red predeterminadas se agregan a todas las máquinas virtuales. &emsp; Windows: 3389 -> 3389 TCP, 5986 -> 5986 TCP; &emsp; Linux: 22 -> 22, TCP (SSH). |
| **Versión del disco**  | Máquina virtual de la solución asociada, especificada por el número de versión del disco y la dirección URL de este. La versión del disco debe estar en formato de [versión semántica](https://semver.org/): `<major>.<minor>.<patch>`.  La dirección URL es el URI de firma de acceso compartido creado para el disco duro virtual del sistema operativo.  Aunque puede agregar hasta ocho versiones del disco por SKU, solo el número de versión más alto del disco de una SKU aparece en Azure Marketplace. Las demás versiones solo son visibles a través de API.  <!--TD: Add more specific link to API --> <br/> La sección de acordeón **Nuevo disco de datos** permite asociar hasta 15 discos de datos a la máquina virtual.  Una vez que se publica una SKU con una versión concreta de máquina virtual y discos de datos asociados, esta configuración no se puede modificar.  Si se agregan versiones adicionales de máquina virtual a la SKU, también deben admitir el mismo número de discos de datos. <br/> Si no ha creado las imágenes de máquina virtual basada en Azure, puede actualizar este campo más adelante.  Para obtener información sobre cómo crear el recurso de máquina virtual asociado, vea la sección [Crear los recursos técnicos](./cpp-create-technical-assets.md).  
|  |  |

<!-- TD: The CPP UX warning msg indicates that underscores are also supported in these SKU IDs. I suspect this might be true for other identifiers. --> 

<br/> Haga clic en **Guardar** para guardar lo que ha hecho hasta ahora. En la pestaña siguiente, se especifica si la oferta admite [Unidad de prueba](./cpp-test-drive-tab.md).


## <a name="additional-pricing-considerations"></a>Consideraciones adicionales sobre precios

El modelo de precios indicado anteriormente es una descripción básica.  Se están llevando a cabo modificaciones y puede verse afectado por la normativa impositiva regional o local y las directivas de precios de Microsoft. 

### <a name="new-core-sizes-added-on-722019"></a>Nuevos tamaños de núcleo agregados el 2/7/2019

A los publicadores de máquinas virtuales se les notificó el 2 de julio de 2019 la incorporación de nuevos precios para los nuevos tamaños de máquina virtual de Azure (en función del número de núcleos).  Los nuevos precios son para los tamaños de núcleo 10, 44, 48, 60, 120, 208 y 416.  Para ofertas de máquinas virtuales existentes, los nuevos precios para estos tamaños de núcleo se han calculado automáticamente en función de los precios actuales.  Los publicadores tienen hasta el 1 de agosto de 2019 para revisar los precios adicionales y realizar los cambios deseados.  Después de esta fecha, si el publicador no los ha vuelto a publicar, se aplicarán los precios calculados automáticamente para estos nuevos tamaños de núcleo.


### <a name="simplified-currency-pricing"></a>Precios en moneda simplificada

A partir del 1 de septiembre de 2018, se agrega al portal una nueva sección denominada **Precios en moneda simplificada**. Microsoft está simplificando el negocio de Azure Marketplace mediante la habilitación de precios más predecibles y colecciones de sus clientes en todo el mundo. Esta optimización incluirá la reducción del número de monedas en que se factura a los clientes.  Para obtener más información, vea [Actualizar una oferta existente de máquina virtual en Azure Marketplace](./cpp-update-existing-offer.md).


### <a name="additional-information-on-taxes-and-prices"></a>Información adicional sobre impuestos y precios

* Microsoft clasifica a algunos países y regiones como *países sujetos a recaudación fiscal*.  En esos países y regiones, Microsoft recauda los impuestos de los clientes y los devuelve al gobierno.  En otros países y regiones, los asociados normalmente son los responsables de recaudar los impuestos de los clientes y de devolverlos al gobierno. Si decide vender en estos países y regiones, debe tener la capacidad de calcular y pagar los impuestos locales.  <!-- TD: Find a good reference on taxing policies. The best I found was in the UWP section: https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps -->
* Los precios no se pueden modificar después de publicar la oferta. Aunque sí puede agregar o quitar regiones admitidas. 
* Microsoft cobra los precios estándar de uso de máquina virtual de Azure además de las tarifas de SKU programadas.
* Los precios se establecen para todas las regiones en la moneda local según el tipo de cambio disponible en el momento en que se fijan los precios.  <!-- TD: Meaning? - Offer created, published, other? -->
* Para establecer el precio de cada región individualmente, exporte la hoja de cálculo de precios, aplique los precios personalizados y luego impórtela. 


## <a name="next-steps"></a>Pasos siguientes

Si lo desea, puede especificar la información de la [versión de prueba](./cpp-test-drive-tab.md) si admite esta característica; de lo contrario, proporcione los datos de [Marketplace](./cpp-marketplace-tab.md) para la oferta.
