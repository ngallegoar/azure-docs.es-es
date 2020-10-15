---
title: Modelo de seguridad de Notification Hubs
description: Obtenga información sobre el modelo de seguridad de Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: ''
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 09/23/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: b871775bc7a6d795e86147ae9cffa27bdd2f3348
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76263768"
---
# <a name="notification-hubs-security"></a>Seguridad de Notification Hubs

## <a name="overview"></a>Información general

En este tema se describe el modelo de seguridad de Azure Notification Hubs.

## <a name="shared-access-signature-security"></a>Seguridad de Firma de acceso compartido

Notification Hubs implementa un esquema de seguridad de nivel de entidad llamado *Firma de acceso compartido* (SAS). Cada regla contiene un nombre, un valor de clave (secreto compartido) y un conjunto de derechos, tal como se explica en [Notificaciones de seguridad](#security-claims). 

Al crear un centro, automáticamente se crean dos reglas: una con derechos de **escucha** (que usa la aplicación cliente) y otra con **todos** los derechos (que usa el back-end de la aplicación):

- **DefaultListenSharedAccessSignature**: otorga solamente permiso de **escucha**.
- **DefaultFullSharedAccessSignature**: otorga permisos de **escucha**, **administración**, y **envío**. Esta directiva solo se usará en el back-end de la aplicación. No la use en aplicaciones cliente, para ellas use una directiva con solo acceso de **escucha**. Para crear una nueva Directiva de acceso personalizada con un nuevo token de SAS, consulte [Tokens de SAS para directivas de acceso](#sas-tokens-for-access-policies) más adelante en este artículo.

Al realizar la administración de registros desde aplicaciones cliente, si la información enviada a través de notificaciones no es confidencial (por ejemplo, actualizaciones del información meteorológica), una forma común de acceder a un Centro de notificaciones es dar al valor de clave de la regla acceso de solo escucha a la aplicación cliente y proporcionar al valor de clave de la regla acceso completo al back-end de la aplicación.

Las aplicaciones no deben insertar el valor clave en las aplicaciones cliente de la Tienda Windows; en su lugar, la aplicación cliente debe recuperarlo del back-end de la aplicación en el inicio.

La clave con acceso de **escucha** permite a una aplicación cliente registrarse para cualquier etiqueta. Si la aplicación debe restringir los registros en etiquetas específicas a clientes específicos (por ejemplo, si las etiquetas representan identificadores de usuario), el back-end de la aplicación debe realizar los registros. Para más información, consulte [Administración de registros](notification-hubs-push-notification-registration-management.md). Tenga en cuenta que, de este modo, la aplicación cliente no tendrá acceso directo a Notification Hubs.

## <a name="security-claims"></a>Notificaciones de seguridad

De modo similar a otras entidades, las operaciones del Centro de notificaciones se permiten para tres notificaciones de seguridad: **escucha**, **envío** y **administración**.

| Notificación   | Descripción                                          | Operaciones permitidas |
| ------- | ---------------------------------------------------- | ------------------ |
| Escuchar  | Crear o actualizar, leer y eliminar registros únicos | Crear o actualizar registro<br><br>Leer el registro<br><br>Leer todos los registros de un controlador<br><br>Eliminar registro |
| Envío    | Enviar mensajes a la instancia de Notification Hubs                | Enviar mensaje |
| Administrar  | CRUD en Notification Hubs (incluida la actualización de las credenciales de PNS y claves de seguridad) y registros de lectura basados en etiquetas |Crear, actualizar, leer, escribir, eliminar centros<br><br>Leer registros por etiqueta |

Notification Hubs acepta tokens de SAS generados con claves compartidas configuradas directamente en el centro.

No es posible enviar una notificación a más de un espacio de nombres. Los espacios de nombres son contenedores lógicos para Notification Hubs y no participan en el envío de notificaciones.

Use las directivas de acceso de nivel de espacio de nombres (credenciales) para operaciones de nivel de espacio de nombres, por ejemplo, para crear listas de centros, crear o eliminar centros, etc. Solo las directivas de acceso de nivel de centro permiten enviar notificaciones.

### <a name="sas-tokens-for-access-policies"></a>Tokens de SAS para directivas de acceso

Para crear una nueva notificación de seguridad o para ver las claves de SAS existentes, haga lo siguiente:

1. Inicie sesión en Azure Portal.
2. Seleccione **Todos los recursos**.
3. Seleccione el nombre de la instancia de Notification Hubs para el que desea crear la notificación o ver la clave de SAS.
4. En el menú izquierdo, seleccione **Directivas de acceso**.
5. Seleccione **Nueva directiva** para crear una nueva notificación de seguridad. Asigne un nombre a la directiva y seleccione los permisos que desea conceder. Después, seleccione **Aceptar**.
6. La cadena de conexión completa (incluida la nueva clave SAS) se muestra en la ventana Directivas de acceso. Puede copiar esta cadena en el portapapeles para su uso posterior.

Para extraer la clave SAS de una directiva específica, seleccione el botón **Copiar** situado junto a la directiva que contenga la clave de SAS que desee. Pegue este valor en una ubicación temporal y, a continuación, copie la parte de la clave de SAS de la cadena de conexión. En este ejemplo se usa un espacio de nombres de Notification Hubs denominado **mytestnamespace1** y una directiva denominada **policy2**. La clave de SAS es el valor situado cerca del final de la cadena, especificado por **SharedAccessKey**:

```shell
Endpoint=sb://mytestnamespace1.servicebus.windows.net/;SharedAccessKeyName=policy2;SharedAccessKey=<SAS key value here>
```

![Obtención las claves de SAS](media/notification-hubs-push-notification-security/access1.png)

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Notification Hubs](notification-hubs-push-notification-overview.md)
