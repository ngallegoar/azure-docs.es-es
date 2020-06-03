---
title: 'Azure Lab Services: guía del administrador | Microsoft Docs'
description: Esta guía ayuda a los administradores que crean y administran cuentas de laboratorio mediante Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2020
ms.author: spelluru
ms.openlocfilehash: ce9f8ee592c1fb2f7ac98339bbd14ce57440bc1a
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83815713"
---
# <a name="azure-lab-services---administrator-guide"></a>Azure Lab Services: guía del administrador
Los administradores de tecnología de la información (TI) que administran los recursos en la nube de una universidad suelen ser también responsables de configurar la cuenta de laboratorio en su institución. Una vez configurada una cuenta de laboratorio, los administradores o educadores crean laboratorios educativos que están incluidos en la cuenta de laboratorio. En este artículo se proporciona información general de alto nivel sobre los recursos de Azure relacionados e instrucciones para crearlos.

![Vista de alto nivel de los recursos de Azure de una cuenta de laboratorio](../media/administrator-guide/high-level-view.png)

- Los laboratorios educativos se hospedan en una suscripción de Azure propiedad de Azure Lab Services.
- Las cuentas de laboratorio, la galería de imágenes compartidas y las versiones de imágenes se hospedan en la suscripción.
- La cuenta de laboratorio y la galería de imágenes compartidas pueden residir en el mismo grupo de recursos. En este diagrama, están en distintos grupos de recursos. 

## <a name="subscription"></a>Subscription
Su universidad tiene una o más suscripciones de Azure. Las suscripciones se usan para administrar la facturación y la seguridad de todos los recursos y servicios de Azure que se usan dentro de esta plataforma, incluidas las cuentas de laboratorio.

La relación entre una cuenta de laboratorio y su suscripción es importante por los siguientes motivos:

- La facturación se notifica mediante la suscripción que contiene la cuenta de laboratorio.
- Puede conceder a los usuarios del inquilino de Azure Active Directory (AD) de la suscripción acceso a Azure Lab Services. Puede agregar un usuario como propietario o colaborador de la cuenta de laboratorio, creador del laboratorio educativo o propietario del laboratorio educativo.

Los laboratorios educativos y sus máquinas virtuales (VM) se administran y hospedan en una suscripción propiedad de Azure Lab Services.

## <a name="resource-group"></a>Resource group
Una suscripción contiene uno o varios grupos de recursos. Los grupos de recursos se usan para crear agrupaciones lógicas de recursos de Azure que se emplean juntos dentro de la misma solución.  

Al crear una cuenta de laboratorio, debe configurar el grupo de recursos que la contiene. 

