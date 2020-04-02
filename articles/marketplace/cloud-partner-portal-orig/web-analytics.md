---
title: Web Analytics
description: En este artículo se brindan instrucciones sobre cómo aprender a usar Web Analytics para hacer crecer mejor su negocio.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 446e579a7205f0e785d7e940c1dbdd36cff8f370
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285340"
---
<a name="web-analytics"></a>Web Analytics
=============

En este artículo se brindan instrucciones sobre cómo aprender a usar Web Analytics para hacer crecer mejor su negocio. Actualmente, esta pestaña Insights está disponible para cualquier oferta de AppSource.

Ahora que compiló y publicó la oferta, la siguiente parte del recorrido será hacer seguimiento y medir su\' éxito. Con **Web Analytics**, agregamos la capacidad de ver exactamente qué tan bien está cada una de sus ofertas tiene en Marketplace. Para empezar su recorrido, vaya a la página Insights a la izquierda de Cloud Partner Portal para ver la nueva pestaña Analytics.

![Página de Web Analytics](./media/si-getting-started/WebAnalytics1.png)

Verá un panel completo para el identificador del editor que se creó con Microsoft Power BI y que le permite ver los datos de cada una de sus ofertas\', los que se actualizan a diario.

<a name="microsoft-campaigns"></a>**Campañas de Microsoft**
-----------------------

Con el fin de aumentar sus ofertas y hacer seguimiento del crecimiento de las mismas, habilitamos la capacidad de usar las **campañas de Microsoft** en Cloud Partner Portal. Las campañas son una característica que se admitió recientemente para Marketplace que le permitirá hacer seguimiento de los distintos canales que envían clientes a la página de detalles de la aplicación.

### <a name="how-to-make-a-campaign"></a>**Realización de una campaña**

La manera más sencilla de describir las campañas es que va a agregar una palabra o término personalizado a la dirección URL que llega a la página de detalles de la aplicación en Marketplace. Google, Bing, LinkedIn y muchos otros sitios recomiendan crear un vínculo de anuncio desde su sitio al sitio deseado.

En general, estos esfuerzos son para ayudar a dirigir nuevos clientes al producto y es fundamental medir qué tan bueno es el rendimiento de cada uno de los canales. Aquí es donde entran en juego las campañas.

Hay dos maneras de generar su propia campaña.

1. Agregue a la dirección URL el parámetro de consulta **mktcmpid** que describe qué es la campaña y de qué página o evento provienen estos clientes.

Por ejemplo, puede usar: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign>

1. (Avanzado): Use uno de nuestros identificadores de campaña compatibles y genéricos en la dirección URL. Queremos ajustarnos a las etiquetas de referencia adicionales que necesita usar, por lo que admitimos la convención para reconocer de manera automática estas etiquetas adicionales:
    
    1. **utm\_campaign**
    2. **utm\_source**
    3. **ref**
    4. **src**

Por ejemplo, puede usar: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=NewCampaign>

Puede elegir tener una combinación de varios de estos identificadores de campaña para seguir identificando los diversos orígenes que impulsan el tráfico de la campaña, como de dónde proviene el cliente (correo electrónico, blog, origen de redes sociales, etc.).

Por ejemplo:

1. Origen de referencia de boletín: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=newsletter>
2. Origen de referencia de LinkedIn: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=LinkedIn>

### <a name="ensuring-campaigns-pass-through-all-your-pages"></a>**Asegúrese de que las campañas pasen por todas las páginas**

Puede haber un escenario en el que las campañas tengan una página intermedia a la que impulsa el tráfico que luego empieza a enviar los clientes a Marketplace. Es importante atravesar los identificadores iniciales de campaña hasta la dirección URL final que se envía a Marketplace.

Este es un ejemplo:

1. Un empleado de marketing compra anuncios a Google para impulsar el tráfico a la página de aterrizaje de la empresa <https://contoso.com>. Esta página de aterrizaje tiene un vínculo para \"probar el producto\" que va a <https://appsource.com>.
2. Un usuario hace clic en el anuncio y llega a la página de aterrizaje de su empresa.
    1.  Dirección URL de referencia = google.com
    2.  Dirección URL de la página de aterrizaje = <https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
3. El usuario hace clic en el vínculo para \"probar el producto\" y llega a AppSource.
    1. Dirección URL de referencia = <https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
    2. Dirección URL de la página de aterrizaje (**Asegúrese de que se agregó utm\_campaign y utm\_source a esta dirección URL**) =  [https://appsource.microsoft.com/product/dynamics-365/contoso.offername?**utm\_ campaign=MyCampaignAdName&utm\_ source=MySourceAdName**](https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName)

<a name="how-to-evaluate-the-success-of-a-campaign"></a>Evaluación de los resultados de una campaña
-----------------------------------------

### <a name="page-visits-by-campaign"></a>**Visitas de la página por campaña**

![WebAnalytics2](./media/si-getting-started/WebAnalytics2.png)

Es el desglose de cada una de las visitas diarias a la página por la campaña de la que provienen.

### <a name="conversion-rate-by-campaign"></a>**Tasa de conversión por campaña**

![WebAnalytics3](./media/si-getting-started/WebAnalytics3.png)

De una manera similar a cómo mostramos la tasa de conversión de toda la oferta, en este gráfico se puede ver el desglose del rendimiento de las distintas campañas. Este gráfico lo debería ayudar a identificar de dónde provienen las campañas con más altas tasas de conversión.

### <a name="distribution-by-campaign"></a>**Distribución por campaña**

![WebAnalytics4](./media/si-getting-started/WebAnalytics4.png)

De una manera similar a cómo observamos los dominios de los clientes, en este gráfico se puede ver la distribución de los datos por campaña en la que los usuarios vienen a Marketplace. \_NoCampaign significa que el cliente no tenía ningún identificador de campaña en la dirección URL cuando navegó a Marketplace.

<a name="next-steps"></a>**Pasos siguientes**
--------------

Ahora que tiene la capacidad de hacer seguimiento de los resultados de las ofertas, le recomendamos crear sus propias campañas.

Si tiene alguna pregunta o solicitudes de características, compártalas en Comentarios, en la esquina superior derecha.

![Comentarios en Cloud Partner Portal](./media/si-getting-started/WebAnalytics5.png)
