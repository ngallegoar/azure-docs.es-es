---
title: 'Tutorial: Configuración de clústeres de Azure Kubernetes Service (AKS) en Azure mediante Ansible'
description: Aprenda a usar Ansible para crear y administrar un clúster de Azure Kubernetes Service en Azure
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, aks, kubernetes
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 6672c3fac1c5d546a61622e3fd6df6c5397f87a2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74156687"
---
# <a name="tutorial-configure-azure-kubernetes-service-aks-clusters-in-azure-using-ansible"></a>Tutorial: Configuración de clústeres de Azure Kubernetes Service (AKS) en Azure mediante Ansible

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

Es posible configurar AKS para que utilice [Azure Active Directory (AD)](/azure/active-directory/) para la autenticación de usuarios. Una vez configurado, use el token de autenticación de Azure AD para iniciar sesión en el clúster de AKS. El RBAC puede basarse en la identidad de un usuario o en la pertenencia al grupo de directorio.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Creación de un clúster de AKS
> * Configuración de un clúster de AKS

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-managed-aks-cluster"></a>Creación de un clúster de AKS administrado

El cuaderno de estrategias de ejemplo crea un grupo de recursos y un clúster de AKS dentro de dicho grupo.

Guarde el siguiente cuaderno de estrategias como `azure_create_aks.yml`:

```yml
- name: Create Azure Kubernetes Service
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
    aks_version: aks_version
tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create a managed Azure Container Services (AKS) cluster
    azure_rm_aks:
      name: "{{ aks_name }}"
      location: "{{ location }}"
      resource_group: "{{ resource_group }}"
      dns_prefix: "{{ aks_name }}"
      kubernetes_version: "{{aks_version}}"
      linux_profile:
        admin_username: "{{ username }}"
        ssh_key: "{{ ssh_key }}"
      service_principal:
        client_id: "{{ client_id }}"
        client_secret: "{{ client_secret }}"
      agent_pool_profiles:
        - name: default
          count: 2
          vm_size: Standard_D2_v2
      tags:
        Environment: Production
```

Antes de ejecutar el cuaderno de estrategias, consulte las notas siguientes:

- La primera sección dentro de `tasks` define un grupo de recursos denominado `myResourceGroup` dentro de la ubicación `eastus`.
- La segunda sección dentro de `tasks` define un clúster de AKS denominado `myAKSCluster` dentro del grupo de recursos `myResourceGroup`.
- Para el marcador de posición `your_ssh_key`, escriba la clave pública de RSA en el formato de línea única: debe empezar por "ssh-rsa" (sin las comillas).
- Para el marcador de posición `aks_version`, use el comando [az aks get-versions](/cli/azure/aks?view=azure-cli-latest#az-aks-get-versions).

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook azure_create_aks.yml
```

Al ejecutar el cuaderno de estrategias, se muestran resultados similares a la salida siguiente:

```Output
PLAY [Create AKS] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create resource group] 
changed: [localhost]

TASK [Create an Azure Container Services (AKS) cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="scale-aks-nodes"></a>Escalado de nodos de AKS

El cuaderno de estrategias de ejemplo de la sección anterior define dos nodos. Ajuste el número de nodos con la modificación del valor `count` en el bloque `agent_pool_profiles`.

Guarde el siguiente cuaderno de estrategias como `azure_configure_aks.yml`:

```yml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Antes de ejecutar el cuaderno de estrategias, consulte las notas siguientes:

- Para el marcador de posición `your_ssh_key`, escriba la clave pública de RSA en el formato de línea única: debe empezar por "ssh-rsa" (sin las comillas).

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook azure_configure_aks.yml
```

Al ejecutar el cuaderno de estrategias, se muestran resultados similares a la salida siguiente:

```Output
PLAY [Scale AKS cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Scaling an existed AKS cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="delete-a-managed-aks-cluster"></a>Eliminación de un clúster de AKS administrado

El cuaderno de estrategias de ejemplo elimina un clúster de AKS.

Guarde el siguiente cuaderno de estrategias como `azure_delete_aks.yml`:


```yml
- name: Delete a managed Azure Container Services (AKS) cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    aks_name: myAKSCluster
  tasks:
  - name:
    azure_rm_aks:
      name: "{{ aks_name }}"
      resource_group: "{{ resource_group }}"
      state: absent
  ```

Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

```bash
ansible-playbook azure_delete_aks.yml
```

Al ejecutar el cuaderno de estrategias, se muestran resultados similares a la salida siguiente:

```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [azure_rm_aks] 

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Escalado de una aplicación en Azure Kubernetes Service (AKS)](/azure/aks/tutorial-kubernetes-scale)