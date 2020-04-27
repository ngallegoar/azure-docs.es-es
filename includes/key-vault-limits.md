---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 04/21/2020
ms.author: jroth
ms.openlocfilehash: 8247b1cedc2c5ebc8577af6be485aed0fcd5d6af
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768742"
---
### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Transacciones clave (n.º máximo de transacciones permitidas en 10 segundos, por almacén y región<sup>1</sup>):

|Tipo de clave|Clave HSM<br>Clave CREATE|Clave HSM<br>Las restantes transacciones|Clave de software<br>Clave CREATE|Clave de software<br>Las restantes transacciones|
|:---|---:|---:|---:|---:|
|2048 bits de RSA|5|1,000|10|2\.000|
|3072 bits de RSA|5|250|10|500|
|4096 bits de RSA|5|125|10|250|
|ECC P-256|5|1,000|10|2\.000|
|ECC P-384|5|1,000|10|2\.000|
|ECC P-521|5|1,000|10|2\.000|
|ECC SECP256K1|5|1,000|10|2\.000|

> [!NOTE]
> En la tabla anterior, vemos que para las claves de software de 2048 bits RSA, se permiten 2000 transacciones GET cada 10 segundos. Para las claves HSM de 2048 bits RSA, se permiten 1000 transacciones GET cada 10 segundos.
>
> Los umbrales de limitación se ponderan y el cumplimiento se basa en su suma. Por ejemplo, como se ha mostrado en la tabla anterior, al realizar operaciones GET en las claves HSM RSA, resulta ocho veces más costoso usar claves de 4096 bits en comparación con las claves de 2048 bits. Eso es porque 1000/125 = 8.
>
> En un intervalo determinado de 10 segundos, un cliente de Azure Key Vault *solo* puede realizar una de las siguientes operaciones antes de encontrar un código de estado HTTP de limitación `429`:
> - 2000 transacciones GET de clave de software de 2048 bits RSA
> - 1000 transacciones GET de clave HSM de 2048 bits RSA
> - 125 transacciones GET de clave HSM de 4096 bits RSA
> - 124 transacciones GET de clave HSM de 4096 bits y 8 transacciones GET de clave HSM de 2048 bits RSA

### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Secretos, claves de cuentas de almacenamiento administradas y transacciones de almacén:

| Tipo de transacciones | N.º máximo de transacciones permitidas en 10 segundos, por almacén y región<sup>1</sup> |
| --- | --- |
| Todas las transacciones |2\.000 |

Vea la [Guía de las limitaciones de Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md) para obtener información sobre cómo controlar la limitación cuando se superan estos límites.

<sup>1</sup> Un límite global para la suscripción para todos los tipos de transacciones es cinco veces el límite del almacén de claves. Por ejemplo, en las otras transacciones HSM por suscripción, el límite es de 5000 transacciones en 10 segundos por suscripción.

### <a name="azure-private-link-integration"></a>Integración de Azure Private Link

| Resource | Límite |
| -------- | ----- |
| Puntos de conexión privados por almacén de claves | 64 |
| Almacenes de claves con puntos de conexión privados por suscripción | 64 |
