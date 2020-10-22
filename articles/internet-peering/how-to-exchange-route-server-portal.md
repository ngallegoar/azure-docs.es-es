---
title: Conexión de emparejamiento para asociados de Exchange con el servidor de ruta mediante el portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento de Exchange con el servidor de ruta mediante Azure Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: 7e10cd91eadd338217845b1504e8e9160bccfc98
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91537198"
---
# <a name="create-or-modify-an-exchange-peering-with-route-server-in-azure-portal"></a>Creación o modificación de un emparejamiento de Exchange con el servidor de ruta en Azure Portal

En este artículo se describe cómo crear un emparejamiento de Microsoft Exchange con un servidor de ruta mediante Azure Portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.


## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md) antes de comenzar la configuración.
* Si ya dispone de emparejamientos de Exchange con Microsoft que no se convierten en recursos de Azure, consulte el artículo [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Creación y aprovisionamiento de un emparejamiento de Exchange

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering-with-route-server"></a><a name=create></a>Creación de un emparejamiento de Exchange con el servidor de ruta


Como proveedor de intercambio de Internet, puede crear una solicitud de emparejamiento de Exchange [creando un emparejamiento]( https://go.microsoft.com/fwlink/?linkid=2129593).

1. En la página **Create a Peering** (Crear un emparejamiento), en la pestaña **Basics** (Aspectos básicos), rellene los cuadros como se muestra aquí:

    > [!div class="mx-imgBorder"] 
    > ![Registro de Peering Service](./media/setup-basics-tab.png)

* Seleccione su suscripción a Azure.

* En Resource group (Grupo de recursos), puede seleccionar un grupo de recursos existente en la lista desplegable o crear un grupo en Create new (Crear nuevo). En este ejemplo se va a crear un grupo de recursos.

* Name (Nombre) corresponde al nombre del recurso y puede ser el que prefiera.

* El valor de Región se selecciona automáticamente si se elige un grupo de recursos existente. Si ha elegido crear un grupo de recursos, también debe elegir la región de Azure en la que quiere que resida el recurso.

    >[!NOTE]
    >La región donde reside un grupo de recursos es independiente de la ubicación en la que quiere crear el emparejamiento con Microsoft. No obstante, se recomienda organizar los recursos de emparejamiento en grupos de recursos que residan en las regiones más cercanas de Azure. Por ejemplo, para los emparejamientos de Ashburn, puede crear un grupo de recursos en Este de EE. UU. o Este de EE. UU. 2.

* Seleccione su ASN en el cuadro **Peer ASN** (ASN del mismo nivel).

    >[!IMPORTANT] 
    >Solo puede elegir un ASN con el valor de ValidationState como aprobado antes de enviar una solicitud de emparejamiento. Si acaba de enviar la solicitud de PeerAsn, espere 12 horas aproximadamente a que el estado de la asociación de ASN sea "aprobado". Si el ASN seleccionado está pendiente de validación, verá un mensaje de error. Si no ve el ASN que debe elegir, compruebe que haya seleccionado la suscripción correcta. Si es así, compruebe si ya ha creado PeerAsn con **[Associate Peer ASN to Azure subscription (Asociar ASN del mismo nivel a suscripción de Azure)](https://go.microsoft.com/fwlink/?linkid=2129592)** .

* Seleccione **Siguiente: Configuración** para continuar.

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío

1. En la página Create a Peering (Crear un emparejamiento), en la pestaña Configuración, rellene los cuadros como se muestra aquí:

    > [!div class="mx-imgBorder"]
    > ![Configurar del servidor de ruta](./media/setup-exchange-conf-tab-routeserver.png)
 
    * En Tipo de emparejamiento, seleccione **Directo**.
    * En Microsoft network (Red de Microsoft), seleccione **AS8075 with exchange route server** (AS8075 con el servidor de ruta de Exchange). 
    * Seleccione el tipo de SKU **Gratis Básico**. No seleccione el nivel prémium gratis, ya que está reservado para aplicaciones especiales.
    * Seleccione la ubicación **Metro** donde quiere configurar el emparejamiento.

1. En **Peering Connections** (Conexiones de emparejamiento), seleccione **Crear nueva**.

1.  En **Direct Peering Connection** (Conexión de emparejamiento directo), rellene los siguientes detalles de la sesión de BGP:

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla que muestra el panel Direct Peering Connection (Conexión de emparejamiento directo) con detalles agregados.](./media/setup-exchange-conf-tab-direct-route.png)


     * En Peering Facility (Instalación de emparejamiento), seleccione la ubicación física adecuada para el emparejamiento.
     * En Session Address Provider (Proveedor de direcciones de sesión), seleccione Del mismo nivel.
     * El valor de Session IPv4 prefix (Prefijo IPv4 de la sesión) lo proporcionará el proveedor de Exchange del mismo nivel.
     * El valor de Peer session IPv4 address (Dirección IPv4 de la sesión del mismo nivel) la seleccionará el emparejamiento de Exchange del servidor de ruta de su intervalo de prefijos de IP.
     * El valor de Microsoft session IPv4 address (Dirección IPv4 de sesión de Microsoft) será la dirección IP del enrutador asignada del intervalo de prefijos de IP.
     * El valor de Session IPv6 address (Dirección IPv6 de la sesión) es opcional en este momento.
     * El valor de Maximum advertised IPv4 prefix (Prefijo de IPv4 anunciado máximo) puede ser hasta 20000. 
     * La opción Use for Peering Service (Uso para el servicio de emparejamiento) está deshabilitada de forma predeterminada. Se puede habilitar una vez que el proveedor de Exchange haya firmado un acuerdo de Peering Service con Microsoft.

1. Al finalizar, haga clic en **Guardar**. 

1. En Create a Peering (Crear un emparejamiento), verá que se ha superado la validación. Una vez superada la validación, haga clic en **Crear**.

    > [!div class="mx-imgBorder"]
    > ![Validación de la configuración](./media/setup-exchange-conf-tab-validation.png)

    >[!NOTE]
    >En el caso de los proveedores de servicios de Internet (ISP) normales que son asociados de Microsoft Peering Service, es necesario el registro de prefijos IP de cliente. Sin embargo, en el caso de los asociados de Exchange con un servidor de ruta, es necesario registrar los ASN de cliente y no los prefijos. La misma clave ASN sería válida para el registro de prefijos del cliente.

1. Seleccione **ASN registrados** en la sección Configuración.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla que muestra el panel Emparejamiento con el elemento de menú Registered A S Ns (A S N registrados) activado.](./media/setup-exchange-registered-asn.png)

1. Seleccione **Add registered ASN** (Agregar ASN registrado) para crear un nuevo ASN de cliente en su suscripción de Exchange.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla que muestra el panel Register an A S N (Registrar un A S N) con los cuadros de texto Name (Nombre) y A S N.](./media/setup-exchange-register-new-asn.png)

1. En Register an ASN (Registrar un ASN), seleccione un nombre, rellene el ASN de cliente y haga clic en Guardar.

1. En Registered ASNs (ASN registrados), habrá una clave de prefijo asociada asignada a cada ASN. Como proveedor de Exchange, tendrá que proporcionar esta clave de prefijo al cliente para que pueda registrar Peering Service en su suscripción.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla que muestra el panel Registered A S Ns (A S N registrados) con claves de prefijo.](./media/setup-exchange-register-asn-prefixkey.png)




### <a name="verify-an-exchange-peering"></a><a name=get></a>Comprobación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Modificación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Desaprovisionamiento de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
* [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para más información, consulte las [preguntas frecuentes sobre el emparejamiento de Internet](faqs.md).
