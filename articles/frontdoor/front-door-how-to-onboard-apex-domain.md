---
title: 'Incorporación de un dominio raíz o de vértice a una instancia existente de Front Door: Azure Portal'
description: Obtenga información sobre cómo incorporar un dominio raíz o de vértice a una instancia existente de Front Door mediante Azure Portal.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 11/13/2020
ms.author: duau
ms.openlocfilehash: 55eefe7a7490df050aa7ebc2bb41fbadcc8d8279
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94646345"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Incorporación de un dominio raíz o de vértice a una instancia de Front Door
Azure Front Door usa registros CNAME a fin de validar la propiedad del dominio para la incorporación de dominios personalizados. Front Door no expone la dirección IP de front-end asociada a su perfil de Front Door. Por lo tanto, no puede asignar el dominio de vértice a una dirección IP si su intención es incorporarlo a Azure Front Door.

El protocolo DNS evita la asignación de registros CNAME en el vértice de zona. Por ejemplo, si el dominio es `contoso.com`, puede crear registros CNAME para `somelabel.contoso.com`, pero no puede crear CNAME para `contoso.com`. Esta restricción presenta un problema para los propietarios de aplicaciones que tienen aplicaciones con equilibrio de carga detrás de Azure Front Door. Puesto que el uso de un perfil de Front Door requiere la creación de un registro CNAME, no es posible apuntar al perfil de Front Door desde el vértice de la zona.

Este problema se puede resolver mediante registros de alias en Azure DNS. A diferencia de los registros CNAME, los registros de alias se crean en el vértice de zona. Los propietarios de aplicaciones pueden usarlo para apuntar su registro de vértice de zona a un perfil de Front Door que tenga puntos de conexión públicos. Los propietarios de aplicaciones pueden apuntar al mismo perfil de Front Door que se use para cualquier otro dominio dentro de su zona DNS. Por ejemplo, `contoso.com` y `www.contoso.com` pueden apuntar al mismo perfil de Front Door. 

La asignación del dominio de vértice o raíz a su perfil de Front Door requiere básicamente el acoplamiento de CNAME o el seguimiento de DNS. Mecanismo en el que el proveedor de DNS resuelve de forma recursiva la entrada CNAME hasta que encuentra una dirección IP. Esta funcionalidad es compatible con Azure DNS para puntos de conexión de Front Door. 

> [!NOTE]
> También hay otros proveedores de DNS que admiten la eliminación del formato de CNAME o la búsqueda de DNS, pero Azure Front Door recomienda a sus clientes el uso de Azure DNS para hospedar sus dominios.

Puede usar Azure Portal para incorporar un dominio de vértice en Front Door y habilitar HTTPS si lo asocia con un certificado para la terminación TSL. Los dominios de vértice también se conocen como dominios raíz o "desnudos".

## <a name="create-an-alias-record-for-zone-apex"></a>Creación de un registro de alias para el vértice de la zona

1. Abra la configuración de **Azure DNS** del dominio que se va a incorporar.

1. Cree o edite el registro del vértice de la zona.

1. Seleccione el **Tipo** de registro *A* y haga clic en *Sí* para el **Conjunto de registros del alias**. El **Tipo de alias** debe establecerse en *Recurso de Azure*.

1. Seleccione la suscripción de Azure donde se hospeda el perfil de Front Door. Después, seleccione el recurso de Front Door en la lista desplegable **Recurso de Azure**.

1. Seleccione **Aceptar** para enviar los cambios.

    :::image type="content" source="./media/front-door-apex-domain/front-door-apex-alias-record.png" alt-text="Registro de alias para el vértice de la zona":::

1. El paso anterior creará un registro de vértice de zona que apunta al recurso de Front Door y también una asignación de registros CNAME "afdverify" (por ejemplo, `afdverify.contosonews.com`) que se usará para la incorporación del dominio en el perfil de Front Door.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Incorporación del dominio personalizado a la instancia de Front Door

1. En la pestaña del diseñador de Front Door, seleccione el icono "+" en la sección Hosts de front-end para agregar un nuevo dominio personalizado.

1. Escriba el nombre de dominio raíz o de vértice en el campo Nombre de host personalizado, por ejemplo, `contosonews.com`.

1. Una vez validada la asignación de CNAME del dominio a la instancia de Front Door, seleccione **Agregar** para agregar el dominio personalizado.

1. Seleccione **Guardar** para enviar los cambios.

   :::image type="content" source="./media/front-door-apex-domain/front-door-onboard-apex-domain.png" alt-text="Menú Dominio personalizado":::

## <a name="enable-https-on-your-custom-domain"></a>Habilitación de HTTPS en el dominio personalizado

1. Haga clic en el dominio personalizado que se ha agregado y, en la sección **Personalizar HTTPS de dominio**, cambie el estado a **Habilitado**.

1. En **Tipo de administración de certificados**, seleccione *Usar mi propio certificado*.

   :::image type="content" source="./media/front-door-apex-domain/front-door-onboard-apex-custom-domain.png" alt-text="Configuración de HTTPS de dominio personalizado":::    

   > [!WARNING]
   > Actualmente no se admite el tipo de administración de certificados Front Door administrado para dominios raíz o de vértice. La única opción disponible para habilitar HTTPS en un dominio raíz o de vértice para Front Door consiste en usar su propio certificado TLS/SSL personalizado hospedado en Azure Key Vault.

1. Asegúrese de que ha configurado los permisos adecuados para que Front Door acceda a Key Vault, como se indica en la interfaz de usuario, antes de continuar con el paso siguiente.

1. Elija una **cuenta de Key Vault** desde su suscripción actual y, luego, seleccione el **Secreto** y la **Versión del secreto** adecuados para asignar el certificado correcto.

1. Seleccione **Actualizar** para guardar la selección y, después, elija **Guardar**.

1. Seleccione **Actualizar** tras un par de minutos y, luego, elija el dominio personalizado de nuevo para ver el progreso del aprovisionamiento de certificados. 

> [!WARNING]
> Asegúrese de que ha creado las reglas de enrutamiento adecuadas para su dominio de vértice o de que ha agregado el dominio a las reglas de enrutamiento existentes.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).
