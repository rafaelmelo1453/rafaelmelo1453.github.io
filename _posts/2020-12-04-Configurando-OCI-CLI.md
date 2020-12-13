---
layout: post
title: Como configurar OCI-CLI e criar Instances VM na OCI
subtitle: Configure o OCI-CLI para macOS, Linux ou Windows e crie uma Instance VM via CLI na Oracle Cloud Infrastructure
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

![cli-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/R5Q-v1grMpuKQTAdLMApTfSCh8OK5j4W3ZtOiJ_hhrnvlhOKNPmlgujioqxhpvnF/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-01.png)

## Configurando Oracle Cloud Infrastructure CLI

Com o CLI instalado, agora é preciso configura-lo. Nesta etapa será necessário user OCID e tenancy OCID.

**Obtendo User OCID e Tenancy OCID** 

User OCID: Navegue até Identity>Users>Seu User

![cli-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/jTQcwLHf8e2tQKYsXMZmlHuuyFH06JCYJTYEL1lvCYcjVrIt0CRF082Qm4oHnnj0/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-02.png)

Tenancy OCID: Navegue até Administration>Compartment>Tenancy Details

![cli-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/1n38wKfEf0w-BErgc9ZY5EeLe0SOEglJiA3XE9WT1Fq2T0sjRTNp2VrDS7THnoCz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-03.png)

Para iniciar o processo de configuração execute o comando abaixo, aceite as configurações default ou personalize conforme a necessidade. Salve o diretório de armazenamento do arquivo config e SSH Key.

```javascript
oci setup config
```
![cli-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/OfpO6KKw9UbfjCEAaHV0xvtq4C6oOLjhmA4BOxD5cY0sai-k-aHfdbvS7MogkaE3/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-04.png)

Nesta etapa será criada uma API Keys e será utilizado a public key recém criada nesse processo.

**MacOS/Linux**

Preencha com as suas informações (caminho/nome da public key) apontadas no momento da configuração.

```javascript
$ cd /home/ubuntu/.oci/
$ vim oci_api_key_public.pem 
```
Copie o conteúdo completo da Public Key.

![cli-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/xv26tqbPsJntUwCGilSQMhUjwJM1G4TbD5bcW1qRaU5UUnbx6KtcaG06ImjPZ130/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-05.png)

**Windows**

Abra a Public Key com notepad e copie todo o conteúdo.

![cli-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/xv26tqbPsJntUwCGilSQMhUjwJM1G4TbD5bcW1qRaU5UUnbx6KtcaG06ImjPZ130/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-05.png)

Acesse a console Oracle Cloud e vá até o caminho Identity>Users>Seu User>API Keys>Add API Key.

Selecione "Paste Public Key", cole o conteúdo da Public Key e clique em Add.

![cli-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/-vYmSNY9tlIsirHCmrTFpzk78Y0dYM9KYvfFO2gO9zoDb909ouPpiga7Vrlj98S5/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-06.png)

Após adicionada a API Key é gerada um Fingerprint. 

![cli-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/WIv_A6X6rBOnY4wWpTFa39xRJtLYXJP9nXL_Bus5H5mNSzTFnNb1bBogGt_xZ3f1/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-07.png)

Visualize o arquivo de configuração.

**MacOS/Linux**

```javascript
$ cd /home/ubuntu/.oci/
$ cat config
```

![cli-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/PA1xwT1YdFs8uyfK1VtgbqKHHcKmIeyeLeOsprxWbXEBKjwFhMe5TXFLuk5u9BI7/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-08.png)

**Windows**

Navegue até o diretório do arquivo config e abra com notepad.

![cli-09](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/vxG8Srd2nTULCGoA5VbanwE4ZTSVW5Hg7QNUEZJ-t8EprOQkH1occ7T_JpKxA3G8/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-09.png)

Certifique-se que a configuração foi bem sucedida listando os compartments do Tenancy.

