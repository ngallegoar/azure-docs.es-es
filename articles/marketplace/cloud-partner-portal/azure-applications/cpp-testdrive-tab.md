---
title: Versión de prueba de la oferta de aplicación de Azure | Azure Marketplace
description: Cómo configurar la versión de prueba para una oferta de aplicación de Azure en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 2f35dd69c1f29350049ed8f62dd9cbb0e60a7718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289012"
---
# <a name="azure-applications-test-drive-tab"></a>Pestaña Versión de prueba de aplicaciones de Azure

Use la pestaña Versión de prueba para proporcionar una experiencia de evaluación para sus clientes.

## <a name="test-drive-benefits"></a>Ventajas de la versión de prueba

La creación de una experiencia de evaluación para los clientes es un procedimiento recomendado para garantizar que puedan comprar con confianza. De las opciones de prueba disponibles, Versión de prueba es la más eficaz a la hora de generar clientes potenciales de alta calidad y lograr un aumento en la conversión de estos.

Proporciona a los clientes una versión de prueba práctica y autodidacta con las principales características y ventajas del producto, y una demostración en un escenario de implementación del mundo real.

## <a name="how-a-test-drive-works"></a>Funcionamiento de una versión de prueba

Un cliente potencial busca y encuentra la aplicación en Marketplace. El cliente inicia sesión y acepta los términos de uso. En ese momento, el cliente recibe su entorno preconfigurado de prueba durante un número determinado de horas, mientras que usted recibe una notificación de cliente potencial altamente cualificado que le permite realizar un seguimiento del mismo. Para más información, consulte [¿Qué es la versión de prueba?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

## <a name="setting-up-a-test-drive"></a>Configuración de una versión de prueba

Para habilitar y configurar una versión de prueba, siga los pasos descritos a continuación:

### <a name="to-enable-a-test-drive"></a>Para habilitar una versión de prueba:

1. En **Nueva oferta**, seleccione la pestaña **Versión de prueba**.
2. En **Versión de prueba**, seleccione **Sí** en la opción **Enable a Test Drive** (Habilitar una versión de prueba).

   ![Habilitación de una versión de prueba](./media/managed-app-enable-testdrive.png)

### <a name="to-configure-a-test-drive"></a>Para configurar una versión de prueba:

Después de habilitar una versión de prueba, deberá rellenar los formularios siguientes para configurar la versión de prueba:
  
 - Detalles
 - Configuración técnica
 - Detalles de suscripción de la implementación de la versión de prueba

La captura de pantalla siguiente muestra todos los formularios de Versión de prueba. Un asterisco (*) junto al nombre del campo indica que es obligatorio. 

![Configuración de una versión de prueba](./media/managed-app-configure-testdrive.png)

En la tabla siguiente se describen los campos obligatorios para configurar la versión de prueba de la aplicación administrada.  Los campos anexados con un asterisco son obligatorios.

|      Campo         |  Descripción      |
|  ---------------   |  ---------------  |
| **Descripción\***  |  Describe lo que puede realizarse en la versión de prueba. Puede usar etiquetas HTML básicas para dar formato a esta descripción. Por ejemplo, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; y títulos.                |
| **Manual del usuario\***  |  Cargue un manual del usuario que los clientes puedan usar para completar la experiencia de versión de prueba. Este documento debe ser un archivo PDF.    |
| **Vídeo de demostración de la versión de prueba** |  Tutorial en vídeo opcional de la versión de prueba. Un cliente puede ver este vídeo antes de realizar una prueba. Proporcione una dirección URL de YouTube o Vimeo para el vídeo. Si selecciona **+ Agregar vídeo**, se le solicitará que proporcione la siguiente información:<ul><li>Nombre</li><li>URL</li><li>Miniatura (formato PNG, 533 x 324 píxeles)</li></ul>  |
| **Instancias\***      | Configure el número de instancias que desea, en qué regiones y la rapidez con la que sus clientes pueden obtener la versión de prueba. Para más información, consulte [Publicación de una versión de prueba](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive#how-to-publish-a-test-drive).           |
| **Duración de la versión de prueba (horas)\*** | Escriba un número entero para el número de horas. El intervalo permitido es de 1 a 999. |
| **Plantilla ARM de la versión de prueba\***     | Cargue un archivo comprimido (.zip) que tenga las plantillas de Azure Resource Manager para su aplicación. Para más información, consulte [Versión de prueba de Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive). |
| **Información de acceso\***          | Proporcione información de acceso después de que el cliente obtenga la versión de prueba. Por ejemplo, una dirección URL para acceder a la versión de prueba y firmar información. . Puede usar etiquetas HTML básicas para dar formato a esta descripción. Por ejemplo, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; y títulos. |
| **Identificador de suscripción de Azure\***       | Este identificador concede acceso a servicios de Azure y a Azure Portal. En la suscripción es donde se notifica la utilización de recursos y se facturan los servicios. Si aún no tiene una suscripción de Azure independiente solo para las versiones de prueba, cree una.  |
| **Identificador de inquilino de Azure AD\***          | Proporcione un inquilino de Azure Active Directory existente o cree un inquilino para esta versión de prueba.  |
| **Identificador de aplicación de Azure AD\***             | Cree y registre una nueva aplicación. Microsoft utiliza esta aplicación para realizar operaciones en la instancia de la versión de prueba.  |
| **Clave de aplicación de Azure AD\***            | Cree una clave de autenticación para la aplicación y péguela en este campo.   |
|  |  |

Después de proporcionar toda la información necesaria, seleccione **Guardar** para finalizar la configuración de la versión de prueba.


## <a name="next-steps"></a>Pasos siguientes

[Pestaña Marketplace](./cpp-marketplace-tab.md)
