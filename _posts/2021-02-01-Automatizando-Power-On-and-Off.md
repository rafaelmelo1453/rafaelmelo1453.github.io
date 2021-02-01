---
layout: post
title: Automatizando Power On/Off de recursos OCI
subtitle: Economize habilitando o Power On/Off em recursos que não operam 24/7
tags: [Cloud, Oracle, OCI, CLI, Visual Builder Studio, DevCS, Cost, Schedule, Shell]
comments: true
---

## Porque automatizar Power On/Off de recursos que não operam 24/7?

Os recursos na Oracle Cloud Infrastructure como na maioria dos Cloud Providers são faturados por horas de uso, logo quanto maior o tempo de uso maior o custo, mas o recurso provisionado não é cobrado durante o tempo em que permanece desligado. 

Caso possua um ambiente não produtivo e sem a necessidade de permanecer ligado 24/7, manter estes recursos desligados durante a madrugada e finais de semanas pode significar uma boa economia no fim do mês.

Se uma empresa que possue ambientes de desenvolvimento e homologação que são utilizados apenas durante o horário comercial. Caso essa mesma empresa mantenha os recursos desligados durante os períodos entre 21:00 e 7:00 e durante todo o fim de semana a economia com estes 2 ambientes pode chegar a 60% após 1 mês

## Scripts para Power On/Off

