---
title: Introducción a Azure Lab Services
description: En este artículo se describe cómo empezar a usar Azure Lab Services.
ms.topic: article
ms.date: 10/02/2020
ms.openlocfilehash: 33e052931b0c3bd1bb1434b7eeefeed7a2a7ceab
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93380098"
---
# <a name="get-started-with-lab-services"></a>Introducción a Lab Services 

Como alumno, puede usar Azure Lab Services para acceder al software estándar del sector necesario para los programas de estudio que se realizan en las máquinas virtuales (VM). 

Los profesores han de saber cómo enseñar a los alumnos a utilizar Azure Lab Services mediante sus indicaciones en el hardware enviado a cada alumno de forma individual.

En este artículo se proporciona información al personal docente sobre cómo pueden acceder a Azure Lab Services, administrarlo y enseñar a los alumnos a usarlo.

## <a name="overview"></a>Información general

¿Qué es una máquina virtual y cómo funciona?

Una máquina virtual (VM) es un entorno virtual que actúa como equipo virtual. Las máquinas virtuales tienen su propio procesador, memoria y almacenamiento. Las máquinas virtuales proporcionan un sustituto para una máquina real y pueden conceder a los usuarios acceso a sistemas operativos y software sin necesidad de tenerlos en sus propios dispositivos. Azure Lab Services ofrece una herramienta para que los alumnos puedan acceder a las máquinas virtuales y navegar por ellas, y para que el personal administre sus laboratorios de equipos virtuales. 

Para más información, consulte [Creación y administración de laboratorios educativos](how-to-manage-classroom-labs.md).

## <a name="lab-dashboards"></a>Paneles del laboratorio

Los paneles de los laboratorios educativos de Azure Lab Services proporcionan una instantánea de los diferentes aspectos de un laboratorio determinado, como la información de la máquina virtual, el número de máquinas virtuales asignadas y sin asignar, el número de usuarios registrados y sin registrar, e información sobre las programaciones de laboratorio. 

> [!NOTE]
> Aunque la mayoría de los aspectos administrativos del panel y del [sitio web de Azure Lab Services](https://labs.azure.com/) estarán visibles para los profesores, los permisos específicos de su rol pueden afectar a su capacidad para modificar algunos de sus criterios. Si encuentra algún problema con su configuración de laboratorio en particular, póngase en contacto con el administrador de CTE.

:::image type="content" source="./media/use-dashboard/dashboard.png" alt-text="El portal de Azure Lab Services":::

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com/) e inicie sesión.
1. Seleccione el laboratorio.
1. Verá un **panel** en el lado izquierdo de la ventana. Haga clic en **Panel**; se mostrarán en él varios iconos.
1. Debajo del icono **Costos y facturación**, también hay iconos para Plantillas, Grupos de máquinas virtuales, Usuarios y Programaciones, que le permiten modificar aspectos y ver más detalles sobre el laboratorio educativo.

    1. Plantilla: describe la fecha en que se creó la plantilla y la última vez que se publicó. 
    1. Grupo de máquinas virtuales: número de máquinas virtuales asignadas y sin asignar.
    1. Usuarios: número de usuarios registrados y usuarios que se han agregado al laboratorio, pero no se han registrado.
    1. Programaciones: muestra los próximos eventos programados del laboratorio y un vínculo para ver más eventos.

Para más información, consulte el artículo [Uso del panel](use-dashboard.md).

## <a name="quota-hours"></a>Horas de cuota

Los alumnos pueden acceder a sus máquinas virtuales en cualquier momento durante la hora de clase programada sin que ello afecte a sus horas de cuota. Las horas de cuota se establecen para todo el semestre y determinan el número de horas que un alumno puede usar su máquina virtual fuera de la hora de clase programada regularmente.

8 horas por semana, se restablecen el domingo, no son acumulativas.

