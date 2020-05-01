---
title: ¿Qué es Dedicated HSM? - Azure Dedicated HSM | Microsoft Docs
description: La introducción a Azure Dedicated HSM ofrece funcionalidades de almacenamiento de claves en Azure que cumplen con la certificación FIPS 140-2 nivel 3.
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: dd5ce117645ef2b368bbf8f0e441770d6e746b5b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "70881080"
---
# <a name="what-is-azure-dedicated-hsm"></a>¿Qué es Azure Dedicated HSM?

Azure Dedicated HSM ofrece almacenamiento de claves criptográficas en Azure. Dedicated HSM cumple los requisitos de seguridad más estrictos. Es la solución idónea para los clientes que necesitan dispositivos validados con la certificación FIPS 140-2 nivel 3 y un control completo y exclusivo del dispositivo HSM. 

 Los dispositivos HSM se implementan de manera global en varias regiones de Azure. Se pueden aprovisionar fácilmente como un par de dispositivos y configurar para alta disponibilidad. Los dispositivos HSM también se pueden aprovisionar entre regiones para protección frente a conmutaciones por error en el nivel regional. Microsoft entrega el servicio Dedicated HSM mediante la aplicación [SafeNet Luna Network HSM 7 (Modelo A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) de Gemalto. Este dispositivo ofrece los mayores niveles de rendimiento y opciones de integración criptográficas. 

Una vez aprovisionados, los dispositivos HSM se conectan directamente a la red virtual de un cliente. También son accesibles mediante una aplicación local y herramientas de administración si se configura la conectividad de punto a sitio o de VPN de sitio a sitio. Los clientes obtienen el software y la documentación para configurar y administrar los dispositivos HSM desde el portal de soporte técnico de Gemalto.

## <a name="why-use-azure-dedicated-hsm"></a>Motivos para usar Azure Dedicated HSM

### <a name="fips-140-2-level-3-compliance"></a>Cumplimiento de la norma FIPS 140-2 nivel 3

Muchas organizaciones tienen estrictas regulaciones normativas del sector que dictan que el almacenamiento de claves criptográfico debe cumplir con los requisitos de [FIPS 140-2 nivel 3](https://csrc.nist.gov/publications/detail/fips/140/2/final). El servicio multiinquilino Azure Key Vault de Microsoft solo proporciona actualmente la certificación FIPS 140-2 nivel 2. Azure Dedicated HSM viene a dar respuesta a una necesidad real que tiene el sector de los servicios financieros, las agencias gubernamentales y otros destinatarios que deben cumplir con los requisitos de la certificación FIPS 140-2 nivel 3.

### <a name="single-tenant-devices"></a>Dispositivos de un único inquilino

Muchos de nuestros clientes requieren un único inquilino en el dispositivo de almacenamiento criptográfico. El servicio Azure Dedicated HSM les permite aprovisionar un dispositivo físico desde uno de los centros de datos distribuidos globalmente de Microsoft. Una vez que se aprovisiona a un cliente, solo ese cliente puede acceder al dispositivo.

### <a name="full-administrative-control"></a>Control administrativo completo

Muchos clientes requieren un control administrativo completo y acceso exclusivo a su dispositivo para fines administrativos. Una vez que se aprovisiona un dispositivo, solo el cliente tiene acceso de nivel administrativo o de aplicación al mismo.

 Microsoft no tiene control administrativo una vez que el cliente accede por primera vez al dispositivo, momento en el cual el cliente cambia la contraseña. A partir de ese punto, el cliente es un único inquilino verdadero con control administrativo completo y funcionalidad de administración de aplicaciones. Microsoft sí conserva el acceso a nivel de supervisión (no un rol de administrador) para la telemetría a través de la conexión de puerto serie. Este acceso abarca indicadores de hardware, como temperatura, estado del suministro eléctrico y estado del ventilador. 
 
 Si lo desea, el cliente puede deshabilitar esta supervisión necesaria pero, si lo hace, no recibirá alertas de estado proactivas de parte de Microsoft.

### <a name="high-performance"></a>Alto rendimiento

Para este servicio, se seleccionó el dispositivo Gemalto por diversos motivos. Ofrece una amplia variedad de compatibilidad con algoritmos criptográficos, una variedad de sistemas operativos compatibles y una amplia compatibilidad con API. El modelo específico implementado ofrece un rendimiento excelente con 10 000 operaciones por segundo para RSA-2048. Admite 10 particiones que se pueden usar para las instancias de aplicaciones únicas. Este dispositivo es de baja latencia, alta capacidad y alto rendimiento.

### <a name="unique-cloud-based-offering"></a>Oferta única basada en la nube

Microsoft reconoció una necesidad concreta en un conjunto exclusivo de clientes y es el único proveedor de servicios en la nube que ofrece un servicio HSM que cumple con la certificación FIPS 140-2 nivel 3 y que ofrece una integración tan amplia de aplicaciones basadas en la nube y locales.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>¿Es Azure Dedicated HSM adecuado para usted?

Azure Dedicated HSM es un servicio especializado que permite cumplir los requisitos únicos de un tipo específico de organización a gran escala. Como resultado, es de esperar que la mayoría de los clientes de Azure no se encuadren en el perfil de cliente que usará este servicio. Muchas descubrirán que el servicio Azure Key Vault es más adecuado y rentable. Para ayudarlo a decidir si es adecuado para sus requisitos, hemos identificado los siguientes criterios.

### <a name="best-fit"></a>Casos en los que está indicado

Azure Dedicated HSM es el más adecuado para escenarios de "migración mediante lift-and-shift" que requieren un acceso directo y exclusivo a los dispositivos HSM. Algunos ejemplos son:

- Migración de aplicaciones locales a Azure Virtual Machines
- Migración de aplicaciones de Amazon AWS EC2 a máquinas virtuales que usan el servicio AWS Cloud HSM Classic (Amazon no ofrece este servicio para los nuevos clientes)
- Ejecución de un software empaquetado como Apache/Ngnix SSL Offload, Oracle TDE y ADCS en Azure Virtual Machines 

### <a name="not-a-fit"></a>Casos en los que no está indicado

Azure Dedicated HSM no es idóneo para el siguiente tipo de escenario: Los servicios en la nube de Microsoft que admiten el cifrado con claves administradas por los clientes (como Azure Information Protection, Azure Disk Encryption, Azure Data Lake Storage, Azure Storage, Azure SQL Database y clave de cliente para Office 365) no están integrados con Azure Dedicated HSM.

### <a name="it-depends"></a>Depende

Si Azure Dedicated HSM funcionará para usted depende de una posible combinación compleja de requisitos y compromisos que puede o no asumir. Un ejemplo es el requisito de la certificación FIPS 140-2 nivel 3. Este requisito es común y Dedicated HSM es actualmente la única opción para cumplirlo. Si estos requisitos obligatorios no son pertinentes, a menudo es una elección entre Azure Key Vault y Dedicated HSM. Evalúe sus requisitos antes de tomar una decisión.

Las situaciones en las que tendrá que ponderar sus opciones incluyen las siguientes: 

- Nuevo código que se ejecuta en una máquina virtual de Azure del cliente
- TDE de SQL Server en una máquina virtual de Azure
- Cifrado en el lado de cliente de Azure Storage
- SQL Server y Azure SQL Database Always Encrypted

## <a name="next-steps"></a>Pasos siguientes

Se trata de un servicio altamente especializado. Por lo tanto, se recomienda que comprenda totalmente los conceptos clave de este conjunto de documentación, incluidos los precios, el soporte técnico y los contratos de nivel de servicio. 

Las [guías de integración de Gemalto](https://safenet.gemalto.com/partners/microsoft/) le permiten facilitar el aprovisionamiento de dispositivos HSM en un entorno de red virtual existente. También hay guías paso a paso para ayudarlo a determinar cómo configurar la arquitectura de la implementación.

* [Alta disponibilidad](high-availability.md)
* [Seguridad física](physical-security.md)
* [Redes](networking.md)
* [Compatibilidad](supportability.md)
* [Supervisión](monitoring.md)
