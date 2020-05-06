---
title: SKU de los módulos Azure IoT Edge | Azure Marketplace
description: Cree SKU para un módulo IoT Edge.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 0a394c14842fe0c4eb4baa18cbb13acb8ca3170a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142459"
---
# <a name="iot-edge-module-skus-tab"></a>Pestaña SKU de módulo IoT Edge

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de módulos de IoT Edge al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una nueva oferta de módulo de IoT Edge](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation) para administrar las ofertas migradas.

La pestaña **SKU** de la página **Nueva oferta** permite crear una o varias SKU y asociarlas a la nueva oferta.  Puede usar diferentes SKU para diferenciar una solución por conjuntos de características, modelos de facturación o alguna otra característica.


## <a name="sku-settings"></a>Configuración de SKU

Cuando se empieza a crear una nueva oferta, no hay ninguna SKU asociada a ella. Para crear una nueva SKU, siga estos pasos:

- En la página **Módulos IoT Edge > Nueva oferta**, seleccione la pestaña **SKU**.
- En SKU, seleccione **+ Nueva SKU** para abrir un cuadro de diálogo.

  ![Botón Nueva SKU en la pestaña Nueva oferta de Módulos IoT Edge](./media/iot-edge-module-skus-tab-new-sku.png)

- En el cuadro de diálogo **Nueva SKU**, escriba un identificador de la SKU y seleccione **Aceptar**.
(La tabla siguiente proporciona las convenciones de nomenclatura del identificador).

La pestaña **SKU** se actualiza y muestra los campos que edita el usuario para configurar la SKU. Un asterisco (*) junto al nombre del campo indica que es obligatorio.

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
| **Id. de SKU\***       | Identificador de esta SKU. Este nombre tiene un máximo de 50 caracteres, consta de caracteres alfanuméricos en minúscula o guiones (-), pero no puede terminar con un guión. **Nota:** No se puede cambiar este nombre una vez publicada la oferta. El nombre es públicamente visible en las direcciones URL del producto. |
|  |  |


## <a name="sku-details"></a>Detalles de la SKU

Configure los **Detalles de la SKU** para definir cómo se va a mostrar la SKU en los sitios web de Azure Marketplace y Azure Portal.

![Metadatos de SKU de módulo IoT Edge](media/iot-edge-module-skus-tab-metadata.png)

En la tabla siguiente se describen el propósito, el contenido y el formato de los campos de **Detalles de la SKU**. Los campos obligatorios se indican con un asterisco (*).

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
| **Título\***        | Título de esta SKU. La longitud máxima es de 50 caracteres. <br/> Se muestra en Azure Portal y se usa como nombre de módulo predeterminado (sin espacios ni caracteres especiales) cuando se implementa. Vea las imágenes siguientes para saber exactamente dónde aparece este campo.|
| **Resumen\***      | Resumen breve de esta SKU. Tiene una longitud máxima de 100 caracteres. **NO** resume la oferta, solo la SKU.  Este resumen aparece en Azure Marketplace. Vea las imágenes siguientes para saber exactamente dónde aparece este campo.|
| **Descripción\***  | Breve descripción de esta SKU. Longitud máxima de 3 000 caracteres. NO describe la oferta, simplemente esta SKU. Aparece en Azure Marketplace y en Azure Portal. En Azure Portal, se anexa a la descripción de Marketplace que describe la oferta definida en la pestaña Marketplace.  Puede ser igual que el resumen de la SKU. Vea las imágenes siguientes para saber exactamente dónde aparece este campo.|
| **Hide this SKU\*** (Ocultar esta SKU) | Mantenga el valor predeterminado, que es **No**. |
|  |  |


### <a name="sku-example"></a>Ejemplo de SKU

 En los ejemplos siguientes se muestra cómo aparecen los campos **Título**, **Resumen** y **Descripción** en diferentes vistas.
 

#### <a name="on-the-azure-marketplace-website"></a>En el sitio web de Azure Marketplace:

