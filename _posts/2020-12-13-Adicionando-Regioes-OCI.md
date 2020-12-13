---
layout: post
title: Como adicionar regiões ao Oracle Cloud Infrastructure
subtitle: Como habilitar multiplas regiões na Oracle Cloud Infrastructure
tags: [Cloud, Oracle, OCI, Pay-as-You-Go, Região, Limites]
comments: true
---

## Requisitos

| Conta Pay-as-You-Go | Obrigatório |

Não é possível adicionar novas regiões na Oracle Cloud Infrastructure com uma conta Free Tier, para isso é preciso ter uma conta paga do tipo Pay-as-You-Go ou Contrato com a Oracle.

## Limite de Regiões

Verifique se seus limites permitem adicionar uma nova região em **Governance > Limits, Quotas and Usage** com os seguintes filtros.

![limite-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/U-veHKuEauMe2X0hJmlvYA6WaX945GZwtX4b4QyDXh1MWjkmGNtb_UE134XcnZdE/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/limite-01.png)

Caso o service limit seja igual que o número de regiões utilizadas você precisará abrir um SR para aumentar o limite conforme abaixo, caso o service limit seja maior pule essa etapa.

**Criar SR:** Abra um SR para aumentar o limite de regiões clicando em request a service limit increase.

![limite-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/RnncV9tl817nUiJbnoIJZnqqBQZZyGiC-hzd5neAExlrOTxGdiPocuztQmYrPfpt/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/limite-02.png)

Preencha com os dados da sua conta.

- Name
- E-mail
- Service Category: Regions
- Resource: Subscribed region count
- Tenancy Limit: 6
- Reason for Request: Increase the limite so that you can subscribe to other OCI regions.

![limite-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/d8FAPnSGSJz2KYh-no3l7zFJn0HRrjK7rgg-ZUB3SRdAR-9tXy19mzqRen7iSssX/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/limite-03.png)

Aguarde até que o SR seja finalizado para prosseguir. Em média leva 24 horas para ser atendido, quando seu SR for encerrado você receberá um e-mail como esse.

![limite-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/R4GnwP-f6bslqhHxBg4APfyEDRlwKmUPPKtv4f2MN4GWSNiWt3-gxXc3TIln9pH-/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/limite-04.png)

## Habilitando nova região

Clique na sua Home Region no meu caso é **Brasil East(São Paulo)** e depois em **Manage Regions**.

![region-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/O3Q-NcLpHiuROoKNStL9_4NsBHkAmz72Y2Mkz9PkW-wwopUXFzB_fuOyQF_qFSTr/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/region-01.png)

Clique em **Subscribe** a direita da região que deseja adicionar.

![region-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Apztf9VqEqtwJeL2QwdCO_p_FSoKMEQGUhC_5lw-rxlW7DyjCf9Q56Hwm1koxg0c/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/region-02.png)

Verifique se confere com a região desejada e confirme clicando em **Subscribe**. 

**OBS:** Não é possível remover uma região depois de adicionada.

![region-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/HA4BPrGIKFK9jh6VL54TBUOp5Pxu_Zp9HuOYhINoOBd2tAwp_7IbnvjzTVBHjsSi/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/region-03.png)

Aguarde pelo menos 10 minutos para que a nova região esteja disponível.

![region-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/do8huo_43GAe0Be4c5Ag6EiUX1yNCA4kEcLedyAQig9cK-QNbk5i2J4Lv5Pv7K_R/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/region-04.png)

Alterne entre regiões clicando em Home Region e depois na região recém adicionada.

![region-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ahY8acBrGWyEWKmcrmXIg5anOr0MeHa3umuoTGshr8xt12NVdffHK9bcPZHZpIHh/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/region-05.png)

Liste todas as regiões habilitadas no Tenancy para confirmar que a nova região está realmente disponível, para isso use o OCI CLI local caso tenha configurado ou via Cloud Shell.

```javascript
$ oci iam region-subscription list --query "data [*].{\"Regiao\":\"region-name\"}" --output table
```

![region-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/urBkT8gkVVFExuwr1n1q_8ytRDH-LC1zB7-nYA8upFXs7jnL8bHHldbuC7uKIAMY/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/NEW-REGION/region-06.png)