Para más información, consulte [Establecimiento de la cuota](how-to-configure-student-usage.md#set-quotas-for-users).

### <a name="automatic-shut-down"></a>Apagado automático

Para ayudar a reducir los costos y ahorrar horas de cuota a los alumnos, se habilitarán los apagados automáticos en nuestros laboratorios. Los apagados automáticos desactivarán las máquinas virtuales después de un período de inactividad (sin entradas del mouse o del teclado). Funcionan en dos fases. en primer lugar, se desconecta a un alumno de la máquina virtual después de un período de inactividad. En este momento, la máquina virtual sigue **en ejecución** y los alumnos pueden conectarse. Después de otro período de inactividad una vez desconectado, la máquina virtual se apaga sola.

Los apagados automáticos son una importante herramienta de ahorro de costos, pero presentan un inconveniente para los alumnos en lo que respecta a guardar su trabajo y representar archivos de proyecto de gran tamaño si con frecuencia se desconecta a los alumnos o las máquinas virtuales se apagan demasiado rápido. If you students are frequently being disconnected or VMs are turning off too quickly. Si tiene este tipo de problema, póngase en contacto con el administrador de CTE. 

Para más información, consulte [Configuración del apagado automático de las máquinas virtuales de una cuenta de laboratorio](how-to-configure-lab-accounts.md).

## <a name="managing-virtual-machines"></a>Administración de máquinas virtuales

La administración del laboratorio permite a los profesores controlar aspectos como la capacidad del laboratorio (el número de máquinas virtuales disponibles para los alumnos) e iniciar, detener o restablecer manualmente las máquinas virtuales. Los profesores también pueden conectarse a las máquinas virtuales para experimentar la interfaz del alumno, acceder a archivos y solucionar problemas relacionados con el software o la propia máquina virtual.

Lo más importante que se debe recordar al administrar nuestras máquinas virtuales es que cada vez que una máquina está **en ejecución**, se generan costos, incluso si no hay nadie conectado a ella.

### <a name="manually-starting-vms"></a>Inicio manual de las máquinas virtuales

1. En la página **Grupo de máquinas virtuales**, puede iniciar todas las máquinas virtuales de un laboratorio; para ello, haga clic en el botón **Iniciar todo** de la parte superior de la página.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/start-all-vms-button.png" alt-text="Inicio de las máquinas virtuales":::
1. Para iniciar máquinas virtuales individuales, haga clic en el botón de alternancia de estado. 

    El botón de alternancia mostrará **Iniciando** cuando se inicie la máquina virtual y, luego, **En ejecución** una vez iniciada esta.
1. También puede seleccionar una serie de máquinas virtuales mediante los botones de opción situados a la izquierda de la columna **Nombre**. 

    Cuando haya seleccionado las máquinas virtuales que desee, haga clic en el botón **Iniciar** de la parte superior de la pantalla.
1. Después de iniciarlas, puede hacer clic en el botón **Detener todo** para detener todas las máquinas virtuales.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png" alt-text="Detención de las máquinas virtuales":::

### <a name="stopping-and-resetting-vms"></a>Detención y restablecimiento de las máquinas virtuales

* Puede detener máquinas virtuales individuales haciendo clic en el botón de alternancia de estado.
* También puede detener varias máquinas virtuales mediante los botones de opción y haciendo clic en el botón "Detener" de la parte superior de la pantalla.

Si un alumno experimenta dificultades para conectarse a la máquina virtual o, por cualquier motivo, la máquina virtual debe restablecerse, puede usar la función de restablecimiento.
1. Para restablecer una o varias máquinas virtuales, selecciónelas con los botones de opción y haga clic en el botón **Restablecer** en la parte superior de la página.
1. En la ventana emergente, haga clic en **Restablecer**.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png" alt-text="Restablecimiento de la máquina virtual":::

    > [!NOTE]
    > El encendido de la máquina virtual de un alumno no afecta a su cuota. La cuota de los usuarios especifica el número de horas de laboratorio disponibles para ellos fuera del tiempo de clase programado.

### <a name="connect-to-virtual-machines"></a>Conexión a las máquinas virtuales

Los profesores pueden conectarse a la máquina virtual de un alumno siempre que esté encendida y el alumno no esté conectado a ella. Al conectarse a la máquina virtual, podrá acceder a los archivos locales de esta y ayudar a los alumnos a solucionar los problemas.

1. Para conectarse a la máquina virtual del alumno, mantenga el puntero sobre la máquina virtual en la lista y haga clic en el botón **Conectar**. 
1. A continuación, siga la guía de introducción para alumnos de Chromebooks, Mac o PC.

:::image type="content" source="./media/how-to-set-virtual-machine-passwords/connect-student-vm.png" alt-text="Botón de conexión a la máquina virtual de alumno":::

## <a name="add-and-manage-lab-users"></a>Incorporación y administración de usuarios de laboratorio

Los profesores pueden agregar alumnos a un laboratorio y supervisar sus cuotas de hora. 

### <a name="add-users-by-email-address"></a>Adición de usuarios por dirección de correo electrónico

1. En el [sitio web de Azure Lab Services](https://labs.azure.com/), haga clic en **Usuarios** en la parte izquierda de la ventana.
1. En la parte superior de la ventana, haga clic en **Agregar usuarios** y seleccione **Agregar por dirección de correo electrónico**. 

    :::image type="content" source="./media/how-to-configure-student-usage/add-users-button.png" alt-text="El botón &quot;Agregar usuarios&quot;":::
1. En el panel **Agregar usuarios** que aparece a la derecha, escriba las direcciones de correo electrónico de los alumnos en varias líneas o en una sola, separadas por punto y coma.
1. Haga clic en **Save**(Guardar).

    :::image type="content" source="./media/get-started-manage-labs/add-students.png" alt-text="Adición de alumnos al laboratorio":::
1. La lista de usuarios ahora se actualizará con los correos electrónicos, el estado, la invitación y las horas de cuota.

    Una vez que los alumnos están registrados en un laboratorio, sus nombres se actualizarán con el nombre y los apellidos del directorio de MPS.

    > [!NOTE]
    > Mantenga activado el botón de conmutación de la opción Restringir acceso para los usuarios. Esto significa que solo los usuarios de la lista pueden registrarse en el laboratorio mediante el vínculo de registro enviado.

### <a name="add-users-using-a-spreadsheet"></a>Adición de usuarios mediante una hoja de cálculo 

También puede agregar usuarios cargando un archivo CSV que contenga sus direcciones de correo electrónico.

1. En Microsoft Excel, cree un archivo CSV e incluya en él las direcciones de correo electrónico de los estudiantes en una columna.
1. En el [sitio web de Azure Lab Services](https://labs.azure.com/), en la parte superior de la página **Usuarios**, haga clic en el botón **Agregar usuarios**.
1. Seleccione **Cargar CSV**.
1. Seleccione el archivo CSV que contiene las direcciones de correo electrónico de los alumnos y, después, haga clic en **Abrir**.

    :::image type="content" source="./media/get-started-manage-labs/add-users-spreadsheet.png" alt-text="Adición de usuarios mediante una hoja de cálculo":::
1. Los correos electrónicos aparecerán ahora en la ventana de la derecha. Haga clic en **Save**(Guardar).

    :::image type="content" source="./media/get-started-manage-labs/register-users.png" alt-text="Registro de usuarios":::

### <a name="register-users"></a>Registro de usuarios

Una vez que los usuarios se han agregado al laboratorio, tendrán que registrarse para acceder a las máquinas virtuales. Para ello, puede invitarlos desde el portal de servicios web de Azure, que enviará un correo electrónico con el vínculo de registro del laboratorio. Otra manera es copiar y pegar el vínculo de registro en un correo electrónico u otra forma de comunicación con los alumnos.

1. En la página **Usuarios**, seleccione uno o varios alumnos de la lista.

    En la fila del alumno que ha seleccionado, seleccione el icono de sobre en la lista o haga clic en **Invitar** en la parte superior de la pantalla.

    :::image type="content" source="./media/get-started-manage-labs/send-invitation.png" alt-text="Envío de una invitación":::
    
    En la ventana **Enviar invitación por correo electrónico**, escriba un mensaje opcional (por ejemplo, un nombre de usuario y una contraseña) para los alumnos y, luego, haga clic en **Enviar**. 
    
    :::image type="content" source="./media/get-started-manage-labs/send-invitation-mail.png" alt-text="Envío de una invitación por correo":::

    Otra opción es, desde la misma página **Usuarios**, hacer clic en el botón **Vínculo de registro** de la parte superior de la pantalla. 

    :::image type="content" source="./media/get-started-manage-labs/registration-link.png" alt-text="Vínculo de registro del usuario":::
    
    Copie el vínculo de registro del campo de texto y péguelo en el correo electrónico o en la herramienta de mensajería segura que prefiera.  
    
    :::image type="content" source="./media/get-started-manage-labs/user-registration.png" alt-text="Envío del registro del usuario":::

Después de haber invitado a los usuarios mediante Azure Portal o de haber compartido el vínculo, podrá supervisar qué usuarios se han registrado correctamente en la página **Usuarios** de la columna **Estado**. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando los recursos que ha creado en este inicio rápido, elimínelos.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de una cuenta de laboratorio](tutorial-setup-lab-account.md)