---
title: Pestaña Versión de prueba de máquina virtual en Cloud Partner Portal para Azure Marketplace
description: Se describe la pestaña Versión de prueba que se usa en la creación de una oferta de máquina virtual de Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: d4fc8762e25825b21637b16b751d57a0dcbf369e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288808"
---
# <a name="virtual-machine-test-drive-tab"></a>Pestaña Versión de prueba de máquina virtual

La pestaña **Versión de prueba** de la página **Nueva oferta** permite proporcionar a los clientes potenciales una demostración práctica y autodidacta de las principales características y ventajas del producto, ilustradas en un escenario estandarizado.  Versión de prueba es una característica opcional para los tipos de oferta que admiten la versión de prueba.  Versión de prueba requiere que los recursos complementarios se implementen correctamente.  Para obtener más información, vea el artículo [Azure Marketplace Test Drive](https://azure.microsoft.com/blog/azure-marketplace-test-drive/) (Versión de prueba de Azure Marketplace).  <!--TD: Replace with migrated version of Test Drive article! -->

Para habilitar esta característica, en la pestaña **Versión de prueba**, haga clic en la opción **Sí** en **Enable a Test Drive** (Habilitar una versión de prueba).  En la pestaña **Versión de prueba** se muestran los campos disponibles para editar.  Un asterisco (*) anexo al nombre del campo indica que es obligatorio.

![Pestaña Versión de prueba en el formulario Nueva oferta para máquinas virtuales](./media/publishvm_007.png)


## <a name="field-values"></a>Valores de campo

En la tabla siguiente se describen la finalidad y el contenido de estos campos.  Los campos obligatorios se indican con un asterisco (*).


|    Campo                  |       Descripción                                                            |
|  ---------                |     ---------------                                                          |
|  *Detalles*   |  |
| **Descripción\***           | Proporciona una introducción del escenario de versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo admite HTML básico, si quiere proporcionar contenido con formato.  |
| **Manual del usuario\***           | Carga un manual del usuario detallado (.pdf) que ayuda a los usuarios de la versión de prueba a aprender a usar la solución.  |
| **Vídeo de demostración de la versión de prueba** | Carga un vídeo en el que se presente la solución.  Si elige esta opción, debe proporcionar un nombre, una dirección URL del vídeo (hospedado en YouTube o Vimeo) y una miniatura (533 x 324 píxeles) del vídeo. |
| *Configuración técnica* |  |
| **Instancias\***             | Especifica la disponibilidad de la región y la disponibilidad relativa de la instancia de máquina virtual (haga clic en el icono de información para obtener más detalles).  <br/>Las sesiones simultáneas posibles de Versión de prueba no deben superar el límite de cuota para la suscripción.  Lo anterior se calcula de esta forma: [Número de regiones seleccionadas] x [Instancias activas] + [Número de regiones seleccionadas] x [Instancias semiactivas] + [Número de regiones seleccionadas] x [Instancias inactivas] |
| **Duración de la versión de prueba\***   | Duración máxima de la sesión en horas. La sesión de la versión de prueba finaliza de forma automática cuando finaliza este período de tiempo.  |
|**Plantilla ARM de la versión de prueba\***| Carga la plantilla de Azure Resource Manager asociada con esta versión de prueba. Para obtener más información, vea [Transforming Virtual Machine Deployment Template for Test Drive](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive) (Transformación de la plantilla de implementación de máquina virtual para la versión de prueba). |
| **Información de acceso\***    | Información de acceso de Azure Resource Manager e inicio de sesión de prueba, escrita como texto sin formato o HTML simple. |
| *Detalles de la suscripción de implementación de la versión de prueba* |  |
| **Identificador de suscripción de Azure\*** | Se puede obtener al iniciar sesión en [Microsoft Azure Portal](https://ms.portal.azure.com) y hacer clic en **Suscripciones** en la barra de menús de la izquierda. (Ejemplo: "a83645ac-1234-5ab6-6789-1h234g764ghty") Este identificador debe ser un GUID con el formato `a83645ac-1234-5ab6-6789-1h234g764ghty`.|
| **Identificador de inquilino de Azure AD\***    | Identificador del inquilino de Azure Active Directory.  Se puede obtener si inicia sesión en [Microsoft Azure Portal](https://ms.portal.azure.com), hace clic en **Azure Active Directory** en la barra de menús de la izquierda, luego en **Propiedades** en la barra de menús central y, después, copia el **Id. de directorio** desde el formulario.  Este identificador también debe ser un GUID.  Si está en blanco, debe crear un identificador de inquilino para la organización. |
| **Identificador de aplicación de Azure AD\***       | Identificador de la solución de máquina virtual de Azure registrada  |
| **Clave de aplicación de Azure AD\***      | Clave de autenticación para la solución registrada |
|   |   |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña [Marketplace](./cpp-marketplace-tab.md) siguiente, proporcionará información de marketing y legal sobre la solución.