También se requiere un grupo de recursos para crear una [galería de imágenes compartidas](#shared-image-gallery). Puede optar por colocar su cuenta de laboratorio y la galería de imágenes compartidas en dos grupos de recursos distintos, lo que es habitual si planea compartir la galería de imágenes entre diferentes soluciones. También, puede elegir colocarlas en el mismo grupo de recursos.

Al crear una cuenta de laboratorio, puede crear y adjuntar automáticamente una galería de imágenes compartidas al mismo tiempo.  Esta opción da como resultado la creación de la cuenta de laboratorio y la galería de imágenes compartidas en grupos de recursos independientes. Observará este comportamiento cuando siga los pasos descritos en este tutorial: [Configuración de la galería de imágenes compartidas en el momento de crear la cuenta de laboratorio](how-to-attach-detach-shared-image-gallery.md#configure-at-the-time-of-lab-account-creation). La imagen que se encuentra en la parte superior de este artículo también usa esta configuración. 

Se recomienda dedicar tiempo por adelantado a planear la estructura de los grupos de recursos ya que, una vez creado, *no* es posible cambiar el grupo de recursos de una cuenta de laboratorio o de la galería de imágenes compartidas. Si necesita cambiar el grupo de recursos de estos recursos, deberá eliminar y volver a crear la cuenta de laboratorio o la galería de imágenes compartidas.

## <a name="lab-account"></a>Cuenta de laboratorio

Una cuenta de laboratorio funciona como contenedor de uno o varios laboratorios educativos. Al empezar a trabajar con Azure Lab Services, es habitual tener solo una cuenta de laboratorio. A medida que se amplía el uso del laboratorio, puede optar por crear más adelante otras cuentas de laboratorio.

En la lista siguiente se resaltan escenarios en los que puede ser beneficioso tener más de una cuenta de laboratorio:

- **Administración de distintos requisitos de directivas entre laboratorios educativos**

    Cuando se configura una cuenta de laboratorio, se establecen directivas que se aplican a *todos* los laboratorios educativos de la cuenta de laboratorio, por ejemplo:
    - La red virtual de Azure con recursos compartidos a los que puede acceder el laboratorio educativo. Por ejemplo, puede tener un conjunto de laboratorios educativos que necesiten acceso a un conjunto de datos compartido dentro de una red virtual.
    - Las imágenes de máquina virtual (VM) que los laboratorios educativos pueden usar para crear máquinas virtuales. Por ejemplo, puede tener un conjunto de laboratorios educativos que necesiten acceso a la imagen de Marketplace de [Data Science VM para Linux](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804).

    Si tiene laboratorios educativos con requisitos de directivas únicos entre sí, puede ser beneficioso crear cuentas de laboratorio independientes para administrar estos laboratorios por separado.

- **Separación de presupuestos por cuenta de laboratorio**
  
    En lugar de presentar todos los costos de laboratorios educativos en una sola cuenta de laboratorio, puede que deba tener un presupuesto separado más claramente. Por ejemplo, puede crear cuentas de laboratorio para el departamento de matemáticas de la universidad, el departamento de informática, etc., para separar el presupuesto entre departamentos.  Con [Azure Cost Management](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview), puede ver luego el costo de cada cuenta de laboratorio.

- **Aislamiento de los laboratorios piloto de los laboratorios activos o de producción**
  
    Puede que haya casos en los que quiera realizar cambios en la directiva piloto de una cuenta de laboratorio sin que estos cambios afecten a los laboratorios activos o de producción. En este tipo de escenario, la creación de una cuenta de laboratorio independiente con fines piloto permite aislar los cambios. 

## <a name="classroom-lab"></a>Laboratorio educativo

Un laboratorio educativo contiene máquinas virtuales (VM) que se asignan a un único alumno.  En general, puede esperar lo siguiente:

- Tener un laboratorio educativo para cada clase.
- Cree un nuevo conjunto de laboratorios educativos cada semestre (o para cada período de tiempo en el que se ofrece la clase). Normalmente, para clases en las que se van a usar las mismas imágenes, se debe usar una [galería de imágenes compartidas](#shared-image-gallery) para reutilizar imágenes entre laboratorios y semestres.

Tenga en cuenta los siguientes puntos a la hora de determinar cómo estructurar los laboratorios educativos:

- **Todas las máquinas virtuales de un laboratorio educativo se implementan con la misma imagen con la que se han publicado**

    Como resultado, si tiene una clase que requiere que se publiquen distintas imágenes de laboratorio al mismo tiempo, deben crearse laboratorios de clase distintos para cada una.
  
- **La cuota de uso se establece en el nivel de laboratorio y se aplica a todos los usuarios del laboratorio**

    Para establecer cuotas diferentes para los usuarios, debe crear distintos laboratorios educativos. Sin embargo, es posible agregar más horas a un usuario específico después de haber establecido la cuota.
  
- **La programación de inicio o apagado se establece en el nivel de laboratorio y se aplica a todas las máquinas virtuales del laboratorio**

    De forma similar al punto anterior, si necesita establecer diferentes programaciones para los usuarios, debe crear distintos laboratorios educativos.

De forma predeterminada, cada laboratorio educativo tendrá su propia red virtual.  Si tiene habilitado el emparejamiento de VNET, cada laboratorio educativo tendrá su propia subred emparejada a la red virtual especificada.

## <a name="shared-image-gallery"></a>Galería de imágenes compartidas

Una galería de imágenes compartidas se asocia a una cuenta de laboratorio y sirve como repositorio central para almacenar imágenes. Una imagen se guarda en la galería cuando un educador elige exportarla desde la máquina virtual (VM) de plantilla de un laboratorio educativo. Cada vez que un educador realiza cambios en la máquina virtual de plantilla y los exporta, se guardan nuevas versiones de la imagen y se mantienen las versiones anteriores.

Los instructores pueden publicar una versión de una imagen desde la galería de imágenes compartidas cuando crean un laboratorio educativo. Aunque la galería almacena varias versiones de una imagen, los educadores solo pueden seleccionar la versión más reciente durante la creación del laboratorio.

La galería de imágenes compartidas es un recurso opcional que es posible que no necesite de inmediato al empezar con solo algunos laboratorios educativos. Sin embargo, su uso tiene muchas ventajas que le resultarán útiles a la hora de tener más laboratorios educativos:

- **Permite guardar y administrar versiones de una imagen de máquina virtual de plantilla**

    Resulta útil cuando se crea una imagen personalizada o se realizan cambios (software, configuración, etc.) en una imagen de la galería pública de Marketplace.  Por ejemplo, es habitual que los educadores requieran la instalación de software o herramientas diferentes. En lugar de solicitar a los alumnos que instalen manualmente estos requisitos previos por su cuenta, se pueden exportar diferentes versiones de la imagen de máquina virtual de plantilla en una galería de imágenes compartidas. Estas versiones de imagen se pueden usar luego para crear otros laboratorios educativos.
- **Permite el uso compartido o la reutilización de imágenes de máquina virtual de plantilla entre laboratorios educativos**

    Puede guardar y reutilizar una imagen para que no tenga que configurar la imagen desde cero cada vez que cree un nuevo laboratorio educativo. Por ejemplo, si se ofrecen varias clases que necesitan la misma imagen, esta imagen solo debe crearse una vez y exportarse en la galería de imágenes compartidas para que se pueda compartir entre los laboratorios educativos.
- **Garantiza la disponibilidad de imágenes mediante la replicación**

    Al guardar la imagen en la galería de imágenes compartidas de un laboratorio educativo, esta se replica automáticamente en otras [regiones de la misma geografía](https://azure.microsoft.com/global-infrastructure/regions/). En el caso de que se produzca una interrupción en una región, la publicación de la imagen en el laboratorio educativo no se ve afectada, ya que se puede usar una réplica de imagen de otra región.  La publicación de máquinas virtuales desde varias réplicas también puede ayudar al rendimiento.

Para agrupar imágenes compartidas de forma lógica, tiene un par de opciones:

- Crear varias galerías de imágenes compartidas. Cada cuenta de laboratorio solo se puede conectar a una galería de imágenes compartidas, por lo que esta opción también requerirá la creación de varias cuentas de laboratorio.
- También, puede usar una sola galería de imágenes compartidas entre varias cuentas de laboratorio. En este caso, cada cuenta de laboratorio puede habilitar solo las imágenes aplicables a los laboratorios educativos que contenga.

## <a name="naming"></a>Nomenclatura

Cuando empiece a trabajar con Azure Lab Services, se recomienda establecer convenciones de nomenclatura para los grupos de recursos, las cuentas de laboratorio, los laboratorios educativos y la galería de imágenes compartidas. Aunque las convenciones de nomenclatura que establezca serán únicas para las necesidades de su organización, en la tabla siguiente se describen las directrices generales.

| Tipo de recurso | Role | Patrón sugerido | Ejemplos |
| ------------- | ---- | ----------------- | -------- | 
| Resource group | Contiene una o varias cuentas de laboratorio y una o varias galerías de imágenes compartidas. | \<nombre corto de organización\>-\<entorno\>-rg<ul><li>**Nombre corto de la organización**: identifica el nombre de la organización que admite el grupo de recursos.</li><li>**Entorno**: identifica el entorno del recurso, por ejemplo, piloto o producción.</li><li>**Rg**: significa el tipo de recurso: grupo de recursos (del inglés resource group).</li></ul> | contosouniversitylabs-rg<br/>contosouniversitylabs-pilot-rg<br/>contosouniversitylabs-prod-rg |
| Cuenta de laboratorio | Contiene uno o varios laboratorios. | \<nombre corto de laboratorio\>-\<entorno\>-la<ul><li>**Nombre corto de la organización**: identifica el nombre de la organización que admite el grupo de recursos.</li><li>**Entorno**: identifica el entorno del recurso, por ejemplo, piloto o producción.</li><li>**La**: significa el tipo de recurso: cuenta de laboratorio (del inglés lab account).</li></ul> | contosouniversitylabs-la<br/>mathdeptlabs-la<br/>sciencedeptlabs-pilot-la<br/>sciencedeptlabs-prod-la |
| Laboratorio educativo | Contiene una o varias máquinas virtuales. |\<nombre de clase\>-\<período de tiempo\>-\<identificador del educador\><ul><li>**Nombre de clase**: identifica el nombre de la clase que admite el laboratorio.</li><li>**Período de tiempo**: identifica el período de tiempo en el que se ofrece la clase.</li>**Identificador del educador**: identifica el educador que posee el laboratorio.</li></ul> | CS1234-fall2019-johndoe<br/>CS1234-spring2019-johndoe |
| Galería de imágenes compartidas | Contiene una o más versiones de imágenes de máquina virtual. | \<nombre corto de la organización\>galería | contosouniversitylabsgallery |

Para más información sobre la asignación de nombres a otros recursos de Azure, consulte las [convenciones de nomenclatura para recursos de Azure](/azure/architecture/best-practices/naming-conventions).

## <a name="regionslocations"></a>Regiones o ubicaciones

Al configurar los recursos de Azure Lab Services, se le pedirá que proporcione una región (o ubicación) del centro de datos que hospedará el recurso. A continuación se muestran más detalles sobre cómo la región afecta a cada uno de los recursos implicados en la configuración de un laboratorio.

### <a name="resource-group"></a>Resource group

La región especifica el centro de datos donde se almacena la información sobre el grupo de recursos. Los recursos de Azure contenidos en el grupo de recursos pueden estar en otras regiones que no son la principal.

### <a name="lab-account"></a>Cuenta de laboratorio

La ubicación de la cuenta de un laboratorio indica la región en que existe este recurso.  

### <a name="classroom-lab"></a>Laboratorio educativo

La ubicación en la que existe un laboratorio educativo varía en función de estos factores:

  - **La cuenta de laboratorio está emparejada con una red virtual (VNet)**
  
    Una cuenta de laboratorio se puede [emparejar con una red virtual](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network) cuando se encuentran en la misma región.  Cuando una cuenta de laboratorio se empareja con una red virtual, los laboratorios educativos se crean automáticamente en la misma región que la cuenta de laboratorio y la red virtual.

    > [!NOTE]
    > Cuando una cuenta de laboratorio se empareja con una red virtual, la configuración **Permitir al creador del laboratorio seleccionar la ubicación correspondiente** está deshabilitada. Encontrará información adicional sobre esta opción en el artículo: [Selección de la ubicación del laboratorio por parte del creador de este en Azure Lab Services](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location).
    
  - **No hay ninguna red virtual emparejada ***y*** los creadores del laboratorio no pueden elegir la ubicación del laboratorio.**
  
    Cuando **no** hay ninguna red virtual emparejada con la cuenta de laboratorio *y* los [creadores del laboratorio **no** pueden elegir la ubicación del laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location), los laboratorios educativos se crean automáticamente en una región que tiene capacidad de VM disponible.  En concreto, Azure Lab Services busca disponibilidad en [regiones que se encuentran en la misma geografía que la cuenta de laboratorio](https://azure.microsoft.com/global-infrastructure/regions).

  - **No hay ninguna red virtual emparejada ***y*** los creadores del laboratorio pueden elegir la ubicación del laboratorio**
       
    Cuando **no** hay ninguna red virtual emparejada y [los creadores del laboratorio pueden elegir la ubicación del laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location), el creador del laboratorio puede seleccionar las ubicaciones según la capacidad disponible.

> [!NOTE]
> Para asegurarse de que hay suficiente capacidad de máquina virtual para una región, es importante que primero solicite capacidad mediante la cuenta de laboratorio o al crear el laboratorio.

Una regla general es establecer la región de un recurso en la más próxima a sus usuarios. En el caso de los laboratorios educativos, esto significa crear el laboratorio lo más cerca posible de sus alumnos. En el caso de los cursos en línea donde los alumnos están ubicados en cualquier parte del mundo, deberá usar su mejor criterio para crear un laboratorio educativo que esté ubicado en un lugar central. También puede dividir una clase en varios laboratorios educativos según la región de los alumnos.

### <a name="shared-image-gallery"></a>Galería de imágenes compartidas

La región indica la región de origen en la que se almacena la primera versión de la imagen antes de replicarse automáticamente en las regiones de destino.

## <a name="vm-sizing"></a>Tamaño de máquina virtual

Cuando los administradores o creadores de laboratorios crean un laboratorio educativo, pueden elegir entre los siguientes tamaños de máquina virtual en función de las necesidades de su aula. Recuerde que los tamaños de proceso que haya disponibles dependerán de la región en la que se encuentre la cuenta de laboratorio:

| Size | Especificaciones | Serie | Sugerencia de uso |
| ---- | ----- | ------ | ------------- |
| Pequeña| <ul><li>2 núcleos</li><li>3,5 GB de RAM</li> | [Standard_A2_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Este tamaño es el más adecuado para la línea de comandos, apertura de navegador web, servidores web de poco tráfico, bases de datos pequeñas o medianas. |
| Media | <ul><li>4 núcleos</li><li>7 GB de RAM</li> | [Standard_A4_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Este tamaño es el más adecuado para bases de datos relacionales, análisis y almacenamiento en caché en memoria. |
| Mediano (virtualización anidada) | <ul><li>4 núcleos</li><li>16 GB de RAM</li></ul> | [Standard_D4s_v3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#dsv3-series) | Este tamaño es el más adecuado para bases de datos relacionales, análisis y almacenamiento en caché en memoria.
| grande | <ul><li>8 núcleos</li><li>16 GB de RAM</li></ul>  | [Standard_A8_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series) | Este tamaño es ideal para aplicaciones que necesitan CPU más rápidas, un mejor rendimiento de los discos locales, bases de datos grandes y memorias caché grandes.  Este tamaño admite la virtualización anidada. |
| Grande (virtualización anidada) | <ul><li>8 núcleos</li><li>16 GB de RAM</li></ul>  | [Standard_A8_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series) | Este tamaño es ideal para aplicaciones que necesitan CPU más rápidas, un mejor rendimiento de los discos locales, bases de datos grandes y memorias caché grandes. |
| GPU pequeña (visualización) | <ul><li>6 núcleos</li><li>56 GB de RAM</li>  | [Standard_NV6](https://docs.microsoft.com/azure/virtual-machines/nv-series) | Este tamaño es más adecuado para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX. |
| GPU pequeña (proceso) | <ul><li>6 núcleos</li><li>56 GB de RAM</li></ul>  | [Standard_NC6](https://docs.microsoft.com/azure/virtual-machines/nc-series) |Este tamaño es más adecuado para aplicaciones de proceso intensivo, como la inteligencia artificial y el aprendizaje profundo. |
| GPU mediana (visualización) | <ul><li>12 núcleos</li><li>112 GB de RAM</li></ul>  | [Standard_NV12](https://docs.microsoft.com/azure/virtual-machines/nv-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Este tamaño es más adecuado para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX. |

## <a name="manage-identity"></a>Administración de identidades

Con el [control de acceso basado en roles de Azure](https://docs.microsoft.com/azure/role-based-access-control/overview), se pueden asignar los siguientes roles para conceder acceso a las cuentas de laboratorio y a los laboratorios educativos:

- **Propietario de la cuenta del laboratorio**

    El administrador que crea la cuenta de laboratorio se agrega automáticamente al rol **Propietario** de la cuenta de laboratorio.  Un administrador con el rol **Propietario** asignado puede:
     - Cambiar la configuración de la cuenta de laboratorio.
     - Conceder a otros administradores acceso a la cuenta de laboratorio como propietarios o colaboradores.
     - Ofrecer a los educadores acceso a laboratorios educativos como creadores, propietarios o colaboradores.
     - Crear y administrar todos los laboratorios educativos dentro de la cuenta de laboratorio.

- **Colaborador de la cuenta de laboratorio**

    Un administrador con el rol **Colaborador** asignado puede:
    - Cambiar la configuración de la cuenta de laboratorio.
    - Crear y administrar todos los laboratorios educativos dentro de la cuenta de laboratorio.

    Sin embargo, *no puede* conceder a otros usuarios acceso a cuentas de laboratorio o laboratorios educativos.

- **Creador del laboratorio educativo**

    Para crear laboratorios educativos dentro de una cuenta de laboratorio, un educador debe ser miembro del rol **Creador de laboratorio**.  Cuando un educador crea un laboratorio educativo, se agrega automáticamente como propietario del laboratorio.  Consulte el tutorial sobre cómo [agregar un usuario al rol **Creador del laboratorio**](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role). 

- **Propietario o colaborador de laboratorio educativo**
  
    Un educador puede ver y cambiar la configuración de un laboratorio educativo si es miembro del rol **Propietario** o **Colaborador** de un laboratorio, además de miembro del rol **Lector** de la cuenta de laboratorio.

    Una diferencia clave entre los roles **Propietario** y **Colaborador** de un laboratorio es que un colaborador *no puede* conceder a otros usuarios acceso para administrar el laboratorio; solo los propietarios pueden conceder acceso a otros usuarios para hacerlo.

    Además, un educador *no puede* crear nuevos laboratorios educativos, a menos que también sea miembro del rol **Creador de laboratorio**.

- **Galería de imágenes compartidas**

    Al asociar una galería de imágenes compartidas a una cuenta de laboratorio, se concede acceso automáticamente a los propietarios y colaboradores de la cuenta de laboratorio y a los creadores, propietarios y colaboradores del laboratorio para ver y guardar imágenes en la galería.

Estas son algunas sugerencias que le ayudarán a asignar roles:
   - Normalmente, solo los administradores deben ser miembros de los roles **Propietario** o **Colaborador** de una cuenta de laboratorio; puede haber más de un propietario o colaborador.
   - Para proporcionar a un educador la capacidad de crear nuevos laboratorios educativos y administrar los laboratorios que cree, solo es necesario asignar acceso al rol **Creador de laboratorio**.
   - Para proporcionar a un educador la capacidad de administrar determinados laboratorios educativos, pero *no* la capacidad de crear nuevos laboratorios, debe asignar acceso al rol **Propietario** o **Colaborador** para cada uno de los laboratorios educativos que administrará.  Por ejemplo, puede que desee permitir que un profesor titular y un profesor ayudante sean propietarios conjuntamente de un laboratorio educativo.  Consulte la guía sobre cómo [agregar un usuario como propietario de un laboratorio educativo](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-add-user-lab-owner).

## <a name="pricing"></a>Precios

### <a name="azure-lab-services"></a>Azure Lab Services

Los precios de Azure Lab Services se describen en el artículo siguiente: [Precios de Azure Lab Services](https://azure.microsoft.com/pricing/details/lab-services/).

También debe tener en cuenta los precios de la galería de imágenes compartidas si tiene previsto usarla para almacenar y administrar versiones de imágenes. 

### <a name="shared-image-gallery"></a>Galería de imágenes compartidas

La creación de una galería de imágenes compartidas y la asociación a su cuenta de laboratorio es gratuita. No se generan gastos hasta que se guarda una versión de la imagen en la galería. Por lo general, el precio por usar una galería de imágenes compartidas es bastante insignificante, pero es importante comprender cómo se calcula, ya que no se incluye en los precios de Azure Lab Services.  

#### <a name="storage-charges"></a>Cargos de almacenamiento

Para almacenar versiones de imágenes, una galería de imágenes compartidas usa discos estándar administrados por HDD. El tamaño del disco administrado por HDD que se usa depende del tamaño de la versión de la imagen que se almacena. Consulte el siguiente artículo para ver los precios: [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).

#### <a name="replication-and-network-egress-charges"></a>Cargos por salida de replicación y red

Cuando se guarda una versión de una imagen mediante la máquina virtual de plantilla (VM) de un laboratorio educativo, Azure Lab Services la almacena primero en una región de origen y, luego, replica automáticamente la versión de la imagen de origen en una o varias regiones de destino. Es importante tener en cuenta que Azure Lab Services replica automáticamente la versión de la imagen de origen en todas las [regiones de destino dentro de la geografía](https://azure.microsoft.com/global-infrastructure/regions/) donde se encuentra el laboratorio educativo. Por ejemplo, si el laboratorio educativo está en la geografía de EE. UU., se replica una versión de la imagen en cada una de las ocho regiones que existen en EE. UU.

Un cargo por salida de red se produce cuando una versión de una imagen se replica de la región de origen a regiones de destino adicionales. La cantidad que se cobra se basa en el tamaño de la versión de la imagen cuando los datos de la imagen se transfieren inicialmente desde la región de origen.  Para información detallada sobre precios, consulte el artículo siguiente: [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).

En las [soluciones educativas](https://www.microsoft.com/licensing/licensing-programs/licensing-for-industries?rtc=1&activetab=licensing-for-industries-pivot:primaryr3) los clientes pueden estar exentos de pagar cargos por salida. Hable con el administrador de cuentas para más información.  Para más información, **consulte la sección de preguntas frecuentes** en el documento vinculado, en concreto, la pregunta sobre qué programas de transferencia de datos existen para los clientes académicos y cómo se puede acceder a ellos.

#### <a name="pricing-example"></a>Ejemplo de precios

Para resumir los precios descritos anteriormente, echemos un vistazo a un ejemplo de cómo guardar la imagen de la máquina virtual de plantilla en la galería de imágenes compartidas. Considere los casos siguientes:

- Tiene una imagen de máquina virtual personalizada.
- Va a guardar dos versiones de la imagen.
- El laboratorio está en Estados Unidos, que tiene ocho regiones en total.
- Cada versión de la imagen tiene un tamaño de 32 GB; como resultado, el precio del disco administrado por HDD es de 1,54 USD al mes.

El costo total se calcula de la manera siguiente:

Número de imágenes × número de versiones × número de réplicas × precio del disco administrado

En este ejemplo, el costo es:

1 imagen personalizada (32 GB) x 2 versiones x 8 regiones de EE. UU. x 1,54 USD = 24,64 USD

#### <a name="cost-management"></a>Administración de costos

Es importante que el administrador de la cuenta de laboratorio administre los costos mediante la eliminación rutinaria de las versiones de imágenes innecesarias de la galería. 

No debe eliminar la replicación a regiones específicas para reducir los costos (esta opción existe en la galería de imágenes compartidas). Los cambios en la replicación pueden tener efectos negativos sobre la capacidad de Azure Lab Services para publicar máquinas virtuales a partir de imágenes guardadas en una galería de imágenes compartidas.

## <a name="next-steps"></a>Pasos siguientes

Consulte el tutorial para obtener instrucciones paso a paso sobre cómo crear una cuenta de laboratorio y un laboratorio: [Guía de configuración](tutorial-setup-lab-account.md)
