---
title: Guía de configuración acelerada de un laboratorio para Azure Lab Services
description: Esta guía ayuda a los creadores de laboratorios a configurar rápidamente una cuenta de laboratorio para usarla en su centro educativo.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: f7423a76fd3ceb238c8c5c1a4ea794ff83b28b4a
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491671"
---
# <a name="lab-setup-guide"></a>Guía de instalación de un laboratorio

El proceso de publicación de un laboratorio para los alumnos puede tardar varias horas.  La cantidad de tiempo depende del número de máquinas virtuales (VM) que se crearán en el laboratorio. Debe reservar al menos un día para configurar un laboratorio a fin de asegurarse de que funciona correctamente y de dejar tiempo suficiente para publicar las VM de los alumnos.

## <a name="understand-the-lab-requirements-of-your-class"></a>Descripción de los requisitos del laboratorio de la clase

Antes de configurar un laboratorio nuevo, debe tener en cuenta las preguntas siguientes.

### <a name="what-software-requirements-does-the-class-have"></a>¿Qué requisitos de software tiene la clase?

En función de los objetivos de aprendizaje de la clase, debe decidir qué sistema operativo, aplicaciones y herramientas se deben instalar en las máquinas virtuales del laboratorio. Para configurar máquinas virtuales de laboratorio, tiene tres opciones:

- **Usar una imagen de Azure Marketplace**: Azure Marketplace proporciona cientos de imágenes que puede usar al crear un laboratorio. En algunas clases, es posible que una de estas imágenes ya contenga todo lo que se necesita para la clase.

- **Crear una nueva imagen personalizada**: puede crear su propia imagen personalizada a partir de una imagen de Azure Marketplace y personalizarla mediante la instalación de software adicional y la realización de cambios en la configuración.

- **Usar una imagen personalizada existente**: Puede reutilizar imágenes personalizadas existentes que haya creado anteriormente o creadas por otros administradores o profesores de la escuela. Para usar imágenes personalizadas, los administradores deben configurar una instancia de Shared Image Gallery.  Shared Image Gallery es un repositorio que se puede usar para guardar imágenes personalizadas.

> [!NOTE]
> Los administradores son los responsables de habilitar imágenes de Azure Marketplace e imágenes personalizadas para que pueda usarlas. Colabore con el departamento de TI para asegurarse de que las imágenes que necesita están habilitadas. Las imágenes personalizadas que cree se habilitan de forma automática para su uso en los laboratorios de su propiedad.

### <a name="what-hardware-requirements-does-the-class-have"></a>¿Qué requisitos de hardware tiene la clase?

Puede elegir entre distintos tamaños de proceso:

- Tamaños de virtualización anidados, para que pueda conceder a los alumnos acceso a una VM capaz de hospedar varias VM anidadas. Por ejemplo, puede usar este tamaño de proceso para las redes o las clases de piratería ética.

- Tamaños de GPU, de modo que los alumnos puedan usar tipos de aplicaciones de proceso intensivo. Por ejemplo, esta opción se suele utilizar con la inteligencia artificial y el aprendizaje automático.

Para obtener instrucciones sobre cómo seleccionar el tamaño de VM adecuado, lea los artículos siguientes:
- [Tamaño de máquina virtual](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing)
- [Traslado de un laboratorio físico a Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)

> [!NOTE]
> En función de la región del laboratorio, es posible que vea menos tamaños de proceso disponibles, ya que varían según la región. Por lo general, debe seleccionar el menor tamaño de proceso más cercano a sus necesidades. Con Azure Lab Services, puede configurar un laboratorio nuevo con otra capacidad de proceso más adelante si es necesario.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>¿De qué recursos externos de Azure o de red depende la clase?
Es posible que las VM de su laboratorio necesiten acceder a recursos externos, como una base de datos, un recurso compartido de archivos o un servidor de licencias.  Para permitir que las VM de su laboratorio usen recursos externos, coordine con los administradores de TI.

