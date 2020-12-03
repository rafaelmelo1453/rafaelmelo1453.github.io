---
layout: post
title: Como configurar OCI-CLI no macOS, Linux e Windows e criar uma Instance VM na OCI
subtitle: Instale o OCI-CLI no macOS, Linux e Windows e crie uma Instance VM via CLI na Oracle Cloud Infrastructure
tags: [Cloud, Oracle, OCI, CLI, Instance, VM, Windows, Linux, MacOs]
comments: true
---

## Requisitos

| Uma conta Oracle Cloud | Obrigatório |
| Python v2.7 ou superior | Obrigatório |
| Tenancy OCID | Obrigatório |
| Compartment OCID | Obrigatório |
| User OCID | Obrigatório |

## Baixando e instalando o Oracle Cloud Infrastructure CLI

Aceite as configurações default ou personalize conforme a necessidade.

**MacOS/Linux**  
```javascript
bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"
```

**Windows:** Execute o comando abaixo com permissão de administrador. Aceite a instalação do PythonCore caso seja solicitado, reabra o CMD após a instalação.
```javascript
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1'))"
```

Certifique-se que a instalação foi bem sucedida. 

```javascript
oci --help
```

Caso a instalação tenha sido bem sucedida receberá um output como esse.

![Oci-Help](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/E4YcwQoBdQsKXZm8fVrLci4xiInG0FiRaGWSQfNEXxVXJAmfiSXS-3-PPKoWV2Vr/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/oci-help.png)

## Configurando Oracle Cloud Infrastructure CLI

Com o CLI instalado, agora é preciso configura-lo. Nesta etapa será necessário user OCID e tenancy OCID.

**Obtendo User OCID e Tenancy OCID** 

User OCID: Navegue até Identity>Users>Seu User

![User-Ocid](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/np_zGbe9Zaz9UINKmPcGJhRIxsGrUHuVEEcDhLG6RQow1d6SMvspygRAq_UCpzff/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/user-ocid.png)

Tenancy OCID: Navegue até Administration>Compartment>Tenancy Details

![Tenancy-Ocid](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/AgiXs372OaF4Jl9P2gdjVXpgceo-c_u7So_hh2oFnDolVOGFv3VwgCEniswKKxQB/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/tenancy-ocid.png)

Para iniciar o processo de configuração execute o comando abaixo, aceite as configurações default ou personalize conforme a necessidade. Salve o diretório de armazenamento do arquivo config e SSH Key.

```javascript
oci setup config
```
![Oci-Setup](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Wu9yqvH68zmY2b7VbxXnbi1pwXZl54R7-YfNC8WVNpf-kz_xCczdbvMNjpwQagLy/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/oci-setup-config.png)

Nesta etapa será criada uma API Keys e será utilizado a public key recém criada nesse processo.

**MacOS/Linux**

Preencha com as suas infomações (caminho/nome da public key) apontadas no momento da confinguração.

```javascript
$ cd /home/ubuntu/.oci/
$ vim oci_api_key_public.pem 
```
Copie o conteúdo completo da Public Key.

![Public-Key](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/vOVDjB3xmwdJUYLaQRSVXluCGDaCtPUbN0gv6At8kYdtnrgLK5wJSptvLISA0nxT/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/public-key.png)

**Windows**

Abra a Public Key com notepad e copie todo o conteúdo.

![Public-Key-Windows](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/vOVDjB3xmwdJUYLaQRSVXluCGDaCtPUbN0gv6At8kYdtnrgLK5wJSptvLISA0nxT/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/public-key.png)

Acesse a console Oracle Cloud e vá atá o caminho Identity>Users>Seu User>API Keys>Add API Key.

Selecione "Paste Public Key", cole o conteúdo da Public Key e clique em Add.

![Api-Key](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/GFEO45ZEfNHoh4PEG-JULwJJ5h-sPIdLFyc8kRyIUiPalFoyrwEd4CdkCPVZhnT2/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/api-key.png)

Após adicionada a API Key é gerada um Fingerprint. 

![Finger-Print](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/J-fdWrzqA79ic4IYMXvSTJg0gslBP0R1KeGY4wfuY0jFus4Kx0ZTago6Uhi2QtJc/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/fingerprint.png)

Visualize o arquivo de configuração.

**MacOS/Linux**

```javascript
$ cd /home/ubuntu/.oci/
$ cat config
```

![Config-Cat](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/QP1n5BBih4rC9YNK2DSH5bS3xtB5NJZSqtGeIsJr-xsNbuZqbf1ucSKhplx_QIm7/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/config.png)

**Windows**

Navegue até o diretório do arquivo config e abra com notepad.

![Config-Cat-Windows](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/uEdpaH0pQjgD4EQ04W7AqkVGFTqKIkaWt180qI7FpypikIpvb46_hukwPIV3qbT3/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/config-windows.png)

Certifique-se que a configuração foi bem sucedida listando os compartments do Tenancy.

```javascript
$ oci iam compartment list
```

![Compartment-List](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/FmAMKaP-SLgK1KhRNlVZGlXRsg0qTQD0PPydkh4BnKz7z7R3Usd8p64-Td6J0Jpc/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/compartment-list.png)

## Deploy de uma Instance VM com OCI-CLI

Após instalar, configurar e certificar-se que tudo funciona bem com OCI-CLI, siga os passos abaixo para criar uma Instance VM via CLI.

