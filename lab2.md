# Lab 2 - SOAP SimpleStockQuoteServiceProxy

### Objetivo

Neste laboratório vamos expor um serviço de backend através de um Proxy SOAP.

### Recursos

* WSO2 ESB 4.8.1 (certifique-se que o offset da porta esteja devidamente configurado).

### Procedimento

1. Antes de iniciar a criação do Proxy vamos subir um serviço SOAP de exemplo
 que, para nosso cenário, servirá como um serviço de backend. Este exemplo pode
 ser encontrado no próprio ESB, na pasta `samples`.

2. Acesse o diretório `ESB_HOME/samples/axis2Server/src/SimpleStockQuoteService `
e execute o comando `ant` para compilar, empacotar e deployar o serviço
`SimpleStockQuoteService`
   ```shell
   $ cd ESB_HOME/samples/axis2Server/src/SimpleStockQuoteService
   $ ant
   ```
   Com isso, o arquivo
`ESB_HOME/samples/axis2Server/repository/services/SimpleStockQuoteService.aar`
será criado.

3. Para rodar o serviço execute o utilitário `axis2server.sh`

  ```shell
  $ cd ESB_HOME/samples/axis2Server
  $ ./axis2server.sh
  ```
  Isso vai iniciar um servidor Axis2 nas portas 9002 (https) e 9000 (http).
  O serviço `SimpleStockQuoteService` possui quatro operações:
  * getQuote (in-out) - Gera uma cotação de ações para um símbolo.
  * getFullQuote (in-out) - Gera um histórico de contações para um símbolo.
  * getMarketActivity (in-out) - Retorna uma contações para uma lista de símbolos.
  * placeOrder (in-only) - Recebe uma mensagem `one way` que simula uma ordem de
   compra.

  A maior parte do tempo estaremos usando a operação `getQuote`.

4. Agora vamos criar nosso Proxy. Primeiro, inicie o ESB:
  ```shell
  $ cd $ESB_HOME/bin
  $ ./wso2server.sh
  ```
5. Acesse o **Management Console** usando o caminho https://localhost:9445/carbon
 e as credenciais `admin`/`admin`.

6. Criando o Proxy

  * Siga o caminho: `Add > Proxy Service > Custom Proxy`
  * Dê o nome **SimpleStockQuoteServiceProxy**,
  * Em `Publishing WSDL` escolha a opção **Specify source URL** e informe: `http://localhost:9000/services/SimpleStockQuoteService?wsdl`
  * deixe os demais campos como
  estão e clique `next`  

7. Criando o **In Sequence**

  * Em `Define In Sequence` selecione `Define Inline` e então clique `Create`
  * Agora, em `Root`, faça `Add Child > Core > Log`
  * Configure o `Log Level` para **Full** e clique em `Update`
  * Em `Log` navegue  `Add Sibling > Core > Send`
  * No Painel `Send Mediator`, em `Select Endpoint Type`, selecione
  `Define Inline` e navegue em `Add > Address Endpoint`
  * Preencha o campo Address com o valor `http://localhost:9000/services/SimpleStockQuoteService`
  * Clique em `Show Advanced Options` e selecione no campo `Format` a opção
  **SOAP 1.1**
  * Clique `Save & Close`, depois `Update`, depois `Save & Close` novamente e
   ,então, `next`

8. Criando o **Out Sequence**

  * Em `Define Out Sequence` selecione `Define Inline` e então clique `Create`
  * Agora, em `Root`, faça `Add Child > Core > Log`
  * Configure o `Log Level` para **Full** e clique em `Update`
  * Em `Log` navegue  `Add Sibling > Core > Send`
  * Clique em `Update`, `Save & Close` e então `Finish`

9. Na lista de Proxies é possível ver um link para o wsdl do serviço. Use-o para
testar o serviço usando o Soap UI.
