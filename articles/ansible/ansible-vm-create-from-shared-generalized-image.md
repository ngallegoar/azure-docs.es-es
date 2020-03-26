---
title: 'Tutorial: Creación de una máquina virtual o un conjunto de escalado de máquinas virtuales desde Azure Shared Image Gallery mediante Ansible'
description: Aprenda a usar Ansible para crear una máquina virtual o un conjunto de escalado de máquinas virtuales basado en una imagen generalizada en Azure Shared Image Gallery.
keywords: ansible, azure, devops, bash, playbook, virtual machine, virtual machine scale set, shared image gallery
ms.topic: tutorial
ms.date: 10/14/2019
ms.openlocfilehash: f784419736854095cc1bc5da14f3867ac3f7eb12
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74155825"
---
# <a name="tutorial-create-a-vm-or-virtual-machine-scale-set-from-the-azure-shared-image-gallery-using-ansible"></a>Tutorial: Creación de una máquina virtual o un conjunto de escalado de máquinas virtuales desde Azure Shared Image Gallery mediante Ansible

[!INCLUDE [ansible-29-note.md](../../includes/ansible-29-note.md)]

[Azure Shared Image Gallery](/azure/virtual-machines/windows/shared-image-galleries) es un servicio que le permite administrar, compartir y organizar fácilmente imágenes administradas de forma personalizada. Esta característica es útil para escenarios en los que se mantienen y comparten muchas imágenes. Las imágenes personalizadas se pueden compartir entre las suscripciones y los inquilinos de Azure Active Directory. Las imágenes también se pueden replicar en varias regiones para un escalado más rápido de las implementaciones.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Creación de una máquina virtual generalizada y una imagen personalizada
> * Creación de una instancia de Shared Image Gallery
> * Creación de una versión de imagen y de una imagen compartida
> * Creación de una máquina virtual con la imagen generalizada
> * Creación de un conjunto de escalado de máquinas virtuales con la imagen generalizada
> * Obtenga información sobre Shared Image Gallery, la imagen y la versión.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="get-the-sample-playbooks"></a>Obtención de los cuadernos de estrategias de ejemplo

Hay dos formas de obtener el conjunto completo de cuadernos de estrategias de ejemplo:

- [Descargue la carpeta SIG](https://github.com/Azure-Samples/ansible-playbooks/tree/master/SIG_generalized_image) y guárdela en la máquina local.
- Cree un nuevo archivo para cada sección y copie el cuaderno de estrategias de ejemplo en él.

El archivo `vars.yml` contiene las variables usadas por todos los cuadernos de estrategias de ejemplo de este tutorial. Puede editar el archivo para proporcionar nombres y valores únicos.

El primer cuaderno de estrategias de ejemplo `00-prerequisites.yml` crea lo que es necesario para completar este tutorial:
- Un grupo de recursos, que es un contenedor lógico en el que se implementan y se administran los recursos de Azure.
- Una red virtual, subred, dirección IP pública y tarjeta de interfaz de red para la máquina virtual.
- Una máquina virtual de origen, que se usa para crear la imagen generalizada.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Create resource group if doesn't exist
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    
    - name: Create virtual network
      azure_rm_virtualnetwork:
        resource_group: "{{ resource_group }}"
        name: "{{ virtual_network_name }}"
        address_prefixes: "10.0.0.0/16"

    - name: Add subnet
      azure_rm_subnet:
        resource_group: "{{ resource_group }}"
        name: "{{ subnet_name }}"
        address_prefix: "10.0.1.0/24"
        virtual_network: "{{ virtual_network_name }}"

    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}"
        allocation_method: Static
        name: "{{ ip_name }}"

    - name: Create virtual network inteface cards for VM A and B
      azure_rm_networkinterface:
        resource_group: "{{ resource_group }}"
        name: "{{ network_interface_name }}"
        virtual_network: "{{ virtual_network_name }}"
        subnet: "{{ subnet_name }}"

    - name: Create VM
      azure_rm_virtualmachine:
        resource_group: "{{ resource_group }}"
        name: "{{ source_vm_name }}"
        admin_username: testuser
        admin_password: "Password1234!"
        vm_size: Standard_B1ms
        network_interfaces: "{{ network_interface_name }}"
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 00-prerequisites.yml
```

En [Azure Portal](https://portal.azure.com), compruebe el grupo de recursos que especificó en `vars.yml` para ver la nueva máquina virtual y los distintos recursos que creó.

## <a name="generalize-the-vm-and-create-a-custom-image"></a>Generalización de la máquina virtual y creación de una imagen personalizada

En el siguiente cuaderno de estrategias, `01a-create-generalized-image.yml`, se generaliza la máquina virtual de origen creada en el paso anterior y, a continuación, se crea una imagen personalizada basada en ella.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Generalize VM
      azure_rm_virtualmachine:
        resource_group: "{{ resource_group }}"
        name: "{{ source_vm_name }}"
        generalized: yes

    - name: Create custom image
      azure_rm_image:
        resource_group: "{{ resource_group }}"
        name: "{{ image_name }}"
        source: "{{ source_vm_name }}"
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 01a-create-generalized-image.yml
```

