---
title: Configuración de laboratorios de Project Lead the Way con Azure Lab Services
description: Obtenga información sobre cómo configurar laboratorios para impartir clases de Project Lead the Way.
ms.topic: article
ms.date: 10/28/2020
ms.openlocfilehash: e3783ae4fa07bf783841022903c4bcf3ab6fbe23
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94648011"
---
# <a name="set-up-labs-for-project-lead-the-way-classes"></a>Configuración de laboratorios para las clases de Project Lead the Way

[Project Lead the Way (PLTW](https://www.pltw.org/)) es una organización sin ánimo de lucro que proporciona un currículum de preescolar, primera y secundaria en informática, ingeniería y ciencias biomédicas en Estados Unidos.  En cada clase de PLTW, los alumnos usan varias aplicaciones de software como parte de su experiencia de aprendizaje práctico.  Muchas de las aplicaciones de software requieren una CPU rápida o, en algunos casos, una GPU.  En este artículo se muestra cómo configurar laboratorios para las siguientes clases de PLTW, que normalmente se imparten a los alumnos de los cursos de 9 a 12:

- **Introducción al diseño de ingeniería**

    Los alumnos se introducen en el proceso de diseño de ingeniería, que incluye el uso del software de [diseño asistido por PC (CAD) Autodesk Inventor](https://www.autodesk.com/products/inventor/new-features) para el modelado en 3D.

- **Principios de ingeniería**
    
    Los alumnos adquieren aprendizaje sobre los mecanismos de ingeniería, la resistencia estructural o de los materiales y la automatización.  Esta clase usa software como [MD Solids](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/MD+Solids/MD+Solids+Software+Installation+Guide.pdf), [West Point Bridge Designer](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/West+Point+Bridge+Builder/Installation+Guide+for+West+Point+Bridge+Designer.pdf) y [America’s Army Simulation](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/America's+Army/Installation+Guide+for+Americas+Army+Simulation+17-18.pdf).

- **Arquitectura e ingeniería civil**

    Los alumnos adquieren conocimientos de construcción y de diseño y desarrollo de sitios con el software de diseño de arquitectura [Autodesk Revit](https://www.autodesk.com/products/revit/overview) para el modelado de información de construcción en 3D (BIM).

- **Fabricación integrada en PC**

    Los alumnos exploran los procesos de fabricación modernos, que incluyen robótica y automatización.   En esta clase, los alumnos usan el software de [CAD Autodesk Inventor](https://www.autodesk.com/products/inventor/new-features) y de [fabricación asistida por PC (CAM) de Autodesk Inventor](https://www.autodesk.com/products/inventor-cam/overview). 

- **Electrónica digital**

    Los alumnos estudian los circuitos lógicos y los dispositivos electrónicos con el software de simulación y diseño de circuitos [National Instrument’s Multisim](https://www.ni.com/en-us/shop/electronic-test-instrumentation/application-software-for-electronic-test-and-instrumentation-category/what-is-multisim.html).

- **Diseño y desarrollo de ingeniería**

    Los alumnos contribuyen a una solución completa que combina investigación, diseño y pruebas, que presentan a un panel de ingenieros.  En esta clase, los alumnos usan software [CAD de Autodesk Inventor](https://www.autodesk.com/products/inventor/new-features).

- **Conceptos básicos de la informática**

    Se presentan conceptos y herramientas de cálculo a los alumnos.  Comienzan con la programación basada en bloque y después pasan a usar la codificación basada en texto mediante entornos de codificación, como [VEXcode V5 Blocks](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/VEXcode+V5+Blocks/VexCode+V5+Blocks+Installation+Guide.pdf).

- **Principios de la informática**
    
    Los alumnos amplían su experiencia de programación con [Python](https://www.python.org/) mediante el [entorno de desarrollo de Microsoft Visual Studio Code](https://code.visualstudio.com/). 

- **Informática A**

    En esta clase, los alumnos desarrollan su experiencia de programación mediante el aprendizaje de desarrollo de aplicaciones móviles.  En esta clase, aprenden a usar [Java](https://www.java.com/) con el [entorno de desarrollo de Microsoft Visual Studio Code](https://code.visualstudio.com/).  Los alumnos también usan un emulador que les permite ejecutar y probar el código de su aplicación móvil.  Para obtener información sobre cómo configurar un emulador en Azure Labs, póngase en contacto con nosotros en la dirección azlabspilot@microsoft.com.

Consulte el sitio de PLTW para obtener la [lista de software completa](https://www.pltw.org/pltw-software) para cada clase.

## <a name="lab-configuration"></a>Configuración del laboratorio
Para configurar laboratorio de PLTW, necesita una suscripción a Azure y una cuenta de laboratorio para empezar. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. Una vez que obtenga una suscripción a Azure, puede crear una nueva cuenta de laboratorio en Azure Lab Services. Para más información sobre la creación de una nueva cuenta de laboratorio, consulte el tutorial sobre la [configuración de una cuenta de laboratorio](./tutorial-setup-lab-account.md). También puede usar una cuenta de laboratorio existente.

Una vez que tenga una cuenta de laboratorio, debe crear laboratorios independientes para cada sesión de una clase de PLTW que su escuela ofrezca.  También se recomienda crear imágenes independientes para cada tipo de clase de PLTW.  Para obtener más información sobre cómo estructurar sus laboratorios e imágenes, lea la entrada de blog: [Traslado de un laboratorio físico a Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931).

### <a name="lab-account-settings"></a>Configuración de la cuenta de laboratorio
Habilite la configuración que se describe en la tabla siguiente para la cuenta de laboratorio. Para obtener más información sobre cómo habilitar imágenes de marketplace, consulte el artículo sobre la [especificación de las imágenes de Marketplace disponibles para los creadores de laboratorios](./specify-marketplace-images.md).

| Configuración de la cuenta de laboratorio | Instructions |
| -------------------- | ----- |
| Imagen de Marketplace | Habilite la imagen de Windows 10 Pro para su uso en la cuenta de laboratorio. |

### <a name="lab-settings"></a>Configuración del laboratorio
El tamaño de la VM que se recomienda usar para las clases de PLTW depende de los tipos de cargas de trabajo que asumen los alumnos en la clase.  Para las clases anteriores, se recomienda usar tamaños de VM de GPU grandes y pequeños (visualización).  Consulte las instrucciones de la tabla siguiente al configurar los laboratorios para las clases de PLTW.

| Configuración del laboratorio | Valor/instrucciones |
| ------------ | ------------------ |
|Tamaño de la máquina virtual| **GPU pequeña (visualización)** .  Esta máquina virtual es más adecuada para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX.  Se recomienda usar este tamaño para las siguientes clases de PLTW: Arquitectura e ingeniería civil, Electrónica digital, Fabricación integrada en PC, y Diseño y desarrollo de ingeniería.
|Tamaño de la máquina virtual| **Grande**.  Este tamaño es ideal para aplicaciones que necesitan CPU más rápidas, un mejor rendimiento de los discos locales, bases de datos grandes y memorias caché grandes.  Se recomienda usar este tamaño para las siguientes clases de PLTW: Introducción al diseño de ingeniería, Principios de ingeniería, Conceptos básicos de la informática, Principios de la informática e Informática A.

### <a name="licensing-server"></a>Servidor de licencias
La mayor parte del software que se usa en las clases de PLTW anteriores **_no_* _ requiere acceso a un servidor de licencias.  Sin embargo, tendrá que acceder a un servidor de licencias si tiene previsto usar el modelo de licencias de red de Autodesk para el siguiente software:
-   Revit
-   Inventor
-   Inventor CAM

Para usar las licencias de red con el software de Autodesk, [PLTW proporciona pasos detallados](https://www.pltw.org/pltw-software) para instalar el administrador de licencias de Autodesk en el servidor de licencias.  Este servidor de licencias se encuentra normalmente en la red local u hospedado en una máquina virtual (VM) de Azure en Azure Virtual Network (VNet).

Una vez configurado el servidor de licencias, tendrá que [emparejar la red virtual](./how-to-connect-peer-virtual-network.md) con la [cuenta de laboratorio](./tutorial-setup-lab-account.md). El emparejamiento de redes debe realizarse _antes* de crear el laboratorio para que las máquinas virtuales de este puedan acceder al servidor de licencias y viceversa.

Los archivos de licencia generados por Autodesk insertan la dirección MAC del servidor de licencias.  Si decide hospedar el servidor de licencias mediante una VM de Azure, es importante que se asegure de que la dirección MAC del servidor de licencias no cambie.   De lo contrario, si la dirección MAC cambia, los archivos de licencias deberán volver a generarse.  Siga estas sugerencias para evitar que la dirección MAC cambie:

- [Establezca una dirección IP privada estática y una dirección MAC](./how-to-create-a-lab-with-shared-resource.md#static-private-ip-and-mac-address) para la VM de Azure que hospeda el servidor de licencias.
- Asegúrese de configurar la red virtual del servidor de licencias y la cuenta del laboratorio en una región o ubicación que tenga suficiente capacidad de VM para no tener que trasladar estos recursos a otra región o ubicación más adelante.

Además, lea el artículo sobre [cómo configurar un servidor de licencias como un recurso compartido](./how-to-create-a-lab-with-shared-resource.md) para obtener más información.

### <a name="template-machine"></a>Máquina de plantilla
Algunos de los archivos de instalación que necesita para PLTW son grandes y tardan mucho tiempo en copiarse cuando se descargan en la máquina de plantilla de un laboratorio.

En lugar de descargar los archivos de instalación en la máquina de plantilla e instalarlo todo allí, se recomienda crear las imágenes de PLTW en el entorno físico.  Después, puede importar las imágenes en Shared Image Gallery y usar estas imágenes personalizadas para crear sus laboratorios.  Lea el siguiente artículo para obtener más información: [Carga de una imagen compartida en Shared Image Gallery](./upload-custom-image-shared-image-gallery.md).

Siguiendo esta recomendación, las siguientes son las tareas principales para configurar un laboratorio:

1. En el entorno físico, cree la imagen para la clase.

    a.  Use los pasos detallados de PLTW para descargar los archivos de instalación e instalar el software necesario.

    > [!NOTE]    
    > Al instalar aplicaciones de Autodesk, el equipo en el que se instala Autodesk debe poder comunicarse con el servidor de licencias (el Asistente para instalación de Autodesk le pedirá que especifique el nombre de equipo de la máquina que hospeda el servidor de licencias).  Si hospeda el servidor de licencias en una VM de Azure, es posible que tenga que esperar para instalar Autodesk en la máquina de plantilla del laboratorio para que el Asistente para instalación de Autodesk pueda acceder al servidor de licencias.

    b.  [Instale y configure OneDrive](./how-to-prepare-windows-template.md#install-and-configure-onedrive) (u otras opciones de copia de seguridad que pueda usar su escuela).
    
    c.  [Instale y configure las actualizaciones de Windows](./how-to-prepare-windows-template.md#install-and-configure-updates).

1.  Cargue la imagen personalizada en la instancia de [Shared Image Gallery conectada a su cuenta de laboratorio](./how-to-attach-detach-shared-image-gallery.md).

1.  Cree un laboratorio y seleccione la imagen personalizada que cargó en el paso anterior.

1.  Una vez creado el laboratorio, inicie la máquina de plantilla y conéctese a esta para asegurarse de que la imagen funciona según lo previsto.

1.  Por último, publique la máquina de plantilla para crear las VM de los alumnos.

## <a name="student-devices"></a>Dispositivos de los alumnos
Los alumnos pueden conectarse a sus VM de laboratorio desde equipos Windows\Mac y Chromebook.  Estos son los vínculos a las instrucciones para cada una de estas opciones:

- [Conexión desde Windows](./how-to-use-classroom-lab.md#connect-to-the-vm)
- [Conexión desde Mac](./connect-virtual-machine-mac-remote-desktop.md)
- [Conexión desde Chromebook](./connect-virtual-machine-chromebook-remote-desktop.md)

## <a name="cost"></a>Coste
Vamos a cubrir una posible estimación de costos para las clases de PLTW anteriores.  Esta estimación no incluye el costo de la ejecución de un servidor de licencias ni el uso de la Shared Image Gallery.  Usaremos una clase de 25 alumnos.  Hay 20 horas de tiempo de clase programado.  Además, cada alumno obtiene una cuota de 10 horas para deberes o tareas fuera del tiempo de clase programado.  Consulte las siguientes estimaciones de costos para los tamaños de **GPU grande** y **pequeño (visualización)** .

- **VM grande**

    25 alumnos x (20 horas programadas + 10 horas de cuota) x 70 unidades de laboratorio x 0,01 USD por hora = 525,00 USD

- **GPU pequeña (visualización)**

    25 alumnos x (20 horas programadas + 10 horas de cuota) x 160 unidades de laboratorio * 0,01 USD por hora = 1200,00 USD

> [!IMPORTANT] 
> La estimación de costos solo se utiliza con fines de ejemplo.  Para conocer los detalles actuales sobre los precios, consulte [Precios de Azure Lab Services](https://azure.microsoft.com/pricing/details/lab-services/).

> [!NOTE] 
> Muchas de las clases de PLTW usan aplicaciones a las que se accede a través de un explorador, como MIT App Inventor.  Estas aplicaciones basadas en explorador no requieren una CPU o una GPU rápida, y son accesibles desde cualquier dispositivo que tenga una conexión a Internet.  Cuando los alumnos usan estos tipos de aplicaciones, se recomienda usar el explorador en su dispositivo físico en lugar de usar el explorador en sus VM de laboratorio.  Esto le ayudará a reducir los costos usando solo las VM de laboratorio para las aplicaciones que requieren una CPU o una GPU rápida.

## <a name="next-steps"></a>Pasos siguientes
Los pasos siguientes son comunes a la configuración de cualquier laboratorio:

- [Incorporación de usuarios](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Establecimiento de la cuota](how-to-configure-student-usage.md#set-quotas-for-users)
- [Establecimiento de la programación](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Vínculos de registro por correo electrónico para los alumnos](how-to-configure-student-usage.md#send-invitations-to-users)