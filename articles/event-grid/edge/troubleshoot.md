---
title: 'Solución de problemas: Azure Event Grid IoT Edge | Microsoft Docs'
description: Solución de problemas en Event Grid en IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: 0196522618d4b61f615f7cc6faeacbe9a8c7c5b4
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171353"
---
# <a name="common-issues"></a>Problemas comunes

Si experimenta problemas al ejecutar Azure Event Grid en IoT Edge en el entorno, este artículo le sirve como guía para solucionarlos.

## <a name="view-event-grid-module-logs"></a>Ver registros del módulo de Event Grid

Para solucionar problemas, puede que deba tener acceso a los registros del módulo de Event Grid. Para ello, en la máquina virtual donde se implementa el módulo, ejecute el siguiente comando:

En Windows:

```sh
docker -H npipe:////./pipe/iotedge_moby_engine container logs eventgridmodule
```

En Linux:

```sh
sudo docker logs eventgridmodule
```

## <a name="unable-to-make-https-requests"></a>No se pueden realizar solicitudes HTTPS

* En primer lugar, asegúrese de que el módulo de Event Grid tiene establecido **inbound:serverAuth:tlsPolicy** en **estricto**  o **habilitado**.

* Si es comunicación de módulo a módulo, asegúrese de que está realizando la llamada en el puerto **4438** y el nombre del módulo coincide con lo que se implementa. 

  Por ejemplo, si el módulo de Event Grid se implementó con el nombre **eventgridmodule**, la dirección URL debe ser **https://eventgridmodule:4438** . Asegúrese de que el uso de mayúsculas y el número de puerto son correctos.
    
* Si se trata de un módulo que no es de IoT, asegúrese de que el puerto de Event Grid está asignado en el equipo host durante la implementación, por ejemplo:

    ```json
    "HostConfig": {
                "PortBindings": {
                  "4438/tcp": [
                    {
                        "HostPort": "4438"
                    }
                  ]
                }
     }
    ```

## <a name="unable-to-make-http-requests"></a>No se pueden realizar solicitudes HTTP

* En primer lugar, asegúrese de que el módulo de Event Grid tiene establecido **inbound:serverAuth:tlsPolicy** en **estricto**  o **deshabilitado**.

* Si es comunicación de módulo a módulo, asegúrese de que está realizando la llamada en el puerto **5888** y el nombre del módulo coincide con lo que se implementa. 

  Por ejemplo, si el módulo de Event Grid se implementó con el nombre **eventgridmodule**, la dirección URL debe ser **http://eventgridmodule:5888** . Asegúrese de que el uso de mayúsculas y el número de puerto son correctos.
    
* Si se trata de un módulo que no es de IoT, asegúrese de que el puerto de Event Grid está asignado en el equipo host durante la implementación, por ejemplo:

    ```json
    "HostConfig": {
                "PortBindings": {
                  "5888/tcp": [
                    {
                        "HostPort": "5888"
                    }
                  ]
                }
    }
    ```

## <a name="certificate-chain-was-issued-by-an-authority-thats-not-trusted"></a>La cadena de certificados la emitió una autoridad que no es de confianza

De forma predeterminada, el módulo de Event Grid está configurado para autenticar a los clientes con el certificado emitido por el demonio de seguridad de IoT Edge. Asegúrese de que el cliente presenta un certificado que está en la raíz de esta cadena.

La clase **IoTSecurity** en [https://github.com/Azure/event-grid-iot-edge](https://github.com/Azure/event-grid-iot-edge) muestra cómo recuperar certificados del demonio de seguridad de IoT Edge y cómo usarlos para configurar las llamadas salientes.

Si se trata de un entorno que no es de producción, tiene la opción de desactivar la autenticación de cliente. Consulte [Seguridad y autenticación](security-authentication.md) para obtener más información sobre cómo hacerlo.

## <a name="debug-events-not-received-by-subscriber"></a>El suscriptor no recibe los eventos de depuración

Los motivos comunes son:

* El evento nunca se publicó correctamente. Al publicar un evento en el módulo de Event Grid, se debe recibir un código de estado de HTTP 200 (correcto).

* Compruebe la suscripción del evento para comprobar que:
    * La dirección URL del punto de conexión es válida
    * Los filtros de la suscripción no hacen que el evento se "quite".

* Compruebe si se ejecuta el módulo de suscriptor

* Inicie sesión en la máquina virtual donde se implementa el módulo de Event Grid y vea sus registros.

* Active el registro por entrega estableciendo **broker:logDeliverySuccess=true** y volviendo a implementar el módulo de Event Grid y reintentando la solicitud. Activar el registro por entrega puede afectar al rendimiento y la latencia, de modo que una vez completada la depuración, nuestra recomendación es volver a **broker:logDeliverySuccess=false** y volver a implementar el módulo de Event Grid.

* Active las métricas mediante la configuración **metrics:reportertype=console** y vuelva a implementar el módulo de Event Grid. Cualquier operación después de esto provocará que se registren las métricas en la consola del módulo Event Grid, que se pueden usar para depurar más. Nuestra recomendación es activar las métricas solo para la depuración y, una vez completada, desactivarlas estableciendo **metrics:reportertype=none** y volver a implementar el módulo de Event Grid.

## <a name="next-steps"></a>Pasos siguientes

Notifique cualquier problema o sugerencia con el uso de Event Grid en IoT Edge en [https://github.com/Azure/event-grid-iot-edge/issues](https://github.com/Azure/event-grid-iot-edge/issues).