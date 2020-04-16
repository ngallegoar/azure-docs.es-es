---
title: Soluciones de Oracle en Azure Virtual Machines | Microsoft Docs
description: Aprenda sobre las configuraciones admitidas y las limitaciones de las imágenes de máquinas virtuales de Oracle en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 0a2374a4c3526b77a25f9fa8faa94c9cb0d4c4ea
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263240"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Imágenes de máquinas virtuales de Oracle y su implementación en Microsoft Azure

Este artículo incluye información sobre soluciones de Oracle basadas en las imágenes de máquinas virtuales publicadas por Oracle en Azure Marketplace. Si está interesado en soluciones de aplicaciones entre nubes con Oracle Cloud Infrastructure, consulte [Soluciones de aplicaciones de Oracle que integran Microsoft Azure y Oracle Cloud Infrastructure](oracle-oci-overview.md).

Para obtener una lista de imágenes disponibles actualmente, ejecute el siguiente comando:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

A partir de mayo de 2019 están disponibles las siguientes imágenes:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Estas imágenes se consideran "Traiga su propia licencia" y, por tanto, solo se le cobrarán los costos de proceso, almacenamiento y red derivados de la ejecución de una máquina virtual.  Se supone que tiene la licencia correcta para usar software Oracle y que cuenta con un acuerdo de soporte técnico actual con Oracle. Oracle ha garantizado la movilidad de licencias del entorno local a Azure. Para más información sobre la movilidad de licencias, consulte la nota publicada de [Oracle y Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html). 

Los individuos pueden elegir también basar sus soluciones en una imagen personalizada que crean desde cero en Azure o carga su imagen personalizada de su entorno local.

