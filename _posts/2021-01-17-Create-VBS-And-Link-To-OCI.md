---
layout: post
title: Habilitando Visual Builder Studio e vinculando ao Oracle Cloud
subtitle: Criando conta do Visual Builder Studio e conectando tenancy do Oracle Cloud Infrastructure
share-img: /assets/img/oci.jpg
thumbnail-img: /assets/img/oci.jpg
tags: [Cloud, Oracle, OCI, CLI, Visual Builder Studio, DevCS, DevOps]
comments: true
---

## O que é Oracle Visual Builder Studio?

O Visual Builder Studio é um serviço/produto da Oracle que permite o uso de pipelines de CI/CD, gerenciamento ágil de software, compilações automatizadas, verificação, controle e qualidade de código, gerenciamento de artefatos, em resumo VB Studio é o concorrente direto ao Azure DevOps.

## Como funciona o licenciamento do VB Studio?

**Oracle Visual Builder Studio:** É gratuito para clientes com assinatura paga do Oracle Cloud, com limite de 20GB de armazenamento e 200 projetos.

**Oracle Visual Builder Cloud Service:** Ao ultrapassar os limites da versão gratuita o faturamento é realizado da seguinte forma: R$6,5212 x OCPU por hora.

## Habilitando Oracle Visual Builder Studio

Na console do Oracle Cloud Infrastructure navegue até **More Oracle Cloud Services > Platform Services > Developer.**

![devcs-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/xj9srMULWogekAo4GNV_I-rU75T6l4Cxtig3GmIFes5wnFNfQKatXs-AwB0CyBrV/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-01.png)

Na aba Instances clique em **Create Instance** para criar uma conta do Oracle Visual Builder Studio.

![devcs-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/tMVMvHIDz9T777hrWHd8sfPnThf8C2QNJa1MXcOHejHvZn7RKkM_xvIB4zxypCw9/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-02.png)

Preencha com as informações da sua conta, avance e clique em **Create.**

![devcs-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/3ldIcVDC7y6iFlhiLjBJP-_vF3TDyP6gdJbW95kPIDTuNZ6-2YIdhdJdMIJgRSbk/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-03.png)

Para acessar o ambiente do VB Studio clique no ícone à direita e então **Access Service Instance.**

![devcs-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/hX-XmUMptic4G4UVyEtQAPm9FjCla2CTNvlnwTwOtZO2PHvt35iadW-TV8NNsvbP/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-04.png)

Essa é a console do VB Studio.

![devcs-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/D42zzldyfTg0mRS-zl9aHbqd3uN2sV0U-wjlYpJWBardlE6YH57WzKDJyh3vh_DT/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-05.png)

## Vinculando conta OCI Cloud ao VB Studio

Para concluir esse step é necessário provisionar alguns recursos na OCI são eles:

- 1 Compartment
- 1 IAM User
- 1 par de SSH Keys (private e public) no formato PEM.
- 1 Group
- 1 Policy

**Compartment:** Na console do OCI navegue até **Identity > Compartment > Create Compartment.**

![devcs-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Wxet3-q_bZJCOuMm9m7rtsHzvaefixxv1iIWUu7snyvTorehtT2TxIS7Yp1_9zu0/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-06.png)

**IAM User:** Na console do OCI navegue até **Identity > Users > Create User.**

![devcs-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/70iZR2WmNMD2XdSpuFSs6OM5lsIqeW_ELU8gkZzVwLsahn2I2VUrn1Wv5DHJCJMk/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-07.png)

**SSH Keys:** Na console do OCI navegue até **Identity > Users > Clique no usuário recém criado > API Keys > Add API Key** use a SSH Key única criada pela Oracle e faça download, pois será utilizada nos próximos passos ou use suas próprias SSH Keys. 

![devcs-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/QH6gkquH1MdVAvo2cNpfnufW98KuJXp1WE5GuVUYgZVWnNBbH24EAq9Wgj52aeFZ/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-08.png)

Copie as informações presente no **Configuration File Preview.**

![devcs-09](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/hotJ-KPyDL5lWX5wCH9WQRDEBXRCK509DVTZWaq7HUBHzDs6kDot2GiteLJquldF/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-09.png)

**Group:** Na console do OCI navegue até **Identity > Groups > Create Group.** 

![devcs-10](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/GQVUXR3NGnDIbT-DMBDarCN_0WgdPRLgaGQVcy6JZdPirvR8iCpqBTMaG-_crXpa/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-10.png)

Clique em **Add User to Group**

![devcs-10-b](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/B3ZGL-uwd7CgBDYwuLXxMPd7MA1mko4Ab9J-trUEL1NN_sZpcnB-ojw4vsxkCH0T/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-10-b.png)

**Policies:** Na console do OCI navegue até **Identity > Policies > Create Policy.**

Em Policy Builder adicione permissão para o grupo **GROUP-OVBC** gerenciar todos os recursos no compartment **COMPARTMENT-OVBC** recém criado e permissão de leitura em todo o tenancy.

```javascript
allow group GROUP-OVBC to manage all-resources in compartment COMPARTMENT-OVBC
allow group GROUP-OVBC to read all-resources in tenancy
```

![devcs-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/evol2tcaJ69ejd_hKBZG1WobyzAdXtvH1OziNIFwBTmRzbzH6WFiDNakQR1IY_vE/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-11.png)

Retorne para a console do VB Studio e vá até **Organization > OCI Account > Clique em Connect**

![devcs-12](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/fDJOieuxLpNuK_ZQQ4Hnp4NNQtWxlFDmeazSwoVfGS8Q6rmCAGLrhlnMVjszRFFD/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-12.png)

Para **Tenancy OCID, User OCID, Home Region e Fingerprint** use os dados presente no Configuration File Preview.

Para **Compartment OCID** Vá até **Identity > Compartment > COMPARTMENT-OVBC** e copie o OCID.

Para **Storage Namespace** Vá até **Administration > Tenancy Details** e copie o valor da **Object Storage Namespace.**

![devcs-13](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/nJG6bckt8q_DwkvtFomfe8vvrJYdp5MCuOGeaSd7bA6NrPGMjiSuk0Lc6jHrt7Xu/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/DEVCS/devcs-13.png)

Agora você é capaz de criar projetos integrados com Visual Builder Studio e Oracle Cloud Infrastructure.
