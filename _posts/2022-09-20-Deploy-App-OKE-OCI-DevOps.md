---
layout: post
title: Deploy App com OCI DevOps Pipeline
subtitle: Usando OCI DevOps Pipeline para realizar o deploy de um aplicativo em um cluster OKE.
share-img: /assets/img/oci.jpg
thumbnail-img: /assets/img/oci.jpg
tags: [Cloud, Oracle, OCI, DevOps, Pipeline, OKE, Cluster, LoadBalancer]
comments: true
---

OCI DevOps é um serviço Oracle Cloud Native para Integração e Entrega Contínua (CI/CD), neste tutorial usaremos para criar uma pipeline para implantar uma aplicação num cluster OKE. 

## Criando um Repository para armazenar Imagens.

Na Console de OCI navegue até **Developer Services** e clique em **Container Registry**. 

- Escolha o compartment e região (neste exemplo, br-est-sao-work e Brazil East, São Paulo).

![devops-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-01.png)

Clique em **Create Repository.**

- Escolha o compartment.
- Insira um nome para o repositório (neste exemplo, helloworld).
- Selecione a opção **Private.**

Então clique em **Create Repository** para finalizar.

![devops-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-02.png)

- Insira uma descrição e novamente clique em **Generate Token.**
- Copie o auth token imediatamente para um notepad, pois não é possível recuperá-lo após fechar.

## Push uma Imagem para o OCIR (Oracle Cloud Infrastructure Registry).

Para fazer o push da imagem docker para o OCIR voçe vai precisar das seguintes informações sobre o tenancy OCI:

- Tenancy namespace.
- Username.
- Auth Token (necessário gerar).
- Region Key.

Para obter o **Tenancy Namespace** clique em **Governance & Administration** e navegue até **Tenancy details**.

![devops-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-03.png)

Para obter o **Username** vá até **Profile** e clique em **User Settings**.

![devops-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-04.png)

Caso seu tenancy seja federado com o Oracle Identity Cloud Service, copie o texto completo incluído o oracleidentitycloudservice.

Ainda em **User Settings** clique em **Auth Tokens** e depois em  **Generate Token** para gerar um novo **Auth Token**.

- Insira uma descrição e novamente clique em **Generate Token.**
- Copie o auth token imediatamente para um notepad, pois não é possível recuperá-lo após fechar.

![devops-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-05.png)

Para obter a Region Key acesse [Regiões e Domínios de Disponibilidade](https://docs.oracle.com/pt-br/iaas/Content/General/Concepts/regions.htm)

Com isso temos todos as informações para os próximos passos.

Abra uma janela do terminal num desktop com Docker instalado, faça login no Oracle Cloud Infrastructure Registry usando o seguinte comando:

```javascript
# docker login <region-key>.ocir.io
```

Insira o username com o seguinte formato \<tenancy-namespace\>/oracleidentitycloudservice/\<username\> para usuários federados ou \<tenancy-namespace\>/\<username\> para **usuários não federados.**
  
Então insira o Auth Token copiado anteriomente no lugar da senha.
  
![devops-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-06.png)

