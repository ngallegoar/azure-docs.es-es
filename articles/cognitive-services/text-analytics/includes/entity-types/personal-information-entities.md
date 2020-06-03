---
title: Entidades con nombre de información personal
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 57be24142a8504347f420e5780e9621cd2eac91d
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83778159"
---
## <a name="personal-information-entity-types"></a>Tipos de entidades de información personal:

### <a name="person"></a>Person
Reconocimiento de nombres de persona en el texto.

Lenguajes:
* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                               | Disponible a partir de la versión del modelo |
|--------------|-----------------------------------------------------------|----------------------------------------|
| N/D          | Nombres reconocidos, como `Bill Gates` o `Marie Curie` | `2020-02-01`                           |

### <a name="organization"></a>Organización  

Reconocimiento de organizaciones, corporaciones, agencias, compañías, clubs y otros grupos de personas.

Lenguajes: 

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                                                                       | Disponible a partir de la versión del modelo|
|--------------|---------------------------------------------------------------------------------------------------|--------------|
| N/D          | Organizaciones, como `Microsoft`, `NASA`, `National Oceanic and Atmospheric Administration` | `2020-02-01` |

### <a name="phone-number"></a>Número de teléfono

Números de teléfono (solo números de teléfono de EE. UU.). 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                    | Disponible a partir de la versión del modelo |
|--------------|------------------------------------------------|----------------------------------------|
| N/D          | Números de teléfono de EE. UU., como `(312) 555-0176`. | `2020-02-01`                           |

### <a name="email"></a>Email

Dirección de correo electrónico. 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                      | Disponible a partir de la versión del modelo |
|--------------|--------------------------------------------------|----------------------------------------|
| N/D          | Dirección de correo electrónico, por ejemplo `support@contoso.com` | `2020-02-01`                           |

### <a name="url"></a>URL

Direcciones URL de Internet.

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                                          | Disponible a partir de la versión del modelo |
|--------------|------------------------------------------------------|----------------------------------------|
| N/D          | Direcciones URL a sitios web, como `https://www.bing.com`. | `2020-02-01`                           |

### <a name="ip-address"></a>Dirección IP

Dirección de protocolo de Internet

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                              | Disponible a partir de la versión del modelo |
|--------------|------------------------------------------|----------------------------------------|
| N/D          | Dirección de red, como `10.0.0.101` | `2020-02-01`                           |

### <a name="quantity"></a>Cantidad 

Cantidades numéricas

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                   | Disponible a partir de la versión del modelo |
|--------------|-------------------------------|----------------------------------------|
| Age          | `90 days old`, `30 years old` | `2020-02-01`                           |

### <a name="datetime"></a>DateTime

Entidades de fecha y hora

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                   | Disponible a partir de la versión del modelo. |
|--------------|-------------------------------|----------------------------------------|
| Date         | `May 2nd, 2017`, `05/02/2017` | `2020-02-01`                           |

### <a name="eu-gps-coordinates"></a>Coordenadas GPS de la UE

 Coordenadas GPS de ubicaciones de la Unión Europea. 

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo | Descripción                               | Disponible a partir de la versión del modelo |
|--------------|-------------------------------------------|----------------------------------------|
| N/D          | Coordenadas GPS de la Unión Europea | `2019-10-01`                           |

### <a name="azure-information"></a>Información de Azure

Información de Azure identificable, incluida la información de autenticación y las cadenas de conexión. 

* Disponible a partir de la versión del modelo `2019-10-01`

Lenguajes:

* Versión preliminar pública: `English`

| Nombre del subtipo                          | Descripción                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Clave de autorización de Azure DocumentDB             | Clave de autorización para un servidor de Azure DocumentDB.                           |
| Cadena de conexión de base de datos IaaS de Azure | Cadena de conexión de una base de datos de infraestructura como servicio (IaaS) de Azure. |
| Cadena de conexión de Azure SQL           | Cadena de conexión de una instancia de Azure SQL Database.                                |
| Cadena de conexión de Azure IoT           | Cadena de conexión de Azure IoT (Internet de las cosas).                        |
| Contraseña de configuración de publicación de Azure        | Contraseña para la configuración de publicación de Azure.                                        |
| Cadena de conexión de Azure Redis Cache   | Cadena de conexión de una instancia de Azure Cache for Redis.                             |
| SAS de Azure                             | Cadena de conexión de SAS (software como servicio) de Azure Service Bus.                     |
| Cadena de conexión de Azure Service Bus   | Cadena de conexión de una instancia de Azure Service Bus.                                |
| Clave de la cuenta de Azure Storage             | Clave de una cuenta de Azure Storage.                                   |
| Clave de la cuenta de Azure Storage (genérica)   | Clave de cuenta genérica para una cuenta de Azure Storage.                           |
| Cadena de conexión de SQL Server          | Cadena de conexión de una instancia de SQL Server.                                         |

