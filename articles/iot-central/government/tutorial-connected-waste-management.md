---
title: 'Tutorial: Creación de una aplicación de administración de residuos conectada con Azure IoT Central'
description: 'Tutorial: Aprenda a crear una aplicación de administración de residuos conectada con las plantillas de aplicación de Azure IoT Central.'
author: miriambrus
ms.author: miriamb
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 274d725a0e7987c8cb15363ba276aec7ebc541d5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77426367"
---
# <a name="tutorial-create-a-connected-waste-management-application-in-iot-central"></a>Tutorial: Creación de una aplicación de administración de residuos conectada en IoT Central



Este tutorial le guía por los pasos para crear una aplicación de administración de residuos conectada con la plantilla de aplicación **Administración de residuos conectada** de Azure IoT Central. 

En este tutorial, aprenderá a: 

> [!div class="checklist"]

> * Usar la plantilla **Administración de residuos conectada** de Azure IoT Central para crear la aplicación de administración de residuos conectada
> * Explorar y personalizar el panel del operador 
> * Explorar la plantilla de dispositivo de cubo de basura conectado
> * Explorar los dispositivos simulados
> * Exploración y configuración de reglas
> * Trabajos de configuración
> * Realizar la personalización de marca de la aplicación mediante etiquetado.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:
-  Se recomienda una suscripción de Azure. Opcionalmente, puede usar una evaluación gratuita de 7 días. Si no tiene una suscripción de Azure, puede crear una en la [página de suscripción a Azure](https://aka.ms/createazuresubscription).

## <a name="create-connected-waste-management-app-in-iot-central"></a>Creación de una aplicación de administración de residuos conectada en IoT Central

En esta sección, se usará la plantilla **Connected waste management** de Azure IoT Central para crear la aplicación de administración de residuos conectada en IoT Central.

Para crear una aplicación de administración de residuos conectada de Azure IoT Central, siga estos pasos:  

1. Vaya a la [página principal de Azure IoT Central](https://aka.ms/iotcentral).

    Si tiene una suscripción a Azure, inicie sesión con las credenciales que usa para acceder a ella; si no la tiene, inicie sesión con una cuenta de Microsoft:

    ![Incorporación de la cuenta de la organización](./media/tutorial-connectedwastemanagement/sign-in.png)

2. Haga clic en **Build** (Crear) en el panel izquierdo y seleccione la pestaña **Government** (Administración Pública). En la página Government (Administración Pública) se muestran varias plantillas de aplicación.

    ![Plantillas de creación de aplicaciones de la Administración Pública](./media/tutorial-connectedwastemanagement/iotcentral-government-tab-overview.png)

1. Seleccione la plantilla de aplicación **Connected Waste Management**. Esta plantilla incluye la plantilla de dispositivo Connected Waste Bin, un dispositivo simulado, el panel del operador y las reglas de supervisión preconfiguradas.    

2. Haga clic en **Create app** (Crear aplicación); se abrirá el formulario de creación **New application** (Nueva aplicación) con los siguientes campos:
    * **Application name** (Nombre de la aplicación). De forma predeterminada, la aplicación usa *Connected waste management* seguido de una cadena de identificador única que genera IoT Central. Tiene la opción de elegir un nombre descriptivo para la aplicación. También puede cambiar el nombre de la aplicación más adelante.
    * **URL**: tiene la opción de elegir la dirección URL que prefiera. También puede cambiarla más adelante. 
    * Si tiene una suscripción de Azure, escriba el *directorio, la suscripción de Azure y la región*. Si no tiene una suscripción, puede activar **7-day free trial** (Evaluación gratuita de 7 días) y completar la información de contacto necesaria.  

    Para más información acerca de los directorios y las suscripciones, consulte la [guía de inicio rápido para crear una aplicación](../core/quick-deploy-iot-central.md).

5. En la parte inferior de la página, haga clic en **Create** (Crear). 

    ![Página de creación de una aplicación de residuos conectados de Azure IoT Central](./media/tutorial-connectedwastemanagement/new-application-connectedwastemanagement.png)
    
    ![Página de información de facturación de la creación de una aplicación conectada de Azure IoT Central](./media/tutorial-connectedwastemanagement/new-application-connectedwastemanagement-billinginfo.png)

 
6. Ahora ha creado una aplicación de administración de residuos conectados mediante la plantilla **Connected waste management** de Azure IoT Central. 

Felicidades. La aplicación recién creada incluye la siguiente configuración:
* Paneles de operadores de ejemplo
* Plantillas de dispositivo de ejemplo predefinidas de cubo de basura conectado
* Dispositivos de cubo de basura conectados
* Reglas y trabajos preconfigurados
* Ejemplo de personalización de marca mediante etiquetado 

Como es su aplicación, puede modificarla en cualquier momento. Ahora, explore la aplicación y realice algunas personalizaciones.  

## <a name="explore-and-customize-operator-dashboard"></a>Explorar y personalizar el panel del operador 
Después de crear la aplicación, llegará al panel **Wide Waste connected waste management dashboard**.

   ![Panel de administración de residuos conectada](./media/tutorial-connectedwastemanagement/connectedwastemanagement-dashboard1.png)

Como desarrollador, puede crear y personalizar las vistas del panel para los operadores. Pero antes de probar la personalización, es interesante explorar el panel. 

> [!NOTE]
> Todos los datos que se muestran en el panel se basan en los datos del dispositivo simulado, que se explorarán en la sección siguiente. 

El panel consta de diferentes tipos de iconos:

* ***Icono de imagen del servicio Wide World Waste***: el primer icono del panel es un icono de imagen de un servicio de residuos ficticio, "Wide World Waste". Puede personalizar el icono y colocar su propia imagen o quitarlo. 

* ***Icono de imagen de Waste bin***: puede usar iconos de imagen y contenido para crear una representación visual del dispositivo que se va a supervisar junto con un texto descriptivo. 

* ***Icono de KPI de nivel de llenado***: el icono muestra un valor devuelto por un sensor *Fill level* (Nivel de llenado) de un cubo de basura. *Fill level* (Nivel de llenado) y otros sensores como *Odor meter* (Medidor de olor) o *Weight* (Peso) de un cubo de basura se pueden supervisar de forma remota. Un operador puede tomar medidas, como enviar el camión de la basura. 

*  ***Waste monitoring area map***: el mapa usa Azure Maps, que puede configurar directamente en Azure IoT Central. El icono de mapa muestra la ubicación del dispositivo. Intente mantener el puntero sobre el mapa y pruebe los controles: acercar, alejar o ampliar.

     ![Mapa del panel de administración de residuos conectada](./media/tutorial-connectedwastemanagement/connectedwastemanagement-dashboard-map.png)


* ***Gráfico de barras de Fill, odor, weight level**: puede visualizar uno o varios datos de telemetría de los dispositivos en un gráfico de barras. También puede expandir este gráfico.  

  ![Gráfico de barras del panel de administración de residuos conectada](./media/tutorial-connectedwastemanagement/connectedwastemanagement-dashboard-barchart.png)


* **Icono de contenido de Field Services**: el panel incluye un vínculo a cómo realizar la integración con los servicios de campo de Dynamics 365 desde la aplicación de Azure IoT Central. Por ejemplo, puede usar Field Services para crear vales para enviar los servicios de basura. 


### <a name="customize-dashboard"></a>Personalización del panel 

Como desarrollador, puede personalizar las vistas del panel para los operadores. Pruebe lo siguiente:
1. Haga clic en **Edit** (Editar) para personalizar el panel **Wide World connected waste management dashboard**. Para personalizar el panel, haga clic en el menú **Edit** (Editar). Cuando el panel esté en modo de **edición**, puede agregar nuevos iconos o configurarlo. 

    ![Edición del panel](./media/tutorial-connectedwastemanagement/edit-dashboard.png)

2. También puede hacer clic en **+ New** (+ Nuevo) para crear un panel y configurarlo desde el principio. Puede tener varios paneles y navegar entre ellos desde el menú del panel. 

## <a name="explore-connected-waste-bin-device-template"></a>Explorar la plantilla de dispositivo de cubo de basura conectado

Una plantilla de dispositivo de Azure IoT Central define la funcionalidad de un dispositivo, que puede ser telemetría, propiedad o comando. Como desarrollador, puede definir plantillas de dispositivo que representen la funcionalidad de los dispositivos que se van a conectar. 
 

La aplicación **Administración de residuos conectada** incluye una plantilla de dispositivo de cubo de basura conectado de ejemplo.

Para ver la plantilla de dispositivo, siga estos pasos:

1. Haga clic en **Device templates** (Plantillas de dispositivo) en el panel izquierdo de la aplicación de IoT Central. 

    ![Plantilla de dispositivo](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devicetemplate.png)

2. En la lista de plantillas de dispositivos, verá **Connected Waste Bin**. Para abrirla, haga clic en el nombre.

3. Familiarícese con las funcionalidades de la plantilla de dispositivo. Puede ver que se definen sensores como *Fill level* (Nivel de llenado), *Odor meter* (Medidor de olor), *Weight* (Peso), *Location* (Ubicación), etc.

   ![Plantilla de dispositivo](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devicetemplate-connectedbin.png)

   

### <a name="customizing-the-device-template"></a>Personalización de la plantilla de dispositivo

Pruebe a personalizar lo siguiente:
1. Vaya a **Customize** (Personalizar) en el menú de la plantilla de dispositivo.
2. Busque el tipo de telemetría `Odor meter`.
3. Actualice el valor de **Display name** (Nombre para mostrar) de `Odor meter` a `Odor level`.
4. También puede intentar actualizar la unidad de medida o establecer *Min value* (Valor mínimo) y *Max value* (Valor máximo).
5. Haga clic en **Save** (Guardar) para guardar los cambios. 

### <a name="add-a-cloud-property"></a>Adición de una propiedad de la nube 

1. Vaya a **Cloud property** (Propiedad de la nube) en el menú de la plantilla de dispositivo.
2. Haga clic en **+ Add Cloud Property** (+ Agregar propiedad de la nube). En IoT Central, puede agregar a un dispositivo una propiedad que sea pertinente, aunque no se espera que la envíe el dispositivo. Por ejemplo, una propiedad de la nube podría ser un umbral de alerta específico del área de instalación, información de recursos o información de mantenimiento, etc. 
3. Haga clic en **Save** (Guardar) para guardar los cambios. 
 
### <a name="views"></a>Vistas 
* La plantilla de dispositivo de cubo de basura conectado incluye vistas predefinidas. Explore las vistas y realice actualizaciones. Las vistas definen cómo verán los operadores los datos del dispositivo, pero también establecen las propiedades de la nube. 

  ![Vistas de la plantilla de dispositivo](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devicetemplate-views.png)

### <a name="publish"></a>Publicar 

* Si realizó algún cambio, seleccione **Publish** (Publicar) para publicar la plantilla de dispositivo. 

### <a name="create-a-new-device-template"></a>Creación de una nueva plantilla de dispositivo 

* Seleccione **+ New** (+ Nueva) para crear una plantilla de dispositivo y siga el proceso de creación. Podrá crear una plantilla de dispositivo personalizada desde el principio o elegir una del catálogo de dispositivos de Azure. 

## <a name="explore-simulated-devices"></a>Explorar los dispositivos simulados

En IoT Central, puede crear dispositivos simulados para probar la aplicación y la plantilla de dispositivo. 

La aplicación **Administración de residuos conectada** tiene dos dispositivos simulados asignados a la plantilla de dispositivo de cubo de basura conectado. 

### <a name="to-view-the-devices"></a>Para ver los dispositivos, siga estos pasos:

1. Vaya a **Devices** (Dispositivos) en el panel izquierdo de IoT Central. 

   ![Dispositivos](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devices.png)

2. Seleccione y haga clic en el dispositivo Connected Waste Bin.  

     ![Dispositivo 1](./media/tutorial-connectedwastemanagement/connectedwastemanagement-devices-bin1.png)

3. Vaya a la pestaña **Cloud Properties** (Propiedades de la nube) e intente actualizar el valor `Bin full alert threshold` de `95` a `100`. 
* Explore las pestañas **Device Properties** (Propiedades del dispositivo) **Device Dashboard** (Panel del dispositivo). 

> [!NOTE]
> Tenga en cuenta que todas las pestañas se han configurado a partir de las **vistas de la plantilla de dispositivo**.

### <a name="add-new-devices"></a>Adición de nuevos dispositivos

* Para agregar nuevos dispositivos, haga clic en **+ New** (+ Nuevo) en la pestaña **Devices** (Dispositivos). 

## <a name="explore-and-configure-rules"></a>Exploración y configuración de reglas

En Azure IoT Central puede crear reglas para supervisar automáticamente la telemetría de los dispositivos y desencadenar acciones cuando se cumplan una o varias condiciones. Las acciones pueden incluir el envío de notificaciones por correo electrónico, el desencadenamiento de una acción de Microsoft Flow o una acción de webhook para enviar datos a otros servicios.

La aplicación **Administración de residuos conectada** tiene cuatro reglas de ejemplo.

### <a name="to-view-rules"></a>Para ver las reglas, siga estos pasos:
1. Vaya a **Rules** (Reglas) en el panel de navegación izquierdo de IoT Central.

   ![Reglas](./media/tutorial-connectedwastemanagement/connectedwastemanagement-rules.png)

2. Seleccione **Bin full alert**.

     ![Bin full alert](./media/tutorial-connectedwastemanagement/connectedwastemanagement-binfullalert.png)

 3. `Bin full alert` comprueba cuando **Condition** (Condición) es `Fill level is greater than or equal to Bin full alert threshold`.

    `Bin full alert threshold` es una *propiedad de la nube* definida en la plantilla de dispositivo `Connected waste bin`. 

Ahora, se creará una acción de correo electrónico.

### <a name="create-an-email-action"></a>Creación de una acción de correo electrónico
Para configurar una acción de correo electrónico en la lista de acciones de la regla:
1. Seleccione **+ Email** (+ Correo electrónico). 
2. Escriba *High pH alert* como valor descriptivo de **Display name** (Nombre para mostrar) de la acción.
3. Escriba la dirección de correo electrónico asociada a la cuenta de IoT Central en **To** (Para). 
4. Opcionalmente, escriba una nota para incluirla en el texto del correo electrónico.
5. Seleccione **Done** (Listo) para completar la acción.
6. Seleccione **Save** (Guardar) para guardar y activar la nueva regla. 

Recibirá un correo electrónico cuando se cumpla la **condición** configurada.

> [!NOTE]
> La aplicación enviará un correo electrónico cada vez que se cumpla una condición. **Deshabilite** la regla para dejar de recibir correo electrónico de la regla automatizada. 
  
Para crear una regla, siga estos pasos: 
1. Seleccione **+ Nueva** en la pestaña **Reglas** del panel izquierdo.

## <a name="configure-jobs"></a>Configuración de trabajos

En IoT Central, los trabajos permiten desencadenar actualizaciones de propiedades de la nube o del dispositivo en varios dispositivos. Además de las propiedades, los trabajos se pueden usar también para desencadenar comandos de dispositivo en varios dispositivos. IoT Central automatizará el flujo de trabajo. 

1. Vaya a **Jobs** (Trabajos) en el panel izquierdo. 
2. Haga clic en **+New** (+ Nuevo) y configure uno o varios trabajos. 


## <a name="customize-your-application"></a>Personalización de la aplicación 

Como creador, puede cambiar varias opciones de configuración para personalizar la experiencia del usuario en la aplicación.

### <a name="to-change-the-application-theme"></a>Para cambiar el tema de la aplicación:

1. Vaya a **Administration > Customize your application** (Administración > Personalizar la aplicación).
3. Use el botón **Change** (Cambiar) para elegir una imagen para cargar en el campo **Application logo** (Logotipo de la aplicación).
4. Use el botón **Change** (Cambiar) para elegir una imagen para **Browser icon** (Icono del explorador) que aparecerá en las pestañas del explorador.
5. También puede reemplazar los valores predeterminados de **Browser colors** (Colores del explorador) mediante la adición de códigos de color HTML hexadecimales.

   ![Personalización de la aplicación de Azure IoT Central](./media/tutorial-connectedwastemanagement/connectedwastemanagement-customize-your-application.png)

6. También puede cambiar las imágenes de la aplicación. Para ello, vaya a **Administration > Application settings** (Administración > Configuración de la aplicación) y haga clic en el botón **Select image** (Seleccionar imagen) para elegir la imagen que se va a cargar como imagen de la aplicación.
7. Por último, también puede cambiar el valor de **Theme** (Tema) haciendo clic en **Settings** (Configuración) en la cabecera de la aplicación.

  
## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, puede eliminarla mediante estos pasos:

1. Abra la pestaña Administration (Administración) en el panel izquierdo de la aplicación de IoT Central.
2. Seleccione la configuración de la aplicación y haga clic en el botón Eliminar situado en la parte inferior de la página.

  

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre los [conceptos de administración de residuos conectada](./concepts-connectedwastemanagement-architecture.md)
