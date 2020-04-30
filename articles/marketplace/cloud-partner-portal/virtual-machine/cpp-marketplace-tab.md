---
title: Pestaña Marketplace de máquina virtual en Cloud Partner Portal para Azure Marketplace
description: Se describe la pestaña Marketplace que se usa en la creación de una oferta de máquina virtual de Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: 2c3d316d0d2810cb2a25ffd3bc4e34cf3454c10d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146855"
---
# <a name="virtual-machine-marketplace-tab"></a>Pestaña Marketplace de la máquina virtual

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) para administrar las ofertas migradas.

La pestaña **Marketplace** de la página **Nueva oferta** permite proporcionar a los clientes potenciales información de marketing, ventas y legal, así como acuerdos, y administrar los clientes potenciales generados a partir de Marketplace. Este largo formulario se divide en cuatro secciones: **Información general**, **Artefactos de Marketing**, **Administración de clientes potenciales** e **Información legal**.


## <a name="overview-section"></a>Sección Información general
En esta sección, se escribe la información general sobre la oferta de Azure Marketplace.  Un asterisco (*) anexo al nombre del campo indica que es obligatorio.

![Sección Información general de la pestaña Marketplace para máquinas virtuales](./media/publishvm_008.png)

En la tabla siguiente se describen la finalidad y el contenido de estos campos. Los campos obligatorios se indican con un asterisco (*).

