---
title: Descripción de Marketplace de un módulo Azure IoT Edge | Azure Marketplace
description: Cree la descripción de Marketplace de un módulo IoT Edge.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: dsindona
ms.openlocfilehash: 3ff65a637c1d3722e773ab27f051b3b5bc109a23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281738"
---
# <a name="iot-edge-module-marketplace-tab"></a>Pestaña Marketplace de módulo IoT Edge

La pestaña **Marketplace** de la página **Nueva oferta** permite proporcionar a los clientes potenciales información de marketing, ventas y legal, así como acuerdos, y administrar los clientes potenciales generados a partir de Marketplace. Este largo formulario se divide en cuatro secciones: **Información general**, **Artefactos de marketing**, **Administración de clientes potenciales** y **Legal**.


## <a name="overview"></a>Información general

En esta sección, se escribe la información general sobre la oferta de Azure Marketplace.  Un asterisco (*) junto al nombre del campo indica que es obligatorio.

![Sección Información general de la pestaña Marketplace en el formulario Nueva oferta para módulos IoT Edge](./media/iot-edge-module-marketplace-tab-overview.png)

En la tabla siguiente se describen la finalidad y el contenido de estos campos. Los campos obligatorios se indican con un asterisco (*).

|  **Campo**                |     **Descripción**                                                          |
|  ---------                |     ---------------                                                          |
| **Título\***                 | Título de la oferta. Se muestra de forma destacada en Marketplace.  La longitud máxima es de 50 caracteres. <!--ADD PICTURE IN ACTION-->|
| **Resumen\***               | Breve resumen de la oferta. Tiene una longitud máxima de 100 caracteres. <!--ADD PICTURE IN ACTION-->|
| **Resumen largo\***          | Resumen más largo de la oferta (aunque podría ser igual que el **resumen**).  Tiene una longitud máxima de 256 caracteres. <!--ADD PICTURE IN ACTION-->|
| **Descripción\***           | Descripción de la oferta.  La longitud máxima es de 3000 caracteres y admite el formato HTML simple.<br/> Debe incluir un párrafo de *requisitos mínimos de hardware* en la parte inferior. Por ejemplo: <br/> <p><u>Requisitos mínimos de hardware:</u> Sistema operativo Linux x64 y arm32, 1 GB de RAM, 500 MB de almacenamiento</p>
| **Identificador de marketing\***  | Dirección URL única para asociar a esta oferta, normalmente incluye la organización y el nombre de la solución, longitud máxima de 50 caracteres.  Por ejemplo: <br/> `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleIoTEdgeModule`  |
| **Id. de suscripción de versión preliminar** | Agregue entre uno y 100 identificadores de suscripción de versión preliminar. Estas suscripciones permitidas tendrán acceso a la oferta una vez publicada, antes de su lanzamiento. |
| **Vínculos útiles**          | Selección múltiple de categorías empresariales y técnicas con las que mejor se puede asociar la oferta.  Se permite un máximo de diez. Asegúrese de agregar al menos un vínculo a la documentación y un vínculo a los dispositivos de IoT Edge compatibles desde el [catálogo de dispositivos de IoT de Azure](https://catalog.azureiotsolutions.com/). |
| **Categorías sugeridas\*** | Elija un máximo de cinco categorías. Se muestran en la página de detalles del producto. En las páginas de exploración, todos los módulos IoT Edge aparecen en la categoría *Internet de las cosas \>Módulo IoT Edge*.|
|  |  |


### <a name="offer-example"></a>Ejemplo de oferta

 En los ejemplos siguientes se muestra cómo aparecen los campos **Título**, **Resumen**, **Descripción**, **Logotipos** y **Capturas de pantalla** de la oferta en diferentes vistas.

 
#### <a name="on-the-azure-marketplace-website"></a>En el sitio web de Azure Marketplace:

- Al explorar ofertas:

    ![Cómo se muestran las ofertas de Marketplace en el sitio web de Azure Marketplace](./media/iot-edge-module-ampdotcom-card.png)

- Al examinar los detalles de una oferta:

    ![Cómo aparece el módulo IoT Edge al examinar los detalles del producto en el sitio web](./media/iot-edge-module-ampdotcom-pdp.png)


#### <a name="on-the-azure-portal-website"></a>En el sitio web de Azure Portal:

- Al explorar ofertas:

    ![Cómo aparece el módulo IoT Edge al examinar Azure Portal n. º 1](./media/iot-edge-module-portal-browse.png)

    ![Cómo aparece el módulo IoT Edge al examinar Azure Portal n. º 2](./media/iot-edge-module-portal-product-picker.png)

- Al buscar una oferta:

    ![Cómo aparece el módulo IoT Edge al buscar en Azure Portal](./media/iot-edge-module-portal-search.png)

- Al examinar los detalles de una oferta:

    ![Cómo aparece el módulo IoT Edge al examinar los detalles del producto en el portal](./media/iot-edge-module-portal-pdp.png)


## <a name="marketing-artifacts"></a>Artefactos de marketing

Esta sección contiene las siguientes subsecciones: **Logotipos**, **Capturas de pantalla** y **Vídeos**. 

>[!Note]
>Los logotipos son los únicos artefactos de marketing obligatorios, aunque todos son muy recomendables para atraer al consumidor.

![Sección Artefactos de marketing de la pestaña Marketplace en el formulario Nueva oferta para máquinas virtuales](./media/publishvm_009.png)

|  **Campo**                |     **Descripción**                                                          |
|  ---------                |     ---------------                                                          |
| *Logotipos*  | Vea las capturas de pantalla anteriores para ver cómo y dónde se usan los logotipos.  |
| **Pequeña\***                 | Formato PNG de 40 x 40 píxeles                                                     |
| **Mediana\***                | Formato PNG de 90 x 90 píxeles                                                     |
| **Grande\***                 | Formato PNG de 115 x 115 píxeles                                                  |
| **Ancho\***                  | Formato PNG de 255 x 115 píxeles                                                   |
| **Imagen prominente**                  | Formato PNG de 815 x 290 píxeles.  Opcional, aunque una vez cargado, el icono de la imagen prominente no se puede eliminar. |
| *Capturas de pantalla*  | Las capturas de pantalla se muestran en la página de detalles del producto. Son una excelente forma de comunicar de manera visual lo que hace el módulo IoT Edge y cómo funciona. Por ejemplo, puede mostrar diagramas de arquitectura o ilustraciones de casos de uso. Opcional, aunque un máximo de cinco capturas de pantalla por SKU. |
| **Nombre**                  | Nombre o título. Tiene una longitud máxima de 100 caracteres.                             |
| **Imagen**                 | Imagen de captura de pantalla, formato PNG de 533 x 324 píxeles                               |
| *Vídeos*  | Los vídeos se muestran en la página de detalles del producto. Son una excelente forma de comunicar de manera visual lo que hace el módulo IoT Edge y cómo funciona. |
| **Nombre**                  | Nombre o título. Tiene una longitud máxima de 100 caracteres.                             |
| **Vínculo**                  | Dirección URL del vídeo, hospedado en YouTube o Vimeo                                        |
| **Miniatura**             | Formato PNG de 533 x 324 píxeles                                                     |
|  |  |


### <a name="logo-guidelines"></a>Directrices para logotipos

<!-- It seems like this section could be better located in some common area, maybe an AMP Marketing/Design section 
+1 this should all be in a common area and referenced from here to that location.-->

Todos los logotipos cargados en Cloud Partner Portal deben seguir estas instrucciones:

*  El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.
*  Los colores de tema de Azure Portal son el blanco y el negro. Evite emplearlos como color de fondo de los logotipos. Use un color que permita destacar los logotipos en Azure Portal. Nosotros recomendamos usar colores primarios simples. Si usa un fondo transparente, asegúrese de que el texto o los logotipos no sean de color azul, blanco o negro.
*  No use un fondo degradado en el logotipo.
*  Evite colocar texto, incluso el nombre de la empresa o la marca, en el logotipo. El aspecto del logotipo debe ser "plano" y evitar los degradados.
*  No estire el logotipo.


#### <a name="hero-logo"></a>Logotipo de imagen prominente

El logotipo de imagen prominente es opcional.

>[!Important]
>Una vez cargado el logotipo de imagen principal, no se puede eliminar.

Use las siguientes directrices para un logotipo de imagen principal: 

*  Los fondos transparentes y de color negro o blanco no pueden usarse.
*  Evite el uso de colores claros como fondo del logotipo.  El nombre para mostrar del publicador, el título del plan y el resumen largo de la oferta se muestran en color de fuente blanco y deben destacar contra el fondo.
*  Evite el uso de mucho texto cuando diseñe el logotipo. El nombre del publicador, el título del plan, el resumen largo de la oferta y un botón Crear se insertan mediante programación dentro del logotipo cuando se publica la oferta. 
* Incluya un espacio rectangular sin usar en el lado derecho del logotipo de imagen principal. Este espacio en blanco tiene 415 x 100 píxeles y el desplazamiento desde la izquierda es de 370 píxeles.  

<!-- P2: would be nice to also have an picture int he context of iot edge of the hero image
As an example, the following hero icon is for the Azure Container Service.

![Example hero icon for Azure Container Service](./media/publishvm_010.png)
-->


## <a name="lead-management"></a>Administración de clientes potenciales

Esta sección permite configurar las opciones para recopilar los clientes potenciales generados a partir de las ofertas de Azure Marketplace. Puede seleccionar las siguientes opciones de almacenamiento en una lista desplegable.

* **Ninguna**: no se recopila la información predeterminada de clientes potenciales.
* Tabla de Azure: se escribe en la tabla de Azure especificada por una cadena de conexión.
* Dynamics CRM Online: se escribe en la instancia de [Microsoft Dynamics 365 Online](https://dynamics.microsoft.com/) especificada por una dirección URL y credenciales de autenticación.
* Punto de conexión HTTPS: se escribe en el punto de conexión HTTPS especificado como una carga JSON.
* Marketo: se escribe en la instancia de [Marketo](https://www.marketo.com/) especificada por Id. de servidor, de Munchkin y de formulario.
* Salesforce: se escribe en una base de datos de [Salesforce](https://www.salesforce.com/) especificada por un identificador de objeto.

Después de publicar correctamente la oferta, se comprueba la conexión de clientes potenciales y se envía automáticamente un cliente potencial de prueba al destino que ha configurado. 

>[!Note]
>La información de clientes potenciales se debe administrar de forma continua y este valor se debe actualizar con prontitud cada vez que se realicen cambios en la arquitectura de administración de clientes.

<!-- there is missing some marketing imagess
1. inside azure portal the wide logo/regular logo
2. inside azure portal the top curation section
3. amp.com the pricing tab
4. amp.com the tile -->


## <a name="legal"></a>Información legal

Esta sección permite ofrecer los dos documentos legales que son necesarios para cada oferta: Directiva de privacidad y Términos de uso.

|  **Campo**                    |     **Descripción**                                                          |
|  ---------                    |     ---------------                                                          |
| **Dirección URL de la directiva de privacidad\***      | Dirección URL de la directiva de privacidad publicada                                            |
| **Usar el Contrato estándar\***  | Si se usa la plantilla de contrato estándar de Microsoft.  Para más información, consulte [Contrato estándar](https://docs.microsoft.com/azure/marketplace/standard-contract).   |
| **Términos de uso\***            | *Términos de uso* como HTML simple insertado o vínculo a la página de términos de uso publicados     |
|  |  |


## <a name="next-steps"></a>Pasos siguientes

Use la pestaña [Soporte](./cpp-support-tab.md) para proporcionar los recursos técnicos y de soporte técnico de la oferta.
