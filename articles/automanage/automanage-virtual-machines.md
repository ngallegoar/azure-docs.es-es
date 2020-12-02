---
title: Azure Automanage para máquinas virtuales
description: Conozca Azure Automanage para máquinas virtuales.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: deanwe
ms.custom: references_regions
ms.openlocfilehash: ffbf3fa3e2d404b7bc5888ef01707cbd816600b3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96182859"
---
# <a name="azure-automanage-for-virtual-machines"></a>Azure Automanage para máquinas virtuales

En este artículo se incluye información sobre Azure Automanage para máquinas virtuales. Este servicio presenta las siguientes ventajas:

- Incorpora máquinas virtuales de forma inteligente a servicios de Azure de procedimientos recomendados.
- Configura automáticamente cada servicio según los procedimientos recomendados de Azure.
- Supervisa las desviaciones y las corrige cuando se detectan.
- Proporciona una experiencia sencilla (punto, clic, establecer, olvidar)


## <a name="overview"></a>Información general

Azure Automanage para máquinas virtuales es un servicio que elimina la necesidad de detectar, saber incorporar y configurar determinados servicios en Azure que serían beneficiosos para una máquina virtual. Estos servicios ayudan a mejorar la confiabilidad, la seguridad y la administración de las máquinas virtuales y se consideran servicios de procedimientos recomendados de Azure. Algunos de ellos son [Azure Update Management](../automation/update-management/overview.md) y [Azure Backup](../backup/backup-overview.md), por nombrar unos pocos.

Después de incorporar las máquinas virtuales a Azure Automanage, se configura automáticamente cada servicio de procedimientos recomendados en su configuración recomendada. Los procedimientos recomendados son diferentes para cada uno de los servicios. Por ejemplo, en Azure Backup el procedimiento recomendado es realizar una copia de seguridad de la máquina virtual una vez al día y tener un período de retención de seis meses.

Azure Automanage también supervisa automáticamente las desviaciones y las corrige cuando se detectan. Esto significa que, si la máquina virtual se incorpora a Azure Automanage, no solo se configurará según los procedimientos recomendados de Azure, sino que se supervisará para garantizar que sigue cumpliendo esos procedimientos recomendados durante todo su ciclo de vida. Si la máquina virtual se desvía de esos procedimientos, se corregirá y se la devolverá al estado deseado.

Por último, la experiencia es increíblemente sencilla.


## <a name="prerequisites"></a>Requisitos previos

Antes de intentar habilitar Azure Automanage en las máquinas virtuales, debe tener en cuenta varios requisitos previos.

- Máquinas virtuales solo de Windows Server
- Las máquinas virtuales deben estar en ejecución
- Las máquinas virtuales deben estar en una región admitida.
- El usuario debe tener los permisos correctos (consulte el párrafo siguiente).
- Automanage no admite suscripciones de espacio aislado en este momento

Debe tener el rol **Colaborador** en el grupo de recursos que contiene las máquinas virtuales para habilitar Automanage en las máquinas virtuales con una cuenta existente de Automanage. Si va a habilitar Automanage con una cuenta nueva de Automanage, necesita los siguientes permisos en la suscripción: el rol **Propietario** o **Colaborador** junto con el rol **Administrador de acceso de usuario**. 

> [!NOTE]
> Si quiere usar Automanage en una máquina virtual que está conectada a un área de trabajo en una suscripción diferente, debe tener los permisos descritos anteriormente en cada suscripción.

También es importante advertir que Automanage solo es compatible con máquinas virtuales Windows ubicadas en las siguientes regiones: Europa Occidental, Este de EE. UU., Oeste de EE. UU. 2, Centro de Canadá, Centro-oeste de EE. UU.

## <a name="participating-services"></a>Servicios participantes

:::image type="content" source="media\automanage-virtual-machines\intelligently-onboard-services.png" alt-text="Incorporación inteligente de servicios.":::

Consulte [Procedimientos recomendados de Azure Automanage para máquinas virtuales](virtual-machines-best-practices.md) para obtener la lista completa de los servicios de Azure participantes, así como sus perfiles de configuración admitidos.

 Le incorporaremos automáticamente a estos servicios participantes. Estos servicios son fundamentales para nuestra nota del producto de procedimientos recomendados, que puede encontrar en nuestro marco [Cloud Adoption Framework](/azure/cloud-adoption-framework/manage/azure-server-management).