## <a name="oracle-database-vm-images"></a>Imágenes de máquinas virtuales de base de datos de Oracle
Oracle admite la ejecución de Oracle Database 12.1 y posteriores en edición Standard y Enterprise en Azure en imágenes de máquina virtual basadas en Oracle Linux.  Para conseguir el mejor rendimiento para las cargas de trabajo de producción de Oracle Database en Azure, asegúrese de ajustar correctamente el tamaño de la imagen de máquina virtual y de usar discos administrados SSD Premium o SSD Ultra. Para obtener instrucciones sobre cómo conseguir poner en marcha rápidamente una instancia de Oracle Database en Azure mediante la imagen de máquina virtual publicada de Oracle, [pruebe el tutorial de inicio rápido de Oracle Database](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Opciones de configuración de discos conectados

Los discos conectados se basan en el servicio de almacenamiento de blobs de Azure. Teóricamente, cada disco estándar puede realizar aproximadamente 500 operaciones de entrada/salida por segundo (IOPS). Nuestra oferta de disco premium es preferible para las cargas de trabajo de base de datos de alto rendimiento y puede alcanzar las 5000 IOPS disco. Puede usar un único disco si con ello cubre sus necesidades de rendimiento. No obstante, puede mejorar el rendimiento de IOPS efectivo si usa varios discos conectados, reparte los datos de la base de datos entre ellos y, a continuación, usa Oracle Automatic Storage Management (ASM). Consulte la [información general sobre Oracle Automatic Storage](https://www.oracle.com/technetwork/database/index-100339.html) para más información específica de Oracle ASM. Para ver un ejemplo de cómo instalar y configurar Oracle ASM en una máquina virtual Linux de Azure, consulte el tutorial [Instalación y configuración de Oracle Automated Storage Management](configure-oracle-asm.md).

### <a name="shared-storage-configuration-options"></a>Opciones de configuración de almacenamiento compartido

Azure NetApp Files se diseñó para satisfacer los requisitos principales de la ejecución de cargas de trabajo de alto rendimiento, como bases de datos, en la nube y proporciona:
- Un servicio de almacenamiento nativo NFS compartido para ejecutar cargas de trabajo de Oracle mediante un cliente NFS nativo de máquina virtual o de Oracle dNFS
- Niveles de rendimiento escalables que reflejan el intervalo real de demandas de IOPS
- Baja latencia
- Alta disponibilidad, durabilidad y capacidad de administración a escala, normalmente exigidas por las cargas de trabajo empresariales críticas (como SAP y Oracle)
- Copia de seguridad y recuperación rápidas y eficaces, para lograr el RTO y RPO del Acuerdo de Nivel de Servicio más agresivo

Estas funcionalidades son posibles porque Azure NetApp Files está basado en sistemas all-flash de NetApp® ONTAP® que se ejecutan en el entorno del centro de datos de Azure como un servicio nativo de Azure. El resultado es una tecnología de almacenamiento de base de datos idónea que se puede aprovisionar y utilizar como otras opciones de almacenamiento de Azure. Consulte la [documentación de Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/) para más información sobre cómo implementar y acceder a los volúmenes NFS de Azure NetApp Files. Consulte [Guía de procedimientos recomendados para la implementación de Oracle en Azure con Azure NetApp Files](https://www.netapp.com/us/media/tr-4780.pdf) para conocer los procedimientos recomendados para manejar una base de datos de Oracle en Azure NetApp Files.


## <a name="licensing-oracle-database--software-on-azure"></a>Licencias para Oracle Database y software en Azure
Microsoft Azure es un entorno de nube autorizado para ejecutar Oracle Database. La tabla de factores de núcleos de Oracle no se aplica al conceder licencias a las bases de datos de Oracle en la nube. En su lugar, al usar las máquinas virtuales con la tecnología Hyper-Threading habilitada para las bases de datos de la edición Enterprise, cuente dos vCPU como equivalentes a una licencia de procesador de Oracle si el hyperthreading está habilitado (como se indica en el documento de directiva). Puede consultar los detalles de la directiva [aquí](http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf).
Las bases de datos de Oracle suelen requerir una mayor cantidad de memoria y E/S. Por esta razón, se recomienda usar [máquinas virtuales optimizadas para memoria](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) para estas cargas de trabajo. Para optimizar aún más las cargas de trabajo, se recomiendan las [CPU virtuales restringidas en núcleos](https://docs.microsoft.com/azure/virtual-machines/linux/constrained-vcpu) para las cargas de trabajo Oracle Database que requieran gran cantidad de memoria, almacenamiento y ancho de banda de E/S, pero no un gran número de núcleos.

Al migrar el software y las cargas de trabajo de Oracle del entorno local a Microsoft Azure, Oracle ofrece movilidad de licencias como se indica en las [preguntas más frecuentes sobre Oracle en Azure](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html).


## <a name="oracle-real-application-cluster-oracle-rac"></a>Clúster de aplicaciones reales de Oracle (Oracle RAC)
Oracle Real Application Clusters (Oracle RAC) se ha diseñado para mitigar el error de un nodo individual en una configuración local de clúster de varios nodos. Se basa en dos tecnologías locales que no son nativas para los entornos de nube pública de hiperescala: multidifusión y disco compartido de red. Si la solución de base de datos requiere Oracle RAC en Azure, se necesita software de terceros para habilitar estas tecnologías. Para más información sobre Oracle RAC, consulte la [página de FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Consideraciones sobre la alta disponibilidad y la recuperación ante desastres
Si usa bases de datos de Oracle en Azure, es responsable de implementar una solución de recuperación ante desastres y alta disponibilidad para evitar tiempo de inactividad. 

La alta disponibilidad y recuperación ante desastres para Oracle Database Enterprise Edition (sin basarse en Oracle RAC) se puede lograr en Azure mediante [Protección de datos, Protección de datos activa](https://www.oracle.com/database/technologies/high-availability/dataguard.html) u [Oracle Golden Gate](https://www.oracle.com/technetwork/middleware/goldengate), con dos bases de datos en dos máquinas virtuales independientes. Ambas máquinas virtuales deben estar en la misma [red virtual](https://azure.microsoft.com/documentation/services/virtual-network/) para asegurarse de que pueden acceder entre sí a través de la dirección IP privada persistente.  Además, recomendamos colocar las máquinas virtuales en el mismo conjunto de disponibilidad para permitir a Azure colocarlas en dominios de error y de actualización independientes. Si desea tener redundancia geográfica, puede configurar las dos bases de datos para que se repliquen entre dos regiones diferentes y conectar las dos instancias con una instancia de VPN Gateway.

En el tutorial [Implement Oracle DataGuard on Azure](configure-oracle-dataguard.md) (Implementación de Oracle DataGuard en Azure) se describe el procedimiento de configuración básico en Azure.  

Con la Protección de datos de Oracle, se puede lograr alta disponibilidad con una base de datos principal en una máquina virtual, una base de datos secundaria (de reserva) en otra máquina virtual y la replicación unidireccional establecida entre ellos. El resultado es un acceso de lectura a la copia de la base de datos. Con Oracle GoldenGate, puede configurar la replicación bidireccional entre las dos bases de datos. Para más información sobre cómo configurar una solución de alta disponibilidad para bases de datos mediante estas herramientas, consulte la documentación de [Protección de datos activa](https://www.oracle.com/database/technologies/high-availability/dataguard.html) y [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) en el sitio web de Oracle. Si se necesita acceso de lectura-escritura a la copia de la base de datos, puede usar [Protección de datos activa de Oracle](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

En el tutorial [Implement Oracle GoldenGate on Azure](configure-oracle-golden-gate.md) (Implementación de Oracle GoldenGate en Azure) se describe el procedimiento de configuración básico en Azure.

Además de tener una solución de alta disponibilidad y recuperación ante desastres diseñada en Azure, debe contar con una estrategia de copia de seguridad en vigor para restaurar la base de datos. En el tutorial [Backup and recover an Oracle Database](oracle-backup-recovery.md) (Copia de seguridad y recuperación de una instancia de Oracle Database) se describe el procedimiento básico para establecer una copia de seguridad coherente.


## <a name="support-for-jd-edwards"></a>Compatibilidad con JD Edwards
De acuerdo con la nota de compatibilidad de Oracle con el [identificador de documento 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), las versiones de JD Edwards EnterpriseOne 9.2 y superiores se admiten en **cualquier oferta de nube pública** que satisfaga sus `Minimum Technical Requirements` (MTR) específicos.  Debe crear imágenes personalizadas que cumplan sus especificaciones de MTR para la compatibilidad con el sistema operativo y la aplicación de software. 


## <a name="oracle-weblogic-server-virtual-machine-images"></a>Imágenes de máquina virtual de Oracle WebLogic Server

* **La agrupación en clústeres solo se admite en Enterprise Edition.** Tiene licencia para usar la agrupación en clústeres de WebLogic solo cuando utiliza la versión Enterprise Edition de Oracle WebLogic Server. No use la agrupación en clústeres con Oracle WebLogic Server Standard Edition.
* **Tampoco se admite la multidifusión UDP.** Azure admite la unidifusión UDP, pero no admite la multidifusión ni la difusión. Oracle WebLogic Server es capaz de depender de las funcionalidades de unidifusión UDP de Azure. Para obtener mejores resultados dependiendo de la unidifusión UDP, recomendamos que el tamaño del clúster WebLogic se mantenga estático o con no más de 10 servidores administrados.
* **Oracle WebLogic Server espera que los puertos públicos y privados sean los mismos para el acceso T3 (por ejemplo, al usar Enterprise JavaBeans).** Considere un escenario de varios niveles, en el que una aplicación de capa de servicio (EJB) se ejecuta en un clúster de servidor Oracle WebLogic Server compuesto por dos o más máquinas virtuales, en una red virtual llamada *SLWLS*. El nivel de cliente está en una subred diferente de la misma red virtual, ejecutando un programa Java simple que intenta llamar a EJB en el nivel de servicio. Dado que es necesario equilibrar la carga de la capa de servicio, debe crearse un punto de conexión público con equilibrio de carga para las máquinas virtuales en el clúster de Oracle WebLogic Server. Si el puerto privado que especifique es diferente del puerto público (por ejemplo, 7006:7008), se producirá un error como el siguiente:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Esto se debe a que para cualquier acceso remoto T3, Oracle WebLogic Server espera que el puerto con equilibrador de carga y el puerto del servidor WebLogic administrado sean el mismo. En el caso anterior, el cliente tiene acceso al puerto 7006 (el puerto del equilibrador de carga) y el servidor administrado está escuchando en 7008 (el puerto privado). Esta restricción se aplica solo al acceso T3, no a HTTP.

   Para evitar este problema, use una de las siguientes soluciones:

  * Use los mismos números de puerto públicos y privados para los extremos de equilibrio de carga dedicados al acceso a T3.
  * Incluya el siguiente parámetro de JVM al iniciar Oracle WebLogic Server:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

Para información relacionada, consulte el artículo de KB **860340.1** en <https://support.oracle.com>.

* **Agrupación en clústeres dinámica y limitaciones de equilibrio de carga.** Supongamos que desea usar un clúster dinámico en Oracle WebLogic Server y exponerlo mediante un único punto de conexión con equilibrio de carga público en Azure. Esto puede hacerse siempre que use un número de puerto fijo para cada uno de los servidores administrados (no asignados dinámicamente a partir de un rango) y no inicie más servidores administrados que máquinas esté siguiendo el administrador. Es decir, que no haya más de un servidor administrado por máquina virtual. Si la configuración provoca que se inicien más servidores Oracle WebLogic que máquinas virtuales (es decir, donde varias instancias de Oracle WebLogic Server compartirán la misma máquina virtual), entonces no será posible para más de una de esas instancias de servidores de Oracle WebLogic Server establecer un enlace a un número de puerto determinado. Los demás de esa máquina virtual generan un error.

   Si configura el servidor de administración para asignar automáticamente números de puerto exclusivos para los servidores administrados, el equilibrio de carga no será posible porque Azure no admite la asignación de un único puerto público a varios puertos privados, como sería necesario para esta configuración.
* **Varias instancias de Oracle WebLogic Server en una máquina virtual.** En función de los requisitos de su implementación, puede considerar la opción de ejecutar varias instancias de Oracle WebLogic Server en la misma máquina virtual, si la máquina virtual es suficientemente grande. Por ejemplo, en un máquina virtual de tamaño medio que contiene dos núcleos, puede elegir ejecutar dos instancias de Oracle WebLogic Server. Sin embargo, tenga en cuenta que todavía se recomienda evitar introducir puntos únicos de error en la arquitectura, que sería el caso si usase una sola máquina virtual que ejecuta varias instancias de Oracle WebLogic Server. Usar al menos dos máquinas virtuales podría ser un mejor enfoque y, a continuación, cada máquina virtual puede ejecutar varias instancias de Oracle WebLogic Server. Cada instancia de Oracle WebLogic Server podría continuar formando parte del mismo clúster. No obstante, tenga en cuenta que actualmente no resulta posible usar Azure para efectuar el equilibrio de carga de los puntos de conexión que están expuestos por tales implementaciones de Oracle WebLogic Server dentro de la misma máquina virtual, porque el equilibrador de carga de Azure requiere que los servidores con equilibrio de carga se distribuyan entre máquinas virtuales únicas.

## <a name="oracle-jdk-virtual-machine-images"></a>Imágenes de máquina virtual de Oracle JDK
* **Actualizaciones más recientes de JDK 6 y 7.** Aunque se recomienda usar la última versión compatible pública de Java (actualmente Java 8), Azure también ofrece imágenes de JDK 6 y 7. Esto está pensado para las aplicaciones heredadas que aun no están preparadas para actualizarse a JDK 8. Mientras que es posible que las actualizaciones a imágenes de JDK anteriores ya no estén disponibles para el público general, dada la asociación de Microsoft con Oracle, las imágenes de JDK 6 y 7 ofrecidas por Azure están diseñadas para contener una actualización más reciente no pública que la ofrecida normalmente por Oracle para tan solo un grupo selecto de clientes compatibles con Oracle. Con el tiempo habrá disponibles nuevas versiones de las imágenes de JDK con versiones actualizadas de JDK 6 y 7.

   Tenga en cuenta que el JDK disponible en estas imágenes de JDK 6 y 7 y las máquinas virtuales e imágenes derivadas de estas solo se podrán usar dentro de Azure.
* **JDK de 64 bits.** Las imágenes de máquina virtual de Oracle WebLogic Server y las imágenes de máquina virtual de JDK de Oracle proporcionadas por Azure contienen las versiones de 64 bits de Windows Server y el JDK.

## <a name="next-steps"></a>Pasos siguientes
Ahora tiene una visión general de las soluciones actuales de Oracle basadas en imágenes de máquinas virtuales de Microsoft Azure. El siguiente paso es implementar su primera base de datos de Oracle Database en Azure.

> [!div class="nextstepaction"]
> [Creación de una base de datos de Oracle Database en Azure](oracle-database-quick-create.md)
