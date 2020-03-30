---
title: Consorcio de Azure Blockchain Service
description: Información general sobre cómo Azure Blockchain Service implementa las redes de cadena de bloques del consorcio.
ms.date: 11/21/2019
ms.topic: conceptual
ms.reviewer: zeyadr
ms.openlocfilehash: 7b8885ba08d35db20d1eb7e75141cb173913b386
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225976"
---
# <a name="azure-blockchain-service-consortium"></a>Consorcio de Azure Blockchain Service

Mediante Azure Blockchain Service, puede crear redes de cadenas de bloques de consorcio privadas, donde cada red de cadena de bloques puede estar limitada a participantes específicos de la red. Solo los participantes de la red de cadena de bloques de consorcio privada pueden ver e interactuar con la cadena de bloques. Las redes de consorcio en Azure Blockchain Service pueden contener dos tipos de roles de participante miembro:

* **Administrador**: participantes con privilegios que pueden realizar acciones de administración de consorcios y participar en transacciones de cadenas de bloques.

* **Usuario**: participantes que no pueden realizar acciones de administración de consorcios, pero pueden participar en transacciones de cadenas de bloques.

Las redes de consorcio pueden ser una combinación de roles de participantes y pueden tener un número arbitrario de cada tipo de rol. Debe haber al menos un administrador.

En el siguiente diagrama se muestra una red de consorcio con varios participantes:

![Diagrama de red de consorcio privada](./media/consortium/network-diagram.png)

Con la administración de consorcios en Azure Blockchain Service, puede administrar los participantes en la red de consorcio. La administración del consorcio se basa en el modelo de consenso de la red. En la versión preliminar actual, Azure Blockchain Service proporciona un modelo de consenso centralizado para la administración de consorcios. Cualquier participante con privilegios con un rol de administrador puede realizar acciones de administración de consorcios, como agregar o quitar participantes de una red.

## <a name="roles"></a>Roles

Los participantes de un consorcio pueden ser individuos u organizaciones y se les puede asignar un rol de usuario o de administrador. En la tabla siguiente se muestran las diferencias de alto nivel entre los dos roles:

| Acción | Rol de usuario | Rol de administrador
|--------|:----:|:------------:|
| Crear nuevo miembro | Sí | Sí |
| Invitar nuevos miembros | No | Sí |
| Establecer o cambiar el rol de participante miembro | No | Sí |
| Cambiar el nombre para mostrar del miembro | Solo para el propio miembro | Solo para el propio miembro |
| Eliminación de miembros | Solo para el propio miembro | Sí |
| Participar en transacciones de cadenas de bloques | Sí | Sí |

### <a name="user-role"></a>Rol de usuario

Los usuarios son participantes de consorcio sin funcionalidades de administrador. No pueden participar en la administración de miembros relacionados con el consorcio. Los usuarios pueden cambiar el nombre para mostrar de miembro y pueden quitarse de un consorcio.

### <a name="administrator"></a>Administrador

Un administrador puede administrar los miembros dentro del consorcio. Un administrador puede invitar a miembros, quitar miembros o actualizar los roles de los miembros del consorcio.
Siempre debe haber al menos un administrador dentro de un consorcio. El último administrador debe especificar a otro participante como rol de administrador antes de abandonar un consorcio.

## <a name="managing-members"></a>Administración de miembros

Solo los administradores pueden invitar a otros participantes al consorcio. Los administradores invitan a los participantes con su identificador de suscripción de Azure.

Una vez invitados, los participantes pueden unirse al consorcio de la cadena de bloques mediante la implementación de un nuevo miembro en Azure Blockchain Service. Para ver el consorcio invitado y unirse a él, debe especificar el mismo identificador de suscripción de Azure usado por el administrador de red en la invitación.

Los administradores pueden quitar a cualquier participante del consorcio, incluidos otros administradores. Los miembros solo pueden quitarse a sí mismos de un consorcio.

## <a name="consortium-management-smart-contract"></a>Contrato inteligente de administración de consorcios

La administración de consorcios en Azure Blockchain Service se realiza a través de los contratos inteligentes de administración de consorcios. Los contratos inteligentes se implementan automáticamente en los nodos al implementar un nuevo miembro de la cadena de bloques.

La dirección del contrato inteligente de administración de consorcios raíz puede verse en Azure Portal. La **dirección de RootContract** está en la sección de información general del miembro de la cadena de bloques.

![Dirección RootContract](./media/consortium/rootcontract-address.png)

Puede interactuar con el contrato inteligente de administración de consorcios mediante el [módulo de PowerShell](manage-consortium-powershell.md) de administración de consorcios, Azure Portal o directamente a través del contrato inteligente mediante la cuenta de Ethereum generada por Azure Blockchain Service.

## <a name="ethereum-account"></a>Cuenta de Ethereum

Cuando se crea un miembro, se crea una clave de cuenta de Ethereum. Azure Blockchain Service usa la clave para crear transacciones relacionadas con la administración de consorcios. La clave de cuenta de Ethereum se administra mediante Azure Blockchain Service automáticamente.

La cuenta de miembro puede verse en Azure Portal. La cuenta del miembro está en la sección de información general del miembro de la cadena de bloques.

![Cuenta del miembro](./media/consortium/member-account.png)

Para restablecer la cuenta de Ethereum, haga clic en la cuenta de miembro y escriba una contraseña nueva. Se restablecerá la dirección de la cuenta de Ethereum y la contraseña.  

## <a name="next-steps"></a>Pasos siguientes

Se puede acceder a las acciones de administración del consorcio a través de PowerShell. Para más información, consulte [Administración de los miembros del consorcio en Azure Blockchain Service mediante PowerShell](manage-consortium-powershell.md).
