---
title: Seguridad de la capa de transporte en Azure Backup
description: Obtenga información acerca de cómo habilitar Azure Backup para usar el protocolo de cifrado Seguridad de la capa de transporte (TLS) para proteger los datos cuando se transfieren a través de una red.
ms.topic: conceptual
ms.date: 11/01/2020
ms.openlocfilehash: 9881fd3532cbc7d67c7d5adbce75e02fc62e0bcf
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280656"
---
# <a name="transport-layer-security-in-azure-backup"></a>Seguridad de la capa de transporte en Azure Backup

Seguridad de la capa de transporte (TLS) es un protocolo de cifrado que protege los datos cuando se transfieren a través de una red. Azure Backup utiliza el protocolo Seguridad de la capa de transporte para proteger la privacidad de los datos de copia de seguridad que se transfieren. En este artículo se describen los pasos para habilitar el protocolo TLS 1.2, que proporciona seguridad mejorada con respecto a las versiones anteriores.

## <a name="earlier-versions-of-windows"></a>Versiones anteriores de Windows

Si el equipo ejecuta versiones anteriores de Windows, se deben instalar las actualizaciones correspondientes que se indican a continuación y se deben aplicar los cambios de registro documentados en los artículos de KB.

|Sistema operativo  |artículo de KB |
|---------|---------|
|Windows Server 2008 SP2   |   <https://support.microsoft.com/help/4019276>      |
|Windows Server 2008 R2, Windows 7, Windows Server 2012   | <https://support.microsoft.com/help/3140245>         |

>[!NOTE]
>La actualización instalará los componentes de protocolo necesarios. Después de la instalación, debe realizar los cambios en la clave del Registro mencionados en los artículos de KB anteriores para habilitar correctamente los protocolos necesarios.

## <a name="verify-windows-registry"></a>Comprobación del Registro de Windows

### <a name="configuring-schannel-protocols"></a>Configuración de los protocolos de SChannel

Las siguientes claves del Registro garantizan que el protocolo TLS 1.2 está habilitado en el nivel de componente de SChannel:

```reg
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]
    "Enabled"=dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client]
    "DisabledByDefault"=dword:00000000
```

>[!NOTE]
>Los valores que se muestran se establecen de forma predeterminada en Windows Server 2012 R2 y versiones más recientes. En el caso de estas versiones de Windows, si las claves del Registro no están presentes, no es necesario crearlas.

### <a name="configuring-net-framework"></a>Configuración de .NET Framework

Las claves del Registro siguientes configuran .NET Framework para que admita la criptografía segura. Puede obtener más información sobre la [configuración de .NET Framework aquí](https://docs.microsoft.com/dotnet/framework/network-programming/tls#configuring-schannel-protocols-in-the-windows-registry).

```reg
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
    "SystemDefaultTlsVersions"=dword:00000001
    "SchUseStrongCrypto" = dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
    "SystemDefaultTlsVersions"=dword:00000001
    "SchUseStrongCrypto" = dword:00000001
```

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="why-enable-tls-12"></a>¿Por qué habilitar TLS 1.2?

TLS 1.2 es más seguro que los protocolos criptográficos anteriores, como SSL 2.0, SSL 3.0, TLS 1.0 y TLS 1.1. Los servicios de Azure Backup ya admiten totalmente TLS 1.2.

### <a name="what-determines-the-encryption-protocol-used"></a>¿Qué determina el protocolo de cifrado usado?

La versión de protocolo más alta admitida por el cliente y el servidor se negocia para establecer la conversación cifrada. Para obtener más información sobre el protocolo de enlace TLS, vea [Establecimiento de una sesión segura mediante TLS](https://docs.microsoft.com/windows/win32/secauthn/tls-handshake-protocol#establishing-a-secure-session-by-using-tls).

### <a name="what-is-the-impact-of-not-enabling-tls-12"></a>¿Cuál es el impacto de no habilitar TLS 1.2?

Para mejorar la seguridad de los ataques por degradación del protocolo, Azure Backup está empezando a deshabilitar las versiones de TLS anteriores a 1.2 de forma escalonada. Esto forma parte de un cambio a largo plazo en los servicios para prohibir las conexiones de protocolos heredados y conjuntos de cifrado. Los servicios y los componentes de Azure Backup son totalmente compatibles con TLS 1.2. Sin embargo, las versiones de Windows que carecen de actualizaciones necesarias o de ciertas configuraciones personalizadas pueden seguir impidiendo la oferta de protocolos TLS 1.2. Esto puede causar errores, incluidos, entre otros, uno o varios de los siguientes:

- Posibles errores en las operaciones de copia de seguridad y restauración.
- Errores de conexión de componentes de copia de seguridad 10054 (el host remoto ha cerrado forzosamente una conexión existente).
- Los servicios relacionados con Azure Backup no se detendrán o iniciarán del modo habitual.

## <a name="additional-resources"></a>Recursos adicionales

- [Protocolo Seguridad de la capa de transporte](https://docs.microsoft.com/windows/win32/secauthn/transport-layer-security-protocol)
- [Garantía de la compatibilidad con TLS 1.2 en sistemas operativos implementados](https://docs.microsoft.com/security/engineering/solving-tls1-problem#ensuring-support-for-tls-12-across-deployed-operating-systems)
- Para obtener más información, consulte [Procedimientos recomendados sobre la seguridad de la capa de transporte (TLS) con .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/tls).
