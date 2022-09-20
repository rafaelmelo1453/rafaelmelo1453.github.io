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

Para obter a Region Key acesse [Regiões e Domínios de Disponibilidade](https://www.oracle.com/cloud/free](https://docs.oracle.com/pt-br/iaas/Content/General/Concepts/regions.htm)




![devops-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VBDyGiRs1ur5DMLj9Ic5oSsJusz8ViCPmDc1WaAa0ynwBnSzzAEkwOG3Hh-KiJrA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-DEVOPS-PIPELINE/devops-03.png)





![oac-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/zcjayskMSkCdIGyBwFx3w0T7HIFAAQOYJ4xkunGXVZ1FJpcEmoDVUNFpru1_q-8d/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-4.png)



[Oracle Cloud Free Tier](https://www.oracle.com/cloud/free)

**Importante:** O provisionamento do Private Access Channel dura entre 1h e 2h. A console do Analytics Cloud estará acessível, mas pode haver interrupção temporária do serviço durante o provisionamento.

![oac-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4ZA_X2ZBczX6QE22gxdFmCFkdzhTxrdZ9FyAEkamcXG-jW5i2fybF8wLTtyRLiTz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-5.png)

Quando o provisionamento for concluído o Private Access Channel será apresentado dessa forma em **Analytics Cloud > Analytics Instance > Private Access Channel.**

![oac-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4ZA_X2ZBczX6QE22gxdFmCFkdzhTxrdZ9FyAEkamcXG-jW5i2fybF8wLTtyRLiTz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-6.png)

Antes de prosseguir verifique NSG ou Security List da fonte de dados e adicione regras permitindo o acesso a partir dos endereços IPs do Private Access Channel (PAC).

![oac-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4ZA_X2ZBczX6QE22gxdFmCFkdzhTxrdZ9FyAEkamcXG-jW5i2fybF8wLTtyRLiTz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-11.png)

## Conectando Analytics Cloud com fonte de dados via Private Access Channel

Navegue até **Analytics Cloud > Analytics Instance** e clique na URL do Analytics Cloud em **Access Information**. 

Na conlose do Analytics Cloud clique em **Criar** e depois em **Conexão.**

![oac-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4ZA_X2ZBczX6QE22gxdFmCFkdzhTxrdZ9FyAEkamcXG-jW5i2fybF8wLTtyRLiTz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-7.png)

Escolha o tipo de fonte de dados desejado, neste caso é um Autonomous Transaction Processing.

![oac-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4ZA_X2ZBczX6QE22gxdFmCFkdzhTxrdZ9FyAEkamcXG-jW5i2fybF8wLTtyRLiTz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-8.png)

Insira os dados da fonte de dados e clique em **Salvar.**

- Nome da conexão
- Descrição.
- Wallet do Autonomous no formato .zip.
- User e password do Autonomous
- Tipo de serviço

![oac-09](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4ZA_X2ZBczX6QE22gxdFmCFkdzhTxrdZ9FyAEkamcXG-jW5i2fybF8wLTtyRLiTz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-9.png)

Feito isso a conexão com uma fonte de dados a partir do Private Access Channel é estabelecida.

![oac-10](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/mO3zrzrXRdXAEDG9iNQdV6i1If5B0Al3i4caL42JdStbsoOSRIA0WDll7h5z3e70/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/OAC-PAC/oac-10.png)

Dessa forma o Private Access Channel (PAC) está habilitado em uma instância do Analytics Cloud.

