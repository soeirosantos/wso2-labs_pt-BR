# Lab 3 - Rest SimpleStockQuoteServiceAPI

### Objetivo

Neste laboratório vamos expor um serviço de backend através de uma API Rest.

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

6. Criando a API

  * Siga o caminho `APIs > Add API`
  * Informe os seguintes dados

  ```
  API Name: SimpleStockQuoteServiceAPI
  Context: stocks
  ```

  * Clique em `Add Resource`
  * Marque `Method` **GET**
  * Selecione `URL Style` como **URI-Template** e no campo `URI-Template` informe
  o valor **/quote/{symbol}**

7. Configurando o **In Sequence**

  * Em `In Sequence` selecione `Define Inline` e então clique `Create`
  * Agora, em `Root`, faça `Add Child > Core > Log`
  * Configure o `Log Level` para **Custom**
  * Clique em `Add Property` e informe `Property Name` como **LOCATED** e
  `Value/Expression` como **Quote - In Sequence**, deixe `Property Value` como
  **Value**
  * Clique em `Update`
  * Em `Log` navegue  `Add Sibling > Transform > PayloadFacotry`
  * Selecione `Define inline` e informe o seguinte valor na caixa de texto
  ```xml
  <ser:getQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
      <ser:request>
        <xsd:symbol>$1</xsd:symbol>
      </ser:request>
  </ser:getQuote>
  ```
  * Clique em `Add Argument`, selecione o `Argument Type` como **Expression** e
  preencha `Argument Value/Expression` com o seguinte valor
  **get-property('uri.var.symbol')**. Deixe o campo `Evaluator` como **xml**
  * Clique em `Update` para finalizar o PayloadFactory
  * Adicione um novo passo na sequence, selecione `Add Sibling > Transform > Header`
  * Informe os valores
  ```
  Name: Action
  Value: urn:getQuote
  ```
  * Deixe os demais campos como estão e pressione `Update`
  * Faça `Add Sibling > Core > Send`
  * No Painel `Send Mediator`, em `Select Endpoint Type`, selecione
  `Define Inline` e navegue em `Add > Address Endpoint`
  * Preencha o campo Address com o valor `http://localhost:9000/services/SimpleStockQuoteService`
  * Clique em `Show Advanced Options` e selecione no campo `Format` a opção
  **SOAP 1.1**
  * Clique `Save & Close`, depois `Update`, depois `Save & Close`

8. Criando o **Out Sequence**

    * Em `Out Sequence` selecione `Define Inline` e então clique `Create`
    * Agora, em `Root`, faça `Add Child > Core > Log`
    * Configure o `Log Level` para **Full** e clique em `Update`
    * Em `Log` navegue  `Add Sibling > Core > Send`
    * Clique em `Update`, `Save & Close`, `Update` novamente e, por fim, `Save`
    * Obs: Cheque se o `URL Style` ainda está devidamente configurado

9. Agora use o plugin de Chrome **Rest Client** ou mesmo o **SOAP UI** para testar
a API

  * Um exemplo de URL: http://localhost:8282/stocks/quote/WSO2
