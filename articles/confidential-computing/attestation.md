---
title: Atestación de enclaves en Azure
description: Obtenga información sobre cómo usar la atestación para comprobar que el entorno de confianza de computación confidencial es seguro
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 9/22/2020
ms.author: JenCook
ms.openlocfilehash: 70a17aacde67744eae74ca263200f2c65fbd300a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993849"
---
# <a name="attesting-sgx-enclaves"></a>Atestación de enclaves de SGX

La computación confidencial en Azure ofrece máquinas virtuales basadas en Intel SGX que pueden aislar una parte del código o los datos. Al trabajar con estos [enclaves](confidential-computing-enclaves.md), se recomienda lograr la comprobación y la validación de que el entorno de confianza es seguro. Esta comprobación es el proceso de atestación. 

## <a name="overview"></a>Información general 

La atestación permite que un usuario de confianza tenga mayor seguridad de que el software (1) se ejecuta en un enclave y (2) que el enclave está actualizado y es seguro. Por ejemplo, un enclave pide al hardware subyacente que genere una credencial que incluye la prueba de que el enclave existe en la plataforma. Luego, se puede dar al informe un segundo enclave que comprueba que el informe se generó en la misma plataforma.

![Código de atestación en el enclave](media/attestation/attestation.png)



La atestación debe implementarse mediante un servicio de atestación seguro que sea compatible con el software del sistema y el silicio. Algunos ejemplos de servicios que puede usar son

- [Microsoft Azure Attestation (versión preliminar)](https://docs.microsoft.com/azure/attestation/overview) o
- [Servicios de atestación y aprovisionamiento de Intel](https://software.intel.com/sgx/attestation-services),


ambos compatibles con la infraestructura de Intel SGX de computación confidencial de Azure. 

## <a name="next-steps"></a>Pasos siguientes
Pruebe los [ejemplos de Microsoft Azure Attestation para aplicaciones compatibles con enclaves](https://docs.microsoft.com/samples/azure-samples/microsoft-azure-attestation/sample-code-for-intel-sgx-attestation-using-microsoft-azure-attestation/).