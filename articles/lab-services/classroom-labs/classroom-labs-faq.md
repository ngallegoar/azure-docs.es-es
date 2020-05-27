---
title: 'Laboratorios de clase en Azure Lab Services: preguntas frecuentes| Microsoft Docs'
description: En este artículo se ofrecen respuestas a las preguntas más frecuentes sobre laboratorios de clase en Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2020
ms.author: spelluru
ms.openlocfilehash: 11cb0d995537b8979c3727bf508045c6e31fc208
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592617"
---
# <a name="classroom-labs-in-azure-lab-services--frequently-asked-questions-faq"></a>Laboratorios de clase en Azure Lab Services: preguntas frecuentes
Obtenga respuestas a algunas de las preguntas más habituales sobre los laboratorios de clase en Azure Lab Services. 

## <a name="quotas"></a>Cuotas

### <a name="is-the-quota-per-user-or-per-week-or-per-entire-duration-of-the-lab"></a>¿La cuota es por usuario, por semana o por toda la duración del laboratorio? 
La cuota que establezca para un laboratorio es para cada alumno por toda la duración del laboratorio. Además, la [hora de ejecución programada de las máquinas virtuales](how-to-create-schedules.md) no cuenta en la cuota asignada a un usuario. La cuota es el tiempo fuera del horario programado que un alumno pasa en las máquinas virtuales.  Para más información sobre las cuotas, vea [Establecimiento de cuotas para los usuarios](how-to-configure-student-usage.md#set-quotas-for-users).

### <a name="if-educator-turns-on-a-student-vm-does-that-affect-the-student-quota"></a>¿Si un formador activa la VM de un alumno, afecta a la cuota del alumno? 
No. No la afecta. Cuando el formador activa la VM del alumno, no afecta a la cuota asignada al alumno. 

## <a name="schedules"></a>Programaciones

### <a name="do-all-vms-in-the-lab-start-automatically-when-a-schedule-is-set"></a>¿Se inician automáticamente todas las máquinas virtuales del laboratorio cuando se establece una programación? 
No. No todas las máquinas virtuales. Solo las máquinas virtuales que se asignan a los usuarios según una programación. Las máquinas virtuales que no están asignadas a un usuario no se inician automáticamente. Es por diseño. 

## <a name="lab-accounts"></a>Cuentas de laboratorio

### <a name="why-am-i-not-able-to-create-a-lab-because-of-unavailability-of-the-address-range"></a>¿Por qué no puedo crear un laboratorio debido a la falta de disponibilidad del intervalo de direcciones? 
Los laboratorios de clase pueden crear máquinas virtuales de laboratorio dentro de un intervalo de direcciones IP que se especifique al crear la cuenta de laboratorio en Azure Portal. Cuando se proporciona un intervalo de direcciones, cada laboratorio que se crea después de haber asignado 512 direcciones IP para las máquinas virtuales del laboratorio. El intervalo de direcciones de la cuenta de laboratorio debe ser lo suficientemente grande como para dar cabida a todos los laboratorios que se van a crear en la cuenta de laboratorio. 

Por ejemplo, si tiene un bloque de /19 - 10.0.0.0/19, este intervalo de direcciones abarca 8192 direcciones IP y 16 laboratorios (8192/512 = 16 laboratorios). En este caso, la creación del laboratorio 17 genera un error.

### <a name="what-port-ranges-should-i-open-on-my-organizations-firewall-setting-to-connect-to-lab-virtual-machines-via-rdpssh"></a>¿Qué intervalos de puertos debo abrir en la configuración del firewall de mi organización para conectarme a las máquinas virtuales de laboratorio a través de RDP/SSH?

Los puertos son: 49152–65535. Los laboratorios de clase se encuentran detrás de un equilibrador de carga. Cada laboratorio tiene una sola dirección IP pública y cada máquina virtual del laboratorio tiene un puerto único. 

También puede ver la dirección IP privada de cada máquina virtual en la pestaña **Grupo de máquinas virtuales** de la página principal del laboratorio en Azure Portal. Si vuelve a publicar un laboratorio, la dirección IP pública del laboratorio no cambiará, pero la dirección IP privada y el número de puerto de cada máquina virtual del laboratorio pueden cambiar. Puede obtener más información en el artículo: [Configuración del firewall para Azure Lab Services](how-to-configure-firewall-settings.md).

### <a name="what-public-ip-address-range-should-i-open-on-my-organizations-firewall-settings-to-connect-to-lab-virtual-machines-via-rdpssh"></a>¿Qué intervalos de direcciones IP públicas debo abrir en la configuración del firewall de mi organización para conectarme a las máquinas virtuales de laboratorio a través de RDP/SSH?
Consulte [Intervalos IP de Azure y etiquetas de servicio: nube pública](https://www.microsoft.com/download/details.aspx?id=56519), que proporciona el intervalo de direcciones IP públicas para centros de datos en Azure. Puede abrir las direcciones IP de las regiones en las que se encuentran las cuentas de laboratorio.

## <a name="virtual-machine-images"></a>Imágenes de máquinas virtuales

### <a name="as-a-lab-creator-why-cant-i-enable-additional-image-options-in-the-virtual-machine-images-dropdown-when-creating-a-new-lab"></a>Como creador de laboratorios, ¿por qué no se pueden habilitar opciones de imagen adicionales en la lista desplegable de imágenes de máquina virtual al crear un nuevo laboratorio?

Cuando un administrador le agrega como creador de laboratorios a una cuenta de laboratorio, se le conceden los permisos para crear laboratorios. Sin embargo, no tiene permisos para editar ninguna configuración dentro de la cuenta de laboratorio, incluyendo la lista de imágenes de máquina virtual habilitadas. Para habilitar imágenes adicionales, póngase en contacto con el administrador de la cuenta de laboratorio para que lo haga, o pida al administrador que le agregue con el rol Colaborador a la cuenta de laboratorio. El rol Colaborador le proporcionará los permisos para editar la lista de imágenes de máquina virtual en la cuenta de laboratorio.

### <a name="can-i-attach-additional-disks-to-a-virtual-machine"></a>¿Puedo conectar discos adicionales a una máquina virtual?
No. No es posible conectar discos adicionales a una VM en un laboratorio de clase. 

## <a name="users"></a>Usuarios

### <a name="how-many-users-can-be-in-a-classroom-lab"></a>¿Cuántos usuarios pueden estar en un laboratorio educativo?
Puede agregar hasta 400 usuarios a un laboratorio educativo. 

## <a name="blog-post"></a>Entrada de blog
Suscríbase al [blog de Azure Lab Services](https://aka.ms/azlabs-blog).

## <a name="update-notifications"></a>Notificaciones de actualización
Suscríbase a las [actualizaciones de Lab Services](https://azure.microsoft.com/updates/?product=lab-services) para mantenerse informado sobre las nuevas características de Lab Services.

## <a name="general"></a>General
### <a name="what-if-my-question-isnt-answered-here"></a>Mi pregunta no está respondida aquí. ¿Qué debo hacer?
Si su pregunta no aparece aquí, háganoslo saber para que podamos ayudarlo a encontrar una respuesta.

- Publique una pregunta al final de este documento. 
- Para llegar a un público más amplio, publique una pregunta en [Azure Lab Services: foro de Tech Community](https://techcommunity.microsoft.com/t5/azure-lab-services/bd-p/AzureLabServices). 
- Para solicitudes de características, envíe sus solicitudes e ideas al sitio [User Voice de Azure Lab Services](https://feedback.azure.com/forums/320373-lab-services?category_id=352774).

