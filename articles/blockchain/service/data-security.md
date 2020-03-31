---
title: Seguridad de Azure Blockchain Service
description: Conceptos de acceso a datos y seguridad de Azure Blockchain Service
ms.date: 11/22/2019
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 06bf4e0fa4037b07505a4f816fc7af56c14576d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75982226"
---
# <a name="azure-blockchain-service-security"></a>Seguridad de Azure Blockchain Service

Azure Blockchain Service usa varias capacidades de Azure para mantener los datos seguros y disponibles. Los datos se protegen mediante aislamiento, cifrado y autenticación.

## <a name="isolation"></a>Aislamiento

Los recursos de Azure Blockchain Service están aislados en una red virtual privada. Cada nodo de transacción y validación es una máquina virtual (VM). Las máquinas virtuales de una red virtual no se pueden comunicar directamente con las máquinas virtuales de una red virtual diferente. El aislamiento garantiza que la comunicación siga siendo privada dentro de la red virtual. Para obtener más información sobre el aislamiento de la red virtual de Azure, consulte el [aislamiento en la nube pública de Azure](../../security/fundamentals/isolation-choices.md#networking-isolation).

![Diagrama de VNET](./media/data-security/vnet.png)

## <a name="encryption"></a>Cifrado

Los datos de usuario se almacenan en Azure Storage. Los datos de usuario se cifran en movimiento y en reposo por motivos de seguridad y confidencialidad. Para más información, consulte: [Guía de seguridad de Azure Storage](../../storage/blobs/security-recommendations.md).

## <a name="authentication"></a>Authentication

Las transacciones se pueden enviar a los nodos de cadena de bloques a través de un punto de conexión RPC. Los clientes se comunican con un nodo de transacción mediante un servidor proxy inverso que controla la autenticación de usuario y cifra los datos a través de SSL.

![Diagrama de autenticación](./media/data-security/authentication.png)

Existen tres modos de autenticación para el acceso RPC.

### <a name="basic-authentication"></a>Autenticación básica

La autenticación básica utiliza un encabezado de autenticación HTTP que contiene el nombre de usuario y la contraseña. El nombre de usuario es el nombre del nodo de la cadena de bloques. La contraseña se establece durante el aprovisionamiento de un miembro o un nodo. La contraseña se puede cambiar desde Azure Portal o desde la CLI.

### <a name="access-keys"></a>Claves de acceso

Las claves de acceso usan una cadena generada aleatoriamente que se incluye en la dirección URL del punto de conexión. Dos claves de acceso ayudan a permitir la rotación de claves. Las claves se pueden volver a generar desde Azure Portal y la CLI.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory (Azure AD) usa un mecanismo de autenticación basado en notificaciones donde el usuario se autentica en Azure AD utilizando las credenciales de usuario de Azure AD. Azure AD proporciona la administración de identidades basada en la nube y permite a los clientes usar una identidad única en toda una empresa y las aplicaciones de acceso en la nube. Azure Blockchain Service se integra con Azure AD mediante la habilitación del id. de federación, el inicio de sesión único y la autenticación multifactor. Puede asignar usuarios, grupos y roles de aplicación de su organización para el acceso de nodo y miembro de cadena de bloques.

El proxy de cliente de Azure AD está disponible en [GitHub](https://github.com/Microsoft/azure-blockchain-connector/releases). El proxy de cliente dirige al usuario a la página de inicio de sesión de Azure AD y obtiene un token de portador tras la autenticación correcta. Posteriormente, el usuario conecta una aplicación de cliente de Ethereum, como Geth o Truffle, al punto de conexión del proxy de cliente. Por último, cuando se envía una transacción, el proxy de cliente inyecta el token de portador en el encabezado HTTP y el proxy inverso valida el token mediante el protocolo OAuth.

## <a name="keys-and-ethereum-accounts"></a>Claves y cuentas de Ethereum

Al aprovisionar un miembro de Azure Blockchain Service, se genera una cuenta de Ethereum y un par de claves pública y privada. La clave privada se utiliza para enviar las transacciones a la cadena de bloques. La cuenta de Ethereum es los últimos 20 bytes del hash de la clave pública. La cuenta de Ethereum también se denomina cartera.

El par de claves pública y privada se almacena como un archivo de clave en formato JSON. La clave privada se cifra con la contraseña que escribió al crear el servicio de libro de contabilidad de la cadena de bloques.

Las claves privadas se utilizan para firmar digitalmente las transacciones. En las cadenas de bloques privadas, un contrato inteligente firmado por una clave privada representa la identidad del firmante. Para comprobar la validez de la firma, el receptor puede comparar la clave pública del firmante con la dirección que se calcula a partir de la firma.

Las claves de constelación se usan para identificar de forma única un nodo de Quorum. Las claves de constelación se generan en el momento de aprovisionamiento de nodo y se especifican en el parámetro privateFor de una transacción privada en Quorum.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Configuración del acceso a Azure Active Directory](configure-aad.md).
