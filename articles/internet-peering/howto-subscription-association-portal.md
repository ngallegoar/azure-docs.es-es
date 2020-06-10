---
title: Asociación de un ASN del mismo nivel a una suscripción de Azure mediante el portal
titleSuffix: Azure
description: Asociación de un ASN del mismo nivel a una suscripción de Azure mediante el portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 5/18/2020
ms.author: derekol
ms.openlocfilehash: 9feefa8801892a668784ed7406f46dba2afd69da
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266957"
---
# <a name="associate-peer-asn-to-azure-subscription-using-the-portal"></a>Asociación de un ASN del mismo nivel a una suscripción de Azure mediante el portal

Como proveedor de servicios de Internet o proveedor de intercambio de Internet, antes de enviar una solicitud de emparejamiento debe asociar su ASN con una suscripción de Azure siguiendo estos pasos.

Si lo prefiere, puede completar esta guía con [PowerShell](howto-subscription-association-powershell.md).

## <a name="create-peerasn-to-associate-your-asn-with-azure-subscription"></a>Creación de un PeerAsn (ASN del mismo nivel) para asociar su ASN con la suscripción a Azure

### <a name="sign-in-to-the-portal"></a>Iniciar sesión en el portal
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="register-for-peering-resource-provider"></a>Registrarse como proveedor de recursos de emparejamiento
Para registrarse como proveedor de recursos de emparejamiento en su suscripción, siga los pasos a continuación. Si no hace esto, no se podrá acceder a los recursos de Azure necesarios para configurar el emparejamiento.

1. Haga clic en **Suscripciones** en la esquina superior izquierda del portal. Si no se muestra la opción, haga clic en **Más servicios** y búsquela.

    > [!div class="mx-imgBorder"]
    > ![Abrir la página de Suscripciones](./media/rp-subscriptions-open.png)

1. Haga clic en la suscripción que quiere usar para el emparejamiento.

    > [!div class="mx-imgBorder"]
    > ![Abrir la suscripción](./media/rp-subscriptions-launch.png)

1. Una vez que haya abierto la suscripción, del lado izquierdo, haga clic en **Proveedores de recursos**. A continuación, en el panel derecho, busque *emparejamiento* en la ventana de búsqueda, o use la barra de desplazamiento para navegar hasta **Microsoft.Peering** y examine el **Estado**. Si el estado es ***Registered***, omita los pasos siguientes y vaya a la sección **Crear un PeerAsn**. Si el estado es ***NotRegistered***, seleccione **Microsoft.Peering** y haga clic en **Registrar**.

    > [!div class="mx-imgBorder"]
    > ![Inicio del registro](./media/rp-register-start.png)

1. Observe que el estado cambia a ***Registering***.

    > [!div class="mx-imgBorder"]
    > ![Registro en curso](./media/rp-register-progress.png)

1. Espere aproximadamente un minuto para que finalice el registro. A continuación, haga clic en **Actualizar** y compruebe si el estado es ***Registered***.

    > [!div class="mx-imgBorder"]
    > ![Registro completado](./media/rp-register-completed.png)

### <a name="create-peerasn"></a>Crear un PeerAsn
Como proveedor de servicios de Internet o proveedor de intercambio de Internet, puede crear un recurso de PeerAsn para asociar un número de sistema autónomo (ASN) con una suscripción a Azure en la [página Asociar un ASN del mismo nivel](https://go.microsoft.com/fwlink/?linkid=2129592). Puede asociar varios ASN a una suscripción al crear un **PeerAsn** para cada ASN que necesite asociar.

1. En la página **Asociar un ASN del mismo nivel**, en la pestaña **Aspectos básicos**, rellene los campos como se muestra a continuación.

    > [!div class="mx-imgBorder"]
    > ![Pestaña de aspectos básicos de PeerAsn](./media/peerasn-basics-tab.png)

    * **Nombre** corresponde al nombre del recurso y puede ser el que prefiera.  
    * Elija la **Suscripción** que necesita asociar con el ASN.
    * **Nombre de mismo nivel** corresponde al nombre de la empresa y debe asemejarse lo máximo posible a su perfil de PeeringDB. Tenga en cuenta que el valor solo admite los caracteres a-z, A-Z y espacios.
    * Escriba su ASN en el campo **ASN del mismo nivel**.
    * Haga clic en **Crear nuevo** y escriba la **dirección de correo electrónico** y el **número de teléfono** del Centro de operaciones de red (NOC)
1. A continuación, haga clic en **Revisar y crear** y observe cómo el portal ejecuta una validación básica de la información especificada. Esta información se muestra en una cinta en la parte superior, con el texto *Ejecutando la validación final...* .

    > [!div class="mx-imgBorder"]
    > ![Pestaña de revisión de PeerAsn](./media/peerasn-review-tab-validation.png)

1. Cuando el mensaje de la cinta se convierta en *Validación superada*, compruebe la información y envíe la solicitud. Para ello, haga clic en **Crear**. Si no se supera la validación, haga clic en **Anterior** y repita los pasos anteriores para modificar la solicitud y asegurarse de que los valores que especifique no tienen errores.

    > [!div class="mx-imgBorder"]
    > ![Pestaña de revisión de PeerAsn](./media/peerasn-review-tab.png)

1. Después de enviar la solicitud, espere a que finalice la implementación. Si se produce un error en la implementación, póngase en contacto con el [equipo de emparejamiento de Microsoft](mailto:peering@microsoft.com). Una implementación correcta tendrá un aspecto similar al siguiente.

    > [!div class="mx-imgBorder"]
    > ![Implementación de PeerAsn correcta](./media/peerasn-success.png)

### <a name="view-status-of-a-peerasn"></a>Ver el estado de un PeerAsn
Una vez que el recurso de PeerAsn se haya implementado correctamente, deberá esperar a que Microsoft apruebe la solicitud de asociación. La aprobación puede tardar hasta 12 horas. Una vez aprobada, recibirá una notificación a la dirección de correo electrónico que especificó en la sección anterior.

> [!IMPORTANT]
> Espere a que el estado de ValidationState cambie a "Approved" (aprobado) antes de enviar una solicitud de emparejamiento. La aprobación puede tardar hasta 12 horas.

## <a name="modify-peerasn"></a>Modificación de un PeerAsn
Actualmente no se admite la modificación de un PeerAsn. Si necesita realizar modificaciones, póngase en contacto con el equipo de [emparejamiento de Microsoft](mailto:peering@microsoft.com).

## <a name="delete-peerasn"></a>Eliminación de un PeerAsn
Actualmente no se admite la eliminación de un PeerAsn. Si necesita eliminar un PeerAsn, póngase en contacto con el equipo de [emparejamiento de Microsoft](mailto:peering@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
* [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md)
* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para más información, vea las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).
