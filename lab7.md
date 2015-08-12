# LAB 7 - Expor DSS através do API Manager

### Objetivo

Expor a API REST que criamos no Lab 6 através do API Manager

### Recursos

* WSO2 DSS 3.2.2

### Procedimento

1. Acesse o API Publiser e crie a API clicando em `Add`
  * Informe os dados básicos da API na seção `Design`
  * Avance para a seção `Implement`. Inicie com uma configuração Prototype, para
  isso basta informar o Endpoint da API cria no DSS em `Prototype Endpoint`. Isso
  permite que a API seja testada sem a necessidade de subscribe e autorização.

2. Teste a API usando o Advanced Rest Client. É possível usar os mesmos cenários
criados para o Lab 6, só é necessário atualizar a URL. Para obter a URL completa
da API e outros dados acesse `API Store` (na tela de **Overview** da API no
`API Publisher` há um link *"Go to API Store"*).

3. Depois de testar sua APi em modo Prototype acesso a API pelo `API Publisher` e
atualize-a para **Managed API**. Avance para a seção `Managed` e faça as
configurações necessárias. Basicamente, o que você precisa fazer nessa seção é
informar o `Tier Availability`, marque as 3 opções Gold, Silver e Bronze.

4. Depois de salvar suas configurações você verá que não é mais possível acessar
a API via Rest Client. Agora vamos fazer o subscribe na API e gerar um token de
acesso.
  * Faça log in na `API Store` (pra efeito de testes é possível utilizar o mesmo
    usuário admin/admin)
  * Depois de logar, acesse a API criada no passo (1) e clique em **Subscribe**.
   Observe que você pode criar uma nova Aplicação na combo `Applications`.
  * Depois de realizar o subscribe, vá até `My Subscriptions` e clique em
  `Generate keys` para gerar as chaves de acesso e um token.
  * Copie o token gerado e adicione o seguinte Header HTTP ao Rest Client:
  ```
  Authorization: Bearer <token_de_acesso>
  ```

5. Teste novamente sua API. Procure enviar requisições suficientes para estourar
 o limite do Throttling definido.
