---
layout: post
title: Como exportar usuários do Azure AD para Oracle Cloud e Oracle IDCS
subtitle: Sincronize os usuários do Azure AD para Oracle Cloud Infrastructure e Oracle IDCS 
tags: [Cloud, Oracle, Azure, Azure AD, User, Group, Sync, IDCS]
comments: true
---

Sincronize usuários do Azure AD com IDCS e console Oracle Cloud Infrastructrure de forma automatizada.

## Sincronize Azure AD com OCI e IDCS

Navegue até **Identity > Federation > Identity Provider Details** para acessar a console do IDCS. 

![oci-01](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/8br-lFOSkD_feLDcdtZ3FhqhOA65BVJHntQr8ILIaiuMUy_iBqM9lT_rI-ot4DTG/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-01.png)

![oci-02](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ZxbfRmmjp5Sxm7ew7YMSRpP-N3j1t_xCfA6Xu7ISYNCHXsmTOCbTRaNPDTWAYiuL/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-02.png)

![oci-03](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/72XT_0iFaYuyi40j8Zjs8RxB69UiFN6azvbU-4BO52OCmHfL_kO0HFt5iK7Q3nlY/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-03.png)

Já na console do IDCS clique em **Applications and Services,** em **Add**  e selecione **Confidential Application.**

![oci-04](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Nyh6yGjP70WaKAZZZhIBQap_QZZp_VRY42xA392MU3dSE3Ir4sRriFyiM-wxsqK-/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-04.png)

![oci-05](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/jtam_Fi3JE6okUkyRmyUOxLBo9XOQgT8c2ofpaxbEBXKgzLVd-WLE4X2DpwgFIwM/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-05.png)

Dê um nome para o aplicativo e clique em **Next** em todas as próximas etapas e então clique e, **Finish.**

![oci-06](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/HOnXTi0RlJ4nhAhRRIJJD0Ohl91sEV6eNlhjvvpl3JwVPz9mZ6DjTT_1Diz5k4_9/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-06.png)

Em seguida ative o aplicativo. 

![oci-07](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/HH1wT1cWCQs1rMvn1vAa5SNF8ZxVXGdvstJe7_gMUdIPpjsk9DSQyHS1DhOoFMGv/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-07.png)

Abra o aplicativo e vá até **Configuration > Client Configuration** e preencha os campos exatamente como nas imagens mostradas abaixo e salve a configuração clicando em **Save.**

**Allowed Grand Types**
- Client Credentials
- JWT Assention
- Refresh Token
- Authotization

**Redirect URL:** https://localhost/callback

**Client Type:** Confidential

**Grant the client access to Identity Cloud Service Admin APIs**
- Identity Domain Administrator

![oci-08](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/DtMQtnc-pLAK9c_5rHR71I0KJnBNmkPiRrjmML4a2LEZsgLUrd7TnEKK-6fXALPB/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-08.png)

![oci-09](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/2uuTWPbgbTDxFCI9wpSk7whdylwSzIHE8zY7Wr5OMoSfxm_f2fHy0WEJLZSVENqr/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-09.png)

Um novo campo chamado **General Information** deve ser apresentado, salve o valor do **Client ID e Client Secret,** serão utilizados para configurar do lado da Azure.

![oci-10](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/hQ0uHrgfuAIE2-6KvF0w1DsjU3yxMiAkXJkmIpyT_NR00aByQuN6WMWjg25P9b64/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-10.png)

## Crie e configure um Enterprise Application no Azure AD

Navegue até **Azure AD > Enterprise applications > All Applications,** clique em  **New Application** e em seguida em **Create your own application,** dê um nome e escolha a opção **Integrate any other application you don't find in the gallery** e clique em **Create.**

![oci-11](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/f4q8yhV1UPLxeBxisUgzlcgPLXjmMsok5cGK2jB-632VrjOMAj4nRiQ9y-gaVwma/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-11.png)

![oci-12](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/pjyF2tVamRZTSlEmyDi4UaiUPYsObNkDMCyGwG8ol4cagplbdIsBRIE56pgaf4zf/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-12.png)

![oci-13](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/vu1e4JP37gInUYfLXBVqGIRG4IIG6btEUgCW1Bpp-Wj7OJm_mwezQUGSKSgM4Ju6/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-13.png)

Navegue até **Provisioning,** clique em **Get Start.**

![oci-14](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/zkF2DLBhZ0rVSKqjGS5NAEsTu7c_EgbU76fQN3rlF9-GezyR71v7wPIz3edEz8lN/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-14.png)

Em seguida selecione o **Provisioning Mode** para **Automatic.**

![oci-15](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/ZZY13wJ2nuDw3d-3HPzhKfx4bQnWd0Zwx_U1zVUBZ-v1c-QZA4cGrQ6hK99zGkLy/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-15.png)

**Admin Credentials:** 

**Tenant URL:** Insira a URL IDCS da sua conta OCI. Por exemplo https://idcs-xxxxxxxxxxxxxxxxxxx.identity.oraclecloud.com/admin/v1

**Secret Token:** O Secret Token precisa ser convertido para Base64 e para isso será necessário o Client ID e Client Secret que foi salvo anteriomente.

- **Para Macbook:** Execute o seguinte comando no terminal:

