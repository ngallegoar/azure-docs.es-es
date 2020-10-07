---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: aec13c6beb8dbfcdd5f38e7f96b86bf03e42fa37
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80986642"
---
## <a name="create-a-ruby-application"></a>Creación de una aplicación de Ruby
Para obtener instrucciones, vea cómo [crear una aplicación de Ruby en Azure](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Configuración de la aplicación para usar Service Bus
Para usar Service Bus, descargue y use el paquete de Ruby de Azure, que incluye un conjunto de útiles bibliotecas que se comunican con los servicios REST de almacenamiento.

### <a name="use-rubygems-to-obtain-the-package"></a>Uso de RubyGems para obtener el paquete
1. Use una interfaz de línea de comandos como **PowerShell** (Windows), **Terminal** (Mac) o **Bash** (Unix).
2. Escriba "gem install azure" en la ventana de comandos para instalar la gema y las dependencias.

### <a name="import-the-package"></a>Importación del paquete
Con el editor de texto que prefiera, agregue lo siguiente al principio del archivo de Ruby en el que pretenda utilizar el almacenamiento:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Configuración de una conexión de Service Bus
Utilice el código siguiente para establecer los valores de espacio de nombres, nombre de la clave, clave, firmante y host:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Defina el valor del espacio de nombres en el valor que creó en lugar de hacerlo en la dirección URL completa. Por ejemplo, utilice **"suespaciodenombresdeejemplo"** , no "suespaciodenombresdeejemplo.servicebus.windows.net".

Al trabajar con varios espacios de nombres, puede pasar la clave y su nombre al constructor al crear objetos `SharedAccessSigner`.

```ruby
sb_namespace = '<your azure service bus namespace>'
sb_sas_key_name = '<your azure service bus access keyname>'
sb_sas_key = '<your azure service bus access key>'

signer = Azure::ServiceBus::Auth::SharedAccessSigner.new(sb_sas_key_name, sb_sas_key)
sb_host = "https://#{sb_namespace}.servicebus.windows.net"
```
