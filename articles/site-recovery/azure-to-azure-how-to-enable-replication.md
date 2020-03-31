---
title: Configuración de la replicación de VM de Azure en Azure Site Recovery
description: Aprenda a configurar la replicación en otra región para VM de Azure mediante Site Recovery.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/29/2018
ms.openlocfilehash: 1c6b7cfbf193f02598052b6922efec17fb16ec83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75973689"
---
# <a name="replicate-azure-vms-to-another-azure-region"></a>Replicación de máquinas virtuales de Azure en otra región de Azure


En este artículo se describe cómo habilitar la replicación de máquinas virtuales de Azure de una región de Azure a otra.

## <a name="before-you-start"></a>Antes de comenzar

En este artículo se da por supuesto que está preparado para la implementación de Site Recovery, como se describe en [Tutorial de recuperación ante desastres de Azure en Azure](azure-to-azure-tutorial-enable-replication.md).

Se deben cumplir los requisitos previos y debería haber creado un almacén de Recovery Services.


## <a name="enable-replication"></a>Habilitar replicación

Habilite la replicación. En este procedimiento se supone que la región principal de Azure es Asia Pacífico, y la secundaria es Sudeste de Asia.

1. En el almacén, haga clic en **+ Replicar**.
2. Especifique los siguientes campos:
   - **Origen**: el punto de origen de las máquinas virtuales, que en este caso es **Azure**.
   - **Ubicación de origen**: la región de Azure desde la que se quieren proteger las máquinas virtuales. En este ejemplo, la ubicación de origen es "Asia Oriental".
   - **Modelo de implementación**: el modelo de implementación de Azure de las máquinas de origen.
   - **Suscripción de origen**: suscripción a la que pertenecen las máquinas virtuales de origen. Puede tratarse de cualquier suscripción dentro del mismo inquilino de Azure Active Directory donde exista el almacén de Recovery Services.
   - **Grupo de recursos**: el grupo de recursos al que pertenecen las máquinas virtuales de origen. Todas las máquinas virtuales del grupo de recursos seleccionado se enumeran para su protección en el paso siguiente.

     ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. En **Máquinas virtuales > Seleccionar máquinas virtuales**, haga clic en cada máquina virtual que quiera replicar y selecciónela. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. A continuación, haga clic en **Aceptar**.
    ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. En **Configuración**, tiene la opción de configurar el sitio de destino:

   - **Ubicación de destino**: ubicación donde se replicarán los datos de la máquina virtual de origen. Según la ubicación de las máquinas seleccionadas, Site Recovery proporcionará la lista de regiones de destino adecuadas. Se recomienda mantener como la ubicación de destino la misma ubicación del almacén de Recovery Services.
   - **Suscripción de destino**: la suscripción de destino utilizada para la recuperación ante desastres. De forma predeterminada, la suscripción de destino será la misma que la suscripción de origen.
   - **Grupo de recursos de destino**: el grupo de recursos al que pertenecen todas las máquinas virtuales replicadas.
       - De forma predeterminada, Site Recovery crea un nuevo grupo de recursos en la región de destino con un sufijo "asr" en el nombre.
       - Si el grupo de recursos creado por Site Recovery ya existe, se vuelve a usar.
       - Puede personalizar la configuración del grupo de recursos.
       - La ubicación del grupo de recursos de destino puede ser cualquier región de Azure excepto la región en la que se hospedan las máquinas virtuales de origen.
   - **Red virtual de destino**: De forma predeterminada, Site Recovery crea una nueva red virtual en la región de destino con un sufijo "asr" en el nombre. Esta se asigna a la red de origen y se usa para todas las protecciones futuras. [más información](site-recovery-network-mapping-azure-to-azure.md) sobre la asignación de red.
   - **Cuentas de almacenamiento de destino (la máquina virtual de origen no usa discos administrados)** : de forma predeterminada, Site Recovery crea una cuenta de almacenamiento de destino mediante la imitación de la configuración de almacenamiento de la máquina virtual de origen. En caso de que ya exista una cuenta de almacenamiento, esta se vuelve a usar.
   - **Discos administrados de réplica (la máquina virtual de origen usa discos administrados)** : Site Recovery crea nuevos discos administrados de réplica en la región de destino para reflejar los discos administrados de la máquina virtual de origen con el mismo tipo de almacenamiento (Estándar o Premium) que el disco administrado de la máquina virtual de origen.
   - **Cuentas de almacenamiento en caché**: Site Recovery necesita una cuenta de almacenamiento adicional denominada almacenamiento en caché en la región de origen. Todos los cambios que se producen en las máquinas virtuales de origen se siguen y se envían a la cuenta de almacenamiento en caché antes de su replicación en la ubicación de destino. Esta cuenta de almacenamiento debe ser Estándar.
   - **Conjuntos de disponibilidad de destino**: de forma predeterminada, Site Recovery crea un nuevo conjunto de disponibilidad en la región de destino con el sufijo "asr" en el nombre para las máquinas virtuales que forman parte de un conjunto de disponibilidad en la región de origen. Si el conjunto de disponibilidad que crea Site Recovery ya existe, se vuelve a usar.
   - **Zonas de disponibilidad de destino**: de forma predeterminada, Site Recovery asigna el mismo número de zona que la región de origen en la región de destino si esta admite zonas de disponibilidad.

     Si la región de destino no admite zonas de disponibilidad, las máquinas virtuales de destino se configurarán como instancias únicas de forma predeterminada. Si es necesario, puede configurar esas máquinas virtuales para que formen parte de los conjuntos de disponibilidad de la región de destino. Para ello, haga clic en "Personalizar".

     >[!NOTE]
     >No puede cambiar el tipo de disponibilidad (instancia única, zona de disponibilidad o conjunto de disponibilidad) después de habilitar la replicación. Tendrá que deshabilitar y habilitar la replicación para cambiar el tipo de disponibilidad.
     >

   - **Directiva de replicación**: define la configuración del historial de retención del punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De manera predeterminada, Azure Site Recovery crea una nueva directiva de replicación con la configuración predeterminada de "24 horas" para la retención del punto de recuperación y "4 horas" para la frecuencia de instantánea coherente con la aplicación.

     ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

