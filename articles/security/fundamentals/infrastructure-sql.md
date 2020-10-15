---
title: Características de seguridad de Azure SQL Database
description: En este artículo se proporciona una descripción general de cómo Azure SQL Database protege los datos de los clientes en Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2020
ms.author: terrylan
ms.openlocfilehash: e0e7089e7c674f324c2c3d293661c518b41731b9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84021864"
---
# <a name="azure-sql-database-security-features"></a>Características de seguridad de Azure SQL Database    
Azure SQL Database ofrece un servicio de base de datos relacional en Azure. Para proteger los datos de los clientes y proporcionar características sólidas de seguridad que los clientes esperan de un servicio de base de datos relacional, SQL Database tiene sus propios conjuntos de funcionalidades de seguridad. Estas funcionalidades se basan en los controles que se heredan de Azure.

## <a name="security-capabilities"></a>Funcionalidades de seguridad

### <a name="usage-of-the-tds-protocol"></a>Uso del protocolo TDS
Azure SQL Database solo admite el protocolo de flujo TDS, que requiere que la base de datos sea accesible solo a través del puerto predeterminado de TCP/1433.

### <a name="azure-sql-database-firewall"></a>Firewall de Azure SQL Database
Para ayudar a proteger los datos de los clientes, Azure SQL Database incluye una funcionalidad de firewall que, de forma predeterminada, impide todo acceso a SQL Database, como se muestra a continuación.

![Firewall de Azure SQL Database](./media/infrastructure-sql/sql-database-firewall.png)

El firewall de puerta de enlace puede limitar las direcciones, lo que permite que los clientes tengan control pormenorizado sobre intervalos específicos de direcciones IP aceptables. El firewall otorga acceso según la dirección IP de origen de cada solicitud.

Los clientes pueden configurar el firewall desde un portal de administración o mediante programación con la API REST de Azure SQL Database Management. El firewall de puerta de enlace de Azure SQL Database impide, de forma predeterminada, todo acceso de los clientes a Azure SQL Database. Los clientes deben configurar el acceso mediante listas de control de acceso (ACL) para permitir conexiones de Azure SQL Database por protocolos, números de puertos y direcciones de Internet de origen y destino.

### <a name="dosguard"></a>DoSGuard
Los ataques por denegación de servicio (DoS) se reducen mediante un servicio de puerta de enlace de SQL Database denominado DoSGuard. DoSGuard realiza un seguimiento activo de inicios de sesión erróneos desde direcciones IP. Si hay varios inicios de sesión erróneos desde una dirección IP específica dentro de un período, se bloquea la dirección IP y no podrá acceder a ningún recurso en el servicio durante un período definido previamente.

Además, la puerta de enlace de Azure SQL Database realiza los siguiente:

- Negociaciones de capacidad de canal seguro para implementar conexiones cifradas validadas por TDS FIPS 140-2 al conectarse a los servidores de bases de datos.
- Inspección de paquetes TDS con estado al aceptar conexiones de los clientes. La puerta de enlace valida la información de conexión y pasa los paquetes TDS al servidor físico adecuado según el nombre de la base de datos especificado en la cadena de conexión.

El principio general de seguridad de red de la oferta de Azure SQL Database es permitir únicamente la conexión y la comunicación necesarias para que el servicio funcione. Todos los otros puertos, protocolos y conexiones se bloquean de manera predeterminada. Las redes de área local virtual (VLAN) y las ACL se usan para restringir las comunicaciones de red por protocolos, números de puerto y redes de origen y destino.

Los mecanismos aprobados para implementar las ACL basadas en red incluyen: ACL en enrutadores y equilibradores de carga. Estos mecanismos se administran mediante reglas de firewall de puerta de enlace de Microsoft Azure SQL Database y reglas de firewall de máquina virtual invitada, que están configuradas por el cliente.

## <a name="data-segregation-and-customer-isolation"></a>Segregación de datos y aislamiento de clientes
La red de producción de Azure está estructurada de modo que los componentes del sistema accesibles públicamente estén separados de los recursos internos. Existen límites físicos y lógicos entre los servidores web, lo que proporciona acceso a Azure Portal orientado al público y la infraestructura virtual subyacente de Azure, donde residen las instancias de las aplicaciones de los clientes y los datos de los clientes.

Toda la información accesible públicamente se administra dentro de la red de producción de Azure. La red de producción está sujeta a la autenticación en dos fases y a los mecanismos de protección de límites, usa el firewall y el conjunto de características de seguridad descritos en la sección anterior, y utiliza las funciones de aislamiento de datos, como se indica en las siguientes secciones.

