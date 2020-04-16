---
title: Pestaña de Marketplace de la oferta de aplicación de Azure
description: Use la pestaña de Marketplace para identificar los recursos de marketing de una oferta de aplicación de Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 6113759cc68d07af4c22edf03b3274d92dcbe780
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416293"
---
# <a name="azure-application-marketplace-tab"></a>Pestaña de Marketplace de aplicación de Azure

Utilice la pestaña de Marketplace para describir su aplicación de Azure y proporcionar recursos de marketing. Esta pestaña incluye los siguientes formularios: Información general, Artefactos de Marketing, Administración de clientes potenciales e Información legal.

## <a name="overview-form"></a>Formulario Información general

El formulario Información general tiene los campos obligatorios y opcionales que se muestran en la siguiente captura de pantalla. Los campos obligatorios se indican con un asterisco (*).

![Formulario Información general](./media/azureapp-marketplace-overview.png)

En la tabla siguiente se describe la configuración que se usará para la creación de un escaparate de la oferta.   Los campos anexados con un asterisco son obligatorios.

|      Campo         |    Descripción    |
|  ---------------   |  ---------------  |
| **Título\***        | Título de la oferta. Se muestra de forma destacada en Marketplace. La longitud máxima es de 50 caracteres. |
| **Resumen\***      | Breve resumen de la oferta. La longitud máxima es de 100 caracteres.           |
| **Resumen largo\*** | Resumen más largo de la oferta (aunque podría ser igual que el resumen). La longitud máxima es de 256 caracteres.           |
| **Descripción\***  | Descripción de la oferta. La longitud máxima es de 3000 caracteres. Se permite HTML simple, incluidas las etiquetas &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; y header.  |
| **Identificador de marketing\*** | Dirección URL única para asociar a esta oferta, normalmente incluye la organización y el nombre de la solución, longitud máxima de 50 caracteres. Elija un identificador de marketing corto y descriptivo para el servicio. Este se usará en las direcciones URL de Marketplace para esta oferta. Por ejemplo, si el identificador de anunciante es "contoso" y el identificador de marketing es "sampleApp", la dirección URL de la oferta en Azure Marketplace es `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`.  
| **Id. de suscripción de versión preliminar\*** | Agregue entre uno y 100 identificadores de suscripción de versión preliminar. Estas suscripciones permitidas tienen acceso a la oferta mientras está disponible en versión preliminar después de su publicación, antes de su lanzamiento.          |
| **Vínculos útiles**    | Como opción, puede proporcionar vínculos a varios recursos para los usuarios de su oferta, por ejemplo, soporte técnico, documentación, foros, etc.  Se recomienda agregar al menos un vínculo a la documentación.            |
| **Categorías sugeridas (5 como máximo)\*** | Seleccione una de cinco categorías. Las categorías seleccionadas se usan para asociar la oferta con las categorías de productos disponibles en Azure Marketplace y Azure Portal. Se muestran en páginas de navegación y en la página de detalles del producto. |
|  |  |


## <a name="marketing-artifacts"></a>Artefactos de marketing

El formulario Artefactos de marketing tiene los campos obligatorios y opcionales que se muestran en la siguiente captura de pantalla. Los campos obligatorios se indican con un asterisco (*).

![Formulario Artefactos de marketing](./media/azureapp-marketplace-artifacts.png)

En la siguiente tabla se describen los artefactos de marketing:

|      Campo         |    Descripción    |
|  ---------------   |  ---------------  |
| **Pequeña\***        | Logotipo pequeño: 40 x 40 píxeles en formato PNG     |
| **Mediana\***       | Logotipo mediano: 90 x 90 píxeles en formato PNG    |
| **Grande\***        | Logotipo grande: 115 x 115 píxeles en formato PNG   |
| **Ancho\***         | Logotipo ancho: 255 x 115 píxeles en formato PNG    |
| **Imagen prominente**           | Logotipo de imagen prominente opcional: 815 x 290 píxeles en formato PNG **Nota:** Una vez cargado el icono de imagen prominente, no se puede eliminar. |
| **Capturas de pantalla (5 como máximo)** |        Las capturas de pantalla se muestran en la página de detalles del producto. Son una excelente forma de comunicar de manera visual lo que hace la aplicación y cómo funciona. Por ejemplo, puede mostrar diagramas de arquitectura o ilustraciones de casos de uso. Las capturas de pantalla son opcionales y están limitadas a 5 por SKU. Para agregar una captura de pantalla:<ul><li>Seleccione **+ Agregar captura de pantalla** para abrir la ventana Captura de pantalla.</li><li>**Nombre**: escriba un nombre o título (de 100 caracteres como máximo).</li><li>**Cargar**: cargue la imagen. Debe estar en formato PNG, con un tamaño de 533 × 324 píxeles.</li></ul>           |
| **Agregar vídeo**      | Como opción, los vídeos se muestran en la página de detalles del producto. Son una excelente forma de comunicar de manera visual lo que hace la aplicación y cómo funciona. Para agregar un vídeo: <ul><li>Seleccione **+ Agregar vídeo** para abrir la ventana Vídeo.</li><li>**Nombre**: escriba un nombre o título (de 100 caracteres como máximo).</li><li>**Vínculo**: escriba la dirección URL del sitio que hospeda el vídeo (YouTube o Vimeo).</li><li>**Miniatura**: cargue una miniatura. Debe estar en formato PNG, con un tamaño de 533 × 324 píxeles.</li></ul>          |
|  |  |


