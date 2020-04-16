---
title: Guía de configuración acelerada de una cuenta de laboratorio para Azure Lab Services
description: Esta guía ayuda a los administradores a configurar rápidamente una cuenta de laboratorio para usarla en su centro educativo.
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
ms.date: 3/18/2020
ms.author: spelluru
ms.openlocfilehash: 8fcc46487e7f7c2d075639f10a30cae9950ff31b
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879588"
---
# <a name="lab-account-setup-guide"></a>Guía de configuración de una cuenta de laboratorio

Como primer paso, los administradores deben configurar una cuenta de laboratorio dentro de la suscripción de Azure. Una cuenta de laboratorio es un contenedor para los laboratorios educativos y solo lleva unos minutos configurarla.

## <a name="understand-your-schools-lab-account-requirements"></a>Información sobre los requisitos de la cuenta de laboratorio de su escuela

Para comprender cómo configurar la cuenta de laboratorio en función de las necesidades de su escuela, debe tener en cuenta estas preguntas.

### <a name="do-i-have-access-to-an-azure-subscription"></a>¿Tengo acceso a una suscripción de Azure?

Para crear una cuenta de laboratorio necesita tener acceso a una suscripción de Azure configurada para su escuela. Su escuela podría tener una o más suscripciones. Las suscripciones se usan para administrar la facturación y la seguridad de todos los recursos y servicios de Azure, incluidas las cuentas de laboratorio.

### <a name="how-many-lab-accounts-need-to-be-created"></a>¿Cuántas cuentas de laboratorio es necesario crear?

Para empezar a trabajar rápidamente, cree una sola cuenta de laboratorio y, después, cree cuentas de laboratorio adicionales según sea necesario. Por ejemplo, podría tener una cuenta de laboratorio por departamento.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>¿Quiénes deben ser propietarios y colaboradores de la cuenta de laboratorio?

Los administradores suelen ser propietarios y colaboradores de una cuenta de laboratorio. Son responsables de administrar las directivas que se aplican a todos los laboratorios incluidos en la cuenta de laboratorio. La persona que crea la cuenta de laboratorio se convierte automáticamente en propietario. Puede agregar propietarios y colaboradores adicionales, normalmente desde el inquilino de Azure Active Directory (Azure AD) asociado a la suscripción. Esto puede ser útil para ayudar a administrar una cuenta de laboratorio mediante la asignación del rol de propietario o colaborador en el nivel de la cuenta de laboratorio.

### <a name="who-will-be-allowed-to-create-and-manage-labs"></a>¿A quién se le permitirá crear y administrar laboratorios?

Puede decidir que los administradores y los miembros del profesorado creen y administren laboratorios. Estos usuarios (normalmente desde el inquilino de Azure AD asociado a la suscripción) se asignan al rol de creador de laboratorio dentro de la cuenta de laboratorio.

### <a name="do-you-want-to-give-lab-creators-the-ability-to-save-images-that-can-be-shared-across-labs"></a>¿Desea proporcionar a los creadores de laboratorios la capacidad de guardar imágenes que se puedan compartir entre varios laboratorios?

Shared Image Gallery es un repositorio que se puede usar para guardar y compartir imágenes. Si tiene varias clases que necesitan las mismas imágenes, los creadores de laboratorio pueden crear la imagen una vez y compartirla entre varios laboratorios. Sin embargo, para empezar, no necesita necesariamente una galería de imágenes compartidas, ya que siempre puede agregar una más adelante.

Si respondió afirmativamente a esta pregunta, tendrá que crear o adjuntar una instancia de Shared Image Gallery a la cuenta de laboratorio. Si su respuesta es "no lo sé", puede posponer esta decisión hasta más adelante.

### <a name="which-images-in-azure-marketplace-will-your-classroom-labs-use"></a>¿Qué imágenes de Azure Marketplace van a usar los laboratorios educativos?

Azure Marketplace proporciona cientos de imágenes que puede habilitar para que los creadores de laboratorios puedan usar dichas imágenes para crear su laboratorio. Algunas imágenes pueden incluir ya todo lo que necesita un laboratorio. En otros casos, una imagen sirve como punto de partida, que el creador del laboratorio puede personalizar mediante la instalación de aplicaciones o herramientas adicionales.

Si no sabe qué imágenes va a necesitar, siempre tiene la posibilidad de volver a este punto más adelante para habilitarlas. Además, la mejor manera de ver qué imágenes están disponibles es crear primero una cuenta de laboratorio. Esto le proporciona acceso para que pueda revisar la lista de imágenes disponibles y su contenido.
  