### <a name="identification"></a>Identificación

* Disponible a partir de la versión del modelo `2019-10-01`

Lenguajes:

* Versión preliminar pública: `English`

#### <a name="financial-account-identification"></a>Identificación de cuenta financiera

| Nombre del subtipo               | Descripción                                                                |
|----------------------------|----------------------------------------------------------------------------|
| Números de enrutamiento ABA        | Números de enrutamiento de tránsito de American Banker Association (ABA).                  |
| Código SWIFT                 | Códigos SWIFT de información de instrucciones de pago.                           |
| Tarjeta de crédito                | Números de tarjeta de crédito.                                                       |
| Código IBAN                  | Códigos IBAN de información de instrucciones de pago.                            |

#### <a name="government-and-countryregion-specific-identification"></a>Identificación específica del país o región y del gobierno

Las entidades siguientes se agrupan y enumeran por país o región:

Argentina
* Número de identidad nacional (DNI)

Australia
* Número de expediente fiscal 
* Id. del permiso de conducir
* Id. del pasaporte
* Número de cuenta del seguro médico
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Bélgica
* Número nacional

Brasil
* Número de entidad jurídica (CNPJ)
* Número CPF
* Tarjeta de identificación nacional (RG)

Canadá
* Id. del pasaporte
* Id. del permiso de conducir
* Número de seguro médico
* Número de identificación personal médico (PHIN)
* Número del seguro social
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Chile
* Número del carné de identidad 

China
* Número del carné de identidad
* Número de la tarjeta de identificación de residente (PRC)

Croacia
* Número del carné de identidad
* Número de identificación personal (OIB)

República Checa
* Número de carné de identificación nacional

Dinamarca
* Número de identificación personal

Unión Europea (UE)
* Número de identificación nacional
* Id. del pasaporte
* Id. del permiso de conducir
* Número del seguro social (SSN) o identificador equivalente
* Número de identificación fiscal de la UE (TIN)
* Número de tarjeta de débito de la UE

Finlandia
* Número de identificación nacional
* Id. del pasaporte

Francia
* Tarjeta de identificación nacional (CNI)
* Número del seguro social (INSEE)
* Id. del pasaporte
* Id. del permiso de conducir

Alemania
* Número del carné de identidad
* Id. del pasaporte
* Id. del permiso de conducir

Grecia 
* Número de carné de identificación nacional

RAE de Hong Kong
* Número del carné de identidad (HKID)

India
* Número de cuenta permanente (PAN)
* Número de identificador único (Aadhaar)

Indonesia
* Número del carné de identidad (KTP)

Irlanda
* Número de servicio público personal (PPS)

Israel
* Identificador nacional
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Italia
* Id. del permiso de conducir

Japón
* Número de registro de residente
* Número de la tarjeta de residencia
* Id. del permiso de conducir
* Número del seguro social (SIN)
* Id. del pasaporte
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)

Malasia
* Número del carné de identidad

Países Bajos
* Número de servicio ciudadano (BSN)

Nueva Zelanda
* Número del Ministerio de Sanidad

Noruega
* Número del carné de identidad

Filipinas
* Número de identificación multiuso unificado

Polonia
* Número del carné de identidad
* Identificador nacional (PESEL)
* Id. del pasaporte

Portugal 
* Número de la tarjeta de ciudadano

Arabia Saudí
* Identificador nacional

Singapur
* Número de la tarjeta de identificación de registro nacional (NRIC)

Sudáfrica
* Número de identificación
* Número de registro de residente

Corea del Sur
* Número de registro de residente

España 
* Número de seguridad social

Suecia
* Identificador nacional
* Id. del pasaporte

Taiwán 
* Identificador nacional
* Número de certificado de residente (ARC/TARC)
* Id. del pasaporte

Tailandia
* Código de identificación de población

Reino Unido
* Id. del pasaporte
* Id. del permiso de conducir
* Número del seguro social (NINO)
* Número del NHS (National Health Service)

Estados Unidos
* Número de seguridad social
* Id. del permiso de conducir
* Id. del pasaporte
* Número de lista electoral
* Número de identificación fiscal individual (ITIN)
* Número de la agencia antidroga de Estados Unidos (DEA)
* Números de cuenta bancaria (por ejemplo, cuentas corrientes, de ahorros y de débito)
