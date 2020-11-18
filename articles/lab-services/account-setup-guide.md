---
title: Guía de configuración acelerada de una cuenta de laboratorio para Azure Lab Services
description: Esta guía ayuda a los administradores a configurar rápidamente una cuenta de laboratorio para usarla en su centro educativo.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: aa3e7b586546b3d87f5c6029b284eeb1402ed171
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659869"
---
# <a name="lab-account-setup-guide"></a>Guía de configuración de una cuenta de laboratorio
Para configurar el entorno de Azure Lab Services, los administradores deben configurar primero una **cuenta de laboratorio** en su suscripción de Azure. Una cuenta de laboratorio es un contenedor para los laboratorios y solo lleva unos minutos configurarla.

En esta guía se incluyen tres secciones:
-  La primera sección se centra en los requisitos previos que deben completarse *antes* de configurar la cuenta de laboratorio.
-  La segunda sección proporciona instrucciones sobre cómo planear la configuración de la cuenta de laboratorio.
-  La tercera sección proporciona instrucciones paso a paso para configurar una cuenta de laboratorio.

## <a name="prerequisites-for-setting-up-your-lab-account"></a>Requisitos previos para configurar la cuenta de laboratorio
En esta sección se describen los requisitos previos que debe completar antes de poder configurar una cuenta de laboratorio.