### <a name="do-the-labs-virtual-machines-need-to-have-access-to-other-azure-or-on-premises-resources"></a>¿En necesario que las máquinas virtuales del laboratorio tengan acceso a otros recursos de Azure o locales?

Al configurar una cuenta de laboratorio, también tiene la opción de emparejamiento con una red virtual. Para decidir si lo necesita, tenga en cuenta las siguientes preguntas:

- **¿Necesita proporcionar acceso a un servidor de licencias?**
  
   Si planea usar imágenes de Azure Marketplace, el costo de la licencia de sistema operativo se incluye en los precios de los servicios de laboratorio. Por lo tanto, no es necesario proporcionar licencias para el propio sistema operativo. Sin embargo, para el software y las aplicaciones adicionales que se instalen, tendrá que proporcionar una licencia según corresponda.

- **¿Necesitan las máquinas virtuales de laboratorio acceder a otros recursos locales como un recurso compartido de archivos o una base de datos?**

   Debe crear una red virtual para proporcionar acceso a los recursos locales, normalmente mediante una puerta de enlace de red virtual de sitio a sitio. Si no dispone de una red virtual configurada, debe invertir un tiempo adicional para ello.

- **¿Necesitan las máquinas virtuales de laboratorio acceder a otros recursos de Azure que se encuentran en una red virtual?**

   Si necesita acceder a recursos de Azure que *no* están protegidos en una red virtual, puede acceder a estos recursos mediante la red pública de Internet, sin realizar ningún emparejamiento.

Si ha respondido "sí" a una o más preguntas, tendrá que emparejar la cuenta de laboratorio con una red virtual. Si su respuesta es "no sé", puede posponer esta decisión hasta más adelante. Siempre puede emparejar una red virtual después de crear la cuenta de laboratorio.

## <a name="set-up-your-lab-account"></a>Configuración de la cuenta de laboratorio

Una vez que comprenda los requisitos de la cuenta de laboratorio, está listo para configurarla.

1. **Cree la cuenta de laboratorio.** Consulte el tutorial sobre [creación de una cuenta de laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) para obtener instrucciones.

   Al crear una cuenta de laboratorio, es posible que le resulte útil familiarizarse con los recursos de Azure implicados. Para más información, consulte los siguientes artículos.

   - [Suscripción](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)
   - [Grupos de recursos](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#resource-group)
   - [Cuenta de laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#lab-account)
   - [Laboratorio educativo](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#classroom-lab)
   - [Selección de una región y una ubicación](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)
   - [Guía de nomenclatura para los recursos](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **Incorporación de usuarios al rol de creador de laboratorio.** Consulte [Incorporación de usuarios al rol de creador de laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role) para obtener instrucciones.

   Además, para más información sobre los diferentes roles que se pueden asignar a los usuarios que administrarán laboratorios y cuentas de laboratorio, consulte la [guía sobre la administración de identidad](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#manage-identity).

3. **Conéctese a una red virtual del mismo nivel.** Para obtener instrucciones, consulte [Conexión de la red del laboratorio con una red virtual del mismo nivel](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network).

   También puede que necesite consultar instrucciones sobre cómo [configurar el intervalo de direcciones de las máquinas virtuales del laboratorio](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab).

4. **Habilite y revise imágenes.** Para obtener instrucciones, consulte [Habilitación de imágenes de Azure Marketplace para creadores de laboratorios](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images).

   Para revisar el contenido de cada imagen de Azure Marketplace, seleccione el nombre de la imagen. Por ejemplo, la captura de pantalla siguiente muestra los detalles de la imagen de Data Science Virtual Machine de Ubuntu:

   ![Captura de pantalla de la revisión de imágenes de Azure Marketplace](../media/setup-guide/review-marketplace-images.png)

   Si tiene una instancia de Shared Image Gallery adjunta a la cuenta de laboratorio y desea habilitar imágenes personalizadas para compartirlas con los creadores de laboratorio, tendrá que completar unos pasos similares a los que se muestran en la siguiente captura de pantalla:

   ![Captura de pantalla de la habilitación de imágenes personalizadas en Shared Image Gallery](../media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Pasos siguientes

- [Administración de cuentas de laboratorio](how-to-manage-lab-accounts.md)

- [Guía de configuración del laboratorio educativo](setup-guide.md)