> [!NOTE]
> Debe considerar si puede reducir las dependencias del laboratorio de recursos externos y proporcionar el recurso directamente en la máquina virtual. Por ejemplo, para eliminar la necesidad de leer datos de una base de datos externa, puede instalarla directamente en la máquina virtual.  

### <a name="how-will-costs-be-controlled"></a>¿Cómo se controlarán los costos?
En Lab Services se usa un modelo de precios de pago por uso, lo que significa que solo se paga por el tiempo que se ejecute una máquina virtual de laboratorio. Para controlar los costos, tiene tres opciones que se suelen usar de forma conjunta:

- **Programación**: una programación permite controlar de forma automática cuándo se inician y se apagan las máquinas virtuales de los laboratorios.
- **Cuota**: la cuota controla el número de horas que los alumnos tendrán acceso a una máquina virtual fuera de las horas programadas.  Cuando un alumno usa su VM y se alcanza su cuota, la VM se apaga automáticamente.  El alumno no podrá reiniciar la máquina virtual a menos que se aumente la cuota.
- **Apagado automático**: cuando se habilita, la configuración de apagado automático hace que las VM de Windows se apaguen automáticamente después de que un alumno se haya desconectado de su sesión de Protocolo de escritorio remoto (RDP). Esta configuración está deshabilitada de forma predeterminada.

