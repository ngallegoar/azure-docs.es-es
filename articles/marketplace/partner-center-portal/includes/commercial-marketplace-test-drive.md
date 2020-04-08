---
title: archivo de inclusión
description: archivo de inclusión
documentationcenter: partner-center-commercial-marketplace
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
ms.date: 08/13/2019
ms.author: dsindona
ms.custom: include file
ms.openlocfilehash: f2446a924ceed37c51779efc9d9e94c0252a2067
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80321672"
---
La pestaña **Versión de prueba** permite configurar una demostración (o "versión limitada de prueba") que permitirá a los clientes probar su oferta antes de comprarla. Puede encontrar más información al respecto en el artículo [¿Qué es la versión de prueba?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Si ya no quiere ofrecer una versión de prueba de la oferta, vuelva a la página **Configuración de la oferta** y desactive **Habilitar versión de prueba**.

### <a name="technical-configuration"></a>Configuración técnica
Los siguientes tipos de versiones de prueba están disponibles, cada uno de ellos con sus propios requisitos de configuración técnica.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Aplicación lógica](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (la configuración técnica no es necesaria)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Configuración técnica para la versión de prueba de Azure Resource Manager

Una plantilla de implementación que contiene todos los recursos de Azure que componen la solución. Los productos que se ajustan a este escenario usan solo recursos de Azure. Más información acerca de cómo configurar una [versión de prueba de Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regiones** (se requiere): actualmente hay 26 regiones admitidas de Azure en las que se puede usar la versión de prueba. La habitual es que desee que su versión de prueba esté disponible en las regiones en que prevé un mayor número de clientes, con el fin de que puedan seleccionar la región más cercana para lograr el mejor rendimiento posible. Tendrá que asegurarse de que su suscripción puede implementar todos los recursos necesarios en cada una de las regiones que seleccione.

- **Instances**: seleccione el tipo (activa o inactiva) y el número de instancias disponibles, que va multiplicarán por el número de regiones en las que está disponible la oferta.

**Acceso frecuente**: Este tipo de instancia se implementa y está en espera de acceso por cada región seleccionada. Los clientes pueden acceder al instante a las instancias *frecuentes* de una versión prueba, en lugar de tener que esperar a una implementación. La contrapartida es que estas instancias se ejecutan siempre en su suscripción de Azure, por lo que incurrirán en un mayor costo de tiempo de actividad. Se recomienda tener al menos una instancia *frecuente*, ya que la mayoría de los clientes no quieren esperar a que terminen las implementaciones completas, lo que genera un descenso en la utilización por parte de los clientes si no hay ninguna instancia *frecuente*.

**Inactiva**: este tipo de instancia representa el número total de instancias que se pueden implementar por región. Las instancias inactivas requieren que se implemente toda la plantilla de la versión de prueba de Resource Manager en el momento en que el cliente solicita la versión de prueba, por lo que las instancias *inactivas* tardan mucho más tiempo en cargarse que las *activas*. A cambio solo tiene que pagar por la duración de la versión de prueba, *no* se ejecuta de forma ininterrumpida en su suscripción de Azure como las instancias *activas*.

- **Plantilla de versión de prueba de Azure Resource Manager**: Cargue el archivo .zip que contiene la plantilla de Azure Resource Manager.  Para más información acerca de cómo crear una plantilla de Azure Resource Manager, consulte el artículo del tutorial de inicio rápido [Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Duración de la versión de prueba (horas)** (se requiere): Especifique el período de tiempo que la versión de prueba permanecerá activa, en número de horas. La versión de prueba finaliza automáticamente cuando finaliza este período de tiempo. Esta duración solo se puede indicar con un número completo de horas (p. ej. "2" horas; "1,5" no es un valor válido).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Configuración técnica de la versión de prueba de Dynamics 365

Microsoft puede eliminar la complejidad de configurar una versión de prueba hospedando y manteniendo el aprovisionamiento y la implementación del servicio de mediante este tipo de versión de prueba. La configuración de este tipo de versión de prueba hospedada es el mismo, independientemente de que esta destinado a un público de Business Central, Customer Engagement u Operations.

- **Número máximo de versiones de prueba simultáneas** (se requiere): establezca el número máximo de clientes que puede usar la versión de prueba al mismo tiempo. Cada usuario simultáneo consumirá una licencia de Dynamics 365 mientras la versión de prueba esté activa, por lo que deberá asegurarse de que tiene suficientes licencias disponibles para admitir el límite máximo establecido. Se recomienda un valor entre 3 y 5.

- **Duración de la versión de prueba (horas)** (se requiere): Especifique el período de tiempo que la versión de prueba permanecerá activa. Para ello debe definir el número de horas. Después de este número de horas, la sesión finalizará y dejará de consumir una de las licencias. Se recomienda un valor que oscile entre 2 y 24 horas, en función de la complejidad de la oferta. Esta duración solo se puede indicar con un número completo de horas (p. ej. "2" horas; "1,5" no es un valor válido).  El usuario puede solicitar una nueva sesión si se agota el tiempo y desea volver a acceder a la versión de prueba.

- **Dirección URL de la instancia** (se requiere): la dirección URL en la que el cliente comenzará su versión de prueba. Normalmente es la dirección URL de la instancia de Dynamics 365 en la que se ejecuta la aplicación con los datos de ejemplo instalados (p. ej., `https://testdrive.crm.dynamics.com`).

- **Dirección URL de API web de la instancia** (se requiere): Recupere la dirección URL de la API web de la instancia de Dynamics 365. Para ello, inicie sesión en su cuenta de Microsoft 365 y vaya a **Configuración** \&gt; **Personalización** \&gt; **Recursos para desarrolladores** \&gt; **API web de instancia (URL raíz de servicio)** . Copie la dirección URL que se encuentra aquí (p. ej., `https://testdrive.crm.dynamics.com/api/data/v9.0`).

- **Nombre de rol** (se requiere): Proporcione el nombre del rol de seguridad que ha definido en su versión de prueba de Dynamics 365 personalizada. Se le asignará al usuario durante su versión de prueba (por ejemplo, test-drive-role).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Configuración técnica de la versión de prueba de Logic App

Los productos personalizados deben usar este tipo de plantilla de implementación de versión de prueba, que engloba varias arquitecturas de soluciones complejas. Para más información acerca de cómo configurar versiones de prueba de Logic App, visite [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) y [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) en GitHub.

- **Región** (se requiere, una lista desplegable con un solo elemento seleccionado): actualmente hay 26 regiones admitidas de Azure en las que se puede usar la versión de prueba. Los recursos de la aplicación lógica se implementarán en la región que seleccione. Si la aplicación lógica tiene recursos personalizados almacenado en una región concreta, asegúrese de que se selecciona aquí dicha región. La mejor forma de hacerlo es implementar totalmente Logic App de forma local en la suscripción de Azure en el portal y comprobar que funciona antes de realizar esta selección.

- **Número máximo de versiones de prueba simultáneas** (se requiere): establezca el número máximo de clientes que puede usar la versión de prueba al mismo tiempo. Estas versiones de prueba ya están implementadas, lo que permitiendo a los clientes acceder al instante a ellas sin tener que esperar que se implementen.

- **Duración de la versión de prueba (horas)** (se requiere): Especifique el período de tiempo que la versión de prueba permanecerá activa, en número de horas. La versión de prueba termina automáticamente al finalizar este período.

- **Nombre del grupo de recursos de Azure** (se requiere): Escriba el nombre del [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) en el que se guarda la versión de prueba de Logic App.

- **Nombre de aplicación lógica de Azure** (se requiere): escriba el nombre de la aplicación lógica que asigna a la versión de prueba al usuario. Dicha aplicación lógica debe guardarse en el grupo de recursos de Azure anterior.

- **Desaprovisionar nombre de aplicación lógica** (se requiere): Escriba el nombre de la aplicación lógica que desaprovisiona la versión de prueba una vez que el cliente ha finalizado. Dicha aplicación lógica debe guardarse en el grupo de recursos de Azure anterior.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>No es necesaria configuración técnica para las versiones de prueba de Power BI

Los productos que desean demostrar que un objeto visual interactivo de Power BI puede usar un vínculo insertado para compartir un panel personalizado como versión de prueba, no se necesita ninguna configuración técnica. Más información acerca de cómo configurar aplicaciones de la plantilla[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview).

### <a name="deployment-subscription-details"></a>Detalles de suscripción de implementación

Para implementar la versión de prueba en su nombre, cree y proporcione una suscripción de Azure independiente y única. (no es necesario para las versiones de prueba de Power BI).

- **Id. de suscripción de Azure** (necesario para Azure Resource Manager y Logic Apps): Escriba el identificador de la suscripción para conceder acceso a los servicios de la cuenta de Azure para la generación de informes y facturación del uso de los recursos. Se recomienda considerar la posibilidad de [crear una suscripción de Azure independiente](https://docs.microsoft.com/azure/billing/billing-create-subscription) que se use para las versiones de prueba, en caso de que no se tenga ninguna. Para buscar su identificador de la suscripción de Azure inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a la pestaña **Suscripciones** del menú izquierdo. Al seleccionar la pestaña se mostrará el identificador de la suscripción (por ejemplo, "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **Identificador de inquilino de Azure AD** (se requiere): Escriba el [id. de inquilino](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) de Azure Active Directory (AD). Para buscar dicho identificador, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Azure Active Directory del menú de la izquierda, seleccione **Propiedades** y busque el número de **identificador de directorio** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). El id. de inquilino de su organización también lo puede buscar mediante la dirección URL de su nombre de dominio en: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Nombre de inquilino de Azure AD** (se requiere para Dynamic 365): Escriba el nombre de Azure Active Directory (AD). Para buscar este nombre, inicie sesión en [Azure Portal](https://portal.azure.com/) y en la esquina superior derecha se mostrará su nombre del inquilino, debajo de su nombre de cuenta.

- **Identificador de aplicación de Azure AD** (se requiere): Escriba el [identificador de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in). Para buscarlo, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Active Directory del menú de la izquierda, seleccione **Registros de aplicaciones** y busque el número de **Id. de aplicación** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Secreto de cliente de aplicación de Azure AD** (se requiere): Escriba el [secreto de cliente](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)de su aplicación de Azure AD. Para encontrar este valor, inicie sesión en [Azure Portal](https://portal.azure.com/). Seleccione la pestaña **Azure Active Directory** en el menú izquierdo, seleccione **Registros de aplicaciones** y, después, seleccione la versión de prueba de su aplicación. A continuación, seleccione **Certificados y secretos**, seleccione **Nuevo secreto de cliente**, escriba una descripción, seleccione **Nunca** en **Expira** y, después, elija **Agregar**. No olvide copiar el valor. (no abandone la página sin hacerlo, ya que, de lo contrario no tendrá acceso al valor).

No olvide **guardar** antes de pasar a la siguiente sección.

### <a name="test-drive-listings-optional"></a>Anuncios de versión de prueba (opcional)

La opción **Anuncios de versión de prueba** de la pestaña **Versión de prueba** muestra los idiomas (y los mercados) en los que la versión de prueba; actualmente el inglés (Estados Unidos) es la única ubicación disponible. Además, esta página muestra el estado de la descripción específica del idioma y la fecha y hora en que se agregó. Deberá definir los detalles de unidad de prueba (descripción, manual del usuario, vídeos, etc.) para cada idioma o el mercado.

- **Descripción** (se requiere): se describe la versión de prueba, lo que se demostrará, los objetivos con los que experimentará el usuario, las características que se van a explorar y cualquier información relevante que ayude al usuario a determinar si debe adquirir la oferta. En este campo se puede escribir hasta 3000 caracteres de texto. 

- **Información de acceso** (se requiere para las versiones de prueba de Azure Resource Manager y Logic): se explica todo lo que los clientes necesitan saber para acceder a esta versión de prueba y usarla. se muestra un escenario para usar la oferta y exactamente lo que el cliente debe saber para acceder a las características en la versión de prueba. En este campo se puede escribir hasta 10 000 caracteres de texto.

- **Manual del usuario** (se requiere): un tutorial detallado de la experiencia con la versión de prueba. Debe abarcar exactamente lo que desee que el cliente aprenda al usar la versión de prueba y puede servir como referencia para las preguntas que puedan surgirle. El archivo debe estar en formato PDF y hay que asignarle un nombre (255 caracteres como máximo) después de cargarlo.

- **Vídeos: agregar vídeos** (opcional): Se pueden cargar vídeos en YouTube o Vimeo y hace referencia a ellos aquí con un vínculo o una imagen en miniatura (533 × 324 píxeles) para que los clientes puedan ver toda la información que pueda ayudarles a conocer mejor la versión de prueba, incluida la forma de usar correctamente las características de la oferta y los escenarios que destacan sus ventajas.
  - **Nombre** (se requiere)
  - **URL (solo YouTube o Vimeo)** (se requiere)
  - **Miniatura (533 x 324 px)** : El archivo de imagen debe estar en formato PNG.

Después de completar estos campos seleccione **Guardar**.