Todos estos servicios se incorporarán y configurarán automáticamente, se supervisarán en busca de desviaciones y, en caso de detectarse estas, se corregirán.


## <a name="enabling-automanage-for-vms-in-azure-portal"></a>Habilitación de Automanage para máquinas virtuales en Azure Portal

En Azure Portal, puede habilitar Automanage en máquinas virtuales nuevas o existentes. Para conocer los pasos resumidos de este proceso, consulte el [inicio rápido de Automanage para máquinas virtuales](quick-create-virtual-machines-portal.md).

Si es la primera vez que habilita Automanage en la máquina virtual, puede buscar **Automanage: procedimientos recomendados de máquinas virtuales de Azure** en Azure Portal. Haga clic en **Enable on existing VM** (Habilitar en máquina virtual existente), seleccione las máquinas virtuales que quiere incorporar, haga clic en **Seleccionar** y en **Habilitar**, y ya está.

El único caso en que podría tener que interactuar con esta máquina virtual para administrar estos servicios es si intentamos corregir la máquina virtual, pero no lo conseguimos. Si hemos corregido la máquina virtual correctamente, la devolveremos a su estado de conformidad sin avisarle.


## <a name="configuration-profiles"></a>Perfiles de configuración

Para habilitar Automanage en una máquina virtual, se requiere un perfil de configuración. Los perfiles de configuración son la base de este servicio. Definen exactamente qué servicios se incorporan a las máquinas y, hasta cierto punto, cuál será la configuración de esos servicios.

### <a name="default-configuration-profiles"></a>Perfiles de configuración predeterminados

Actualmente, hay dos perfiles de configuración disponibles.

- El perfil de configuración **Azure virtual machine best practices - Dev/Test** (Procedimientos recomendados de máquinas virtuales: desarrollo/pruebas) está diseñado para máquinas de desarrollo y pruebas.
- El perfil de configuración **Azure virtual machine best practices - Production** (Procedimientos recomendados de máquinas virtuales: producción) es para producción.

La razón de esta diferencia es que algunos servicios se recomiendan en función de la carga de trabajo en ejecución. Por ejemplo, en una máquina de producción le incorporaremos automáticamente a Azure Backup. Sin embargo, en el caso de una máquina de desarrollo/pruebas, un servicio de copia de seguridad sería un costo innecesario, ya que las máquinas de desarrollo/pruebas suelen tener un menor efecto para la empresa.

### <a name="customizing-a-configuration-profile-using-preferences"></a>Personalización de un perfil de configuración mediante preferencias

Además de los servicios estándar a los que le incorporamos, le permitimos configurar un determinado subconjunto de preferencias. Estas preferencias se permiten dentro de una variedad de opciones de configuración que no incumplen nuestros procedimientos recomendados. Por ejemplo, en el caso de Azure Backup, le permitiremos definir la frecuencia de la copia de seguridad y el día de la semana en el que se produce. Sin embargo, *no* le permitimos desactivar Azure Backup completamente.

> [!NOTE]
> En el perfil de configuración de desarrollo/pruebas, no se realizará una copia de seguridad de la máquina virtual.

Se puede ajustar la configuración de un perfil de configuración predeterminado mediante las preferencias. Aprenda a crear una preferencia [aquí](virtual-machines-custom-preferences.md).

> [!NOTE]
> Mientras Automanage está habilitado, no se puede cambiar el perfil de configuración de la máquina virtual. Tendrá que deshabilitar Automanage en esa máquina virtual y, luego, habilitarlo de nuevo con el perfil y las preferencias de configuración deseados.


## <a name="automanage-account"></a>Cuenta de Automanage

La cuenta de Automanage es el contexto de seguridad o la identidad bajo los que tienen lugar las operaciones automatizadas. Por lo general, no es necesario seleccionar la opción de cuenta de Automanage, pero si hubiera un escenario de delegación en el que quisiera dividir la administración automatizada (quizás entre dos administradores del sistema), esta opción le permite definir una identidad de Azure para cada uno de esos administradores.