- Al examinar los detalles de una SKU:

    ![Cómo aparecen las SKU en el sitio web de Azure Marketplace](media/iot-edge-module-ampdotcom-pdp-plans.png)


#### <a name="on-the-azure-portal-website"></a>En el sitio web de Azure Portal:

- Al examinar SKU:

    ![Cómo aparece el módulo IoT Edge al examinar Azure Portal n. º 1](media/iot-edge-module-portal-browse.png)

    ![Cómo aparece el módulo IoT Edge al examinar Azure Portal n. º 2](media/iot-edge-module-portal-product-picker.png)

- Al buscar SKU:

    ![Cómo aparece el módulo IoT Edge al buscar en Azure Portal](media/iot-edge-module-portal-search.png)

- Al examinar los detalles de una SKU:

    ![Cómo aparece el módulo IoT Edge al examinar los detalles del producto en el portal](./media/iot-edge-module-portal-pdp.png)

- Al implementar el módulo:
    
    ![Cómo aparece el módulo IoT Edge al implementarse](./media/iot-edge-module-deployment.png)


## <a name="sku-content"></a>Contenido de la SKU

En **Edge Module Images** (Imágenes de módulo de Edge), proporcione la información necesaria para cargar el módulo IoT Edge.

Proporciónenos acceso a la instancia de [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR) que contiene la imagen del módulo IoT Edge para que podamos cargarlo y certificarlo. Una vez publicado, el módulo IoT Edge se copia y se distribuye mediante un registro de contenedor público hospedado en Azure Marketplace.

Puede elegir como destino varias plataformas y proporcionar varias versiones por medio de etiquetas. Obtenga más información sobre [etiquetas y control de versiones en "Preparar los recursos técnicos del módulo IoT Edge"](./cpp-create-technical-assets.md).

![Imágenes de módulo IoT Edge](./media/iot-edge-module-skus-tab-acr.png)

En la tabla siguiente se describe el propósito, el contenido y el formato de los campos de las secciones **Image Repository Details** (Detalles del repositorio de imágenes) y **Versión de la imagen**.  Los campos obligatorios se indican con un asterisco (*).