### <a name="enable-replication-for-added-disks"></a>Habilitación de la replicación para discos agregados

Si se agregan discos a una máquina virtual de Azure para la que está habilitada la replicación, ocurre lo siguiente:
-   El mantenimiento de la replicación de la máquina virtual muestra una advertencia y una nota le informa de que están disponibles uno o más discos para la protección.
-   Si habilita la protección para los discos agregados, la advertencia desaparecerá después de la replicación inicial del disco.
-   Si decide no habilitar la replicación para el disco, puede seleccionar descartar la advertencia.


    ![Nuevo disco agregado](./media/azure-to-azure-how-to-enable-replication/newdisk.png)

Para habilitar la replicación para un disco agregado, haga lo siguiente:

1.  En el almacén > **Elementos replicados**, haga clic en la VM a la que agregó el disco.
2.  Haga clic en **Discos** y, luego, seleccione el disco de datos para el que quiere habilitar la replicación (estos discos tienen un estado de **No protegido**).
3.  En **Detalles del disco**, haga clic en **Habilitar replicación**.

    ![Habilitación de la replicación para un disco agregado](./media/azure-to-azure-how-to-enable-replication/enabled-added.png)

Después de la ejecución del trabajo de habilitación de la replicación y cuando la replicación inicial finaliza, se elimina la advertencia de mantenimiento de replicación de la incidencia del disco.



## <a name="customize-target-resources"></a>Personalizar los recursos de destino

Puede modificar la configuración de destino predeterminada utilizada por Site Recovery.

1. Haga clic en la opción **Personalizar:** que se encuentra al lado de "Suscripción de destino" para modificar la suscripción de destino predeterminada. Seleccione la suscripción en la lista de todas las suscripciones disponibles en el mismo inquilino de Azure Active Directory (AAD).

2. Haga clic en **Personalizar:** para modificar la configuración predeterminada:
    - En **Grupo de recursos de destino**, seleccione el grupo de recursos en la lista de todos los grupos de recursos que existen en la ubicación de destino de la suscripción.
    - En **Red virtual de destino**, seleccione la red en una lista de todas las redes virtuales en la ubicación de destino.
    - En **Conjunto de disponibilidad**, puede agregar la configuración del conjunto de disponibilidad a la máquina virtual en caso de que forme parte de un conjunto de disponibilidad de la región de origen.
    - En **Cuentas de almacenamiento de destino**, seleccione la cuenta que desea usar.

        ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
3. Haga clic en **Personalizar:** para modificar la configuración de replicación.
4. En **Coherencia entre varias máquinas virtuales**, seleccione las máquinas virtuales que desea replicar juntas.
    - Todas las máquinas de un grupo de replicación tendrán puntos de recuperación compartidos coherentes con los bloqueos y coherentes con la aplicación cuando conmutan por error.
    - La habilitación de la coherencia entre varias máquinas virtuales puede afectar al rendimiento de la carga de trabajo (consume mucha CPU). Se debe habilitar únicamente si las máquinas ejecutan la misma carga de trabajo y se necesita coherencia entre varias máquinas.
    - Por ejemplo, si una aplicación tiene dos máquinas virtuales de SQL Server y dos servidores web, solo debe agregar a un grupo de replicación las máquinas virtuales de SQL Server.
    - Puede tener un máximo de 16 máquinas virtuales en un grupo de replicación.
    - Si habilita la coherencia entre varias máquinas virtuales, las máquinas del grupo de replicación se comunican entre sí a través del puerto 20004.
    - Asegúrese de que no haya ningún dispositivo de firewall que bloquee la comunicación interna entre las máquinas virtuales en el puerto 20004.
    - Si desea que las máquinas virtuales Linux formen parte de un grupo de replicación, asegúrese de que el tráfico saliente en el puerto 20004 se abra manualmente según la guía de la versión específica de Linux.
![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)

5. Haga clic en **Crear recurso de destino** > **Habilitar replicación**.
6. Después de que las máquinas virtuales están habilitadas para la replicación, puede comprobar el estado de mantenimiento de las máquinas virtuales en **Elementos replicados**.

>[!NOTE]
>Durante la replicación inicial, el estado de la máquina virtual puede tardar un tiempo en actualizarse, sin ningún progreso. Haga clic en el botón **Actualizar** para ver el estado más reciente.
>

## <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
