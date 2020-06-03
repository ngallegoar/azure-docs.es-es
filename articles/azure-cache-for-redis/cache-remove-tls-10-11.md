---
title: Quitar TLS 1.0 y 1.1 del uso de Azure Cache for Redis
description: Obtenga información sobre cómo quitar TLS 1.0 y 1.1 de la aplicación al comunicarse con Azure Cache for Redis
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: yegu
ms.openlocfilehash: b7b3556896f2d8bb8fea7ffc4543356e248df60d
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848828"
---
# <a name="remove-tls-10-and-11-from-use-with-azure-cache-for-redis"></a>Quitar TLS 1.0 y 1.1 del uso de Azure Cache for Redis

Hay un envío de cambios que afecta a todo el sector hacia el uso exclusivo de la Seguridad de la capa de transporte (TLS) versión 1.2 o posterior. Las versiones de TLS 1.0 y 1.1 son susceptibles de sufrir ataques como BEAST y POODLE, y tienen otros puntos vulnerables y exposiciones comunes (CVE). Tampoco admiten los métodos de cifrado modernos y los conjuntos de cifrado recomendados por las normas de cumplimiento del sector de las tarjetas de pago (PCI). En este [blog de seguridad de TLS](https://www.acunetix.com/blog/articles/tls-vulnerabilities-attacks-final-part/) se explican con más detalle algunas de estas vulnerabilidades.

Como parte de este trabajo, realizaremos los siguientes cambios en Azure Cache for Redis:

* **Fase 1:** : la versión de TLS mínima predeterminada configurada será la 1.2 para las instancias de caché recién creadas (anteriormente TLS 1.0).  Las instancias de caché existentes no se actualizarán en este momento. Se le permitirá [cambiar la versión de TLS mínima](cache-configure.md#access-ports) a 1.0 o 1.1 por compatibilidad con versiones anteriores, si es necesario. Este cambio se puede realizar mediante Azure Portal u otras API de administración.
* **Fase 2:** se dejarán de admitir las versiones de TLS 1.0 y 1.1. Después de ese cambio, la aplicación deberá usar TLS 1.2 o posterior para comunicarse con la caché.

Además, como parte de este cambio, quitaremos la compatibilidad con los conjuntos de cifrado más antiguos y poco seguros.  Los conjuntos de cifrado compatibles se limitarán a los siguiente cuando la caché se configure con una versión de TLS mínima de 1.2.

* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256

En este artículo se proporcionan instrucciones generales sobre cómo detectar dependencias en estas versiones anteriores de TLS y cómo quitarlas de la aplicación.

Las fechas en las que estos cambios surten efecto son:

| Nube                | Fecha de inicio de Fase 1 | Fecha de inicio de Fase 2      |
|----------------------|--------------------|-------------------------|
| Azure (global)       |  13 de enero de 2020  | 11 de mayo de 2020            |
| Azure Government     |  13 de marzo de 2020    | 11 de mayo de 2020            |
| Azure Alemania        |  13 de marzo de 2020    | 11 de mayo de 2020            |
| Azure China 21Vianet |  13 de marzo de 2020    | 11 de mayo de 2020            |

## <a name="check-whether-your-application-is-already-compliant"></a>Compruebe si la aplicación ya es compatible

La manera más sencilla de averiguar si la aplicación funcionará con TLS 1.2 es establecer el valor de la **versión mínima de TLS** en TLS 1.2 en una caché de prueba o de almacenamiento provisional que use. La configuración de la **versión mínima de TLS** está en la [Configuración avanzada](cache-configure.md#advanced-settings) de la instancia de caché en Azure Portal. Si la aplicación sigue funcionando según lo esperado después de este cambio, es probable que sea compatible. Es posible que tenga que configurar algunas bibliotecas de cliente de Redis que usa la aplicación específicamente para habilitar TLS 1.2, de modo que puedan conectarse a Azure Cache for Redis a través de ese protocolo de seguridad.

## <a name="configure-your-application-to-use-tls-12"></a>Configurar la aplicación para usar TLS 1.2

La mayoría de las aplicaciones usan bibliotecas de cliente de Redis para controlar la comunicación con sus cachés. A continuación se proporcionan instrucciones para configurar algunas de las bibliotecas de cliente más populares, en diversos marcos y lenguajes de programación, para usar TLS 1.2.

### <a name="net-framework"></a>.NET Framework

Los clientes .NET de Redis usan la versión de TLS más antigua de forma predeterminada en .NET Framework 4.5.2 o versiones anteriores, y usan la versión más reciente de TLS en .NET Framework 4.6 o posterior. Si utiliza una versión anterior de .NET Framework, puede habilitar TLS 1.2 de forma manual:

* **StackExchange.Redis:** Establezca `ssl=true` y `sslprotocols=tls12` en la cadena de conexión.
* **ServiceStack.Redis:** siga las instrucciones de [ServiceStack.Redis](https://github.com/ServiceStack/ServiceStack.Redis#servicestackredis-ssl-support) y requiere ServiceStack.Redis v5.6, como mínimo.

### <a name="net-core"></a>.NET Core

Los clientes .NET Core de Redis usan de forma predeterminada la versión de TLS predeterminada del sistema operativo, que obviamente depende del propio sistema operativo. 

En función de cuándo se haya lanzado el sistema operativo y de si cualquier otra revisión ha cambiado la versión de TLS predeterminada, la versión de TLS del sistema operativo podría variar bastante. Aunque no hay información completa sobre esto, para el sistema operativo Windows en concreto se puede encontrar más información [aquí](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12). 

Pero, si se usa un sistema operativo antiguo o simplemente quiere asegurarse, se recomienda configurar manualmente la versión de TLS preferida a través del cliente.


### <a name="java"></a>Java

Los clientes de Java de Redis usan TLS 1.0, versión 6 o anterior de Java. Jedis, Lettuce y Redisson no se pueden conectar a Azure Cache for Redis si TLS 1.0 está deshabilitado en la memoria caché. Actualice el marco de Java para usar las nuevas versiones de TLS.

En Java 7, los clientes de Redis no usan TLS 1.2 de forma predeterminada, pero se pueden configurar para ello. Jedis permite especificar la configuración de TLS subyacente con el siguiente fragmento de código:

``` Java
SSLSocketFactory sslSocketFactory = (SSLSocketFactory) SSLSocketFactory.getDefault();
SSLParameters sslParameters = new SSLParameters();
sslParameters.setEndpointIdentificationAlgorithm("HTTPS");
sslParameters.setProtocols(new String[]{"TLSv1.2"});
 
URI uri = URI.create("rediss://host:port");
JedisShardInfo shardInfo = new JedisShardInfo(uri, sslSocketFactory, sslParameters, null);
 
shardInfo.setPassword("cachePassword");
 
Jedis jedis = new Jedis(shardInfo);
```

Los clientes de Lettuce y Redisson todavía no admiten la especificación de la versión de TLS, por lo que se interrumpirán si la caché acepta solo conexiones TLS 1.2. Se están revisando las correcciones de estos clientes, por lo que debe comprobar si hay una versión actualizada para esos paquetes con esta compatibilidad.

En Java 8, se usa TLS 1.2 de forma predeterminada y no es necesario actualizar la configuración del cliente en la mayoría de los casos. Para estar más seguro, pruebe la aplicación.

### <a name="nodejs"></a>Node.js

Los nodos de Redis e IORedis usan TLS 1.2 de forma predeterminada.

### <a name="php"></a>PHP

#### <a name="predis"></a>Predis
 
* Versiones anteriores a PHP 7: Predis solo admite TLS 1.0. Estas versiones no funcionan con TLS 1.2; debe actualizar para usar TLS 1.2.
 
* PHP 7.0 a PHP 7.2.1: Predis solo usa TLS 1.0 o 1.1 de forma predeterminada. Puede usar la siguiente solución alternativa para usar TLS 1.2. Puede especificar TLS 1.2 al crear la instancia de cliente:

  ``` PHP
  $redis=newPredis\Client([
      'scheme'=>'tls',
      'host'=>'host',
      'port'=>6380,
      'password'=>'password',
      'ssl'=>[
          'crypto_type'=>STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT,
      ],
  ]);
  ```

* PHP 7.3 y versiones posteriores: Predis usa la versión más reciente de TLS.

#### <a name="phpredis"></a>PhpRedis

PhpRedis no admite TLS en ninguna versión de PHP.

### <a name="python"></a>Python

Redis-py usa TLS 1.2 de forma predeterminada.

### <a name="go"></a>GO

Redigo usa TLS 1.2 de forma predeterminada.

## <a name="additional-information"></a>Información adicional

- [Configuración de Azure Cache for Redis](cache-configure.md)
