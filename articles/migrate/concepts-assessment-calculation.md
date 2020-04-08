---
title: Evaluaciones en Server Assessment de Azure Migrate
description: Obtenga información sobre las evaluaciones en Server Assessment de Azure Migrate.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: ae55686f0152d9c2b170ae1b34d7493ed7ac8d94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127774"
---
# <a name="assessments-in-azure-migrateserver-assessment"></a>Evaluaciones en Server Assessment de Azure Migrate

Este artículo sirve de introducción a las evaluaciones en la herramienta [Server Assessment de Azure Migrate](migrate-services-overview.md#azure-migrate-server-assessment-tool). La herramienta Server Assessment evalúa las máquinas virtuales de VMware locales, las máquinas virtuales de Hyper-V y los servidores físicos para migrarlos a Azure.

## <a name="whats-an-assessment"></a>¿Qué es una evaluación?

Una evaluación con la herramienta Server Assessment mide el grado de preparación (y calcula el impacto) de una migración de servidores locales a Azure.

## <a name="types-of-assessments"></a>Tipos de evaluaciones

Las evaluaciones que se crean con Server Assessment son una instantánea puntual de los datos. Server Assessment proporciona dos tipos de evaluaciones.

**Tipo de evaluación** | **Detalles** | **Data**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones que realizan recomendaciones basadas en datos de rendimiento recopilados | La recomendación del tamaño de VM se basa en los datos de uso de la CPU y la memoria.<br/><br/> La recomendación de tipo de disco (SSD/disco duro estándar o discos administrados prémium) se basa en el IOPS y el rendimiento de los discos locales.
**Tal cual en el entorno local** | Evaluaciones que no usan datos de rendimiento para hacer recomendaciones. | La recomendación de tamaño de la máquina virtual se basa en el tamaño de la máquina virtual local.<br/><br> El tipo de disco recomendado se basa en el tipo de almacenamiento para la evaluación.

## <a name="how-do-i-run-an-assessment"></a>¿Cómo se ejecuta una evaluación?

Existen dos formas de realizar una evaluación:

- Evaluar máquinas usando metadatos de servidor recopilados por un dispositivo de Azure Migrate ligero. El dispositivo detecta las máquinas locales y envía los metadatos/datos de rendimiento de la máquina a Azure Migrate.
- Evaluar máquinas usando metadatos de servidor importados en un formato de valores separados por comas (CSV).

## <a name="how-do-i-assess-with-the-appliance"></a>¿Cómo se evalúa con el dispositivo?

Haga lo siguiente si va a implementar un dispositivo de Azure Migrate para detectar servidores locales:

1. Configure Azure y el entorno local para que funcionen con Server Assessment.
2. Para realizar la primera evaluación, cree un proyecto de Azure y, luego, agréguele la herramienta Server Assessment.
3. Implemente un dispositivo de Azure Migrate ligero. Este dispositivo detecta las máquinas locales de forma constante y envía a Azure los metadatos y los datos de rendimiento de las máquinas. El dispositivo se implementa como una máquina virtual o física. No es necesario instalar nada en los equipos que quiera evaluar.
4. Una vez que el dispositivo inicia la detección de máquinas, puede reunir aquellas que quiera evaluar en un grupo y realizar una evaluación de ese grupo.

Para probar estos procedimientos, puede seguir nuestros tutoriales de [VMware](tutorial-prepare-vmware.md), [Hyper-V](tutorial-prepare-hyper-v.md) o [servidores físicos](tutorial-prepare-physical.md).

## <a name="how-do-i-assess-with-imported-data"></a>¿Cómo se evalúa con datos importados?

Si está evaluando servidores con un archivo CSV, no necesita un dispositivo, sino hacer lo siguiente:

1. Configure Azure para que funcione con Server Assessment.
2. Para realizar la primera evaluación, cree un proyecto de Azure y, luego, agréguele la herramienta Server Assessment.
3. Descargue una plantilla CSV y agregue a ella los datos de los servidores.
4. Importe la plantilla a Server Assessment.
5. Detecte los servidores agregados con la importación, reúnalos en un grupo y realice una evaluación de ese grupo.

## <a name="what-data-does-the-appliance-collect"></a>¿Qué datos recopila el dispositivo?

Si usa el dispositivo Azure Migrate para la evaluación, obtenga información sobre los metadatos y los datos de rendimiento que se recopilan en [VMware](migrate-appliance.md#collected-data---vmware) y [Hyper-V](migrate-appliance.md#collected-data---hyper-v).

## <a name="how-does-the-appliance-calculate-performance-data"></a>¿Cómo calcula el dispositivo los datos de rendimiento?

Si usa el dispositivo para la detección, los datos de rendimiento de la configuración de proceso se recopilan de la siguiente manera:

1. El dispositivo recopila un punto de ejemplo en tiempo real:

    - **VM de VMware**: el dispositivo recopila un punto de ejemplo en tiempo real en intervalos de 20 segundos.
    - **VM de Hyper-V**: el punto de ejemplo en tiempo real se recopila en intervalos de 30 segundos.
    - **Servidores físicos**: el punto de ejemplo en tiempo real se recopila en cada intervalo de cinco minutos. 
    
2. El dispositivo acumula los puntos de ejemplo (20 segundos, 30 segundos, cinco minutos) para crear un único punto de datos cada 10 minutos. Para crear un único punto, el dispositivo selecciona el valor máximo de todas las muestras y lo envía a Azure.
3. Server Assessment almacena todos los puntos de ejemplo de 10 minutos del último mes.
4. Al crear una evaluación, Server Assessment identifica el punto de datos apropiado que se debe usar para el ajuste de tamaño adecuado en función de los valores del percentil de *Historial de rendimiento* y *Utilización del percentil*.

    - Por ejemplo, si el historial de rendimiento está establecido en una semana y la utilización del percentil es 95, Server Assessment ordena los puntos de ejemplo almacenados cada 10 minutos de la última semana, los clasifica en orden ascendente y selecciona el percentil 95 para el ajuste de tamaño adecuado. 
    - El valor del percentil 95 garantiza que se van a omitir los valores atípicos que se pueden incluir si elige el percentil 99.
    - Si quiere elegir el uso máximo para el período y no desea omitir ningún valor atípico, debe seleccionar el percentil 99 como percentil de uso.

5. Este valor se multiplica por el factor de confort para obtener los datos de uso de rendimiento efectivos para cada métrica (uso de CPU, uso de memoria, IOPS de disco [lectura y escritura], rendimiento de disco [lectura y escritura], rendimiento de red [entrada y salida]) que el dispositivo recopila.



## <a name="how-are-assessments-calculated"></a>¿Cómo se calculan las evaluaciones? 

Las evaluaciones en Server Assessment se calculan usando los metadatos y los datos de rendimiento de las máquinas locales. Si implementa el dispositivo de Azure Migrate, las evaluaciones se realizarán usando los datos recopilados por dicho dispositivo. Si se realiza una evaluación de las máquinas importadas usando un archivo .CSV, hay que proporcionar los metadatos para el cálculo. Los cálculos se producen en tres fases:

1. **Cálculo de la preparación para Azure**: se evalúa si las máquinas son adecuadas para la migración a Azure.
2. **Cálculo de la recomendaciones de tamaño**: se calcula el tamaño de red, almacenamiento y proceso. 
2. **Cálculo de los costos mensuales**: Se calculan los costos mensuales estimados de proceso y almacenamiento por ejecutar las máquinas en Azure tras la migración.

Los cálculos aparecen en orden y un servidor de máquina solo se mueve a una fase posterior si aprueba la anterior. Por ejemplo, si un servidor no supera la preparación para Azure, se marca como no adecuado para Azure, y ya no se calculan el ajuste de tamaño ni los costos para él.


## <a name="whats-in-an-assessment"></a>¿Qué es una evaluación?

Esto es lo que se incluye en una evaluación en Server Assessment.

**Propiedad** | **Detalles**
--- | ---
**Ubicación de destino** | Ubicación a la que se quiere migrar. Server Assessment admite actualmente las siguientes regiones de Azure de destino:<br/><br/> Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Este de Canadá, Centro de la India, Centro de EE. UU., Este de China, Norte de China, Este de Asia, Este de EE. UU., Este de EE. UU. 2, Сentro de Alemania, Nordeste de Alemania, Este de Japón, Oeste de Japón, Centro de Corea del Sur, Corea del Sur, Centro y norte de EE. UU., Norte de Europa, Centro y sur de EE. UU., Sudeste de Asia, Sur de la India, Sur de Reino Unido, Oeste del Reino Unido, US Gov Arizona, US Gov Texas, US Gov Virginia, Centro-oeste de EE. UU., Oeste de Europa, Oeste de la India, Oeste de EE. UU. y Oeste de EE. UU. 2.
*Disco de almacenamiento de destino (ajuste de tamaño tal cual)* * | Tipo de discos que se van a usar para el almacenamiento en Azure. <br/><br/> Especifique el disco de almacenamiento de destino como administrado premium, administrado de SSD estándar o administrado de HDD estándar.
**Disco de almacenamiento de destino (ajuste de tamaño basado en el rendimiento)** | Especifique el tipo de disco de almacenamiento de destino como automático, administrado premium, administrado de HDD estándar o administrado de SSD estándar.<br/><br/> **Automático**: La recomendación de disco se realiza en función de los datos de rendimiento de los discos (las operaciones de entrada/salida por segundo [IOPS] y el rendimiento).<br/><br/>**Premium o estándar**:  La evaluación recomienda una SKU de disco dentro del tipo de almacenamiento seleccionado.<br/><br/> Si quiere lograr un Acuerdo de Nivel de Servicio de VM de una sola instancia del 99,9 %, considere el uso de discos administrados premium. Esto garantiza que todos los discos de la evaluación se recomienden como discos administrados premium.<br/><br/> Azure Migrate solo admite discos administrados para la valoración de la migración.
**Instancias reservadas (RI)** | Especifique [instancias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) en Azure para que las estimaciones de costos en la evaluación tengan en cuenta los descuentos de RI.<br/><br/> Actualmente, las instancias reservadas solo se admiten en ofertas de pago por uso en Azure Migrate.
**Criterio de tamaño** | Se usa para ajustar el tamaño de la VM en Azure.<br/><br/> Se usa el ajuste de tamaño tal cual o basado en el rendimiento.
**Historial de rendimiento** | Se usa el ajuste de tamaño basado en el rendimiento. Especifique la duración usada al evaluar los datos de rendimiento.
**Uso de percentil** | Se usa el ajuste de tamaño basado en el rendimiento. Especifica el valor de percentil del ejemplo de rendimiento que se va a usar para determinar el tamaño adecuado. 
**Series de VM** | Especifique la serie de VM de Azure que quiere tener en cuenta para determinar el tamaño adecuado. Por ejemplo, si no tiene un entorno de producción que necesite VM de serie A en Azure, puede excluir la serie A de la lista o serie.
**Factor de confort** | Búfer usado durante la evaluación. Se aplica además de los datos de uso de la máquina en las VM (CPU, memoria, disco y red). Se tiene en cuenta en problemas como el uso estacional, un historial de rendimiento corto y los posibles aumentos en el uso futuro.<br/><br/> Por ejemplo, una VM de 10 núcleos con un uso del 20 % normalmente genera una VM de dos núcleos. Con un factor de confort de 2.0x, el resultado es una VM de cuatro núcleos.
**Oferta** | Muestra la [oferta de Azure](https://azure.microsoft.com/support/legal/offer-details/) en la que está inscrito. Server Assessment calcula el costo en consecuencia.
**Moneda** | Moneda de facturación de la cuenta.
**Descuento (%)** | Muestra cualquier descuento específico de la suscripción que recibe además de la oferta de Azure. La configuración predeterminada es 0 %.
**Tiempo de actividad de VM** | Si las VM de Azure no se ejecutan 24 horas al día, 7 días a la semana, puede especificar la duración (días por mes y horas por día) de la ejecución. Las estimaciones de costos se controlan en consecuencia.<br/><br/> El valor predeterminado es 31 días al mes y 24 horas al día.
**Ventaja híbrida de Azure** | Especifique si dispone de Software Assurance y tiene derecho a la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Si se establece en Sí (configuración predeterminada), los precios de Azure que no son de Windows se toman en cuenta para las VM de Windows.

[Revise los procedimientos recomendados](best-practices-assessment.md) para crear una evaluación con Server Assessment.


## <a name="calculate-readiness"></a>Cálculo de la preparación

No todas las máquinas son adecuadas para ejecutarse en Azure. Server Assessment evalúa cada máquina local y la asigna a una categoría de preparación. 
- **Preparada para Azure**: la máquina se puede migrar como está a Azure sin realizar ningún cambio. Se iniciará en Azure con soporte técnico de Azure completo.
- **Condicionalmente preparada para Azure**: es posible que la máquina se inicie en Azure, pero puede que no tenga soporte técnico de Azure completo. Por ejemplo, en Azure no se admite una máquina que ejecute una versión anterior de Windows Server. Debe tener cuidado antes de migrar estas máquinas a Azure. Siga las instrucciones de soluciones sugeridas en la evaluación para solucionar los problemas de preparación.
- **No está preparado para Azure**: la máquina no se iniciará en Azure. Por ejemplo, si un disco de máquina local tiene más de 64 TB, no se puede hospedar en Azure. Siga las instrucciones de soluciones para solucionar el problema antes de la migración. 
- **Preparación desconocida**: Azure Migrate no ha podido encontrar la preparación de la máquina debido a una recopilación insuficiente de metadatos.

Para calcular la preparación, Server Assessment revisa las propiedades de la máquina y la configuración del sistema operativo resumidas en las tablas siguientes. 

### <a name="machine-properties"></a>Propiedades de la máquina

Server Assessment revisa las siguientes propiedades de la máquina virtual local para identificar si se puede ejecutar en Azure.

**Propiedad** | **Detalles** | **Estado de la preparación para Azure**
--- | --- | ---
**Tipo de arranque** | Azure admite máquinas virtuales con un tipo de arranque de BIOS, no UEFI. | Condicionalmente preparada si el tipo de arranque es UEFI.
**Núcleos** | El número de núcleos de las máquinas debe ser igual o menor que el número máximo de núcleos (128) admitidos en una máquina virtual de Azure.<br/><br/> Si el historial de rendimiento está disponible, Azure Migrate tiene en cuenta los núcleos utilizados para la comparación. Si se especifica un factor de confort en la configuración de evaluación, el número de núcleos utilizados se multiplica por dicho factor de confort.<br/><br/> Si no hay ningún historial de rendimiento, Azure Migrate usa los núcleos asignados sin aplicar el factor de confort. | Preparada si es menor o igual a los límites.
**Memoria** | La memoria de la máquina debe tener un tamaño igual o menor que la memoria máxima (3892 gigabytes [GB] en Azure M serie Standard_M128m&nbsp;<sup>2</sup>) permitida en una máquina virtual de Azure. [Más información](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Si hay algún historial de rendimiento disponible, Azure Migrate tiene en cuenta la memoria utilizada para la comparación. Si se especifica un factor de confort, la memoria utilizada se multiplica por dicho factor de confort.<br/><br/> Si no hay ningún historial, se usa la memoria asignada sin aplicar el factor de confort.<br/><br/> | Preparada si está dentro de los límites.
**Disco de almacenamiento** | El tamaño asignado de un disco debe ser de 32 TB o menos. Aunque Azure admite discos de 64 TB con discos SSD Ultra, Azure Migrate: Server Assessment utiliza actualmente 32 TB como límite de tamaño del disco, ya que todavía no admite SSD Ultra. <br/><br/> El número de discos conectados a la máquina debe ser 65 o menos, incluido el disco de sistema operativo. | Preparada si está dentro de los límites.
**Redes** | Una máquina debe tener 32 o menos interfaces de red (NIC) conectadas. | Preparada si está dentro de los límites.

### <a name="guest-operating-system"></a>Sistema operativo invitado
Junto con las propiedades de la máquina virtual, Server Assessment examina el sistema operativo invitado de las máquinas para determinar si se puede ejecutar en Azure.

> [!NOTE]
> En el caso de las máquinas virtuales de VMware, Server Assessment utiliza el sistema operativo especificado para la máquina virtual en vCenter Server para realizar el análisis del sistema operativo invitado. En el caso de las máquinas virtuales de Linux que se ejecutan en VMware, actualmente no se identifica la versión exacta del kernel del sistema operativo invitado.

Server Assessment emplea la siguiente lógica para identificar la preparación para Azure en función del sistema operativo.

**Sistema operativo** | **Detalles** | **Estado de la preparación para Azure**
--- | --- | ---
Windows Server 2016 y todos los Service Pack | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2012 R2 y todos los Service Pack | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2012 y todos los Service Pack | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2008 R2 con todos los Service Pack | Azure proporciona compatibilidad completa.| Preparada para Azure
Windows Server 2008 (32 bits y 64 bits) | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2003 y 2003 R2 | Estos sistemas operativos alcanzaron la fecha de finalización del soporte técnico y necesitan un [contrato de soporte técnico personalizado (CSA)](https://aka.ms/WSosstatement) para obtener soporte técnico en Azure. | Condicionalmente preparada para Azure. Considere la posibilidad de actualizar el sistema operativo antes de migrar a Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Estos sistemas operativos han superado la fecha de finalización de soporte técnico. La máquina puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. | Condicionalmente preparada para Azure. Se recomienda actualizar el sistema operativo antes de migrar a Azure.
Cliente de Windows 7, 8 y 10 | Azure solo proporciona compatibilidad con la [suscripción de Visual Studio](https://docs.microsoft.com/azure/virtual-machines/windows/client-images). | Condicionalmente preparada para Azure
Windows 10 Pro Desktop | Azure proporciona compatibilidad con [derechos de hospedaje multiinquilino.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Condicionalmente preparada para Azure
Windows Vista, XP Professional | Estos sistemas operativos han superado la fecha de finalización de soporte técnico. La máquina puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. | Condicionalmente preparada para Azure. Se recomienda actualizar el sistema operativo antes de migrar a Azure.
Linux | Azure aprueba estos [sistemas operativos Linux](../virtual-machines/linux/endorsed-distros.md). Otros sistemas operativos Linux pueden iniciarse en Azure, pero recomendamos actualizar el sistema operativo a una versión aprobada antes de migrar a Azure. | Preparada para Azure si la versión está aprobada.<br/><br/>Condicionalmente preparada si la versión no se ha aprobado.
Otros sistemas operativos<br/><br/> Por ejemplo, Oracle Solaris, Apple macOS, FreeBSD, etc. | Azure no aprueba estos sistemas operativos. La máquina puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. | Condicionalmente preparada para Azure. Se recomienda instalar un sistema operativo compatible antes de migrar a Azure.  
Sistema operativo especificado como **otro** en vCenter Server. | Azure Migrate no puede identificar el sistema operativo en este caso. | Preparación desconocida. Asegúrese de que el sistema operativo que se ejecuta dentro de la máquina virtual se admite en Azure.
Sistemas operativos de 32 bits | Es posible que la máquina se inicie en Azure, pero que Azure no proporcione soporte técnico completo. | Condicionalmente preparada para Azure. Considere la posibilidad de actualizar el sistema operativo de la máquina de un sistema operativo de 32 bits a uno de 64 bits antes de migrar a Azure.

## <a name="calculating-sizing"></a>Cálculo del tamaño


Una vez que la máquina se marca como preparada para Azure, Server Assessment realiza recomendaciones de tamaño para identificar la VM de Azure y la SKU del disco. Los cálculos de tamaño dependen de si se usa el ajuste de tamaño local tal cual o en función del rendimiento.

### <a name="calculate-sizing-as-is-on-premises"></a>Cálculo del ajuste de tamaño (local tal cual)

 Si usa el ajuste de tamaño local tal cual, Server Assessment no tiene en cuenta el historial de rendimiento de las VM ni los discos.

- **Ajuste de tamaño de proceso** : Asigna una SKU de VM en Azure en función del tamaño asignado en el entorno local.
- **Ajuste de tamaño de almacenamiento y disco**: Server Assessment examina el tipo de almacenamiento especificado en las propiedades de evaluación (HDD estándar/SSD/premium) y recomienda el tipo de disco adecuado. El tipo de almacenamiento predeterminado es discos premium.
- **Ajuste de tamaño de red**: Server Assessment considera el adaptador de red en la máquina local.


### <a name="calculate-sizing-performance-based"></a>Cálculo del ajuste de tamaño (basado en el rendimiento)

Si usa el ajuste de tamaño basado en el rendimiento, Server Assessment realiza recomendaciones de ajuste de tamaño de la siguiente manera:

- Server Assessment tiene en cuenta el historial de rendimiento de la máquina para identificar el tamaño y el tipo de disco de la VM en Azure.
- Si los servidores se han importado mediante un archivo .csv, se usan los valores especificados. Este método es especialmente útil si ha asignado en exceso la máquina local, pero el uso es bajo realmente, y quiere ajustar adecuadamente el tamaño de la VM en Azure para ahorrar costos. 
- Si no quiere usar los datos de rendimiento, restablezca el criterio de ajuste de tamaño a local tal cual según se describe en la sección anterior.

#### <a name="calculate-storage-sizing"></a>Cálculo del ajuste de tamaño de almacenamiento

En el caso del ajuste de tamaño de almacenamiento, Azure Migrate intenta asignar cada disco asociado a la máquina a un disco en Azure y funciona de la siguiente manera:

1. Server Assessment suma los valores de IOPS de lectura y escritura de un disco para obtener el valor total de IOPS necesario. Del mismo modo, suma los valores de rendimiento de lectura y escritura para obtener el rendimiento total de cada disco.
2. Si ha especificado el tipo de almacenamiento como Automático, en función de los valores de rendimiento e IOPS efectivos, Server Assessment determina si el disco debe asignarse a un disco HDD estándar, un disco SSD estándar o un disco premium en Azure. Si el tipo de almacenamiento se establece en HDD estándar/SSD/premium, Server Assessment intentará encontrar una SKU de disco en el tipo de almacenamiento seleccionado (discos HDD estándar/SSD/premium).
3. Los discos se seleccionan de la siguiente manera:
    - Si Server Assessment no puede encontrar un disco con los valores de rendimiento e IOPS necesarios, marca la máquina como no adecuada para Azure.
    - Si Server Assessment encuentra un conjunto de discos adecuados, selecciona aquellos que admiten la ubicación especificada en la configuración de evaluación.
    - Si hay varios discos coincidentes, Server Assessment selecciona el disco con el costo más bajo.
    - Si no están disponibles los datos de rendimiento de algún disco, se usan sus datos de configuración (tamaño del disco) para buscar un disco SSD estándar en Azure.

#### <a name="calculate-network-sizing"></a>Cálculo del ajuste de tamaño de red

Server Assessment intenta encontrar una máquina virtual de Azure que pueda admitir el número de adaptadores de red conectados a la máquina local y el rendimiento que requieren estos adaptadores de red.
- Para obtener el rendimiento de red efectivo de la máquina virtual local, Server Assessment suma los datos transmitidos por segundo (MBps) fuera de la máquina (salida de red), en todos los adaptadores de red, y aplica el factor de confort. Utiliza este número para buscar una máquina virtual de Azure que pueda cumplir el rendimiento de red requerido.
- Junto con el rendimiento de la red, Server Assessment también tiene en cuenta si la máquina virtual de Azure puede admitir el número de adaptadores de red necesarios.
- Si no hay datos de rendimiento de red disponibles, Server Assessment solo tiene en cuenta el número de adaptadores de red para el ajuste de tamaño de la máquina virtual.


#### <a name="calculate-compute-sizing"></a>Cálculo del ajuste de tamaño de proceso

después de calcular los requisitos de almacenamiento y red, Server Assessment tiene en cuenta los requisitos de CPU y memoria para encontrar un tamaño de máquina virtual adecuado en Azure.
- Azure Migrate examina los núcleos y la memoria utilizados efectivos para encontrar un tamaño de máquina virtual adecuado en Azure.
- Si no encuentra ningún tamaño adecuado, la máquina se marca como no adecuada para Azure.
- Si encuentra un tamaño adecuado, Azure Migrate aplica los cálculos de red y almacenamiento. Después, aplica la configuración de ubicación y plan de tarifa para realizar la recomendación final de tamaño de máquina virtual.
- Si hay varios tamaños de máquina virtual de Azure adecuados, se recomienda el que tiene el costo más bajo.


## <a name="confidence-ratings-performance-based"></a>Clasificaciones de confianza (basadas en el rendimiento)

Cada evaluación basada en el rendimiento de Azure Migrate se asocia a una clasificación de confianza comprendida entre una estrella (la más baja) y cinco estrellas (la más alta). La clasificación de confianza sirve de ayuda para calcular la confiabilidad de las recomendaciones de tamaño que Azure Migrate proporciona.

- La clasificación de confianza se asigna a una valoración que se basa en la disponibilidad de puntos de datos necesarios para calcular tal valoración.
- Para ajustes de tamaño según el rendimiento, Server Assessment necesita lo siguiente:
    - Los datos de uso de la CPU y la memoria de la máquina virtual.
    - Los datos de IOPS y rendimiento de cada disco conectado a la máquina virtual.
    - La entrada o la salida de red de cada adaptador de red conectado a una máquina virtual.
    - Si alguna de las cifras de uso anteriores no está disponible, la recomendación de tamaño podría no ser confiable.

> [!NOTE]
> No se asignan clasificaciones de confianza en el caso de los servidores evaluados con un archivo .CSV importado. Las clasificaciones tampoco son aplicables a las evaluaciones locales tal cual.
   
### <a name="ratings"></a>Clasificaciones

A continuación se muestra la clasificación de confianza para la evaluación según el porcentaje de puntos de datos disponibles.

   **Disponibilidad de puntos de datos** | **Clasificación de confianza**
   --- | ---
   De 0 a 20 % | 1 estrella
   De 21 a 40 % | 2 estrellas
   De 41 a 60 % | 3 estrellas
   De 61 a 80 % | 4 estrellas
   De 81 a 100 % | 5 estrellas

### <a name="low-confidence-ratings"></a>Clasificaciones de confianza bajas

Estos son algunos de los motivos por los que una evaluación puede obtener una clasificación de confianza baja:

- No ha generado un perfil de su entorno durante el tiempo que ha estado creando la evaluación. Por ejemplo, si crea la evaluación con la duración de rendimiento establecida en un día, debe esperar al menos un día después de empezar la detección para que se recopilen todos los puntos de datos.
- Se apagaron algunas máquinas virtuales en el período durante el que se calculó la valoración. Si alguna máquina virtual se apaga durante un tiempo, Server Assessment no puede recopilar los datos de rendimiento de ese período.
- Se crearon algunas máquinas virtuales en el período durante el que se calculó la valoración. Por ejemplo, si creó una evaluación para el historial de rendimiento del último mes, pero se crearon algunas máquinas virtuales en el entorno hace solo una semana, el historial de rendimiento de las nuevas máquinas virtuales no abarcará toda la duración.

> [!NOTE]
> Si la clasificación de confianza de una evaluación no llega a las cinco estrellas, se recomienda que espere al menos un día para que el dispositivo genere el perfil del entorno y luego recalcule la evaluación. En caso contrario, es posible que el ajuste de tamaño basado en el rendimiento no sea confiable. En ese caso, se recomienda cambiar la evaluación a un ajuste de tamaño local.

## <a name="calculate-monthly-costs"></a>Cálculo de los costos mensuales

Una vez completadas las recomendaciones de ajuste de tamaño, Azure Migrate calcula los costos de almacenamiento y proceso para después de la migración.

- **Costo de proceso**: con el tamaño recomendado de máquina virtual de Azure, Azure Migrate usa Billing API para calcular el costo mensual de la máquina virtual.
    - El cálculo tiene en cuenta la configuración del sistema operativo, Software Assurance, las instancias reservadas, el tiempo de actividad de las máquinas virtuales, la ubicación y la moneda.
    - Para calcular el costo de proceso mensual total, suma el costo de todas las máquinas.
- **Costo de almacenamiento**: el costo de almacenamiento mensual de una máquina se calcula sumando el costo mensual de todos los discos conectados a la máquina, del siguiente modo:
    - Server Assessment calcula los costos de almacenamiento mensuales totales sumando los costos de almacenamiento de todas las máquinas.
    - Actualmente, el cálculo no tiene en cuenta las ofertas especificadas en la configuración de evaluación.

Los costos se muestran en la moneda especificada en la configuración de evaluación.


## <a name="next-steps"></a>Pasos siguientes

[Revise](best-practices-assessment.md) los procedimientos recomendados para crear evaluaciones. 


- Obtenga información sobre la ejecución de evaluaciones para [VM de VMware](tutorial-prepare-vmware.md), [VM de Hyper-V](tutorial-prepare-hyper-v.md) y [servidores físicos](tutorial-prepare-physical.md).
- Obtenga información sobre la evaluación de servidores [importados con un archivo CSV](tutorial-assess-import.md).
- Información acerca de la configuración de [visualización de dependencias](concepts-dependency-visualization.md).