Foram criados scripts em shell com comando do OCI-CLI para realizar o stop e start. Acesse o repositório do [GitHub](https://smallskills.github.io/2021-01-17-Create-VBS-And-Link-To-OCI/) e baixe os scripts para os seguintes recursos.

- Instances
- Instance Pool
- Oracle Analytics Cloud
- Autonomous Database
- DBSystem Database
- Exatada Database

Os scripts podem ser facilmente modificados para outros tipos de recursos na OCI. São necessário dois scripts para cada recurso, sendo um para stop e um para start.

Adicione o seu escopo aos scripts substituindo o OCID do compartment "Parent". O código foi programado para ler os compartments de primeiro nível "child", mas não os subcompartments do do "child" ou seja "grandchildren".  

## Opções para automação 

Com os scripts baixados é preciso definir a estrátegia para automatizar o processo de Power On/Off e são varias as opcões:

- Oracle Visual Builder Studio
- Azure DevOps
- OCI Instance Principal
- Schedule numa VM

Abaixo um passo a passo da configuração do Oracle Visual Builder Studio.

## Automatizando Power On/Off com Oracle Visual Builder Studio

VB Studio será utilizado para executar a ação de stop e start dos recursos, caso não tenha o VB Studio configurado siga este [tutorial](https://smallskills.github.io/2021-01-17-Create-VBS-And-Link-To-OCI/) antes de prosseguir.

Os scrits podem ser facilmente configurados para rodar no Azure DevOps caso por algum motivo o VB Studio não seja um opção.

**Crie uma Virtual Machine Template**

Acesse o VB Studio indo até **Platform Services > Developer.**

![ss-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/bSHXe1-VoKSEdEe4Pgj2QIWInsAG0UnlXNGLJ9zrLAWmSlREAJNXi3kYwkuPchs3/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-01.png)

Vá até **Virtual Machines Templates > Create Template.**

![ss-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/xro1mH7hoDctTODu8PECKCFUKdv6bzqM3FX2t2YId5uwnRHs2LW6et8dXLjjFwXH/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-02.png)

Adicione um nome para o template e em Platform selecione Oracle Linux 7.

![ss-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/fLexV5jSM0K9D5i5VPuTKkkb0j9jhrT6CqZsqxNRPW8wLq-Bor1s2xF4y7maK6ni/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-03.png)

Clique em **Configure Software** e adicione os seguintes software packages **OCIcli** e **Python3.**

![ss-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/fIvkX0KNSt_YkwSSQwMtagVsyh56J9Aj6uFQE29S-kcmRJ-VSJUY9x4Za_EtLGIK/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-04.png)
 
![ss-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/NiEwaGYML76zKXHjR0_sE_PLi8kNrOD4ydE4BbCA-nWo4r8X5MfHiOB31NEPgz7u/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-05.png)

**Crie uma Virtual Machine**

Essa será a virtual machine em onde seus scritps serão rodados, essa VM pode ser uma Always Free que não gera custos. A instance será totalmente gerenciada pelo VB Studio. 

Vá até **Virtual Machines > Create.**

![ss-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Mrm5ewJBJ7A97Iq_FsADNkwOOuhb5rduB4vfya2MmNLpEApGZtUzMOINTo8QrzlR/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-06.png)

Selecione a quantidade de instances, a região, o shape e aponte o template recém criado.  

![ss-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/LNfcPiSbArWCIYfRrYCpFX__iIq2Cii0osJB1JByEcnKnv_brMOJT61GQj-E3C1P/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-07.png)

Clique em start para que a instance seja provisionada e aguarde até o status seja running.

![ss-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/MJJi2N3LrRnM8BIm8ONBAtzEwkCC6jV445AVEH1CprWS4UPIVZAzchYbc4pKce-F/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-08.png)

**Crie um projeto no VB Studio**

Vá até **Projects > Create.**

![ss-09](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/fprsc2dciMl399bv9wjv1BWazjmcv9XI2mkkkyVmSr2RjXK3uE9RxRgUfazhLcH_/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-09.png)

Dê o nome Power On-Off para o projeto.

![ss-10](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/OQFd3Wblwr9Kd6LeCHBqKm0JpHJ25IQz1qtd7lfoGvl2Vxr9Ff4f9eLok_3O17yJ/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-10.png)

Selecione Empty Project e clique em next.

![ss-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/iGDIikVMfTYfqbFNa0yYCLhPxpCvx0Ej3v8f9BFz2JnpKF-KgIOIxb7l84CYjiV7/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-11.png)

Selecione Markdonw e clique em finish.

![ss-12](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/OyX1K5INfSvsvs-8MIGmYk8mJWuapSt1CbfjAMv_rEzCUat1VIg5qfvQKOzGnV-H/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-12.png)

**Create Git Repository**

Vá até **Git > Create Repository.**

![ss-13](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/QAJIVScW-byK8M2q7S60b1ulHaBTCc9hcEDIqLFf4MjsNhwR8wm8ADeUSPS3LJcP/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-13.png)

Dê o nome git-power-on-off para Git Repository.

![ss-14](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/7wrEA2qMuCqUgAFTlBdCL3JL-4jhyc3KK5l9XkCQX4Ps27cPER2O8IGRzYOcpAYh/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-14.png)

Clique em Add File para adicionar um novo arquivo.

![ss-15](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/5RHtS6MXJ2cERbL0brXpLTPAeTCBD9hUZtmQDVmIXJ7-APakQCQw0eDNsPL9RNcn/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-15.png)

Selecione o nome da pasta que o arquivo será salvo + o nome do arquivo e então arraste o arquivo do script para area de código. Clique em commit e repita isso para todos os outros arquivos.

![ss-16](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/twfln64tnw0C4f5IElcu3rP5piS_837fJ29bYIKmBYJfmQgUBaHgj1arpd3E-Lab/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-16.png)

**Create Build**

Vá até **Build > Create Builds.**

![ss-17](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/dsayM9VlyRxNqW0rO73IhfNMXkRe8qmIamgrP-DaCH4PyYv5XLYvftjH_ZV00F4H/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-17.png)

Dê um nome para o build e selecione o template que foi configurado anteriomente.

![ss-18](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ApfI39HdJinbKwG7PWPL2T_ED5ZMD0nmJgSLd_Ywj6yhecTV2lHxo20yDoyeb1Tk/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-18.png)

Clique em **Add Git.**

![ss-19](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/W-Eni-uFHoSdWnwfxF6umV3crh0eSjsv_f3OsXY8cngdfB-ZMU0Hyb0hmjYzlkO1/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-19.png)

Selecione o repositório do Git criado anteriomente e clique em save.

![ss-20](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/2vGoUebkvCGU0pKig9STC23Y5BSLys41RwoV-0vqI2TP57-omNsiK-75pRFQZn3q/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-20.png)

Vá até **Steps** e clique em **Add Step** e selecione OCIcli.

Preecha com os dados do seu tenancy, utilize o usuário criado durante a configuração do VB Studio ou crie outro com permissões compativéis. Recomendo a utilização de keys sem passphrase, pois obtive muitos erros ao utilizar keys com a passphrase.

![ss-21](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/A0mZP1Gani0eDWCNarJMD1Qj0ufUD6HrtXn23zG0HXNF0pLgMjIkjl8XlaMjTCsn/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-21.png)

Adicione um novo **Step. Dessa vez escolha Common Builf Tools > Unix Shell.** 

Aqui adicione os scripts que serão executados.

![ss-22](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/VtQ066ASfRObciDr9vA5AT1X8IwRHbwqLJdjbi0XDmyFdIccT3jD1isuX5_heMLe/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-22.png)

Faça o agendamento do desligamento clicando na engrenagem e em seguida em **Triggers**. Defina o escopo desejado e salve as alterações.

![ss-23](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/NVeXQ1brMAU-TsmuXZE4bhWo23VHJw-Ku-7Z98X4iPfYO7EMIg7I0oHSx3khZ7hQ/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-23.png)

Refaça o processo de criacao de build, mas dessa vez adicione os scripts de start.

Revise as configurações e teste o processo de stop dos recursos clicando em **Build Now.** Logo em seguida clique em **Build Log** e verifique se tudo ocorreu bem.

![ss-24](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/RsnIRH4dvjtzQtxP1T8ypBU1J7u665tFpHT8lQo-r-kdY09_lwoKDdJ8UyaT5SlR/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/POST-SCRIPTS/ss-24.png)

Dessa forma está configurado a rotina de power on/off diariamente.