### <a name="unauthorized-systems-and-isolation-of-the-fc"></a>Sistemas no autorizados y aislamiento de FC
Puesto que el controlador de tejido (FC) es el orquestador central del tejido de Azure, existen controles importantes para mitigar las amenazas, sobre todo desde los FA potencialmente comprometidos dentro de las aplicaciones de los clientes. El FC no reconoce ningún hardware cuya información de dispositivo (por ejemplo, la dirección MAC) no esté cargada previamente en el FC. Los servidores DHCP en FC tienen listas configuradas de direcciones MAC de los nodos que están dispuestas a arrancar. Incluso si se conectan sistemas no autorizados, no se incorporan al inventario del tejido y, por lo tanto, no se conectan ni están autorizados para comunicarse con ningún sistema en el inventario del tejido. Esto reduce el riesgo de que sistemas no autorizados se comuniquen con el FC y obtengan acceso a la VLAN y Azure.

### <a name="vlan-isolation"></a>Aislamiento de VLAN
La red de producción de Azure se divide lógicamente en tres redes VLAN principales:

- La red VLAN principal: interconecta nodos de cliente que no son de confianza.
- La red VLAN FC: contiene controladores de tejido (FC) de confianza y sistemas auxiliares.
- La red VLAN de dispositivo: contiene dispositivos de red y otra infraestructura de confianza.

### <a name="packet-filtering"></a>Filtrado de paquetes
El IPFilter y los firewalls de software implementados en el sistema operativo raíz y el sistema operativo invitado de los nodos aplican restricciones de conectividad y evitan el tráfico no autorizado entre las máquinas virtuales.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hipervisor, sistema operativo raíz y máquinas virtuales invitadas
El hipervisor y el sistema operativo raíz administran el aislamiento del sistema operativo raíz de las máquinas virtuales invitadas y de las máquinas virtuales invitadas entre sí.

### <a name="types-of-rules-on-firewalls"></a>Tipos de reglas en firewalls
Una regla se define como:

{IP de Src, puerto de Src, IP de destino, puerto de destino, protocolo de destino, entrada/salida, con estado/sin estado, tiempo de espera de flujo con estado}.

Los paquetes de carácter inactivo síncrono (SYN) solo pueden entrar o salir si alguna de las reglas lo permite. Para TCP, Azure usa reglas sin estado, donde el principio es que solo permite que todos los paquetes que no sean SYN entren o salgan de la máquina virtual. La premisa de seguridad es que toda pila de host sea resistente de omitir un paquete que no sea SYN si no ha visto anteriormente un paquete SYN. El propio protocolo TCP es con estado y, en combinación con la regla sin estado basada en SYN, logra un comportamiento general de una implementación con estado.

Para el Protocolo de datagramas de usuario (UDP), Azure usa una regla con estado. Cada vez que un paquete UDP coincide con una regla, se crea un flujo inverso en la otra dirección. Este flujo tiene un tiempo de espera integrado.

Los clientes son responsables de configurar sus propios firewalls además de lo que ofrece Azure. Aquí los clientes pueden definir las reglas para el tráfico entrante y saliente.

### <a name="production-configuration-management"></a>Administración de la configuración de producción
Los equipos de operaciones respectivos en Azure y Azure SQL Database se encargan del mantenimiento de las configuraciones de seguridad estándar. Todos los cambios de configuración en los sistemas de producción se documentan y registran a través de un sistema de seguimiento central. Los cambios de hardware y software se registran a través del sistema de seguimiento central. Los cambios de red relacionados con ACL se registran mediante un servicio de administración de ACL.

Todos los cambios de configuración en Azure se desarrollan y prueban en el entorno de ensayo y, más adelante, se implementan en el entorno de producción. Las compilaciones de software se revisan como parte de las pruebas. Las comprobaciones de seguridad y privacidad se revisan como parte de los criterios de la lista de comprobación de entrada. El equipo de implementación correspondiente implementa los cambios en intervalos programados. El personal del equipo de implementación correspondiente revisa y aprueba las versiones antes de implementarlas en producción.

Los cambios se supervisan para comprobar que sean correctos. En un escenario de error, el cambio se revierte a su estado anterior o se implementa una revisión para corregir el error con la aprobación del personal designado. Source Depot, Git, TFS, Master Data Services (MDS), Runners, supervisión de seguridad de Azure, el FC y la plataforma de WinFabric se usan para administrar, aplicar y comprobar de manera centralizada las opciones de configuración en el entorno virtual de Azure.

De forma similar, los cambios de hardware y de red cuentan con pasos de validación establecidos para evaluar el cumplimiento de los requisitos de compilación. Las versiones se revisan y autorizan a través de un comité de evaluación de cambios (CAB) coordinado de los respectivos grupos a través de la pila.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](physical-security.md)
- [Disponibilidad de la infraestructura de Azure](infrastructure-availability.md)
- [Componentes y límites del sistema de información de Azure](infrastructure-components.md)
- [Arquitectura de red de Azure](infrastructure-network.md)
- [Red de producción de Azure](production-network.md)
- [Operaciones de producción y administración de Azure](infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](protection-customer-data.md)