|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
|  |  ***Detalles del repositorio de imágenes***    |
| **Id. de suscripción\***        | Id. de suscripción de Azure de ACR.|
| **Nombre del grupo de recursos\***      | Nombre del grupo de recursos de ACR.|
| **Nombre del Registro\***  | Nombre del Registro de ACR. Simplemente copie el nombre del Registro, NO el nombre del servidor de inicio de sesión (por ejemplo, sin `azurecr.io`). |
| **Nombre del repositorio\***  | Nombre del repositorio de ACR que contiene el módulo IoT Edge. **Nota:** Una vez establecido el nombre, no se puede cambiar más adelante. Use un nombre único para asegurarse de que ninguna otra oferta de la cuenta tenga el mismo. |
| **Nombre de usuario\*** | Nombre de usuario asociado a ACR (nombre de usuario de administrador). |
| **Contraseña\*** | Contraseña asociada a la instancia de ACR. |
|    |  ***Versión de la imagen***   |
| **Etiqueta o resumen de imagen\*** | Debe incluir al menos una etiqueta `latest` y una etiqueta de versión (que, por ejemplo, empiece por `xx.xx.xx-`, donde xx es un número). Deben ser [etiquetas de manifiesto](https://github.com/estesp/manifest-tool) para varias plataformas. Todas las etiquetas a las que hace referencia una etiqueta de manifiesto también se deben agregar para que podamos cargarlas. Puede agregar varias versiones de un módulo IoT Edge mediante etiquetas. Todas las etiquetas de manifiesto (excepto `latest`) deben comenzar por `X.Y-` o `X.Y.Z-`, donde X, Y, Z son números enteros. Obtenga más información sobre [etiquetas y control de versiones en "Preparar los recursos técnicos del módulo IoT Edge"](./cpp-create-technical-assets.md). <br/> Por ejemplo, si una etiqueta `latest` apunta a `1.0.1-linux-x64`, `1.0.1-linux-arm32` y `1.0.1-windows-arm32`, estas seis etiquetas tienen que agregarse aquí. |
|  |  |


### <a name="help-your-customers-launch-by-using-default-settings"></a>Ayudar a los clientes a publicar con la configuración predeterminada

Defina la configuración más común para implementar el módulo IoT Edge. Optimice las implementaciones de los clientes al permitirles publicar el módulo IoT Edge directamente con estos valores predeterminados.

![Configuración predeterminada del módulo IoT Edge durante la implementación](./media/iot-edge-module-skus-tab-iot-edge-defaults.png)

En la tabla siguiente se describen el propósito, el contenido y el formato de los campos de **Rutas predeterminadas**, **Propiedades deseadas gemelas predeterminadas**, **Variables de entorno predeterminadas** y **CreateOptions predeterminado**.

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
| **Rutas predeterminadas**        | Cada nombre de ruta predeterminado y valor debe tener menos de 512 caracteres. Puede definir hasta cinco rutas predeterminadas. Asegúrese de usar una [sintaxis de ruta](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) correcta en el valor de ruta. Para hacer referencia al módulo, use el nombre de módulo predeterminado, que será el **Título de la SKU** sin espacios ni caracteres especiales. Para hacer referencia a otros módulos aún no conocidos, use la convención `<FROM_MODULE_NAME>` para que los clientes sepan que deben actualizar esta información. Obtenga más información sobre [rutas de IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes). <br/> Por ejemplo, si el módulo `ContosoModule` escucha las entradas de `ContosoInput` y saca los datos en `ContosoOutput`, tiene sentido definir las dos rutas predeterminadas siguientes:<br/>- Nombre n. º 1: `ToContosoModule`<br/>- Valor n. º 1: `FROM /messages/modules/<FROM_MODULE_NAME>/outputs/* INTO BrokeredEndpoint("/modules/ContosoModule/inputs/ContosoInput")`<br/>- Nombre n. º 2: `FromContosoModuleToCloud`<br/>- Valor n. º 2: `FROM /messages/modules/ContonsoModule/outputs/ContosoOutput INTO $upstream`<br/>  |
| **Propiedades deseadas gemelas predeterminadas**      | Cada nombre y valor de propiedad deseada gemela predeterminada debe tener menos de 512 caracteres. Puede definir hasta cinco nombres o valores de propiedades deseadas gemelas. Los valores de las propiedades deseadas gemelas deben ser un JSON válido, sin escape, sin matrices y con una jerarquía máxima de cuatro. Obtenga más información sobre [propiedades deseadas gemelas](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties). <br/> Por ejemplo, si un módulo admite una frecuencia de actualización configurable dinámicamente a través de propiedades deseadas gemelas, tiene sentido definir la siguiente propiedad deseada gemela predeterminada:<br/> - Nombre n. º 1: `RefreshRate`<br/>- Valor n. º 1: `60`|
| **Variables de entorno predeterminadas**  | Cada nombre y valor de variable de entorno predeterminada debe tener menos de 512 caracteres. Puede definir hasta cinco nombres o valores de variables de entorno. <br/>Por ejemplo, si un módulo exige aceptar los términos de uso para iniciarse, puede definir la variable de entorno siguiente:<br/> - Nombre n. º 1: `ACCEPT_EULA`<br/>- Valor n. º 1: `Y`|
| **Elemento createOptions predeterminado**  | createOptions debe tener menos de 512 caracteres. Debe ser un JSON válido, sin escape. Obtenga más información sobre [createOptions](https://docs.microsoft.com/azure/iot-edge/module-composition#configure-modules). <br/> Por ejemplo, si un módulo exige enlazar un puerto, puede definir el siguiente elemento createOptions:<br/>  `"HostConfig":{"PortBindings":{"5012/tcp":[{"HostPort":"5012"}]}`|
|   |   |

Seleccione **Guardar** para guardar la configuración de la SKU. 


## <a name="next-steps"></a>Pasos siguientes

Use la [pestaña Marketplace](./cpp-marketplace-tab.md) para crear una descripción de Marketplace de la oferta.