```javascript
echo -n "f6251e54daaa4a67a7f26614b883aeb1:18fa5872-88c5-4879-b327-32eebe9712bc" | base64
```

- **Para Linux:** Execute o seguinte comando no terminal:

```javascript
echo -n "f6251e54daaa4a67a7f26614b883aeb1:18fa5872-88c5-4879-b327-32eebe9712bc" | base64 -w 0
```

- **Para Windows:** Crie um arquivo .txt na pasta C:\temp com o nome appCreds.txt, abra o CMD como administrador e execute o seguinte comando:

```javascript
certutil -encode C:\temp\appCreds.txt C:\temp\appbase64Creds.txt
```
Abra o arquivo C:\temp\appbase64Creds.txt, copie o conteúdo e feche o arquivo.

![oci-16](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/qBFwZmCkFT6ny5-X44HXt3IO8SfPeoqS2oRuxNALrwM1hIxdZKjxJZwMThGojBQ7/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-16.png)

Adicione os valores e clique em **Test Connection.** Caso a conexão seja feita com sucesso clique em **Save.**

![oci-17](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/4jQ5BG9MZ93XYI-vybVSDKM6VcHoZ0Myhve6JqLt1MupLm6Utm9vHPsqC6M9lTOr/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-17.png)

**Mappings:** Mantenha a configuracão padrão ou personalize como desejar.

![oci-18](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/Ck36GD-LGAyk36tAwUaX9aqM2T7HP6BgEowgJb3hyzoVMe94K6DNKBgnA6RBWCuz/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-18.png)

**Settings:** Adicione um e-mail e marque a opção **Send an email notification when a failure occurs**, em **Scope** escolha a opção **Sync only assigned users and groups** isso significa que apenas os usuário e grupos associados serão sincronizados, por fim clique em **Save.**

![oci-19](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/PLtf131tZFEim33VOLVNwSx9XzcOslCTCUVNA7H8mAggbCBSRjDxAzFLEDBsdp_f/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-19.png)

## Usuários e Grupos que serão sincronizados

No Azure AD crie um grupo de segurança e adicione os usuários que devem ser sincronizados, caso precise de ajuda com essa task acesse este [tutorial.](https://docs.microsoft.com/pt-br/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal)

Navegue até **Azure AD > Enterprise applications > Seu Aplicativo > Users and groups** clique em **Add User/Group** para associar o grupo recém criado. **Obs:** Um requisito para adicionar um grupo é ter Azure AD Premium P2 habilitada, adicione a licença Premium P2 em 5 passos com este [tutorial.](https://azure.microsoft.com/pt-br/trial/get-started-active-directory/)

![oci-20](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/aAkGnS7zLTqA4wgsR45bq-N58GN_vS20fIg8zg8rkq0SvvigE_-jl-50sn8GXyd-/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-20.png)

![oci-21](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/lizLi9qAYzxUuzulefrf1IIrJkMcvmsGi3EC-uDpYucbXX21GGOAQU7IB1xQCr6G/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-21.png)

Volte para **Provisioning** em  **Azure AD > Enterprise applications > Seu Aplicativo > Provisioning** e clique em **Edit Provisioning** e marque **Provisioning Status** para ON e clique em **Save.**

![oci-22](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/T6DyKskRLfefZ8WzHbWZiJRK-53HtRy1OiJJrOv1DDIJum0agQz5b1S5UdEjYeOs/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-22.png)

Agora com tudo configurado volte para **Provisioning** em  **Azure AD > Enterprise applications > Seu Aplicativo > Provisioning** e verifique a que o provisionamento foi iniciado. A primeira sincronização pode levar até 40 minutos, mas isso depende da quantidade de usuários que serão adicionados para a console Oracle Cloud e IDCS.

![oci-23](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/JVw6iMVRuI3bsBI8lHslgBMxqNf5sQCFtC-zmxOEIzaiK4v3FvaJnhOL7a4iE4y9/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-23.png)

## Usuários e Grupos Sincronizados 

Verifique se o grupo e os usuários foram sincronizados corretamente no Dashboard do IDCS e Console do Oracle Cloud Infrastructure.

**IDCS**

Na Dashboard do IDCS acesse a guia de **Groups** e **Users** e verifique os objetos sincronizados.

![oci-24](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/bVUz19vYL42Y7Wtj0dGnzqcqpJGGAg_vqY7rCKb9R9PnPJbE1UtTD4Gmz3z2TxOt/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-24.png)

**Console Oracle Cloud**

Na console do Oracle Clou navegue até **Identity > Federation> OracleIdentityCloudService** e verifique os objetos sincronizados em **Usuários** e **Grupos**.

![oci-25](https://objectstorage.sa-saopaulo-1.oraclecloud.com/p/S0sWSi0Z4LEemQUpzgp6YwVWxUjqndoHSDkMeFIjJ_pZiSw4jbcwKK77ZdIvauAd/n/gr8gkzaf8nit/b/bucket-euoraf4-site/o/azure-oci/oci-25.png)

Dessa forma sempre um usuário for adicionado no grupo de segurança associado ao aplicativo do Azure AD será sincronizado para IDCS e console Oracle Cloud em até 40 minutos.
