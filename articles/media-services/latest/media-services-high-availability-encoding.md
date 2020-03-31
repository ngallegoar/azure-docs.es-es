---
title: Codificación de alta disponibilidad de Azure Media Services
description: Aprenda a conmutar por error a una cuenta secundaria de Media Services si se produce una interrupción o un error en un centro de datos regional.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: afaa7545fbcbab016249e73a2247817310c5cdfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78934201"
---
# <a name="media-services-high-availability-encoding"></a>Codificación de alta disponibilidad de Azure Media Services 

El servicio de codificación de Azure Media Services es una plataforma de procesamiento por lotes regional y no está diseñada actualmente para una alta disponibilidad dentro de una única región. Actualmente, el servicio de codificación no proporciona conmutación por error instantánea del servicio si se produce una interrupción de un centro de datos regional o un error de componentes subyacentes o de servicios dependientes (por ejemplo, almacenamiento, SQL, etc.). En este artículo se explica cómo implementar Media Services para mantener una arquitectura de alta disponibilidad con conmutación por error y garantizar una disponibilidad óptima para las aplicaciones.

Al seguir las instrucciones y los procedimientos recomendados que se describen en el artículo, disminuirá el riesgo de errores de codificación y los retrasos, y minimizará el tiempo de recuperación si se produce una interrupción en una sola región.

## <a name="how-to-build-a-cross-regional-encoding-system"></a>Compilación de un sistema de codificación entre regiones

* [Cree](create-account-cli-how-to.md) dos cuentas de Azure Media Services (o más).

    Las dos cuentas deben estar en distintas regiones. Para más información, consulte las [regiones en las que se implementa el servicio Azure Media Services](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).
* Cargue los medios en la misma región desde la que planea enviar el trabajo. Para más información sobre cómo iniciar la codificación, consulte [Crear una entrada de trabajo desde una dirección URL de HTTPS](job-input-from-http-how-to.md) o [Creación de una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md).

    Si luego necesita volver a enviar el [trabajo](transforms-jobs-concept.md) a otra región, puede usar JobInputHttp o [Copy-Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) para copiar los datos del contenedor de recursos de origen en un contenedor de recursos de la región alternativa.
* Suscríbase a mensajes de JobStateChange en cada cuenta a través de Azure Event Grid. Para más información, consulte:

    * [Ejemplo de análisis de audio](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer) que muestra cómo supervisar un trabajo con Azure Event Grid, incluida la adición de una reserva en caso de que los mensajes de Azure Event Grid se retrasen por algún motivo.
    * [Esquemas de Azure Event Grid para eventos de Media Services](media-services-event-schemas.md)
    * [Regístrese para eventos a través de Azure Portal o de la CLI](reacting-to-media-services-events.md) (también puede hacerlo con el SDK de administración de EventGrid).
    * [SDK Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/) (que admite eventos de Media Services de forma nativa).

    También puede consumir eventos Event Grid a través de Azure Functions.
* Al crear un [trabajo](transforms-jobs-concept.md):

    * Seleccione aleatoriamente una cuenta de la lista de cuentas usadas actualmente (esta lista contendrá normalmente ambas cuentas, pero si se detectan problemas, es posible que solo contenga una cuenta). Si la lista está vacía, se genera una alerta para que un operador pueda investigar.
    * Como guía general, necesita una [unidad reservada de multimedia](media-reserved-units-cli-how-to.md) por [JobOutput](https://docs.microsoft.com/rest/api/media/jobs/create#joboutputasset) (a menos que use [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) donde se recomienda 3 unidades reservadas multimedia por JobOutput).
    * Obtenga el recuento de unidades reservadas de multimedia (MRU) para la cuenta elegida. Si el recuento actual de **unidades reservadas de multimedia** no está ya en el valor máximo, agregue el número de MRU que necesita el trabajo y actualice el servicio. Si la tasa de envío de trabajos es alta y consulta con frecuencia las MRU para encontrar que está en el máximo, use una caché distribuida para el valor con un tiempo de espera razonable.
    * Mantenga un recuento del número de trabajos en proceso.

* Cuando el controlador de JobStateChange reciba una notificación de que un trabajo ha alcanzado el estado programado, registre la hora en que entra en el estado de la programación y la región o cuenta usadas.
* Cuando el controlador de JobStateChange reciba una notificación de que un trabajo ha alcanzado el estado de procesamiento, marque el registro del trabajo como en proceso.
* Cuando el controlador de JobStateChange reciba una notificación de que un trabajo ha alcanzado el estado Finalizado/Error/Cancelado, marque el registro del trabajo como final y disminuya el número de trabajos en proceso. Obtenga el número de unidades reservadas de codificación para la cuenta elegida y compare el número de MRU actual con el recuento de trabajos en proceso. Si el número de trabajos en proceso es menor que el recuento de MRU, disminuya el servicio y actualícelo.
* Tenga un proceso independiente que examine periódicamente los registros de los trabajos.
    
    * Si tiene trabajos en el estado programado que no han avanzado al estado de procesamiento en una cantidad de tiempo razonable para una región determinada, quite esa región de la lista de cuentas usadas actualmente.  En función de los requisitos de su empresa, podría decidir cancelar esos trabajos de inmediato y volver a enviarlos a la región. O bien, puede darles más tiempo para pasar al estado siguiente.
    * Según el número de unidades reservadas de multimedia configuradas en la cuenta y la tasa de envío, también puede haber trabajos con el estado en cola que el sistema no ha seleccionado aún para el procesamiento.  Si la lista de trabajos con el estado en cola crece por encima de un límite aceptable en una región, esos trabajos se pueden cancelar y enviar a la otra región.  Sin embargo, esto puede ser un síntoma de no tener suficientes unidades reservadas de multimedia configuradas en la cuenta para la carga actual.  Si es necesario, puede solicitar una cuota de unidades reservadas de multimedia más alta a través del soporte técnico de Azure.
    * Si se ha quitado una región de la lista de cuentas, debe comprobar que se haya recuperado antes de volver a agregarla a la lista.  El estado regional se puede supervisar a través de los trabajos existentes en la región (si no se cancelaron y reenviaron); para ello, agregue la cuenta de nuevo a la lista después de un período de tiempo, y use los operadores que supervisan las comunicaciones de Azure sobre las interrupciones que pueden afectar a Azure Media Services.
    
Si observa que el recuento de MRU aumenta y disminuye excesivamente, mueva la lógica de decremento a la tarea periódica. Haga que la lógica de envío previo al trabajo compare el recuento en proceso con el recuento de MRU actual para ver si es necesario actualizar las MRU.

## <a name="next-steps"></a>Pasos siguientes

* [Compilación de streaming de vídeo a petición entre regiones](media-services-high-availability-streaming.md)
* Consulte los [ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
