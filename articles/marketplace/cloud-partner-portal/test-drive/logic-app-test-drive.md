---
title: Versión de prueba de aplicación lógica | Azure Marketplace
description: Explica cómo compilar su versión de prueba que se conecta con una instancia de Dynamics AX/CRM o con cualquier otro recurso distinto de Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 766f893d71ca0830fe8b69c50145603c6544cc3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278389"
---
<a name="logic-app-test-drive"></a>Versión de prueba de aplicación lógica
====================

Este artículo está pensado para anunciantes que tienen su oferta en AppSource y desean compilar una versión de prueba que se conecte con una instancia de Dynamics AX/CRM o con cualquier otro recurso distinto de Azure.

<a name="how-to-build-a-logic-app-test-drive"></a>Compilación de una versión de prueba de una aplicación lógica
-----------------------------------

La documentación sobre las versiones de prueba de una aplicación lógica se encuentra todavía en GitHub para [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) y [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app). Vaya allí para más información.

<a name="how-to-publish-a-test-drive"></a>Publicación de una versión de prueba
---------------------------

Ahora que ha compilado la versión de prueba, en esta sección se explica cada uno de los campos necesarios para publicarla correctamente.

![Habilitar la característica de versión de prueba](./media/azure-resource-manager-test-drive/howtopub1.png)

En el primer campo, y el más importante, debe decidir si desea que se le presente o no una prueba del formulario con todos los campos obligatorios para rellenarlos. Si selecciona **No**, se deshabilita el formulario y si vuelve a publicar con la versión de prueba deshabilitada, esta se eliminará de producción.

*Nota*: Si los usuarios usan activamente cualquier versión de prueba, esas versiones continuarán ejecutándose hasta que expiren sus sesiones.

### <a name="details"></a>Detalles

La siguiente sección que hay que rellenar corresponde a los detalles de la versión de prueba.

![Detalles de la versión de prueba](./media/azure-resource-manager-test-drive/howtopub2.png)

**Descripción:** *[campo obligatorio]* Aquí es donde se escribe la descripción principal sobre la versión de prueba. El cliente leerá esta descripción para conocer qué escenarios abarca la versión de prueba del producto. 

**Manual del usuario:** *[campo obligatorio]* Este es el tutorial detallado sobre su experiencia con la versión de prueba. El cliente lo abrirá y podrá seguir exactamente lo que se quiere que haga durante la versión de prueba. Es importante que este contenido sea fácil de entender y de seguir. (Debe ser un archivo .pdf)

**Vídeo de demostración de la versión de prueba:** \[Recomendable\] de forma parecida al Manual del usuario, es mejor incluir un tutorial en vídeo con la experiencia de la versión de prueba. El cliente lo verá antes o durante el uso de la versión de prueba y hará exactamente lo que desea que hagan con esa versión de prueba. Es importante que este contenido sea fácil de entender y de seguir.

- **Nombre**: título del vídeo.
- **Vínculo**: Debe ser una dirección URL insertada de YouTube o Vimeo. Abajo puede ver un ejemplo de cómo obtener la dirección URL insertada:
- **Miniatura**: Debe ser una imagen de alta calidad (533 × 324 píxeles). Aquí es recomendable realizar una captura de pantalla de alguna parte de su experiencia con la versión de prueba.

A continuación se muestra cómo aparecerán estos campos para el cliente durante su experiencia con la versión de prueba.

![Aspecto de los campos de la versión de prueba](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Configuración técnica

En la sección siguiente es donde puede configurar la versión de prueba de la aplicación lógica y definir cómo funciona concretamente esa instancia.

![Configuración técnica de la versión de prueba](./media/azure-resource-manager-test-drive/howtopub5_logicapp.png)

- **Región** -  *[campo obligatorio]* La región que seleccione es en la que se implementarán los recursos de la versión de prueba de la aplicación lógica.

    *Nota:* Si la aplicación lógica tiene algún recurso personalizado almacenado en una región, asegúrese de que se haya seleccionado esa región. La mejor forma de hacerlo es **implementar la aplicación lógica completamente de forma local en la suscripción de Azure del portal y comprobar que funciona** antes de escribir en ella aquí.

- **Número máximo de versiones de prueba simultáneas** -  *[campo obligatorio]* Número de instancias de versiones de prueba que ya están implementadas y que esperan acceso por cada región seleccionada. Los clientes tienen acceso inmediato a estas versiones de prueba en lugar de tener que esperar a una implementación.

    *Nota:* Si va a realizar un seminario web o clase en los que quiere que todos los alumnos hagan una versión de prueba, se recomienda publicar un número N de instancias frecuentes y, después, una vez que la clase haya terminado, volver a publicar el número de instancias frecuentes habitual.

- **Duración de la versión de prueba (horas):** *[campo obligatorio]* Duración del tiempo que la versión de prueba estará activa en número \# de horas. La versión de prueba finaliza automáticamente cuando finaliza este período de tiempo.

- **Nombre del grupo de recursos de Azure:** *[campo obligatorio]* Escriba el nombre del grupo de recursos en el que se guardan las versiones de prueba de la aplicación lógica.

- **Asignar nombre a la aplicación lógica:** *[campo obligatorio]* Escriba la aplicación lógica que se usa para asignar un usuario a la versión de prueba antes de que el cliente la obtenga, escriba aquí el nombre de esa aplicación lógica. Asegúrese de que este archivo se guarda en el grupo de recursos anterior.

- **Desaprovisionar nombre de la aplicación lógica:** *[campo obligatorio]* Escriba el nombre de la aplicación lógica para desaprovisionarla de todos los recursos que se crearon en la versión de prueba. Asegúrese de que este archivo se guarda en el grupo de recursos anterior.

- **Información de acceso:** *[campo obligatorio]* después de que un cliente obtiene su versión de prueba, se le presenta la información de acceso. Estas instrucciones tienen por objeto compartir los parámetros de salida útiles de la plantilla de Resource Manager de la versión de prueba. Para incluir parámetros de salida, use corchetes dobles (por ejemplo, **{{outputname}}** ) y se insertarán correctamente en la ubicación. (En este caso se recomienda usar el formato de cadena HTML para la representación en el front-end).

### <a name="test-drive-deployment-subscription-details"></a>Detalles de suscripción de la implementación de la versión de prueba

La última sección que hay que rellenar permite implementar las versiones de prueba automáticamente mediante la conexión de la suscripción de Azure y Azure Active Directory (AD).

![Detalles de suscripción de la implementación de la versión de prueba](./media/azure-resource-manager-test-drive/subdetails1.png)

**Identificador de suscripción de Azure** *[campo obligatorio]* Este concede acceso a servicios de Azure y a Azure Portal. En la suscripción es donde se notifica la utilización de recursos y se facturan los servicios. Si no tiene todavía una suscripción de Azure **independiente** solo para las versiones de prueba, consiga una. Para encontrar los identificadores de suscripción de Azure, inicie sesión en Azure Portal y navegue a Suscripciones en el menú de la izquierda.
(Ejemplo: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Suscripciones de Azure](./media/azure-resource-manager-test-drive/subdetails2.png)

**Identificador de inquilino de Azure AD** *[campo obligatorio]* Si ya tiene un identificador de inquilino disponible, puede encontrarlo en Propiedades -\> Identificador de directorio.

![Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails3.png)

En caso contrario, cree un inquilino en Azure Active Directory.

![Pantalla de propiedades de Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails4.png)

!Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails5.png)