En la experiencia de Azure Portal, al habilitar Automanage en las máquinas virtuales, hay una lista desplegable de opciones avanzadas en la hoja **Enable Azure VM best practice** (Habilitar procedimiento recomendado de máquina virtual de Azure) que le permite asignar o crear manualmente la cuenta de Automanage.

> [!NOTE]
> Debe tener el rol **Colaborador** en el grupo de recursos que contiene las máquinas virtuales para habilitar Automanage en las máquinas virtuales con una cuenta existente de Automanage. Si va a habilitar Automanage con una cuenta nueva de Automanage, necesita los siguientes permisos en la suscripción: el rol **Propietario** o **Colaborador** junto con el rol **Administrador de acceso de usuario**.


## <a name="status-of-vms"></a>Estado de las máquinas virtuales

En Azure Portal, vaya a la página **Automanage – Azure virtual machine best practices** (Automanage: procedimientos recomendados de máquina virtual de Azure), que muestra todas las máquinas virtuales administradas automáticamente. Aquí verá el estado general de cada máquina virtual.

:::image type="content" source="media\automanage-virtual-machines\configured-status.png" alt-text="Lista de máquinas virtuales configuradas.":::

Para cada máquina virtual de la lista, se muestran los detalles siguientes: nombre, perfil de configuración, preferencia de configuración, estado, cuenta, suscripción y grupo de recursos.

La columna **Status** (Estado) puede mostrar los siguientes estados:
- *In-progress* (En curso). la máquina virtual se acaba de habilitar y se está configurando.
- *Configured* (Configurada): la máquina virtual está configurada y no se detecta ninguna desviación.
- *Failed*(Error): la máquina virtual presenta desviaciones y no somos capaces de corregirlas.

Si en **Status** (Estado), aparece *Failed* (Error), puede solucionar el problema de la implementación mediante el grupo de recursos en el que se encuentra la máquina virtual. Vaya a **Resource groups** (Grupos de recursos), seleccione el grupo de recursos, haga clic en **Deployments** (Implementaciones) y vea ahí el estado *Failed* (Error) con los detalles del error.


## <a name="disabling-automanage-for-vms"></a>Deshabilitación de Automanage para máquinas virtuales

Puede decidir un día deshabilitar Automanage en determinadas máquinas virtuales. Por ejemplo, la máquina está ejecutando una carga de trabajo segura muy confidencial y debe bloquearla incluso por encima de lo que Azure haría de forma natural, por lo que debe configurar la máquina fuera de los procedimientos recomendados de Azure.

Para ello, en Azure Portal, vaya a la página **Automanage – Azure virtual machine best practices** (Automanage: procedimientos recomendados de máquina virtual de Azure), que muestra todas las máquinas virtuales administradas automáticamente. Active la casilla situada junto a la máquina virtual en la que quiere deshabilitar Automanage y, luego, haga clic en el botón **Disable automanagment** (Deshabilitar la administración automática).

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Deshabilitación de Automanage en una máquina virtual.":::

Lea detenidamente los mensajes del elemento emergente resultante antes de aceptar **Disable** (Deshabilitar).

> [!NOTE]
> Al deshabilitar la administración en una VM, se produce el comportamiento siguiente:
>
> - La configuración de la VM y los servicios que se incorporan no cambian.
> - Los cargos en los que incurran esos servicios se seguirán facturando y se continuarán generando.
> - Cualquier comportamiento de Automanage se detiene inmediatamente.


En primer lugar, no se desactivará la máquina virtual de ninguno de los servicios que hayamos incorporado y configurado. Los cargos que generen esos servicios se seguirán facturando. Si es necesario, debe desactivarlos. Cualquier acción de Automanage se detendrá inmediatamente. Por ejemplo, ya no se supervisarán las desviaciones de la máquina virtual.


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido que Automanage para máquinas virtuales proporciona un medio para eliminar la necesidad de conocer, incorporar y configurar servicios de Azure de procedimientos recomendados. Además, si una máquina que se ha incorporado a Automanage para máquinas virtuales se desvía de los perfiles de configuración establecidos, la devolveremos automáticamente a su estado de conformidad.

Intente habilitar Automanage para máquinas virtuales en Azure Portal.

> [!div class="nextstepaction"]
> [Habilitación de Automanage para máquinas virtuales en Azure Portal](quick-create-virtual-machines-portal.md)