```javascript
$ oci iam compartment list
```

![cli-10](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/7Z6bf47oRoEanLwvqYKyfKqFmVTKhubaUo29OQtfVqDNQ5yRYvfk158yVr9pTWlI/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-10.png)

## Deploy de uma Instance VM com OCI-CLI

Após instalar, configurar e certificar-se que tudo funciona bem com OCI-CLI, siga os passos abaixo para criar uma Instance VM via CLI.

Para essa task será necessario os seguintes recursos:

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

**Shape:** Utilize o shape **VM.Standard.E2.1.Micro** que faz parte dos recursos **Always Free** , consulte todos os shapes disponíveis no site oficial [Oracle Cloud Infrastructure.](https://docs.cloud.oracle.com/en-us/iaas/Content/Compute/References/computeshapes.htm)

![cli-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ipvT0uOwAYjzTClJfHrfloPPcZcF9fiSDoCKCCiVBM7jQVJ7u2Exwsq9s1NUHMhA/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-11.png)

**Iniciando Deploy da Instance VM**

Inicie criando um diretório para armazenar a SSH Key.

```javascript
$ mkdir keys
```

Gerando uma nova SSH Key. Substitua pelo caminho que deseja salvar a SSH Key, exemplo C:\Keys no Windows. 

```javascript
$ ssh-keygen -t rsa -N "" -b 2048 -C "CiCd-Compute-Instance" -f /home/ubuntu/keys/key-test
```

Faça o deploy da Instance VM utilizando as informações salvas anteriormente availability-domain, compartment-id, shape, subnet-id, image-id, display-name, ssh-authorized-keys-file e adicione o parâmetro assign-public-ip como true para que seja atribuído um IP público na Instance VM.

```javascript
$ oci compute instance launch --availability-domain syxp:SA-SAOPAULO-1-AD-1 --compartment-id ocid1.compartment.oc1..aaaaaaaa2tjqxvk2oxw45php23trjixcrzwb3bhzhcw4qqjpjcpvozny6mza --shape VM.Standard.E2.1.Micro --subnet-id ocid1.subnet.oc1.sa-saopaulo-1.aaaaaaaa2j4d7too2lkyjtlzujdegwl3m37tpoqxilnsyunykc2nh3fy65kq --image-id ocid1.image.oc1.sa-saopaulo-1.aaaaaaaa7inha53kcyutiqdbz3w4gvms2ab5z3bc624loheugh7fbvg4wada --assign-public-ip true --display-name instance-vm-teste --ssh-authorized-keys-file /home/ubuntu/keys/key-test.pub
```

Aguarde até que o estado da Intance VM seja Running.

![cli-12](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/v5WENWAKlT54Rd_dSRSt6_0lBB00FCehl2oXwITcMwYXQyzaGhsOuiwuSquYL7c0/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-12.png)

Acesse a Instance VM recém criada.

**MacOS/Linux**

```javascript
$ chmod 400 /home/ubuntu/keys/key-test
$ ssh -i /home/ubuntu/keys/key-test opc@152.67.40.241
```

![cli-13](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/V8QXPYRNP8g9ZLUK13BRtam-RSk72Knjal9E07wBv86UEdC-SLlCod4D3Bw2gcGU/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-13.png)

**Windows**

Instale o [MobaXterm](https://mobaxterm.mobatek.net/), abra uma nova sessão e acesse a Instance VM.

```javascript
$ chmod 400 'C:\keys\key-test'
$ ssh -i 'C:\keys\key-test' opc@152.67.40.241
```

![cli-14](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/QgtX0qJGOS3Sfil9Oi6Kcfwv5jBc7HXjcmlYHcRj9bG_ruBnQrivWbkZqTFXQ676/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/2020-11-30-Configurando-OCI-CLI/cli-14.png)

Agora você é capaz de acessar o OCI-CLI e criar Instances VMs na Oracle Cloud Infrastructure.