![Inquilinos de Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails6.png)

**Identificador de aplicación de Azure AD** *[campo obligatorio]* El siguiente paso consiste en crear y registrar una nueva aplicación. Esta aplicación se usará para realizar operaciones en la instancia de la versión de prueba.

1. Navegue al directorio recién creado o a uno que ya existe y seleccione Azure Active Directory en el panel de filtro.
2. Busque "Registros de aplicaciones" y haga clic en "Agregar".
3. Proporcione un nombre para la aplicación.
4. Como Tipo, seleccione "Aplicación web o API".
5. Puede proporcionar cualquier valor en URL de inicio de sesión ya que ese campo no se usará.
6. Haga clic en Crear.
7. Cuando haya creado la aplicación, vaya a Propiedades -\> Set the application as multi-tenant (Establecer la aplicación como multiinquilino) y presione Guardar.

Haga clic en Guardar. El último paso consiste en copiar el identificador de aplicación de esta aplicación registrada y pegarlo en el campo Versión de prueba.

![Identificador de la aplicación de Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails7.png)

Dado que vamos a usar la aplicación para implementar en la suscripción, es necesario agregar la aplicación como colaborador en la suscripción. Las instrucciones son las siguientes:

1. Vaya a la hoja Suscripciones y seleccione la suscripción adecuada que va a usar solo para la versión de prueba.
1. Haga clic en **Control de acceso (IAM).**
1. Haga clic en la pestaña **Asignaciones de roles**.  ![Azure Active Directory, incorporación de una nueva entidad de seguridad de Access Control](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. Haga clic en **Agregar asignación de roles**.
1. Establezca el rol en **Colaborador**.
1. Escriba el nombre de la aplicación de Azure AD y selecciónela para asignar el rol.
    ![Permisos de Azure Active Directory](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. Haga clic en **Save**(Guardar).

**Clave de aplicación de Azure AD** *[campo obligatorio]* El campo final es para generar una clave de autenticación. En Claves, agregue una descripción de la clave, establezca la duración para que no expire nunca y luego seleccione Guardar. Esto es **importante** para evitar tener una clave expirada, lo que interrumpiría la versión de prueba en producción. Copie este valor y péguelo en el campo obligatorio Versión de prueba.

![Sección de claves de Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails8.png)

> [!CAUTION]
> No puede usar la versión preliminar de registro de Aplicación de Azure porque actualmente no generar una clave codificada en base64.


<a name="next-steps"></a>Pasos siguientes
----------

Ahora que ha rellenado todos los campos de la versión de prueba, revíselos y elija **Volver a publicar** la oferta. Una vez que la versión de prueba ha pasado el proceso de certificación, debe comprobar ampliamente la experiencia del cliente en la **versión preliminar** de la oferta. Inicie una versión de prueba en la interfaz de usuario y compruebe que las versiones de prueba se han implementado correctamente.

Es importante tener en cuenta que no ha eliminado ninguna parte de la versión de prueba ya que están aprovisionadas para los clientes, por lo que el servicio de versión de prueba limpiará automáticamente estos grupos de recursos una vez que el cliente haya finalizado.

Cuando esté satisfecho con la oferta de versión preliminar, **publíquela**. Hay un proceso de revisión final que lleva a cabo Microsoft una vez publicada la oferta que comprueba de nuevo toda la experiencia. Si por algún motivo se rechaza la oferta, le enviaremos una notificación al contacto de ingeniería de la oferta explicando qué se necesita para arreglarla.

Si tiene más preguntas, busca consejos de solución de problemas o desea hacer la versión de prueba más satisfactoria, consulte las secciones de [preguntas más frecuentes, solución de problemas y procedimientos recomendados](./marketing-and-best-practices.md).