Compruebe el grupo de recursos y asegúrese de que aparece `testimagea`.

## <a name="create-the-shared-image-gallery"></a>Creación de la instancia de Shared Image Gallery

La galería de imágenes es el repositorio para compartir y administrar las imágenes. El código del cuaderno de estrategias de ejemplo de `02-create-shared-image-gallery.yml` crea una instancia de Shared Image Gallery en el grupo de recursos.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Create shared image gallery
      azure_rm_gallery:
        resource_group: "{{ resource_group }}"
        name: "{{ shared_gallery_name }}"
        location: "{{ location }}"
        description: This is the gallery description.
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 02-create-shared-image-gallery.yml
```

Ahora verá una nueva galería, `myGallery`, en el grupo de recursos.

## <a name="create-a-shared-image-and-image-version"></a>Creación de una versión de imagen y de una imagen compartida

El siguiente cuaderno de estrategias, `03a-create-shared-image-generalized.yml`, crea una definición de imagen y una versión de imagen.

Las definiciones de imagen incluyen el tipo de imagen (Windows o Linux), notas de la versión y los requisitos de memoria mínima y máxima. La versión es la de la imagen. La galería, la definición de imagen y la versión de la imagen le ayudan a organizar las imágenes en grupos lógicos. 

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
    - name: Create shared image
      azure_rm_galleryimage:
        resource_group: "{{ resource_group }}"
        gallery_name: "{{ shared_gallery_name }}"
        name: "{{ shared_image_name }}"
        location: "{{ location }}"
        os_type: linux
        os_state: generalized
        identifier:
          publisher: myPublisherName
          offer: myOfferName
          sku: mySkuName
        description: Image description
    
    - name: Create or update a simple gallery image version.
      azure_rm_galleryimageversion:
        resource_group: "{{ resource_group }}"
        gallery_name: "{{ shared_gallery_name }}"
        gallery_image_name: "{{ shared_image_name }}"
        name: "{{ shared_image_version }}"
        location: "{{ location }}"
        publishing_profile:
          end_of_life_date: "2020-10-01t00:00:00+00:00"
          exclude_from_latest: yes
          replica_count: 3
          storage_account_type: Standard_LRS
          target_regions:
            - name: West US
              regional_replica_count: 1
            - name: East US
              regional_replica_count: 2
              storage_account_type: Standard_ZRS
          managed_image:
            name: "{{ image_name }}"
            resource_group: "{{ resource_group }}"
      register: output

    - debug:
        var: output
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 03a-create-shared-image-generalized.yml
```

Ahora el grupo de recursos tiene una definición de imagen y una versión de imagen para la galería.

## <a name="create-a-vm-based-on-the-generalized-image"></a>Creación de una máquina virtual basada en la imagen generalizada