|  **Campo**                |     **Descripción**                                                          |
|  ---------                |     ---------------                                                          |
| **Título\***                 | Título de la oferta, a menudo el nombre largo y formal. Este título se mostrará de forma destacada en Marketplace.  La longitud máxima es de 50 caracteres. |
| **Resumen\***               | Breve propósito o función de la solución.  Tiene una longitud máxima de 100 caracteres. |
| **Resumen largo\***          | Propósito o función de la solución.  Tiene una longitud máxima de 256 caracteres. |
| **Descripción\***           | Descripción de la solución.  La longitud máxima es de 3000 caracteres y admite el formato HTML simple. |
| **Canal de revendedor de CSP de Microsoft\*** | La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](../../cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft. |
| **Identificador de marketing\***  | Dirección URL única para asociar a esta oferta, normalmente incluye la organización y el nombre de la solución, longitud máxima de 50 caracteres.  Por ejemplo: <br/> `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  |
| **Id. de suscripción de versión preliminar\*** | Agregue entre uno y 100 identificadores de suscripción de versión preliminar. Estas suscripciones permitidas tendrán acceso a la oferta una vez publicada, antes de su lanzamiento. |
| **Vínculos útiles**          | Agregue direcciones URL a la documentación, notas de la versión y preguntas más frecuentes, entre otros. |
| **Categorías sugeridas\*** | Seleccione un máximo de dos (2) categorías, incluidas una categoría principal y una secundaria (opcional). Seleccione un máximo de dos (2) subcategorías para cada categoría principal o secundaria. Si no se selecciona ninguna subcategoría, solo se podrá detectar en la categoría seleccionada. |
|  |  |


## <a name="marketing-artifacts-section"></a>Sección de artefactos de marketing

Esta segunda sección se divide en tres subsecciones: **Logotipos**, **Captura de pantalla** y **Vídeos**. Los logotipos son los únicos artefactos de marketing obligatorios, aunque todos son muy recomendables para atraer al consumidor. 

![Sección Artefactos de marketing de la pestaña Marketplace en el formulario Nueva oferta para máquinas virtuales](./media/publishvm_009.png)

En la tabla siguiente se describen la finalidad y el contenido de estos campos. Los campos obligatorios se indican con un asterisco (*).

|  **Campo**                |     **Descripción**                                                          |
|  ---------                |     ---------------                                                          |
| *Logotipos*  |  |
| **Pequeña\***                 | Mapa de bits .ico de 40 x 40 píxeles                                                      |
| **Mediana\***                | Mapa de bits .ico de 90 x 90 píxeles                                                      |
| **Grande\***                 | Mapa de bits .ico de 115 x 115 píxeles                                                   |
| **Ancho\***                  | Mapa de bits .ico de 255 x 115 píxeles                                                    |
| **Imagen prominente**                  | Mapa de bits de 815 x 290.  Opcional, aunque una vez cargado, el icono de la imagen prominente no se puede eliminar. |
| *Capturas de pantalla*  | Opcional, aunque un máximo de cinco capturas de pantalla por SKU. |
| **Nombre**                  | Nombre o título <!-- TODO - max char length? none specified in UI -->                               |
| **Imagen**                 | Imagen de captura de pantalla, de 533 x 324 píxeles                                         |
| *Vídeos*  |  |
| **Nombre**                  | Nombre o título  <!-- TODO - max char length? -->                              |
| **Vínculo**                  | Dirección URL del vídeo, hospedado en YouTube o Vimeo                                        |
| **Miniatura**             | Mapa de bits de 533 x 324                                                               |
|   |   |

### <a name="logo-guidelines"></a>Directrices para logotipos

<!-- TD: It seems like this section could be better located in some common area, maybe an AMP Marketing/Design section 
+1 this should all be in a common area and referenced from here to that location.-->

Todos los logotipos cargados en Cloud Partner Portal deben seguir estas instrucciones:

*  El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.
*  Los colores del tema del Portal de Azure son el blanco y el negro. Por tanto, evite el uso de estos colores como fondo de los logotipos. Utilice algún color que haga destacar a los logotipos en el Portal de Azure. Nosotros recomendamos usar colores primarios simples. Si usa un fondo transparente, asegúrese de que el texto y los logotipos no sean de color azul, blanco o negro.
*  No use un fondo degradado en el logotipo.
*  Evite colocar texto, incluso el nombre de la empresa o la marca, en el logotipo. El aspecto del logotipo debe ser "plano" y evitar los degradados.
*  No extienda el logotipo.

#### <a name="hero-logo"></a>Logotipo de imagen prominente

El logotipo de imagen prominente es opcional, aunque una vez cargado, el icono de la imagen prominente no se puede eliminar.  El icono de imagen prominente debe seguir estas instrucciones:

*  Los fondos transparentes y de color negro y blanco no se pueden usar en los iconos de imagen prominente.
*  Evite el uso de colores claros en el fondo del icono de imagen prominente.  El nombre para mostrar del publicador, el título del plan y el resumen largo de la oferta se muestran en color de fuente blanco y deben destacar contra el fondo.
*  Evite el uso de texto cuando diseñe el logotipo de imagen prominente.  El nombre del publicador, el título del plan, el resumen largo de la oferta y un botón Crear se insertan mediante programación dentro del icono de imagen prominente al mostrar la oferta. 
* Incluya un rectángulo sin usar en el lado derecho del icono de imagen prominente, con un tamaño de 415 x 100 píxeles y un desplazamiento de 370 píxeles desde la izquierda.  

Por ejemplo, el icono de imagen prominente siguiente es para Azure Container Service.  <!-- TD: It would be nice to have the raw bitmap, e.g.before and after embedding. -->

![Icono de imagen prominente de ejemplo para Azure Container Service](./media/publishvm_010.png)


### <a name="marketing-information-example"></a>Ejemplo de información de marketing 

En la imagen siguiente se ilustra cómo se muestra la información de marketing en la página de producto principal de Microsoft Windows Server.

![Página de producto de ejemplo para Microsoft Windows Server](./media/publishvm_011.png)


## <a name="lead-management-section"></a>Sección de administración de clientes potenciales

La tercera sección permite recopilar los clientes potenciales generados a partir de las ofertas de Azure Marketplace. Ofrece las opciones de almacenamiento siguientes (en una lista desplegable) para esta información de clientes potenciales.

* **Ninguna**: no se recopila la información predeterminada de clientes potenciales.
* Tabla de Azure: se escribe en la tabla de Azure especificada por una cadena de conexión.
* Dynamics CRM Online: se escribe en la instancia de [Microsoft Dynamics 365 Online](https://dynamics.microsoft.com/) especificada por una dirección URL y credenciales de autenticación.
* Punto de conexión HTTPS: se escribe en el punto de conexión HTTPS especificado como una carga JSON.
* Marketo: se escribe en la instancia de [Marketo](https://www.marketo.com/) especificada por Id. de servidor, de Munchkin y de formulario.
* Salesforce: se escribe en una base de datos de [Salesforce](https://www.salesforce.com/) especificada por un identificador de objeto.

Después de publicar correctamente la oferta, se valida la conexión de clientes potenciales y se envía de forma automática un cliente potencial de prueba al destino configurado. La información de clientes potenciales se debe administrar de forma continua y este valor se debe actualizar con prontitud cada vez que se realicen cambios en la arquitectura de administración de clientes.

<!-- TD: For more info, see [Need a topic on lead information and processing that mimics the Appendix of the VM Pub Guide]. -->

## <a name="legal-section"></a>Sección legal

Esta última sección permite proporcionar los documentos legales necesarios para cada oferta.  

|  **Campo**                    |     **Descripción**                                        |
|  ---------                    |     ---------------                                        |
| **Dirección URL de la directiva de privacidad\***      | Dirección URL de la directiva de privacidad publicada                          |
| **¿Quiere usar el Contrato estándar?\***  |   |
| **Términos de uso\***            | Directiva como texto sin formato o HTML simple.                       |
|  |  |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña [Soporte técnico](./cpp-support-tab.md) siguiente, proporcionará los recursos de soporte técnico y de usuario para la oferta.