Para essa task será necessario as seguintes recursos:

| Recurso | Onde Conserguir |
| :------ |:--- |
| Availability Domain | CLI-OCI |
| Compartment OCID | CLI-OCI |
| Subnet OCID | CLI-OCId |
| Image OCID | CLI-OCI |
| Name Shape | Site Oracle Cloud |
| Display Name | Defina um Nome | 
| SSH Key | Gerar durante Deploy |

**Availability Domain:** Salve o OCID do availability domain.

```javascript
$ oci iam availability-domain list --query "data [*].{\"Name\":\"name\"}" --output table
```

**Compartimet OCID:** Salve o OCID do compartment que deseja criar a Instance VM.

```javascript
$ oci iam compartment list --lifecycle-state active --query "data [*].{\"Name\":\"name\",\"id\":\"id\"}" --output table
```

**Subnet OCID:** Substitua o valor do --compartment-id pela OCID do compartiment listado acima onde sua subnet está alocada. Salve o OCID da subnet.

```javascript
$ oci network subnet list --compartment-id ocid1.compartment.oc1..aaaaaaaa2tjqxvk2oxw45php23trjixcrzwb3bhzhcw4qqjpjcpvozny6mza --query "data [*].{\"Name\":\"display-name\",\"id\":\"id\"}" --output table
```

**Imagem OCID:** Substitua o valor do --compartment-id pela OCID do compartiment listado acima onde será realizado o deploy da Instance VM. Salve o OCID da imagem desejada, nesta task será utilizada a Oracle-Linux-7.9-2020.11.10-1, mas é possível escolher qualquer [imagem disponível na Oracle Cloud Infrastructure](https://docs.cloud.oracle.com/en-us/iaas/images/).

```javascript
$ oci compute image list --compartment-id ocid1.compartment.oc1..aaaaaaaayu2eqzztrf7nrvi2dc5h2vl2rw2xoqphiucblfg7ossq7rzc5wsq --query "data [?contains(\"display-name\",'Oracle-Linux-7.9')].{\"IMAGE\":\"display-name\",\"ID\":\"id\"}" --output table
```

**Shape:** Utilize o shape **Always Free** para testes, consulte todos os shapes disponíveis no site oficial [Oracle Cloud Infrastructure.](https://docs.cloud.oracle.com/en-us/iaas/Content/Compute/References/computeshapes.htm)

![Resources-Get](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/pACSZBitORcnthvHOrTAg4R5nFyflz6fP3Es08mvZ4kxE8nwsYt10ZzAcwRwpHDY/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/resource-get.png)

**Iniciando Deploy da Instance VM**

Inicie criando um diretório para armazenar a SSH Key.

```javascript
$ mkdir keys
```

Gerando uma nova SSH Key. Substitua pelo caminho que deseja salvar a SSH Key, por exemplo C:\Keys no Windows. 

```javascript
$ ssh-keygen -t rsa -N "" -b 2048 -C "CiCd-Compute-Instance" -f /home/ubuntu/keys/key-test
```

Faça o deploy da Instance VM utilizando as informações salvas anteriomente availability-domain, compartment-id, shape, subnet-id, image-id, display-name, ssh-authorized-keys-file e adicione o parâmetro assign-public-ip como true para que seja atribuído um IP público na Instance VM.

```javascript
$ oci compute instance launch --availability-domain syxp:SA-SAOPAULO-1-AD-1 --compartment-id ocid1.compartment.oc1..aaaaaaaa2tjqxvk2oxw45php23trjixcrzwb3bhzhcw4qqjpjcpvozny6mza --shape VM.Standard.E2.1.Micro --subnet-id ocid1.subnet.oc1.sa-saopaulo-1.aaaaaaaa2j4d7too2lkyjtlzujdegwl3m37tpoqxilnsyunykc2nh3fy65kq --image-id ocid1.image.oc1.sa-saopaulo-1.aaaaaaaa7inha53kcyutiqdbz3w4gvms2ab5z3bc624loheugh7fbvg4wada --assign-public-ip true --display-name instance-vm-teste --ssh-authorized-keys-file /home/ubuntu/keys/key-test.pub
```

Aguarde até que o estado da Intance VM seja Running.

![VM-Run](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/frjmu6F0Z5y29QjmRqTmV8xDhwrUcLPBHN6E0NsT6p3loROKM2m6-BPcNFlAXsqL/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/running-instance.png)

Acesse a Instance VM recém criada.

**MacOS/Linux**

```javascript
$ chmod 400 /home/ubuntu/keys/key-test
$ ssh -i /home/ubuntu/keys/key-test opc@152.67.40.241
```

![Access-macos](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/MiuLVTjjSNXUtaqxJCT_CAI0z4Ia9B7OUbOE87GjmXgOorvN5fL3b-kICe9tbZhT/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/access-macos.png)

**Windows**

Instale o [MobaXterm](https://mobaxterm.mobatek.net/), abra uma nova sessão e acesse a Instance VM.

```javascript
$ chmod 400 'C:\keys\key-test'
$ ssh -i 'C:\keys\key-test' opc@152.67.40.241
```

![Acess-Windows](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/MKR3wsGjo5S0o1C-i19XtWHpINaXKoPA0qFYYNqCus9fUhkXl4I7jW-sue1tZbU6/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/access-windows.png)

Agora você é capaz de acessar o OCI-CLI e criar Instances VMs na Oracle Cloud Infrastructure.