### <a name="obtain-an-azure-subscription"></a>Obtención de una suscripción a Azure
Para crear una cuenta de laboratorio necesita obtener acceso a una suscripción de Azure configurada para su escuela. Recuerde que es posible que su escuela tenga una o más suscripciones. Las suscripciones se usan para administrar la facturación y la seguridad de todos los recursos y servicios de Azure, incluidas las cuentas de laboratorio.  Normalmente, el departamento de TI se encarga de administrar las suscripciones de Azure.  Para obtener más información, consulte el siguiente tema:
 - [Guía del administrador: suscripción](./administrator-guide.md#subscription)

### <a name="estimate-the-number-of-vms-and-vm-sizes-that-you-need"></a>Calcule el número de VM y los tamaños de las mismas que necesita.
Deberá calcular el número de máquinas virtuales (VM) y los [tamaños de estas](./administrator-guide.md#vm-sizing) que necesite su escuela.  Lea la siguiente entrada de blog para obtener instrucciones sobre cómo estructurar los laboratorios y las imágenes.  Esta entrada de blog también le ayudará a decidir el número y tamaño de las VM que necesitará:
- [Traslado de un laboratorio físico a Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)

Asimismo, consulte este artículo en el que se detallan instrucciones adicionales sobre cómo estructurar laboratorios:
- [Guía del administrador: laboratorio](./administrator-guide.md)

### <a name="understand-subscription-vm-limits-and-regional-vm-capacity"></a>Descripción de los límites de VM de suscripción y la capacidad de la VM regional
Una vez que tenga una estimación del número y los tamaños de las de VM de los laboratorios, debe hacer lo siguiente:

- Asegúrese de que el límite de capacidad de su suscripción de Azure permite el número y tamaño de VM que va a usar en los laboratorios.

- Cree su cuenta de laboratorio en una región que tenga suficiente capacidad de VM disponible.

Para obtener más información, lea la siguiente entrada de blog: [Límites de suscripción de VM y capacidad regional](https://techcommunity.microsoft.com/t5/azure-lab-services/vm-subscription-limits-and-regional-capacity/ba-p/1845553).

### <a name="decide-how-many-lab-accounts-to-create"></a>Decisión del número de cuentas de laboratorio que se van a crear

Para empezar a trabajar rápidamente, cree una cuenta de laboratorio única dentro de su propio grupo de recursos.  Más adelante podrá crear más cuentas de laboratorio y grupos de recursos según sea necesario. Por ejemplo, puede tener una cuenta de laboratorio y un grupo de recursos por departamento, para poder separar claramente los costos.  Lea los artículos siguientes para obtener más información acerca de las cuentas de laboratorio, los grupos de recursos y la separación de costos:
- [Guía del administrador: grupo de recursos](./administrator-guide.md#resource-group)
- [Guía del administrador: cuenta de laboratorio](./administrator-guide.md#lab-account) 
- [Administración de costos de Azure Lab Services](./cost-management-guide.md)

## <a name="planning-your-lab-accounts-settings"></a>Planeación de la configuración de la cuenta de laboratorio

Para planear la configuración de la cuenta de laboratorio, debe tener en cuenta las siguientes preguntas.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>¿Quiénes deben ser propietarios y colaboradores de la cuenta de laboratorio?

   Los administradores de IT de la escuela suelen ser propietarios y colaboradores de una cuenta de laboratorio. Son los responsables de administrar las directivas que se aplican a todos los laboratorios incluidos en la cuenta de laboratorio. La persona que crea la cuenta de laboratorio se convierte automáticamente en propietario. Puede agregar propietarios y colaboradores adicionales desde el inquilino de Azure Active Directory (Azure AD) asociado a la suscripción. Para obtener más información sobre los roles de colaborador y propietario de la cuenta de laboratorio, lea lo siguiente:
   -  [Guía del administrador: administración de identidad](./administrator-guide.md#manage-identity).

   [!INCLUDE [Select a tenant](./includes/multi-tenant-support.md)]

   Los usuarios del laboratorio solo ven una lista con las máquinas virtuales que tienen acceso a diferentes inquilinos de Azure Lab Services.

### <a name="who-will-be-allowed-to-create-labs"></a>¿A quién se le permitirá crear laboratorios?

   Puede decidir que sus miembros de TI y los profesores sean los que creen laboratorios. Cuando un usuario crea un laboratorio, se asigna automáticamente como propietario del laboratorio.  Para crear laboratorios, estos usuarios (normalmente desde el inquilino de Azure AD asociado a la suscripción) se asignan al rol de creador de laboratorio dentro de la cuenta de laboratorio.  Para obtener más información sobre el rol de creador de laboratorio, lea lo siguiente:
   -  [Guía del administrador: administración de identidad](./administrator-guide.md#manage-identity).

### <a name="who-will-be-allowed-to-own-and-manage-labs"></a>¿A quién se le permitirá poseer y administrar laboratorios?

   También puede hacer que los miembros de TI y los profesores sean los propietarios o administren los laboratorios *sin* tener que proporcionarles la capacidad de crearlos.  En este caso, a los usuarios del inquilino de Azure AD de la suscripción se les asigna el rol de propietario o colaborador de los laboratorios existentes.  Para obtener más información sobre los roles de colaborador y propietario de un laboratorio, lea lo siguiente:
   - [Guía del administrador: administración de identidad](./administrator-guide.md#manage-identity).

### <a name="do-you-want-to-save-images-that-can-be-shared-across-labs"></a>¿Quiere proporcionar guardar imágenes que se puedan compartir entre varios laboratorios?
Shared Image Gallery es un repositorio que se puede usar para guardar y compartir imágenes. En el caso de las clases que necesiten la misma imagen, los creadores de laboratorio pueden crearla y, a continuación, exportarla a Shared Image Gallery.  Una vez que se exportan imágenes a Shared Image Gallery, se pueden usar para crear nuevos laboratorios.

Asimismo, puede crear las imágenes en un entorno físico y, a continuación, importarlas en Shared Image Gallery.  Para obtener información más detallada sobre este proceso, consulte la siguiente entrada de blog: 
- [Importación de una imagen compartida en Shared Image Gallery](https://techcommunity.microsoft.com/t5/azure-lab-services/import-custom-image-to-shared-image-gallery/ba-p/1777353)

Si decide que necesita usar una instancia de Shared Image Gallery, deberá crear o adjuntar una a su cuenta de laboratorio. Igualmente, puede posponer esta decisión, ya que se puede adjuntar a una cuenta de laboratorio en cualquier momento.  Para obtener más información sobre Shared Image Gallery, lea lo siguiente:
- [Guía del administrador: Shared Image Gallery](./administrator-guide.md#shared-image-gallery)
- [Guía del administrador: precio de Shared Image Gallery](./administrator-guide.md#shared-image-gallery-2)

### <a name="which-images-in-azure-marketplace-will-your-labs-use"></a>¿Qué imágenes de Azure Marketplace van a usar los laboratorios?
Azure Marketplace proporciona cientos de imágenes que puede habilitar para que los creadores de laboratorios puedan usar dichas imágenes para crear su laboratorio. Algunas imágenes pueden incluir ya todo lo que necesita un laboratorio. En otros casos, una imagen sirve como punto de partida, que el creador del laboratorio puede personalizar mediante la instalación de aplicaciones o herramientas adicionales.

Si no sabe qué imágenes necesita, puede volver más adelante para habilitarlas. Además, la mejor manera de ver qué imágenes están disponibles es crear primero una cuenta de laboratorio. Esto le proporciona acceso para que pueda revisar la lista de imágenes disponibles y su contenido.  Para obtener más información sobre las imágenes de Marketplace, lea lo siguiente:
- [Especificación de las imágenes de Marketplace disponibles para los creadores de laboratorios](./specify-marketplace-images.md)
  
### <a name="do-the-labs-vms-need-to-have-access-to-other-azure-or-on-premises-resources"></a>¿En necesario que las VM del laboratorio tengan acceso a otros recursos de Azure o locales?
Al configurar una cuenta de laboratorio, también tiene la opción de emparejarla con una red virtual (VNet).  Tenga en cuenta que la red virtual y la cuenta de laboratorio deben encontrarse en la misma región.  Para decidir si necesita emparejar la cuenta con una red virtual, tenga en cuenta los siguientes escenarios:

- **Acceso a un servidor de licencias**
  
   Cuando use imágenes de Azure Marketplace, el costo de la licencia de sistema operativo se incluye en los precios de los servicios de laboratorio. Por lo tanto, no es necesario proporcionar licencias para el propio sistema operativo. Sin embargo, para el software y las aplicaciones adicionales que se instalen, tendrá que proporcionar una licencia según corresponda.  Para obtener acceso a un servidor de licencias:
   - Puede optar por conectarse a un servidor de licencias local.  La conexión a un servidor de licencias local requiere que realice una instalación adicional.
   - Otra opción más rápida de configurar, es crear un servidor de licencias que se hospede en una VM de Azure.  La VM de Azure se encuentra en una red virtual que se empareja con su cuenta de laboratorio.

- **Acceso a otros recursos locales como un recurso compartido de archivos o una base de datos**

   Debe crear una red virtual para proporcionar acceso a los recursos locales, normalmente mediante una puerta de enlace de red virtual de sitio a sitio. Tenga que cuenta que la configuración de este tipo de entorno le llevará más tiempo.

- **Acceso a otros recursos de Azure que se encuentran fuera de la red virtual**

   Si necesita obtener acceso a recursos de Azure que *no* están protegidos en una red virtual, puede acceder a ellos a través de la red pública de Internet, sin tener que realizar ningún emparejamiento.

Para obtener más información, sobre las redes virtuales, lea lo siguiente:
- [Aspectos básicos de la arquitectura: Virtual Network](./classroom-labs-fundamentals.md#virtual-network)
- [Cómo conectarse a una red virtual](./how-to-connect-peer-virtual-network.md)
- [Creación de un laboratorio con un recurso compartido en Azure Lab Services](./how-to-create-a-lab-with-shared-resource.md)

## <a name="set-up-your-lab-account"></a>Configuración de la cuenta de laboratorio

Una vez finalizada la planeación, ya está listo para configurar la cuenta de laboratorio.  Estos mismos pasos son aplicables a la configuración de un laboratorio de [Azure Lab Services con Teams](./lab-services-within-teams-overview.md).

1. **Cree la cuenta de laboratorio.** Consulte el tutorial sobre [creación de una cuenta de laboratorio](./tutorial-setup-lab-account.md#create-a-lab-account) para obtener instrucciones.
   
    Para obtener más información sobre la nomenclatura, consulte el artículo siguiente:

   - [Guía de nomenclatura para los recursos](./administrator-guide.md#naming)

2. **Incorporación de usuarios al rol de creador de laboratorio.** Consulte [Incorporación de usuarios al rol de creador de laboratorio](./tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role) para obtener instrucciones.


3. **Conéctese a una red virtual del mismo nivel.** Para obtener instrucciones, consulte [Conexión de la red del laboratorio con una red virtual del mismo nivel](./how-to-connect-peer-virtual-network.md).

   También puede que necesite consultar instrucciones sobre cómo [configurar el intervalo de direcciones de las máquinas virtuales del laboratorio](./how-to-configure-lab-accounts.md).

4. **Habilite y revise imágenes.** Para obtener instrucciones, consulte [Habilitación de imágenes de Azure Marketplace para creadores de laboratorios](./specify-marketplace-images.md).

   Para revisar el contenido de cada imagen de Azure Marketplace, seleccione el nombre de la imagen. Por ejemplo, la captura de pantalla siguiente muestra los detalles de la imagen de Data Science Virtual Machine de Ubuntu:

   ![Captura de pantalla de la revisión de imágenes de Azure Marketplace](./media/setup-guide/review-marketplace-images.png)

   Si tiene una instancia de Shared Image Gallery adjunta a su cuenta de laboratorio y quiere habilitar imágenes personalizadas para compartirlas con los creadores de laboratorio, tendrá que completar unos pasos similares a los que se muestran en la siguiente captura de pantalla:

   ![Captura de pantalla de la habilitación de imágenes personalizadas en Shared Image Gallery](./media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Pasos siguientes

Los pasos siguientes son comunes para configurar un entorno de laboratorio:

- [Administración de cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Guía de instalación de un laboratorio](setup-guide.md)