Por último, ejecute `04a-create-vm-using-generalized-image.yml` para crear una máquina virtual basada en la imagen generalizada que creó en el paso anterior.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Create VM using shared image
    azure_rm_virtualmachine:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}"
      vm_size: Standard_DS1_v2
      admin_username: adminUser
      admin_password: PassWord01
      managed_disk_type: Standard_LRS
      image:
        id: "/subscriptions/{{ lookup('env', 'AZURE_SUBSCRIPTION_ID') }}/resourceGroups/{{ resource_group }}/providers/Microsoft.Compute/galleries/{{ shared_gallery_name }}/images/{{ shared_image_name }}/versions/{{ shared_image_version }}"
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 04a-create-vm-using-generalized-image.yml
```

## <a name="create-a-virtual-machine-scale-sets-based-on-the-generalized-image"></a>Creación de un conjunto de escalado de máquinas virtuales basada en la imagen generalizada

También puede crear un conjunto de escalado de máquinas virtuales basado en la imagen generalizada. Para ello, ejecute `05a-create-vmss-using-generalized-image.yml`.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Create a virtual machine scale set using a shared image
    azure_rm_virtualmachinescaleset:
      resource_group: "{{ resource_group }}"
      name: "{{ vmss_name }}"
      vm_size: Standard_DS1_v2
      admin_username: adminUser
      admin_password: PassWord01
      capacity: 2
      virtual_network_name: "{{ virtual_network_name }}"
      upgrade_policy: Manual
      subnet_name: "{{ subnet_name }}"
      managed_disk_type: Standard_LRS
      image:
        id: "/subscriptions/{{ lookup('env', 'AZURE_SUBSCRIPTION_ID') }}/resourceGroups/{{ resource_group }}/providers/Microsoft.Compute/galleries/{{ shared_gallery_name }}/images/{{ shared_image_name }}/versions/{{ shared_image_version }}"
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 05a-create-vmss-using-generalized-image.yml
```

## <a name="get-information-about-the-gallery"></a>Obtención de información acerca de la galería

Puede obtener información sobre la galería, la definición de la imagen y la versión si ejecuta `06-get-info.yml`.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Get Shared Image Gallery information
    azure_rm_gallery_info:
      resource_group: "{{ resource_group }}"
      name: "{{ shared_gallery_name }}"
  - name: Get shared image information
    azure_rm_galleryimage_info:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      name: "{{ shared_image_name }}"
  - name: Get Shared Image Gallery image version information
    azure_rm_galleryimageversion_info:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      gallery_image_name: "{{ shared_image_name }}"
      name: "{{ shared_image_version }}"
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 06-get-info.yml
```

## <a name="delete-the-shared-image"></a>Eliminación de la imagen compartida

Para eliminar los recursos de la galería, consulte el cuaderno de estrategias `07-delete-gallery.yml` de ejemplo. Elimine los recursos en orden inverso. Empiece por eliminar la versión de la imagen. Después de eliminar todas las versiones de imagen, puede eliminar la definición de imagen. Después de eliminar todas las definiciones de imagen, puede eliminar la galería.

```yml
- hosts: localhost
  connection: local
  vars_files:
    - ./vars.yml

  tasks:
  - name: Delete gallery image version.
    azure_rm_galleryimageversion:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      gallery_image_name: "{{ shared_image_name }}"
      name: "{{ shared_image_version }}"
      state: absent

  - name: Delete gallery image
    azure_rm_galleryimage:
      resource_group: "{{ resource_group }}"
      gallery_name: "{{ shared_gallery_name }}"
      name: "{{ shared_image_name }}"
      state: absent

  - name: Delete a simple gallery.
    azure_rm_gallery:
      resource_group: "{{ resource_group }}"
      name: "{{ shared_gallery_name }}"
      state: absent
```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook 07-delete-gallery.yml
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine los recursos creados en este artículo. 

El código del cuaderno de estrategias de ejemplo de esta sección se utiliza para:

- Eliminar los dos grupos de recursos creados anteriormente.

Guarde el siguiente cuaderno de estrategias como `cleanup.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: "{{ resource_group_name }}"
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        force_delete_nonempty: yes
        state: absent
```

Estas son algunas notas importantes que hay que tener en cuenta al trabajar con el cuaderno de estrategias de ejemplo:

- Reemplace el marcador de posición `{{ resource_group_name }}` por el nombre del grupo de recursos.
- Se eliminarán todos los recursos de los dos grupos de recursos especificados.

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Ansible en Azure](/azure/ansible/)