---
title: Herramientas de desarrollo de computación confidencial de Azure
description: Use herramientas y bibliotecas para desarrollar aplicaciones para computación confidencial
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: JenCook
ms.openlocfilehash: 6bb3b8dbc7887419f7901a52b56c25f60c869abb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993852"
---
# <a name="application-development-on-intel-sgx"></a>Desarrollo de aplicaciones en Intel SGX 


La infraestructura de computación confidencial requiere herramientas y software específicos. En esta página se habla de manera específica de los conceptos relacionados con el desarrollo de aplicaciones para máquinas virtuales de computación confidencial de Azure que se ejecutan en Intel SGX. Antes de leer esta página, [lea la introducción sobre las máquinas virtuales y los enclaves de Intel SGX](confidential-computing-enclaves.md). 

Para aprovechar la eficacia de los enclaves y los entornos aislados, deberá usar herramientas que admitan la computación confidencial. Hay varias herramientas que admiten el desarrollo de aplicaciones de enclave. Por ejemplo, puede usar estos marcos de código abierto: 

- [Open Enclave Software Development Kit (OE SDK)](#oe-sdk)
- [El marco de trabajo del Consorcio de Computación Confidencial (CCF)](#ccf)

## <a name="overview"></a>Información general

Una aplicación compilada con enclaves se divide en dos sentidos:

1. Un componente que no es de confianza (el host)
1. Un componente que es de confianza (el enclave)


![Desarrollo de aplicaciones](media/application-development/oe-sdk.png)


**El host** es donde se ejecuta la aplicación de enclave y es un entorno que no es de confianza. El host no puede acceder al código del enclave implementado en el host. 

**El enclave** es el lugar en el que se ejecuta el código de aplicación y sus datos o memoria en caché. Los cálculos seguros deben realizarse en los enclaves para garantizar que los secretos y los datos confidenciales estén protegidos. 


Durante el diseño de la aplicación, es importante identificar y determinar qué parte de la aplicación debe ejecutarse en los enclaves. El código que elija incluir en el componente de confianza está aislado del resto de la aplicación. Una vez que se inicializa el enclave y el código se carga en la memoria, ese código no se puede leer ni cambiar desde los componentes que no son de confianza. 

## <a name="open-enclave-software-development-kit-oe-sdk"></a>Open Enclave Software Development Kit (OE SDK) <a id="oe-sdk"></a>

Use una biblioteca o un marco de trabajo compatible con el proveedor si quiere escribir código que se ejecute en un enclave. [Open Enclave SDK](https://github.com/openenclave/openenclave) (OE SDK) es un SDK de código abierto que permite la abstracción sobre diferente hardware habilitado para la computación confidencial. 

OE SDK se compila como una única capa de abstracción en cualquier hardware de cualquier CSP. OE SDK se puede usar sobre las máquinas virtuales de computación confidencial de Azure para crear y ejecutar aplicaciones a partir de los enclaves.

## <a name="confidential-consortium-framework-ccf"></a>Marco de trabajo del Consorcio de Computación Confidencial (CCF) <a id="ccf"></a>

El [CCF](https://github.com/Microsoft/CCF) es una red distribuida de nodos, cada uno de los cuales ejecuta sus propios enclaves. La red de nodos de confianza le permite ejecutar un libro de contabilidad distribuida. El libro de contabilidad proporciona componentes seguros y de confianza para el protocolo que se va a usar. 

![Nodos de CCF](media/application-development/ccf.png)

Este marco de código abierto habilita una elevada confidencialidad específica global y una gobernanza de consorcio para la cadena de bloques. Debido a que cada nodo usa TEE, puede garantizar un consenso y un procesamiento de transacciones seguros.


## <a name="next-steps"></a>Pasos siguientes 
- [Implementación de una máquina virtual de computación confidencial serie DCsv2](quick-create-portal.md)
- [Descarga e instalación de OE SDK y desarrollo de aplicaciones](https://github.com/openenclave/openenclave)