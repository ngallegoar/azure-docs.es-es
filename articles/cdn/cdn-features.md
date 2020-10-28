---
title: Comparación de las características de los productos de Azure Content Delivery Network (CDN)
description: Conozca las características que admite cada producto de Azure Content Delivery Network (CDN).
services: cdn
documentationcenter: ''
author: asudbring
ms.service: azure-cdn
ms.topic: overview
ms.date: 11/15/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: c8d8b01e8c5f4ea3054e639ecc9dd8b14bbf048b
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148736"
---
# <a name="what-are-the-comparisons-between-azure-cdn-product-features"></a>¿Cuál es la comparación entre las características de los productos de Azure CDN?

Azure Content Delivery Network (CDN) incluye cuatro productos: 

* **Azure CDN Estándar de Microsoft**
* **Azure CDN Estándar de Akamai**
* **Azure CDN estándar de Verizon**
* **Azure CDN prémium de Verizon** . 

En la tabla siguiente se comparan las características disponibles con cada producto.

| **Características y optimizaciones de rendimiento** | **Microsoft estándar** | **Akamai estándar** | **Verizon estándar** | **Verizon premium** |
| --- | --- | --- | --- | --- |
| [Aceleración de sitios dinámicos](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration)  | Se ofrece a través de [Azure Front Door Service](https://docs.microsoft.com/azure/frontdoor/front-door-overview) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Aceleración de sitios dinámicos: compresión de imagen adaptable](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Aceleración de sitios dinámicos: captura previa de objetos](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-azure-cdn-from-akamai-only)  |  | **&#x2713;**  |  |  |
| [Optimización de la entrega web general](https://docs.microsoft.com/azure/cdn/cdn-optimization-overview#general-web-delivery)  | **&#x2713;** | **&#x2713;** Seleccione este tipo de optimización si el tamaño medio de los archivos es inferior a 10 MB.  | **&#x2713;** |  **&#x2713;** |
| [Optimización de streaming de vídeo](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization)  | Mediante entrega web general | **&#x2713;**  | Mediante entrega web general |  Mediante entrega web general |
| [Optimización de archivos grandes](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization)  | Mediante entrega web general | **&#x2713;** Seleccione este tipo de optimización si el tamaño medio del archivo es superior a 10 MB.   | Mediante entrega web general |  Mediante entrega web general |
| Cambio del tipo de optimización | |**&#x2713;** | | |
| Puerto de origen |Todos los puertos TCP |[Puertos de origen permitidos](https://docs.microsoft.com/previous-versions/azure/mt757337(v%3Dazure.100)#allowed-origin-ports) |Todos los puertos TCP |Todos los puertos TCP |
| [Equilibrio de carga del servidor global (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Purga rápida](cdn-purge-endpoint.md)  | **&#x2713;** |**&#x2713;** Purgar todo y la purga con carácter comodín no se admiten actualmente en Azure CDN desde Akamai. |**&#x2713;** |**&#x2713;** |
| [Carga previa de activos](cdn-preload-endpoint.md)  |  | |**&#x2713;** |**&#x2713;** |
| Configuración de la memoria caché o del encabezado (mediante [reglas de almacenamiento en caché](cdn-caching-rules.md))  |**&#x2713;** mediante un [motor de reglas estándar](cdn-standard-rules-engine.md)  |**&#x2713;** |**&#x2713;** | |
| Motor de entrega de contenido personalizable basado en reglas |**&#x2713;** mediante un [motor de reglas estándar](cdn-standard-rules-engine.md)  | | |**&#x2713;** mediante un [motor de reglas](cdn-rules-engine.md) |
| Configuración de caché/encabezado  |**&#x2713;** mediante un [motor de reglas estándar](cdn-standard-rules-engine.md) | | |**&#x2713;** mediante un [motor de reglas Prémium](cdn-rules-engine.md) |
| Redireccionamiento/reescritura de URL |**&#x2713;** mediante un [motor de reglas estándar](cdn-standard-rules-engine.md)  | | |**&#x2713;** mediante un [motor de reglas Prémium](cdn-rules-engine.md) |
| Reglas de dispositivo móvil  |**&#x2713;** mediante un [motor de reglas estándar](cdn-standard-rules-engine.md) | | |**&#x2713;** mediante un [motor de reglas Prémium](cdn-rules-engine.md) |
| [Almacenamiento en caché de cadena de consulta](cdn-query-string.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Pila dual IPv4/IPv6 | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Compatibilidad con HTTP/2](cdn-http2.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
||||
 **Seguridad** | **Microsoft estándar** | **Akamai estándar** | **Verizon estándar** | **Verizon premium** | 
| Compatibilidad con HTTPS con el punto de conexión de red CDN | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Dominio personalizado HTTPS](cdn-custom-ssl.md)  | **&#x2713;** | **&#x2713;** , Requiere CNAME directo para habilitar |**&#x2713;** |**&#x2713;** |
| [Compatibilidad con nombre de dominio personalizado](cdn-map-content-to-custom-domain.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Filtrado geográfico](cdn-restrict-access-by-country.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Autenticación de token](cdn-token-auth.md)  |  |  |  |**&#x2713;**| 
| [Protección contra DDOS](https://www.us-cert.gov/ncas/tips/ST04-015)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Traiga su propio certificado](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) |**&#x2713;** |  | **&#x2713;** | **&#x2713;** |
| Versiones de TLS admitidas | TLS 1.2, TLS 1.0/1.1 - [Configurable](https://docs.microsoft.com/rest/api/cdn/customdomains/enablecustomhttps#usermanagedhttpsparameters) | TLS 1.2 | TLS 1.2 | TLS 1.2 |
||||
| **Análisis e informes** | **Microsoft estándar** | **Akamai estándar** | **Verizon estándar** | **Verizon premium** | 
| [Registros de diagnóstico de Azure](cdn-azure-diagnostic-logs.md)  | **&#x2713;** | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Informes principales de Verizon](cdn-analyze-usage-patterns.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Informes personalizados de Verizon](cdn-verizon-custom-reports.md)  |  | |**&#x2713;** |**&#x2713;** |
| [Informes de HTTP avanzados](cdn-advanced-http-reports.md)  |  | | |**&#x2713;** |
| [Estadísticas en tiempo real](cdn-real-time-stats.md)  |  | | |**&#x2713;** |
| [Rendimiento del nodo perimetral](cdn-edge-performance.md)  |  | | |**&#x2713;** |
| [Alertas en tiempo real](cdn-real-time-alerts.md)  |  | | |**&#x2713;** |
||||
| **Facilidad de uso** | **Microsoft estándar** | **Akamai estándar** | **Verizon estándar** | **Verizon premium** | 
| Fácil integración con los servicios de Azure, como [Storage](cdn-create-a-storage-account-with-cdn.md), [Web Apps](cdn-add-to-web-app.md) y [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md)  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Se puede administrar mediante la [API REST](/rest/api/cdn/), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) o [PowerShell](cdn-manage-powershell.md).  | **&#x2713;** |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tipos MIME de compresión](https://docs.microsoft.com/azure/cdn/cdn-improve-performance)  |Solo predeterminado |Configurable |Configurable  |Configurable  |
| Codificaciones de compresión  |gzip, brotli |gzip |gzip, deflate, bzip2, brotili  |gzip, deflate, bzip2, brotili  |

## <a name="migration"></a>Migración

Para obtener información sobre cómo migrar un perfil de **Azure CDN Estándar de Verizon** a **Azure CDN Premium de Verizon** , consulte [Migración de un perfil de Azure CDN Estándar de Verizon a Premium de Verizon](cdn-migrate.md). 

> [!NOTE]
> Aunque hay una ruta de acceso de actualización de la versión estándar a la prémium de Verizon, en este momento no hay ningún mecanismo de conversión entre otros productos.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [Azure CDN](cdn-overview.md).