### <a name="artifact-examples-in-azure-marketplace"></a>Ejemplos de artefactos en Azure Marketplace

La captura de pantalla siguiente muestra un ejemplo de un resultado de búsqueda en Marketplace.

![Resultados de la búsqueda de ofertas en Marketplace](./media/azureapp-marketplace-example-browse.png)

En la siguiente imagen se muestra cómo aparece la oferta en Marketplace después de que un cliente haga clic en el icono de la oferta en el resultado de la búsqueda.

![Detalles de los resultados de la búsqueda de ofertas en Marketplace](./media/azureapp-marketplace-example-details.png)


### <a name="artifact-examples-in-azure-portal"></a>Ejemplos de artefactos en Azure Portal

La siguiente captura de pantalla muestra cómo aparece una oferta en Azure Portal. En este ejemplo, la oferta de la aplicación se encuentra en **Marketplace > Todo > Desarrollo y prueba > Jenkins**. La oferta de Jenkins muestra un logotipo, un título y el nombre para mostrar del anunciante.

![Navegación por ofertas en Azure Portal](./media/azureapp-portalbrowse-artifacts-jenkins.png)

La captura de pantalla siguiente muestra información detallada acerca de la aplicación cuando un usuario selecciona Jenkins.

![Detalles de la oferta en Azure Portal](./media/azureapp-portal-artifacts-jenkins-details.png)


### <a name="logo-guidelines"></a>Directrices para logotipos

Todos los logotipos cargados en Cloud Partner Portal deben seguir estas instrucciones:

- El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.
- Los colores de tema de Azure Portal son el blanco y el negro. Evite emplearlos como color de fondo de los logotipos. Use un color que permita destacar los logotipos en Azure Portal. Nosotros recomendamos usar colores primarios simples. Si usa un fondo transparente, asegúrese de que el texto o los logotipos no sean de color azul, blanco o negro.
- No use un fondo degradado en el logotipo.
- Evite colocar texto, incluso la empresa o el nombre de marca, en el logotipo. El aspecto del logotipo debe ser "plano" y evitar los degradados.
- No estire el logotipo.


#### <a name="hero-logo"></a>Logotipo de imagen prominente

El logotipo de imagen prominente es opcional.

>[!IMPORTANT]
>Una vez cargado el logotipo de imagen principal, no se puede eliminar.

Use las siguientes directrices para un logotipo de imagen principal:

- Los fondos transparentes y de color negro o blanco no pueden usarse.
- Evite el uso de colores claros como fondo del logotipo. El nombre para mostrar del anunciante, el título del plan y el resumen largo de la oferta se muestran en color de fuente blanco y deben destacar contra el fondo.
- Evite el uso de mucho texto cuando diseñe el logotipo. El nombre del publicador, el título del plan, el resumen largo de la oferta y un botón Crear se insertan mediante programación dentro del logotipo cuando se publica la oferta.
- Incluya un espacio rectangular sin usar en el lado derecho del logotipo de imagen prominente. Este espacio en blanco tiene 415 x 100 píxeles y el desplazamiento desde la izquierda es de 370 píxeles.


## <a name="lead-management"></a>Administración de clientes potenciales

El formulario Administración de clientes potenciales tiene un campo opcional para configurar la administración de clientes potenciales. Para configurar la administración de clientes potenciales, seleccione el Destino de clientes potenciales en la lista desplegable. En la siguiente captura de pantalla se muestran los destinos disponibles.

![Selección del destino de gestión de clientes potenciales](./media/azureapp-marketplace-leadmgmt.png)

>[!TIP]
>Seleccione el icono de información para ver este mensaje: "Seleccione el sistema donde se van a almacenar los clientes potenciales. Obtenga información sobre cómo conectar su sistema CRM [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)".

Para más información, consulte [Configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads).


## <a name="legal"></a>Información legal

Use el formulario Información legal para proporcionar la documentación legal necesaria para cada oferta.

Proporcione la siguiente información:

- **Dirección URL de la directiva de privacidad\*** : escriba un vínculo a la directiva de privacidad de la aplicación.
- **Términos de uso\*** : escriba los términos de uso de la aplicación. Los clientes deben aceptar estos términos antes de poder probar la aplicación.

![Formulario Información legal](./media/azureapp-marketplace-legal.png)


## <a name="next-steps"></a>Pasos siguientes

[Pestaña Soporte técnico](./cpp-support-tab.md)
