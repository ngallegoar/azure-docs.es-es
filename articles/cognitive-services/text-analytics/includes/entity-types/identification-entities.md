---
title: Entidades de identificación
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: f07b71bf8996612798b87d32a21a15ec72db0b32
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140955"
---
Esta categoría de entidad incluye información financiera y formas oficiales de identificación. Disponible a partir de la versión del modelo `2019-10-01` A continuación se enumeran los subtipos. 

### <a name="financial-account-identification"></a>Identificación de cuenta financiera

| Nombre del subtipo               | Descripción                                                                |
|----------------------------|----------------------------------------------------------------------------|
| Número de enrutamiento ABA        | Números de enrutamiento de tránsito de American Banker Association (ABA).                  |
| Código SWIFT                 | Códigos SWIFT de información de instrucciones de pago.                           |
| Tarjeta de crédito                | Números de tarjeta de crédito.                                                       |
| Número de cuenta bancaria internacional (IBAN)                  | Códigos IBAN de información de instrucciones de pago.                            |


### <a name="government-and-countryregion-specific-identification"></a>Identificación específica del país o región y del gobierno

> [!NOTE]
> Las entidades financieras y específicas del país siguientes no se devuelven con el parámetro `domain=phi`:
> * Números de pasaporte
> * Id. de impuesto

Las entidades siguientes se agrupan y enumeran por país:

Argentina
* Número de identidad nacional de Argentina (DNI)

Australia
* Número de pasaporte de Australia
* Número de expediente fiscal de Australia
* Número de permiso de conducir de Australia
* Número de cuenta del seguro médico de Australia
* Número de cuenta bancaria de Australia

Bélgica
* Número nacional de Bélgica

Brasil 
* Número de entidad jurídica de Brasil (CNPJ)
* Número de CPF de Brasil
* Tarjeta de identificación nacional de Brasil (RG)

Canadá
* Número del seguro social de Canadá
* Número de permiso de conducir de Canadá
* Número de cuenta bancaria de Canadá
* Número de pasaporte de Canadá
* Número de identificación sanitaria personal de Canadá (PHIN)
* Número del servicio de salud de Canadá

Chile
* Número del carné de identidad 

China
* Número del carné de identidad de residente de China (PRC)

Croacia
* Número del carné de identidad de Croacia
* Número de identificación personal de Croacia (OIB)

República Checa
* Número de identidad personal de la República Checa

Dinamarca
* Número de identificación personal de Dinamarca

Unión Europea (UE)
* Número de identificación nacional de la UE
* Número de pasaporte de la UE
* Número de permiso de conducir de la UE
* Número del seguro social (SSN) o identificador equivalente de la UE
* Número de identificación fiscal de la UE (TIN)
* Número de tarjeta de débito de la UE

Finlandia
* Documento nacional de identidad de Finlandia
* Número de pasaporte de Finlandia

Francia
* Carné de identidad nacional de Francia (CNI)
* Número del seguro social de Francia (INSEE)
* Número de pasaporte de Francia
* Número de permiso de conducir de Francia

Alemania
* Número del carné de identidad de Alemania
* Número de pasaporte de Alemania
* Número de permiso de conducir de Alemania

Grecia 
* Número de carné de identificación nacional de Grecia

RAE de Hong Kong
* Número del carné de identidad de Hong Kong (HKID)

India
* Número de cuenta permanente de India (PAN)
* Número de identificación única de India (Aadhaar)

Indonesia
* Número del carné de identidad de Indonesia (KTP)

Irlanda
* Número de servicio público personal de Irlanda (PPS)

Israel
* Identificación nacional
* Número de cuenta bancaria de Israel

Italia
* Id. del permiso de conducir de Italia

Japón
* Número de registro de residente de Japón
* Número de la tarjeta de residencia japonesa
* Número de permiso de conducir de Japón
* Número del seguro social (SIN)
* Número de pasaporte de Japón
* Número de cuenta bancaria de Japón

Malasia
* Número del carné de identidad de Malasia

Países Bajos
* Número de servicio ciudadano de Países Bajos (BSN)

Nueva Zelanda
* Número del Ministerio de Sanidad de Nueva Zelanda

Noruega
* Número de identidad de Noruega

Filipinas
* Número de identificación multiuso unificado de Filipinas

Polonia
* Carné de identidad de Polonia
* Identificador nacional de Polonia (PESEL)
* Pasaporte de Polonia

Portugal 
* Número de la tarjeta de ciudadano de Portugal

Arabia Saudí
* Identificador nacional de Arabia Saudí

Singapur
* Número de la tarjeta de identificación de registro nacional de Singapur (NRIC)

Sudáfrica
* Número de identificación de Sudáfrica

Corea del Sur
* Número de registro de residente de Corea del Sur

España 
* Número de seguridad social de España (SSN)

Suecia
* Identificación nacional de Suecia
* Número de pasaporte de Suecia

Taiwán 
* Identificación nacional de Ta
* Certificado de residente de Taiwán (ARC/TARC)
* Número de pasaporte de Taiwán

Tailandia
* Código de identificación de población tailandés

Reino Unido
* Id. del pasaporte
* Reino Unido Número de permiso de conducir
* Reino Unido Número del seguro social (NINO)
* Reino Unido Número del servicio de salud nacional
* Reino Unido Número de lista electoral
* EE. UU./Reino Unido Número de pasaporte

Estados Unidos
* EE. UU. Número de seguridad social
* EE. UU. Número de permiso de conducir
* EE. UU./Reino Unido Número de pasaporte
* EE. UU. Número de identificación fiscal (NIF) individual
* Número de la agencia antidroga de Estados Unidos (DEA)
* Número de cuenta bancaria de EE. UU.
