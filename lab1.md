# LAB 1 - SOAP CreditServiceProxy

### Objetivo

O objetivo deste laboratório é ter um primeiro contato com o WSO2 ESB através de
 um exemplo simples. Ao final, você irá expor um Proxy SOAP que apresenta um
 resultado estático.

### Recursos

* WSO2 ESB 4.8.1 (certifique-se que o offset da porta esteja devidamente configurado).

### Procedimento

1. Iniciando o ESB:

  ```shell
  $ cd $ESB_HOME/bin
  $ ./wso2server.sh
  ```
2. Acesse o **Management Console** usando o caminho https://localhost:9445/carbon
e as credenciais `admin`/`admin`.

3. O resultado que esperamos do nosso Proxy é o seguinte:
  * Enviar uma requisição SOAP com o seguinte corpo:

    ```xml
    <c:customer xmlns:c="http://core.bankxyz.com">
         <c:id>00001</c:id>
    </c:customer>
    ```
  * E ter como resposta um XML como:

    ```xml
    <c:customerCreditProfile xmlns:c="http://core.bankxyz.com">
         <c:id>00001</c:id>
         <c:cpf>121312314</c:cpf>
         <c:status>active</c:status>
         <c:grading>A</c:grading>
    </c:customerCreditProfile>
    ```

4. Criando o Proxy

  * Siga o caminho: `Add > Proxy Service > Custom Proxy`

  * Dê o nome **CredServiceProxy**, deixe os demais campos como estão e clique
  `next`

5. Criando o **In Sequence**

  * Em `Define In Sequence` selecione `Define Inline` e então clique `Create`
  * Agora, em `Root`, faça `Add Child > Core > Log`
  * Configure o `Log Level` para **Full** e clique em `Update`
  * Em `Log` navegue  `Add Sibling > Core > Log`
  * Set o `Log Level` para **Custom**
  * Clique em `Add Property` e set
    ```
    Property Name: CUSTOMER ID
    Property Value: Expression
    Value/Expression: //c:customer/c:id/text()
    ```
  * Clique em `NameSpaces` e informe: `Prefix: c` e `URI: http://core.bankxyz.com`
  , `Save` o namespace
  * Clique em `Update`
  * Agora, clique no segundo Log e faça `Add Sibling > Transform > PayloadFactory`
  * Clique em Define inline e preencha com o seguinte XML:

    ```xml
    <c:customerCreditProfile xmlns:c="http://core.bankxyz.com">
        <c:id>$1</c:id>
        <c:cpf>121312314</c:cpf>
        <c:status>active</c:status>
        <c:grading>A</c:grading>
    </c:customerCreditProfile>
    ```
  * Clique em `Add Argument` e set
    ```
    Argument Type: Expression
    Evaluator: xml
    Argument Value/Expression: //c:customer/c:id/text()
    ```
  * Clique em `NameSpaces` e informe: `Prefix: c` e `URI: http://core.bankxyz.com`
  , e `Save` o namespace, assim como fizemos anteriormente
  * Clique em `Update`
  * Para finalizar, em `PayloadFactory` navegue: `Add Sibling > Core > Respond`
  * Finalize o **In Sequence**: `Save & Close`

6. Finalize o Proxy: `Next` > `Next` > `Finish`

7. Para testar o Proxy, utilize a URL do wsdl:
http://localhost:8282/services/CredServiceProxy?wsdl e o SOAP UI. Preencha o
Body com o XML que mencionamos anteriormente:
 ```xml
 <c:customer xmlns:c="http://core.bankxyz.com">
     <c:id>00001</c:id>
 </c:customer>
 ```
8. Observe no console os Logs que configuramos