Lea los siguientes artículos para obtener más información:
- [cálculo de los costos](https://docs.microsoft.com/azure/lab-services/cost-management-guide#estimate-the-lab-costs)
- [Administrar costos](https://docs.microsoft.com/azure/lab-services/cost-management-guide#manage-costs)

### <a name="how-will-students-save-their-work"></a>¿Cómo guardarán el trabajo los alumnos?
A los alumnos se les asigna su propia máquina virtual mientras dure el laboratorio. Pueden optar por:

- Guardarlo directamente en la máquina virtual.
- Guardarlo en una ubicación externa, como OneDrive o GitHub.

Es posible configurar OneDrive automáticamente para los alumnos en sus máquinas virtuales de laboratorio.

> [!NOTE]
> Para asegurarse de que los alumnos tienen acceso continuado al trabajo que han guardado fuera del laboratorio, después de que finalice la clase, se recomienda que lo guarden en un repositorio externo.

### <a name="how-will-students-connect-to-their-vm"></a>¿Cómo se conectarán los alumnos a sus máquinas virtuales?
En el caso de RDP para máquinas virtuales Windows, se recomienda que los alumnos usen el [cliente de Escritorio remoto de Microsoft](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients). El cliente de escritorio es compatible con Mac, Chromebooks y Windows.

En el caso de las máquinas virtuales Linux, los alumnos pueden usar SSH o RDP. Para que los alumnos se conecten mediante RDP, debe instalar y configurar los paquetes de GUI y RDP necesarios.

### <a name="will-students-also-be-using-microsoft-teams"></a>¿Los alumnos también usarán Microsoft Teams?
Azure Lab Services se integra con Microsoft Teams para que los profesores puedan crear y administrar sus laboratorios en Teams.  Del mismo modo, los alumnos pueden acceder al laboratorio en Teams.

Para obtener más información, consulte el artículo siguiente:
- [Azure Lab Services en Microsoft Teams](https://docs.microsoft.com/azure/lab-services/lab-services-within-teams-overview)

## <a name="set-up-your-lab"></a>Configuración del laboratorio

Una vez que comprenda los requisitos del laboratorio de la clase, está listo para configurarlo. Siga los vínculos de esta sección para ver cómo configurar el laboratorio.  Tenga en cuenta que se proporcionan pasos diferentes en función de si usa los laboratorios en Teams.

1. **Cree un laboratorio.** Consulte los tutoriales sobre la creación de un laboratorio:
    - [Creación de un laboratorio educativo](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab) para obtener instrucciones.
    - [Creación de un laboratorio desde Teams](https://docs.microsoft.com/azure/lab-services/how-to-get-started-create-lab-within-teams)

    > [!NOTE]
    > Si en la clase se necesita virtualización anidada, consulte los pasos en [Habilitación de la virtualización anidada](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm).

1. **Personalice imágenes y publique máquinas virtuales de laboratorio.** Conéctese a una máquina virtual especial llamada máquina virtual de plantilla. Consulte los pasos en las siguientes guías:
    - [Creación y administración de una máquina virtual de plantilla](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Uso de una galería de imágenes compartidas](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Si usa Windows, también debe consultar las instrucciones [Preparación de una máquina virtual de plantilla con Windows](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template). Estas instrucciones incluyen pasos para configurar OneDrive y Office para que los usen los alumnos.

1. **Administre la capacidad y el grupo de máquinas virtuales.** Puede escalar o reducir verticalmente la capacidad de la máquina virtual según sea necesario para la clase. Tenga en cuenta que el aumento de la capacidad de la VM puede tardar varias horas debido a la configuración de nuevas VM. Consulte los pasos descritos en estos artículos:
    - [Configuración y administración de un grupo de VM](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords)
    - [Administración de un grupo de máquinas virtuales en Lab Services desde Teams](https://docs.microsoft.com/azure/lab-services/how-to-manage-vm-pool-within-teams)

1. **Agregue y administre los usuarios del laboratorio.** Para agregar usuarios al laboratorio, consulte los pasos de los tutoriales siguientes:
   - [Incorporación de usuarios al laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Envío de invitaciones a los usuarios](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)
   - [Administración de las listas de usuarios de Lab Services desde Teams](https://docs.microsoft.com/azure/lab-services/how-to-manage-user-lists-within-teams)

    Para más información sobre los tipos de cuentas que los alumnos pueden usar, consulte [Cuentas de alumnos](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts).
  
1. **Establezca controles de costos.** Para controlar los costos del laboratorio, establezca programaciones, cuotas y apagado automático. Vea los siguientes tutoriales:

   - [Establecimiento de la programación](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)

        > [!NOTE]
        > En función del tipo de sistema operativo instalado, una máquina virtual puede tardar varios minutos en iniciarse. Para asegurarse de que una máquina virtual de laboratorio está lista para usarse durante las horas programadas, se recomienda iniciarla 30 minutos antes.

   - [Establecimiento de cuotas para usuarios](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users) y [establecimiento de una cuota adicional para un usuario concreto](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-additional-quotas-for-specific-users)
  
   - [Habilitación del apagado automático al desconectar](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

        > [!NOTE]
        > Las programaciones y las cuotas no se aplican a la máquina virtual de plantilla, pero sí se aplica la configuración de apagado automático. 
        > 
        > Al crear un laboratorio, se crea la máquina virtual de plantilla, pero no se inicia. Puede iniciarla, conectarse a ella e instalar cualquier software necesario para el laboratorio y, a continuación, publicarla. Al publicar la máquina virtual de plantilla, se apagará automáticamente si el usuario no lo ha hecho. 
        > 
        > Las máquinas virtuales de plantilla incurren en un **costo** al ejecutarse, por lo que debe asegurarse de que la máquina virtual de la plantilla se cierre cuando no sea necesario que se esté ejecutando.

    - [Creación y administración de programaciones de Azure Lab Services en Teams](https://docs.microsoft.com/azure/lab-services/how-to-create-schedules-within-teams) 

1. **Use el panel.** Para obtener instrucciones, consulte [Uso del panel del laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard).

    > [!NOTE]
    > Los costos estimados que se muestran en el panel son el costo máximo que puede esperar del uso del laboratorio por parte de los alumnos. Por ejemplo, *no* se le cobrará por las horas de cuota que los alumnos no usen. Los costes estimados *no* reflejan ningún cargo derivado de usar la máquina virtual de plantilla, la galería de imágenes compartidas o cuando el creador de laboratorio inicia un equipo de usuario.

## <a name="next-steps"></a>Pasos siguientes

- [Seguimiento del uso de un laboratorio educativo](tutorial-track-usage.md)
  
- [Acceso a un laboratorio educativo](tutorial-connect-virtual-machine-classroom-lab.md)
